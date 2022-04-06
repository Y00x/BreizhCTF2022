##Baby

```
Value: 50

Description:

Le reverse c'est quand même vachement compliqué... ou pas ?

Auteur: Worty

Format : BZHCTF{}
```

Nous pouvons faire un `file baby` pour remarquer que le binaire est un ELF 64-bit.

Pour les challenge de reverse easy nous pouvons faire un strings dessus pour voir si il nous retourne des informations intéréssantes et pourquoi pas le flag. 

```bash
strings baby | grep -i bzh
```

Bingo !! 
Nous validons notre premier flag du BreizhCTF 2022

`BZHCTF{b4by_r3_f0r_y0u_g00d_luck!!}``
