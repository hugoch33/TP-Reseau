# TP3 : On va router des trucs
# I. ARP

## 1. Echange ARP

🌞Générer des requêtes ARP

    [hugo@localhost ~]$ ping 10.3.1.12
    PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
    64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=1.21 ms
    64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=0.573 ms
    64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.585 ms

```

    [hugo@localhost ~]$ ip neigh show
    10.3.1.12 dev enp0s3 lladdr 08:00:27:b9:79:1d STALE  == adresse MAC de marcel
    10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:0c REACHABLE

```

    [hugo@localhost ~]$ ip neigh show
    10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:0c REACHABLE 
    10.3.1.11 dev enp0s3 lladdr 08:00:27:c9:f0:58 STALE == adresse MAC de john

```
    
    [hugo@localhost ~]$ ip neigh show
    10.3.1.12 dev enp0s3 lladdr 🌞08:00:27:b9:79:1d🌞 STALE
    10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:0c REACHABLE
    
```

    [hugo@localhost ~]$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 🌞08:00:27:b9:79:1d🌞 brd ff:ff:ff:ff:ff:ff
        inet 10.3.1.12/24 brd 10.3.1.255 scope global noprefixroute enp0s3
        valid_lft forever preferred_lft forever
        inet6 fe80::a00:27ff:feb9:791d/64 scope link
        valid_lft forever preferred_lft forever


### 2. Analyse de trames

```

        $ ssh hugo@10.3.1.11
    hugo@10.3.1.11's password:
    Last login: Tue Oct 24 11:33:28 2023 from 10.3.1.1
    [hugo@localhost ~]$ sudo ip neigh flush all
    [hugo@localhost ~]$ ping 10.3.1.12
    PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
    64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=1.27 ms
    64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=0.711 ms
    64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.761 ms
    64 bytes from 10.3.1.12: icmp_seq=4 ttl=64 time=0.701 ms
    64 bytes from 10.3.1.12: icmp_seq=5 ttl=64 time=0.559 ms
    64 bytes from 10.3.1.12: icmp_seq=6 ttl=64 time=0.571 ms
    ^C
    --- 10.3.1.12 ping statistics ---
    6 packets transmitted, 6 received, 0% packet loss, time 5069ms
    rtt min/avg/max/mdev = 0.559/0.761/1.266/0.237 ms

```
    $ ssh hugo@10.3.1.12
    hugo@10.3.1.12's password:
    Last login: Tue Oct 24 11:33:41 2023 from 10.3.1.1
    [hugo@localhost ~]$ sudo ip neigh flush all
    [hugo@localhost ~]$  sudo tcpdump -i enp0s3 -c 10 -w tp3_arp.pcapng not port 22    
    dropped privs to tcpdump
    tcpdump: listening on enp0s3, link-type EN10MB (Ethernet), snapshot length 262144 bytes
    10 packets captured
    10 packets received by filter
    0 packets dropped by kernel
    [hugo@localhost ~]$ exit
    logout
    Connection to 10.3.1.12 closed.

    chama@LAPTOP-J4TG9IM0 MINGW64 ~
    $ scp hugo@10.3.1.12:/home/hugo/tp3_arp.pcapng ./
    hugo@10.3.1.12's password:
    tp3_arp.pcapng

## II. Routage
