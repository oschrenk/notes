# Build Open JDK 7 #

    // change symlinks to gcc 4.0
    // not sure if needed, if using CC, CXX make options, use with care
    $ sudo rm /usr/bin/gcc
    $ sudo ln -s /usr/bin/gcc-4.2 /usr/bin/gcc

    // change to path where you want to put all the stuff
    cd /path/to/downloads

    // get sources an apply patch
    $ hg fclone http://hg.openjdk.java.net/bsd-port/bsd-port
    $ cd bsd-port/hotspot

    // get John's patch, not hosted anywherem get it from attachment and unzip it
    $ patch -p1 /path/to/downlods>/snowleopard.patch
    $ cd ..

    // set environment
    $ export LC_ALL=C
    $ export LANG=C
    $ unset CLASSPATH
    $ unset JAVA_HOME

    // get soylatte 
    // get password from: http://landonf.bikemonkey.org/static/soylatte/
    // remember to precede a whitespace in the password with a backslash "\"
    $ wget http://jrl:<password>@hg.bikemonkey.org/archive/javasrc_1_6_jrl_darwin/soylatte16-i386-1.0.3.tar.bz2
    tar -xvjf soylatte16-i386-1.0.3.tar.bz2 

    // get jibx, info: http://jibx.sourceforge.net/
    // (1.2.1 didj't work for me), 
    wget http://downloads.sourceforge.net/project/jibx/jibx/jibx-1.1.6a/jibx_1_1_6a.zip
    unzip jibx_1_1_6a.zip

    // replace "/path/to/downloads" with appropiate folder
    $ make \
    	ALT_BOOTDIR=/path/to/downloads/soylatte16-i386-1.0.3/ \
    	ALT_FREETYPE_HEADERS_PATH=/usr/X11R6/include \
    	ALT_FREETYPE_LIB_PATH=/usr/X11R6/lib \
    	ALT_JIBX_LIBS_PATH=/path/to/downloads/jibx \
    	ALT_CUPS_HEADERS_PATH=/usr/include \
    	ANT_HOME=/usr/share/ant \
    	ARCH_DATA_MODEL=32 \
    	JAVA_TOOLS_DIR=/Users/Oliver/Downloads/soylatte16-i386-1.0.3/bin
    	NO_DOCS=true \
    	CC="gcc-4.0" \
    	CXX="g++-4.0" \
    	HOTSPOT_BUILD_JOBS=2

    // get a real coffee or a tea and wait for Java7 to be built


    ////////////////// snowleopard.patch

    diff --git a/src/cpu/x86/vm/methodHandles_x86.cpp b/src/cpu/x86/vm/methodHandles_x86.cpp
    --- a/src/cpu/x86/vm/methodHandles_x86.cpp
    +++ b/src/cpu/x86/vm/methodHandles_x86.cpp
    @@ -273,7 +273,7 @@
                                   intptr_t* entry_sp,
                                   intptr_t* saved_sp) {
       // called as a leaf from native code: do not block the JVM!
    -  printf("MH %s "PTR_FORMAT" "PTR_FORMAT" "INTX_FORMAT"\n", adaptername, (void*)mh, entry_sp, entry_sp - saved_sp);
    +  printf("MH %s "INTPTR_FORMAT" "INTPTR_FORMAT" "INTX_FORMAT"\n", adaptername, (intptr_t)mh, (intptr_t)entry_sp, (intptr_t)(entry_sp - saved_sp));
     }
     #endif //PRODUCT
     
    diff --git a/src/share/vm/classfile/javaClasses.cpp b/src/share/vm/classfile/javaClasses.cpp
    --- a/src/share/vm/classfile/javaClasses.cpp
    +++ b/src/share/vm/classfile/javaClasses.cpp
    @@ -952,7 +952,7 @@
         }
         nmethod* nm = method->code();
         if (WizardMode && nm != NULL) {
    -      sprintf(buf + (int)strlen(buf), "(nmethod " PTR_FORMAT ")", (intptr_t)nm);
    +      sprintf(buf + (int)strlen(buf), "(nmethod " INTPTR_FORMAT ")", (intptr_t)nm);
         }
       }
     
    diff --git a/src/share/vm/utilities/globalDefinitions.hpp b/src/share/vm/utilities/globalDefinitions.hpp
    --- a/src/share/vm/utilities/globalDefinitions.hpp
    +++ b/src/share/vm/utilities/globalDefinitions.hpp
    @@ -1125,9 +1125,18 @@
     #define SSIZE_FORMAT_W(width)   INT64_FORMAT_W(width)
     #define SIZE_FORMAT_W(width)    UINT64_FORMAT_W(width)
     #else
    +#ifndef FORMATL32_MODIFIER
     #define UINTX_FORMAT_W(width)   UINT32_FORMAT_W(width)
     #define SSIZE_FORMAT_W(width)   INT32_FORMAT_W(width)
     #define SIZE_FORMAT_W(width)    UINT32_FORMAT_W(width)
    +#else   //FORMATL32_MODIFIER
    +// A few ILP32 platforms also need to define FORMATL32_MODIFIER as "l".
    +#define INTL32_FORMAT_W(width)   "%" #width FORMATL32_MODIFIER "d"
    +#define UINTL32_FORMAT_W(width)  "%" #width FORMATL32_MODIFIER "u"
    +#define UINTX_FORMAT_W(width)   UINTL32_FORMAT_W(width)
    +#define SSIZE_FORMAT_W(width)   INTL32_FORMAT_W(width)
    +#define SIZE_FORMAT_W(width)    UINTL32_FORMAT_W(width)
    +#endif  //FORMATL32_MODIFIER
     #endif // _LP64
     
     // Format pointers and size_t (or size_t-like integer types) which change size
    @@ -1146,11 +1155,23 @@
     #define SIZE_FORMAT   UINT64_FORMAT
     #define SSIZE_FORMAT  INT64_FORMAT
     #else   // !_LP64
    +#ifndef FORMATL32_MODIFIER
     #define PTR_FORMAT    PTR32_FORMAT
     #define UINTX_FORMAT  UINT32_FORMAT
     #define INTX_FORMAT   INT32_FORMAT
     #define SIZE_FORMAT   UINT32_FORMAT
     #define SSIZE_FORMAT  INT32_FORMAT
    +#else   //FORMATL32_MODIFIER
    +// A few ILP32 platforms also need to define FORMATL32_MODIFIER as "l".
    +#define INTL32_FORMAT  "%" FORMATL32_MODIFIER "d"
    +#define UINTL32_FORMAT "%" FORMATL32_MODIFIER "u"
    +#define PTRL32_FORMAT  "0x%08" FORMATL32_MODIFIER "x"
    +#define PTR_FORMAT    PTRL32_FORMAT
    +#define UINTX_FORMAT  UINTL32_FORMAT
    +#define INTX_FORMAT   INTL32_FORMAT
    +#define SIZE_FORMAT   UINTL32_FORMAT
    +#define SSIZE_FORMAT  INTL32_FORMAT
    +#endif  //FORMATL32_MODIFIER
     #endif  // _LP64
     
     #define INTPTR_FORMAT PTR_FORMAT
    diff --git a/src/share/vm/utilities/globalDefinitions_gcc.hpp b/src/share/vm/utilities/globalDefinitions_gcc.hpp
    --- a/src/share/vm/utilities/globalDefinitions_gcc.hpp
    +++ b/src/share/vm/utilities/globalDefinitions_gcc.hpp
    @@ -280,6 +280,12 @@
     #define FORMAT64_MODIFIER "ll"
     #endif // _LP64
     
    +#if (__GNUC__ == 4) && (__GNUC_MINOR__ >= 2)
    +// GCC 4.2 complains about printf("%d", (intptr_t)10).
    +// This occurs with intptr_t and intx types, which are typedefs of "long int".
    +#define FORMATL32_MODIFIER "l"
    +#endif //__GNUC__ >= 4.2
    +
     // HACK: gcc warns about applying offsetof() to non-POD object or calculating
     //       offset directly when base address is NULL. Use 16 to get around the
     //       warning. gcc-3.4 has an option -Wno-invalid-offsetof to suppress
    ////////////////////
