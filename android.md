# Android #

- Android 2.3 is based on Linux kernel 2.6.35
- Android 3.0 is based on Linux kernel 2.6.36
- Android 4.0 is based on Linux kernel 3.0.1
- Android 4.1 is based on Linux kernel 3.0.31

## Development ##

### SDK ###

For OS X

	brew install android-sdk

### Native Development Kit (NDK) ###

For Linux

	wget http://dl.google.com/android/ndk/android-ndk-r8d-linux-x86.tar.bz2
	tar xjf android-ndk-r8d-linux-x86.tar.bz2
	rm android-ndk-r8d-linux-x86.tar.bz2
	export ANDROID_NDK_ROOT=$(find $(pwd) -name "android-ndk-r8d" )

For OS X

	brew install android-ndk

### Eclipse Plugin ###

Start Eclipse, then select `Help > Install New Software` and add the following URL

	https://dl-ssl.google.com/android/eclipse/

In the Available Software dialog, select the checkbox next to Developer Tools and click `Next` and follow the rest of the instructions.

### Code Samples ###

To help you understand some fundamental Android APIs and coding practices, a variety of sample code is available from the Android SDK Manager. Each version of the Android platform available from the SDK Manager offers its own set of sample apps.

To download the samples:

- Launch the Android SDK Manager.
- On Windows, double-click the SDK Manager.exe file at the root of the Android SDK directory.
- On Mac or Linux, open a terminal to the tools/ directory in the Android SDK, then execute android sdk.
- Expand the list of packages for the latest Android platform.
- Select and download Samples for SDK.
- When the download is complete, you can find the source code for all samples at `<sdk>/samples/android-<version>/` where the `<version>` number corresponds to the platform's API level.

You can easily create new Android projects with the downloaded samples, modify them if you'd like, and then run them on an emulator or device.

## Devices ##

### Sony Xperia S ###

[Sony Xperia S](http://www.sonymobile.com/global-en/products/phones/xperia-s/)

- CPU: 1.5 GHz dual-core Scorpion
- GPU: Qualcomm Adreno 220

CPU Architecture

	# cat /proc/cpuinfo
	Processor	: ARMv7 Processor rev 1 (v7l)
	processor	: 0
	BogoMIPS	: 4.80

	processor	: 1
	BogoMIPS	: 4.80

	Features	: swp half thumb fastmult vfp edsp neon vfpv3
	CPU implementer	: 0x41
	CPU architecture: 7
	CPU variant	: 0x2
	CPU part	: 0xc09
	CPU revision	: 1

	Hardware	: riogrande
	Revision	: 0000
	Serial		: 0000000000000000
