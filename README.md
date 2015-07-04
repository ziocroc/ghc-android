ghc-android
===========

Build script for building ghc cross compilers targeting Android. This
script wraps up all the hacks and tweaks that are currently needed to
get a working ghc android crosscompiler. It will download the Android
NDK, the iconv library, the ncurses library and GHC itself as
required. If you already have the Android NDK you can put it in the
tarfiles directory and the build script will reuse it. If you already
have a ghc clone, you can use the mirror script to avoid doing a full
re-download during cloning.

Before starting a build you will need a working gcc, git, happy, alex
and (at least for arm builds) llvm-3.2 on the build host. You will also
need enough 32 bit support to run the Android NDK compiler. Usually
just glibc, libstdc++ and zlib should be enough.

To build a ghc-android compiler execute:

    ./build

By default this should give you a android-21 arm-linux-androideabi-4.9
compiler at:

    ~/.ghc/android-21/arm-linux-androideabi-4.9/bin

Add this directory to your PATH to use the cross compiler with the
full target prefix.

Or add:

    ~/.ghc/android-21/arm-linux-androideabi-4.9/arm-linux-androideabi/bin

to your PATH to use it without prefixes. There is also a cabal wrapper
script in this directory that will cross compile packages with cable
and add them to the ghc package-db.

You can change various variables via the environment to give you other
configurations.

There is also a pre-configured x86 variation you can build like this:

    ./build --x86

If you want to build more than one configuration it is worth building
a local mirror repo to speed up subsequent ghc clones. To build a
mirror run:

    ./mirror {path to existing ghc clone}
    
During ./build the GHC repo is cloned into appropriate path, e.g.:

    ./build-android-21-arm-linux-androideabi-4.9/ghc
    
So the mirror path can become this:

    ./mirror build-android-21-arm-linux-androideabi-4.9/ghc

This will build a mirror that will automatically be used by the build
script to speed up cloning.

The following configurations have been tested on an up-to-date Arch
Linux install and are known to work:

    * ghc-android-21-arm-linux-androideabi-4.9
    * ghc-android-21-x86-4.9

Comments, patches and success reports are welcome.

## Tested working build with Ubuntu 15.04

```sh
apt-get update
apt-get -y install build-essential git libncurses5-dev
apt-get -y install ca-certificates file m4 autoconf zlib1g-dev
apt-get -y install libgnutls-dev libxml2-dev libgsasl7-dev pkg-config

wget http://llvm.org/releases/3.2/llvm-3.2.src.tar.gz
tar xf llvm-3.2.src.tar.gz
cd llvm-3.2.src
./configure
make -j6
sudo make install

git clone https://github.com/rhaps0dy/ghc-android
cd ghc-android
./build
```
