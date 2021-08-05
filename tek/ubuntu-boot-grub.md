
vim /etc/default/grub
GRUB_DEFAULT="1> 6"

update-grub
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.4.0-74-generic
Found initrd image: /boot/initrd.img-5.4.0-74-generic
Found linux image: /boot/vmlinuz-5.3.0-40-generic
Found initrd image: /boot/initrd.img-5.3.0-40-generic
Found linux image: /boot/vmlinuz-5.0.0-37-generic
Found initrd image: /boot/initrd.img-5.0.0-37-generic
Found linux image: /boot/vmlinuz-4.18.0-15-generic
Found initrd image: /boot/initrd.img-4.18.0-15-generic
Found Windows Boot Manager on /dev/nvme0n1p1@/EFI/Microsoft/Boot/bootmgfw.efi
Adding boot menu entry for EFI firmware configuration



uname -a
Linux ccc 4.18.0-15-generic #16~18.04.1-Ubuntu SMP Thu Feb 7 14:06:04 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

reboot

uname -a
Linux ccc 4.18.0-15-generic #16~18.04.1-Ubuntu SMP Thu Feb 7 14:06:04 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux


###############

Ubuntu下Grub配置详解
https://blog.csdn.net/humanof/article/details/100726977


Ubuntu更改默认启动内核
https://blog.csdn.net/m0_37340681/article/details/97896696

GRUB_DEFAULT=“1> 3”



要将默认内核更改为启动，可以执行以下操作：

打开文件/ etc / default / grub。

将GRUB_DEFAULT的值更改为您希望选择的菜单选项的索引值。

例如，在启动过程中的GRUB菜单中有：

Ubuntu

Advanced options for Ubuntu

Windows 10 (loader) (on /dev/sda1)

system setup

我的 “Advananced options for Ubuntu” 子菜单如下所示：

Ubuntu, with Linux 4.13.0-26-generic

Ubuntu, with Linux 4.13.0-26-generic (upstart)

Ubuntu, with Linux 4.13.0-26-generic (recovery mode)

Ubuntu, with Linux 4.10.0-42-generic

Ubuntu, with Linux 4.10.0-42-generic (upstart)

Ubuntu, with Linux 4.10.0-42-generic (recovery mode)

现在，第一个选项是索引0，第二个是1，第三个是2，依此类推。（即GRUB菜单中的 Ubuntu为0，Advanced options for Ubuntu为1，…）

在我的情况下，我想选择 “Advanced options for Ubuntu” 子菜单中的 “Ubuntu, with Linux 4.10.0-42-generic”（“以前旧的内核版本”）

设置 # sudo /etc/default/grub

将GRUB_DEFAULT设为：

GRUB_DEFAULT=“1> 3”
1
使用 ‘>’ 符号来指定有一个子菜单（注意符号 > 和数字 3 之间有空格，所以需要双引号）。在这种情况下，我在主菜单中选择第2个选项（索引1），在子菜单中选择第四个选项（索引3）。

菜单选项来自文件/boot/grub/grub.cfg（不要编辑这个文件）。

一旦对/etc/default/grub进行了更改，请保存并运行以下命令来更新GRUB配置文件（必须，否则不生效）：
sudo update-grub
重新启动，现在应该默认启动旧的内核版本。