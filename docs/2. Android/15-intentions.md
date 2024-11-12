# 15. Intentions

[Dépôt Git](https://github.com/profdenis/Activites_Multiples)

## Structure Générale

Le code définit une activité principale (`MainActivity`) qui contient deux composants Composable pour démarrer d'autres
activités.

### MainActivity

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Column {
                StartOtherActivity()
                StartOtherActivityWithValue()
            }
        }
    }
}
```

- C'est l'activité principale qui hérite de `ComponentActivity`
- Dans `onCreate`, elle définit son contenu avec deux composants Composable dans une `Column`

### StartOtherActivity

```kotlin
@Composable
fun StartOtherActivity() {
    val context = LocalContext.current
    Column(
        modifier = Modifier.fillMaxWidth(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Écran Principal")
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = {
            val intent = Intent(context, SecondActivity::class.java)
            context.startActivity(intent)
        }) {
            Text("Aller à l'écran secondaire")
        }
    }
}
```

- Ce composant affiche un titre et un bouton
- Quand on clique sur le bouton, il crée un `Intent` pour démarrer `SecondActivity`
- L'`Intent` est un mécanisme Android pour démarrer une autre activité

### StartOtherActivityWithValue

```kotlin
@Composable
fun StartOtherActivityWithValue() {
    val context = LocalContext.current
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Button(onClick = {
            val intent = Intent(context, ThirdActivity::class.java)
            intent.putExtra("buttonValue", 1)
            context.startActivity(intent)
        }) {
            Text("Bouton 1")
        }
        // Deuxième bouton similaire avec valeur 2
    }
}
```

- Ce composant affiche deux boutons
- Chaque bouton démarre `ThirdActivity` en passant une valeur différente
- `putExtra` permet de passer des données à l'activité suivante

## Points Importants à Noter

1. L'utilisation de `LocalContext.current` pour obtenir le contexte nécessaire à la création d'`Intent`
2. La différence entre un simple changement d'activité et un changement avec passage de données
3. L'organisation du layout avec `Column`, `Spacer`, et les modificateurs pour l'alignement
4. L'utilisation de composants Jetpack Compose (`Button`, `Text`, etc.)

Je vais expliquer ce code qui représente une activité secondaire dans l'application.

## Structure de SecondActivity

### La Classe SecondActivity

```kotlin
class SecondActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            SecondScreen(
                onNavigateBack = {
                    finish()
                }
            )
        }
    }
}
```

- Cette classe hérite de `ComponentActivity`, comme l'activité principale
- Dans `onCreate`, elle définit son contenu avec le composant `SecondScreen`
- La fonction `finish()` est passée comme callback pour gérer le retour à l'écran précédent
- `finish()` est une méthode Android qui termine l'activité courante et retourne à l'activité précédente

### Le Composant SecondScreen

```kotlin
@Composable
fun SecondScreen(onNavigateBack: () -> Unit) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Écran Secondaire")
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = onNavigateBack) {
            Text("Retourner à l'écran principal")
        }
    }
}
```

- Le composant prend un paramètre `onNavigateBack` qui est une fonction lambda sans paramètres (`() -> Unit`)
- L'interface est simple avec :
    - Un titre "Écran Secondaire"
    - Un espace vertical de 16dp
    - Un bouton pour revenir à l'écran principal
- Le layout utilise une `Column` centrée horizontalement et verticalement
- Le modificateur `fillMaxSize()` fait occuper tout l'espace disponible à la colonne

## Points Importants à Noter

1. **Gestion de la Navigation**
    - La navigation retour est gérée proprement avec `finish()`
    - Le callback est passé du niveau activité au composant Composable

2. **Structure du Code**
    - Séparation claire entre l'activité et l'interface utilisateur
    - Utilisation de composants réutilisables

3. **Bonnes Pratiques**
    - Le composant `SecondScreen` est découplé de l'activité
    - La navigation est gérée via un callback plutôt qu'une référence directe à l'activité

4. **Interface Utilisateur**
    - Layout simple et centré
    - Utilisation appropriée des espacements
    - Interface claire et intuitive

## Structure de ThirdActivity

### La Classe ThirdActivity

```kotlin
class ThirdActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val buttonValue = intent.getIntExtra("buttonValue", 0)

        setContent {
            ThirdScreen(
                buttonValue = buttonValue,
                onNavigateBack = {
                    finish()
                }
            )
        }
    }
}
```

- Cette activité récupère une valeur passée via l'`Intent` avec `getIntExtra`
- Le second paramètre `0` est la valeur par défaut si aucune valeur n'est trouvée
- La valeur récupérée est passée au composant `ThirdScreen`

### Le Composant ThirdScreen

```kotlin
@Composable
fun ThirdScreen(
    buttonValue: Int,
    onNavigateBack: () -> Unit
) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Valeur reçue : $buttonValue")
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = onNavigateBack) {
            Text("Retour")
        }
    }
}
```

- Le composant prend deux paramètres :
    - `buttonValue` : la valeur reçue de l'activité précédente
    - `onNavigateBack` : le callback pour retourner à l'écran précédent
- L'interface affiche la valeur reçue et un bouton de retour

## Points Importants à Noter

1. **Passage de Données**
    - Utilisation de `getIntExtra` pour récupérer la donnée passée
    - Correspond au `putExtra` vu dans le premier fichier
    - Important de spécifier une valeur par défaut

2. **Cycle de Communication**
    - L'activité principale envoie une valeur
    - La troisième activité la récupère
    - La valeur est affichée à l'écran

3. **Architecture**
    - Séparation claire entre la logique (récupération des données) et l'affichage
    - Le composant Composable reste pur et réutilisable

4. **String Template**
    - Utilisation de `$buttonValue` pour insérer la valeur dans le texte
    - Exemple simple d'interpolation de chaînes en Kotlin

Ce code complète bien les deux premiers fichiers en montrant comment :

- Recevoir des données d'une autre activité
- Afficher ces données dans l'interface
- Maintenir une architecture propre et modulaire
