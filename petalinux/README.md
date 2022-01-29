# PetaLinux

PetaLinux (or Petalinux SDK) is a Xilinx development tool that contains everything necessary to build, develop, test, and deploy embedded Linux systems (Zynq and ZynqMP boards). For more details see the Reference Guide [UG114 v2019.1](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2019_1/ug1144-petalinux-tools-reference-guide.pdf).

## Quick Install

The following guidelines have been tested wtih Ubuntu Linux 18.04.6 LTS. For a complete list of supported OSes see the Reference Guide [UG114 v2019.1](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2019_1/ug1144-petalinux-tools-reference-guide.pdf).

Install the required packages:
```
sudo apt-get install -y gcc git make net-tools libncurses5-dev tftpd zlib1g-dev libssl-dev flex bison libselinux1 gnupg wget diffstat chrpath socat xterm autoconf libtool tar unzip texinfo zlib1g-dev gcc-multilib build-essential libsdl1.2-dev libglib2.0-dev zlib1g:i386 screen pax gzip
```

Download the [PetaLinux 2019.1 installer](https://www.xilinx.com/member/forms/download/xef.html?filename=petalinux-v2019.1-final-installer.run). Autentication is required.

Run the PetaLinux installer:
```
sudo mkdir -p /tools/Xilinx/Petalinux/2019.1
sudo chown -R $USER:$USER /tools/Xilinx/Petalinux/2019.1
chmod 755 ./petalinux-v2019.1-final-installer.run
./petalinux-v2019.1-final-installer.run /tools/Xilinx/Petalinux/2019.1
```

Check the setup of the PetaLinux working environment:
```
source /tools/Xilinx/Petalinux/2019.1/settings.sh
echo $PETALINUX
```

## Overview of the PetaLinux Design Flow

| Step # | Step Description | Tool / Workflow |
| :---: | --- | --- |
| 1. | Hardware Platform Creation (_for custom hardware only_) | Vivado design tools |
| 2. | Create PetaLinux Project | `petalinux-create -t project` |
| 3. | Initialize PetaLinux Project (_for custom hardware only_) | `petalinux-config --get-hw-description` |
| 4. | Configure System-Level Options | `petalinux-config` |
| 5. | Create User Components | `petalinux-create -t COMPONENT` |
| 6. | Configure the Linux Kernel | `petalinux-config -c kernel` |
| 7. | Configure the Root Filesystem | `petalinux-config -c rootfs` |
| 8. | Build the System | `petalinux-build` |
| 9. | Package for Deploying the System | `petalinux-package` |
| 10. | Boot the System for Testing | `petalinux-boot` |

## Expedite Project Creation

- Just run:
  ```
  make create-kernel-image
  ```

- Mount the SD card on your host machine.

- Be sure that the first partition of the SD card is in FAT32 format.

- Run:
  ```
  make cp-image
  ```

- Unmount the SD card.

- See *Boot from SD Card* section.

## Quick Project Creation

- Setup the PetaLinux working environment:
  ```
  source envsetup
  ```

- Set an environment variable for the project directory:
  ```
  export PROJECT_DIR=ultra96v2_oob_2019_1
  ```

- Download the Ultra96-v2 reference Board Support Package (BSP). See the additional instructions in the directory `bsp`:
  ```
  cd bsp
  make
  cd -
  ```

- Create the project from the ZCU106 BSP:
  ```
  petalinux-create -t project -s bsp/ultra96v2_oob_2019_1.bsp
  ```

- Configure the project given a hardware configuration:
  ```
  cd $PROJECT_DIR
  petalinux-config --get-hw-description=../../hdf --silentconfig
  cd -
  ```

- Build the project:
  ```
  cd $PROJECT_DIR
  petalinux-build
  cd -
  ```

- Create the boot image:
  ```
  cd $PROJECT_DIR
  petalinux-package --boot --fsbl images/linux/zynqmp_fsbl.elf --fpga images/linux/system.bit --u-boot images/linux/u-boot.elf --pmufw images/linux/pmufw.elf --force
  cd -
  ```

- Mount the SD card on your host machine.

- Copy the boot image into the `boot` directory on the SD card whose partition should be in FAT32 format:
  ```
  cd $PROJECT_DIR
  cp images/linux/BOOT.BIN /media/$USER/boot
  cp images/linux/image.ub /media/$USER/boot
  cd -
  ```

- Copy the `rootfs` into the `root` directory on the SD card whose partition should be in EXT4 format:
  ```
  cd $PROJECT_DIR
  cd images/linux
  sudo tar xvfzp rootfs.ext4.gz --directory /media/$USER/root
  cd - 
  ```

- Edit the SSID and password to be able to connect to the WiFi:
  ```
  cd $PROJECT_DIR
  vim /media/$USER/root/home/root/wpa_supplicant.conf
  cd -
  ```

- Unmount the SD card.

- See *Boot from SD Card* section.

## Boot from SD Card

- Connect the serial port on the board to your workstation.

- Open a console on the workstation and start the preferred serial communication program:
  ```
  sudo minicom -D /dev/ttyUSB1
  ```
  The baud rate must be set to 115200 on that console.

- Power off the board.

- Set the boot mode of the board to SD boot.

- Plug the SD card into the board.

- Power on the board.

- Watch the serial console.

- Login with
  - user `root`
  - password `root`
 
- Start enable the WiFi
  ```
  sudo ./wifi.sh
  ```
  
