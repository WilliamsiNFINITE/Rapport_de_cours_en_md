# Projet de Vérification
Ce rapport documente la réalisation du projet de Vérification du binôme constitué de Stéphane Perrin et de Williams Hoarau
Le but est d'essayer de modeliser un modèle de régulateur de vitesse avec UPPAAL. La modélisation est aussi importante que l'implémentation des exigences.
Ces exigences se trouvent dans le chapitre [5.4 Software Functions](https://moodle.ensta-bretagne.fr/pluginfile.php/88221/mod_resource/content/1/casestudyABZ2020v1.17.pdf) du document d'étude de cas. Nous nous concentrerons sur la partie **Cruise Control** du régulateur.

## Sommaire

* [Introduction](#Introduction)
* [Traduction des vérifications](#Traduction-des-vérifications)
* [Ecriture et test de propriétés](#Ecriture-et-test-de-propriétés)

## Introduction
Nous allons procéder de la manière suivante : il y a deux opérateurs principaux, la commande et le régulateur en lui-même. La commande est toujours dans le même état (p. 15, "The lever always returns to the neutral position when not touched by the user."). Nous allons donc créer un automate pour la manette et un autre pour le régulateur, et c'est ce dernier qui effectuera les actions demandées par le conducteur.

Déclarations des variables dans UPPAAL:

```

// Variable de frein
int[0,255] brakePedal = 0; //  valeur comprise dans [0-255] // non utilisé
int brakePressure = 0; // non utilisé

// Variable du moteur
bool engineOn = false; // non utilisé
int[0,45] gasPedal = 0; // valeur comprise dans [0;45] // non utilisé

// Variable de vitesse
int setVehicleSpeed  = 0; // Vitesse désirée
int previousSetVehicleSpeed = 0; // Ancienne vitesse désirée
int currentSpeed=1; // Vitesse de la voiture
bool speedLimiterSwitchOn = false; // Pour activer la fonctionnalité adaptative du régulateur
int delta_v; // = setVehicleSpeed - currentSpeed

// Channel
chan deltaV;
chan speed;
chan onChan;
chan offChan;
chan inc;
chan dec;
chan set; // non utilisé
chan commodoStop;// non utilisé
chan startChan; // non utilisé
chan freinStop; // non utilisé
chan pause; 
chan resume; // non utilisé

```

**Attention, il n'y a pas de lien entre gasPedal (ou setVehicleSpeed) et la vitesse du véhicule.**

Nous n'avons pas fait toutes les commandes dans le régulateur : en effet nous nous sommes limités à une augmentation ou une diminution de 1 km/h de la vitesse désirée. Nous n'avons pas fait les commandes liées aux dizaines de km/h qui nous demanderaient de refaire les même méchanismes pour une différente vitesse, de même cela nous demanderait de mutliples vérifications pour chaque vitesse que nous avons entre 1 et 200 km/h.
Nous avons fait de même avec le "maintien appuyé des leviers de commadne", mais le fait que l'augmentation "simple" marche nous permet de penser qu'il ne s'agit que de créer un nouveau signal dans une nouvelle boucle qui part de l'état "engaged" vers lui-même qui met à jour la vitesse désirée.

Forcément, nous n'avons pas pu tester les propriétés qui découlent des focntionnalités que nous n'avons pas crées, mais toutes celles qui existent dans notre automate marchent et sont vérifiées.

## Traduction des vérifications
### SCS 1
>After engie start, there is no previous desired speed. The valid values for desired speed are from 1 km/h to 200 km/h.

Il faut donc qu'au démarrage du véhicule, au moment où le régulateur est encore éteint, la vitesse désirée ne soit pas valide (soit égale à 0). La condition est donc `regulateur.off ==> setVehicleSpeed = 0`
et bien `moteur.start & regulateur.on  ==> setVehiculeSpeed > 1 & setVehiculeSpeed < 200`

### SCS 2
>When pulling the cruise control lever to 1 , the desired speed is either the current vehicle speed (if there is no previous desired speed) or the previous desired speed (if already set).

Quand on appuie sur le bouton 1, la vitesse voulue est soit la vitesse actuelle (si il n'y avait pas de vitessse désirée en mémoire) soit la précédente vitesse désirée en mémoire. On peut donc la résumer ainsi :

`button1 ==> (desired_speed==speed & previous_desired_speed=0) || (desired_speed==previous_desired_speed)`

### SCS 3
>If the current vehicle speed is below 20km/h and there is no previous desired speed, then pulling the cruise control lever to 1 does not activate the (adaptive) cruise control.

Si la vitesse actuelle du véhicule est sous les 20km/h et qu'il n'y a pas de vitesse désirée, appuyer sur le bouton 1 n'active pas le régulateur de vitesse. La condition peut s'écrire ainsi :

`(speed<20 & desired_speed = 0 & button1)==> regulateur.off`

### SCS 4
>If the driver pushes the cruise control lever to 2 up to the first resistance level (5◦) and the (adaptive) cruise control is activated, the desired speed is increased by 1 km/h.

Si le conducteur appuie sur le bouton 2 jusqu'à une résistance de 5° et que le régulateur est activé, la vitesse désirée est augmentée de 1km/h. On a ici besoin de soit rajouter un observateur, soit stocker un historique des valeurs de vitesse précédente.

On ne peut pas vraiment faire une condition du type `(speed=50 & button2_5) --> speed=51` car il faudrait faire cela avec toutes les vitesses entre 20 et 200 km/h.

`button 2 --> previous_desired_speed+1=desired_speed`

### SCS 5
>If the driver pushes the cruise control lever to 2 above the first resistance level (7◦, beyond the pressure point) and the (adaptive) cruise control is activated, the desired speed is increased to the next ten’s place.
Example: Current desired speed is 57 km/h −→ new desired speed is
60 km/h.

Si le conducteur appuie sur le bouton 2 jusqu'à une résistance de 7° et que le régulateur est activé, la vitesse désirée est augmentée jusqu'à la prochaine dizaine de km/h. (ex: on est à 54 km/h -> on passe à 60 km/h). On a ici besoin de soit rajouter un observateur, soit stocker un historique des valeurs de vitesse précédente.

`button 2 --> (previous_desired_speed//10)*10 +10 =desired_speed`

### SCS 6
>Pushing the cruise control lever to 3 reduces the desired speed accordingly to Req. SCS-4 and Req. SCS-5. The lowest desired speed that can be set by pushing the cruise control lever beyond the pressure point is 10 km/h.

Appuyer sur le bouton 3 réduit la vitesse désirée selon les exigences [SCS 4](#SCS-4) et [SCS 5](#SCS-5). La vitesse désirée la plus basse avec cette commande doit être 10 km/h.

`button 2 --> previous_desired_speed-1=desired_speed`
`button 2 --> (previous_desired_speed//10)*10 -10 =desired_speed`
`desired_speed>=10`

### SCS 7
>If the driver pushes the cruise control lever to 2 with activated cruise control within the first resistance level (5◦, not beyond the pressure point) and holds it there for 2 seconds, the desired speed of the cruise control is increased every second by 1 km/h until the lever is in neutral position again. Example: Current desired speed is 57 km/h −→ new desired speed is 58 km/h (due to Req. SCS-4), after holding 2 seconds, desired speed is set to 59 km/h, after holding another second, desired speed is set to 60 km/h, after holding another second, desired speed is set to 61 km/h, etc

Si le conducteur appuie sur le bouton 2 avec le premier niveau de résistance (5°) et le garde appuyé, la vitesse désirée du régulateur augmente de 1 km/h chaque seconde jusqu'à ce que le bouton revienne en position normale. 

`button 2 pendant x secondes--> previous_desired_speed+x=desired_speed`

### SCS 8
>If the driver pushes the cruise control lever to 2 with activated cruise control through the first resistance level (7◦, beyond the pressure point) and holds it there for 2 seconds, the speed set point of the cruise control is increased every 2 seconds to the next ten’s place until the lever is in neutral position again.
Example: Current desired speed is 57 km/h −→ new desired speed is 60 km/h (due to Req. SCS-5), after holding 2 seconds, desired speed is set to 70 km/h, after another 2 seconds, desired speed is set to 80 km/h, after holding another 2 seconds, desired speed is set to 90 km/h, etc.

Même chose que la [#SCS-7](#SCS-7) mais pour les dizaines de km/h

### SCS 9
>If the driver pushes the cruise control lever to 3 with activated cruise control within the first resistance level (5◦, not beyond the pressure point) and holds it there for 2 seconds, the desired speed of the cruise control is reduced every second by 1 km/h until the lever is in neutral position again. Example: Current desired speed is 57 km/h −→ new desired speed is 56 km/h (due to Req. SCS-6) after holding 2 seconds,desired speed is set to 55 km/h, after another second, desired speed is set to 54 km/h, after holding another second, desired speed is set to 53 km/h, etc.

Même chose que la [#SCS-](#SCS-7) mais pour décrémenter de 1 km/h.

### SCS 10
>If the driver pushes the cruise control lever to 3 with activated cruise control through the first resistance level (7◦, beyond the pressure point) and holds it there for 2 seconds, the speed set point of the cruise control is increased every 2 seconds to the next ten’s place until the lever is in neutral position again.

Même chose que la [#SCS-](#SCS-7) mais pour décrémenter de dizaine en dizaine de km/h.

### SCS 11
>If the (adaptive) cruise control is deactivated and the cruise control lever is moved up or down (either to the first or above the first resistance level, the current vehicle speed is used as desired speed.

Si le régulateur de vitesse est désactivé et que la manette est levée ou baissée (boutons 2 et 3), quel que soit le niveau (5 ou 7°), la vitesse du véhicule est considérée comme la vitesse désirée.

`(bouton 2 || bouton 3) & regulateur.desactivé --> desired_speed=speed`

### SCS 12
>Pressing the cruise control lever to 4 deactivates the (adaptive) cruise control. setVehicleSpeed = 0 indicates to the car that there is no speed to maintain.

Appuyer sur le bouton 4 désactive le régulateur de vitesse. setVehiceSpeed=0 indique à la voiture qu'il n'y a pas de vitesse à maintenir.

`bouton 4 -> setVehicleSpeed =0`Lorsque le frein est utilisé, le signal envoyé fait passer le régulateur en "standby" s'il était actif. Cela fait bien le travail décrit 

### SCS 13
>The cruise control is activated using the cruise control lever according to Reqs. SCS-1 to SCS-12.

Le régualteur de vitesse est activé avec la manette et se comporte selon les exigences SCS-1 à SCS-12.
Facile car il suffit de remplir les exigences en question !

### SCS 14
>As long as the cruise control is activated, the vehicle maintains the current vehicle speed at the desired speed without the driver having to press the gas pedal or the brake pedal.

Tant que le régulateur de vitesse est activé, le véhicule maintient la vitesse du véhicule à la vitesse désirée sans que le conducteur ait à appuyer sur la pédale de frein ou d'accélérateur.
`tous les chemins qui ne passent pas par frein ou accélérateur & régulateur.on --> la vitesse reste la même`

### SCS 15
>If the driver pushes the gas pedal and by the position of the gas pedal more acceleration is demanded than by the cruise control, the acceleration setting as demanded by the driver is adopted. Note: This handling is done by the engine autonomously.

Si le conducteur appuie plus fort sur l'accélérateur que l'accélération demandée par le régulateur, l'accélération demandée par le conducteur est adoptée. Note: Cela est géré de manière autonome par le moteur.
Nous n'avons donc rien à faire puisque c'est le moteur qui doit gérer cela.

### SCS 16
>By pushing the brake, the cruise control is deactivated until it is activated again.

En freinant, le régulateur de vitesse est désactivé jusqu'à ce qu'on le réactive.
### SCS 17
>By pushing the control lever backwards, the cruise control is deactivated until it is activated again.

Quand on désactive le régulateur, on arrive dans l'état "standby" jusqu'à ce qu'on le réactive.
## Ecriture et test de propriétés
### SCS-1
- On veut d'abord que `regulateur.off --> desired_speed==0` au démarrage. Or lorsuq'on éteint le régulateur, celui-ci garde en mémoire la vitesse précédente et donc la première fois qu'on l'éteint, il reviendrait dans l'état `off` sans que la vitesse voulue soit nulle. Pour pallier ce problème, on divise l'état "off" en deux : un état initial que l'on quitte lorsqu'on active le rgulateur la première fois, et un autre "en pause" qui permet de sauvegarder la vitese voulue. Ainsi, la propriété se transforme en : `regulateur.off --> setVehicleSpeed==0` (on a créé l'état `standby` qui mémorise la vitesse voulue). **Cette propriété est vérifiée par notre modèle.**
- La valeur de la vitesse est entre 1 et 200 km/h: lorsqu'on a activé le régulateur de vitesse (état "engaged"), la vitesse est entre 1 et 200 km/h. On les traduit ainsi : `regulateur.engaged-->setVehicleSpeed<6 & setVehicleSpeed>0`.
On rappelle ici que notre vitesse n'est qu'entre 1 et 5 au lieu de 1 et 200 km/h pour éviter trop d'état (1 par vitesse...)
### SCS-2
On veut que quand on active le régulateur, pour la première fois, la vitesse désirée soit celle actuelle ou que la précédente si jamais il y en a déjà une d'enregistrée. Cela veut dire que quand on passe de l'état "standby" ou "off" à "engaged", la vitesse désirée est soit la vitesse courante (et alors la vitesse désirée précédente est nulle) soit la vitesse précédente désirée. On peut donc dire que quand on arrive dans l'état "engaged" 
`regulateur.engaged --> (setVehicleSpeed==currentSpeed & previousSetVehicleSpeed==0) || (setvehicleSpeed==previousSetVehicleSpeed)` La permière partie du OU est pour le cas ou on vient de l'état "standby" et la deuwième de l'état "off" (la vitesse précédente est de 0).

Cette propriété n'est pas vérifiée car ce n'est pas `regulateur.engaged` qu'il faut mais `quand on arrive sur regulateur.engaged en venant de "off" ou "standby"`
Pour palier ce problème, on crée un nouvel état "activation" qui est de type "commit" c'est-à-dire atomique, et qui prend les deux transitions arrivant des états "off" et "standby" et les redirige vers l'état "engaged". Ainsi, on peut transformer la propriété en modifiant la condition de l'implication : `regulateur.engaged` devient `regulateur.activation` et la propriété finale est : 

`regulateur.activation --> (setVehicleSpeed==currentSpeed & previousSetVehicleSpeed==0) || (setvehicleSpeed==previousSetVehicleSpeed)`
**Cette propriété est vérifiée par notre modèle.**
### SCS-3
Si la vitesse est inférieure à 20 km/h (**remplacée ici par la valeur 1**) appuyer sur le bouton 1 n'active pas le régulateur de vitesse.
**Cette propriété est vérifiée** par la garde de l'état "off" vers "activation" : `currentSpeed>=1 || setVehicleSpeed!=0`.
### SCS-4
Augmentation de 1km/h par une pression du bouton: on a ajouté un état "INCREASE" qui nous permet de vérifier que si on passe par cet état, c'est bien pour augmenter la vitesse de 1. La propriété est donc : `regulateur.INCREASE --> setVehicleSpeed==previousSetVehicleSpeed+1`
**Cette propriété est vérifiée par notre modèle.**
### SCS-5 
Augmentation de 10km/h par une pression du bouton: on a decidé de ne pas faire cet état afin de garantir la concision de notre modèle. En pratique il aurait été question d'ajouter un état "INCREASE_7", similaire à l'état "INCREASE" qui nous permet de vérifier que si on passe par cet état, c'est bien pour augmenter la vitesse de 10. La propriété est donc : `regulateur.INCREASE_7 --> setVehicleSpeed==previousSetVehicleSpeed+10`
### SCS-6
Mêmes propriétés que pour SCS-4&5; il suffit de mettre decrease et tout est bien **vérifié**. La propriété devient par exemple `regulateur.DECREASE --> setVehicleSpeed==previousSetVehicleSpeed-1`
### SCS-7, 8, 9 & 10
Ces deux exigences requièrent une application de temps pour déterminer de combien de dizaine de km/h on veut augmenter la vitesse. Pour des raisons de practicité de timer, nous ne l'avons pas implémenté, neanmoins comme les fonctions inc et dec fonctionnent, un peut supposer que si la manette envoie le signal avec le bon nombre de secondes, tout marchera bien.
### SCS-11
Si on appuie sur inc ou dec alors qu'on est dans l'état "off" ou "standby", on arrive dans l'état "engaged" avec la vitesse courante égale à la vitesse voulue. On crée l'état "offtoEngage" pour vérifier cette propriété. `regulateur.offToEngage --> setVehicleSpeed==currentSpeed`
### SCS-12
L'appui sur le bouton 4 met le régulateur en standby : c'est le rôle de la transition "engaged" vers "standby" avec le channel `pause`. On vérifie ensuite que si on est dans l'état "standby", ie le régulateur est désactivé, la vitesse voulue est bien nulle. `regulateur.standby --> setVehicleSpeed==0`.
### SCS-13
C'est l'ensemble de SCS-1 & SCS-2.
### SCS-14
Si on n'appuie pas sur le frein ou l'accélérateur, on ne quite pas la boucle "engaged-incstructions" et donc la vitesse du véhicule va se mettre sur la vitesse désirée, à la fréquence à laquelle on passe par la boucle instruction (ie on pourrait donner plusieurs instructions par la commande avant que le régulateur donne des consignes au moteur).
### SCS-15
Rien à faire
### SCS-16
Lorsque le frein est utilisé, le signal envoyé fait passer le régulateur en "standby" s'il était actif. Cela fait bien le travail décrit pas l'éxigence.
### SCS-17

### Deadlock
`A[] not deadlock` est **vérifiée** : il n'y a pas de deadlock dans notre automate.
