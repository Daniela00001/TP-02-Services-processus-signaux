# TP-02-Services-processus-signaux


1.1 Exercice : Connection ssh root (reprise fin tp-01)
______________________________________________________________________________________________________________________


<pre>PermitRootLogin yes</pre>

<pre>PasswordAuthentication yes</pre>

Il faut decommenter les elements si elles sont commentées (#).

Redémarrage du service SSH pour appliquer les modifications :

Explications des éléments de configuration modifiés :
<pre>PermitRootLogin yes </pre>
Permet les connexions SSH en tant qu’utilisateur root.
Avantages : Accès direct et complet au système pour l’administration.
Inconvénients : Augmente le risque de sécurité car le compte root est une cible privilégiée.
Utilisation : À utiliser avec précaution, généralement désactivé en production pour des raisons de sécurité.


<pre>PasswordAuthentication yes</pre>

Autorise l’authentification par mot de passe.
Avantages : Facilité d’utilisation sans nécessiter de gestion de clés SSH.
Inconvénients : Moins sécurisé que l’authentification par clé, vulnérable aux attaques par force brute.




<h1>1.2 Exercice : Authentification par clef / G´en´eration de clefs</h1>


Pour générer une clé publique et privée sur Windows, j'ai utilisé Cmder. J'ai entré la commande <pre>ssh-keygen</pre> dans le terminal et suivi les étapes en validant chacune.


<h1>1.3 Exercice : Authentification par clef / Connection serveur</h1>


Pour me connecter au serveur en utilisant ma clé publique, j'ai d'abord dû transférer la clé sur le serveur.

<h3>Étapes :</h3>
Je me suis connecté en SSH depuis ma machine physique.

Ensuite, je suis allé dans le répertoire /root du serveur.
J'ai vérifié si le dossier caché .ssh existait avec ls -a.Puis, j'ai créé le fichier authorized_keys dans ce répertoire avec la commande :<pre>nano .ssh/authorized_keys</pre>
Sur ma machine locale, j'ai affiché le contenu de ma clé publique avec :<pre>cat ~/.ssh/id_rsa.pub</pre>

J'ai copié la clé, puis je l'ai collée dans le fichier authorized_keys sur le serveur. Ensuite, j'ai sauvegardé et fermé le fichier.
Pour des raisons de sécurité, j'ai vérifié les permissions du fichier avec <pre>ls -l .ssh/</pre> puis j'ai attribué les bons droits :

<pre>chmod 744 .ssh/authorized_keys</pre>

Ensuite, je me suis déconnecté du serveur avec exit et j'ai retesté la connexion avec la commande suivante :
<pre>ssh -i ~/.ssh/id_rsa root@localhost</pre>
Maintenant, je n'ai plus besoin d'entrer de mot de passe pour me connecter.






<h1>
Exercice 3 : les tubes
</h1>
<h2>Quelle est la diff´erence entre tee et cat ?</h2>

cat est utilisé pour afficher le contenu d'un fichier ou d'un flux. Si utilisé avec une redirection (>), il écrase ou redirige totalement la sortie.
tee permet à la fois d'afficher la sortie dans le terminal et de l'enregistrer dans un fichier.

<h2>Que font les commandes suivantes :</h2>

<pre>$ ls | cat</pre>
ls génère la liste des fichiers, puis cette sortie est envoyée à cat via le pipe. cat affiche simplement la liste à nouveau.
En résumé, cela revient à faire un simple ls, car cat ne modifie pas la sortie.


<pre>$ ls -l | cat > liste</pre>
La sortie de ls -l (les informations détaillées sur les fichiers du répertoire) est sauvegardée dans un fichier nommé liste, mais n'est pas affichée dans le terminal.


<pre>$ ls -l | tee liste</pre>
La sortie de ls -l est affichée dans le terminal et est enregistrée dans un fichier nommé liste.


<pre>$ ls -l | tee liste | wc -l</pre>
La sortie de ls -l est affichée à l'écran, enregistrée dans le fichier liste, puis le nombre de lignes de cette sortie est compté et affiché. Cela donne le nombre d'éléments (fichiers ou répertoires) dans le répertoire courant.























