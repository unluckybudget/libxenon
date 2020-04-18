Forked from: https://github.com/xenia-project/libxenon, which was created by:
https://github.com/Free60Project/libxenon

This repository contains changes to the toolchain build script to get it operational in 2020. Tested on Ubuntu 19.10. I also reduced the crashdump timeout from 30 seconds to 10 seconds, and XeLL reboot to SMC reboot. ISO9660 had a minor bug with strncmp which was also fixed.

Ubuntu Depends:
  automake autoconf build-essential libmpfr-dev libmpc-dev libgmp-dev bison bc
  
Build the Toolchain

  	Clone this repository
	
  	cd libxenon/toolchain
  
	nano buid-xenon-toolchain - set PARALLEL=jx where x is the amount of CPU cores available.
  
  	sudo mkdir /usr/local/xenon
	sudo chown -R USER:USER /usr/local/xenon
  
	./build-xenon-toolchain toolchain
  
  If you're watching the terminal, you will see 2 occurances the below error, one in each GCC stage:
  
	"cc1: error: no include path in which to search for stdc-predef.h"
  
	This can be safely disregarded.
  
  Once the script finishes (~1 hour with a Core2Duo processor), modify ~/.bashrc as instructed.
  
  Once bashrc is modified run: ./build-xenon-toolchain libxenon


Installing extra libraries for added functionality

	./build-xenon-toolchain libxenon
  	Will build and install libxenon library.

	./build-xenon-toolchain filesystems
  	This will build ext2, xtaf, and ntfs libraries.

	./build-xenon-toolchain freetype
  	Will build the freetype font engine.

	./build-xenon-toolchain zlib
  	Run this, let the script fail, then cd into the zlib directory.
  	Run export CC=xenon-gcc
  	Run ./configure --prefix=/usr/local/xenon
 	Run make && make install

	./build-xenon-toolchain libpng
  	Will install libpng, a library for loading and displaying .png images.
  
	./build-xenon-toolchain libs
  	Will install bzip2 compression library, and bin2s
  
  libSDL 1.2
  	
	git clone https://github.com/LibXenonProject/libSDLXenon
  	cd libSDLXenon
  	make -f Makefile.xenon
  
  An undefined reference to ceil() will be thrown despite the correct includes. To fix, edit the file reporting the error
  and add a function definition for ceil(). 
  
libZLX - A custom C++ library for interfacing with libxenon, for the purpose of building CLI and GUI apps.

	git clone https://github.com/LibXenonProject/zlx-library
  	cd zlx-library
  	make -f Makefile_lib && make install
  

To build other libraries not listed, keep in mind all dependencies must be manually resolved.

  	Download the source tarball for whatever library you want to cross compile.
  
  	Open a shell and export the following variables.
  
  	export CC=xenon-gcc
  
  	export CFLAGS="export CFLAGS="-mcpu=cell -mtune=cell -m32 -fno-pic \
  	-mpowerpc64 $DEVKITXENON/usr/lib/libxenon.a -L$DEVKITXENON/xenon/lib/32/ -T$DEVKITXENON/app.lds \
  	-u read -u _start -u exc_base -L$DEVKITXENON/usr/lib -I$DEVKITXENON/usr/include"
  
  	export LDFLAGS=$CFLAGS
  
  	./configure --disable-shared --enable-static --prefix=$DEVKITXENON/usr --host=ppc-elf
  
  	Attempt to make && make install
  
  The configure options listed for example are bare minumum requirements. It's likely it will not satisfy 
  heftier libraries. If there are errors, or you would simply like to fine-tune the library, ./configure --help 
  will almost always display options specific available to the library being compiled.
