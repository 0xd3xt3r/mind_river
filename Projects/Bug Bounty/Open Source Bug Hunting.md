---
aliases:
  - kernel bugs
  - Android driver bug hunting
tags:
  - linux-kernel
  - vulnerability-research
---

https://github.com/PixelExperience-Devices/kernel_xiaomi_polaris/blob/dce351ed301019553d148266a62fd3274f69ff22/techpack/audio/dsp/usf.c#L829

## Samsung 
https://googleprojectzero.github.io/0days-in-the-wild//0day-RCAs/2022/CVE-2022-22265.html

# Motorola Kernel Modules

- [source](https://github.com/MotorolaMobilityLLC/motorola-kernel-modules/blob/88a1aee809588023f670433e5710199f6e8de9b4/sound/soc/codecs/msm-cirrus-playback.c#L767)
- can be opened with /dev/msm_cirrus_playback
## Bug 1

```C
struct crus_se_ioctl_header {
	uint32_t size;
	uint32_t module_id;
	uint32_t param_id;
	uint32_t data_length;
	void *data;
};

static long crus_se_shared_ioctl(struct file *f, unsigned int cmd, void __user *arg) {

	// size controlled by user
	if (copy_from_user(&size, arg, sizeof(size))) {
	
		pr_err("%s: copy_from_user (size) failed\n", __func__);
		
		result = -EFAULT;
		
		goto exit;
	
	}

	/* Copy IOCTL header from usermode */
	// buffer overwrite since size is provided by the user
	// crus_se_hr is a global variable
	if (copy_from_user(&crus_se_hdr, arg, size)) {
		pr_err("%s: copy_from_user (struct) failed\n", __func__);
		result = -EFAULT;
		goto exit;
	}
	
	bufsize = crus_se_hdr.data_length;
	io_data = kzalloc(bufsize, GFP_KERNEL);
	if (!io_data) {	
		pr_err("%s: Memory allocation failed!\n", __func__);
		return -ENOMEM;
	}
	
	switch (cmd) {
		
		case CRUS_SE_IOCTL_GET:
		
			switch (crus_se_hdr.module_id) {
			
			case CRUS_MODULE_ID_RX:
			
				port = cirrus_ff_port;
			
			break;
			
			default:
			
			pr_debug("%s: Unrecognized port ID (%d)\n", __func__,
			
			crus_se_hdr.module_id);
			
			port = cirrus_ff_port;
			
			}

			// this function also have interger overflow vunerability
			// since we control the bufsize.
			crus_get_param(port, CIRRUS_SE, crus_se_hdr.param_id, bufsize, io_data);
			// buffer over read since we can control bufsize
			result = copy_to_user(crus_se_hdr.data, io_data, bufsize);
		
		}
		case CRUS_SE_IOCTL_SET:
		// bufsize can be controlled so we can do buffer overwrite
		result = copy_from_user(io_data, (void *)crus_se_hdr.data, bufsize);
		// buffer overwrite occurs in this case
		crus_set_param(port, CIRRUS_SE, crus_se_hdr.param_id, bufsize, io_data);
```

```C

static int crus_get_param(int port, int module, int param, int length, void *data) {
...
	// integer overflow for length param
	crus_se_buffer_size = length + 24;
	crus_se_get_buffer = kzalloc(crus_se_buffer_size, GFP_KERNEL);
	if (!crus_se_get_buffer) {
		pr_err("%s: Memory allocation failed!\n", __func__);
		mutex_unlock(&crus_se_get_param_lock);
		return -ENOMEM;
	}
...

}
```


[code](https://github.com/MotorolaMobilityLLC/motorola-kernel-modules/blob/android-11-release-rri/sound/soc/codecs/msm-cirrus-playback.c)


## Bug 2

[code](https://github.com/MotorolaMobilityLLC/motorola-kernel-modules/blob/android-11-release-rrb/drivers/input/touchscreen/synaptics_tcm_mmi/synaptics_tcm_device.c)

```C
static ssize_t device_read(struct file *filp, char __user *buf, size_t count, loff_t *f_pos) {

	// count can be controlled by user, and this can lead to
	// out of bounds read
	if (copy_to_user(buf, device_hcd->resp.buf, count)) {
	
		LOGE(tcm_hcd->pdev->dev.parent, "Failed to copy data to user space\n");
		UNLOCK_BUFFER(device_hcd->resp);
		retval = -EINVAL;
		goto exit;
	
	}
}
```

```C
static ssize_t device_write(struct file *filp, const char __user *buf, size_t count, loff_t *f_pos) {

...
	// out of bounds write
	if (copy_from_user(device_hcd->out.buf, buf, count)) {
...
}
```

### Bug 3

[link](https://github.com/MotorolaMobilityLLC/motorola-kernel-modules/blob/ca925c7c061bbfdec1b43a4ed07f8951561739e6/drivers/input/touchscreen/nova_36525_mmi/nt36xxx_mp_ctrlram.c#L1567)

```C

if (count > (40 * (40 + 4) * 2 + 7)) {
	NVT_ERR("error count=%zu\n", count);
	return -EFAULT;
}

// count variable is sanitized
if (copy_from_user(str, buff, count)) {

data_len = (int32_t)((str[1] << 8) + str[2]);
// this can leak kernel data to userland program.
ret = copy_to_user(buff, str, data_len);
```

## Bug 4

```C
static int32_t cci_intf_xfer(
		struct msm_cci_intf_xfer *xfer, // tainted
		int cmd) //tainted
{
	case MSM_CCI_INTF_WRITE:
		/* write */
		// integer overflow
		reg_conf_tbl = kzalloc(xfer->data.count *
				sizeof(struct cam_sensor_i2c_reg_array),
				GFP_KERNEL);
		if (!reg_conf_tbl) {
			rc = -ENOMEM;
			goto release;
		}
		addr = xfer->reg.addr;
		// Bug "xfer->data.count"
		for (i = 0; i < xfer->data.count; i++) {
			reg_conf_tbl[i].reg_addr = addr++;
			reg_conf_tbl[i].reg_data = xfer->data.buf[i];
			reg_conf_tbl[i].delay = 0;
		}
		cci_ctrl.cmd = MSM_CCI_I2C_WRITE;
		cci_ctrl.cfg.cci_i2c_write_cfg.reg_setting = reg_conf_tbl;
		cci_ctrl.cfg.cci_i2c_write_cfg.addr_type =
			(xfer->reg.width == 1 ?
				CAMERA_SENSOR_I2C_TYPE_BYTE :
				CAMERA_SENSOR_I2C_TYPE_WORD);
		cci_ctrl.cfg.cci_i2c_write_cfg.data_type =
			CAMERA_SENSOR_I2C_TYPE_BYTE;
		cci_ctrl.cfg.cci_i2c_write_cfg.size = xfer->data.count;
		rc = v4l2_subdev_call(cam_cci_get_subdev(),
				core, ioctl, VIDIOC_MSM_CCI_CFG, &cci_ctrl);
		kfree(reg_conf_tbl);
		if (rc < 0) {
			pr_err("%s: cci write fail (%d)\n", __func__, rc);
			goto release;
		}
		rc = cci_ctrl.status;
		break;
	default:
		pr_err("%s: Unknown command (%d)\n", __func__, cmd);
		rc = -EINVAL;
		break;
	}
}

static long cci_intf_ioctl(struct file *file, unsigned int cmd,
		unsigned long arg)
{
	struct msm_cci_intf_xfer xfer;
	int rc;

	pr_debug("%s cmd=%x arg=%lx\n", __func__, cmd, arg);

	switch (cmd) {
	case MSM_CCI_INTF_READ:
	case MSM_CCI_INTF_WRITE:
		if (copy_from_user(&xfer, (void __user *)arg, sizeof(xfer)))
			return -EFAULT;
		rc = cci_intf_xfer(&xfer, cmd);
		if (copy_to_user((void __user *)arg, &xfer, sizeof(xfer)))
			return -EFAULT;
		return rc;
	default:
		return -ENOIOCTLCMD;
	}
}
	
```

## Bug 5
[link](https://github.com/MotorolaMobilityLLC/motorola-kernel-modules/blob/88a1aee809588023f670433e5710199f6e8de9b4/sound/soc/codecs/tfa98xx.c#L1007)
```C
static ssize_t tfa98xx_dbgfs_rpc_send(struct file *file,
				     const char __user *user_buf,
				     size_t count, loff_t *ppos)
{
	struct i2c_client *i2c = file->private_data;
	struct tfa98xx *tfa98xx = i2c_get_clientdata(i2c);
	nxpTfaFileDsc_t *msg_file;
	enum Tfa98xx_Error error;
	int err = 0;

	if (tfa98xx->tfa == NULL) {
		pr_debug("[0x%x] dsp is not available\n", tfa98xx->i2c->addr);
		return -ENODEV;
	}

	if (count == 0)
		return 0;
	// overflow
	/* msg_file.name is not used */
	msg_file = kmalloc(count + sizeof(nxpTfaFileDsc_t), GFP_KERNEL);
	if ( msg_file == NULL ) {
		pr_debug("[0x%x] can not allocate memory\n", tfa98xx->i2c->addr);
		return  -ENOMEM;
	}
	msg_file->size = count;
	// overwrite
	if (copy_from_user(msg_file->data, user_buf, count))
		return -EFAULT;

	mutex_lock(&tfa98xx->dsp_lock);
	// exit without doing anything else, very clean overwrite
	if ((msg_file->data[0] == 'M') && (msg_file->data[1] == 'G')) {
		error = tfaContWriteFile(tfa98xx->tfa, msg_file, 0, 0); /* int vstep_idx, int vstep_msg_idx both 0 */
		if (error != Tfa98xx_Error_Ok) {
			pr_debug("[0x%x] tfaContWriteFile error: %d\n", tfa98xx->i2c->addr, error);
			err = -EIO;
		}
	} else {
		error = dsp_msg(tfa98xx->tfa, msg_file->size, msg_file->data);
		if (error != Tfa98xx_Error_Ok) {
			pr_debug("[0x%x] dsp_msg error: %d\n", tfa98xx->i2c->addr, error);
			err = -EIO;
		}
	}
	mutex_unlock(&tfa98xx->dsp_lock);

	kfree(msg_file);

	if (err)
		return err;
	return count;
}
```

### Bug 6

[link](https://github.com/OnePlusOSS/android_kernel_modules_and_devicetree_oneplus_sm6375/blob/86fedfea1aec0d81009acb6891dc8fc2199d0de4/source/android/kernel/msm-5.4/techpack/audio/asoc/codecs/sia81xx/sia81xx_tuning_if_dev.c#L158)
https://www.twblogs.net/a/5e4f235dbd9eee101df6f993/?lang=zh-cn
```C
static ssize_t sia81xx_tuning_if_dev_write(struct file *fp, 
	const char __user *buf, size_t count, loff_t *f_pos)
{
	struct cal_module_unit *module = NULL;

	pr_debug("[debug][%s] %s: run !! \r\n", LOG_FLAG, __func__);

	module = is_file_exist(fp);
	if(NULL == module)
		return -ENOENT;
	// buffer overflow
	// "module->cmd" is of size 4096 and count is 
	// taken from user
	if(0 != copy_from_user(module->cmd, buf, count))
		return 0;

	/* fixed me : to option "state" should be in only one place, 
	 * but there will change the "state" value, it is not safe */
	atomic_set(&module->state, MODULE_STATE_WRITEN);
	
	return count;
}

```

# Baseband Bugs

- [Research Blog link](https://research.checkpoint.com/2022/vulnerability-within-the-unisoc-baseband/)
	- This bug show message parsing bug in NAS layer of modem
	- The bug is in UNISOC T700 chip.
	- Device used for testing is Motorola Moto G20 (XT2128-2) with the Android January 2022 update (RTAS31.68.29)
	- [Firmware Download link](https://mirrors.lolinet.com/firmware/motorola/java/official/RETAIL/)
		- use this tool to unpack PAC firmware file
		```bash
		git clone https://github.com/divinebird/pacextractor
		cd pacextractor
		make
		```




# Xiaomi Phones

## Device Redmi K70E

- Repo [link](https://github.com/MiCode/mtkcam-kernel_device_modules/)

### Bug #1

```C
static void module_notify(struct mtk_hcp *hcp_dev, 
						  struct share_buf *user_data_addr) //user controlled
{
[...]
	swbuf_data = (struct img_sw_buffer *)user_data_addr->share_data;
[...]
	if (gce_buf)
		swfrm_info = (struct swfrm_info_t *)(gce_buf + (swbuf_data->offset));
		// "offset" is user-controlled an can lead to out of bound access vulnerability
	
	if (swfrm_info && swfrm_info->is_lastfrm)
		req = (struct mtk_imgsys_request *)swfrm_info->req_vaddr; // more offset

	if (req) {
		req_fd = req->tstate.req_fd;
		req_stat = req->req_stat;
	}

	if (req_stat) {
		*req_stat = *req_stat + 1; // increment of the value present at the location.
		if (hcp_dbg_enable())
		dev_dbg(hcp_dev->dev, "req:%d req_stat(%p):%llu\n",
			req_fd, req_stat, *req_stat);
	}
}
```