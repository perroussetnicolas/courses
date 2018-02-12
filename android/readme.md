# Cours

Quand on regarde un programme il faut toujours regarder l'autre du programme

* Comment je pense qu'il marche ? 

Il faut savoir ce qu'on veut en sortie, trouver l'entrée et le milieu va tout seul 

L'ordre d'éxecution est le plus important ! 
 
=> Analyse fonctionnelle


__IP__

Qu'est-ce qu'une ip ?

C'est une adresse sur 32 bits (4 groups de 8bits)

    xxx.xxxxxx.xxx
    adress : 192.168.0.1

Et un `masque de sous réseau` :

    net mask: 255.255.255.0

Quand on prend l'adresse IP et le masque, si ce sont les même, les deux machines peuvent discuter entre elles. 

UDP est un broadcast, tout le monde reçoit les paquets UDP. 
Il sert surtout à faire du streaming par exemple.

TCP est une communication entre deux machines qui ont leur propre discussion. 

Un paquet se présente avec : 

* Destinataire 32bits
* Envoyeur 32 bits
* Données (environ Maax 240 Octets )
* ID 8bits
* TTL (Chemin le plus court) 8bits
* Vérification (Signature qui témoigne de l'intégrité des données) 8bits

La moitié du réseau internet est dû à de l'adressage de paquet plus que de poids des données.
C'est pour quoi il faut optimiser le taux d'envoi des requetes

__HTTP__

Marche avec une `commande` et une `réponse`

-> GET `http://www.eemi.com`

<- `<HTML>...</HTML>`

__Le serveur__

Le serveur est un programme. On achète une machine qui fait tourner un serveur.

De quoi a-t-il besoin ? 

* Une adresse
* Un port, soit une entrée. 

Il a pour job d egérer un protocole (tcp, dns, ssh, telnet, etc...)

Une seule connexion peut se faire à la fois sur chaque ports.
Dès qu'un client essaye de se connecter sur un port utilisé, le serveur en récréer un. 
Le maximum de ports est de 65 000, c'est les méthodes de DOS && DDOS
