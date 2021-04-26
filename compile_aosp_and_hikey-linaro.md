# Compile aosp and hikey-linaro

## 文档

首先最有用的就是官方文档了，附上Google的官网地址。https://source.android.com/source/devices

官网上的说明是简明概要的，但是在我们的编译过程中其实可能会遇到各种奇奇怪怪的问题。下面就一些 我们踩过的雷给排掉。

* Hikey960官方网站：

 [https://www.96boards.org/product/hikey960/](https://www.96boards.org/product/hikey960/)

* Hikey960的aosp编译安装文档：

[https://source.android.com/setup/build/devices#hikey960](https://source.android.com/setup/build/devices#hikey960)

* Hikey960官方预先编译好的代码：

[https://www.96boards.org/documentation/consumer/hikey/hikey960/downloads/aosp.md.html](https://www.96boards.org/documentation/consumer/hikey/hikey960/downloads/aosp.md.html)


## 准备工作

* 交叉编译工具Android NDK [https://developer.android.com/ndk/downloads/index.html](https://developer.android.com/ndk/downloads/index.html) （注意从r18版本开始就不支持使用gcc编译了，而内核编译还是用gcc的，所以我安装的是r16b）
* AOSP相关 [https://source.android.com/setup/build/initializing](https://source.android.com/setup/build/initializing)
* Python3

## （一）首先就是下载aosp和hikey-linaro的源码。

aosp比较大，压缩后都有60G左右，解压后有115G左右。hikey-linaro压缩不超过2G。这两个源码官网上 可以下载，但是要翻墙！！！

好在校园网ipv6可以直连Google，但是我在实验室下载很慢，据说寝室下载 要快一些。hikey-linaro我们下载的master分支，前期编译4.4版本，
后期编译4.9版本。以下描述二个版本可以通用。

**下载清华的hikey-linaro镜像**

aosp配套的内核代码
git clone https://aosp.tuna.tsinghua.edu.cn/kernel.common.git

hikey专用内核镜像
git clone heeps://aosp.tuna.tsinghua.edu.cn/kernel/hikey-linaro.git

**补充一下从清华的镜像下载aosp和相关工具的方法（该方法非常好用）**

详细资料参考他们的官网：
https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/

* 正常方法只是因为无法翻墙才无法下载，但是可以将 https://android.googlesource.com/ 全部使用 https://aosp.tuna.tsinghua.edu.cn/ 代替即可。
* 有一个步骤是repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest 该步骤无法完成的原因是repo这个脚本里面默认使用了google的REPO_URL。解法：vim repo，按照下面内容进行修改：

```
if not REPO_URL:
  REPO_URL = 'https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/'
```

可能的error:
```AttributeError: 'str' object has no attribute 'decode'```
解决方法：直接去掉代码 ```decode('utf8')```


scripts:
```
#aosp
mkdir WORKING_DIRECTORY
cd WORKING_DIRECTORY
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest

#This step may take a very long time
repo sync -j24

#hikey-linaro
git clone https://aosp.tuna.tsinghua.edu.cn/kernel/hikey-linaro

```

**其他资料：**

**可能需要下载HDMI二进制文件**

```
Download and extract HDMI binaries:
$ wget https://dl.google.com/dl/android/aosp/arm-hikey960-OPR-cf4e0c80.tgz
$ tar xzf arm-hikey960-OPR-cf4e0c80.tgz
$ ./extract-arm-hikey960.sh
```

我的做法是在能翻墙的电脑上把.tgz文件下载下来然后ftp传到linux服务器上

aosp在github上的镜像：
https://github.com/aosp-mirror

96系列板子的linaro-hikey在github上的镜像（补：这个好像是教程）
https://github.com/96boards-hikey/aosp-device-linaro-hikey

## （二）编译aosp源码。（括号内内容对应de21上的目录）

（cd /mnt/aosp）

. ./build/envsetup.sh //注意2个点之间有空格

lunch hikey960-userdebug//选择的是userdebug模式，可惜我们现在都没有编译成功engineer模式。也可以是lunch hikey960-eng

make -j32 //编译时间2h-4h不等，编译一遍后再编译会快一些

**其他需要注意的细节**

1. 不建议使用root用户进行操作，因为会产生warning。
2. 如果之前使用的是root用户，可以通过 chown -R @username @FILE_DIRECTORY 来修改目录的所有者
3. 之前下载的HDMI二进制文件解压后，需要放在和aosp同一个目录下（也就是vendor文件夹需要和aosp的其他文件夹在同一个目录下）
4. 有可能之前下载的HDMI是老版本，此时会弹出warning，可以根据warning的提示，下载新版本的HDMI。
5. 执行了lunch hikey960-userdebug之后，应该是没有 warning 的只有两排等于号夹起来的系统信息和配置信息。
6. 编译出错有可能是没有安装编译需要的环境，比如python3，可以根据错误提示进一步谷歌。编译环境的需求可看[https://source.android.com/setup/build/initializing](https://source.android.com/setup/build/initializing)
7. `lunch hikey960-userdebug`之后其实hikey960会帮你选一个内核版本，所以在make的时候，其实内核的版本已经选好了。这个选择内核版本的代码非常隐蔽，但是[https://discuss.96boards.org/t/hikey960-aosp-kernel-build/4496/9](https://discuss.96boards.org/t/hikey960-aosp-kernel-build/4496/9)给出了答案，其实在`device/linaro/hikey/hikey960.mk`中有设定默认值。如果需要自己手动指定，可以在后面make的时候设置`export TARGET_KERNEL_USE=4.9`来手动指定内核版本（注意，如果这样设置了，那后面在make bootimage之类的也需要手动指定）。  
或者使用 `make TARGET_KERNEL_USE=4.9 -j32`。 **这里设置的内核版本一定要跟下面的hikey-linaro的内核版本一致。**  
8. （2019-12-18）之前在服务器上run python程序，因为调用大量库，升级到python3.6版本，再编译aosp，报一大堆error，缺少依赖，如：you can set allow_missing_dependencies=true in your environment if this is intentional, but that may defer real problems until later in the build
.cts/hostsidetests/devicepolicy/app/assistant/android.mk: error: ctsdevicepolicyassistapp (apps android-arm64) missing android-support-test (java_libraries android-arm64)；修改python软连接，修改python3.6到python2.7版本就可。

## （三）编译Hikey-linaro源码

（cd /mnt/hikey-linaro-google/hikey-linaro/）

1.git checkout -b android-hikey-linaro-4.9 origin/android-hikey-linaro-4.9 //我的理解是在 master下切到4.9分支，
若编译的是4.4分支，将4.9改为4.4就好。踩雷1：出现4.9分支已存在<具体的 error statement我不记得了，这是一篇总结性文章>。
排雷：rm .git/refs/heads/your_branch_name， 进入到heads目录删掉Android-hikey-linaro-4.9分支<大概叫这个名字>

error: Your local changes to the following files would be overwritten by checkout
git commit -m "a"
git add .


2.make ARCH=arm64 hikey960_defconfig //设置配置，在第一次编译时做过后.config生成，之后再做时 可以不用做这句话。
踩雷2：这句话是生成.config文件，之后在adb shell里mount f2fs时报错无法挂载 ，因为transport endpoint等等的坑要在这里填。
是因为没有配置F2FS的编译configuration。vim  arch/arm64/configs/hikey960_defconfig,添加如下内容：

CONFIG_F2FS_FS=y 
CONFIG_F2FS_STAT_FS=y
CONFIG_F2FS_FS_ATTR=y
CONFIG_F2FS_POSIX_ACL=y
CONFIG_F2FS_FS_SECURITY=y
CONFIG_F2FS_CHECK_FS=y
CONFIG_F2FS_IO_TRACE=y
CONFIG_F2FS_FS_ENCRYPTION=y

    然后再make就会在.config文件里看到编译F2FS的有关配置。
    根据ext4的相关设置，简化这部分的设置
    CONFIG_F2FS_FS=y
    CONFIG_F2FS_STAT_FS=y
    CONFIG_F2FS_FS_SECURITY=y
    CONFIG_F2FS_FS_ENCRYPTION=y
    
    所有都有：
    CONFIG_F2FS_FS=y
CONFIG_F2FS_STAT_FS=y
CONFIG_F2FS_FS_ATTR=y
CONFIG_F2FS_POSIX_ACL=y
CONFIG_F2FS_FS_SECURITY=y
CONFIG_F2FS_IO_TRACE=y
CONFIG_F2FS_CHECK_FS=y
CONFIG_F2FS_FS_ENCRYPTION=y
CONFIG_F2FS_FAULT_INJECTION=y

3.make ARCH=arm64 CROSS_COMPILE=aarch64-linux-android- -j24//交叉编译，这一句很重要。成不成就看是不是编译successfully。
踩雷3：在服务器上交叉编译安卓内核源码，前期老是报错找不到编译器， Google后可能需要下载对应的编译器，但是有两句话比较神，
至于为什么比较神我现在还无法解释。就是在之前编译aosp的目录做以下两句话：<cd /mnt/aosp> 

        source build/envsetup.sh

        lunch hikey960-userdebug

之后再做就没有问题了。我的理解是进入之前编译aosp的目录修改了编译环境，使得可以编译安卓内核。

一个新的文件夹解压后编译时，报错找不到某个文件。
arch/arm64/boot/dts/arm/juno.dts:11:54: fatal error: dt-bindings/interrupt-controller/arm-gic.h:
No such file or directory

cd arch/arm64/boot/dts/include
ln -s ../../../../../include/dt-bindings dt-bindings

### QHW's way

```
cd hikey-linaro
#查看有什么分支
git branch -a
#切换至分支android-hikey-linaro-4.9
git checkout -b android-hikey-linaro-4.9 origin/android-hikey-linaro-4.9
#配置一下环境变量
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-android-
#生成默认.config文件
make hikey960_defconfig
#根据自己的需要进行配置
make menuconfig
#交叉编译
make -j24
```

交叉编译遇到的问题和解决方法：

1. 交叉编译环境问题：

解法（同上一步骤）：

```
source build/envsetup.sh
lunch hikey960-userdebug
```

2. 编译出错

这一步比较麻烦

可能的原因1：在于make的时候需要访问google网站，而机器无法翻墙导致的。

```
cd hikey-linaro
vim kernel/configs/android-fetch-configs.sh
# 在这里我们看到其实他是要从google上下载一个文件下来然后进行解压，我的解决方法是在自己的能翻墙的电脑上把这个文件下载下来，然后FTP到服务器上放置在
/mnt/sdc/aosp/temp/configs-master-android-4.9.tar.gz
# 然后我修改这个我们用vim打开的文件
cp /mnt/sdc/aosp/temp/configs-master-android-4.9.tar.gz ./
tar -xvzf configs-master-android-4.9.tar.gz
```

使用默认的config，就可以编译通过了（由于默认的编译方式是使用gcc而不是clang，所以报了一堆warning，不过最后还是通过了）.

可能的原因2：

修改config的时候，仍然需要ARCH=arm64 CROSS_COMPILE=aarch64-linux-android-，不能简单的make menuconfig。用export的方式会比较好。

3. 编译make ARCH=arm64 CROSS_COMPILE=aarch64-linux-android- -j24 时可能出现的错误：
```
arch/arm64/Makefile:27: ld does not support --fix-cortex-a53-843419; kernel may be susceptible to erratum
./scripts/gcc-version.sh: line 25: aarch64-linux-android-gcc: command not found
./scripts/gcc-version.sh: line 26: aarch64-linux-android-gcc: command not found
make: aarch64-linux-android-gcc: Command not found
make: aarch64-linux-android-gcc: Command not found
  CHK     include/config/kernel.release
make: aarch64-linux-android-gcc: Command not found
Cannot use CONFIG_CC_STACKPROTECTOR_STRONG: -fstack-protector-strong not supported by compiler
Makefile:1182: recipe for target 'prepare-compiler-check' failed
make: *** [prepare-compiler-check] Error 1
make: *** Waiting for unfinished jobs....
make: *** wait: No child processes.  Stop.
```
原因是没有找到前面安装的NDK里面的aarch64-linux-android-gcc编译器
aarch64-linux-android-gcc位于NDK目下android-ndk-r16b/toolchains/aarch64-linux-android-4.9/prebuilt/linux-x86_64/bin
解决办法就是在环境变量PATH上加上这个目录就写
```
export NDK=/mnt/ssd/android-ndk-r16bi
export AARCH=/mnt/ssd/android-ndk-r16b/toolchains/aarch64-linux-android-4.9/prebuilt/linux-x86_64/bin
export PATH=$AARCH:$NDK:$PATH
```

4. 编译较新版本内核时，因为gcc被弃用，可以使用gnu的交叉编译器
 ```sudo apt install aarch64-linux-gnu-
 export CROSS_COMPILE=aarch64-linux-gnu-```

## （四）按照官方文档中所说的那样复制hikey960.dtb和Image.gz到指定目录生成boot.img。在de21上的相关语句如下：     

```
cp /mnt/hikey-linaro-google/hikey-linaro/arch/arm64/boot/dts/hisilicon/hi3660-hikey960.dtb /mnt/aosp/device/linaro/hikey-kernel
cp /mnt/hikey-linaro-google/hikey-linaro/arch/arm64/boot/Image.gz /mnt/aosp/device/linaro/hikey-kernel
cd /mnt/aosp/device/linaro/hikey-kernel
```
```
# 下面的是在ca01上：
cp /home/ylh/hikey960/hikey-linaro/arch/arm64/boot/dts/hisilicon/hi3660-hikey960.dtb /home/ylh/aosp/device/linaro/hikey-kernel
cp /home/ylh/hikey960/hikey-linaro/arch/arm64/boot/Image.gz /home/ylh/aosp/device/linaro/hikey-kernel
cd home/ylh/aosp/device/linaro/hikey-kernel


mv hi3660-hikey960.dtb hi3660-hikey960.dtb-4.9
mv Image.gz  Image.gz-hikey960-4.9
```

### QHW's way

其实刚才第三步编译成功之后，编译好的东西会放在`arch/arm64/boot/`这个目录下，主要是两个文件：`hi3660-hikey960.dtb`和`Image.gz-dtb`。
现在我们的工作是需要把这编译好的内核和AOSP的东西结合在一起使用。其方法就是把这两个文件copy到aosp的对应目录中，aosp存放内核的目录为` device/VENDOR/NAME-kernel`，在我们这个情况就是`device/linaro/hikey-kernel/`。
这一步其实是为下一步做准备。

### PS
2020-06-09后的更新：
https://cs.android.com/android/_/android/device/linaro/hikey-kernel/+/cc19ab9f1d4653593baa5f113cbcd9b2d7e662cb:hikey960/;bpv=1;bpt=0;drc=650d521ebff78c63c707070b213c4761c9aff65e
```
Switch to using "<board>/<kernel ver>/<file>" rather then "<file>-<board>-<kernel ver>"
```
 也就是说hi3660-hikey960.dtb 和 Image.gz直接复制到目录hikey-kernel/hikey960/4.9下面，之前是用后缀名的方式命名，现在以目录的方式命名。（没有4.9这个子目录，可以自己创建一个）


## （五）生成boot.img和dt.img。目前我们的操作只要刷boot.img。如果是要刷整个板子的话，那么要按官方文档上的做全套。

```
cd /mnt/aosp/
make bootimage
make bootimage out/target/product/hikey960/dt.img
```


## （六）利用x-ftp将boot.img传递到自己的电脑主机。当然直接在服务器上fastboot和adb调试也是可以的，但是跑到小机房去比较麻烦。

   cd out/target/product/hikey960/

在主机windows桌面上建立一个放boot.img的文件夹或直接放到桌面上，随自己开心吧。拨3，拨到与1方向一致

```
fastboot flash boot 你放boot.img的目录/boot.img
fastboot flash dts 你放dt.img的目录/dt.img
```

成功会显示刷进去多少s，时间很短。

fastboot 报错问题参考：
https://bbs.ixmoe.com/t/topic/17256


adb 和 fastboot 找不到设备的解决方法：
windows可能缺少了adb usb驱动，可以在官网找到这个驱动然后安装，但是我没有成功。
简单粗暴的方法：下载驱动精灵，让它自动给我下载一个Android手机驱动，然后就可以了。

## （七）adb调试。<此时将3拨回来>。管理员身份运行cmd，进入adb.exe路径，可以将fastboot和adb加入PATH，这样可以cmd之后直接fastboot和adb操作。

adb devices //连接上数据会出现0123456789...DEF

adb shell  //进入adb调试

adb root //获得root权限，在往hikey960中push文件，即写操作时需要权限

adb push D:\...windows上放test.txt文件的目录 /data/f2fs //将测试文件push到指定目录，相当于是写文件

adb shell进入调试模式后：

su  //获得root权限

mount -o remount,rw /dev/block/sdd14 /data  //修改/data所在的权限，使得可以读写。在/data下

mkdir /data/f2fs作为挂载点。

mkfs.f2fs -l f2fs /dev/block/sdd13 //将指定分区sdd13格式化为f2fs，在再次开机后挂载时这句话可以不做。

mount -t f2fs /dev/block/sdd13 /data/f2fs //挂载f2fs

dmesg //此时dmesg可以看到打印出来的挂载信息

cd /data/f2fs

cat test.txt //可以读取指定文件的内容


关闭ssr:    
echo 0 > /sys/fs/f2fs/sdd14/min_ssr_sections
