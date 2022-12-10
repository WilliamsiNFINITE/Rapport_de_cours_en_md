# Compte rendu du TP7 :  BUILDROOT

Ce rapport documente la manière dont Thalul-De Marcelin et moi même, Williams HOARAU, avons réalisé le TP7.

## EXPLORATION DES DIFFERENTES OPTIONS DU NOYAU TEMPS REEL

- Explorez le menu « General setup » puis « Preemption Model », à quoi correspond chacune des options ?
  -  


```console
# ./4c < 4b.txt
 Nb mesures = 5000
 Minimum = 910
 Maximum = 1079
 Moyenne = 1000
 Ecart-type = 4
 
# ./4d 5000 910 1079 < 4b.txt > histo.txt

# ./4_d_histosh.sh histo.txt "Histogramme 1"

```

![image](https://user-images.githubusercontent.com/91114817/204555311-6f1afd27-c262-4c87-af0e-2b7bba6158fe.png)

Ci-dessous, on a execute le fichier perturbateur du TP2 (sans y apporter de modification) en arrière plain directement depuis la carte avec l'option '&'.

```console
# ./4e & 
# ./4b 1000 > 4b2.txt 
# ./4c < 4b2.txt
 Nb mesures = 5000
 Minimum = 800
 Maximum = 1212
 Moyenne = 1000
 Ecart-type = 6
[1]+  Done                       ./4e

# ./4d 5000 800 1212 < 4b2.txt > histo2.txt
# ./4_d_histosh.sh histo2.txt "Histogramme 2 (avec perturbation)"

# 
```

![image](https://user-images.githubusercontent.com/91114817/204561367-e7a85dcf-8821-4b8f-b83e-c6449c97b94e.png)


Ensuite nous avons perturbé la carte en lui envoyant des ping depuis notre VM. Ci-dessous les résultats :

```console
# ./4c < 4b3.txt
 Nb mesures = 5000
 Minimum = 183
 Maximum = 1858
 Moyenne = 1000
 Ecart-type = 21

```

![image](https://user-images.githubusercontent.com/91114817/204567635-6fa23011-cc9d-41e3-950c-f5ce9804cd1b.png)

On remarque une augmentation de l'écart-type ainsi que des valeurs maximales. On peut en conclure que les ping envoyés depuis notre VM ont été sources de perturbations pour la carte.

## LINUX-RT

Lorsque l'on utilise la commande ` patch -p1 < ./patch-5.2.21-rt15.patch --dry-run`, l'option `--dry-run` permet de tester la validité du patch sans pour autant appliquer le patch. Si la commande ne renvoit pas d'erreur, alors il est possible de patcher notre version de Linux en ré-exécutant la commande précédente sans l'option --dry-run. 

Après l'application du patch on obtient les options en temps réel 

![image](https://user-images.githubusercontent.com/91114817/206841867-72d8814d-7aae-4a84-9ccc-3bfabcfbc192.png)


Dans un premier temps, nous avons utiliser le premier noyau. Nous avons écrit le script ci-dessous afin de pouvoir mesurer le temps dans une boucle infinie et l'afficher dans le terminal 

![image](https://user-images.githubusercontent.com/91114817/206849472-33ff3c40-59f9-47bf-aecf-d3b37550c79b.png)

La carte fait des mesures qui sont espacées d'environ 11000 nanosecondes (le seuil a été placé à 400) sans aucune perturbation.

https://user-images.githubusercontent.com/91114817/206849358-08a82826-5240-4466-8f77-2c8c3850dc15.mp4


A présent nous allons utiliser le noyau en temps réel. Nous recommençons dons les étapes précédentes après changement de noyau

https://user-images.githubusercontent.com/91114817/206850515-bcf8b2eb-e671-42f8-a6b8-644fc7455637.mp4

On remarque d'abord un espacement des mesures qui est plus important qu'avec le noyau précédent.

