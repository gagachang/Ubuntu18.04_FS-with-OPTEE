# 1. Compile the OP-TEE raspberry-pi version
I compiled the OP-TEE 3.6.0 rpi3 version.

# 2. Download Linaro Debian-based Rootfs image
The image can be downloaded from here: https://releases.linaro.org/debian/images/

I choose `developer-arm64/18.04/linaro-stretch-developer-20180416-89.tar.gz` version.

# 3. Decompress the above file
```
/home$ mkdir linaro-rootfs
/home$ sudo tar zxvf linaro-stretch-developer-20180416-89.tar.gz -C /home/linaro-rootfs
```

The contents of linaro-rootfs will be put in `/home/linaro-rootfs/binary/`

# 4. Decompress the kernel module into rootfs
```
/home$ cd optee/module_output
     $ sudo find . | sudo cpio -pudm /home/linaro-rootfs/binary
```

# 5. Copy the needed file into the new linaro-rootfs
```
/home$ cd optee/out_br/target                                       // Default rootfs by buildroot
     $ cd lib
     $ sudo cp -r optee_armtz /home/linaro-rootfs/binary/lib        // TAs of OP-TEE
     $ cd .. && cd usr/lib
     $ sudo cp libteec.so* /home/linaro-rootfs/binary/usr/lib       // libraries of OP-TEE
     $ cd .. && cd bin
     $ sudo cp optee_example_* /home/linaro-rootfs/binary/usr/bin   // CAs in Linux
     $ sudo cp xtest /home/linaro-rootfs/binary/usr/bin             // xtest binary of OP-TEE
     $ cd .. && cd sbin
     $ sudo cp tee-supplicant /home/linaro-rootfs/binary/usr/sbin   // tee-supplicant in Linux
```

# 6. Partition and format the SD card and put the files into it

Insert the SD card into the computer.

Find the location of that SD card:
```
$ sudo fdisk -l
```
[]()
```
/home$ cd optee/build
     $ make img-help