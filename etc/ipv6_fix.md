# IPV6 fix during installation

If you enter archiso, and dhcpcd requests a local IPV6 address (cuz the modem tries to give out one) even when your modem is IPV4 only, this guide can help with the fix.

## Disabling IPV6 for dhcpcd

Add the following relevant line(s) to both files.

```shell
nano /etc/sysctl.d/40-disable-ipv6.conf
```

```shell
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1
net.ipv6.conf.enp8s0.disable_ipv6 = 1
```

```shell
nano /etc/sysctl.conf
```

```shell
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1
net.ipv6.conf.enp8s0.disable_ipv6 = 1
```

> [!WARNING]  
> The network interface will probably be different on various machienes. Example interfaces (eth0, enp8s0) are provided in the two bottom lines. The active, relevant inetface can be checked with the `ip link` command.

Add these two lines at the end of the file.

```shell
nano /etc/dhcpcd.conf
```

```shell
# Disable IPv6
noipv6
noipv6rs
```

Restart dhcpcd.

```shell
systemctl restart dhcpcd
```

You should ONLY get an IPV4 address and an IPV4 default router from your modem's DHCP service.
