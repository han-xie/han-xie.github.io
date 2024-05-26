---
layout: post
title: Compile kaldi for Android
tags: [tech]
---
We NEED to cross compile kaldi for arm64-v8a first in order to use this project.
Following are the instructions which have been tested in Ubuntu 18.04, 64 bits.

## Download Android NDK

To compile native code you will need to download the Android NDK.
These instructions were tested with release 16b, but should work with
other versions.

```
wget -q --output-document=android-ndk-r16b.zip https://dl.google.com/android/repository/android-ndk-r16b-linux-x86_64.zip
```
Tips: you may need VPN to download it.

After that, please, unzip `android-ndk-r16b.zip`. In this example we will call
`<NDK root dir>` the absolute path to the unzipped directory.

Finally, install the toolchain. You **must** explicitly specify `--stl=libc++`,
to copy LLVM libc++ headers and libraries:

```
<NDK root dir>/build/tools/make_standalone_toolchain.py --arch arm64 --api 21 --stl=libc++ --install-dir <NDK root dir>/../my-android-toolchain8
```

This command creates a directory named <NDK root dir>/../my-android-toolchain8/, containing a
copy of the android-21/arch-arm64 sysroot, and of the toolchain binaries for a
64-bit ARM architecture.

For more details about it, please read <https://developer.android.com/ndk/guides/standalone_toolchain.html>.


## Compile OpenBLAS for Android

You **must** use OpenBlas version 0.2.20 or newer. When I wrote these
instructions, I used commit SHA 09e397e4f.

#### Download source

```
git clone https://github.com/xianyi/OpenBLAS
git checkout 09e397e4f
```

#### Add the Android toolchain to your path

```
export ANDROID_TOOLCHAIN_PATH=<NDK root dir>/../my-android-toolchain
export PATH=${ANDROID_TOOLCHAIN_PATH}/bin:$PATH
```

#### Build for ARMV8

```
export CLANG_FLAGS="-target aarch64-linux-android --sysroot ${ANDROID_TOOLCHAIN_PATH}/sysroot -gcc-toolchain ${ANDROID_TOOLCHAIN_PATH}"

make TARGET=ARMV8 ONLY_CBLAS=1 AR=ar CC="clang ${CLANG_FLAGS}" HOSTCC=gcc -j4

```

#### Install library

```
make install NO_SHARED=1 PREFIX=`pwd`/install
```


## Compile CLAPACK for Android

I tested it in commit SHA a71cd07d418a.
But it should work with more recent versions of this code.

```
git clone https://github.com/simonlynen/android_libs.git
git checkout a71cd07d418a
cd android_libs/lapack

# remove some compile instructions related to tests
sed -i 's/LOCAL_MODULE:= testlapack/#LOCAL_MODULE:= testlapack/g' jni/Android.mk
sed -i 's/LOCAL_SRC_FILES:= testclapack.cpp/#LOCAL_SRC_FILES:= testclapack.cpp/g' jni/Android.mk
sed -i 's/LOCAL_STATIC_LIBRARIES := lapack/#LOCAL_STATIC_LIBRARIES := lapack/g' jni/Android.mk
sed -i 's/include $(BUILD_SHARED_LIBRARY)/#include $(BUILD_SHARED_LIBRARY)/g' jni/Android.mk

sed -i 's/10/21/g' AndroidManifest.xml
sed -i 's/10/21/g' project.properties
sed -i 's/armeabi armeabi-v7a /arm64-v8a/g' jni/Application.mk

# build for android
<NDK root dir>/ndk-build
```

Libs will be created in `obj/local/arm64-v8a/`.

**Copy libs from `obj/local/arm64-v8a/` to the same place you installed OpenBLAS libraries
(e.g: OpenBlas/install/lib)**.
Kaldi will look at this  directory for libf2c.a, liblapack.a, libclapack.a and libblas.a.


## Compile kaldi for Android

The following instructions were tested with commit SHA 63110571d of Kaldi. But it should
work with the most recent version of Kaldi and you should first try the most recent Kaldi commit.

#### Download kaldi source code

```
git clone https://github.com/kaldi-asr/kaldi.git kaldi-armv8
git checkout 63110571d
```

#### Compile OpenFST

In the instructions, we are using OpenFST-1.6.7. But you should use the current
version used in your Kaldi repository. You may discover it looking at tools/Makefile.

```
#Be sure $ANDROID_TOOLCHAIN_PATH/bin is in your $PATH before the next step

cd kaldi-armv8/tools

wget -T 10 -t 1 http://openfst.cs.nyu.edu/twiki/pub/FST/FstDownload/openfst-1.6.7.tar.gz

tar -zxvf openfst-1.6.7.tar.gz

cd openfst-1.6.7/

CXX=clang++ CXXFLAGS="-O3 -ftree-vectorize -DFST_NO_DYNAMIC_LINKING" ./configure --prefix=`pwd` --enable-static --enable-shared --with-pic --disable-bin --enable-ngram-fsts --host=aarch64-linux-android LIBS="-ldl"

make -j 4

make install

cd ..

ln -s openfst-1.6.7 openfst
```

#### Compile src

```
cd ../src

#Be sure $ANDROID_TOOLCHAIN_PATH/bin is in your $PATH before the next step

CXX=clang++ CXXFLAGS="-O3 -ftree-vectorize -DFST_NO_DYNAMIC_LINKING -DNDEBUG" ./configure --static --android-incdir=$ANDROID_TOOLCHAIN_PATH/sysroot/usr/include/ --host=aarch64-linux-android --openblas-root=$OPENBLAS/install --use-cuda=no --shared

make clean -j

make depend -j

make -j 4
```

#### Compile kaldi_jni

Download this kaldi_jni project and run `make project` in Android Studio.

Please NOTE that you should set the path in CMakeLists.txt properly in your own environment!!!

This project is based on commit SHA bb8188afe of <https://github.com/alphacep/kaldi-android>.

#### Compile Android apk

Compile commit SHA e4053d67f of <https://github.com/alphacep/kaldi-android-demo> in Android Studio and you can run the app.

Put final.mdl, HCLG.fst, mfcc.conf, online_pitch.conf, word_boundary.int (generated by 
`utils/prepare_lang.sh --position-dependent-phones true data/local/dict "<SPOKEN_NOISE>" data/local/lang data/lang_tmp`),
words.txt and ivector folder in model-aishell folder.

Don't forget to rename the model path in KaldiActivity.java.


## References

<https://developer.android.com/ndk/guides/index.html>

<https://developer.android.com/ndk/guides/standalone_toolchain.html>

<https://github.com/xianyi/OpenBLAS/wiki/How-to-build-OpenBLAS-for-Android>

<http://stackoverflow.com/questions/22774009/android-ndk-stdto-string-support>

<https://github.com/xianyi/OpenBLAS/issues/539>

[compile kaldi for arm64-v8a]<https://medium.com/swlh/compile-kaldi-for-64-bit-android-on-ubuntu-18-70967eb3a308>
