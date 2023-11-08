# TP4 : DHCP

## I. DHCP Client

🌞 Déterminer 

- l'adresse du serveur DHCP

- l'heure exacte à laquelle vous avez obtenu votre bail DHCP

l'heure exacte à laquelle il va expirer

```

    PS C:\Users\chama> ipconfig /all

    Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650i 160MHz Wireless Network Adapter (201NGW)
   Adresse physique . . . . . . . . . . . : 92-D5-3B-F5-79-F5
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::9a24:18b8:e579:d17f%3(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.70.196(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Bail obtenu. . . . . . . . . . . . . . : 🌞vendredi 27 octobre 2023 09:14:55🌞
   Bail expirant. . . . . . . . . . . . . : 🌞 samedi 28 octobre 2023 09:14:49🌞
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 🌞10.33.79.254🌞
   IAID DHCPv6 . . . . . . . . . . . : 59954491
   DUID de client DHCPv6. . . . . . . . : 00-03-00-01-92-D5-3B-F5-79-F5
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

🌞 Capturer un échange DHCP


🌞 Analyser la capture Wireshark

parmi ces 4 trames, laquelle contient les informations proposées au client ?

    c'est la trame "offer" qui contient les informations proposés au client


## II. Serveur DHCP

🌞 Preuve de mise en place

```
[hugo@localhost ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=113 time=21.5 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=113 time=19.3 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=113 time=20.5 ms
```

```
[hugo@localhost ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=113 time=18.7 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=113 time=25.7 ms
```

```
[hugo@localhost ~]$ traceroute google.com
traceroute to google.com (172.217.18.206), 30 hops max, 60 byte packets
 1  _gateway (10.4.1.254)  11.564 ms  10.383 ms  9.973 ms
 2  10.0.3.2 (10.0.3.2)  8.918 ms  8.322 ms  8.035 ms
 3  10.0.3.2 (10.0.3.2)  7.320 ms  6.707 ms  4.674 ms

```

### 4. Serveur DHCP

🌞 Rendu

```
    [hugo@localhost ~]$sudo nano /etc/resolv.conf
    [hugo@localhost ~]$ping ynov.com
    [hugo@localhost ~]$sudo dnf -y install dhcp-server
    [hugo@localhost ~]$sudo nano /etc/dhcp/dhcpd.conf
```

```
[hugo@localhost ~]$ systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset: disabled)
     Active: active (running) since Sun 2023-11-08 18::28 CET; 9min 51s ago
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 1501 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 4672)
     Memory: 4.6M
        CPU: 29ms
     CGroup: /system.slice/dhcpd.service
             └─1501 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid
```

```
    [hugo@localhost ~]$ sudo cat /etc/dhcp/dhcpd.conf
    option domain-name     "srv.world";
    option domain-name-servers     dlp.srv.world;
    default-lease-time 600;
    max-lease-time 7200;
    authoritative;
    subnet 10.4.1.0 netmask 255.255.255.0 {
        range dynamic-bootp 10.4.1.137 10.4.1.147;
        option broadcast-address 10.4.1.255;
        option routers 10.4.1.254;
    }
```

### 5.Client DHCP

🌞 Test !
```
[hugo@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:c8:fa:83 brd ff:ff:ff:ff:ff:ff
    inet 10.4.1.137/24 brd 10.4.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 408sec preferred_lft 408sec
    inet6 fe80::a00:27ff:fec8:fa82/64 scope link
       valid_lft forever preferred_lft forever
```

[hugo@localhost ~]$ nmcli con show enp0s3
connection.timestamp:                   1598415778
```
> Début du bail : 07/11/2023 18:27:04

DHCP4.OPTION[7]:                        expiry = 1698375484

> Fin du bail : 07/11/2023 23:44:03

DHCP4.OPTION[4]:                        dhcp_server_identifier = 10.4.1.253
```
Ping node 1 à node 2

    [hugo@localhost ~]$ ping 10.4.1.12
    PING 10.4.1.12 (10.4.1.12) 56(84) bytes of data.
    64 bytes from 10.4.1.12: icmp_seq=1 ttl=64 time=1.81 ms
    64 bytes from 10.4.1.12: icmp_seq=2 ttl=64 time=1.27 ms
    64 bytes from 10.4.1.12: icmp_seq=3 ttl=64 time=1.52 ms

`````
>sudo dhclient -r enp0s3  
>sudo dhclient enp0s3

🌞 Bail DHCP serveur    

```
    [hugo@localhost ~]$ cat /var/lib/dhcpd/dhcpd.leases
    # The format of this file is documented in the dhcpd.leases(5) manual page.
    # This lease file was written by isc-dhcp-4.4.2b1

    # authoring-byte-order entry is generated, DO NOT DELETE
    authoring-byte-order little-endian;

    lease 10.4.1.137 {
    starts 0 2023/10/29 10:55:57;
    ends 0 2023/10/29 16:55:57;
    tstp 0 2023/10/29 16:55:57;
    cltt 0 2023/10/29 10:55:57;
    binding state free;
    hardware ethernet 08:00:27:c8:fa:83;
    uid "\001\010\000'\310\372\202";
    }
    server-duid "\000\001\000\001,\320\365\355\010\000'O\035/";

    lease 10.4.1.137 {
    starts 2 2023/11/07 17:26:25;
    ends 2 2023/11/07 23:26:25;
    cltt 2 2023/11/07 16:44:44;
    binding state active;
    next binding state free;
    rewind binding state free;
    hardware ethernet 08:00:27:c8:fa:83;
    uid "\001\010\000'\310\372\202";
    }

```
🌞 Nouvelle conf !

    [hugo@localhost ~]$ sudo cat /etc/dhcp/dhcpd.conf
    option domain-name     "srv.world";
    option domain-name-servers     8.8.8.8;
    default-lease-time 21600;
    max-lease-time 22000;
    authoritative;
    subnet 10.4.1.0 netmask 255.255.255.0 {
        range dynamic-bootp 10.4.1.137 10.4.1.147;
        option broadcast-address 10.4.1.255;
        option routers 10.4.1.254;
    }
```
Adresse DNS :

    [hugo@localhost /]$ cat etc/resolv.conf
    ; generated by /usr/sbin/dhclient-script
    search srv.world
    nameserver 8.8.8.8

```
Route par défaut :

    [hugo@localhost /]$ ip r s
    default via 10.4.1.254 dev enp0s3
    default via 10.4.1.254 dev enp0s3 proto dhcp src 10.4.1.137 metric 100
    10.4.1.0/24 dev enp0s3 proto kernel scope link src 10.4.1.137 metric 100


Bail d'une durée de 6h :
```

    lease 10.4.1.138 {
    starts 2 2023/11/07 18:26:25;
    ends 2 2023/11/07 00:26:25;
    cltt 2 2023/11/07 18:26:25;
    binding state active;
    next binding state free;
    rewind binding state free;
    hardware ethernet 08:00:27:c8:fa:83;
    }

```