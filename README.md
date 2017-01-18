#XU-3 Setup

Official tutorial can be found at http://odroid.com/dokuwiki/doku.php?id=en:odroid-xu3

##Preparation

####Boot mode set

If you boot from sd card, turn your DIP-SW(sw1) to 1(off) 2(on).(down is off, up is on)
This pic shows everything.
http://odroid.com/dokuwiki/lib/exe/detail.php?id=en%3Axu3_bootmode_configuration&media=en:others:dip_sw_xu3_sd.png
	
####Serial port

official tutorial http://odroid.com/dokuwiki/doku.php?id=en:usb_uart_kit		

##Flash

1. Download xu3 ubuntu image from http://odroid.in/ubuntu_14.04lts/

2. Flash this image to your sd card by running 
    
	sudo dd if=`.../**.img` of=`/dev/sd*` bs=1M conv=fsync

	sync
	    

##Customization

The image we flashed on to sd card already contains the u-boot ,kernel and filesystem.
If you want to use your costomized u-boot or kernel do the following steps.

####Toolchain

Download these compile tools for your compilation.

For u-boot, http://dn.odroid.com/toolchains/gcc-linaro-arm-linux-gnueabihf-4.7-2013.04-20130415_linux.tar.bz2
For host kernel, http://dn.odroid.com/ODROID-XU/compiler/arm-eabi-4.6.tar.gz

Download the newer toolchain for kernel and u-boot, http://releases.linaro.org/14.11/components/toolchain/binaries/arm-linux-gnueabihf/gcc-linaro-4.9-2014.11-x86_64_arm-linux-gnueabihf.tar.xz
    
##### Update:      http://releases.linaro.org/components/toolchain/binaries/4.9-2016.02/arm-linux-gnueabihf/gcc-linaro-4.9-2016.02-x86_64_arm-linux-gnueabihf.tar.xz
    
####Compile u-boot

Running the following commands:
    
	tar jxf gcc-linaro-arm-linux-gnueabihf-4.7-2013.04-20130415_linux.tar.bz2
	export ARCH=arm
	export CROSS_COMPILE=[path to your compile toolchain folder]/bin/arm-linux-gnueabihf-
	cd u-boot
	make odroid_config
	make
	
Then, flash the u-boot to your sd card by running
	
	sudo dd if=u-boot.bin of=/dev/sd* seek=63
	Here * could be a,b,c... You can check this by running `dmesg` command 
	after you plug your sd card to your computer.

####host-kernel

Running the following commands
    
    tar xvfz arm-eabi-4.6.tar.gz
    export ARCH=arm
	export CROSS_COMPILE=[path to your compile toolchain folder]/arm-eabi-4.6/bin/arm-eabi-
	cd host-kernel
	make odroidxu3_kvm_defconfig
	make 
	make modules_install
	
	
Then, replace the kernel image from arch/arm/boot directory to your mounted Boot partition.

    cp arch/arm/boot/zImage    /.../boot/zImage
	
Note: You can also compile the kernel directly on your board using the source without setting the toolchain.
	

####Guest-kernel
Did this directly on the board.
Running the following commands:
    
	cp configs/guest_config guest-kernel/.config
	make -j9 zImage dtbs
	mkdir ~/kvm
	cp arch/arm/boot/zImage ~/kvm
	cp arch/arm/boot/dts/rtsm_ve-cortex_a15x1.dtb ~/kvm
	
#####Qemu
Download the qemu from http://odroid.in/guides/ kvm/qemu.tar.xz
	
#####Image
Downlaod the image from http://odroid.in/guides/kvm/lubuntu.xu3.img.gz
	
#####Running script 
Download the Running Script from http://odroid.in/guides/kvm/lubuntu.sh
	

Then run the script by ./lubuntu.sh

