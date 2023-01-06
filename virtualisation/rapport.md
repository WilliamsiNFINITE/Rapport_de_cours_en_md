# Compte rendu du TP de virtualisation

Ce rapport documente la manière dont j'ai réalisé le TP sur la virtualisation. 

L'objectif de ce TP est d'implémenter deux fonctions d'appel système dans l'unikernel [Hermitux](https://github.com/ssrg-vt/hermitux). 
Les deux fonctions à implémenter sont truncate et ftruncate. Pour plus d'information sur le TP, veuillez consulter le [sujet](https://olivierpierre.github.io/virt-101/lab-subject.pdf)

## Sommaire

* [Partie 1](#partie-1)
* [Additionnel](#additionnel)


## Partie 1

J'ai consulté la page wiki [Syscall Based Modularity](https://github.com/ssrg-vt/hermitux/wiki/Syscall-Based-Modularity) qui m'a dirigé vers ce [fichier](https://github.com/ssrg-vt/hermitux-kernel/blob/master/include/hermit/syscall-config.h). On peut y voir une liste de syscall (ou appel système) qui peuvent être 'disable'. Pour commencer, je vais y ajouter les appels systèmes `ftruncate` et `truncate`.

```C
/* Uncomment lines in this file to exclude the implementation of the
 * corresponding syscall from the build */

//#define DISABLE_SYS_TRUNCATE
//#define DISABLE_SYS_FTRUNCATE
```

En fouillant un peu on trouve un fichier syscall.h (voir image suivante) dans lequel on trouve la déclaration d'une méthode `sys_read` qui permet d'avoir une piste de recherche. En effet, pour implémenter nos deux appels système, nous nous inspirerons de la manière dont est implémentée cet appel système. On ajoute donc notre déclaration des appels dans le fichier syscall.h et on  crée deux fichiers truncate.c et ftruncate.c 

![image](https://user-images.githubusercontent.com/91114817/211047053-f9532698-1d9f-4b0f-8ad1-f6a505c22056.png)

Check handler de système call. -trouver id de ftruncate pour les syscall.  
/!\ uhyve_send -> commande à utiliser
savoir où on utilise sys_creat dans le code pour trouver le handler. 
/!\ path : utiliser virt_to_phys pour passer l'argument path. Parce que l'adresse n'est pas la même en physique et virtuel

les paramètres --> rdi, rci, 


## Additionnel

