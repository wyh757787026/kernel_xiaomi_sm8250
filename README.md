# About this repo
This repo is forked from https://github.com/UtsavBalar1231/kernel_xiaomi_sm8250. Thanks to [@UtsavBalar1231](https://github.com/UtsavBalar1231/)!

The main purpose is fix [this battery stuck at 1% problem](https://github.com/liyafe1997/Xiaomi-fix-battery-one-percent), and some small changes and optimizations of defconfig. Also provide KernelSU integrated pre-built image(flashable anykernel3 zip), and a more intuitive and easy-to-use build script and build guide that allow you to try to build by yourself. 

This focked branch and the pre-built kernel image/zip is based on the `android12-stable` branch of the original repo. It should works on both stock MIUI and third-party AOSP based Android 12 ROMs. Android 13 also should be works, but I don't have device to test it. As I only have a `lmi` with stock latest MIUI 14.0.4 with Android 12, I only tested on that. If you have a Android 13 based system, or other models you are welcomed to test it and feedback :) 

Note: The zip does not include the `dtbo.img` and it will not replace your `dtbo` partition. It is recommanded to keep the stock `dtbo` or the `dtbo` from your third-party rom (If the builder comfirmed it works well). Since there are some problems with the `dtbo.img` which built from this source, one of them is the screen will suddently goes to the highest brightness when shut try to shut off the screen in the lock screen. If you had flashed any other third-party kernels, and you got some weird problem, you should keep an eye to check your `dtbo` has been replaced or not. 

该repo fork自 https://github.com/UtsavBalar1231/kernel_xiaomi_sm8250 ，感谢 [@UtsavBalar1231](https://github.com/UtsavBalar1231/)！


主要目的是想修复[电量卡在1%的问题](https://github.com/liyafe1997/Xiaomi-fix-battery-one-percent)，以及defconfig有一些小的改动和优化，以及提供带KernelSU的预编译好的内核（原作的没提供有KernelSU，而且原作release里的lmi内核在我的K30Pro上无法启动，所以就想着自己编译了）。以及再提供一个更直观和易用的编译脚本和README，方便大家自己折腾和修改，编译自己的内核。

该focked分支以及release里的编译好的内核成品基于原repo的`android12-stable`分支，应当能在原版MIUI和第三方的基于AOSP的各种Android12的ROM上使用。Android13理论上也许应该也行，但我没有测试。同样我手上只有lmi（K30Pro），所以我只在lmi上的最新版官方MIUI 14.0.4(Android12)上测试了。如果你有Android13，或者其它型号的机器，欢迎测试并反馈：）

注意：该内核的zip包不包含`dtbo.img`，并且不会刷你的dtbo分区。推荐使用原厂的`dtbo`，或者来自第三方系统包自带的dtbo（如果原作者确认那好用的话）。因为该源码build出来的`dtbo.img`有些小问题，比如在锁屏界面上尝试熄屏时，屏幕会突然闪一下到最高亮度。如果你刷过其它第三方内核，或者遇到一些奇怪的问题，建议检查一下你的`dtbo`是否被替换过。

# How to build
1. Prepair the basic build environment. 
    You have to have the basic common toolchains, such as `git`, `make`, `curl`, `bison`, `flex`, `zip`, etc, and some other packages.
    In Debian/Ubuntu, you can
    ```
    sudo apt install build-essential git curl wget bison flex zip bc cpio libssl-dev
    ```
    And also, you have to have `python` (only `python3` is not enough). you can install the apt package `python-is-python3`.

    In RHEL/RPM based OS, you can
    ```
    sudo yum groupinstall 'Development Tools'
    sudo yum install wget bc openssl-devel
    ```

2. Download compiler from Google
    You have to have `aarch64-linux-android`, `arm-linux-androideabi`, `clang`. They are avaliable in these repository:
    https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9
    https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9
    https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86

    The branch `android12-gsi` is recommanded. But it just a compiler, it doesn't really tied to any specific Android/Kernel version, so use similar versions should be OK. I don't know the real meaning of "android12-gsi" branch in these compiler's repo is, I guess Google uses that or recommand that for compiling the Android 12 GSI's kernel :) So we can just use that, and I tested that it has no problem. 

    It is recommand to direcly download the tar ball instead of `git clone` these repository, because it is super large and takes a long time. So you can just follow these commands to get the compilers binary.

    ```
    mkdir aarch64-linux-android-4.9
    cd aarch64-linux-android-4.9
    wget https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9/+archive/refs/heads/android12-gsi.tar.gz
    tar xvf android12-gsi.tar.gz
    cd ..

    mkdir arm-linux-androideabi-4.9
    cd arm-linux-androideabi-4.9
    wget https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/+archive/refs/heads/android12-gsi.tar.gz
    tar xvf android12-gsi.tar.gz
    cd ..

    mkdir clang-android
    cd clang-android
    wget https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/+archive/refs/heads/android12-gsi.tar.gz
    tar xvf android12-gsi.tar.gz
    cd ..
    ```

3. Set the environment.
    If you followed the above command and downloaded to your home directory, you can directly use this command to put them to the `PATH`, otherwise change to the correct location.
    ```
    export PATH="$HOME/arm-linux-androideabi-4.9/bin:$HOME/aarch64-linux-android-4.9/bin:$HOME/clang-android/clang-r416183b1/bin:$PATH"
    ```

4. Build

    Build without KernelSU: 
    ```
    bash build.sh TARGET_DEVICE
    ```
    
    Build with KernelSU:
    ```
    bash build.sh TARGET_DEVICE ksu
    ```

    For example, build for lmi (Redmi K30 Pro/POCO F2 Pro) without KernelSU:
    ```
    bash build.sh lmi
    ````

    For example, build for umi (Mi 10) with KernelSU:
    ```
    bash build.sh umi ksu
    ```



