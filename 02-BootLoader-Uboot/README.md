  # Cross Compiling U-boot Bootloader for Raspberry Pi 3 Model B

## Introduction
When making changes to the Linux kernel, drivers, or file system, it can be cumbersome to manually copy the files to an SD card and insert it into a Raspberry Pi. To streamline this process, we can automate it using U-boot.

U-boot is utilized for the following purposes:
- Initializing basic hardware.
- Loading the kernel and device tree from non-volatile storage or network (e.g., through TFTP).
- Instructing the Linux kernel to load a network file system and pass other essential command-line parameters.

## Raspberry Pi 3 with 64-bit U-Boot
This guide demonstrates how to utilize U-Boot to load the Linux kernel and boot a Raspberry Pi 3.

### Requirements
To proceed, you'll need:
- Cross Toolchain for Raspberry Pi 3 Model B
- Access to the U-Boot source code.
- QEMU Emulator for test u-boot

## 1. Getting the Source Code

Clone the U-Boot git repository and switch to the desired branch:

    git clone --depth 1 --branch v2017.11 git://git.denx.de/u-boot.git v2017.11

![Screenshot_2](https://github.com/Mahmoud-Ismail98/manual-customized-linux-image-for-a-raspberry-pi/assets/63348980/2a515f67-177b-4127-8302-3b9352382a8f)

## 2. Compile U-Boot for the Raspberry Pi 3

    make rpi_3_defconfig
    make CROSS_COMPILE=aarch64-rpi3-linux-gnu-

![Screenshot_3](https://github.com/Mahmoud-Ismail98/manual-customized-linux-image-for-a-raspberry-pi/assets/63348980/c443632a-a0e8-45a5-9415-065b61d51c37)

## 3. Installing U-Boot on QEMU Emulator

To test U-Boot on the QEMU emulator:

    qemu-system-aarch64 \
    -M raspi3b \
    -cpu cortex-a72 \
    -append "rw earlyprintk loglevel=8 console=ttyAMA0,115200" \
    -kernel u-boot.bin \
    -m 1G -smp 4 \
    -serial stdio \
    -usb -device usb-mouse -device usb-kbd \
    -device usb-net,netdev=net0 \
    -netdev user,id=net0,hostfwd=tcp::5555-:22 \

![Screenshot_4](https://github.com/Mahmoud-Ismail98/manual-customized-linux-image-for-a-raspberry-pi/assets/63348980/07f3a98b-470e-41c8-9e37-71e74732f63d)

![Screenshot_5](https://github.com/Mahmoud-Ismail98/manual-customized-linux-image-for-a-raspberry-pi/assets/63348980/bb136b5b-b66d-499f-a992-0b62d1121a95)
