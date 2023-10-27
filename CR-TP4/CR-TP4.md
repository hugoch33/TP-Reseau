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
   Bail expirant. . . . . . . . . . . . . :🌞 samedi 28 octobre 2023 09:14:49🌞
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

```

## II. Serveur DHCP