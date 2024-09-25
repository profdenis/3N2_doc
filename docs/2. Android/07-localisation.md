# 7. Localisation

Voici un guide pour intégrer la localisation dans une application Android développée avec Jetpack Compose :

## Principes généraux de la localisation

La localisation d'une application Android consiste à adapter son contenu et son interface utilisateur à différentes
langues et cultures. Voici les principes clés à suivre :

1. Externalisez toutes les chaînes de caractères dans des fichiers de ressources.
2. Utilisez des identifiants neutres pour les ressources (ex: "welcome_message" au lieu de "english_welcome").
3. Évitez de coder en dur du texte dans votre code.
4. Tenez compte des différences culturelles (formats de date, unités de mesure, etc.).
5. Prévoyez de l'espace supplémentaire dans votre interface pour les traductions plus longues.
6. Testez votre application dans différentes langues et configurations.

## Exemple simple avec des `String` en anglais et français

### Étape 1 : Configurer les ressources linguistiques

1- Dans le dossier `res`, créez un dossier `values-fr` pour les ressources en français.

2- Dans `res/values/strings.xml` (anglais par défaut) :
```xml
    <resources>
        <string name="app_name">My App</string>
        <string name="welcome_message">Welcome to My App!</string>
        <string name="language_selection">Select a language</string>
    </resources>
```

3- Dans `res/values-fr/strings.xml` :
```xml
<resources>
   <string name="app_name">Mon Application</string>
   <string name="welcome_message">Bienvenue dans Mon Application !</string>
   <string name="language_selection">Sélectionnez une langue</string>
</resources>
```

### Étape 2 : Utiliser les chaînes localisées dans Jetpack Compose

```kotlin
@Composable
fun WelcomeScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(
            text = stringResource(R.string.welcome_message),
            style = MaterialTheme.typography.h4
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            text = stringResource(R.string.language_selection),
            style = MaterialTheme.typography.body1
        )
    }
}
```

## Formatage des dates

Pour formater les dates en fonction de la locale de l'utilisateur, utilisez la classe `DateTimeFormatter` avec
`ofLocalizedDate()` :

```kotlin
import java.time.LocalDate
import java.time.format.DateTimeFormatter
import java.time.format.FormatStyle

@Composable
fun DisplayLocalizedDate() {
    val currentDate = LocalDate.now()
    val dateFormatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG)
    val formattedDate = remember(currentDate) {
        currentDate.format(dateFormatter)
    }

    Text(
        text = formattedDate,
        style = MaterialTheme.typography.body1
    )
}
```

Ce code affichera la date actuelle dans le format long approprié pour la locale de l'utilisateur. Par exemple :

- En anglais (US) : "September 18, 2024"
- En français : "18 septembre 2024"

En suivant ces étapes, votre application Jetpack Compose sera correctement localisée en anglais et en français, avec la
possibilité d'ajouter facilement d'autres langues à l'avenir[1][2][4].

Citations:

- [1] [https://www.translized.com/blog/android-localization-with-jetpack-compose---a-comprehensive-guide](https://www.translized.com/blog/android-localization-with-jetpack-compose---a-comprehensive-guide)
- [2] [https://phrase.com/blog/posts/localized-date-time-android/](https://phrase.com/blog/posts/localized-date-time-android/)
- [3] [https://www.youtube.com/watch?v=VdwDawvfH98](https://www.youtube.com/watch?v=VdwDawvfH98)
- [4] [https://phrase.com/blog/posts/internationalizing-jetpack-compose-android-apps/](https://phrase.com/blog/posts/internationalizing-jetpack-compose-android-apps/)
- [5] [https://www.adamormsby.com/posts/013-android-localization-formatting-dates/](https://www.adamormsby.com/posts/013-android-localization-formatting-dates/)

## Exemple complet

Voici un exemple complet d'une application simple utilisant Jetpack Compose avec localisation, sans les
boutons pour changer la langue. Cette application affichera un message de bienvenue localisé et la date actuelle
formatée selon la locale de l'appareil.

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.unit.dp
import java.time.LocalDate
import java.time.format.DateTimeFormatter
import java.time.format.FormatStyle

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyApp()
        }
    }
}

@Composable
fun MyApp() {
    MaterialTheme {
        Surface(
            modifier = Modifier.fillMaxSize(),
            color = MaterialTheme.colors.background
        ) {
            LocalizedContent()
        }
    }
}

@Composable
fun LocalizedContent() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(
            text = stringResource(R.string.welcome_message),
            style = MaterialTheme.typography.h4
        )

        Spacer(modifier = Modifier.height(16.dp))

        DisplayLocalizedDate()
    }
}

@Composable
fun DisplayLocalizedDate() {
    val currentDate = remember { LocalDate.now() }
    val dateFormatter = remember { DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG) }
    val formattedDate = remember(currentDate) {
        currentDate.format(dateFormatter)
    }

    Text(
        text = formattedDate,
        style = MaterialTheme.typography.body1
    )
}
```

Pour que cette application fonctionne correctement, vous devez également configurer les fichiers de ressources
suivants :

- Dans `res/values/strings.xml` (anglais par défaut) :

```xml
<resources>
    <string name="app_name">My Localized App</string>
    <string name="welcome_message">Welcome to My Localized App!</string>
</resources>
```

- Dans `res/values-fr/strings.xml` (pour le français) :

```xml
<resources>
    <string name="app_name">Mon Application Localisée</string>
    <string name="welcome_message">Bienvenue dans Mon Application Localisée !</string>
</resources>
```

- Assurez-vous d'avoir les dépendances nécessaires dans votre fichier `build.gradle` (Module : app) :

```gradle
dependencies {
    implementation "androidx.compose.ui:ui:1.5.0"
    implementation "androidx.compose.material:material:1.5.0"
    implementation "androidx.compose.ui:ui-tooling-preview:1.5.0"
    implementation "androidx.activity:activity-compose:1.7.2"
}
```

Cette application simple affiche :

1. Un message de bienvenue localisé
2. La date actuelle formatée selon la locale de l'appareil

L'application utilisera automatiquement les ressources appropriées en fonction de la langue configurée sur l'appareil de
l'utilisateur. Si l'appareil est configuré en français, il utilisera les chaînes de caractères du fichier
`values-fr/strings.xml`. Pour toute autre langue, il utilisera les chaînes par défaut du fichier `values/strings.xml`.

Pour tester différentes langues, vous pouvez changer la langue de votre appareil ou de l'émulateur dans les paramètres
système. L'application s'adaptera automatiquement à la nouvelle langue sans nécessiter de redémarrage.
915445



-------
<small>
   <cite>
      **Note** : _Page rédigée en partie avec l'aide d'un assistant IA, principalement
      à l'aide de Perplexity AI, avec les _LLM_ `GPT-4 Omni` et `Claude 3.5 Sonnet`. L'IA
      a été utilisée pour générer des explications, des exemples et des suggestions de
      structure. Toutes les informations ont été vérifiées, éditées et complétées par
      l'auteur._
   </cite>
</small>