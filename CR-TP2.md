# Compte-rendu TP2 : Ethernet, IP, et ARP

# I. Setup IP

🌞 Mettez en place une configuration réseau fonctionnelle entre les deux machines

1 ère IP: 10.33.172.26/30
2 ème IP: 10.33.172.25/30

adresse réseau :  255.255.255.252

adresse de broadcast :  10.33.172.27

🌞 Prouvez que la connexion est fonctionnelle entre les deux machines

    PS C:\Users\chama> ping 10.33.172.25

    Envoi d’une requête 'Ping'  10.33.172.25 avec 32 octets de données :
    Réponse de 10.33.172.25 : octets=32 temps=7 ms TTL=128
    Réponse de 10.33.172.25 : octets=32 temps=6 ms TTL=128
    Réponse de 10.33.172.25 : octets=32 temps=7 ms TTL=128
    Réponse de 10.33.172.25 : octets=32 temps=6 ms TTL=128

    Statistiques Ping pour 10.33.172.25:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 6ms, Maximum = 7ms, Moyenne = 6ms

# II. ARP my bro

🌞 Check the ARP table


  PS C:\Users\chama> arp -a

  Interface : 10.33.172.24 --- 0x5
  Adresse Internet      Adresse physique      Type
  10.33.172.25          04-7c-16-ac-f9-ca     dynamique == MAC de mon binôme
  10.33.172.27          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

  Il ne peut y avoir de gateway car seulement 2 adresses sont possibles dans un /30