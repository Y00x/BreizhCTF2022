## My Homework ... No
```
Description:
Je dois rendre mon tp ce soir mais j'ai supprimé le dossier où se trouvaient mes bianires. Par chance il tourne encore, pouvez-vous m'aider à le récupérer ?

Login/Password : gaston:gaston

ssh challenges.ctf.bzh:24001

Auteur: LaChenilleBarbue

Format : BZHCTF{sha512sum(binaire)}
```

Premièrement on regarde les process en cours pour obtenir le PID.

![[Pasted image 20220404101047.png]]

on peux ensuite récupérer l'execurable avec cette commande: 

```bash
cat /proc/13/exe > /home/gaston/newhellobreizh.bin
```

On obtient alors notre binaire.
Il reste plus qu'a construire notre flag

```bash
sha512sum /home/gaston/newhellobreizh.bin
```

`BZHCTF{932abb57c2d1ccad065288579662af5216ed8a8aa4b8aa714d13feb6cb89570eed18bef0bcb7fda33e1b3bee9534c231a5ce349c01399687fe9495cf047db5ae`