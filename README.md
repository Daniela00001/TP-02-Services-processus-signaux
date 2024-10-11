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
