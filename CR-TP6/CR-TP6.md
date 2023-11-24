
# I. Setup routeur
# II. Serveur DNS
## 1. Présentation
## 2. Setup

🌞 Dans le rendu, je veux


une commande ss qui prouve que le service écoute bien sur un port


-un cat des 3 fichiers de conf (fichier principal + deux fichiers de zone)

[hugo@dns ~]$ sudo cat /etc/named.conf
   //
   // named.conf
   //
   // Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
   // server as a caching only nameserver (as a localhost DNS resolver only).
   //
   // See /usr/share/doc/bind*/sample/ for example named configuration files.
   //

   options {
         listen-on port 53 { 127.0.0.1; any; };
         listen-on-v6 port 53 { ::1; };
         directory       "/var/named";
         dump-file       "/var/named/data/cache_dump.db";
         statistics-file "/var/named/data/named_stats.txt";
         memstatistics-file "/var/named/data/named_mem_stats.txt";
         secroots-file   "/var/named/data/named.secroots";
         recursing-file  "/var/named/data/named.recursing";
         allow-query     { localhost; any; };
         allow-query-cache { localhost; any; };

         /*
            - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
            - If you are building a RECURSIVE (caching) DNS server, you need to enable
            recursion.
            - If your recursive DNS server has a public IP address, you MUST enable access
            control to limit queries to your legitimate users. Failing to do so will
            cause your server to become part of large scale DNS amplification
            attacks. Implementing BCP38 within your network would greatly
            reduce such attack surface
         */
         recursion yes;

         dnssec-validation yes;

         managed-keys-directory "/var/named/dynamic";
         geoip-directory "/usr/share/GeoIP";

         pid-file "/run/named/named.pid";
         session-keyfile "/run/named/session.key";

         /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
         include "/etc/crypto-policies/back-ends/bind.config";
   };

   logging {
         channel default_debug {
                  file "data/named.run";
                  severity dynamic;
         };
   };

   zone "tp6.b1" IN {
         type master;
         file "tp6.b1.db";
         allow-update { none; };
         allow-query {any; };
   };

   # référence vers notre fichier de zone inverse
   zone "1.4.10.in-addr.arpa" IN {
      type master;
      file "tp6.b1.rev";
      allow-update { none; };
      allow-query { any; };
   };


   include "/etc/named.rfc1912.zones";
   include "/etc/named.root.key";


[hugo@dns ~]$ sudo cat /var/named/tp6.b1.db
   $TTL 86400
   @ IN SOA dns.tp6.b1. admin.tp6.b1. (
      2019061800 ;Serial
      3600 ;Refresh
      1800 ;Retry
      604800 ;Expire
      86400 ;Minimum TTL
   )

   ; Infos sur le serveur DNS lui même (NS = NameServer)
   @ IN NS dns.tp6.b1.

   ; Enregistrements DNS pour faire correspondre des noms à des IPs
   dns       IN A 10.6.1.101
   john      IN A 10.6.1.11

[hugo@dns ~]$ sudo cat /var/named/tp6.b1.rev
   $TTL 86400
   @ IN SOA dns.tp6.b1. admin.tp6.b1. (
      2019061800 ;Serial
      3600 ;Refresh
      1800 ;Retry
      604800 ;Expire
      86400 ;Minimum TTL
   )

   ; Infos sur le serveur DNS lui même (NS = NameServer)
   @ IN NS dns.tp6.b1.

   ; Reverse lookup
   101 IN PTR dns.tp6.b1.
   11 IN PTR john.tp6.b1.


[hugo@dns ~]$ sudo cat /var/named/tp6.b1.rev
   $TTL 86400
   @ IN SOA dns.tp6.b1. admin.tp6.b1. (
      2019061800 ;Serial
      3600 ;Refresh
      1800 ;Retry
      604800 ;Expire
      86400 ;Minimum TTL
   )

   ; Infos sur le serveur DNS lui même (NS = NameServer)
   @ IN NS dns.tp6.b1.


   ; Reverse lookup
   101 IN PTR dns.tp6.b1.
   11 IN PTR john.tp6.b1.



   ; Infos sur le serveur DNS lui même (NS = NameServer)
   @ IN NS dns.tp6.b1.

   ; Enregistrements DNS pour faire correspondre des noms à des IPs
   dns       IN A 10.6.1.101
   john      IN A 10.6.1.11

[hugo@dns ~]$ sudo systemctl status named
   ● named.service - Berkeley Internet Name Domain (DNS)
      Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; preset: disabled)
      Active: active (running) since Fri 2023-11-17 09:10:45 CET; 1h 19min ago
      Main PID: 696 (named)
         Tasks: 5 (limit: 4605)
      Memory: 21.7M
         CPU: 243ms
      CGroup: /system.slice/named.service
               └─696 /usr/sbin/named -u named -c /etc/named.conf

   Nov 17 10:10:45 dns named[696]: network unreachable resolving './DNSKEY/IN': 2001:dc3::35#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './NS/IN': 2001:dc3::35#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './DNSKEY/IN': 2001:500:2f::f#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './NS/IN': 2001:500:2f::f#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './DNSKEY/IN': 2001:503:c27::2:30#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './NS/IN': 2001:503:c27::2:30#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './DNSKEY/IN': 2001:7fe::53#53
   Nov 17 10:10:45 dns named[696]: network unreachable resolving './DNSKEY/IN': 2001:500:2f::f#53
   Nov 17 10:10:45 dns named[696]: resolver priming query complete
   Nov 17 10:10:46 dns named[696]: managed-keys-zone: Key 20326 for zone . is now trusted (acceptance timer >

-une commande ss qui prouve que le service écoute bien sur un port

[hugo@dns ~]$ ss -atnl


🌞 Ouvrez le bon port dans le firewall

[hugo@dns ~]$ sudo firewall-cmd --add-port=53/udp --permanent
success
[hugo@dns ~]$ sudo firewall-cmd --reload
success


### 3. Test

🌞 Sur la machine john.tp6.b1

[hugo@john ~]$ sudo cat /etc/resolv.conf
   [sudo] password for hugo:


   # Generated by NetworkManager
   nameserver 10.6.1.101


[hugo@john ~]$ dig ynov.com

   ; <<>> DiG 9.16.23-RH <<>> www.ynov.com
   ;; global options: +cmd
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 38522
   ;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

   ;; OPT PSEUDOSECTION:
   ; EDNS: version: 0, flags:; udp: 1232
   ; COOKIE: c9f24617581d6ba20100000065574aee1f8beb4eb64a17ad (good)
   ;; QUESTION SECTION:
   ;ynov.com.                      IN      A

   ;; ANSWER SECTION:
   ynov.com.               288     IN      A       104.26.10.233
   ynov.com.               288     IN      A       104.26.11.233
   ynov.com.               288     IN      A       172.67.74.226

   ;; Query time: 4 msec
   ;; SERVER: 10.6.1.101#53(10.6.1.101)
   ;; WHEN: Fri Nov 17 12:13:47 CET 2023
   ;; MSG SIZE  rcvd: 113
```

[hugo@john ~]$ dig john.tp6.b1

   ; <<>> DiG 9.16.23-RH <<>> john.tp6.b1
   ;; global options: +cmd
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 52186
   ;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

   ;; OPT PSEUDOSECTION:
   ; EDNS: version: 0, flags:; udp: 1232
   ; COOKIE: f4fe935bb24edb650100000065574b93746d2a5a7c890441 (good)
   ;; QUESTION SECTION:
   ;john.tp6.b1.                   IN      A

   ;; ANSWER SECTION:
   john.tp6.b1.            86400   IN      A       10.6.1.11

   ;; Query time: 1 msec
   ;; SERVER: 10.6.1.101#53(10.6.1.101)
   ;; WHEN: Fri Nov 17 12:16:31 CET 2023
   ;; MSG SIZE  rcvd: 84


🌞 Sur votre PC

PS C:\Users\chama> nslookup john.tp6.b1 10.6.1.101
   Serveur :   UnKnown
   Address:  10.6.1.101

   Nom :    john.tp6.b1
   Address:  10.6.1.11