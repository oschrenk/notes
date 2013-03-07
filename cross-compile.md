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

Disable `Debug Facilities > dmalloc` and set the various versions

- binutils: 2.22
- gcc: 4.6.3
- glibc: 2.10.1
- GMP 5.0.2
- MPFR 3.0.1
- PPL 0.11.2
- CLooG/ppl 0.15.11
- MPC 0.9
- libelf: 0.8.13

- Operating System:
	- Target OS: linux
	- Linux kernel version: 2.6.33.20

- Target options
	- Target Architecture: ARM
	- Floating point: Hardware (FPU)

- Disable `Debug Facilities > dmalloc`

Build it.

	 ct-ng build

## Configuring the target platform ##

Know your target platform!

We're interested to build v8 for [Sony Xperia S](http://www.sonymobile.com/global-en/products/phones/xperia-s/)

- CPU: 1.5 GHz dual-core Scorpion
- GPU: Qualcomm Adreno 220

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

It has something to do with OS X.

> This problem has to do with functionality differences between OS X ld and GNU ld. Building GNU ld for OS X isn't possible AFAIK.

	ct-ng menuconfig

Make sure that `Toolchain options > Build Static Toolchain` is disabled (it is disabled by default).

For some reasons the according flag `CT_WANTS_STATIC_LINK` is still set to `y`.

Another user had the [same problem](http://sourceware.org/ml/crossgcc/2012-09/msg00021.html) and suggested to

	sed -i "" 's/^CT_WANTS_STATIC_LINK=y/# CT_WANTS_STATIC_LINK is not set/g' .config

which was [declared a bad idea](http://sourceware.org/ml/crossgcc/2012-09/msg00025.html) by Yann E. MORIN as you shouldn't manually edit your `.config`. Some other options seem to blindly set `CT_WANTS_STATIC_LINK`, such as when you try to statically link libstdc++.

For now I will try it out. Hopefully it won't bite me.

### Build failed in step 'Checking that gcc can statically link libstdc++ (CT_CC_STATIC_LIBSTDCXX)' ###

Again the problem with linking on OS X.

> This problem has to do with functionality differences between OS X ld and GNU ld. Building GNU ld for OS X isn't possible AFAIK.

Disable it via in `C Compiler > Link libstdc++ statically into the gcc binary`. If it still persists fix it with

	sed -i "" 's/^CT_CC_STATIC_LIBSTDCXX=y/# CT_CC_STATIC_LIBSTDCXX is not set/g' .config

Keep in mind that if you go back into menuconfig and save new changes, that will overwrite your change to `.config` and you'll have to make your edits again.

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

I switched to a newer gcc version.

### configure.in:90: error: possibly undefined macro: AC_PROG_LIBTOOL ###

After patching toolchain components I get

	[EXTRA]    Patching 'cloog-ppl-0.15.11'
	[ERROR]    configure.in:90: error: possibly undefined macro: AC_PROG_LIBTOOL
	[ERROR]
	[ERROR]  >>
	[ERROR]  >>  Build failed in step 'Extracting and patching toolchain components'
	[ERROR]  >>        called in step '(top-level)'
	[ERROR]  >>
	[ERROR]  >>  Error happened in: CT_DoExecLog[scripts/functions@257]
	[ERROR]  >>        called from: do_cloog_extract[scripts/build/companion_libs/130-cloog.sh@35]
	[ERROR]  >>        called from: do_companion_libs_extract[scripts/build/companion_libs.sh@22]
	[ERROR]  >>        called from: main[scripts/crosstool-NG.sh@600]

I enabled `Companion Tools > libtool`

### write error: No space left on device ###

After many, many tries I ended up filling the disk, so I had to resize my disk image

	hdiutil resize -size 8g cross-compiling.dmg

### std::domain_error::~domain_error() ###

When building gcc I get

	Installing pass-1 core C compiler
	[EXTRA]    Configuring gcc
	[EXTRA]    Building gcc
	[ERROR]      "std::domain_error::~domain_error()", referenced from:
	[ERROR]      "std::length_error::~length_error()", referenced from:
	[ERROR]    make[2]: *** [lto1] Error 1
	[ERROR]    make[1]: *** [all-gcc] Error 2

with the `build.log` showing

	[ALL  ]          std::list<Parma_Polyhedra_Library::Determinate<Parma_Polyhedra_Library::C_Polyhedron>, std::allocator<Parma_Polyhedra_Library::Determinate<Parma_Polyhedra_Library::C_Polyhedron> > >::operator=(std::list<Parma_Polyhedra_Library::Determinate<Parma_Polyhedra_Library::C_Polyhedron>, std::allocator<Parma_Polyhedra_Library::Determinate<Parma_Polyhedra_Library::C_Polyhedron> > > const&)in libppl_c.a(ppl_c_Pointset_Powerset_C_Polyhedron.o)
	[ALL  ]          ...
	[ALL  ]    ld: symbol(s) not found for architecture x86_64
	[ALL  ]    collect2: ld returned 1 exit status
	[ERROR]    make[2]: *** [lto1] Error 1
	[ALL  ]    make[2]: *** Waiting for unfinished jobs....
	[ALL  ]    rm gcc.pod
	[ERROR]    make[1]: *** [all-gcc] Error 2


I disabled `Install a cross ldd-like helper` but It didn't help. I switched to gcc 4.7.2 (was 4.6.3 before)

### configure: error: forced unwind support is required ###

When I switched to gcc 4.7.2 I got

	Installing C library headers & start files
	[EXTRA]    Configuring C library
	[ERROR]    configure: error: forced unwind support is required
	[ERROR]
	[ERROR]  >>
	[ERROR]  >>  Build failed in step 'Installing C library headers & start files'
	[ERROR]  >>        called in step '(top-level)'
	[ERROR]  >>
	[ERROR]  >>  Error happened in: CT_DoExecLog[scripts/functions@257]
	[ERROR]  >>        called from: do_libc_backend_once[scripts/build/libc/glibc-eglibc.sh-common@347]
	[ERROR]  >>        called from: do_libc_backend[scripts/build/libc/glibc-eglibc.sh-common@143]
	[ERROR]  >>        called from: do_libc_start_files[scripts/build/libc/glibc-eglibc.sh-common@60]
	[ERROR]  >>        called from: main[scripts/crosstool-NG.sh@632]

I enabled `C-library > Force unwind support`. I don't believe this helped though in the long run.

### rpc/types.h:73: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'u_char' ###

After switching to gcc 4.7.2, and forcing unwind support I got

	[EXTRA]    Configuring C library
	[EXTRA]    Installing C library headers
	[ERROR]    rpc/types.h:73: error: expected '=', ',', ';', 'asm' or '__attribute__' before 'u_char'

I never solved this one. At this point I deleted all the sources and started fresh.
