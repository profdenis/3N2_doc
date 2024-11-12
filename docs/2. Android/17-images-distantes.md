# 17. Affichage d'images et de vidéos distantes

[Dépôt Git](https://github.com/profdenis/ImagesVideos)

## Affichage d'une image depuis une URL dans Jetpack Compose

### Prérequis

Pour afficher une image depuis une URL, nous devons :

1. Ajouter la dépendance Coil dans le fichier `build.gradle` (niveau app)
2. Ajouter la permission Internet dans le `AndroidManifest.xml`

```gradle
// build.gradle
dependencies {
    implementation("io.coil-kt:coil-compose:2.5.0")
}
```

```xml
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.INTERNET"/>
```

### Composable de base

Voici un exemple simple d'utilisation :

```kotlin
import androidx.compose.foundation.layout.size
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import coil.compose.AsyncImage

@Composable
fun ImageDistante() {
    AsyncImage(
        model = "https://example.com/image.jpg",
        contentDescription = "Description de l'image",
        modifier = Modifier.size(200.dp)
    )
}
```

### Version plus complète avec gestion du chargement

```kotlin
import androidx.compose.foundation.layout.*
import androidx.compose.material3.CircularProgressIndicator
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.unit.dp
import coil.compose.SubcomposeAsyncImage

@Composable
fun ImageDistanteAvancee() {
    SubcomposeAsyncImage(
        model = "https://example.com/image.jpg",
        contentDescription = "Description de l'image",
        modifier = Modifier.size(200.dp),
        contentScale = ContentScale.Fit,
        loading = {
            Box(
                modifier = Modifier.fillMaxSize(),
                contentAlignment = Alignment.Center
            ) {
                CircularProgressIndicator()
            }
        },
        error = {
            // Vous pouvez personnaliser l'affichage en cas d'erreur
            Box(
                modifier = Modifier.fillMaxSize(),
                contentAlignment = Alignment.Center
            ) {
                Text("Erreur de chargement")
            }
        }
    )
}
```

### Points importants

1. **Coil** est une bibliothèque de chargement d'images pour Android, optimisée pour Kotlin et Jetpack Compose.

2. Deux composables principaux sont disponibles :
    - `AsyncImage` : version simple pour les cas basiques
    - `SubcomposeAsyncImage` : version avancée permettant de gérer les états de chargement

3. Les paramètres principaux :
    - `model` : l'URL de l'image
    - `contentDescription` : description pour l'accessibilité
    - `modifier` : pour personnaliser la taille et l'apparence
    - `contentScale` : pour définir comment l'image doit s'adapter à son conteneur

4. Pour tester, voici quelques URLs d'images libres de droits :

```kotlin
"https://picsum.photos/200"  // Image aléatoire de 200x200
"https://via.placeholder.com/200"  // Image placeholder de 200x200
```

### Exemple d'utilisation complet

```kotlin
@Composable
fun ExempleImageScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Image simple
        AsyncImage(
            model = "https://picsum.photos/200",
            contentDescription = "Image aléatoire",
            modifier = Modifier.size(200.dp)
        )

        // Image avec gestion du chargement
        SubcomposeAsyncImage(
            model = "https://picsum.photos/300",
            contentDescription = "Image aléatoire avec loading",
            modifier = Modifier.size(300.dp),
            loading = { CircularProgressIndicator() }
        )
    }
}
```

## Intégration de vidéos dans Jetpack Compose

### Lecture de vidéos MP4 avec ExoPlayer

Pour lire des vidéos MP4, nous utiliserons ExoPlayer, qui est recommandé par Google pour la lecture de médias sur
Android.

Premièrement, ajoutez les dépendances dans `build.gradle` :

```gradle
dependencies {
    implementation("androidx.media3:media3-exoplayer:1.2.0")
    implementation("androidx.media3:media3-ui:1.2.0")
}
```

Ensuite, créez le composable pour la vidéo :

```kotlin
import androidx.compose.runtime.*
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.viewinterop.AndroidView
import androidx.media3.common.MediaItem
import androidx.media3.common.Player
import androidx.media3.exoplayer.ExoPlayer
import androidx.media3.ui.PlayerView

@Composable
fun VideoPlayer(
    videoUrl: String,
    modifier: Modifier = Modifier
) {
    val context = LocalContext.current

    // Création de l'ExoPlayer
    val exoPlayer = remember {
        ExoPlayer.Builder(context).build().apply {
            setMediaItem(MediaItem.fromUri(videoUrl))
            prepare()
        }
    }

    // Gestion du cycle de vie
    DisposableEffect(Unit) {
        onDispose {
            exoPlayer.release()
        }
    }

    // Interface utilisateur du lecteur
    AndroidView(
        factory = { context ->
            PlayerView(context).apply {
                player = exoPlayer
            }
        },
        modifier = modifier
    )
}
```

Utilisation :

```kotlin
@Composable
fun VideoPlayerScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        // Exemple avec une vidéo MP4
        VideoPlayer(
            videoUrl = "https://example.com/video.mp4",
            modifier = Modifier
                .fillMaxWidth()
                .aspectRatio(16f / 9f)
        )
    }
}
```

### Version complète avec gestion des erreurs et du chargement

```kotlin
@Composable
fun VideoPlayerAvance(
    videoUrl: String,
    modifier: Modifier = Modifier
) {
    var isLoading by remember { mutableStateOf(true) }
    var hasError by remember { mutableStateOf(false) }
    val context = LocalContext.current

    val exoPlayer = remember {
        ExoPlayer.Builder(context).build().apply {
            addListener(object : Player.Listener {
                override fun onPlaybackStateChanged(state: Int) {
                    when (state) {
                        Player.STATE_READY -> isLoading = false
                        Player.STATE_ENDED -> { /* Gérer la fin */
                        }
                        Player.STATE_BUFFERING -> isLoading = true
                        Player.STATE_IDLE -> { /* État initial */
                        }
                    }
                }

                override fun onPlayerError(error: PlaybackException) {
                    hasError = true
                    isLoading = false
                }
            })

            setMediaItem(MediaItem.fromUri(videoUrl))
            prepare()
        }
    }

    DisposableEffect(Unit) {
        onDispose {
            exoPlayer.release()
        }
    }

    Box(modifier = modifier) {
        AndroidView(
            factory = { context ->
                PlayerView(context).apply {
                    player = exoPlayer
                }
            },
            modifier = Modifier.matchParentSize()
        )

        if (isLoading) {
            CircularProgressIndicator(
                modifier = Modifier.align(Alignment.Center)
            )
        }

        if (hasError) {
            Text(
                text = "Erreur de lecture de la vidéo",
                modifier = Modifier.align(Alignment.Center),
                color = Color.Red
            )
        }
    }
}
```

### Points importants

1. Il faut ajouter la permission Internet dans le `AndroidManifest.xml` :
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   ```

2. Pour ExoPlayer :
    - C'est la solution recommandée par Google
    - Supporte de nombreux formats vidéo
    - Gère automatiquement la mise en cache
    - Offre des contrôles de lecture avancés

3. Considérations de performances :
    - Les vidéos consomment beaucoup de ressources
    - Important de bien gérer le cycle de vie
    - Prévoir la gestion du cache et de la bande passante

