# TP-02-Services-processus-signaux


1.1 Exercice : Connection ssh root (reprise fin tp-01)
______________________________________________________________________________________________________________________

<h1>1.2 Exercice : Authentification par clef / G´en´eration de clefs</h1>


Pour générer une clé publique et privée sur Windows, j'ai utilisé Cmder. J'ai entré la commande <pre>ssh-keygen</pre> dans le terminal et suivi les étapes en validant chacune.
<pre>
  +--[ED25519 256]--+
|           o=+.. |
|       .   o++=  |
|      . .   o. + |
|       . .   .. .|
|        S o. .o o|
|        .o+.+ =+o|
|         = B.* *o|
|        . B O.+o.|
|         . E.===.|
+----[SHA256]-----+
</pre>

<h1>1.3 Exercice : Authentification par clef / Connection serveur</h1>


Pour me connecter au serveur en utilisant ma clé publique, j'ai d'abord dû transférer la clé sur le serveur.

<h3>Étapes :</h3>
Je me suis connecté en SSH depuis ma machine physique.

Ensuite, je suis allé dans le répertoire /root du serveur.
J'ai vérifié si le dossier caché .ssh existait avec ls -a.Il  n'existaist pas, je le crée donc <pre>mkdir .ssh</pre>
Puis, j'ai créé le fichier authorized_keys dans ce répertoire avec la commande :<pre>nano .ssh/authorized_keys</pre>
Sur ma machine locale, j'ai affiché le contenu de ma clé publique avec :<pre>cat ~/.ssh/id_rsa.pub</pre>

J'ai copié la clé, puis je l'ai collée dans le fichier authorized_keys sur le serveur. Ensuite, j'ai sauvegardé et fermé le fichier.
Pour des raisons de sécurité, j'ai vérifié les permissions du fichier avec <pre>ls -l .ssh/</pre> puis j'ai attribué les bons droits :

<pre>chmod 744 .ssh/authorized_keys</pre>




<h1>1.4 Exercice : Authentification par clef : depuis la machine hote</h1>


Ensuite, je me suis déconnecté du serveur avec exit et j'ai retesté la connexion avec la commande suivante :
<pre>ssh -i ~/.ssh/id_rsa root@localhost</pre>
Maintenant, je n'ai plus besoin d'entrer de mot de passe pour me connecter.






<h1>1.5 Exercice : S´ecurisez</h1>




Les attaques de type brute-force SSH consistent à essayer toutes les combinaisons possibles pour deviner un mot de passe, soit en utilisant un fichier "dictionnaire" avec des mots préenregistrés, soit en testant chaque combinaison de lettres, chiffres et symboles jusqu’à trouver la bonne. Par exemple, on peut commencer par tester un mot comme "A", puis essayer "Az", "A1", etc., jusqu'à tomber sur la bonne séquence.

Pour protéger l'accès SSH de ce genre d'attaques, il est important de configurer le serveur correctement. D'abord, il faut éditer le fichier de configuration SSH avec la commande :
<pre> /etc/ssh/sshd_config</pre>

Ensuite, il faut trouver la ligne suivante et la modifier pour désactiver l'accès root par mot de passe et n'autoriser que l'authentification par clé :
<pre>PermitRootLogin without-password</pre>

Cela signifie que seuls les utilisateurs ayant une clé SSH valide pourront se connecter en tant que root, évitant ainsi les attaques par force brute sur les mots de passe. 

Enfin, en plus de l’authentification par clé, d’autres mesures peuvent renforcer la sécurité, comme l’utilisation de Fail2Ban pour bloquer temporairement les adresses IP après plusieurs tentatives échouées, ou encore la modification du port SSH par défaut (qui est le 22) pour rendre l’accès moins prévisible. Chacune de ces méthodes a ses avantages : Fail2Ban protège contre les tentatives répétées, tandis que le changement de port rend les attaques plus difficiles à réaliser.


<h1>2 Processus</h1>

Pour afficher la liste de tous les processus tournant on utilise :
<pre>ps -aux</pre>

L’information TIME affichée par la commande ps représente le temps total de CPU utilisé par le processus. Cela inclut le temps durant lequel le processus a été actif sur le processeur, exprimé en minutes et secondes. Cela permet d'évaluer l'intensité d'utilisation du CPU par un processus particulier.




Tous les processus affichent une utilisation très faible du CPU ,le  processus ayant la plus forte utilisation CPU est  0.0  0.2     138 [kworker/0:4-events] .

Le premier processus lancé après le démarrage du système est systemd, qui a le PID (Process ID) 1. systemd est un gestionnaire de services et d'initialisation moderne utilisé dans de nombreuses distributions Linux. Il est responsable de l'initialisation du système, du démarrage des services, et de la gestion des processus en cours d'exécution.
On peut utiliser la commande suivante:
<pre>ps -p 1 -o pid,comm
</pre>

La commande uptime affiche depuis combien de temps le système fonctionne, ainsi que d'autres informations comme le nombre d'utilisateurs connectés et la charge du système.

<pre>uptime   -----> sortie :
 20:01:10 up 17 min,  2 users,  load average: 0.00, 0.00, 0.00
</pre>



La commande who -b affiche l'heure du dernier démarrage du système.
<pre>who -b        -----> system boot  2024-10-15 19:43
</pre>

Cette commande peut vous montrer la dernière fois que le système a démarré :

<pre> last reboot
reboot   system boot  6.1.0-25-amd64   Tue Oct 15 19:43   still running
reboot   system boot  6.1.0-25-amd64   Fri Oct 11 06:55   still running
reboot   system boot  6.1.0-25-amd64   Wed Oct  9 12:10   still running
reboot   system boot  6.1.0-25-amd64   Wed Oct  2 12:24   still running
reboot   system boot  6.1.0-25-amd64   Wed Oct  2 12:07   still running
reboot   system boot  6.1.0-25-amd64   Wed Oct  2 09:49   still running
reboot   system boot  6.1.0-25-amd64   Wed Oct  2 07:44   still running
reboot   system boot  6.1.0-25-amd64   Wed Oct  2 07:33   still running
reboot   system boot  6.1.0-25-amd64   Tue Oct  1 20:24   still running

wtmp begins Tue Oct  1 20:24:01 2024</pre>



Voici une liste des commandes Linux qu'on peut utiliser pour établir le nombre approximatif de processus créés depuis le démarrage de notre machine :
Cette commande affiche le nombre de processus créés depuis le dernier démarrage.
<pre>vmstat -f   ------->
          622 forks</pre>
Cette commande compte le nombre total de processus actifs actuellement
<pre>ps -e --no-headers | wc -l              ----------->
76</pre>

Cela peu lister tous les processus et ensuite les compter manuellement ou via un autre traitement
<pre> ps -eo pid,comm | wc -l           ------------>
77</pre>





Trouver le PPID d'un processus :

<pre>ps -eo pid,ppid,comm   --------->
    PID    PPID COMMAND
      1       0 systemd
      2       0 kthreadd
      3       2 rcu_gp
      4       2 rcu_par_gp </pre>





Liste ordonnée de tous les processus ancêtres de la commande ps

    
<pre>ps -ef --sort=ppid
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 19:43 ?        00:00:00 /sbin/init
root           2       0  0 19:43 ?        00:00:00 [kthreadd]
root         212       1  0 19:43 ?        00:00:00 /lib/systemd/systemd-journald
root         234       1  0 19:43 ?        00:00:00 /lib/systemd/systemd-udevd
systemd+     434       1  0 19:43 ?        00:00:00 /lib/systemd/systemd-timesyncd
root         455       1  0 19:43 ?        00:00:00 /usr/sbin/cron -f
message+     460       1  0 19:43 ?        00:00:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfil
root         463       1  0 19:43 ?        00:00:00 /lib/systemd/systemd-logind
root         469       1  0 19:43 ?        00:00:00 dhclient -4 -v -i -pf /run/dhclient.enp0s3.pid -lf /var/lib/dhcp/dhc
root         474       1  0 19:43 ?        00:00:00 /sbin/wpa_supplicant -u -s -O DIR=/run/wpa_supplicant GROUP=netdev
root         510       1  0 19:43 tty1     00:00:00 /bin/login -p --
root         542       1  0 19:43 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         560       1  0 19:44 ?        00:00:00 /lib/systemd/systemd --user
root           3       2  0 19:43 ?        00:00:00 [rcu_gp]
root           4       2  0 19:43 ?        00:00:00 [rcu_par_gp]
root           5       2  0 19:43 ?        00:00:00 [slub_flushwq]
root           6       2  0 19:43 ?        00:00:00 [netns]
[...]

</pre>




Reprendre la question pr´ec´edente avec la commande pstree.
Pour afficher uniquement les ancêtres de la commande ps, on utilise:
<pre>
    pstree -p $(pgrep -n ps)  --------->
systemd(1)─┬─cron(455)
           ├─dbus-daemon(460)
           ├─dhclient(469)
           ├─login(510)───bash(567)
           ├─sshd(542)───sshd(572)───bash(578)───pstree(653)
           ├─systemd(560)───(sd-pam)(561)
           ├─systemd-journal(212)
           ├─systemd-logind(463)
           ├─systemd-timesyn(434)───{systemd-timesyn}(457)
           ├─systemd-udevd(234)
           └─wpa_supplicant(474)
</pre>
Pour afficher dans top la liste de processus triée par occupation memoire ("resident memory") decroissante on peut utilise la commande :
<pre>top -o %MEM </pre> ou appuyer sur Shift + M.
Le processus systemd est le premier en termes d'utilisation de la mémoire (12152 Ko), suivi de près par sshd (11216 Ko) :

<pre> top -b -o %MEM | head -n 20   ---->
top - 20:25:50 up 42 min,  2 users,  load average: 0.00, 0.00, 0.00
Tasks:  78 total,   1 running,  77 sleeping,   0 stopped,   0 zombie
%Cpu(s):100.0 us,  0.0 sy,  0.0 ni,  0.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1967.3 total,   1789.2 free,    208.4 used,     86.8 buff/cache
MiB Swap:   6173.0 total,   6173.0 free,      0.0 used.   1758.9 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0  102112  12152   9152 S   0.0   0.6   0:00.66 systemd
    572 root      20   0   17996  11216   9392 S   0.0   0.6   0:00.94 sshd
    560 root      20   0   19012  10564   8892 S   0.0   0.5   0:00.05 systemd
    542 root      20   0   15432   8832   7588 S   0.0   0.4   0:00.00 sshd
    212 root      20   0   32964   8484   7384 S   0.0   0.4   0:00.08 systemd+
    463 root      20   0   25396   7884   6852 S   0.0   0.4   0:00.04 systemd+
    434 systemd+  20   0   90080   6576   5696 S   0.0   0.3   0:00.04 systemd+
    234 root      20   0   26180   5852   4556 S   0.0   0.3   0:00.06 systemd+
    474 root      20   0   16532   5840   4972 S   0.0   0.3   0:00.04 wpa_sup+
    671 root      20   0   11416   4956   3072 R   0.0   0.2   0:00.00 top
    578 root      20   0    7992   4896   3548 S   0.0   0.2   0:00.06 bash
    460 message+  20   0    9120   4876   4308 S   0.0   0.2   0:00.03 dbus-da+
    567 root      20   0    7996   4576   3284 S   0.0   0.2   0:00.01 bash</pre>


Pour passer l'affiche de top en rouge, il faut taper sur shift+Z, puis entrer.Pour changer la colonne de tri Shift + O(maj)


La commande htop est un outil de surveillance des processus interactif et visuel .

<h3>Avantages de htop par rapport à top:</h3>
-Les différents types de données (CPU, mémoire, processus) sont affichés sous forme de graphiques et de barres, permettant une visualisation rapide de l'utilisation des ressources.
-htop permet de filtrer et de rechercher des processus en temps réel. 
-Il est facile de changer la colonne de tri dans htop simplement en cliquant sur les en-têtes de colonne. 
-htop permet de tuer, renicer ou mettre en pause des processus directement à partir de l'interface en sélectionnant un processus et en utilisant les touches(F9 pour tuer, F7/F8 pour renicer)

<h3>Inconvénients de htop par rapport à top</h3>
-htop n'est pas toujours installé par défaut sur tous les systèmes UNIX/Linux. 
htop peut consommer un peu plus de ressources système que top en raison de son interface graphique plus complexe. 




<h1>3 Exercice 2 : Arrˆet d’un processus</h1>
On créé le fichier :<pre>touch date-toto.sh   ----->  chmod 700 date-toto.sh ------> (Change les permissions du fichier date-toto.sh pour qu'il soit exécutable uniquement par le propriétaire.)</pre>
Script date.sh:
<pre>while true; do
    sleep 1
    echo -n 'date '
    date +%T
done
</pre>
On refait les memes opérations
Script date-toto.sh
<pre>while true; do
    sleep 1
    echo -n 'toto '
    date --date '5 hours ago' +%T
done</pre>



Lister les "jobs" :

<pre>jobs
[1]+  1234 Stopped                 ./date.sh
[2]+  1235 Stopped                 ./date-toto.sh


</pre>

Remettre au premiers plans, par exemple "date.sh" :

fg %7
Pour, ensuite, kill le "job" avec CTRL+C.

<h1>
4 Exercice 3 : les tubes
</h1>
<h2>Quelle est la diff´erence entre tee et cat ?</h2>
cat est utilisé pour afficher le contenu d'un fichier ou d'un flux. Si utilisé avec une redirection (>), il écrase ou redirige totalement la sortie.
tee permet à la fois d'afficher la sortie dans le terminal et de l'enregistrer dans un fichier.

<h2>Que font les commandes suivantes :</h2>

<pre>$ ls | cat</pre>
ls génère la liste des fichiers, puis cette sortie est envoyée à cat via le pipe. cat affiche simplement la liste à nouveau.
Cela revient à faire un simple ls, car cat ne modifie pas la sortie.


<pre>$ ls -l | cat > liste</pre>
La sortie de ls -l est sauvegardée dans un fichier nommé liste, mais n'est pas affichée dans le terminal.


<pre>$ ls -l | tee liste</pre>
La sortie de ls -l est affichée dans le terminal et est enregistrée dans un fichier nommé liste.


<pre>$ ls -l | tee liste | wc -l</pre>
La sortie de ls -l est affichée à l'écran, enregistrée dans le fichier liste, puis le nombre de lignes de cette sortie est compté et affiché. Cela donne le nombre d'éléments dans le répertoire courant.



<h1>5 Journal syst`eme rsyslog</h1>



- Le service rsyslog est-il lanc´e sur votre syst`eme ? Quel est le PID du d´emon ?
  <pre>systemctl status rsyslog</pre>
Le PID du démon rsyslogd, selon la sortie de la commande systemctl status rsyslog, est 1539.


Le service rsyslog utilise le fichier de configuration principal /etc/rsyslog.conf pour déterminer où il va écrire les différents types de messages de log.Les messages standards sont écrits dans le fichier /var/log/syslog :<pre>cat /var/log/syslog</pre>
Pour voir les messages dans le fichier /var/log/messages
<pre>cat /var/log/messages</pre>


Le service cron est un programme qui permet aux utilisateurs des systèmes Unix d'exécuter automatiquement des scripts, des commandes ou des logiciels à une date et une heure spécifiée à l'avance, ou selon un cycle défini à l'avance.

La commande tail est utilisée pour afficher les dernières lignes d'un fichier.L'option -f permet de suivre le fichier en temps réel,tail -f continuera à afficher les nouvelles lignes ajoutées à ce fichier au fur et à mesure qu'elles sont écrites.


Le fichier /etc/logrotate.conf aide à automatiser la rotation, la compression et la gestion des fichiers journaux, garantissant ainsi que les fichiers journaux ne consomment pas trop d'espace disque et que le système reste organisé et performant.

Modèle de Processeur:<pre>dmesg | grep -i "cpu"</pre>
[    0.191878] smpboot: CPU0: 11th Gen Intel(R) Core(TM) i3-1115G4 @ 3.00GHz (family: 0x6, model: 0x8c, stepping: 0x1)

Modèles de Cartes Réseau :<pre>dmesg | grep -i "network"</pre>
[    0.978564] e1000: Intel(R) PRO/1000 Network Driver
[    1.489859] e1000 0000:00:03.0 eth0: Intel(R) PRO/1000 Network Connection

