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











<pre>$ ls | cat</pre>
ls génère la liste des fichiers, puis cette sortie est envoyée à cat via le pipe. cat affiche simplement la liste à nouveau.
En résumé, cela revient à faire un simple ls, car cat ne modifie pas la sortie.


<pre>$ ls -l | cat > liste</pre>
La sortie de ls -l (les informations détaillées sur les fichiers du répertoire) est sauvegardée dans un fichier nommé liste, mais n'est pas affichée dans le terminal.


<pre>$ ls -l | tee liste</pre>
La sortie de ls -l est affichée dans le terminal et est enregistrée dans un fichier nommé liste.


<pre>$ ls -l | tee liste | wc -l</pre>
La sortie de ls -l est affichée à l'écran, enregistrée dans le fichier liste, puis le nombre de lignes de cette sortie est compté et affiché. Cela donne le nombre d'éléments (fichiers ou répertoires) dans le répertoire courant.























