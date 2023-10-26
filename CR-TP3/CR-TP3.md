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


🌞Ajouter les routes statiques nécessaires pour que john et marcel puissent se ping

Pour john :

    [hugo@localhost ~]$  sudo ip route add 10.3.2.0/24 via 10.3.1.254 dev enp0s3

    [hugo@localhost ~]$ sudo nano /etc/sysconfig/network-scripts/route-enp0s3
    [sudo] password for hugo:
    [hugo@localhost ~]$ sudo systemctl restart NetworkManager

Pour marcel :

    [hugo@localhost ~]$ sudo nano /etc/sysconfig/network-scripts/route-enp0s3
    [sudo] password for hugo:
    [hugo@localhost ~]$ sudo systemctl restart NetworkManager


ping de vérification :

    De marcel à john :

    [hugo@localhost ~]$ ping 10.3.1.11
    PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
    64 bytes from 10.3.1.11: icmp_seq=1 ttl=63 time=1.35 ms
    64 bytes from 10.3.1.11: icmp_seq=2 ttl=63 time=1.16 ms
    64 bytes from 10.3.1.11: icmp_seq=3 ttl=63 time=1.25 ms

    De john à marcel :
    [hugo@localhost ~]$ ping 10.3.2.12
    PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
    64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=1.19 ms
    64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=1.16 ms
    64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=1.35 ms
    64 bytes from 10.3.2.12: icmp_seq=4 ttl=63 time=1.27 ms
    64 bytes from 10.3.2.12: icmp_seq=5 ttl=63 time=1.42 ms

```
    sudo ip neigh flush all

🌞Analyse des échanges ARP

| ordre | type trame  | IP source           | MAC source                   | IP destination      | MAC destination              |
| ----- | ----------- | ------------------- | ---------------------------- | ------------------- | ---------------------------- |
| 1     | Requête ARP | x                   | `john` `08:00:27:42:4a:6f`   | x                   | Broadcast `FF:FF:FF:FF:FF`   |
| 2     | Réponse ARP | x                   | `routeur` `08:00:27:e7:0e:1a`| x                   | `john` `08:00:27:42:4a:6f`   |
| 3     | Ping        | `john` `10.3.1.11`  | `john` `08:00:27:42:4a:6f`   | `marcel` `10.3.2.12`| `routeur` `08:00:27:e7:0e:1a`|
| 3     | Ping        | `john` `10.3.1.11`  | `routeur``08:00:27:fb:fe:b0` | `marcel` `10.3.2.12`| `marcel` `08:00:27:9e:58:94` |
| 4     | Requête ARP | x                   | `marcel` `08:00:27:9e:58:94` | x                   | Broadcast `FF:FF:FF:FF:FF`   |
| 5     | Réponse ARP | x                   | `routeur` `08:00:27:fb:fe:b0`| x                   | `marcel` `08:00:27:9e:58:94` |
| 6     | Pong        | `marcel` `10.3.2.12`| `marcel` `08:00:27:9e:58:94` | `john` `10.3.1.11`  | `routeur` `08:00:27:fb:fe:b0`|
| 6     | Pong        | `marcel` `10.3.2.12`| `routeur` `08:00:27:e7:0e:1a`| `john` `10.3.1.11`  | `john` `08:00:27:42:4a:6f`   |

### 3. Accès internet

🌞Donnez un accès internet à vos machines - config routeur

```
[hugo@localhost ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=21.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=114 time=19.1 ms
^C
--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
```

```
[hugo@localhost ~]$ sudo firewall-cmd --add-masquerade --permanent
success
[hugo@localhost ~]$ sudo firewall-cmd --reload
success
```

🌞Donnez un accès internet à vos machines - config clients

```
[hugo@localhost ~]$ sudo nano /etc/sysconfig/network

[hugo@localhost ~]$ sudo systemctl restart NetworkManager

[hugo@localhost ~]$ sudo nano /etc/resolv.conf
```

Vérification de l'accès à Internet (depuis marcel et john) :

```
[hugo@localhost ~]$ ping google.com
PING google.com (142.250.75.238) 56(84) bytes of data.
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=1 ttl=116 time=30.5 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=2 ttl=116 time=27.4 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=3 ttl=116 time=28.1 ms
^C
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
```

| ordre | type trame  | IP source           | MAC source                   | IP destination      | MAC destination              |
| ----- | ----------- | ------------------- | ---------------------------- | ------------------- | ---------------------------- |
| 1     | Ping        | `john` `10.3.1.11`  | `john` `08:00:27:42:4a:8f`   | `marcel` `10.3.2.12`| `routeur` `08:00:27:e7:0e:1a`|
| 2     | Ping        | `john` `10.3.1.11`  | `routeur` `08:00:26:fb:fe:b0`| `marcel` `10.3.2.12`| `marcel` `08:00:27:9e:58:94` |
| 3     | Pong        | `marcel` `10.3.2.12`| `marcel` `08:00:27:9e:58:94` | `john` `10.3.1.11`  | `routeur` `08:00:27:fb:fe:b0`|
| 4     | Pong        | `marcel` `10.3.2.12`| `routeur` `08:00:27:e7:0e:1a`| `john` `10.3.1.11`  | `john` `08:00:27:42:4a:8f`   |