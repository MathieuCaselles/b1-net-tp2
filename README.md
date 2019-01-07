# Rendu TP

## I – Exploration locale en solo
### 1. Affichage d'informations sur la pile TCP/IP locale

_Afficher infos carte réseau pc:_
Pour se faire j’écris ipconfig /all dans le terminal

_Interface wifi:_
Nom : (dit descption sur mon pc) Qualcomm Atheros QCA61x4A Wireless Network Adapter
Adresse mac : (dit adresse physique sur mon pc) 30-52-CB-17-5E-2B
IP : 10.33.3.164

_Interface ethernet :_
Nom : (dit description sur mon pc) Realtek PCIe GBE Family Controller    
Adresse mac : (dit adresse physique sur mon pc) 30-65-EC-8B-25-DC
IP : je suis pas connecté par Ethernet donc j’en ai pas

Adresse réseau wifi : l’IP est 10.33.3.164 et le masque est  255.255.252.0 dit aussi /22
Pour calculer je convertit donc en binaire ce qui donne pour l’IP : 00001010.00100001.00000011.10100100
Et pour le masque : 11111111.11111111.11111100.00000000
Pour determiner l’adresse réseau il faut donc garder tous les nombre binaire tant qu’il y a des 1 dans le masque et passer le reste a zero
On a donc pour l’ip en binaire 00001010.00100001.00000000.00000000. On reconvertit en base 10 : 10.33.0.0

Adresse broadast : il faut remplacer le dernier nombre du l’adresse ip par 255 donc cela donne 10.33.3.255

__Je ne peux pas calculer pour ethernet car je ne suis pas connecté par ethernet.__

_Afficher Gateaway (adresse passerelle) :_
Il faut faire ipconfig /all et l’adresse passerelle fait partit des information affiché grace a cette commande donc ici 10.33.3.253

_Avec l’interface graphique :_
Pour windows il faut aller dans les parametres, puis cliquer sur réseau et internet, puis centre réseau et partage puis sur wi-fi (wifi@ynov) dans le cadre de ce tp (voir image ci-dessous) ensuite il suffit d’appuyer sur détail et toutes les infos s’afficheront.

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/11.png)
![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/12.png)
 

Dans le screen, l’adresse pysique correspond a l’adresse mac, l’adresse IPv4 est l’IP et passerelle par défaut IPv4 est la gateway


_Question : a quoi sert la gateway dans le réseau d’Ingésup ?_
Réponse : Elle sert à donner l’accès a internet aux réseaux d’ingesup.

### 2 modification d’information :
##### A-	Modification d’adresse IP – pt. 1

Calcul de la premiere et derniere IP du réseau : l’adresse réseau est 10.33.0.0/22 et donc la deniere ip disponible est 10.33.3.255.

Changer d’adresse ip par interface graphique windows : c’est le même chemin que pour voir les détails sauf qu’a la fin au lieu de cliquer sur détail il faut cliquer sur propriété puis sur protocole internet version 4 puis sur propriété


J’ai donc changé d’adresse ip :  


![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/13.png)



NMAP

Il faut rentrer la commande nmap -sn -PE 10.33.0.0/22 et il se passe ceci :  


![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/14.png)

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/15.png)
 
Je change ensuite mon adresse ip grâce a NMAP et ma gateway mais j'avais déja en effet une adresse ip.

## II. Partie 2 Exploration Local en Duo

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


## III Manipulation d’autres outil protocole côté client
### 1 DHCP
Je fais un ipconfig /all et trouve l’adresse ip ip ici :

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/16.png)

et la date d'expiration ici:  
![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/17.png)

 

Ça expire au bout d’une heure.

_Dhcp dans les grandes ligne :_

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/18.png)
 
Pour demander une nouvelle adresse réseau en ligne de commande je tape d’abord ipconfig /release 
On peut voir qu’il n’y a plus bail obtenu et bail expirant (à partir de ce screen le tp a été fait chez moi donc pas depuis la box d’ynov)

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/19.png)
 
Puis je tape maintenant ipconfig /renew
Et après quelques instant je retrouve bien bail obtenu et expirant avec l’adresse du serveur dhcp :

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/20.png)


  

### 2 DNS
Je fait un ipconfig /all et trouve le serveur dns ici :

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/21.png)
 

_Utiliser nslookup_
 
 ![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/22.png)

Les résultats de cette commande montrent que l’IP  de google se trouve a 216.58.198 (l’adresse au dessus est peut être l’adresse ipv6 ?) et l’adresse d’ynov se trouve à 192.168.0.254. 

_Utiliser reverse lookup :_

![alt text](https://github.com/MathieuCaselles/b1-net-tp2/blob/master/screen/23.png)
 
Je suppose donc qu’aucun nom de domaine n’est associé à ces IP
