# Building a Cross Toolchain for Raspberry Pi 3 Model B

## Overview

A toolchain is a comprehensive set of software development tools essential for various stages of the software development process. This guide explains the process of building a cross toolchain for the Raspberry Pi 3 Model B.

## Components of the Toolchain

### 1. Binutils

Binary utilities play a crucial role in creating and managing binaries. Key tools include:

- `as` (assembler)
- `ld` (linker)
- `ar` (archive)
### 2. C/C++ Libraries

Multiple C libraries are available for use, such as:

- glibc
- uClibc
- newlib

A toolchain typically includes one of these libraries.

### 3. C/C++ Compiler

The GNU Compiler Collection (GCC) serves as the primary C/C++ compiler within the toolchain.

### 4. GDB Debugger

While not an integral part of the toolchain, the GNU Debugger (GDB) is a valuable tool for debugging purposes.

## Cross Toolchain Usage

A cross toolchain is specifically designed to generate binaries for a target system. In this scenario:

- **Host System:** A development PC (host system), typically running Ubuntu 16.04 or later, is utilized to build Embedded Linux using the cross toolchain.
  
- **Target System:** The target system is the Raspberry Pi 3 Model B.


# instructions for Building Cross-Compilers with crosstool-ng for Raspberry Pi 3 Model B.

When it comes to building cross-compilers, crosstool-ng stands out as a quick and easy solution. It provides a user-friendly menuconfig-like interface for selecting compiler settings. The tool then automates the process of downloading, patching, configuring, building, and installing the cross-compiler. Follow the steps below to get started:


## Building the Cross-Compiler

1. **Download crosstool-ng Repository:**

   ```bash
   git clone https://github.com/crosstool-ng/crosstool-ng.git
2. **Configure and compile the build tool**   
   ```bash
   ./bootstrap
   ./configure --prefix=${PWD}
   make
   make install
   bin/ct-ng list-samples
   bin/ct-ng  aarch64-rpi3-linux-gnu
3. **open menuconfig for Raspberry Pi 3 Toolchain Configuration Options**   
   ```bash   
   bin/ct-ng menuconfig
    ![Screenshot_12](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/c303a279-b49a-4f34-a2e1-575c723bad11)
- Paths and misc options
    - Check "Try features marked as EXPERIMENTAL"
    - Set "Prefix directory" to "/opt/cross/x-tools/${CT_TARGET}"
    ![Screenshot_22](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/c2b07ff9-3018-4437-a5e6-32a497d4585f)

- Target options
    - Set "Target Architecture" to "arm"
    - Set "Default instruction set mode" to "arm"
    - Set "Endianness" to "Little endian"
    - Set "Bitness" to "64-bit"
    - Set "Emit assembly for CPU" to "cortex-a53"
    - Set "Use specific FPU" to "vfp"
    - Set "Floating point" to "hardware (FPU)"
    - Check "Use EABI"
    ![Screenshot_13](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/184a8e48-f412-4307-aaf9-0804857bad8d)

- Toolchain options
    - Set "Tuple's vendor string" to "rpi"
    ![Screenshot_14](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/96399fb6-d316-4cde-979a-314c62148b68)    
- Operating System
    - Set "Target OS" to "linux"
    - version of linux 6.4.16
    ![Screenshot_15](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/d8ac2888-4f2b-4dce-a9b5-96dc7dfd2508)
- Binary utilities
    - Set "Binary format" to "ELF"
    - Set "binutils version" to "2.30"
    ![Screenshot_16](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/44415c8b-78b4-4152-bfac-b6a0dcb9a827)
- C-library
    - Set "C library" to "glibc"
    - Set "glibc version" to "2.38"
    ![Screenshot_17](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/ca1f86ba-3e2c-4ba8-bd63-717a1bea2e00)    
- C compiler
    - Check "Show Linaro versions"
    - Set "gcc version" to "10.5.0"
    - Set "gcc extra config" to "--with-float=hard"
    - Check "Link libstdc++ statically into the gcc binary"
    - Check "C++" under "Additional supported   languages"
    ![Screenshot_18](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/03eea205-0a54-47ff-85ce-77ec37832ada)

- Companion tools
    - Set m4,cmake,automake,autoconfig
    ![Screenshot_21](https://github.com/Mahmoud-Ismail98/Cmake-Moatasem-Elsayed/assets/63348980/c3f00d96-9304-4f46-b7e0-51d3a4e2024a)

3. **Build toolchain**
   ```bash
    ./ct-ng build

-   the toolchain is installed in ~/x-tools directory. Add this $HOME/x-tools/aarch64-rpi3-linux-gnu/bin/ to PATH env varibale in .bashrc so that it can be detected for compiling the binaries.

    ```bash
    echo 'export PATH=$PATH:$HOME/x-tools/aarch64-rpi3-linux-gnu/bin/'  ~/.bashrc
    source ~/.bashrc

4. **Test the toolchain**

-   Write a sample C program to cross compile with toolchain
file : test.c
Configure system to use new compiler
Your compiler will be located in /home/<yourname>/crosscompile (or whereever you specified in the configuration). 
Now if you run:
    ```bash    
    aarch64-rpi3-linux-gnu-gcc  test.c -o test

5. **Check the binary architecture**
    ```bash
    file test
-   You should see output similar to the following. The output must have words ARM aarch64 and ld-linux-aarch64.so.1.

-   test: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, inter