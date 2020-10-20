uname -r

cat > /etc/yum.repos.d/kernel.repo <<eof
[kernel]
name=kernel-nokmem

baseurl=https://mirrors.bfsu.edu.cn/centos-vault/7.7.1908/updates/x86_64

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

gpgcheck=0
enabled=1
eof

yum update-to -y kernel-3.10.0-1062.4.1.el7 \
kernel-devel-3.10.0-1062.4.1.el7
kernel-headers-3.10.0-1062.4.1.el7
kernel-tools-3.10.0-1062.4.1.el7
kernel-tools-libs-3.10.0-1062.4.1.el7


reboot



cd /etc/default
cp -p grub grub.bk

GRUB_CMDLINE_LINUX="orgcmd cgroup.memory=nokmem"


grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg

reboot



cat /proc/cmdline

