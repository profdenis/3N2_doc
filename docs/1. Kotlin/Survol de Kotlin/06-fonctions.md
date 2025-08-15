# Fonctions

[Source](https://kotlinlang.org/docs/kotlin-tour-functions.html)

Vous pouvez déclarer vos propres fonctions en Kotlin en utilisant le mot-clé `fun`.

```kotlin
fun hello() {
    return println("Bonjour, monde!")
}

fun main() {
    hello()
    // Bonjour, monde!
}
```

En Kotlin:

* les paramètres de fonction sont écrits entre parenthèses `()`.
* chaque paramètre doit avoir un type, et plusieurs paramètres doivent être séparés par des virgules `,` .
* le type de retour est écrit après les parenthèses de la fonction `()`, séparé par un deux-points `:`.
* le corps d'une fonction est écrit entre accolades `{}`.
* le mot-clé `return` est utilisé pour sortir ou retourner quelque chose d'une fonction.

!!! Note
    Si une fonction ne renvoie rien d'utile, le type de retour et le mot-clé `return` peuvent être omis.


Dans l'exemple suivant :

* `x` et `y` sont des paramètres de fonction.
* `x` et `y` ont le type `Int`.
* le type de retour de la fonction est `Int`.
* la fonction renvoie une somme de `x` et `y` lorsqu'elle est appelée.

```kotlin
fun sum(x: Int, y: Int): Int {
    return x + y
}

fun main() {
    println(sum(1, 2))
    // 3
}
```

!!! Note
    Nous recommandons dans nos [conventions de codage](https://kotlinlang.org/docs/coding-conventions.html#function-names)
    que vous nommiez les fonctions en commençant par une lettre minuscule et utilisez camel case sans tirets du bas.

## Arguments nommés

Pour un code concis, lors de l'appel de votre fonction, vous n'avez pas besoin d'inclure les noms des paramètres.
Cependant, l'inclusion des noms des paramètres
rend votre code plus facile à lire. C'est ce qu'on appelle l'utilisation des **arguments nommés**. Si vous incluez les
noms des paramètres, vous pouvez alors
écrire les paramètres dans n'importe quel ordre.

!!! Note
    Dans l'exemple suivant,
    les [templates de chaînes de caractères](https://kotlinlang.org/docs/strings.html#string-templates) (`$`) sont
    utilisés pour accéder aux valeurs des paramètres, les convertir en type `String`, puis les concaténer en une chaîne
    pour l'impression.


```kotlin
fun printMessageWithPrefix(message: String, prefix: String) {
    println("[$prefix] $message")
}

fun main() {
    // Utilise des arguments nommés avec l'ordre des paramètres inversé
    printMessageWithPrefix(prefix = "Log", message = "Bonjour")
    // [Log] Bonjour
}
```

## Valeurs par défaut des paramètres

Vous pouvez définir des valeurs par défaut pour vos paramètres de fonction. Tout paramètre ayant une valeur par défaut
peut être omis lors de
l'appel de votre fonction. Pour déclarer une valeur par défaut, utilisez l'opérateur d'affectation `=` après le type :

```kotlin
fun printMessageWithPrefix(message: String, prefix: String = "Info") {
    println("[$prefix] $message")
}

fun main() {
    // Fonction appelée avec les deux paramètres
    printMessageWithPrefix("Bonjour", "Log")
    // [Log] Bonjour

    // Fonction appelée uniquement avec le paramètre message
    printMessageWithPrefix("Bonjour")
    // [Info] Bonjour

    printMessageWithPrefix(prefix = "Log", message = "Bonjour")
    // [Log] Bonjour
}
```

!!! Note
    Vous pouvez omettre des paramètres spécifiques ayant des valeurs par défaut, plutôt que de tous les omettre.
    Cependant, après le
    premier paramètre omis, vous devez nommer tous les paramètres suivants.


## Fonctions sans retour 

Si votre fonction ne renvoie pas de valeur utile, alors son type de retour est `Unit`. `Unit` est un type avec une seule
valeur -
`Unit`. Vous n'avez pas à déclarer explicitement dans le corps de votre fonction que `Unit` est renvoyé. Cela signifie
que vous n'avez pas
à utiliser le mot-clé `return` ou déclarer un type de retour :

```kotlin
fun printMessage(message: String) {
    println(message)
    // `return Unit` ou `return` est optionnel
}

fun main() {
    printMessage("Bonjour")
    // Bonjour
}
```

## Fonctions à expression unique

Pour rendre votre code plus concis, vous pouvez utiliser des fonctions à expression unique. Par exemple, la
fonction `sum()` peut être raccourcie :

```kotlin
fun sum(x: Int, y: Int): Int {
    return x + y
}

fun main() {
    println(sum(1, 2))
    // 3
}
```

Vous pouvez retirer les accolades `{}` et déclarer le corps de la fonction en utilisant l'opérateur d'affectation `=`.
Et grâce à l'inférence de types de Kotlin,
vous pouvez également omettre le type de retour. La fonction `sum()` devient alors une ligne :

```kotlin
fun sum(x: Int, y: Int) = x + y

fun main() {
    println(sum(1, 2))
    // 3
}
```

!!! Note
    Omettre le type de retour n'est possible que lorsque votre fonction n'a pas de corps (`{}`). À moins que le type de
    retour de votre fonction
    ne soit `Unit`.


## Pratique des fonctions

### Exercice 1

Écrivez une fonction appelée `circleArea` qui prend le rayon d'un cercle au format entier comme paramètre et affiche
l'aire de ce cercle.

!!! Note
    Dans cet exercice, vous importez un package afin de pouvoir accéder à la valeur de pi via `PI`. Pour plus
    d'informations sur l'importation de packages, voir [Packages and imports](https://kotlinlang.org/docs/packages.html).


```kotlin
import kotlin.math.PI

fun circleArea() {
    // Écrivez votre code ici
}

fun main() {
    println(circleArea(2))
}
```

<details>
    <summary>Réponse</summary>

```kotlin
import kotlin.math.PI

fun circleArea(radius: Int): Double {
    return PI * radius * radius
}
fun main() {
    println(circleArea(2)) // 12.566370614359172
}
```

</details>
<br>

### Exercice 2

Réécrivez la fonction `circleArea` de l'exercice précédent comme une fonction à expression unique.

```kotlin
import kotlin.math.PI

// Écrivez votre code ici

fun main() {
    println(circleArea(2))
}
```

<details>
    <summary>Réponse</summary>

```kotlin
import kotlin.math.PI

fun circleArea(radius: Int): Double = PI * radius * radius

fun main() {
    println(circleArea(2)) // 12.566370614359172
}
```

</details>
<br>

### Exercice 3

Vous avez une fonction qui traduit un intervalle de temps donné en heures, minutes et secondes en secondes. Dans la
plupart des cas, vous devez passer seulement un ou deux paramètres de fonction alors que le reste est égal à 0.
Améliorez la fonction et le code qui l'appelle en utilisant des valeurs par défaut pour les paramètres et des arguments
nommés afin que le code soit plus facile à lire.

```kotlin
fun intervalInSeconds(hours: Int, minutes: Int, seconds: Int) =
    ((hours * 60) + minutes) * 60 + seconds

fun main() {
    println(intervalInSeconds(1, 20, 15))
    println(intervalInSeconds(0, 1, 25))
    println(intervalInSeconds(2, 0, 0))
    println(intervalInSeconds(0, 10, 0))
    println(intervalInSeconds(1, 0, 1))
}
```

<details>
    <summary>Réponse</summary>

```kotlin
fun intervalInSeconds(hours: Int = 0, minutes: Int = 0, seconds: Int = 0) =
    ((hours * 60) + minutes) * 60 + seconds

fun main() {
    println(intervalInSeconds(1, 20, 15))
    println(intervalInSeconds(minutes = 1, seconds = 25))
    println(intervalInSeconds(hours = 2))
    println(intervalInSeconds(minutes = 10))
    println(intervalInSeconds(hours = 1, seconds = 1))
}
```

</details>
<br>

## Expressions lambda

Kotlin vous permet d'écrire un code encore plus concis pour les fonctions en utilisant des expressions lambda.

Par exemple, la fonction `uppercaseString()` suivante :

```kotlin
fun uppercaseString(text: String): String {
    return text.uppercase()
}
fun main() {
    println(uppercaseString("bonjour"))
    // BONJOUR
}
```

Peut également être écrite comme une expression lambda :

```kotlin
fun main() {
    println({ text: String -> text.uppercase() }("bonjour"))
    // BONJOUR
}
```

Les expressions lambda peuvent être difficiles à comprendre à première vue, alors décomposons-les. Les expressions
lambda sont écrites entre accolades `{}`.

Dans l'expression lambda, vous écrivez :

* les paramètres suivis par un `->`.
* le corps de la fonction après le `->`.

Dans l'exemple précédent :

* `text` est un paramètre de fonction.
* `text` a le type `String`.
* la fonction renvoie le résultat de la
  fonction [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html)
  appelée sur `text`.

!!! Note
    Si vous déclarez une lambda sans paramètres, alors il n'est pas nécessaire d'utiliser `->`. Par exemple :

```kotlin
{ println("Message de log") }
```

Les expressions lambda peuvent être utilisées de plusieurs façons. Vous pouvez :

* attribuer une lambda à une variable que vous pouvez ensuite invoquer plus tard
* passer une expression lambda comme paramètre à une autre fonction
* retourner une expression lambda d'une fonction
* invoquer une expression lambda indépendamment

### Assigner à une variable

Pour attribuer une expression lambda à une variable, utilisez l'opérateur d'affectation `=` :

```kotlin
fun main() {
    val UpperCaseString = { text: String -> text.uppercase() }
    println(UpperCaseString("bonjour"))
    // BONJOUR
}
```

### Passer à une autre fonction

Un excellent exemple de quand il est utile de passer une expression lambda à une fonction est d'utiliser la
fonction [`.filter()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html)
sur des collections :

```kotlin
fun main() {
    //sampleStart
    val numbers = listOf(1, -2, 3, -4, 5, -6)
    val positives = numbers.filter { x -> x > 0 }
    val negatives = numbers.filter { x -> x < 0 }
    println(positives)
    // [1, 3, 5]
    println(negatives)
    // [-2, -4, -6]
    //sampleEnd
}
```

La fonction `.filter()` accepte une expression lambda en tant que prédicat :

* `{ x -> x > 0 }` prend chaque élément de la liste et ne renvoie que ceux qui sont positifs.
* `{ x -> x < 0 }` prend chaque élément de la liste et ne renvoie que ceux qui sont négatifs.

!!! Note
    Si une expression lambda est le seul paramètre de fonction, vous pouvez supprimer les parenthèses de fonction `()`.
    Il s'agit d'un exemple de _lambda de fin_, qui est discuté plus en détail à la fin de ce chapitre.


Un autre bon exemple est l'utilisation de la
fonction [`.map()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map.html)
pour transformer les éléments d'une collection :

```kotlin
fun main() {
    //sampleStart
    val numbers = listOf(1, -2, 3, -4, 5, -6)
    val doubled = numbers.map { x -> x * 2 }
    val tripled = numbers.map { x -> x * 3 }
    println(doubled)
    // [2, -4, 6, -8, 10, -12]
    println(tripled)
    // [3, -6, 9, -12, 15, -18]
    //sampleEnd
}
```

La fonction `.map()` accepte une expression lambda en tant que fonction de transformation :

* `{ x -> x * 2 }` prend chaque élément de la liste et renvoie cet élément multiplié par 2.
* `{ x -> x * 3 }` prend chaque élément de la liste et renvoie cet élément multiplié par 3.

### Types de fonction

Avant de pouvoir renvoyer une expression lambda d'une fonction, vous devez d'abord comprendre les **types de fonction**.

Vous avez déjà appris les types de base, mais les fonctions elles-mêmes ont aussi un type.
L'inférence de type de Kotlin peut déduire le type d'une fonction à partir du type de paramètre.
Mais il peut y avoir des moments où vous devez explicitement spécifier le type de la fonction.
Le compilateur a besoin du type de fonction pour savoir ce qui est autorisé et ce qui ne l'est pas pour cette fonction.

La syntaxe pour un type de fonction a :

* le type de chaque paramètre écrit entre parenthèses `()` et séparé par des virgules `,`.
* le type de retour écrit après `->`.

Par exemple : `(String) -> String` ou `(Int, Int) -> Int`.

Voici à quoi ressemble une expression lambda si un type de fonction pour `upperCaseString()` est défini :

```kotlin
val upperCaseString: (String) -> String = { text -> text.uppercase() }

fun main() {
    println(upperCaseString("bonjour"))
    // BONJOUR
}
```

Si votre expression lambda n'a pas de paramètres, alors les parenthèses `()` sont laissées vides. Par
exemple : `() -> Unit`

!!! Note
    Vous devez déclarer les types de paramètres et de retour soit dans l'expression lambda, soit en tant que type de
    fonction. Sinon, le
    compilateur ne pourra pas savoir quelle est le type de votre expression lambda.
    
    Par exemple, la ligne suivante ne fonctionnera pas :
    
    `val upperCaseString = { str -> str.uppercase() }`


### Retour d'une fonction

Des expressions lambda peuvent être renvoyées par une fonction. Ainsi, pour que le compilateur comprenne quel est le
type de l'expression lambda renvoyée, vous devez déclarer un type de fonction.

Dans l'exemple suivant, la fonction `toSeconds()` a un type de fonction `(Int) -> Int` car elle renvoie toujours une
expression lambda qui prend un paramètre de type `Int` et renvoie une valeur `Int`.

Cet exemple utilise une expression `when` pour déterminer quelle expression lambda est renvoyée lorsque `toSeconds()`
est appelée :

```kotlin
fun toSeconds(time: String): (Int) -> Int = when (time) {
    "heure" -> { value -> value * 60 * 60 }
    "minute" -> { value -> value * 60 }
    "seconde" -> { value -> value }
    else -> { value -> value }
}

fun main() {
    val tempsEnMinutes = listOf(2, 10, 15, 1)
    val min2sec = toSeconds("minute")
    val tempsTotalEnSecondes = tempsEnMinutes.map(min2sec).sum()
    println("Le temps total est de $tempsTotalEnSecondes secondes")
    // Le temps total est de 1680 secondes
}
```

### Invoquer séparément 

Les expressions lambda peuvent être invoquées seules en ajoutant des parenthèses `()` après les accolades `{}` et en
incluant
tous les paramètres dans les parenthèses :

```kotlin
fun main() {
    //sampleStart
    println({ text: String -> text.uppercase() }("bonjour"))
    // BONJOUR
    //sampleEnd
}
```

### Lambdas de fin 

Comme vous l'avez déjà vu, si une expression lambda est le seul paramètre d'une fonction, vous pouvez supprimer les
parenthèses de fonction `()`. Si une expression lambda est passée en tant que dernier paramètre d'une fonction, alors
l'expression peut être écrite en dehors des parenthèses de fonction `()`. Dans les deux cas, cette syntaxe est appelée
un **lambda de fin**.

Par exemple, la fonction [`.fold()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/fold.html) accepte
une valeur initiale et une opération :

```kotlin
fun main() {
    //sampleStart
    // La valeur initiale est zéro.
    // L'opération somme la valeur initiale avec chaque élément de la liste cumulativement.
    println(listOf(1, 2, 3).fold(0, { x, item -> x + item })) // 6

    // Sinon, sous la forme d'un lambda de fin
    println(listOf(1, 2, 3).fold(0) { x, item -> x + item })  // 6
    //sampleEnd
}
```

Pour plus d'informations sur les expressions lambda,
consultez [Expressions lambda et fonctions anonymes](https://kotlinlang.org/docs/lambdas.html#lambda-expressions-and-anonymous-functions).

La prochaine étape de notre tour est d'apprendre à propos des [classes](07-classes.md) en Kotlin.

## Pratique avec les expressions lambda

### Exercice 1

Vous disposez d'une liste d'actions prises en charge par un service web, d'un préfixe commun pour toutes les requêtes et
d'un identifiant d'une ressource particulière.
Pour demander une action `titre` sur la ressource avec l'ID : 5, vous devez créer l'URL
suivante : `https://example.com/informations-livre/5/titre`.
Utilisez une expression lambda pour créer une liste d'URL à partir de la liste des actions.

```kotlin
fun main() {
    val actions = listOf("titre", "année", "auteur")
    val prefix = "https://example.com/informations-livre"
    val id = 5
    val urls = // Écrivez votre code ici
        println(urls)
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
fun main() {
    val actions = listOf("titre", "année", "auteur")
    val prefix = "https://example.com/informations-livre"
    val id = 5
    val urls = actions.map { action -> "$prefix/$id/$action" }
    println(urls)
}
```
</details>
<br>

### Exercice 2

Écrivez une fonction qui prend une valeur `Int` et une action (une fonction de type `() -> Unit`) qui répète ensuite
l'action le nombre de fois donné. Ensuite, utilisez cette fonction pour imprimer "Bonjour" 5 fois.

```kotlin
fun repeatN(n: Int, action: () -> Unit) {
    // Écrivez votre code ici
}

fun main() {
    // Écrivez votre code ici
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
fun repeatN(n: Int, action: () -> Unit) {
    for (i in 1..n) {
        action()
    }
}
fun main() {
    repeatN(5) {
        println("Bonjour")
    }
}
```
</details>
<br>
