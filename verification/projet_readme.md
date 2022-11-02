# Projet de Vérification
Ce rapport documente la réalisation du projet de Vérification du binôme constitué de Stéphane Perrin et de Williams Hoarau
Le but est d'essayer de modeliser un modèle de régulateur de vitesse avec UPPAAL. La modélisation est aussi importante que l'implémentation des exigences.
Ces exigences se trouvent dans le chapitre [5.4 Software Functions](https://moodle.ensta-bretagne.fr/pluginfile.php/88221/mod_resource/content/1/casestudyABZ2020v1.17.pdf) du document d'étude de cas. Nous nous concentrerons sur la partie **Cruise Control** du régulateur.

## Sommaire

* [Introduction](#Introduction)

## Introduction
Nous allons procéder de la manière suivante : il y a deux opérateurs principaux, la commande et le régulateur en lui-même. La commande est toujours dans le même état (p. 15, "The lever always returns to the neutral position when not touched by the user."). Nous allons donc créer un automate pour la manette et un autre pour le régulateur, et c'est ce dernier qui effectuera les actions demandées par le conducteur.
