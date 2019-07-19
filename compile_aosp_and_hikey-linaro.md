# Compile aosp and hikey-linaro

首先最有用的就是官方文档了，附上Google的官网地址。https://source.android.com/source/devices

官网上的说明是简明概要的，但是在我们的编译过程中其实可能会遇到各种奇奇怪怪的问题。下面就一些 我们踩过的雷给排掉。

## 准备工作

* 交叉编译工具Android NDK [https://developer.android.com/ndk/downloads/index.html](https://developer.android.com/ndk/downloads/index.html)
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
#切换至分支android-hikey-linaro-4.19
git checkout android-hikey-linaro-4.19
#生成默认.config文件
make ARCH=arm64 hikey960_defconfig
#根据自己的需要进行配置
make menuconfig
#交叉编译
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-android- -j24
```


## （四）按照官方文档中所说的那样复制hikey960.dtb和Image.gz到指定目录生成boot.img。在de21上的相关语句如下：     

cp /mnt/hikey-linaro-google/hikey-linaro/arch/arm64/boot/dts/hisilicon/hi3660-hikey960.dtb /mnt/aosp/device/linaro/hikey-kernel
cp /mnt/hikey-linaro-google/hikey-linaro/arch/arm64/boot/Image.gz /mnt/aosp/device/linaro/hikey-kernel

cd /mnt/aosp/device/linaro/hikey-kernel


////下面的是在ca01上：
cp /home/ylh/hikey960/hikey-linaro/arch/arm64/boot/dts/hisilicon/hi3660-hikey960.dtb /home/ylh/aosp/device/linaro/hikey-kernel
cp /home/ylh/hikey960/hikey-linaro/arch/arm64/boot/Image.gz /home/ylh/aosp/device/linaro/hikey-kernel
cd home/ylh/aosp/device/linaro/hikey-kernel


mv hi3660-hikey960.dtb hi3660-hikey960.dtb-4.9
mv Image.gz  Image.gz-hikey960-4.9



## （五）生成boot.img和dt.img。目前我们的操作只要刷boot.img。如果是要刷整个板子的话，那么要按官方文档上的做全套。

cd /mnt/aosp/

make bootimage

make bootimage out/target/product/hikey960/dt.img



## （六）利用x-ftp将boot.img传递到自己的电脑主机。当然直接在服务器上fastboot和adb调试也是可以的，但是跑到小机房去比较麻烦。

cd out/target/product/hikey960/

在主机windows桌面上建立一个放boot.img的文件夹或直接放到桌面上，随自己开心吧。拨3，拨到与1方向一致

fastboot flash boot 你放boot.img的目录/boot.img

fastboot flash dts 你放dt.img的目录/dt.img

成功会显示刷进去多少s，时间很短。



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
