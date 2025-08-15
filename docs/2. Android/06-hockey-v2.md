# 6. Application Hockey Version 2

## Explication de `PlayerListWithSearch`

Cette nouvelle version ajoute une fonctionnalité de recherche à la liste des joueurs. Voici une explication détaillée
des modifications :

### Fonction `getPlayers`

```kotlin
fun getPlayers(name: String? = null): List<Player> =
    if (name == null)
        getSamplePlayers()
    else
        getSamplePlayers().filter { it.name.lowercase().contains(name.lowercase()) }.toList()
```

Cette fonction permet de filtrer la liste des joueurs en fonction d'un critère de recherche :

- Si `name` est `null`, elle retourne tous les joueurs.
- Sinon, elle filtre la liste pour ne retourner que les joueurs dont le nom contient la chaîne de recherche (insensible
  à la casse).

### Composable `PlayerListWithSearch`

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier) {
    var searchCriteria by rememberSaveable { mutableStateOf("") }
    Column {
        TextField(
            value = searchCriteria,
            onValueChange = { searchCriteria = it },
            modifier = modifier
        )
        LazyColumn(modifier = modifier.fillMaxSize()) {
            items(getPlayers(searchCriteria)) {
                PlayerCard(player = it)
            }
        }
    }
}
```

Ce composable remplace `PlayerList` et ajoute une fonctionnalité de recherche :

1. `searchCriteria` :
    - Utilise `rememberSaveable` pour conserver l'état de la recherche même lors des recompositions.
    - `mutableStateOf("")` initialise la valeur de recherche à une chaîne vide.

2. `Column` :
    - Organise verticalement le champ de recherche et la liste des joueurs.

3. `TextField` :
    - Permet à l'utilisateur d'entrer un critère de recherche.
    - `value = searchCriteria` affiche la valeur actuelle.
    - `onValueChange = { searchCriteria = it }` met à jour `searchCriteria` à chaque modification.

4. `LazyColumn` :
    - Similaire à la version précédente, mais utilise maintenant `getPlayers(searchCriteria)`.
    - Cela filtre dynamiquement la liste des joueurs en fonction du critère de recherche.

5. `items` :
    - Crée une `PlayerCard` pour chaque joueur filtré.

Cette nouvelle version offre une expérience utilisateur plus interactive, permettant de rechercher des joueurs
spécifiques dans la liste. La liste se met à jour automatiquement à chaque modification du texte dans le champ de
recherche, grâce à l'utilisation de l'état (`searchCriteria`) et de la recomposition automatique de Jetpack Compose.

Certainement. Voici une explication plus détaillée des connexions entre ces éléments, formatée en markdown avec le code
Kotlin approprié :

## Connexions entre `TextField`, `mutableStateOf`, `onValueChange`, et `getPlayers()`

### 1. `mutableStateOf` et État Composable

```kotlin
var searchCriteria by rememberSaveable { mutableStateOf("") }
```

- `mutableStateOf("")` crée un état mutable initialisé avec une chaîne vide.
- `rememberSaveable` permet de conserver cet état même lors des recompositions ou des changements de configuration.
- `by` est utilisé pour déléguer la propriété, permettant d'accéder et de modifier `searchCriteria` directement.

### 2. `TextField` et `onValueChange`

```kotlin
TextField(
    value = searchCriteria,
    onValueChange = { searchCriteria = it },
    modifier = modifier
)
```

- `value = searchCriteria` : Le `TextField` affiche la valeur actuelle de `searchCriteria`.
- `onValueChange = { searchCriteria = it }` : Lorsque l'utilisateur tape dans le champ :
    - `it` représente la nouvelle valeur du champ.
    - Cette nouvelle valeur est assignée à `searchCriteria`.
    - Cela déclenche une recomposition du composable.

### 3. Connexion avec `getPlayers()`

```kotlin
LazyColumn(modifier = modifier.fillMaxSize()) {
    items(getPlayers(searchCriteria)) {
        PlayerCard(player = it)
    }
}
```

- `getPlayers(searchCriteria)` est appelé avec la valeur actuelle de `searchCriteria`.
- Chaque fois que `searchCriteria` change :
    - `getPlayers()` est rappelé avec la nouvelle valeur.
    - La liste des joueurs est filtrée en fonction de cette nouvelle valeur.
    - `LazyColumn` se recompose avec la nouvelle liste filtrée.

### 4. Flux de données et recomposition

1. L'utilisateur tape dans le `TextField`.
2. `onValueChange` est appelé, mettant à jour `searchCriteria`.
3. La mise à jour de `searchCriteria` déclenche une recomposition.
4. Lors de la recomposition :
    - `TextField` affiche la nouvelle valeur de `searchCriteria`.
    - `getPlayers(searchCriteria)` est appelé avec la nouvelle valeur.
    - `LazyColumn` se recompose avec la nouvelle liste filtrée.


Cette architecture réactive permet une mise à jour en temps réel de l'interface utilisateur. Chaque frappe dans le champ
de recherche déclenche une chaîne de réactions qui aboutit à l'affichage filtré des joueurs, offrant une expérience
utilisateur fluide et réactive.



-------
??? info "Utilisation de l'IA"
    Page rédigée en partie avec l'aide d'un assistant IA. L'IA a été utilisée pour générer des 
    explications, des exemples et/ou des suggestions de structure. Toutes les informations ont 
    été vérifiées, éditées et complétées par l'auteur.