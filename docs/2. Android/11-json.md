# 11. Lire et écrire des fichiers JSON

## Enregistrement et lecture de données JSON en Kotlin

### Étape 1 : Préparation

Prenons par exemple la classe `Player` est définie comme suit :

```kotlin
data class Player(
    val id: Int,
    val number: Int,
    val name: String,
    val image_resource: Int = R.drawable.nhl_logo
)
```

Nous utiliserons la bibliothèque `Gson` pour la sérialisation et la désérialisation JSON. Ajoutez la dépendance suivante
dans le fichier `build.gradle` du module directement dans la section dépendances du fichier `app/build.gradle.kts` :

```kotlin
implementation("com.google.code.gson:gson:2.10.1")
```

Une autre option offerte par Android Studio est de convertir cette dépendance au format utilisant les catalogues de
versions (_Version Catalogs_). Vous pouvez accepter ce changement de format. La ligne plus haut sera remplacée par :

```kotlin
implementation(libs.gson)
```

Et deux lignes seront ajoutées `gradle/libs.versions.toml` pour `gson` :

```toml
[versions]
gson = "2.10.1"

[libraries]
gson = { module = "com.google.code.gson:gson", version.ref = "gson" }
```

### Étape 2 : Enregistrement des données dans un fichier JSON

Pour enregistrer une liste de `Player` dans un fichier JSON, nous allons créer une fonction d'extension :

```kotlin
import com.google.gson.Gson
import java.io.File

fun List<Player>.saveToFile(filename: String) {
    val jsonString = Gson().toJson(this)
    File(filename).writeText(jsonString)
}
```

Cette fonction convertit la liste de `Player` en une chaîne JSON, puis l'écrit dans un fichier.

### Étape 3 : Lecture des données à partir d'un fichier JSON

Pour lire la liste de `Player` à partir d'un fichier JSON, nous allons créer une autre fonction :

```kotlin
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import java.io.File

fun readPlayersFromFile(filename: String): List<Player> {
    val jsonString = File(filename).readText()
    val playerListType = object : TypeToken<List<Player>>() {}.type
    return Gson().fromJson(jsonString, playerListType)
}
```

Cette fonction lit le contenu du fichier JSON, puis le convertit en une liste de `Player`.

### Étape 4 : Utilisation des fonctions

Voici un exemple d'utilisation de ces fonctions :

```kotlin
fun main() {
    // Création d'une liste de joueurs
    val players = listOf(
        Player(1, 99, "Wayne Gretzky"),
        Player(2, 87, "Sidney Crosby"),
        Player(3, 9, "Bobby Orr")
    )

    // Enregistrement des joueurs dans un fichier JSON
    players.saveToFile("players.json")
    println("Joueurs enregistrés dans players.json")

    // Lecture des joueurs à partir du fichier JSON
    val loadedPlayers = readPlayersFromFile("players.json")
    println("Joueurs chargés à partir de players.json:")
    loadedPlayers.forEach { println(it) }
}
```

### Points importants

1. **Sérialisation** : C'est le processus de conversion d'objets en format JSON.
2. **Désérialisation** : C'est le processus inverse, convertissant le JSON en objets Kotlin.
3. **Gestion des fichiers** : Kotlin offre des méthodes simples pour lire et écrire des fichiers.
4. **Bibliothèque Gson** : Elle simplifie grandement le travail avec JSON en Kotlin.
5. **Data class** : L'utilisation de `data class` pour Player facilite la sérialisation/désérialisation.

## Gestion des fichiers dans Android

### Écrasement et création de fichiers

1. **Écrasement du fichier existant**
   Lorsque vous utilisez la méthode `File(filename).writeText(jsonString)`, si le fichier spécifié existe déjà, son
   contenu sera effectivement écrasé. Cette opération remplace entièrement le contenu précédent par les nouvelles
   données.

2. **Création d'un nouveau fichier**
   Si le fichier spécifié n'existe pas, Android le créera automatiquement avant d'y écrire les données. Cela signifie
   que vous n'avez pas besoin de vérifier l'existence du fichier ou de le créer manuellement.

### Emplacement du fichier

L'emplacement du fichier dépend du chemin que vous spécifiez dans le paramètre `filename`. Dans le contexte d'une
application Android, il est crucial de comprendre les différentes options de stockage :

1. **Stockage interne**
    - Par défaut, si vous utilisez un chemin relatif, le fichier sera créé dans le répertoire privé de votre
      application.
    - Chemin typique : `/data/data/[nom_du_package_de_votre_app]/files/`
    - Ce répertoire est privé et accessible uniquement par votre application.

2. **Stockage externe**
    - Pour écrire sur le stockage externe (comme la carte SD), vous devez demander les permissions appropriées et
      utiliser un chemin absolu (voir une autre section).
    - Exemple : `Environment.getExternalStorageDirectory().absolutePath + "/MonDossier/monfichier.json"`

### Bonnes pratiques pour les applications Android

#### Utiliser le contexte de l'application

Pour une meilleure gestion des fichiers dans une application Android, il est recommandé d'utiliser le contexte de
l'application pour obtenir le répertoire approprié :

```kotlin
fun Context.saveToFile(players: List<Player>, filename: String) {
    val file = File(this.filesDir, filename)
    file.writeText(Gson().toJson(players))
}

fun Context.readPlayersFromFile(filename: String): List<Player> {
    val file = File(this.filesDir, filename)
    val jsonString = file.readText()
    val playerListType = object : TypeToken<List<Player>>() {}.type
    return Gson().fromJson(jsonString, playerListType)
}
```

#### Gestion des erreurs

Il est important d'ajouter une gestion des erreurs pour traiter les cas où l'écriture ou la lecture pourrait
échouer :

```kotlin
fun Context.saveToFile(players: List<Player>, filename: String) {
    try {
        val file = File(this.filesDir, filename)
        file.writeText(Gson().toJson(players))
    } catch (e: Exception) {
        Log.e("FileIO", "Erreur lors de l'écriture du fichier", e)
    }
}
```

#### Vérification de l'existence du fichier

Avant de lire un fichier, il est judicieux de vérifier son existence :

```kotlin
fun Context.readPlayersFromFile(filename: String): List<Player>? {
    val file = File(this.filesDir, filename)
    return if (file.exists()) {
        try {
            val jsonString = file.readText()
            val playerListType = object : TypeToken<List<Player>>() {}.type
            Gson().fromJson(jsonString, playerListType)
        } catch (e: Exception) {
            Log.e("FileIO", "Erreur lors de la lecture du fichier", e)
            emptyList()
        }
    } else {
        emptyList()
    }
}
```