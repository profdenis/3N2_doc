# 9. Gestion des états et des événements

## Exemple : ExemplesEntrees

Exemple sur [GitHub](https://github.com/profdenis/ExemplesEntrees)

### Première version

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FruitSelector(modifier: Modifier = Modifier) {
    val fruits = listOf("Pomme", "Banane", "Orange", "Fraise", "Kiwi")
    var expanded by remember { mutableStateOf(false) }
    var selectedFruit by remember { mutableStateOf("") }

    Column(modifier = modifier.padding(16.dp)) {
        ExposedDropdownMenuBox(
            expanded = expanded,
            onExpandedChange = { expanded = !expanded }
        ) {
            TextField(
                value = selectedFruit,
                onValueChange = {},
                readOnly = true,
                label = { Text("Choisissez un fruit") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = expanded) },
                modifier = Modifier
                    .menuAnchor()
                    .fillMaxWidth()
            )

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
        }

        Spacer(modifier = Modifier.height(16.dp))

        if (selectedFruit.isNotEmpty()) {
            Text("Fruit sélectionné : $selectedFruit")
        }
    }
}

@Composable
fun App(modifier: Modifier = Modifier) {
    Column(modifier = modifier) {
        TextLengthCounter()
        Spacer(modifier = Modifier.height(100.dp))
        FruitSelector()
    }
}
```

Bien sûr, je vais vous donner une description générale de ces deux fonctions, en me concentrant particulièrement sur
`FruitSelector`.

#### Description générale

1. `FruitSelector` : C'est une fonction composable qui crée un sélecteur de fruits sous forme de menu déroulant.

2. `App` : C'est la fonction composable principale qui structure l'interface utilisateur de l'application en combinant
   différents composants.

#### Focus sur FruitSelector

La fonction `FruitSelector` est un composant Jetpack Compose qui crée un menu déroulant permettant à l'utilisateur de
sélectionner un fruit parmi une liste prédéfinie. Voici ses principales caractéristiques :

1. **Liste de fruits** : Une liste statique de fruits est définie au début de la fonction.

2. **États** :
    - `expanded` : Un état booléen qui contrôle si le menu déroulant est ouvert ou fermé.
    - `selectedFruit` : Un état qui stocke le fruit actuellement sélectionné.

3. **Interface utilisateur** :
    - Utilise `ExposedDropdownMenuBox` pour créer le conteneur du menu déroulant.
    - Affiche un `TextField` qui sert de déclencheur pour ouvrir le menu. Ce champ est en lecture seule et affiche le
      fruit sélectionné.
    - Le menu déroulant (`ExposedDropdownMenu`) contient la liste des fruits, chacun représenté par un
      `DropdownMenuItem`.

4. **Interaction** :
    - Lorsqu'un fruit est sélectionné, le menu se ferme et le fruit choisi est affiché dans le TextField.
    - Un texte supplémentaire s'affiche en dessous pour confirmer le fruit sélectionné.

5. **Mise en page** :
    - Utilise une `Column` pour organiser verticalement les éléments.
    - Ajoute des espacements et du rembourrage pour améliorer l'apparence.

6. **Personnalisation** :
    - Accepte un `modifier` en paramètre pour permettre une personnalisation supplémentaire si nécessaire.

Cette fonction démontre l'utilisation de plusieurs concepts importants de Jetpack Compose, tels que la gestion d'état,
les composants d'interface utilisateur Material 3, et la création de composants interactifs. Elle offre une interface
utilisateur intuitive pour la sélection d'éléments dans une liste, ce qui est une fonctionnalité courante dans de
nombreuses applications.

### Deuxième version

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FruitSelector(
    modifier: Modifier = Modifier,
    fruits: List<String>,
    selectedFruit: String,
    onFruitSelected: (String) -> Unit = {}
) {
    var expanded by remember { mutableStateOf(false) }

    Column(modifier = modifier.padding(16.dp)) {
        ExposedDropdownMenuBox(
            expanded = expanded,
            onExpandedChange = { expanded = !expanded }
        ) {
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

            ExposedDropdownMenu(
                expanded = expanded,
                onDismissRequest = { expanded = false }
            ) {
                fruits.forEach { fruit ->
                    DropdownMenuItem(
                        text = { Text(fruit) },
                        onClick = {
                            onFruitSelected(fruit)
                            expanded = false
                        }
                    )
                }
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        if (selectedFruit.isNotEmpty()) {
            Text("Fruit sélectionné : $selectedFruit")
        }
    }
}

@Composable
fun App(modifier: Modifier = Modifier) {
    var selectedFruit1 by rememberSaveable { mutableStateOf("") }
    var selectedFruit2 by rememberSaveable { mutableStateOf("") }

    Column(modifier = modifier) {
        TextLengthCounter()
        Spacer(modifier = Modifier.height(100.dp))
        FruitSelector(
            fruits = listOf("Pomme", "Banane", "Orange", "Fraise", "Kiwi"),
            selectedFruit = selectedFruit1,
            onFruitSelected = { fruit -> selectedFruit1 = fruit }
        )
        Spacer(modifier = Modifier.height(10.dp))
        FruitSelector(
            fruits = listOf("Poire", "Mangue", "Orange", "Bleuet", "Pamplemousse"),
            selectedFruit = selectedFruit2,
            onFruitSelected = { fruit -> selectedFruit2 = fruit }
        )
        Spacer(modifier = Modifier.height(10.dp))
        Text("Fruit sélectionné 1 : ${selectedFruit1 ?: "Aucun"}")
        Text("Fruit sélectionné 2 : ${selectedFruit2 ?: "Aucun"}")
    }
}
```

Cette nouvelle version de l'application démontre une refactorisation importante, principalement axée sur l'élévation de
l'état (state hoisting) et la réutilisabilité des composants. Examinons en détail les changements et leurs
implications :

#### Refactorisation de FruitSelector

1. **Élévation de l'état** :
    - `selectedFruit` n'est plus géré à l'intérieur de `FruitSelector`. Il est maintenant passé en tant que paramètre.
    - Une nouvelle fonction `onFruitSelected` est ajoutée comme paramètre pour gérer les changements de sélection.

2. **Paramètres ajoutés** :
    - `fruits: List<String>` : La liste des fruits est maintenant un paramètre, rendant le composant plus flexible.
    - `selectedFruit: String` : L'état du fruit sélectionné est passé en paramètre.
    - `onFruitSelected: (String) -> Unit` : Une fonction de rappel pour gérer la sélection d'un fruit.

3. **État local restant** :
    - `expanded` reste un état local car il concerne uniquement l'affichage du menu déroulant.

#### Modifications dans App

1. **Gestion de l'état** :
    - Deux nouvelles variables d'état sont introduites : `selectedFruit1` et `selectedFruit2`.
    - Ces états sont créés avec `rememberSaveable` pour persister à travers les recompositions et les changements de
      configuration.

2. **Utilisation de FruitSelector** :
    - Deux instances de `FruitSelector` sont créées, chacune avec sa propre liste de fruits et son propre état.
    - L'état et la fonction de mise à jour sont passés à chaque `FruitSelector`.

3. **Affichage des sélections** :
    - Les fruits sélectionnés sont affichés en bas de l'App, démontrant que l'état est maintenant géré au niveau
      supérieur.

#### Avantages de cette refactorisation

1. **Réutilisabilité** : `FruitSelector` peut maintenant être utilisé plusieurs fois avec différentes listes de fruits.

2. **Séparation des responsabilités** : La gestion de l'état est séparée de l'affichage, rendant le code plus modulaire.

3. **Contrôle accru** : L'App a maintenant un contrôle total sur l'état des sélections, permettant des interactions plus
   complexes si nécessaire.

4. **Testabilité améliorée** : Il est plus facile de tester `FruitSelector` car son comportement dépend entièrement des
   props passées.

5. **Flexibilité** : La liste de fruits peut être dynamique, provenant par exemple d'une API ou d'une base de données.

#### Focus sur les variables d'état

- Dans `App`, `selectedFruit1` et `selectedFruit2` sont des variables d'état créées avec `rememberSaveable`. Cela
  signifie qu'elles conserveront leur valeur même lors des changements de configuration (comme la rotation de l'écran).

- Dans `FruitSelector`, seul `expanded` reste une variable d'état locale, car elle ne concerne que l'affichage interne
  du composant.

Cette refactorisation illustre des principes importants de Jetpack Compose, notamment l'élévation de l'état et la
création de composants réutilisables et indépendants. Elle permet une meilleure gestion de l'état de l'application et
une plus grande flexibilité dans la construction de l'interface utilisateur.
