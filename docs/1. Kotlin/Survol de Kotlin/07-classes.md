# Classes

[Source](https://kotlinlang.org/docs/kotlin-tour-classes.html)

Kotlin prend en charge la programmation orientée objet avec des classes et des objets. Les objets sont utiles pour
stocker des données dans votre programme. Les classes vous permettent de déclarer un ensemble de caractéristiques pour
un objet. Lorsque vous créez des objets à partir d'une classe, vous pouvez gagner du temps et des efforts, car vous
n'avez pas à déclarer ces caractéristiques à chaque fois.

Pour déclarer une classe, utilisez le mot-clé `class` :

```kotlin
class Client
```

## Propriétés

Les caractéristiques d'un objet de classe peuvent être déclarées dans des propriétés. Vous pouvez déclarer des
propriétés pour une classe :

* Dans les parenthèses `()` après le nom de la classe.

```kotlin
class Contact(val id: Int, var email: String)
```

* Au sein du corps de la classe défini par des accolades `{}`.

```kotlin
class Contact(val id: Int, var email: String) {
    val category: String = ""
}
```

Nous vous recommandons de déclarer les propriétés en lecture seule (`val`) à moins qu'elles ne doivent être modifiées
après la création d'une instance de la classe.

Vous pouvez déclarer des propriétés sans `val` ou `var` dans les parenthèses, mais ces propriétés ne sont pas
accessibles après la création d'une instance.

> * Le contenu entre parenthèses `()` s'appelle l'**en-tête de la classe**.
> * Vous pouvez utiliser une [virgule finale](https://kotlinlang.org/docs/coding-conventions.html#trailing-commas) lors
    > de la déclaration des propriétés de classe.

Tout comme avec les paramètres de fonction, les propriétés de classe peuvent avoir des valeurs par défaut :

```kotlin
class Contact(val id: Int, var email: String = "example@gmail.com") {
    val category: String = "travail"
}
```

## Créer une instance

Pour créer un objet à partir d'une classe, vous déclarez une **instance** de classe à l'aide d'un **constructeur**.

Par défaut, Kotlin crée automatiquement un constructeur avec les paramètres déclarés dans l'en-tête de la classe.

Par exemple :

```kotlin
class Contact(val id: Int, var email: String)

fun main() {
    val contact = Contact(1, "mary@gmail.com")
}
```

Dans cet exemple :

* `Contact` est une classe.
* `contact` est une instance de la classe `Contact`.
* `id` et `email` sont des propriétés.
* `id` et `email` sont utilisés avec le constructeur par défaut pour créer `contact`.

Les classes Kotlin peuvent avoir de nombreux constructeurs, y compris ceux que vous définissez vous-même. Pour en savoir
plus sur la façon de déclarer
plusieurs constructeurs, voir [Constructeurs](https://kotlinlang.org/docs/classes.html#constructors).

## Accéder aux propriétés

Pour accéder à une propriété d'une instance, écrivez le nom de la propriété après le nom de l'instance, précédé d'un
point `.` :

```kotlin
class Contact(val id: Int, var email: String)

fun main() {
    val contact = Contact(1, "mary@gmail.com")

    // Affiche la valeur de la propriété : email
    println(contact.email)
    // mary@gmail.com

    // Met à jour la valeur de la propriété : email
    contact.email = "jane@gmail.com"

    // Affiche la nouvelle valeur de la propriété : email
    println(contact.email)
    // jane@gmail.com
}
```

> Pour concaténer la valeur d'une propriété dans le cadre d'une chaîne de caractères, vous pouvez utiliser des modèles
> de chaîne de caractères (`$`).
> Par exemple :
> ```kotlin
> println("Leur adresse email est : ${contact.email}")
> ```
>

## Fonctions membres

En plus de déclarer des propriétés dans le cadre des caractéristiques d'un objet, vous pouvez également définir le
comportement d'un objet
avec des fonctions membres.

En Kotlin, les fonctions membres doivent être déclarées à l'intérieur du corps de la classe. Pour appeler une fonction
membre sur une instance, écrivez le
nom de la fonction après le nom de l'instance, précédé d'un point `.`. Par exemple :

```kotlin
class Contact(val id: Int, var email: String) {
    fun printId() {
        println(id)
    }
}

fun main() {
    val contact = Contact(1, "mary@gmail.com")
    // Appelle la fonction membre printId()
    contact.printId()
    // 1
}
```

## Classes de données

Kotlin dispose de **classes de données** particulièrement utiles pour stocker des données. Les classes de données ont la
même fonctionnalité que
les classes, mais elles viennent automatiquement avec des fonctions membres supplémentaires. Ces fonctions membres vous
permettent de facilement imprimer
l'instance en sortie lisible, de comparer les instances d'une classe, de copier les instances, et plus encore. Comme ces
fonctions sont
automatiquement disponibles, vous n'avez pas à passer de temps à écrire le même code standard pour chacune de vos
classes.

Pour déclarer une classe de données, utilisez le mot-clé `data` :

```kotlin
data class Utilisateur(val nom: String, val id: Int)
```

Les fonctions membres prédéfinies les plus utiles des classes de données sont :

| **Fonction**        | **Description**                                                                                    |
|---------------------|----------------------------------------------------------------------------------------------------|
| `.toString()`       | Imprime une chaîne de caractères lisible de l'instance de la classe et ses propriétés.             |
| `.equals()` or `==` | Compare les instances d'une classe.                                                                |
| `.copy()`           | Crée une instance de classe en copiant une autre, potentiellement avec des propriétés différentes. |

Voir les sections suivantes pour des exemples d'utilisation de chaque fonction :

* Imprimer comme une chaîne
* Comparer les instances
* Copier une instance

### Imprimer comme une chaîne

Pour imprimer une chaîne de caractères lisible d'une instance de classe, vous pouvez appeler explicitement la
fonction `.toString()`, ou utiliser des fonctions d'impression
(`println()` et `print()`) qui appellent automatiquement `.toString()` pour vous :

```kotlin
class User(val nom: String, val id: Int)

fun main() {
    //sampleStart
    val utilisateur = User("Alex", 1)

    // Utilise automatiquement la fonction toString() afin que la sortie soit facile à lire
    println(utilisateur)
    // User(nom=Alex, id=1)
    //sampleEnd
}
```

Cela est particulièrement utile lors du débogage ou de la création de journaux.

### Comparer les instances

Pour comparer les instances de classes de données, utilisez l'opérateur d'égalité `==` :

```kotlin
data class User(val name: String, val id: Int)

fun main() {
    //sampleStart
    val utilisateur = User("Alex", 1)
    val secondUtilisateur = User("Alex", 1)
    val troisiemeUtilisateur = User("Max", 2)

    // Compare l'utilisateur au deuxième utilisateur
    println("utilisateur == secondUtilisateur : ${utilisateur == secondUtilisateur}")
    // utilisateur == secondUtilisateur : true

    // Compare l'utilisateur au troisième utilisateur
    println("utilisateur == troisiemeUtilisateur : ${utilisateur == troisiemeUtilisateur}")
    // utilisateur == troisiemeUtilisateur : false
    //sampleEnd
}
```

### Copier une instance

Pour créer une copie exacte d'une instance de classe de données, appelez la fonction `.copy()` sur l'instance.

Pour créer une copie d'une instance de classe de données **et** changer certaines propriétés, appelez la
fonction `.copy()` sur l'instance
**et** ajoutez des valeurs de remplacement pour les propriétés en tant que paramètres de fonction.

Par exemple :

```kotlin
data class User(val name: String, val id: Int)

fun main() {
    //sampleStart
    val utilisateur = User("Alex", 1)
    val secondUtilisateur = User("Alex", 1)
    val troisiemeUtilisateur = User("Max", 2)

    // Crée une copie exacte de l'utilisateur
    println(utilisateur.copy())
    // User(name=Alex, id=1)

    // Crée une copie de l'utilisateur avec le nom : "Max"
    println(utilisateur.copy("Max"))
    // User(name=Max, id=1)

    // Crée une copie de l'utilisateur avec id : 3
    println(utilisateur.copy(id = 3))
    // User(name=Alex, id=3)
    //sampleEnd
}
```

Créer une copie d'une instance est plus sûr que de modifier l'instance d'origine, car tout code qui dépend de l'instance
d'origine n'est pas affecté par la copie et ce que vous en faites.

Pour plus d'informations sur les classes de données,
voir [Classes de données](https://kotlinlang.org/docs/data-classes.html).

Le dernier chapitre de ce tour est à propos de la [sécurité contre null](08-valeurs-nulles.md) de Kotlin.

## Pratique

### Exercice 1

Définissez une classe de données `Employé` avec deux propriétés : une pour un nom, et une autre pour un salaire.
Assurez-vous que la propriété
pour le salaire est modifiable, sinon vous n'obtiendrez pas d'augmentation de salaire à la fin de l'année ! La fonction
principale démontre comment
vous pouvez utiliser cette classe de données.

```kotlin
// Écrivez votre code ici

fun main() {
    val emp = Employe("Mary", 20)
    println(emp)
    emp.salaire += 10
    println(emp)
}
```

<details>
    <summary>Réponse</summary>

```kotlin
data class Employé(val nom: String, var salaire: Int)

fun main() {
    val emp = Employé("Mary", 20)
    println(emp)
    emp.salaire += 10
    println(emp)
}
```

</details>
<br>

### Exercice 2

Pour tester votre code, vous avez besoin d'un générateur capable de créer des employés aléatoires. Définissez une classe
avec une liste fixe de noms potentiels
(à l'intérieur du corps de la classe), et qui est configurée par un salaire minimum et maximum (à l'intérieur de
l'en-tête de la classe). Encore une fois, la fonction principale démontre comment vous pouvez utiliser cette classe.

<details>
    <summary>Indice</summary>

Les listes ont une fonction d'extension
appelée <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/random.html"><code>.random()</code></a>
qui renvoie un élément aléatoire dans une liste.

</details>
<br>

<details>
    <summary>Indice</summary>
    
        <code>Random.nextInt(from = ..., until = ...)</code> vous donne un nombre aléatoire de type <code>Int</code> dans des limites spécifiées.
</details>
<br>

```kotlin
import kotlin.random.Random

data class Employe(val nom: String, var salaire: Int)

// Écrivez votre code ici

fun main() {
    val empGen = GenerateurEmployeAleatoire(10, 30)
    println(empGen.genererEmploye())
    println(empGen.genererEmploye())
    println(empGen.genererEmploye())
    empGen.salaireMin = 50
    empGen.salaireMax = 100
    println(empGen.genererEmploye())
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
import kotlin.random.Random
data class Employe(val nom: String, var salaire: Int)
class GenerateurEmployeAleatoire(var salaireMin: Int, var salaireMax: Int) {
    val noms = listOf("John", "Mary", "Ann", "Paul", "Jack", "Elizabeth")
    fun genererEmploye() = Employe(noms.random(), Random.nextInt(from = salaireMin, until = salaireMax))
}
fun main() {
    val empGen = GenerateurEmployeAleatoire(10, 30)
    println(empGen.genererEmploye())
    println(empGen.genererEmploye())
    println(empGen.genererEmploye())
    empGen.salaireMin = 50
    empGen.salaireMax = 100
    println(empGen.genererEmploye())
}
```
</details>
<br>