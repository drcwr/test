
## ubuntu ip config

static ip

    cat /etc/network/interfaces

    auto enp0s3
    iface enp0s3 inet static
    address 192.168.1.40
    netmask 255.255.255.0
    gateway 192.168.1.1
