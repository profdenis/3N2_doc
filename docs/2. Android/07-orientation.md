# 7. Orientation de l'écran

Pour détecter l'orientation de l'écran (portrait ou paysage) avec Jetpack Compose, vous pouvez utiliser le
`LocalConfiguration` composable. Voici comment procéder :

1- Importez les dépendances nécessaires :

```kotlin
import android.content.res.Configuration
import androidx.compose.runtime.Composable
import androidx.compose.ui.platform.LocalConfiguration
```

2- Utilisez le `LocalConfiguration` dans votre composable pour obtenir la configuration actuelle :

```kotlin
@Composable
fun MyScreen() {
    val configuration = LocalConfiguration.current

    when (configuration.orientation) {
        Configuration.ORIENTATION_LANDSCAPE -> {
            // Code pour l'orientation paysage
            Text("L'écran est en mode paysage")
        }
        else -> {
            // Code pour l'orientation portrait
            Text("L'écran est en mode portrait")
        }
    }
}
```

3- Pour observer les changements d'orientation, vous pouvez utiliser un `State` :

```kotlin
@Composable
fun OrientationAwareLayout() {
    val configuration = LocalConfiguration.current
    val orientation by remember { mutableStateOf(configuration.orientation) }

    LaunchedEffect(configuration) {
        snapshotFlow { configuration.orientation }
            .collect { orientation = it }
    }

    when (orientation) {
        Configuration.ORIENTATION_LANDSCAPE -> {
            // Mise en page pour le mode paysage
        }
        else -> {
            // Mise en page pour le mode portrait
        }
    }
}
```

Cette approche permet à votre composable de se recomposer automatiquement lorsque l'orientation change[2].

En utilisant ces méthodes, vous pouvez créer des interfaces utilisateur réactives qui s'adaptent à l'orientation de
l'écran. Cela est particulièrement utile pour optimiser l'expérience utilisateur sur différents appareils et dans
différentes configurations d'écran[1][2].

Citations:

- [1] [https://developer.android.com/guide/practices/device-compatibility-mode?hl=fr](https://developer.android.com/guide/practices/device-compatibility-mode?hl=fr)
- [2] [https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/](https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/)
- [3] [https://blog.ippon.fr/2023/04/28/developper-app-jetpack-compose-smartphones-pliables/](https://blog.ippon.fr/2023/04/28/developper-app-jetpack-compose-smartphones-pliables/)
- [4] [https://developer.android.com/develop/ui/compose/touch-input/pointer-input/drag-swipe-fling?hl=fr](https://developer.android.com/develop/ui/compose/touch-input/pointer-input/drag-swipe-fling?hl=fr)
- [5] [https://developer.android.com/develop/ui/compose/touch-input/stylus-input/advanced-stylus-features?hl=fr](https://developer.android.com/develop/ui/compose/touch-input/stylus-input/advanced-stylus-features?hl=fr)
- [6] [https://stackoverflow.com/questions/64753944/orientation-on-jetpack-compose](https://stackoverflow.com/questions/64753944/orientation-on-jetpack-compose)
- [7] [https://appmaster.io/fr/blog/comment-creer-une-interface-utilisateur-adaptative-avec-jetpack-compose](https://appmaster.io/fr/blog/comment-creer-une-interface-utilisateur-adaptative-avec-jetpack-compose)


## Exemple complet

```kotlin
import android.content.res.Configuration
import androidx.compose.foundation.layout.*
import androidx.compose.material3.Button
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Text
import androidx.compose.material3.TextField
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalConfiguration
import androidx.compose.ui.unit.dp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun OrientationResponsiveLayout(modifier: Modifier = Modifier) {
    var text1 by remember { mutableStateOf("") }
    var text2 by remember { mutableStateOf("") }
    
    val configuration = LocalConfiguration.current
    val isLandscape = configuration.orientation == Configuration.ORIENTATION_LANDSCAPE

    if (isLandscape) {
        Column(
            modifier = modifier
                .fillMaxSize()
                .padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                TextField(
                    value = text1,
                    onValueChange = { text1 = it },
                    label = { Text("Champ 1") },
                    modifier = Modifier.weight(1f).padding(end = 8.dp)
                )
                TextField(
                    value = text2,
                    onValueChange = { text2 = it },
                    label = { Text("Champ 2") },
                    modifier = Modifier.weight(1f).padding(start = 8.dp)
                )
            }
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { /* Action du bouton */ }) {
                Text("Valider")
            }
        }
    } else {
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.spacedBy(16.dp)
        ) {
            TextField(
                value = text1,
                onValueChange = { text1 = it },
                label = { Text("Champ 1") },
                modifier = Modifier.fillMaxWidth()
            )
            TextField(
                value = text2,
                onValueChange = { text2 = it },
                label = { Text("Champ 2") },
                modifier = Modifier.fillMaxWidth()
            )
            Button(onClick = { /* Action du bouton */ }) {
                Text("Valider")
            }
        }
    }
}
```

Dans cet exemple :

1. Nous utilisons `LocalConfiguration.current` pour obtenir la configuration actuelle de l'écran[5].

2. Nous vérifions si l'orientation est en mode paysage en comparant `configuration.orientation` avec `Configuration.ORIENTATION_LANDSCAPE`[5].

3. En mode portrait (par défaut) :
   - Nous utilisons une `Column` pour agencer verticalement deux `TextField` et un `Button`[5].
   - Les éléments sont espacés uniformément grâce à `verticalArrangement = Arrangement.spacedBy(16.dp)`.

4. En mode paysage :
   - Nous utilisons une `Column` principale pour l'agencement global.
   - À l'intérieur, nous utilisons une `Row` pour placer les deux `TextField` côte à côte.
   - Le `Button` est placé en dessous de la `Row` avec un `Spacer` pour ajouter un espacement.

5. Les `TextField` utilisent `Modifier.weight(1f)` en mode paysage pour occuper un espace égal dans la `Row`[2].

6. Nous utilisons `remember` et `mutableStateOf` pour gérer l'état des champs de texte, permettant ainsi à l'utilisateur d'interagir avec eux[5].

Ce composable s'adaptera automatiquement à l'orientation de l'écran, offrant une mise en page optimisée pour les modes portrait et paysage. Il démontre comment créer des interfaces utilisateur réactives qui s'adaptent aux différentes configurations d'écran en utilisant Jetpack Compose.

Citations:

- [1] [https://developer.android.com/develop/ui/compose/layouts/adaptive](https://developer.android.com/develop/ui/compose/layouts/adaptive)
- [2] [https://www.blog.finotes.com/post/creating-responsive-layouts-in-android-using-jetpack-compose](https://www.blog.finotes.com/post/creating-responsive-layouts-in-android-using-jetpack-compose)
- [3] [https://stackoverflow.com/questions/67157309/how-to-create-responsive-layouts-with-jetpack-compose](https://stackoverflow.com/questions/67157309/how-to-create-responsive-layouts-with-jetpack-compose)
- [4] [https://composables.com/jetpack-compose-tutorials/responsive-layout](https://composables.com/jetpack-compose-tutorials/responsive-layout)
- [5] [https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/](https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/)
- [6] [https://eevis.codes/blog/2024-07-18/dont-lock-the-screen-orientation-handling-orientation-in-compose/](https://eevis.codes/blog/2024-07-18/dont-lock-the-screen-orientation-handling-orientation-in-compose/)


-------
??? info "Utilisation de l'IA"
    Page rédigée en partie avec l'aide d'un assistant IA. L'IA a été utilisée pour générer des 
    explications, des exemples et/ou des suggestions de structure. Toutes les informations ont 
    été vérifiées, éditées et complétées par l'auteur.