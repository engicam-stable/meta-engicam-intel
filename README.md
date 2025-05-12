meta-engicam-intel
==================

## intro 

This BSP provides Engicam hardware specific settings, libraries and applications.

It's based on Yocto scarthgap distro with linux 6.6 kernel

Machines available: 

- smarcore-ehl
- smarcore-ehl-rt

## Table of contents
1. [BSP Dependecis](#bspdependecy)
2. [Set up the build host system](#setupsystem)
3. [Fetch the sources](#fetchsource)
3. [Build the image](#buildimage)

## 1. BSP Dependencies  <a name="bspdependecy"></a>

```text
	URI: git://git.openembedded.org/bitbake
	branch: scarthgap

	URI: git://git.openembedded.org/openembedded-core
	layer: meta
	branch: scarthgap

	URI: git://git.yoctoproject.org/meta-intel
	layer: intel
	branch: scarthgap

	URI: git://git.openembedded.org/meta-openembedded
	layer: openembedded
	branch: scarthgap
```

## 2. Set up the build host system <a name="setupsystem"></a>
	Install 64 bit Ubuntu 20.04 LTS.

	[Note: Ubuntu minimal installation is recommended, see [4]]

	Install required packages :
```bash	
	$ sudo apt update
	$ sudo apt upgrade
	$ sudo apt install gawk wget git diffstat unzip texinfo gcc \
		build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
		xz-utils debianutils iputils-ping python3-git python3-jinja2 \
		libegl1-mesa libsdl1.2-dev pylint3 xterm python3-subunit mesa-common-dev \
		zstd liblz4-tool
```

## 3. Fetch the sources for a new project <a name="fetchsource"></a>

```bash
	$ cd <your working directory>
	$ git clone git://git.yoctoproject.org/poky.git poky -b scarthgap
	$ cd poky
	$ git checkout 9c63e0c9646c61663e8cfc6b4c75865cd0cd3b34
	$ git clone git://git.yoctoproject.org/meta-intel.git -b scarthgap
	$ cd meta-intel
	$ git checkout 1a7ccdcaedc965f019a9263364437356b8a2ba29
	$ cd ..
	$ git clone git://git.openembedded.org/meta-openembedded -b scarthgap
	$ cd meta-openembedded
	$ git checkout e92d0173a80ea7592c866618ef5293203c50544c
```


## 4. Build the image  <a name="buildimage"></a>


Two basic image configurations are available :

	- core-image-sato
	- core-image-minimal


The minimal configuration is a commandline based minimalistic image. The sato 
image configuration includes desktop demo and tools.

  Start the build :
```bash
	$ cd poky
	$ source oe-init-build-env build
	$ bitbake-layers add-layer ../meta-intel
	$ bitbake-layers add-layer ../meta-engicam-intel
	$ bitbake-layers add-layer ../meta-openembedded/meta-oe
	$ bitbake-layers add-layer ../meta-openembedded/meta-python
	$ bitbake core-image-sato
```

Edit the  build/conf/local.conf file adding the engicam SOM board:

and set the MACHINE variable with the engicam machine name , es for SmarCore Elkhart :
      
    # for standard linux kernel
    MACHINE ?= "smarcore-ehl"
    
    or
    
    # for realtime  linux kernel    
    MACHINE ?= "smarcore-ehl-rt"

Remenber to Whitelist the commercial licenses for video encoding/decoding support inserting the license files entry:

    LICENSE_FLAGS_ACCEPTED = "commercial"
