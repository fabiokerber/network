# Dnsmasq

# Adicionar entradas DNS Ubuntu Server dns adicional (18.04+) #
```
$ sudo vi /etc/netplan/00-installer-config.yaml
    nameservers:
        addresses:
        - 8.8.8.8
        - 192.168.0.120
$ sudo netplan apply
```

# Dnsmasq no Ubuntu Server (192.168.0.120)#
https://blog.csainty.com/2016/09/running-dnsmasq-in-docker.html<br>
https://askubuntu.com/questions/191226/dnsmasq-failed-to-create-listening-socket-for-port-53-address-already-in-use<br>
```
$ sudo apt install -y docker.io net-tools && sudo systemctl start docker && sudo systemctl enable docker

$ sudo systemctl stop systemd-resolved
$ sudo systemctl disable systemd-resolved
$ sudo systemctl mask systemd-resolved

$ mkdir /etc/dnsmasq
$ sudo vi /etc/dnsmasq/0.base.conf
domain-needed
bogus-priv
no-hosts
keep-in-foreground
no-resolv
expand-hosts
server=8.8.8.8
server=8.8.4.4

$ sudo vi /etc/dnsmasq/1.rancher.conf
address=/rancher.master.kerberos.com/192.168.0.120
address=/rancher.k8s1.kerberos.com/192.168.0.121
address=/rancher.k8s2.kerberos.com/192.168.0.122
address=/rancher.k8s3.kerberos.com/192.168.0.123

$ sudo docker run -d --name dnsmasq --cap-add=NET_ADMIN --net=host -v /etc/dnsmasq:/etc/dnsmasq storytel/dnsmasq

$ sudo systemd-resolve --flush-caches (limpar cache dns nodes...)
```