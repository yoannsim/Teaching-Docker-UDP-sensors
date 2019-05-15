#  Laboratoire  Serveur de fichiers partie 1

Auteur : Julien Rod, Simonet Yoann 

##  description  de  l’implémentation

### Requesthandler
C' est le thread qui va traiter les requêtes en allant  lire le fichier une fois la lecture terminée il va mettre la réponse (classe Response) dans le buffer de réponses.

si le fichier est introuvable on push une réponse avec "file not found" a la place du texte.

### RequestDispatcherThread
 
C' est le thread qui vas répartir le requetés (qui se trouve dans le buffer de requête) et lancer les Requesthandler (une requête par handler).

Il va aussi delete les threads qui on finis leur traitement
à chaque nouvelle requête via la fonction garbageCollector().

###    MyBuffer
version d' un buffer dynamique (QStack) qui utilise un moniteur mesa QT pour se protéger.

On utilise une classe QStack pour stocker les données car elle a pour avantage d' avoir une complexité en O(log(n)) tout en étant dynamique.

Le wait() du moniteur est utiliser pour rendre la fonction get bloquante si la stack est vide.

### FileServer
Cette classe est utiliser pour générer le server qui va remplir le buffer de requête et envoyer les réponses

Dans se laboratoire nous avons juste du déclarer le buffer de requête le buffer de réponse et RequestDispatcherThread

Et ensuite on a lancer le RequestDispatcherThread.

## Tests effectués


Description de chaque test, et information sur le fait qu'il ait passé ou non

## Réponse aux questions

Comparez la performancce de cette version concurrente avec celle de la version de base. Constatez-vous une amélioration ?

|Number of Requests|Base|Concurrente|
|------------------|----|-----------|
|1|11s|5s|
|5|23s|17s|
|20	|60s|38s|
|100|290s|220s|


**Que se passe-t-il lorsqu'on lance un nombre de reqêtes très important (par ex. 10'000), et comment l'expliquez-vous ?**

Si nous mettons un nombre de requête très grand, le système ne parviendra pas à trouver le chemin spécifié et nous affichera donc "File not found!".

Cela peut s'expliquer par le fait que trop de requêtes font exploser la mémoire (principe du DOS).

**Comment pourrait-on protéger le serveur de cet effet néfaste ?**

On pourrait contourner le problème en limitant le nombre de requêtes maximales exécutées en même temps. 
