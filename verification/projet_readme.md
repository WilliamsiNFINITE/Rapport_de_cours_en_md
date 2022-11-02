# Projet de Vérification
Ce rapport documente la réalisation du projet de Vérification du binôme constitué de Stéphane Perrin et de Williams Hoarau
Le but est d'essayer de modeliser un modèle de régulateur de vitesse avec UPPAAL. La modélisation est aussi importante que l'implémentation des exigences.
Ces exigences se trouvent dans le chapitre [5.4 Software Functions](https://moodle.ensta-bretagne.fr/pluginfile.php/88221/mod_resource/content/1/casestudyABZ2020v1.17.pdf) du document d'étude de cas. Nous nous concentrerons sur la partie **Cruise Control** du régulateur.

## Sommaire

* [Introduction](#Introduction)
* [Traduction des vérifications](#Traduction-des-vérifications)
  * [SCS 1](#SCS-1)
  * [SCS 2](#SCS-2)
  * [SCS 3](#SCS-3)
  * [SCS 4](#SCS-4)
  * [SCS 5](#SCS-5)
  * [SCS 6](#SCS-6)
  * [SCS 7](#SCS-7)
  * [SCS 8](#SCS-8)
  * [SCS 9](#SCS-9)

## Introduction
Nous allons procéder de la manière suivante : il y a deux opérateurs principaux, la commande et le régulateur en lui-même. La commande est toujours dans le même état (p. 15, "The lever always returns to the neutral position when not touched by the user."). Nous allons donc créer un automate pour la manette et un autre pour le régulateur, et c'est ce dernier qui effectuera les actions demandées par le conducteur.

Déclarations :

- setSpeed ()
- brakePedal (0-255)
- brakePressure ()
- engineOn (True, False)
- speedLimiterSwitchOn (True,False)
- setVehicleSpeed (>0)
- gasPedal (max = 45° = maximum setVehicleSpeed)
- d (deceleration = brakePedal/37,5

**Attention, il n'y a pas de lien entre gasPedal (ou setVehicleSpeed) et la vitesse du véhicule.**



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

### SCS 8
>If the driver pushes the cruise control lever to 2 with activated cruise control through the first resistance level (7◦, beyond the pressure point) and holds it there for 2 seconds, the speed set point of the cruise control is increased every 2 seconds to the next ten’s place until the lever is in neutral position again.
Example: Current desired speed is 57 km/h −→ new desired speed is 60 km/h (due to Req. SCS-5), after holding 2 seconds, desired speed is set to 70 km/h, after another 2 seconds, desired speed is set to 80 km/h, after holding another 2 seconds, desired speed is set to 90 km/h, etc.

Même chose que la [#SCS-7](#SCS-7) mais pour les dizaines de km/h

### SCS 9
>If the driver pushes the cruise control lever to 3 with activated cruise control within the first resistance level (5◦, not beyond the pressure point) and holds it there for 2 seconds, the desired speed of the cruise control is reduced every second by 1 km/h until the lever is in neutral position again. Example: Current desired speed is 57 km/h −→ new desired speed is 56 km/h (due to Req. SCS-6) after holding 2 seconds,desired speed is set to 55 km/h, after another second, desired speed is set to 54 km/h, after holding another second, desired speed is set to 53 km/h, etc.

Même chose que la [#SCS-](#SCS-7) mais pour les dizaines de km/h
