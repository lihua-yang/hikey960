compile AOSP 12.0   
===========    
download AOSP 12.0 code
_________   
# 1.中科院的AOSP repo   
但没有android kernel（后期解决）    
来自于https://mirrors.ustc.edu.cn/help/aosp.html    
```
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
## 如果上述 URL 不可访问，可以用下面的：
## curl -sSL  'https://gerrit-googlesource.proxy.ustclug.org/git-repo/+/master/repo?format=TEXT' |base64 -d > ~/bin/repo
chmod a+x ~/bin/repo
```

# 2.初始化仓库   
这里直接确定 版本是android-12.0.0_r21   
```
mkdir aosp   
cd aosp  
repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest -b android-12.0.0_r21  
repo sync -j4  
```   

# 3.编译aosp12  
```  
. build/envsetup.sh   
lunch hikey960-userdebug   
make -j32
```   
aosp12与aosp9编译后的结果已经有较大的不同，生成的镜像也有比较大的不同    

# 4.刷录镜像
ubuntu上刷录镜像的log：   
```
tan@tan-PowerEdge-R730:/mnt/pixel3/aosp/device/linaro/hikey/installer/hikey960$ sudo sh uefi-flash-all.sh 
=== android out dir:./../../../../..//out/target/product/hikey960
=== Checking partition table version...
erasing 'reserved'...
OKAY [  0.039s]
finished. total time: 0.039s
=== Partition table is 1MB aligned. Flashing ATF/UEFI...
target reported max download size of 471859200 bytes
sending 'ptable' (24 KB)...
OKAY [  0.002s]
writing 'ptable'...
OKAY [  0.030s]
finished. total time: 0.032s
target reported max download size of 471859200 bytes
sending 'xloader' (151 KB)...
OKAY [  0.008s]
writing 'xloader'...
FAILED (remote: flash write back vrl failure)
finished. total time: 0.251s
rebooting into bootloader...
OKAY [  0.024s]
finished. total time: 0.074s
< waiting for device >
target reported max download size of 471859200 bytes
sending 'fastboot' (25 KB)...
OKAY [  0.002s]
writing 'fastboot'...
OKAY [  0.037s]
finished. total time: 0.038s
target reported max download size of 471859200 bytes
sending 'fip' (1483 KB)...
OKAY [  0.037s]
writing 'fip'...
OKAY [  0.030s]
finished. total time: 0.067s
target reported max download size of 471859200 bytes
sending 'nvme' (128 KB)...
OKAY [  0.004s]
writing 'nvme'...
OKAY [  0.054s]
finished. total time: 0.058s
target reported max download size of 471859200 bytes
sending 'fw_lpm3' (212 KB)...
OKAY [  0.011s]
writing 'fw_lpm3'...
OKAY [  0.021s]
finished. total time: 0.032s
target reported max download size of 471859200 bytes
sending 'trustfirmware' (145 KB)...
OKAY [  0.009s]
writing 'trustfirmware'...
OKAY [  0.024s]
finished. total time: 0.033s
rebooting into bootloader...
OKAY [  0.030s]
finished. total time: 0.080s
< waiting for device >
target reported max download size of 134217728 bytes
sending 'ptable' (24 KB)...
OKAY [  0.008s]
writing 'ptable'...
OKAY [  0.004s]
finished. total time: 0.012s
target reported max download size of 134217728 bytes
sending 'xloader' (151 KB)...
OKAY [  0.008s]
writing 'xloader'...
OKAY [  0.226s]
finished. total time: 0.234s
target reported max download size of 134217728 bytes
sending 'fastboot' (25 KB)...
OKAY [  0.008s]
writing 'fastboot'...
OKAY [  0.054s]
finished. total time: 0.062s
target reported max download size of 134217728 bytes
sending 'fip' (1483 KB)...
OKAY [  0.053s]
writing 'fip'...
OKAY [  0.206s]
finished. total time: 0.260s
target reported max download size of 134217728 bytes
sending 'boot' (15766 KB)...
OKAY [  0.390s]
writing 'boot'...
OKAY [  0.202s]
finished. total time: 0.592s
target reported max download size of 134217728 bytes
sending sparse 'super' (131068 KB)...
OKAY [  3.221s]
writing 'super'...
OKAY [  0.929s]
sending sparse 'super' (131068 KB)...
OKAY [  3.220s]
writing 'super'...
OKAY [  0.936s]
sending sparse 'super' (131045 KB)...
OKAY [  3.313s]
writing 'super'...
OKAY [  1.104s]
sending sparse 'super' (126301 KB)...
OKAY [  3.135s]
writing 'super'...
OKAY [  0.928s]
sending sparse 'super' (129572 KB)...
OKAY [  3.168s]
writing 'super'...
OKAY [  0.918s]
sending sparse 'super' (131068 KB)...
OKAY [  3.229s]
writing 'super'...
OKAY [  0.969s]
sending sparse 'super' (120868 KB)...
OKAY [  2.954s]
writing 'super'...
OKAY [  0.905s]
sending sparse 'super' (127836 KB)...
OKAY [  3.145s]
writing 'super'...
OKAY [  0.926s]
sending sparse 'super' (126829 KB)...
OKAY [  3.174s]
writing 'super'...
OKAY [  1.006s]
sending sparse 'super' (131068 KB)...
OKAY [  3.228s]
writing 'super'...
OKAY [  0.958s]
sending sparse 'super' (82576 KB)...
OKAY [  2.017s]
writing 'super'...
OKAY [  0.645s]
finished. total time: 44.026s
Creating filesystem with parameters:
    Size: 268435456
    Block size: 4096
    Blocks per group: 32768
    Inodes per group: 8192
    Inode size: 256
    Journal blocks: 1024
    Label: 
    Blocks: 65536
    Block groups: 2
    Reserved block group size: 15
Created filesystem with 11/16384 inodes and 2089/65536 blocks
target reported max download size of 134217728 bytes
erasing 'cache'...
OKAY [  0.043s]
sending 'cache' (6248 KB)...
OKAY [  0.174s]
writing 'cache'...
OKAY [  0.040s]
finished. total time: 0.257s
rebooting...

finished. total time: 2.004s
```

# 5.hikey960板子的工作情况   
（1）板子的电源灯和wifi灯不显示    
（2）df -h:   
```
hikey960:/ # df -h
Filesystem        Size Used Avail Use% Mounted on
/dev/block/dm-0   795M 793M  2.4M 100% /
tmpfs             1.4G 1.0M  1.4G   1% /dev
tmpfs             1.4G    0  1.4G   0% /mnt
/dev/block/dm-1   201M 201M  628K 100% /vendor
/dev/block/dm-2   127M 126M  408K 100% /system_ext
/dev/block/dm-3   229M 228M  712K 100% /product
tmpfs             1.4G  12K  1.4G   1% /apex
tmpfs             1.4G 464K  1.4G   1% /linkerconfig
/dev/block/sdd13   24G 954M   23G   4% /data
/dev/block/sdd5   248M 200K  248M   1% /metadata
tmpfs             1.4G    0  1.4G   0% /data_mirror
/dev/block/loop7  4.8M 4.8M   28K 100% /apex/com.android.conscrypt@319999900
/dev/block/loop6   40M  40M   28K 100% /apex/com.android.vndk.v31@1
/dev/block/loop5  232K 152K   80K  66% /apex/com.android.scheduling@319999900
/dev/block/loop9  2.0M 1.9M   28K  99% /apex/com.android.appsearch@300000000
/dev/block/loop10  35M  35M   32K 100% /apex/com.android.i18n@1
/dev/block/loop11 700K 672K   28K  96% /apex/com.android.sdkext@319999900
/dev/block/loop4  1.6M 1.6M   28K  99% /apex/com.android.os.statsd@319999900
/dev/block/loop13 232K  88K  144K  38% /apex/com.android.apex.cts.shim@1
/dev/block/loop8  832K 804K   28K  97% /apex/com.android.tzdata@319999900
/dev/block/loop12 7.5M 7.5M   36K 100% /apex/com.android.runtime@1
/dev/block/dm-4   8.2M 8.2M   32K 100% /apex/com.android.adbd@319999999
/dev/block/dm-7   7.9M 7.9M   28K 100% /apex/com.android.neuralnetworks@319999900
/dev/block/dm-5   2.6M 2.5M   28K  99% /apex/com.android.tethering@319999900
/dev/block/dm-6   692K 664K   28K  96% /apex/com.android.ipsec@319999900
/dev/block/dm-8   2.4M 2.4M   28K  99% /apex/com.android.resolv@319999900
/dev/block/dm-9   5.6M 5.5M   28K 100% /apex/com.android.wifi@319999900
/dev/block/dm-11   60M  60M   36K 100% /apex/com.android.art@319999900
/dev/block/dm-10   19M  19M   32K 100% /apex/com.android.media.swcodec@319999900
/dev/block/dm-13   18M  18M   28K 100% /apex/com.android.permission@319999900
/dev/block/dm-12  5.3M 5.3M   32K 100% /apex/com.android.extservices@319999900
/dev/block/dm-14  6.5M 6.4M   28K 100% /apex/com.android.media@319999900
/dev/block/dm-15  4.2M 4.1M   32K 100% /apex/com.android.mediaprovider@319999910
```

(3) mount情况：   
```
hikey960:/ # mount
/dev/block/dm-0 on / type ext4 (ro,seclabel,noatime,errors=panic)
tmpfs on /dev type tmpfs (rw,seclabel,nosuid,relatime,mode=755)
devpts on /dev/pts type devpts (rw,seclabel,relatime,mode=600,ptmxmode=000)
proc on /proc type proc (rw,relatime,gid=3009,hidepid=2)
sysfs on /sys type sysfs (rw,seclabel,relatime)
selinuxfs on /sys/fs/selinux type selinuxfs (rw,relatime)
tmpfs on /mnt type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755,gid=1000)
tmpfs on /mnt/installer type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755,gid=1000)
tmpfs on /mnt/androidwritable type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755,gid=1000)
/dev/block/dm-1 on /vendor type ext4 (ro,seclabel,noatime,errors=panic)
/dev/block/dm-2 on /system_ext type ext4 (ro,seclabel,noatime,errors=panic)
/dev/block/dm-3 on /product type ext4 (ro,seclabel,noatime,errors=panic)
tmpfs on /apex type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755)
tmpfs on /linkerconfig type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755)
none on /dev/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
none on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime)
none on /dev/cpuctl type cgroup (rw,nosuid,nodev,noexec,relatime,cpu)
none on /dev/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset,noprefix,release_agent=/sbin/cpuset_release_agent)
none on /dev/memcg type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
tracefs on /sys/kernel/tracing type tracefs (rw,seclabel,relatime)
none on /config type configfs (rw,nosuid,nodev,noexec,relatime)
binder on /dev/binderfs type binder (rw,relatime,max=1048576,stats=global)
none on /sys/fs/fuse/connections type fusectl (rw,relatime)
none on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime)
pstore on /sys/fs/pstore type pstore (rw,seclabel,nosuid,nodev,noexec,relatime)
/sys/kernel/debug on /sys/kernel/debug type debugfs (rw,seclabel,relatime,mode=755)
tracefs on /sys/kernel/debug/tracing type tracefs (rw,seclabel,relatime)
/dev/block/sdd13 on /data type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
/dev/block/sdd5 on /metadata type ext4 (rw,seclabel,nosuid,nodev,noatime,discard)
tmpfs on /storage type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755,gid=1000)
tmpfs on /linkerconfig type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755)
/dev/block/sdd13 on /data/user/0 type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
tmpfs on /data_mirror type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=700,gid=1000)
/dev/block/sdd13 on /data_mirror/data_ce/null type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
/dev/block/sdd13 on /data_mirror/data_ce/null/0 type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
/dev/block/sdd13 on /data_mirror/data_de/null type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
/dev/block/sdd13 on /data_mirror/cur_profiles type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
/dev/block/sdd13 on /data_mirror/ref_profiles type f2fs (rw,lazytime,seclabel,nosuid,nodev,noatime,background_gc=on,discard,no_heap,user_xattr,inline_xattr,acl,inline_data,inline_dentry,flush_merge,extent_cache,mode=adaptive,active_logs=6,alloc_mode=default,fsync_mode=posix)
/dev/block/loop7 on /apex/com.android.conscrypt@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop6 on /apex/com.android.vndk.v31@1 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop7 on /apex/com.android.conscrypt type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop6 on /apex/com.android.vndk.v31 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop5 on /apex/com.android.scheduling@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop5 on /apex/com.android.scheduling type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop9 on /apex/com.android.appsearch@300000000 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop9 on /apex/com.android.appsearch type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop10 on /apex/com.android.i18n@1 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop10 on /apex/com.android.i18n type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop11 on /apex/com.android.sdkext@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop4 on /apex/com.android.os.statsd@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop11 on /apex/com.android.sdkext type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop4 on /apex/com.android.os.statsd type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop13 on /apex/com.android.apex.cts.shim@1 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop8 on /apex/com.android.tzdata@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop13 on /apex/com.android.apex.cts.shim type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop8 on /apex/com.android.tzdata type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop12 on /apex/com.android.runtime@1 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/loop12 on /apex/com.android.runtime type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-4 on /apex/com.android.adbd@319999999 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-7 on /apex/com.android.neuralnetworks@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-7 on /apex/com.android.neuralnetworks type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-4 on /apex/com.android.adbd type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-5 on /apex/com.android.tethering@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-5 on /apex/com.android.tethering type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-6 on /apex/com.android.ipsec@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-6 on /apex/com.android.ipsec type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-8 on /apex/com.android.resolv@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-8 on /apex/com.android.resolv type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-9 on /apex/com.android.wifi@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-9 on /apex/com.android.wifi type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-11 on /apex/com.android.art@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-11 on /apex/com.android.art type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-10 on /apex/com.android.media.swcodec@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-13 on /apex/com.android.permission@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-13 on /apex/com.android.permission type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-12 on /apex/com.android.extservices@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-10 on /apex/com.android.media.swcodec type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-12 on /apex/com.android.extservices type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-14 on /apex/com.android.media@319999900 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-14 on /apex/com.android.media type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-15 on /apex/com.android.mediaprovider@319999910 type ext4 (ro,dirsync,seclabel,nodev,noatime)
/dev/block/dm-15 on /apex/com.android.mediaprovider type ext4 (ro,dirsync,seclabel,nodev,noatime)
tmpfs on /apex/apex-info-list.xml type tmpfs (rw,seclabel,nosuid,nodev,noexec,relatime,mode=755)
adb on /dev/usb-ffs/adb type functionfs (rw,relatime)
mtp on /dev/usb-ffs/mtp type functionfs (rw,relatime)
ptp on /dev/usb-ffs/ptp type functionfs (rw,relatime)
```
与AOSP9.0不同，cache分区做metadata，data分区直接是f2fs，system则分为多个分区，越发与商用智能手机的分区相近

（4）uname -a/r:   
```
hikey960:/ # uname -a
Linux localhost 5.4.96-android12-0-00124-g150b1f5ee283-ab7134844 #1 SMP PREEMPT Tue Feb 9 14:03:58 UTC 2021 aarch64
hikey960:/ # uname -r
5.4.96-android12-0-00124-g150b1f5ee283-ab7134844
```

(5) blkid:   
```
hikey960:/ # blkid
/dev/block/loop0: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop1: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop2: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop3: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop4: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop5: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop6: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop7: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop8: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop9: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop10: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop11: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop12: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop13: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop14: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop15: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/sdd5: UUID="57f8f4bc-abf4-655f-bf67-946fc0f9f25b" TYPE="ext4" 
/dev/block/sdd11: LABEL="vendor" UUID="774de7f3-5834-56f1-8b17-d77ff7eacf11" TYPE="ext4" 
/dev/block/sdd13: UUID="0f266447-7414-4b61-b874-16b54825d012" TYPE="f2fs" 
/dev/block/loop16: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop17: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop18: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop19: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop20: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop21: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop22: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop23: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop24: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
/dev/block/loop25: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4" 
```
compile android kernel 5.4
============
download common-android12-5.4 code
___________
# 1.下载kernel源码   
在清华镜像网站和中科大镜像网站无法直接找到android kernel源码，有中科大的git可以使用，但是需要分别下载，下载后放在一个文件夹里面，在编译时仍然报错各种bug，需要不断打补丁。   
最后可用的方法是在ubuntu下翻墙下载google官网镜像。   
```
mkdir repo-common
cd repo-common
repo init -u https://android.googlesource.com/kernel/manifest -b common-android12-5.4
repo sync -j8 -c
rm -rf out
BUILD_CONFIG=common/build.config.hikey960 build/build.sh
```
注意其中的rm -rf out的out文件夹是在编译一次后出现的镜像文件夹，第一次时可以不用删除。此外，如果编译过程中出现奇怪的bug，大概率是repo sync源码时不完整，需要再sync一次。另外，报关于python程序运行的问题时，多半是与python2与python3的版本相关。因为android kernel编译时使用python3，而第一次使用repo时使用系统默认的python2，此处有冲突，可以用指定路径的repo来sync。如在big77机器上使用：~/.bin/repo sync -j8 -c      
翻墙设置：    
https://notes-by-yangjinjie.readthedocs.io/zh_CN/latest/os/ubuntu-debian/Ubuntu%20Server%E5%91%BD%E4%BB%A4%E8%A1%8C%E9%85%8D%E7%BD%AEshadowsocks%E5%85%A8%E5%B1%80%E4%BB%A3%E7%90%86.html     
https://www.cnblogs.com/yifeichongtian/p/14091231.html    


# 2.删除与串联dtb
删除 ${AOSP_TOPDIR}device/linaro/hikey-kernel/hikey960/5.4/ 中的所有对象，然后将 out/android12-5.4/dist/ 中的内核 build 中的 build 工件复制到 ${AOSP_TOPDIR}/device/linaro/hikey-kernel/hikey960/5.4/    
注：此处的build工件不确定是哪个，目前是复制整个目录    
```
cat device/linaro/hikey-kernel/hikey960/5.4/Image.gz device/linaro/hikey-kernel/hikey960/5.4/hi3660-hikey960.dtb  > device/linaro/hikey-kernel/hikey960/5.4/Image.gz-dtb
```
我理解的就是将Image.gz和hi3660-hikey960.dtb串联到编译aosp时需要的Image.gz-dtb    

# 3.编译AOSP  
```
lunch hikey960-userdebug
make TARGET_KERNEL_USE=5.4 HIKEY_USES_GKI=true -j24
```

# 4.编译后的结果
```
hikey960:/ # uname -a
Linux localhost 5.4.147-android12-9 #1 SMP PREEMPT Wed Dec 15 20:28:35 UTC 2021 aarch64
```
版本号由之前的5.4.96变为了5.4.147   

data=8GB f2fs
============
# 1.data=8GB
通过修改BoardConfig.mk修改data分区大小即可，其他fstab.hikey960等文件不用修改，hisi-ptable.img也无需修改。    
vim device/linaro/hikey/hikey960/BoardConfig.mk 
```
BOARD_BOOTIMAGE_PARTITION_SIZE := 67108864
#BOARD_USERDATAIMAGE_PARTITION_SIZE := 25845301248 # 24648MB
BOARD_USERDATAIMAGE_PARTITION_SIZE := 8589934592    #8GB
BOARD_FLASH_BLOCK_SIZE := 512
```

# 2.compile AOSP with data=f2fs
在AOSP目录    
```
. ./build/envsetup.sh
lunch hikey960-userdebug
make TARGET_KERNEL_USE=5.4 HIKEY_USES_GKI=true -j24
make BOARD_USERDATAIMAGE_FILE_SYSTEM_TYPE=f2fs TARGET_USERIMAGES_USE_F2FS=true TARGET_KERNEL_USE=5.4 HIKEY_USES_GKI=true -j32
```

# 3.fastboot flash
在flash-all.sh中经过不同的判断条件刷录的镜像比较多，在windows下刷录镜像的bat文件修改为如下所示    
vim device/linaro/hikey/installer/hikey960/flash-all.sh    
在flash-all.sh中没有刷录data.img，直接刷录时data分区是f2fs，但修改data分区大小为8GB后，刷录的data分区为ext4，不能禁用fstab.hikey960中USERDATAIMAGE关于ext4的设置，板子会无法adb，需要刷录data.img    
```
    fastboot flash ptable prm_ptable.img
	fastboot flash xloader hisi-sec_xloader.img
	fastboot reboot-bootloader

	fastboot flash fastboot l-loader.bin
	fastboot flash fip fip.bin
	fastboot flash nvme hisi-nvme.img
	fastboot flash fw_lpm3 hisi-lpm3.img
	fastboot flash trustfirmware hisi-bl31.bin
	fastboot reboot-bootloader

	fastboot flash ptable prm_ptable.img
	fastboot flash xloader hisi-sec_xloader.img
	fastboot flash fastboot l-loader.bin
	fastboot flash fip fip.bin

	fastboot flash boot boot.img
	fastboot flash super super.img
	fastboot flash userdata  userdata.img
	fastboot format cache

::    fastboot flash xloader hisi-sec_xloader.img
::	fastboot flash ptable hisi-ptable.img
::	fastboot flash fastboot hisi-fastboot.img
::	fastboot reboot-bootloader
fastboot reboot
pause
```

# 4.刷录结果log
在刷录时有fastboot flash fastboot l-loader.bin和第二次的fastboot flash ptable prm_ptable.img的write失败，但是不影响板子正常运行    
```
hikey960:/ # blkid
/dev/block/loop0: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop1: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop2: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop3: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop4: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop5: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop6: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop7: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop8: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop9: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop10: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop11: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop12: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop13: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop14: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop15: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/sdd5: UUID="547a110a-76ac-11ec-96af-457af1b70166" TYPE="ext4"
/dev/block/sdd11: LABEL="vendor" UUID="774de7f3-5834-56f1-8b17-d77ff7eacf11" TYPE="ext4"
/dev/block/sdd13: UUID="5739724d-eb62-4e34-8524-2ee203981da9" TYPE="f2fs"
/dev/block/loop16: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop17: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop18: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop19: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop20: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop21: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop22: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop23: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop24: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"
/dev/block/loop25: UUID="7d1522e1-9dfa-5edb-a43e-98e3a4d20250" TYPE="ext4"

hikey960:/ # df -h
Filesystem        Size Used Avail Use% Mounted on
/dev/block/dm-0   795M 793M  2.4M 100% /
tmpfs             1.4G 1.0M  1.4G   1% /dev
tmpfs             1.4G    0  1.4G   0% /mnt
/dev/block/dm-1   201M 201M  628K 100% /vendor
/dev/block/dm-2   127M 126M  408K 100% /system_ext
/dev/block/dm-3   229M 228M  712K 100% /product
tmpfs             1.4G  12K  1.4G   1% /apex
tmpfs             1.4G 464K  1.4G   1% /linkerconfig
/dev/block/sdd13  7.9G 586M  7.4G   8% /data
/dev/block/sdd5   232M 120K  232M   1% /metadata
tmpfs             1.4G    0  1.4G   0% /data_mirror
/dev/block/loop6  1.6M 1.6M   28K  99% /apex/com.android.os.statsd@319999900
/dev/block/loop7  232K 152K   80K  66% /apex/com.android.scheduling@319999900
/dev/block/loop5   40M  40M   28K 100% /apex/com.android.vndk.v31@1
/dev/block/loop8  832K 804K   28K  97% /apex/com.android.tzdata@319999900
/dev/block/loop10  35M  35M   32K 100% /apex/com.android.i18n@1
/dev/block/loop11 700K 672K   28K  96% /apex/com.android.sdkext@319999900
/dev/block/loop4  4.8M 4.8M   28K 100% /apex/com.android.conscrypt@319999900
/dev/block/loop13 232K  88K  144K  38% /apex/com.android.apex.cts.shim@1
/dev/block/loop9  2.0M 1.9M   28K  99% /apex/com.android.appsearch@300000000
/dev/block/loop12 7.5M 7.5M   36K 100% /apex/com.android.runtime@1
/dev/block/dm-5   2.6M 2.5M   28K  99% /apex/com.android.tethering@319999900
/dev/block/dm-7   7.9M 7.9M   28K 100% /apex/com.android.neuralnetworks@319999900
/dev/block/dm-4   8.2M 8.2M   32K 100% /apex/com.android.adbd@319999999
/dev/block/dm-6   692K 664K   28K  96% /apex/com.android.ipsec@319999900
/dev/block/dm-11   60M  60M   36K 100% /apex/com.android.art@319999900
/dev/block/dm-8   2.4M 2.4M   28K  99% /apex/com.android.resolv@319999900
/dev/block/dm-9   5.6M 5.5M   28K 100% /apex/com.android.wifi@319999900
/dev/block/dm-12  5.3M 5.3M   32K 100% /apex/com.android.extservices@319999900
/dev/block/dm-10   19M  19M   32K 100% /apex/com.android.media.swcodec@319999900
/dev/block/dm-14  6.5M 6.4M   28K 100% /apex/com.android.media@319999900
/dev/block/dm-13   18M  18M   28K 100% /apex/com.android.permission@319999900
/dev/block/dm-15  4.2M 4.1M   32K 100% /apex/com.android.mediaprovider@319999910

hikey960:/ # uname -a
Linux localhost 5.4.147-android12-9-dirty #1 SMP PREEMPT Wed Dec 15 20:28:35 UTC 2021 aarch64
```
