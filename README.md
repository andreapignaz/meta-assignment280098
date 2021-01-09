# meta-assignment280098
Assignment for the Politecnico di Torino course, "Operating Systems for Embedded Systems"

## Target machine
These Yocto layers have been designed (and tested) to work on the Raspberry Pi 3B.  

## Add the recipes to Yocto and build the Linux image
1. Clone this repository inside your Yocto directory:
```bash
git clone https://github.com/andreapignaz/meta-assignment280098.git
```
2. Go inside the rpi3 build environment:
```bash
source oe-init-build-env build_rpi3
```
3. Add the cloned layer to Poky:
```bash
bitbake-layers add-layer ../meta-assignment280098
```
4. Add the new recipes to the image to be deployed: in the file conf/local.conf inside the build_rpi3 directory, add the following lines:
```
IMAGE_INSTALL_append = " heartbeatapplication"
IMAGE_INSTALL_append = " heartbeatmodule"
KERNEL_MODULE_AUTOLOAD += "heartbeatmodule"
```
5. Run bitbake:
```bash
bitbake core-image-full-cmdline 
```
6. Flash the microSD with the image (replace sdX with the correct device identifier):
```bash
sudo dd if=tmp/deploy/images/raspberrypi3/core-image-full-cmdline-raspberrypi3.rpi-sdimg of=/dev/sdX bs=1M
```

## Use the driver and the application
After the Raspberry Pi did boot up:
1. Read /proc/devices to identify the _mymod\_dev_ kernel module, and take note of major and minor number.
```bash
cat /proc/devices | grep mymod
```
2. Create the device file using _mknod_
```bash
mknod /dev/mymod c 241 0
```
3. Launch the heartbeat application:
```bash
heartbeatapplication
```
