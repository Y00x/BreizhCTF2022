## Application privé du breizh

```
Value: 131

Description:

En arrivant à Rennes au BreizhCTF, vous avez trouvé un téléphone par terre et avez décidé de le garder avec vous. Vous vous rendez compte que sur ce téléphone, il y a une appli 'SuperSecretApp'.

Malheureusement, pour accéder à son contenu, vous devez avoir la bonne combinaison du username et du password.

Vous avez donc décidé de reverse l'application pour trouvez cela!

Auteur: Worty

Format : BZHCTF{username-password}
```

Nous téléchargeons un fihcier en .apk, nous allons donc faire du reverse android.

Toute suite j'ai décidé de le décompiler avec apktool : 

```bash
apktool d breizh_private_appli.apk
```


on obtient donc tous les fichier de l'application et nous pouvons à présent travailler sur le code de l'application que l'on va ouvrir dans jadx Gui.

![Files](https://github.com/Yo-0x/BreizhCTF2022/blob/main/files.png?raw=true)

Ici nous pouvons voir un fichier LoginDataSource dont voici le code

![[Pasted image 20220403111551.png]]

D'après l'énoncé la syntax du flag est BZHCTF{username-password}.
En lisant rapidement le code nous obtenons le username **kalucheAdmin:))**

Maintenant nous devons retrouver le mot de passe qui, comme nous pouvons le voir est le resultats de plusieurs calcule sur chacune des lettres qui le compose, dans la fonction **verifiedPasswod**. 

Après observation il suffit de poser les équation et de les résoudre pour obtenir chaque lettre. 

Calcule de la première condition : 
```java
if (((char) (pwd.charAt(0) ^ "!".charAt(0))) != "@".charAt(0) || (pwd.charAt(1) ^ "m".charAt(0)) + 36 != 120 || ((char) (pwd.charAt(0) ^ pwd.charAt(2))) != "[".charAt(0) || pwd.charAt(3) != "V".charAt(0)) { 
```
 
 `pwd.charAt(0)` correspond à la première lettre du password.
 nous avons donc : ? ^ ! = @ avec "?" inconnu.
connaissant les propriété du XOR nous pouvons simplement faire **!^ @** pour obtenir la première lettre **a**.

Seconde lettre : **(? ^m) + 36 = 120** après résolution nous obtenons 
**? = 120 - 36 ^ m** et après convertion la deuxième lettre est "9".

Nous restons dans la même logique d'équation et obtenons les quatres première lettre du password **a9:V**.

Calcule de la seconde condition :
```java
if ((pwd.charAt(pwd.length() % 4) ^ pwd.charAt(4)) + 98 != "e".charAt(0) || ((char) (pwd.charAt(5) + 7)) != "T".charAt(0) || ((char) ((pwd.charAt(6) & 255) ^ 16)) != "h".charAt(0))
```

même logique ici mais avec des calcule intermédiaire comme **pwd.length() % 4)**.
Dans la fonction **login** nous pouvons voir la taille du password `if (username.length() != 15 || password.length() != 8)`  donc **8%4 = 1**

Nous obtenons l'équation suivante : **(a ^ ?) + 98 = e** ce qui nous donne **? = (e - 98) ^ a** donc la cinquième lettre est "b"

Après le calcule des 7 lettres nous obtenons **a9:VbMx** 

Calcule de la dernière condition : 
```java
if (pwd.charAt(7) == ((char) (pwd.charAt(5) ^ pwd.charAt(2))))
```
Ici il suffit de xor la 6 ème lettre avec la 3 ème lettre du mdp pour obtenir la lettre "w"

Le mdp est donc : **a9:VbMxw**

Nous pouvons maintenant construire notre flag : `BZHCTF{kalucheAdmin:))-a9:VbMxw}`


