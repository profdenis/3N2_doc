# 4. Application Hockey Version 1

Ce code définit une structure pour représenter des joueurs de hockey et crée des composants d'interface utilisateur en
utilisant Jetpack Compose pour afficher ces joueurs dans une liste. Il comprend une classe de données pour les joueurs,
une fonction pour générer des exemples de joueurs, et deux fonctions composables principales : une pour afficher une
carte de joueur individuelle et une autre pour afficher une liste de ces cartes.

## Explication détaillée des fonctions composables

### `PlayerCard`

```kotlin
@Composable
fun PlayerCard(player: Player, modifier: Modifier = Modifier) {
    // ...
}
```

Cette fonction composable crée une carte pour afficher les informations d'un joueur individuel.

- Elle prend en paramètre un objet `Player` et un `Modifier` optionnel.
- Utilise un composant `Card` pour créer une carte avec des coins arrondis et une bordure.
- À l'intérieur de la carte, elle organise le contenu dans une `Column` (colonne verticale).
- La première `Row` (ligne horizontale) contient l'image du joueur.
- La deuxième `Row` affiche le numéro et le nom du joueur.

Composants utilisés :

- `Card` : Crée une surface élevée avec une ombre et un contenu.
- `Column` : Organise les éléments verticalement.
- `Row` : Organise les éléments horizontalement.
- `Image` : Affiche l'image du joueur.
- `Text` : Affiche le texte (numéro et nom du joueur).
- `Spacer` : Crée un espace entre les éléments.

Modificateurs utilisés :

- `fillMaxWidth()` : Remplit toute la largeur disponible.
- `padding()` : Ajoute de l'espace autour des éléments.
- `background()` : Définit la couleur de fond.
- `width()` : Définit une largeur spécifique.

### `PlayerList`

```kotlin
@Composable
fun PlayerList(modifier: Modifier = Modifier) {
    LazyColumn(modifier = modifier) {
        items(getSamplePlayers()) {
            PlayerCard(player = it)
        }
    }
}
```

Cette fonction composable crée une liste déroulante de cartes de joueurs.

- Elle utilise `LazyColumn`, qui est optimisée pour afficher de longues listes d'éléments.
- La fonction `items()` est utilisée pour générer dynamiquement les éléments de la liste à partir de la liste de joueurs
  retournée par `getSamplePlayers()`.
- Pour chaque joueur dans la liste, elle crée une `PlayerCard`.

Composant principal :

- `LazyColumn` : Un conteneur de défilement vertical qui charge et affiche uniquement les éléments visibles à l'écran,
  ce qui le rend efficace pour les longues listes.

Ces deux fonctions composables travaillent ensemble pour créer une interface utilisateur interactive et efficace pour
afficher une liste de joueurs de hockey. `PlayerCard` gère l'affichage des détails individuels des joueurs, tandis que
`PlayerList` organise ces cartes dans une liste déroulante.



-------
??? info "Utilisation de l'IA"
    Page rédigée en partie avec l'aide d'un assistant IA. L'IA a été utilisée pour générer des 
    explications, des exemples et/ou des suggestions de structure. Toutes les informations ont 
    été vérifiées, éditées et complétées par l'auteur.