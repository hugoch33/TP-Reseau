# TP5 : TCP, UDP et services réseau: 

# I.First steps
## 1. SSH

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP


### tp5_service_1
-Youtube visionnage de vidéo : 
UDP
ip : 173.194.3.39
port du serveur : 443
port local : 55452

#### tp5_service_2
-Discord call :
UDP
ip : 66.22.196.27
port du serveur : 50003
port local : 57429 

#### tp5_service_3
-Fortnite :
UDP
ip : 35.242.168.76
port du serveur : 62805
port local : 9033

#### tp5_service_4
-gourvernement.fr :
TCP
ip : 184.73.4.1
port du serveur : 443
port local : 56145

### tp5_service_5
-projet-voltaire
TCP
ip : 172.217.20.206
port du serveur: 443
port local : 56921

## 2. Routage

🌞 Examinez le trafic dans Wireshark

voir tp5_3_way.pcapng

🌞 Demandez aux OS

### Depuis la VM
```tcp    ESTAB   0        0                             10.5.1.11:ssh           10.5.1.1:60319```

### Depuis le PC
```TCP    10.5.1.1:60332         10.5.1.11:ssh          ESTABLISHED```

🦈 tp5_3_way.pcapng


🌞 Prouvez que

node1.tp5.b1 a un accès internet

    [hugo@localhost ~]$ ping google.com
    PING google.com (172.217.18.206) 56(84) bytes of data.
    64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=1 ttl=115 time=17.7 ms
    64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=2 ttl=115 time=18.6 ms
    64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=3 ttl=115 time=18.0 ms
    64 bytes from ham02s14-in-f206.1e100.net (172.217.18.206): icmp_seq=4 ttl=115 time=17.3 ms
    64 bytes from par10s38-in-f14.1e100.net (172.217.18.206): icmp_seq=5 ttl=115 time=17.6 ms

```

node1.tp5.b1 peut résoudre des noms de domaine publics (comme www.ynov.com)


    ping ynov.com
    PING ynov.com (172.67.74.226) 56(84) bytes of data.
    64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=1 ttl=55 time=20.0 ms
    64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=2 ttl=55 time=26.8 ms
    64 bytes from 172.67.74.226 (172.67.74.226): icmp_seq=3 ttl=55 time=20.9 ms

```

## 3. Serveur Web

🌞 Installez le paquet nginx

    [hugo@localhost ~]$ sudo dnf install nginx
    [sudo] password for hugo:
    Rocky Linux 9 - BaseOS                          405  B/s | 4.1 kB     00:10
    Rocky Linux 9 - BaseOS                          122 kB/s | 1.9 MB     00:15
    Rocky Linux 9 - AppStream                       448  B/s | 4.5 kB     00:10
    Rocky Linux 9 - AppStream                       452 kB/s | 7.1 MB     00:16
    Rocky Linux 9 - Extras                          290  B/s | 2.9 kB     00:10
    Rocky Linux 9 - Extras                          730  B/s |  11 kB     00:15
    Dependencies resolved.

```

🌞 Créer le site web

    [hugo@localhost var]$ sudo mkdir www
    [sudo] password for hugo:
    [hugo@localhost var]$ cd /var/www/
    [hugo@localhost www]$ sudo mkdir site_web_nul
    [hugo@localhost www]$ sudo touch index.html
    [hugo@localhost www]$ sudo nano index.html
    [hugo@localhost www]$ sudo rm index.html
    [hugo@localhost www]$ cd site_web_nul/
    [hugo@localhost site_web_nul]$ sudo nano index.html

Installed:
    nginx-1:1.20.1-14.el9_2.1.x86_64
    nginx-core-1:1.20.1-14.el9_2.1.x86_64
    nginx-filesystem-1:1.20.1-14.el9_2.1.noarch
    rocky-logos-httpd-90.14-1.el9.noarch

    Complete!

```
    [hugo@localhost var]$ mkdir www
    mkdir: cannot create directory ‘www’: Permission denied
    [hugo@localhost var]$ sudo mkdir www
    [sudo] password for hugo:
    [hugo@localhost var]$ cd /var/www/
    [hugo@localhost www]$ sudo mkdir site_web_nul
    [hugo@localhost www]$ cd site_web_nul/
    [hugo@localhost site_web_nul]$ sudo nano index.html
    [hugo@localhost site_web_nul]$ sudo cat index.html
    <h1>MEOW</h1>

🌞 Donner les bonnes permissions

   [hugo@localhost var] $ sudo chown -R nginx:nginx /var/www/site_web_nul


🌞 Créer un fichier de configuration NGINX pour notre site web

    [hugo@localhost nginx]$ cd conf.d/
    [hugo@localhost conf.d]$ sudo nano site_web_nul.conf

```

🌞 Démarrer le serveur web !

        [hugo@localhost conf.d]$ sudo systemctl start nginx
        [hugo@localhost conf.d]$ systemctl status nginx
        ● nginx.service - The nginx HTTP and reverse proxy server
        Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: dis>
        Active: active (running) since Fri 2023-11-10 12:07:00 CET; 15s ago
        Process: 1553 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status>
        Process: 1554 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
        Process: 1555 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
        Main PID: 1556 (nginx)
        Tasks: 2 (limit: 4673)
        Memory: 2.0M
            CPU: 21ms
        CGroup: /system.slice/nginx.service
                ├─1556 "nginx: master process /usr/sbin/nginx"
                └─1557 "nginx: worker process"

    Nov 10 12:07:00 localhost.localdomain systemd[1]: Starting The nginx HTTP and rev>
    Nov 10 12:07:00 localhost.localdomain nginx[1554]: nginx: the configuration file >
    Nov 10 12:07:00 localhost.localdomain nginx[1554]: nginx: configuration file /etc>
    Nov 10 12:07:00 localhost.localdomain systemd[1]: Started The nginx HTTP and reve>
    lines 1-18/18 (END)
    [hugo@localhost conf.d]$

```

🌞 Ouvrir le port firewall

    [hugo@localhost ~]$ sudo firewall-cmd --add-port=80/tcp --permanent
    success
    [hugo@localhost ~]$ sudo firewall-cmd --reload
    success

```

🌞 Visitez le serveur web !

    [hugo@localhost ~]$ curl http://10.5.1.12
    <h1>MEOW</h1>

```

🌞 Visualiser le port en écoute

    [hugo@localhost ~]$ ss -atnl
    State    Recv-Q   Send-Q     Local Address:Port     Peer Address:Port  Process
    LISTEN   0        511              0.0.0.0:80            0.0.0.0:*
    LISTEN   0        128              0.0.0.0:22            0.0.0.0:*
    LISTEN   0        511                 [::]:80               [::]:*
    LISTEN   0        128                 [::]:22               [::]:*

```
🌞 Analyse trafic

    [hugo@localhost ~]$ sudo tcpdump -i enp0s3 -w tp5_web.pcapng not port 22
    [sudo] password for hugo:
    dropped privs to tcpdump
    tcpdump: listening on enp0s3, link-type EN10MB (Ethernet), snapshot length 262144 bytes
    ^C13 packets captured
    13 packets received by filter
    0 packets dropped by kernel
    [hugo@localhost ~]$ exit
    logout
    Connection to 10.5.1.12 closed.
    chama@LAPTOP-J4TG9IM0 MINGW64 ~
    $ scp hugo@10.5.1.12:/home/hugo/tp5_web.pcapng ./
    hugo@10.5.1.12's password:
    tp5_web.pcapng                                100% 2462   397.1KB/s   00:00
