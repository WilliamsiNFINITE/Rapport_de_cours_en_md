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



