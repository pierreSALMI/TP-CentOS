# I - Création et utilisation simples d'une VM CentOS
Le tp a été fait jusqu'à la partie 4
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

Afficher la table de routage de l'hôte :
```
IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0      10.33.3.253      10.33.1.139     35
        10.33.0.0    255.255.252.0         On-link       10.33.1.139    291
      10.33.1.139  255.255.255.255         On-link       10.33.1.139    291
      10.33.3.255  255.255.255.255         On-link       10.33.1.139    291
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link    169.254.92.118    281
   169.254.92.118  255.255.255.255         On-link    169.254.92.118    281
  169.254.255.255  255.255.255.255         On-link    169.254.92.118    281
    192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
    192.168.127.1  255.255.255.255         On-link     192.168.127.1    281
  192.168.127.255  255.255.255.255         On-link     192.168.127.1    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     192.168.127.1    281
        224.0.0.0        240.0.0.0         On-link       10.33.1.139    291
        224.0.0.0        240.0.0.0         On-link    169.254.92.118    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     192.168.127.1    281
  255.255.255.255  255.255.255.255         On-link       10.33.1.139    291
  255.255.255.255  255.255.255.255         On-link    169.254.92.118    281
===========================================================================
Itinéraires persistants :
  Aucun
```

Afficher la table de routage de la VM :
```
[root@localhost ~]# ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```

Ce qui permet de se connecter à la Vm :
```
192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
192.168.127.1  255.255.255.255         On-link     192.168.127.1    281 (Windaube)
```

On utilise wget pour telecharger l'index de google : (il a fallu installer le package "wget")
```
[root@localhost ~]# wget google.com
--2019-01-08 16:42:20--  http://google.com/
Résolution de google.com (google.com)... 216.58.208.238, 2a00:1450:4007:810::200e
Connexion vers google.com (google.com)|216.58.208.238|:80...connecté.
requête HTTP transmise, en attente de la réponse...301 Moved Permanently
Emplacement: http://www.google.com/ [suivant]
--2019-01-08 16:42:20--  http://www.google.com/
Résolution de www.google.com (www.google.com)... 172.217.22.132, 2a00:1450:4007:810::2004
Connexion vers www.google.com (www.google.com)|172.217.22.132|:80...connecté.
requête HTTP transmise, en attente de la réponse...200 OK
Longueur: non spécifié [text/html]
Sauvegarde en : «index.html»

    [ <=>                                   ] 11 356      --.-K/s   ds 0,001s

2019-01-08 16:42:20 (17,4 MB/s) - «index.html» sauvegardé [11356]
```

On dig (il a fallu installer le package "bind-utils"):
    # Ynov.com : 217.70.184.38
    # Google.com : 216.58.208.238

# II. Notion de ports et SSH

# 1. Exploration des ports locaux

On utilise le ss pour voir les ports TCP que la machine virtuelle écoute
    # ss -4 (Pour voir que les ports IPv4)
    ``` 
    [root@localhost ~]# ss -4
    Netid  State      Recv-Q Send-Q     Local Address:Port                      Peer Address:Port
    tcp    ESTAB      0      64        192.168.127.10:ssh                      192.168.127.1:sunscalar-dns
    ```
Pour avoir les options TCP et LISTENING on rajoute (trouvé grâce à MAN)
    # ss -4 -t -l
    ```
    [root@localhost ~]# ss -4 -t -l
    State      Recv-Q Send-Q        Local Address:Port                         Peer Address:Port
    LISTEN     0      128                       *:ssh                                     *:*
    LISTEN     0      100               127.0.0.1:smtp                                    *:*
    ```


Pour voir les ports utilisés on rajoute -n et pour connaitre l'application qui écoute sur ce port -p
```
[root@localhost ~]# ss -4 -t -l -n -p
State      Recv-Q Send-Q          Local Address:Port                         Peer Address:Port
LISTEN     0      128                         *:22                                      *:*                   users:(("sshd",pid=3278,fd=3))
LISTEN     0      100                 127.0.0.1:25                                      *:*                   users:(("master",pid=3522,fd=13))
[root@localhost ~]#
```


On vois une application qui écoute sur le port 22, c'est bel et bien le SSH !

(Etant déjà connecté par SSH via Putty avant les commandes le copier coller est autorisé !)

# 2. SSH

On désactive SELinux
```
[root@localhost ~]# cd /etc/ssh/
[root@localhost ssh]# sudo setenforce 0
[root@localhost ssh]# cd /etc/selinux
[root@localhost selinux]# sudo nano config
[root@localhost selinux]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          permissive
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31
```

# 3. Firewall
Une fois ceci fait, on peut commencer sur le Firewall.

On modifie le fichier sshd_config en changeant le port d'écoute du SSH sur 2222,
on tape ensuite
`systemctl restart sshd`

Puis avec ss, on peut voir que le port d'écoute du SSH est désormais 2222.

En essayant de se connecter au SSH via le port 2222, on peut voir que ca marche pas.
Il faut donc ouvrir le port dans le firewall de notre VM

```
firewall-cmd --add-port=2222/tcp --permanent
firewall-cmd --reload
```

Une fois ceci fait, la connexion à SSH refonctionne !

Dans un premier terminal sur la VM, on autorise le port 5454 en TCP et on lance NC en écoute !
```
[root@localhost ~]# firewall-cmd --add-port=5454/tcp --permanent
success
[root@localhost ~]# firewall-cmd --reload
success
[root@localhost ~]# nc -l 5454
```


Dans un deuxième terminal, sur l'ordinateur hôte on se connecte au serveur netcat
```
C:\Users\Ju'>cd Desktop

C:\Users\Ju'\Desktop>cd NetCat

C:\Users\Ju'\Desktop\NetCat>nc 192.168.127.10 5454
aazzaz
```


Dans un troisième terminal, sur la VM on utilise SS et on observe la connexion netcat en cours :
```
tcp    ESTAB      0      0      192.168.127.10:apc-5454             192.168.127.                                         1:25643
```


# III. Routage statique