## Des deux coté 1/2
```
Catégoie: Forensic
Autheur: Worty
```

### début de l'analyse
- On a un dump mémoire.
- On chercher un fichier avec des données confidentielles
- Après un imageinfo on remarque que la machine est un Windows 7

```bash
python2 vol.py -f ../../../../Downloads/memory.dmp imageinfo

INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x86_23418, Win7SP0x86, Win7SP1x86_24000, Win7SP1x86 (Instantiated with WinXPSP2x86)
                     AS Layer1 : IA32PagedMemoryPae (Kernel AS)
                     AS Layer2 : WindowsCrashDumpSpace32 (Unnamed AS)
                     AS Layer3 : FileAddressSpace (/mnt/c/Users/_Yo0x/Downloads/memory.dmp)
                      PAE type : PAE
                           DTB : 0x185000L
             KUSER_SHARED_DATA : 0xffdf0000L
           Image date and time : 2022-02-23 19:29:05 UTC+0000
     Image local date and time : 2022-02-23 11:29:05 -0800
```

On cherche ensuite le fichier secret.

```bash
python2 vol.py -f ../../../../Downloads/memory.dmp --profile=Win7SP1x86_23418 cmdline

```

On obtient alors la liste des processus et des commandes exécuté par les processus.


![Pasted image 20220404102702](https://user-images.githubusercontent.com/51168342/161565901-95df27ba-a93b-4c51-8e2d-39cea1b6e202.png)

Plusieurs informations intéréssantes ici : 

- Une application `RH-Appli-Connect.exe` est en cours d'éxécution. (Utile pour la partie 2)
- Un fichier `Employe Secret.txt` est ouvert avec notpad.exe

D'après l'énoncé nous cherchons un fichier secret. Nous allons donc nous pencher sur la deuxième info intéréssante.

pour cela on dump le process `notepad.exe` avec le PID 3732

```bash
python2 vol.py -f ../../../../Downloads/memory.dmp --profile=Win7SP1x86_23418 memdump -p 3732 -D ../../test/
```

Après de multiples `strings | grep` sur ce process impossible de retrouver l'information secrète.

Je décide donc d'aller me calmer et de retenter ma chance 10 minutes plus tard. 

J'ai repassé en revue les `strings |grep` que j'avais fais et j'ai tilté. 
J'ai fais un `strings 3732.dmp | grep =` mais pas `grep ==`

```bash
strings -a -e l 3732.dmp | grep ==
QlpIQ1RGe2ZyMzNfY3IzZDNudGk0bHN9Cg==
```

Je décode le base64

```bash
echo -n "QlpIQ1RGe2ZyMzNfY3IzZDNudGk0bHN9Cg==" | base64 -d
BZHCTF{fr33_cr3d3nti4ls}
```

Bingo !! le flag

`BZHCTF{fr33_cr3d3nti4ls}`

## Des deux coté 2/2
```
Catégoie: Forensic
Autheur: Worty

format du flag BZHCTF{malveillant.exe:ip:port}
```

Avec la première partie du challenge nous avons plus d'informations.

- Il existe un processus `RH-Appli-Connect.exe`
- Nous devons trouver un process malveillant, une ip et un port
- Nous analysons le même dump

Je ne suis pas certain mais je pense dans un premier temps que notre processus `RH-Appli-Connect.exe` n'est pas légitime.

Je vais donc vérifier les connexions réseaux pour en apprendre plus sur son comportement.

```bash
python2 vol.py -f ../../../../Downloads/memory.dmp --profile=Win7SP1x86_23418 netscan
```

Une connexion me saute au yeux `0x3e1d15e8         TCPv4    192.168.80.131:49636           146.59.156.82:1337   ESTABLISHED      3936     RH-Appli-Conne`

Le processus `RH-Appli-Connect.exe` ouvre un connexion TCP sur une adresse ip distante et un port bien connu pour les revershells tcp

Ce n'est pas dutout discret établir une connexion sur le port 1337.

ça ne fait plus aucun doute, le processus malveillant est bien `RH-Appli-Connect.exe` et il se connecte en tcp sur l'ip `146.59.156.82` et le port `1337`

Nous pouvons maintenant construire notre flag.

`BZHCTF{RH-Appli-Connect.exe-146.59.156.82:1337}`

