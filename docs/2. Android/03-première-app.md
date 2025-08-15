# 3. Première application avec Jetpack Compose

Voici un exemple simple d'une application "Bonjour !" utilisant Jetpack Compose. Je vais vous
présenter le code, puis expliquer chaque élément important étape par étape.

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Box(modifier = Modifier.fillMaxSize()) {
                Text(
                    text = "Bonjour !",
                    modifier = Modifier.align(Alignment.Center)
                )
            }
        }
    }
}
```

Maintenant, expliquons les éléments importants de cette application étape par étape :

## Explication des éléments clés

### 1. Imports

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
```

- Ces imports sont nécessaires pour utiliser les classes et fonctions de Jetpack Compose.

### 2. Définition de l'activité principale

```kotlin
class MainActivity : ComponentActivity() {
    // ...
}
```

- `MainActivity` hérite de `ComponentActivity`, qui est la classe de base recommandée pour les activités utilisant
  Compose.

### 3. Méthode onCreate

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // ...
}
```

- Cette méthode est appelée lorsque l'activité est créée.

### 4. setContent

```kotlin
setContent {
    // Contenu Compose
}
```

- `setContent` est une fonction spécifique à Compose qui définit le contenu de l'interface utilisateur.
- Tout ce qui est à l'intérieur de cette fonction est du code Compose.

### 5. Box Layout

```kotlin
Box(modifier = Modifier.fillMaxSize()) {
    // Contenu du Box
}
```

- `Box` est un composant de mise en page qui permet d'empiler des éléments.
- `Modifier.fillMaxSize()` fait en sorte que le Box occupe tout l'espace disponible à l'écran.

### 6. Text Composable

```kotlin
Text(
    text = "Bonjour !",
    modifier = Modifier.align(Alignment.Center)
)
```

- `Text` est un composant Compose pour afficher du texte.
- `text = "Bonjour !"` définit le texte à afficher.
- `Modifier.align(Alignment.Center)` centre le texte dans le Box.

## Points clés à retenir

1. **Composables** : Dans Compose, l'interface utilisateur est construite à partir de fonctions composables (
   comme `Text`).

2. **Déclaratif** : Le code décrit ce qui doit être affiché, pas comment le construire étape par étape.

3. **Modifiers** : Les `Modifier` sont utilisés pour personnaliser l'apparence et le comportement des composants.

4. **Hiérarchie** : Les composants sont organisés de manière hiérarchique (ici, `Text` est à l'intérieur de `Box`).

5. **Pas de XML** : Contrairement au développement Android traditionnel, il n'y a pas de fichiers XML pour définir la
   mise en page.

Cette application simple affiche "Bonjour !" au centre de l'écran. C'est un point de départ idéal pour commencer à
explorer Jetpack Compose, car elle illustre les concepts de base sans trop de complexité.

## Définition de fonctions composables

Excellent ! Voici l'exemple modifié avec une fonction composable définie pour la `Box`, et son appel dans `onCreate`. Je
vais ensuite expliquer les modifications.

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            BonjourScreen()
        }
    }
}

@Composable
fun BonjourScreen() {
    Box(modifier = Modifier.fillMaxSize()) {
        Text(
            text = "Hello World!",
            modifier = Modifier.align(Alignment.Center)
        )
    }
}

@Preview(showBackground = true)
@Composable
fun BonjourPreview() {
    BonjourScreen()
}
```

Expliquons maintenant les modifications et leurs implications :

## Explications des modifications

### 1. Nouvelle fonction composable `BonjourScreen`

```kotlin
@Composable
fun BonjourScreen() {
    Box(modifier = Modifier.fillMaxSize()) {
        Text(
            text = "Hello World!",
            modifier = Modifier.align(Alignment.Center)
        )
    }
}
```

- Cette nouvelle fonction est annotée avec `@Composable`, indiquant qu'il s'agit d'une fonction composable Jetpack
  Compose.
- Elle encapsule la logique d'affichage qui était précédemment directement dans `setContent`.
- Cette approche améliore la réutilisabilité et la lisibilité du code.

### 2. Modification de `onCreate`

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent {
        BonjourScreen()
    }
}
```

- Au lieu de définir directement le contenu dans `setContent`, nous appelons maintenant notre fonction
  composable `BonjourScreen`.
- Cela rend le code de `onCreate` plus concis et plus clair.

### 3. Ajout d'une fonction de prévisualisation

```kotlin
@Preview(showBackground = true)
@Composable
fun BonjourPreview() {
    BonjourScreen()
}
```

- Cette nouvelle fonction est annotée avec `@Preview`, ce qui permet de prévisualiser le composant dans Android Studio
  sans avoir à exécuter l'application.
- `showBackground = true` ajoute un arrière-plan à la prévisualisation pour une meilleure visibilité.
- Elle appelle `BonjourScreen()`, permettant de voir exactement ce qui sera affiché dans l'application.

## Implications et avantages de ces modifications

1. **Séparation des préoccupations** : La logique d'affichage est maintenant séparée de la logique de l'activité, ce qui
   est une bonne pratique de programmation.

2. **Réutilisabilité** : `BonjourScreen` peut être facilement réutilisé ailleurs dans l'application si nécessaire.

3. **Testabilité** : Il est plus facile de tester `BonjourScreen` de manière isolée.

4. **Prévisualisation** : L'ajout de la fonction de prévisualisation permet aux développeurs de voir rapidement les
   changements d'interface sans avoir à compiler et exécuter l'application entière.

5. **Modularité** : Cette structure facilite l'ajout futur de fonctionnalités ou de modifications à l'interface
   utilisateur.

6. **Lisibilité** : Le code est plus clair et plus facile à comprendre, chaque partie ayant un rôle bien défini.

7. **Évolutivité** : À mesure que l'application se développe, cette structure permet d'ajouter facilement de nouveaux
   composants et écrans.

Cette approche est typique du développement avec Jetpack Compose, où l'interface utilisateur est construite à partir de
composants réutilisables et prévisualisables. C'est une base solide pour développer des applications plus complexes tout
en maintenant un code propre et organisé.



-------
??? info "Utilisation de l'IA"
    Page rédigée en partie avec l'aide d'un assistant IA. L'IA a été utilisée pour générer des 
    explications, des exemples et/ou des suggestions de structure. Toutes les informations ont 
    été vérifiées, éditées et complétées par l'auteur.