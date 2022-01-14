compile AOSP 12.0   
===========    
download AOSP 12.0 code    
_________   
# 1.中科院的AOSP repo，但没有android kernel（后期解决）    
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

compile android kernel 5.4
============
download common-android12-5.4 code
___________
