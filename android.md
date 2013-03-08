# Android #

- Android 2.3 is based on Linux kernel 2.6.35
- Android 3.0 is based on Linux kernel 2.6.36
- Android 4.0 is based on Linux kernel 3.0.1
- Android 4.1 is based on Linux kernel 3.0.31

## SDK ##

### Linux ###

Install the NDK

	wget http://dl.google.com/android/ndk/android-ndk-r8d-linux-x86.tar.bz2
	tar xjf android-ndk-r8d-linux-x86.tar.bz2
	rm android-ndk-r8d-linux-x86.tar.bz2
	export ANDROID_NDK_ROOT=$(find $(pwd) -name "android-ndk-r8d" )

### OS X ###

Install the NDK

	wget http://dl.google.com/android/ndk/android-ndk-r8d-darwin-x86.tar.bz2
	tar xjf android-ndk-r8d-darwin-x86.tar.bz2
	rm android-ndk-r8d-darwin-x86.tar.bz2
	export ANDROID_NDK_ROOT=$(find $(pwd) -name "android-ndk-r8d" )
