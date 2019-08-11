## Debug kernel in Ubuntu with Qemu
---

1. 安装QEMU   
> sudo apt-get install qemu-kvm qemu virt-manager virt-viewer libvirt-bin

2. 初体验：尝试启动自己的电脑的内核(本人用的Ubuntu 18)   
> sudo qemu-system-x86_64 -kernel /boot/vmlinuz-`uname -r`   

此处会提示错误,原因是由于没有文件系统，内核起不来，下面就添加一个文件系统。 
3. 安装debootstrap，debootstrap允许安扎在文件夹中安装deb包
> sudo apt install debootstrap

4. 安装文件系统  
> IMG=qemu-image.img  
> DIR=mount-point.dir  
> qemu-img create $IMG 1g  
> mkfs.ext2 $IMG  
> mkdir $DIR  
> sudo mount -o loop $IMG $DIR  
> sudo debootstrap --arch amd64 jessie $DIR  
> sudo umount $DIR  
> rmdir $DIR  
 5. 启动系统
> sudo qemu-system-x86_64 -kernel /boot/vmlinuz-`uname -r`\
>                           -hda qemu-image.img\
>                           -append "root=/dev/sda single"
6. 下载内核并编译
> git clone --depth=1 git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git  
> cd linux  
> make x86_64_defconfig  
> make kvmconfig  
> make -j 8  
7. 启动新内核
> qemu-system-x86_64 -kernel arch/x86/boot/bzImage  
>                      -hda qemu-image.img  
>                      -append "root=/dev/sda"  
>                       --enable-kvm  
8. 连接新的QEMU 系统
> qemu-system-x86_64 -kernel bzImage  
>                      -append "root=/dev/sda console=ttyS0"  
>                      -hda qemu-image.img  
>                     --enable-kvm  
>                      --nographic  

9. 关闭
> shutdown -h now  
