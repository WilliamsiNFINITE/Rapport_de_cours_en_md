# Compte rendu du TP2 : Ordonnancement

Ce rapport documente la manière dont j'ai réalisé le projet du cours sur l'architecture distribuée et la virtualisation.
Voici le lien du repository du projet : https://github.com/WilliamsiNFINITE/projet_architecture_distribuee_virtualisation

## Sommaire

* [Exercice 1](#exercice-1)


## Exercice 1

- a. En invoquant les appels spécifiques à l’ordonnancement (getpriority()).

Voici le code que j'ai utilisé afin de changer la priorité du processus. Le pid du processus est donné en paramètres.

```C
int main(int argc, char *argv[]) {
    int priority = 0;
    int settedPriority = 0;
    int id = atoi(argv[1]);

    // visualisation des propriétés d'ordonnacement du processus avec getpriority()
    priority = getpriority(PRIO_PROCESS, id);
    printf("priorité du processus : %d \n", priority);

    return 0;

}
```


