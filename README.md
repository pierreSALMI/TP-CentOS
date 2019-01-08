# 4. Configuration réseau d'une machine CentOS
* A : On utilise une commande sur la VM pour prouver que l'on a internet :
```[root@localhost ~]# curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```


* B : Prouver que l'hôte et la VM communiquent se font via un ping.
    * Adresse IP de la VM : 192.168.127.10
    * Adresse IP de l'hôte : 192.168.127.1

Sur l'hôte :
```
C:\Users\Ju'>ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```

Et sur la VM :
```
[root@localhost ~]# ping 192.168.127.1
PING 192.168.127.1 (192.168.127.1) 56(84) bytes of data.
64 bytes from 192.168.127.1: icmp_seq=1 ttl=64 time=0.218 ms
64 bytes from 192.168.127.1: icmp_seq=2 ttl=64 time=0.281 ms
64 bytes from 192.168.127.1: icmp_seq=3 ttl=64 time=0.278 ms
64 bytes from 192.168.127.1: icmp_seq=4 ttl=64 time=0.275 ms
^C
--- 192.168.127.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3311ms
rtt min/avg/max/mdev = 0.218/0.263/0.281/0.026 ms
```

Sur la VM, on affiche nôtre table de routage :
```
[root@localhost ~]# ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```

Sur la première et deuxième ligne, connexion au réseau avec une ip "publique" via 10.0.2.2 et la carte enp0s3 grâce a notre OS principal (ici Windaube)


Sur la troisième, c'est l'IP "privée" pour se connecter au réseau local via 192.168.127.10 et la carte enp0s8


# 5. Faire joujou avec les commandes