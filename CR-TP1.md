# Compte rendu TP1

### 1. Affichage d'informations sur la pile TCP/IP locale
🌞 Affichez les infos des cartes réseau de votre PC

nom, adresse MAC et adresse IP de l'interface WiFi : 

    commmande: ip config /all
    
    Carte réseau sans fil Wi-Fi :

    Suffixe DNS propre à la connexion. . . :
    
    Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650i 160MHz Wireless Network Adapter (201NGW) == nom
    
    Adresse physique . . . . . . . . . . . : 92-D5-3B-F5-79-F5 == adresse MAC
    
    DHCP activé. . . . . . . . . . . . . . : Oui
    
    Configuration automatique activée. . . : Oui
    
    Adresse IPv6 de liaison locale. . . . .: fe80::9a24:18b8:e579:d17f%3(préféré)
    
    Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.17(préféré) == adresse ip
    
    Masque de sous-réseau. . . . . . . . . : 255.255.252.0
    
    Bail obtenu. . . . . . . . . . . . . . : mercredi 11 octobre 2023 09:01:17
    
    Bail expirant. . . . . . . . . . . . . : jeudi 12 octobre 2023 09:01:17
    
    Passerelle par défaut. . . . . . . . . : 10.33.51.254
    
    Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254
    
    IAID DHCPv6 . . . . . . . . . . . : 59954491
    
    DUID de client DHCPv6. . . . . . . . : 00-03-00-01-92-D5-3B-F5-79-F5
    
    Serveurs DNS. . .  . . . . . . . . . . : 208.67.222.220
                                       208.67.220.222
                                       10.188.0.1
    
    NetBIOS sur Tcpip. . . . . . . . . . . : Activé


nom, adresse MAC et adresse IP de l'interface Ethernet :

    commande: ipconfig /all

    Carte Ethernet Ethernet :

    Statut du média. . . . . . . . . . . . : Média déconnecté
    
    Suffixe DNS propre à la connexion. . . :
    
    Description. . . . . . . . . . . . . . : Killer E2600 Gigabit Ethernet Controller == nom
    
    Adresse physique . . . . . . . . . . . : 08-8F-C3-4A-E9-64 == adresse MAC
    
    DHCP activé. . . . . . . . . . . . . . : Oui
    
    Configuration automatique activée. . . : Oui

🌞 Affichez votre gateway

    commande: ipconfig /all
    
    gateway : 10.33.51.254

🌞 Déterminer la MAC de la passerelle
    
    commande: arp -a
    
    Interface : 10.33.48.17 --- 0x3
    Adresse Internet      Adresse physique      Type
    10.33.48.14           e4-b3-18-48-36-68     dynamique
    10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique == MAC de la passerelle
    10.33.51.255          ff-ff-ff-ff-ff-ff     statique
    224.0.0.22            01-00-5e-00-00-16     statique
    224.0.0.251           01-00-5e-00-00-fb     statique
    224.0.0.252           01-00-5e-00-00-fc     statique
    239.255.255.250       01-00-5e-7f-ff-fa     statique
    255.255.255.255       ff-ff-ff-ff-ff-ff     statique
    
    MAC de la passerelle : 7c-5a-1c-cb-fd-a4

🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)

```parametre > réseau et internet > propriétés du matériel```

### 2.  Modifications des informations

🌞 Utilisez l'interface graphique de votre OS pour changer d'adresse IP :

```parametre > réseau et internet > propriétés du matériel > modifier ipv4```

🌞 Il est possible que vous perdiez l'accès internet.

    On perd internet car le une autre machine utilise déjà cette adresse ip

