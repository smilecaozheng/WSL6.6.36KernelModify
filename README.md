# wsl_kernel_modify
更换WSL内核
在wsl中

### 1.安装编译依赖

```bash
sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev 
sudo apt install flex bison bc cpio
```

### 2.下载发行版

```bash
cd ~
wget https://github.com/microsoft/WSL2-Linux-Kernel/archive/refs/tags/linux-msft-wsl-6.6.36.6.tar.gz
```

### 3.解压源码

```bash
tar -xzf linux-msft-wsl-6.6.36.6.tar.gz
```

### 4.下载编译配置文件

```
git clone https://github.com/Zhangkuns/WSL6.6.36Kernel.git
cp ~/WSL6.6.36Kernel/config-wsl-full ~/WSL2-Linux-Kernel-linux-msft-wsl-6.6.36.6/
```

### 5.编译源码

```
cd  WSL2-Linux-Kernel-linux-msft-wsl-6.6.36.6/
sudo make -j8 KCONFIG_CONFIG=config-wsl-full
sudo make modules_install headers_install
```

- ​	sudo make -j8 KCONFIG_CONFIG=Microsoft/config-wsl-full 
- ​	-j4，使得make最多允许编译命令通过4个线程同时执行，这样可以更有效的利用CPU资源，使得编译速度更快。
- ​	sudo make modules_install headers_install 
- ​	安装内核模块和头
- ​	之后一直点回车使用默认参数就行



编译完成后，我们可以看到，编译好的新内核文件`bzImage`位于

arch/x86/boot/bzImage

### 5.复制内核

使用复制命令，将其复制到Windows的F盘，Windows的F盘位于WSL的/mnt/c 或者 /mnt/F

```
cp arch/x86/boot/bzImage  /mnt/f
```

于是在F盘之下，我们便获得了编译后的内核文件，

### 6.替换内核

修改WSL配置文件替换内核

WSL配置文件在C盘%USERPROFILE%目录下，没有可以自己创建.wslconfig。添加以下内容

```
[wsl2]
kernel=F:\\bzImage
```

在替换内核之前，我们需要用带管理员权限的终端关闭wsl

```
wsl --shutdown
```

查看是否关闭

```
wsl -l -v
```

替换掉对应的内核，重启WSL，内核即更换完成。
