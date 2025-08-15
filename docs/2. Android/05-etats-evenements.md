# 5. Gestion des états et des événements

## Exemple : ExemplesEntrees

Exemple sur [GitHub](https://github.com/profdenis/ExemplesEntrees)

### Fonction `TextLengthCounter`

La fonction `TextLengthCounter` est un composable Jetpack Compose qui crée une interface utilisateur permettant à
l'utilisateur de saisir du texte et d'afficher sa longueur en caractères. Elle utilise des états pour gérer le texte
saisi et sa longueur, et met à jour dynamiquement l'affichage.

Voici une description détaillée de la fonction :

### Déclaration de la fonction :

```kotlin
@Composable
fun TextLengthCounter(modifier: Modifier = Modifier)
```
- C'est une fonction composable qui peut recevoir un `Modifier` en paramètre.

### Gestion de l'état :

```kotlin
var text by remember { mutableStateOf("") }
var length by remember { mutableIntStateOf(0) }
```
- Deux variables d'état sont créées : `text` pour stocker le texte saisi et `length` pour la longueur du texte.
- `remember` est utilisé pour conserver ces états entre les recompositions.

### Structure de l'interface :

```kotlin
Column(
    modifier = Modifier
        .padding(16.dp)
        .fillMaxWidth(),
    horizontalAlignment = Alignment.CenterHorizontally
) {
    // Contenu de la colonne
}
```

- Un `Column` est utilisé pour organiser verticalement les éléments de l'interface.
- Un padding de 16dp est appliqué, et la colonne occupe toute la largeur disponible.
- Les éléments sont centrés horizontalement.

### Champ de saisie :

```kotlin
TextField(
    value = text,
    onValueChange = {
        text = it
        length = text.length
    },
    label = { Text("Entrez du texte") },
    modifier = Modifier.fillMaxWidth()
)
```

- Un `TextField` permet à l'utilisateur de saisir du texte.
- La valeur du champ est liée à l'état `text`.
- À chaque changement, `text` est mis à jour et `length` est recalculé.

### Espacement :

```kotlin
Spacer(modifier = Modifier.height(16.dp))
```

- Des `Spacer` sont utilisés pour ajouter de l'espace vertical entre les éléments.

### Bouton de calcul :

```kotlin
Button(
    onClick = { length = text.length }
) {
    Text("Calculer la longueur")
}
```

- Un bouton permet de recalculer manuellement la longueur du texte.

### Affichage de la longueur :

```kotlin
Text("Longueur du texte : $length caractères")
```

- Un `Text` affiche la longueur actuelle du texte.

Cette fonction illustre plusieurs concepts importants de Jetpack Compose, notamment la gestion de l'état, la réactivité
des composants, et l'organisation de l'interface utilisateur. Elle montre comment créer une interface interactive
simple, mais fonctionnelle.

## Gestion de l'état avec `remember` et `mutableStateOf`

Dans Jetpack Compose, l'état est un concept crucial. Il représente toute donnée qui peut changer au fil du temps et qui,
lorsqu'elle change, peut déclencher une recomposition de l'interface utilisateur.

```kotlin
var text by remember { mutableStateOf("") }
var length by remember { mutableIntStateOf(0) }
```

### Rôle de `remember`

- `remember` est une fonction qui permet de conserver un objet entre les recompositions.
- Sans `remember`, chaque recomposition créerait un nouvel objet, perdant ainsi l'état précédent.
- `remember` "mémorise" l'objet initial et le réutilise lors des recompositions suivantes.

### Fonction de `mutableStateOf`

- `mutableStateOf` crée un objet `MutableState<T>` qui encapsule une valeur mutable.
- Lorsque cette valeur change, Compose est notifié et peut déclencher une recomposition si nécessaire.
- `mutableIntStateOf` est une version spécialisée pour les entiers, optimisée pour les performances.

### Délégation avec `by`

- Le mot-clé `by` est utilisé pour la délégation de propriété en Kotlin.
- Il permet d'utiliser directement `text` et `length` comme s'ils étaient des variables normales, tout en bénéficiant de
  la réactivité de `MutableState`.

## Lambdas et mise à jour de l'état

Les lambdas sont utilisées pour définir des comportements en réponse à des événements, comme les changements de valeur
ou les clics.

### Dans le TextField

```kotlin
onValueChange = {
    text = it
    length = text.length
}
```

- Cette lambda est appelée chaque fois que le contenu du `TextField` change.
- `it` représente la nouvelle valeur du champ texte.
- La lambda met à jour `text` avec la nouvelle valeur et recalcule immédiatement `length`.
- Ces mises à jour déclenchent une recomposition, mettant à jour l'interface utilisateur.

### Dans le Button

```kotlin
onClick = { length = text.length }
```

- Cette lambda est plus simple, elle est appelée lors d'un clic sur le bouton.
- Elle recalcule `length` basé sur la valeur actuelle de `text`.
- Bien que cela semble redondant ici (car `length` est déjà à jour), cela pourrait être utile dans des scénarios plus
  complexes.

### Réactivité et flux de données

1. Lorsque l'utilisateur tape du texte, `onValueChange` est appelé.
2. `text` est mis à jour, ce qui notifie Compose d'un changement d'état.
3. `length` est également mis à jour immédiatement.
4. Compose détecte ces changements d'état et déclenche une recomposition.
5. Lors de la recomposition, le `TextField` affiche le nouveau texte, et le `Text` en bas affiche la nouvelle longueur.

Cette approche assure que l'interface utilisateur reste toujours synchronisée avec l'état interne de l'application,
offrant une expérience réactive et cohérente à l'utilisateur.

En résumé, cet exemple illustre comment Jetpack Compose utilise la gestion de l'état, les lambdas, et la recomposition
pour créer une interface utilisateur dynamique et réactive. La combinaison de `remember` et `mutableStateOf` permet de
maintenir et de gérer efficacement l'état, tandis que les lambdas fournissent un moyen élégant de réagir aux
interactions de l'utilisateur et de mettre à jour l'état en conséquence.

## Fonction `FruitSelector`

La fonction `FruitSelector` est un composable Jetpack Compose qui crée un sélecteur de fruits sous forme de menu
déroulant. Elle permet à l'utilisateur de choisir un fruit parmi une liste prédéfinie et affiche le fruit sélectionné.

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FruitSelector(modifier: Modifier = Modifier) {
```

### Déclaration de la fonction
    - `@OptIn(ExperimentalMaterial3Api::class)` indique l'utilisation d'API expérimentales de Material 3.
    - C'est une fonction composable qui accepte un `modifier` optionnel.

```kotlin
    val fruits = listOf("Pomme", "Banane", "Orange", "Fraise", "Kiwi")
    var expanded by remember { mutableStateOf(false) }
    var selectedFruit by rememberSaveable { mutableStateOf("") }
```

### Initialisation des états

- `fruits` est une liste statique de fruits.
- `expanded` est un état booléen pour contrôler l'ouverture/fermeture du menu déroulant.
- `selectedFruit` est un état pour stocker le fruit sélectionné. `rememberSaveable` est utilisé pour conserver la
  sélection même après une reconfiguration (comme une rotation d'écran).

```kotlin
    Column(modifier = modifier.padding(16.dp)) {
       ExposedDropdownMenuBox(
           expanded = expanded,
           onExpandedChange = { expanded = !expanded }
       ) {
```

### Structure de l'interface

- Un `Column` contient tous les éléments avec un padding de 16dp.
- `ExposedDropdownMenuBox` est le conteneur principal du menu déroulant.

```kotlin
            TextField(
                value = selectedFruit,
                onValueChange = {},
                readOnly = true,
                label = { Text("Choisissez un fruit") },
                trailingIcon = {
                    ExposedDropdownMenuDefaults
                        .TrailingIcon(expanded = expanded)
                },
                modifier = Modifier
                    .menuAnchor()
                    .fillMaxWidth()
            )
```

### Champ de texte du sélecteur

- Un `TextField` affiche le fruit sélectionné.
- Il est en lecture seule (`readOnly = true`).
- Une icône de menu déroulant est ajoutée à droite.
- `.menuAnchor()` lie ce champ au menu déroulant.

```kotlin
            ExposedDropdownMenu(
                expanded = expanded,
                onDismissRequest = { expanded = false }
            ) {
                fruits.forEach { fruit ->
                    DropdownMenuItem(
                        text = { Text(fruit) },
                        onClick = {
                            selectedFruit = fruit
                            expanded = false
                        }
                    )
                }
            }
```

### Menu déroulant

- `ExposedDropdownMenu` contient la liste des fruits.
- Chaque fruit est représenté par un `DropdownMenuItem`.
- Lors du clic sur un fruit, `selectedFruit` est mis à jour et le menu se ferme.

```kotlin
        Spacer(modifier = Modifier.height(16.dp))

        if (selectedFruit.isNotEmpty()) {
            Text("Fruit sélectionné : $selectedFruit")
        }
    }
}
```

### Affichage de la sélection

- Un espace vertical est ajouté.
- Si un fruit est sélectionné, il est affiché en dessous du menu.

### Fonctionnement global

- L'utilisateur voit un champ de texte avec un label "Choisissez un fruit".
- En cliquant sur le champ, un menu déroulant s'ouvre avec la liste des fruits.
- La sélection d'un fruit met à jour l'état `selectedFruit` et ferme le menu.
- Le fruit sélectionné est affiché dans le champ de texte et en dessous.

Cette fonction illustre plusieurs concepts avancés de Jetpack Compose :

- Utilisation de composants Material 3 pour créer un menu déroulant interactif.
- Gestion d'états avec `remember` et `rememberSaveable`.
- Création d'une interface utilisateur dynamique et réactive.
- Utilisation de lambdas pour gérer les interactions utilisateur.
- Conditionnement de l'affichage basé sur l'état (`if (selectedFruit.isNotEmpty())`).



-------
??? info "Utilisation de l'IA"
    Page rédigée en partie avec l'aide d'un assistant IA. L'IA a été utilisée pour générer des 
    explications, des exemples et/ou des suggestions de structure. Toutes les informations ont 
    été vérifiées, éditées et complétées par l'auteur.