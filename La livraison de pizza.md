

## Livraison de Pizza
```
Value: 50

Description:

Un nouvel employé travaille tranquillement à son bureau, quand quelqu'un se présente devant pour 'Livraison de galettes saucisses'. Il est nouveau, mais il se dit qu'en Bretagne, après tout, cela doit arriver.

Il est donc venu dans votre bureau vous demandez si vous aviez commandé, mais votre réponse est non. Il revient tout paniqué en vous disant que son anvitirus a enregistré un traffic USB inhabituel.

L'anvitirus de votre entreprise est configuré pour prendre des dumps réseaux de tout le traffic, même le traffic USB ! A vous de l'analyser pour voir ce qu'il s'est passé.

Auteur: Worty

Format: BZHCTF{}
```

Nous téléchargeons la capture de trame wireshark.

Après ouverture et analyse, il s'agit d'une trame USBMS USB Mass Storage donc on suppose que certaine donnée on été exifltré à l'aide d'une clé USB.

On commence d'abord le trie par taille.

![[Pasted image 20220403114313.png]]
on regarde les trames avec la plus grande longueur et on remarque qu'il s'agit d'une image avec le nom **Confidential.png**

![[Pasted image 20220403114510.png]]

![[Pasted image 20220403114633.png]]

sur la trame la plus avec la plus grande longueur on remarque l'entête PNG on est donc sûr qu'il s'agit de l'image que nous devons récupérer. 

Après avoir essayé de la dump depuis wireshark en vain. j'ai copié les bytes "As Hex Stream"
et je les ai collé dans Cyberchef. Après convertion nous retrouvons les données de notre Image.

![[Pasted image 20220403115017.png]]

Il reste une subtilité. Pour que l'image soit valide, il faut que l'entête PNG soit valide.
Elle doit donc commencé par **8950** en hexa. ce qui n'est pas notre cas ici.

![[Pasted image 20220403115132.png]]

Nous enlevons donc tous ce qui se trouve avant l'entête PNG. 
A présent il nous reste plus qu'a enregistré l'image et à l'ouvrir pour obtenir le flag.

![[Pasted image 20220403115245.png]]