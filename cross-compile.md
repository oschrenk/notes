# Cross Compile #

## OS X ##

Based on [Michael Phillips work](http://www.benmont.com/tech/crosscompiler.html):

- Install the latest [Command Line Tools for XCode](https://developer.apple.com/downloads/index.action).
- Install [Homebrew](http://mxcl.github.com/homebrew/):

Install

	brew tap homebrew/dupes
	brew install grep crosstool-ng mpfr gmp

You need a case-sensitive file system to work with. Fortunately, you can create one in Apple's Disk Utility and mount it. Create one that's at least 5 GB to start. Don't use spaces in your volume name. It will cause trouble.

	hdiutil create  -size 5g -nospotlight -volname "cross" -fs "Case-sensitive Journaled HFS+" -verbose ./cross-compiling
	hdiutil attach cross-compiling.dmg

Change working directory to the newly create volume

	cd /Volumes/cross

## In General ##

Initialize toolchain

	ct-ng arm-unknown-linux-gnueabi

Create configuration

	mkdir -p {tarballs,working,prefix}

Update the paths that crosstool-ng uses to the case-sensitive location you just created on the volume

	ct-ng menuconfig

While you're in there, turn off the build of Java and Fortran compilers, since you don't need them and they'll just cause more problems.

Build it.

	 ct-ng build

## Configuring the target platform ##

Know your target platform!

We're interested to build v8 for [Sony Xperia S](http://www.sonymobile.com/global-en/products/phones/xperia-s/)

- CPU: 1.5 GHz dual-core Scorpion
- GPU: Qualcomm Adreno 220

### Configuration ###

- Paths and misc options
	- use obsolete feature
	- Try features marked as EXPERIMENTAL
- Target options
	- Target Architecture: ARM
	- Architecture level: `armv4t`. `armv4t` is for the EP9302. other processors you would pick the right architecture here.
	- Floating point: Hardware (FPU)
	- Set Tune for CPU to ???
	- Set Use specific FPU to ???
- Toolchain Options:
	- Tuple's vendor string: whatever you want. It'll be arm-yourtuple-linux-gnu when you're finished.
- Operating System:
	- Target OS: linux
	- Linux kernel version: 2.6.33.20

## Troubleshooting ##

### /usr/local/bin/bash: line 0: [: /Volumes/Cross: binary operator expected ###

When executing

	ct-ng arm-unknown-linux-gnueabi

I got

	/usr/local/bin/bash: line 0: [: /Volumes/Cross: binary operator expected

Don't use whitespaces in your volume name!

### wget: Can't timestamp and not clobber old files at the same time ###

ct-ng tries to download a linux image

	wget --passive-ftp --tries=3 -nc --progress=dot:binary -T 10 -O /Volumes/cross/working/tarballs/linux-3.7.3.tar.xz.tmp-dl http://ftp.kernel.org/pub/linux/kernel/v3.x/linux-3.7.3.tar.xz

Turns out `-nc` isn't compatible with `timestamping` option, which was enanled in my `$HOME/.wgetrc`

### configure.in:294: error: automatic de-ANSI-fication support has been removed ##

Enable `Paths and misc options > Try features marked as EXPERIMENTAL`. This will enable an additional main menu item called `Companion tools`. Inside the `Companion tools` menu, enable `Build some companion tools > automake`.

### *.tar.gz: not in gzip format ###

The source forge mirroring or redirection was confusing wget in dowbload a HTML file

In the end I downloaded many libraries manually and put them in the tarballs directory.

It happened for `duma`, `expat` and `strace`

In order to resume work I also had to

	rm -r /Volumes/cross/working/src/duma_2_5_15/
	rm /Volumes/cross/working/src/.duma_2_5_15.extracting
	rm -r /Volumes/cross/working/src/expat-2.1.0/
	rm /Volumes/cross/working/src/.expat-2.1.0.extracting
	rm -r /Volumes/cross/working/src/strace-4.5.19
	rm /Volumes/cross/working/src/.strace-4.5.19.extracting

### Building C library, [ERROR] make[2]: *** [iconvdata/others] Error 2 ###

When building C library I got

	make[2]: *** [iconvdata/others] Error 2
	[ERROR]    make[1]: *** [all] Error 2
	[ERROR]
	[ERROR]  >>
	[ERROR]  >>  Build failed in step 'Installing C library'
	[ERROR]  >>        called in step '(top-level)'
	[ERROR]  >>
	[ERROR]  >>  Error happened in: CT_DoExecLog[scripts/functions@257]
	[ERROR]  >>        called from: do_libc_backend_once[scripts/build/libc/glibc-eglibc.sh-common@441]
	[ERROR]  >>        called from: do_libc_backend[scripts/build/libc/glibc-eglibc.sh-common@143]
	[ERROR]  >>        called from: do_libc[scripts/build/libc/glibc-eglibc.sh-common@65]
	[ERROR]  >>        called from: main[scripts/crosstool-NG.sh@632]

Someone else had [the same problem](http://sourceware.org/ml/crossgcc/2012-07/msg00028.html) but the [only answer](http://sourceware.org/ml/crossgcc/2013-01/msg00000.html) came half a year later with the hint to check the limit for open files via `ulimit`. The OP raised it from 256 to 1024 via `ulimit -n 1024`.

When I execute `ulimit` I get `unlimited` as an answer but it helped!

### Your file system in '$HOME/x-tools/alphaev4-unknown-elf' is *not* case-sensitive! ###

When changing the gcc compiler version the paths for tarballs, working directory, and prefix get reset to default.

Change them again.

### uild failed in step 'Checking that gcc can compile a trivial statically linked program (CT_WANTS_STATIC_LINK)' ###

This happens when I change the compiler to gcc 4.5.1, or 4.6.3 on OS X.

### $WORKING_DIR/src/gcc-4.2.2/gcc/regrename.c:1646: error: 'IFCVT_ALLOW_MODIFY_TEST_IN_INSN' undeclared (first use in this function) ###

When building gcc 4.2.2 on OS X

	Installing pass-1 core C compiler
	[EXTRA]    Configuring gcc
	[EXTRA]    Building gcc
	[ERROR]    /Volumes/cross/working/src/gcc-4.2.2/gcc/regrename.c:1646: error: 'IFCVT_ALLOW_MODIFY_TEST_IN_INSN' undeclared (first use in this function)
	[ERROR]    /Volumes/cross/working/src/gcc-4.2.2/gcc/regrename.c:1646: error: (Each undeclared identifier is reported only once
	[ERROR]    /Volumes/cross/working/src/gcc-4.2.2/gcc/regrename.c:1646: error: for each function it appears in.)
	[ERROR]    make[2]: *** [regrename.o] Error 1
	[ERROR]    make[1]: *** [all-gcc] Error 2
	[ERROR]
	[ERROR]  >>
	[ERROR]  >>  Build failed in step 'Installing pass-1 core C compiler'
	[ERROR]  >>        called in step '(top-level)'
	[ERROR]  >>
	[ERROR]  >>  Error happened in: CT_DoExecLog[scripts/functions@257]
	[ERROR]  >>        called from: do_cc_core_backend[scripts/build/cc/gcc.sh@448]
	[ERROR]  >>        called from: do_cc_core_pass_1[scripts/build/cc/gcc.sh@101]
	[ERROR]  >>        called from: main[scripts/crosstool-NG.sh@632]