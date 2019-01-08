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
