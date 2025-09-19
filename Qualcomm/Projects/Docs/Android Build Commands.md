---
up: "[[Android Kernel Driver Fuzzing]]"
tags:
  - "#type/qcom"
  - "#type/pinned-docs"
related:
  - "[[Camera Driver Fuzzing]]"
status: done
created-date: 2024-12-18
summary: Build commands for Androids systems
---

## Commands

### Build Flashing

```bash
# device reboot
adb reboot bootloader
fastboot devices
fastboot reboot

cd <meta_root_path>

# Flash complete meta
# go to project Meta root directory
python <meta_root>/common/build/fastboot_complete.py

# Flash Vendor APPs
# go to project Meta root directory
python common/build/fastboot_complete.py --apps_vendor=<your_vendor_path>

# mount debugfs for kcov
adb shell mount -t debugfs debugfs /sys/kernel/debug

# Command to run in android devices to check enabled mitigations
sanitizer-status

# check if kcov is enabled
ls /sys/kernel/debug/kcov
```

## Camera Device Setup Commands

```bash
# set Camera Driver verbose level
echo 0x1000012 > /sys/module/camera/parameters/debug_mdl

# Generate debug value  from URL : http://camxsensor/logmask/kmd
adb shell "echo <debug value> > /sys/module/camera/parameters/debug_mdl"
# disable camera service
adb remount
adb shell "mount -o remount,rw /vendor"
adb shell "mount -o remount,rw /system"
adb shell 'cd /vendor/bin/hw && mv $(ls vendor.qti.camera.provider*) vendor.qti.camera.provider' || true
adb shell 'mv /vendor/bin/hw/qvrservice /vendor/bin/hw/qvrservice.orig' || true  
adb shell "cd /system/bin && mv cameraserver cameraserver.orig" || true

# print device logs on UART
pyterm.py ftdi:///?

# put the device on fastboot
python3 script/ftdi_gpio_ops.py -c fastboot -p ftdi://ftdi:4232:FT94MYT3/

# turn the device on
python3 script/ftdi_gpio_ops.py -c on -p ftdi://ftdi:4232:FT94MYT3/

# turn off the device
python3 script/ftdi_gpio_ops.py -c off -p ftdi://ftdi:4232:FT94MYT3/

# installing python ftdi dependencies, installing from source
git clone https://github.com/eblot/pyftdi.git
cd pyftdi/
pip3 install -r requirements.txt
python3 setup.py install

# apply cherry picked gerrits
python /local/mnt/workspace/qcom-dev/tools/lint_tools/lib/git/../../lib/git/cherrypickle.py --force apply /local/mnt/workspace/qcom-dev/pakala_build/vendor 5406284/4 ;; 5420402/3 ;; 5455467/2 ;; 5505400/1 ;; 5517951/1 ;; 5509960/2 ;; 5496404/17 ;; 5506926/2 ;; 5507751/5 ;; 55

# ssh agent
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_rsa

sudo sshfs -o allow_other,reconnect,ServerAliveInterval=15 -C rajeg@hu-rajeg-hyd:/local/mnt/workspace/AU540 /local/mnt/workspace/AU540
sudo sshfs -o allow_other,reconnect,cache=no,noauto_cache,ServerAliveInterval=15 -C mshelia@hyd-lablnxqpsi05:/local/mnt/workspace/QSBT/LA.VENDOR.15.4.0_03_13_2025_12_43_14 /local/mnt/workspace/pakala/AU646
```

### go-lang debugging

```bash

# compile a test-case with debug symbols and output is mytest.test
# this will output as a binary
go test -gcflags='all=-N -l' -c -v ./prog -o mytest.test

# run the single test-case `TestTransitivelyEnabledCalls` from the binary
dlv exec ./mytest.test -- -test.run ^TestTransitivelyEnabledCalls$
dlv exec ./mytest.test -- -test.run ^TestResourceCtors$ > data.txt


```

### Compiling Syzkaller

```bash

# Running the camera unit test cases
python3 internal_tools/scripts/dv_test/devtest.py -t cslunittest.CSLDeviceTestProbePacket

# Syzkaller build
./tools/syz-env make generate

# compile everything
./tools/syz-env make TARGETOS=linux TARGETARCH=arm64 CFLAGS=-Iexecutor/qcom_src/include

# compile only the executor, don't forget to push these binareis to target device
./tools/syz-env make TARGETOS=linux TARGETARCH=arm64 CFLAGS=-Iexecutor/qcom_src/include executor
```

### Seed Corpus generation

```bash

# Generate debug value  from URL : http://camxsensor/logmask/kmd
adb shell "echo <debug value> > /sys/module/camera/parameters/debug_mdl"
adb shell "echo 0x20022 > /sys/module/camera/parameters/debug_mdl"

# Generate Request manager
./bin/syz-mutate -arch arm64 -os linux -enable openat\$camx_req_mgr,ioctl\$VIDIOC_CAM_REQ_MGR_*,openat\$camx_req_mgr,ioctl\$VIDIOC_CAM_MGR_ALLOC_CMD

# generate grammar
./bin/syz-mutate -arch arm64 -os linux -enable openat\$camx_req_mgr,ioctl\$VIDIOC_CAM_REQ_*,openat\$camx_dev_tpg1,ioctl\$VIDIOC_CAM_TPG_*,syz_memcpy_off\$CAMX_TPG_CMD_COPY,syz_memcpy_off\$CAMX_DMA_CMD_COPY,syz_tpg_init_device

./bin/syz-mutate -arch arm64 -os linux -enable syz_init_device_buf,openat\$camx_dev_icp1,ioctl\$VIDIOC_CAM_ICP_*

# generate a sample corpus with specific call in mind
while true; do ./bin/syz-mutate -arch arm64 -os linux -enable openat\$camx_dev_sensor1,syz_init_device_buf,ioctl\$VIDIOC_CAM_SENSOR_*,syz_memcpy_off\$CAMX_SENSOR_* | grep cont_wr; done

# execute the sample program
./syz-execprog -cover -vv 5 -debug -threaded=0 -procs 1 tpg_test.txt
```

### Corpus Database commands 

```bash
# Database packing/unpacking

# syzkaller location : /local/mnt/workspace/mshelia/gopath/src/github.com/google/syzkaller
# including -os & -arch in newer verion fixed some bugs
./bin/syz-db -arch arm64 -os linux pack /local/mnt/workspace/qcom-dev/syz-fuzz-v2/syz-corpus/ manual-corpus.db

# merging databases. No additional file will be created: 
# The first file will be replaced by the merged result:
# syz-db merge <results database> <other database>

./bin/syz-db merge dst-corpus.db add-corpus.db* add-prog*
```

### Constant Extraction with syz-extract

```bash
# verify the kernel configuration of Android MTP
zcat /proc/config.gz | grep KCOV

# extract constant for driver
TARGET=sun script/extract.sh <build directory> <syzkaller directory> msm_camx.txt

# example
TARGET=sun script/extract.sh /local/mnt/workspace/qcom-dev/pakala_vendor_src/builddir1 /local/mnt/workspace/mshelia/gopath/src/github.com/google/syzkaller msm_camx.txt

# extracting constant for camera driver
SYZ_CLANG=1 ./bin/syz-extract -config /local/mnt/workspace/qcom-dev/pakala_vendor_src/out/msm-kernel-sun-consolidate/dist/.config -os linux -arch arm64 -sourcedir /local/mnt/workspace/qcom-dev/pakala_vendor_src/kernel_platform/msm-kernel/ -build -includedirs /local/mnt/workspace/qcom-dev/pakala_vendor_src/vendor/qcom/opensource,/local/mnt/workspace/qcom-dev/pakala_vendor_src/kernel_platform/msm-kernel,/local/mnt/workspace/qcom-dev/pakala_vendor_src/vendor/qcom/opensource/camera-kernel/include/uapi,/local/mnt/workspace/qcom-dev/pakala_vendor_src/vendor/qcom/opensource/camera-kernel/include/uapi/camera msm_camx.txt

# KASAN issue when generating constant
# disable CONFIG_KASAN in kernel_platform/out/msm-kernel-sun-consolidate/dist/.config
```

1. Sometime you will encounter following command value
	1. CAM_FLASH_PACKET_OPCODE_INIT = ???  
	2. CAM_FLASH_PACKET_OPCODE_NON_REALTIME_SET_OPS = ??? 
	3. it means there headers are missing for these variables either include these headers or define these variable if no such headers

## GCOV Coverage report generation

[[Linux kernel coverage reporting using GCOV]]

```bash
# Script to generate coverage
# https://github.qualcomm.com/LinuxSecurity/syzkaller/wiki/Enable-kernel-GCOV

cd /sys/kernel/debug/gcov

# pull gcov coverage data from device
adb shell "cp -r /d/gcov /data/local/tmp/; cd /data/local/tmp; tar -czf gcov.tar.gz gcov"; adb pull /data/local/tmp/gcov.tar.gz
tar -xf gcov.tar.gz

# match gcna file to corresponding gcno file
python3 ~/bin/gcov-kernel-module.py -b . -g ../tmp_data/gcov
 
alias llvm-gcov='/local/mnt/workspace/qcom-dev/pakala_build/tmp_data/android-ndk-r25c/toolchains/llvm/prebuilt/linux-x86_64/bin/llvm-cov gcov'

# covert gcov coverage to lcov format
lcov -c -d ../gcov -d bazel-cache -d out -d kernel_platform/out -o coverage.info --gcov-tool=/usr2/mshelia/bin/llvm-gcov --ignore-errors source,gcov

sed -i 's|/proc/self/cwd/||g' coverage.info
sed -i 's|msm-kernel|kernel_platform/msm-kernel|g' coverage.info

# covert lcov data to html data
genhtml -q -o cov source --prefix $PWD coverage.info

python3 -m http.server
```