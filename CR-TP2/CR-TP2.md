# Compte-rendu TP2 : Ethernet, IP, et ARP

# I. Setup IP

🌞 Mettez en place une configuration réseau fonctionnelle entre les deux machines

1 ère IP: 10.33.172.253
2 ème IP: 10.33.172.254

adresse réseau :  255.255.255.252

adresse de broadcast :  10.33.172.27

🌞 Prouvez que la connexion est fonctionnelle entre les deux machines

    PS C:\Users\chama> ping 10.33.172.254

    Envoi d’une requête 'Ping'  10.33.172.254 avec 32 octets de données :
        Réponse de 10.33.172.254 : octets=32 temps<1ms TTL=128
        Réponse de 10.33.172.254 : octets=32 temps<1ms TTL=128
        Réponse de 10.33.172.254 : octets=32 temps<1ms TTL=128
        Réponse de 10.33.172.254 : octets=32 temps<1ms TTL=128

    Statistiques Ping pour 10.33.172.253:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 6ms, Maximum = 7ms, Moyenne = 6ms

# II. ARP my bro

🌞 Check the ARP table


        PS C:\Users\chama> arp -a

        Interface : 10.33.172.253 --- 0xd
        Adresse Internet      Adresse physique      Type
        10.33.172.254        🌟 b4-2e-99-c1-96-6e 🌟    dynamique
        10.33.172.255         ff-ff-ff-ff-ff-ff     statique
        224.0.0.22            01-00-5e-00-00-16     statique
        224.0.0.251           01-00-5e-00-00-fb     statique
        224.0.0.252           01-00-5e-00-00-fc     statique
        224.0.0.253           01-00-5e-00-00-fd     statique
        239.255.255.250       01-00-5e-7f-ff-fa     statique
        255.255.255.255       ff-ff-ff-ff-ff-ff     statique

        Interface : 192.168.1.168 --- 0x14
        Adresse Internet      Adresse physique      Type
        192.168.1.1           20-54-fa-24-ef-26     dynamique
        192.168.1.255         ff-ff-ff-ff-ff-ff     statique
        224.0.0.22            01-00-5e-00-00-16     statique
        224.0.0.251           01-00-5e-00-00-fb     statique
        224.0.0.252           01-00-5e-00-00-fc     statique
        224.0.0.253           01-00-5e-00-00-fd     statique
        239.255.255.250       01-00-5e-7f-ff-fa     statique
        255.255.255.255       ff-ff-ff-ff-ff-ff     statique

🌞 Wireshark it (TP2ICP)


🌞 Manipuler la table ARP

        PS C:\Users\chama> netsh interface IP delete arpcache
        Ok.```

        ```console
        PS C:\WINDOWS\system32> arp -a

        Interface : 10.33.172.253 --- 0xd
        Adresse Internet      Adresse physique      Type
        10.33.172.255         ff-ff-ff-ff-ff-ff     statique
        224.0.0.22            01-00-5e-00-00-16     statique
        255.255.255.255       ff-ff-ff-ff-ff-ff     statique```

        ```console
        PS C:\WINDOWS\system32> arp -a

        Interface : 10.33.172.253 --- 0xd
        Adresse Internet      Adresse physique      Type
        10.33.172.254         b4-2e-99-c1-96-6e     dynamique
        10.33.172.255         ff-ff-ff-ff-ff-ff     statique
        224.0.0.22            01-00-5e-00-00-16     statique
        224.0.0.251           01-00-5e-00-00-fb     statique
        224.0.0.253           01-00-5e-00-00-fd     statique
        255.255.255.255       ff-ff-ff-ff-ff-ff     statique

        🌞 Wireshark it (TP2ART)

