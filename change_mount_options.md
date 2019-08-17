#修改/data分区的mount option方法

目标：原来的/data分区挂载的时候采用的是data=ordered模式，也就是元数据记日志，数据不记日志。我需要将其改为data=journal模式，源数据和数据都记日志.

```
cd aosp
vim device/linaro/hikey/hikey960/fstab.hikey960
```

里面有各个分区的挂载方式，将/data里面的data=ordered改为data=journal，保存

```
#Under aosp directory
make -j32 > makeout
```

查看makeout文件发现需要刷的文件里面只有out/target/product/hikey960/vendor.img改变了，因此只需要把这个文件刷进hikey960里面就可以了

```
fastboot flash vendor vendor.img
```

启动hikey960后查看

```
mount | grep data
```

显示

```
/dev/block/sdd13 on /data type ext4 (rw,seclabel,nosuid,nodev,noatime,discard,nodelalloc,noauto_da_alloc,data=journal)
```

说明模式修改成功
