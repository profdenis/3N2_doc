# 8. Application Hockey Version 3

## Différentes versions de `PlayerListWithSearch`

### Version précédente

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier) {
    var nameSearch by rememberSaveable { mutableStateOf("") }
    var numberSearch: Int? by rememberSaveable { mutableStateOf(null) }
    Column(modifier = modifier) {
        Column(modifier = Modifier.padding(top = 20.dp, bottom = 20.dp)) {
            TextField(
                value = nameSearch,
                label = { Text(text = "Nom") },
                onValueChange = { nameSearch = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 20.dp, end = 20.dp)
            )
            TextField(
                value = (numberSearch ?: "").toString(),
                label = { Text(text = "Numéro") },
                onValueChange = {
                    numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch
                },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 20.dp, end = 20.dp)
            )
        }

        LazyColumn(modifier = Modifier.fillMaxSize()) {
            items(getPlayers(nameSearch, numberSearch)) {
                HockeyPlayerCard(player = it)
            }
        }
    }
}
```

#### Description générale

Ce code définit une fonction composable nommée `PlayerListWithSearch` qui crée une interface utilisateur pour afficher
une liste de joueurs de hockey avec une fonctionnalité de recherche. L'interface comprend deux champs de texte pour la
recherche (un pour le nom et un pour le numéro) et une liste déroulante des joueurs correspondant aux critères de
recherche.

#### Description détaillée

1. Déclaration de la fonction :
   ```kotlin
   @Composable
   fun PlayerListWithSearch(modifier: Modifier = Modifier)
   ```
    - C'est une fonction composable qui accepte un `modifier` optionnel comme paramètre.

2. Variables d'état :
   ```kotlin
   var nameSearch by rememberSaveable { mutableStateOf("") }
   var numberSearch: Int? by rememberSaveable { mutableStateOf(null) }
   ```
    - `nameSearch` : Une chaîne de caractères pour stocker la recherche par nom.
    - `numberSearch` : Un entier nullable pour stocker la recherche par numéro.
    - Ces variables utilisent `rememberSaveable` pour conserver leur état même après une reconfiguration.

3. Structure principale :
   ```kotlin
   Column(modifier = modifier) {
       // Contenu
   }
   ```
    - Utilise un `Column` comme conteneur principal avec le modificateur passé en paramètre.

4. Champs de recherche :
   ```kotlin
   Column(modifier = Modifier.padding(top = 20.dp, bottom = 20.dp)) {
       // Champs de texte
   }
   ```
    - Un `Column` interne avec un padding pour contenir les champs de recherche.

5. Champ de recherche par nom :
   ```kotlin
   TextField(
       value = nameSearch,
       label = { Text(text = "Nom") },
       onValueChange = { nameSearch = it },
       modifier = Modifier
           .fillMaxWidth()
           .padding(start = 20.dp, end = 20.dp)
   )
   ```
    - Un `TextField` pour la recherche par nom.
    - La valeur est liée à `nameSearch`.
    - Le label affiche "Nom".
    - `onValueChange` met à jour `nameSearch` avec la nouvelle valeur.

6. Champ de recherche par numéro :
   ```kotlin
   TextField(
       value = (numberSearch ?: "").toString(),
       label = { Text(text = "Numéro") },
       onValueChange = {
           numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch
       },
       modifier = Modifier
           .fillMaxWidth()
           .padding(start = 20.dp, end = 20.dp)
   )
   ```
    - Un `TextField` pour la recherche par numéro.
    - La valeur affichée est la conversion en chaîne de `numberSearch` (ou une chaîne vide si null).
    - Le label affiche "Numéro".
    - `onValueChange` tente de convertir l'entrée en entier, conservant la valeur précédente si la conversion échoue.

7. Liste des joueurs :
   ```kotlin
   LazyColumn(modifier = Modifier.fillMaxSize()) {
       items(getPlayers(nameSearch, numberSearch)) {
           HockeyPlayerCard(player = it)
       }
   }
   ```
    - Utilise un `LazyColumn` pour afficher efficacement une liste potentiellement longue de joueurs.
    - `getPlayers(nameSearch, numberSearch)` est appelé pour obtenir la liste filtrée des joueurs.
    - Chaque joueur est affiché en utilisant un composant `HockeyPlayerCard`.

Ce code crée une interface utilisateur interactive permettant aux utilisateurs de rechercher des joueurs de hockey par
nom et numéro, avec une mise à jour dynamique de la liste affichée en fonction des critères de recherche.

### Nouvelle version

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier) {
    var nameSearch by rememberSaveable { mutableStateOf("") }
    var numberSearch: Int? by rememberSaveable { mutableStateOf(null) }
    Column(modifier = modifier) {
        SearchTextFields(
            nameSearch = nameSearch,
            onNameChange = { nameSearch = it },
            numberSearch = numberSearch,
            onNumberChange =
            { numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch })

        LazyColumn(modifier = Modifier.fillMaxSize()) {
            items(getPlayers(nameSearch, numberSearch)) {
                HockeyPlayerCard(player = it)
            }
        }
    }
}

@Composable
private fun SearchTextFields(
    nameSearch: String,
    onNameChange: (String) -> Unit,
    numberSearch: Int?,
    onNumberChange: (String) -> Unit,

    ) {
    Column(modifier = Modifier.padding(top = 20.dp, bottom = 20.dp)) {
        TextField(
            value = nameSearch,
            label = { Text(text = "Nom") },
            onValueChange = onNameChange,
            modifier = Modifier
                .fillMaxWidth()
                .padding(start = 20.dp, end = 20.dp)
        )
        TextField(
            value = (numberSearch ?: "").toString(),
            label = { Text(text = "Numéro") },
            onValueChange = onNumberChange,
            modifier = Modifier
                .fillMaxWidth()
                .padding(start = 20.dp, end = 20.dp)
        )
    }
}
```

Cette refactorisation illustre bien comment on peut améliorer la structure et la
réutilisabilité du code en Jetpack Compose. Voici une explication détaillée de la transition entre les deux versions, en
mettant l'accent sur la gestion des variables d'état et des fonctions lambda :

1. Création du nouveau composable `SearchTextFields` :
    - Un nouveau composable privé a été créé pour encapsuler la logique des champs de recherche.
    - Cela améliore la lisibilité et la réutilisabilité du code.

2. Paramètres du nouveau composable :
    - `nameSearch: String` : La valeur actuelle de la recherche par nom.
    - `onNameChange: (String) -> Unit` : Une fonction lambda pour gérer les changements de nom.
    - `numberSearch: Int?` : La valeur actuelle de la recherche par numéro.
    - `onNumberChange: (String) -> Unit` : Une fonction lambda pour gérer les changements de numéro.

3. Passage des variables d'état :
    - Dans `PlayerListWithSearch`, les variables d'état `nameSearch` et `numberSearch` sont passées directement à
      `SearchTextFields`.
    - Cela permet à `SearchTextFields` d'utiliser ces valeurs sans les posséder ou les modifier directement.

4. Passage des fonctions lambda :
    - Pour `onNameChange`, une simple lambda est passée : `{ nameSearch = it }`.
      Cette lambda met à jour directement la variable d'état `nameSearch`.
    - Pour `onNumberChange`, la logique de conversion est passée en tant que lambda :
      ```kotlin
      { numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch }
      ```
      Cette lambda gère la conversion de la chaîne en entier et la mise à jour de `numberSearch`.

5. Utilisation dans `SearchTextFields` :
    - Les `TextField` utilisent maintenant les paramètres passés au lieu d'accéder directement aux variables d'état.
    - `value = nameSearch` et `value = (numberSearch ?: "").toString()` utilisent les valeurs passées.
    - `onValueChange = onNameChange` et `onValueChange = onNumberChange` utilisent les lambdas passées.

6. Avantages de cette approche :
    - Séparation des préoccupations : `SearchTextFields` ne gère que l'affichage et la collecte des entrées.
    - État élevé (State hoisting) : L'état reste dans le composant parent, rendant `SearchTextFields` plus flexible et
      réutilisable.
    - Testabilité améliorée : Il est plus facile de tester `SearchTextFields` indépendamment.

7. Implications pour la gestion de l'état :
    - L'état (`nameSearch` et `numberSearch`) est toujours géré dans `PlayerListWithSearch`.
    - `SearchTextFields` devient un composant "sans état" (stateless), ne faisant que refléter l'état qui lui est passé.

Cette refactorisation est un excellent exemple de la façon dont on peut appliquer le principe de "state hoisting" en
Jetpack Compose, en élevant l'état et les fonctions de modification d'état vers le composant parent. Cela rend le code
plus modulaire, plus facile à maintenir et à tester, tout en conservant une séparation claire des responsabilités entre
les composants.