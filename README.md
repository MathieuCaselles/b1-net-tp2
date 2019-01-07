# II. Partie 2 Exploration Local en Duo

Pour désactiver le firewall aller ici « Panneau de configuration\Tous les Panneaux de configuration\Pare-feu Windows\Personnaliser les paramètres » et cliquez sur « Désactiver le Pare-feu Windows (non recommandé) » pour les 2 modes de réseaux puis sauvegarder en cliquant sur « Ok »

Pierre désactive aussi Avast on ne sait jamais.

Pierre met en ipv4 : 192.168.137.1 et moi en 192.168.137.2, nous avons désactivé nos cartes wifi. Il fais un ping dans le powershell sur mon ip, la réponse est direct avec 0 paquets perdus.

**Utilisation d&#39;un des deux comme gateway**

Sur mon pc sans internet on a mis en passerelle son ipv4 de sa carte réseau ethernet

Sur son ordinateur avec sa carte wifi allumée, il va dans le centre réseau et partage, puis dans Propriété de mon wifi@ynov puis dans &#39;Partage&#39;, il coche la case &#39;Autoriser les autres utilisateurs du réseau à se connecter via la connexion Internet de cet ordinateur&#39;

De mon côté j'ai mis en passerelle 192.168.137.1. J'effectue la commande wget google.com et nous avons la réponse. Le partage fonctionne.

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/1.png)

**Petit chat privé ?**

Je me met en mode serveur et tape donc dans le powershell après avoir cd mon dossier netcat

.\nc.exe –l –p 8888

De son côté, Pierre tape

.\nc.exe 192.168.137.2 8888

Le chat est ouvert et on peut discuter en instantané:

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/3.png)
 
De son côté :

 ![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/2.png)

Je précise que quand on a essayé avec Pierre comme serveur on avait cette erreur

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/4.png)

Son pc le détectait mais le bloquait même avec les pare feu et antivirus éteint

**Wireshark**

Voici ci-dessous les trames circulant sur le pc de Pierre quand il me ping

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/5.png)

Voici ce que j'ai a de mon côté

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/6.png)

Quand en netcat j'envoi un message, Pierre reçois ceci

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/9.png)

Quand Pierre en envoit un:

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/10.png)

**Firewall**

Vous allez dans « Panneau de configuration\Tous les Panneaux de configuration\Pare-feu Windows » puis cliquez sur Paramètres avancés. Allez dans Règles de Traffic entrant et activer la règle « Partage de fichiers et d&#39;imprimantes (Demande d&#39;écho - Trafic entrant ICMPv4) ». Faites pareil en règle de Traffic sortant.
![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/7.png)

**Netcat**

Je choisis le port 8888

Allez dans Règles de Traffic entrant puis dans «  Nouvelle Règle », dans type de règle vous mettez port, vous mettez tcp, en port local spécifique vous mettez 8888 et vous choisissez « Autorisez la connexion »Appliquez cette règle aux 3 choix. Enfin donnez un nom à votre règle. Faites de même pour la règle de Traffic sortant.

On teste le netcat et ça fonctionne.

 ![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/8.png)
#