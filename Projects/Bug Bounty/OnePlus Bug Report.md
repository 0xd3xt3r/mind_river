---
tags:
  - vulnerability-research
status: in-progress
created-date: 26-03-2021
---

https://github.com/OnePlusOSS/android_kernel_modules_and_devicetree_oneplus_sm8150/blob/oneplus/sm8150_s_12.1_op7pro/vendor/oplus/kernel/touchpanel/oplus_touchscreen/chipone/cts_tool.c#L779

File : kernel/touchpanel/oplus_touchscreen/chipone/cts_tool.c
```C
// Gobal variable which hold the command
static struct cts_tool_cmd cts_tool_cmd;

#ifdef CONFIG_CTS_I2C_HOST
/* If CFG_CTS_MAX_I2C_XFER_SIZE < 58(PC tool length), this is neccessary */
static u32 cts_tool_direct_access_addr = 0;
#endif /* CONFIG_CTS_I2C_HOST */

static int cts_tool_open(struct inode *inode, struct file *file)
{
    file->private_data = PDE_DATA(inode);
    return 0;
}

static ssize_t cts_tool_read(struct file *file,
        char __user *buffer, size_t count, loff_t *ppos)
{
    struct chipone_ts_data *cts_data;
    struct cts_tool_cmd *cmd;
    struct cts_device *cts_dev;
    // default value
    int ret = 0;

    cts_data = (struct chipone_ts_data *)file->private_data;
    if (cts_data == NULL) {
        TPD_INFO("<E> Read with private_data = NULL\n");
        return -EIO;
    }

    cmd = &cts_tool_cmd;

    // some code we don't bother
    
    switch (cmd->cmd) {
    
    // other cases
    
    //Step 3: Attacker will execute this case so that ret value
    // is not affected
    case CTS_TOOL_CMD_GET_DRIVER_INFO:
        break;
    // Step X : we can take this case so taht we have "ret" as 0
    default:
        TPD_INFO("<W> Read unknown command %u\n", cmd->cmd);
        ret = -EINVAL;
        break;
    }

if (ret == 0) {
        if(cmd->cmd == CTS_TOOL_CMD_I2C_DIRECT_READ) {
            ret = copy_to_user(buffer + CTS_TOOL_CMD_HEADER_LENGTH,
                                cmd->data, cmd->data_len);
        } else {
            // Step3 : This will leak the data into the user memory
            ret = copy_to_user(buffer, cmd->data, cmd->data_len);
        }
        if (ret) {
            TPD_INFO("<E> Copy data to user buffer failed %d\n", ret);
            return 0;
        }

        return cmd->data_len;
    }

    return 0;
}

static ssize_t cts_tool_write(struct file *file,
        const char __user * buffer, size_t count, loff_t * ppos)
{
    struct chipone_ts_data *cts_data;
    struct cts_device *cts_dev;
    struct cts_tool_cmd *cmd;
    int ret = 0;

    if (count < CTS_TOOL_CMD_HEADER_LENGTH || count > PAGE_SIZE) {
        TPD_INFO("<E> Write len %zu invalid\n", count);
        return -EFAULT;
    }

    cts_data = (struct chipone_ts_data *)file->private_data;
    if (cts_data == NULL) {
        TPD_INFO("<E> Write with private_data = NULL\n");
        return -EIO;
    }
    // Step 2: "cts_tool_cmd" is a global variable
    cmd = &cts_tool_cmd;
    // copies the bufer suppied data in to "cmd" variable
    // attacker can give large value like 0x5000
    // cmd->data_len = 0x5000
    ret = copy_from_user(cmd, buffer, CTS_TOOL_CMD_HEADER_LENGTH);
    if (ret) {
        TPD_INFO("<E> Copy command header from user buffer failed %d\n", ret);
        return -EIO;
    } else {
        ret = CTS_TOOL_CMD_HEADER_LENGTH;
    }
    // validation for "cmd->data_len = 0x5000" will fail here
    // but still the data is copied into the global variable
    if (cmd->data_len > PAGE_SIZE) {
        TPD_INFO("<E> Write with invalid count %d\n", cmd->data_len);
        return -EIO;
    }

}

static struct file_operations cts_tool_fops = {
    .owner = THIS_MODULE,
    .llseek = no_llseek,
    .open  = cts_tool_open,
    .read  = cts_tool_read,
    .write = cts_tool_write,
    .unlocked_ioctl = cts_tool_ioctl,
};

int cts_tool_init(struct chipone_ts_data *cts_data)
{
    TPD_INFO("<I> Init\n");
    // this explosed the above driver functions to userland
    cts_data->procfs_entry = proc_create_data(CFG_CTS_TOOL_PROC_FILENAME,
            0666, NULL, &cts_tool_fops, cts_data);
    if (cts_data->procfs_entry == NULL) {
        TPD_INFO("<E> Create proc entry failed\n");
        return -EFAULT;
    }

    return 0;
}
```

This vulnerability can help the attacker to leak global variable of kernel memory to userspace application. This can be do with following way:
1. First user opens device file '/proc/icn85xx_tool'. This device file is create by the kernel driver in file "kernel/touchpanel/oplus_touchscreen/chipone/cts_tool.c" line number 1078.
2. Then we can do write operation on device file this will trigger "cts_tool_write" function. User provided data is in "buffer" function parameter. Here Attacker will provide "struct cts_tool_cmd" in the buffer with malicious length (for example cmd->data_length = 0x5000) file say for example 0x5000. The data is copied into "cmd" variable with this coded "cmd = &cts_tool_cmd; (cmd, buffer, CTS_TOOL_CMD_HEADER_LENGTH);". "cmd" variable points to "cts_tool_cmd" which is a global variable. The validation fails on line "if (cmd->data_len > PAGE_SIZE) {" but still the data is already copied into the global variable "cts_tool_cmd". This is the vulnerability. That even if the validation of cmd->data_len fails the data is copied into "cts_tool_cmd".
3. Next attacker can do device read operation on the device file this will tigger "cts_tool_read" function. The function set "cmd" variable to "cts_tool_cmd" global variable then it executes switch-case. Attacker can set the value of "cmd->cmd" to "CTS_TOOL_CMD_GET_DRIVER_INFO" this is important because the function will maintain the value of "ret=0". After the switch-case is executed if ret is zero the "copy_to_user" function is executed which is like "ret = copy_to_user(buffer, cmd->data, cmd->data_len);" this can leak the kernel data to user-land. Since we can control "data_len" and set it any value we want.
4. Since this is leaking data from Gobal memory it can leak memory points which can lead to bypassing KASLR.