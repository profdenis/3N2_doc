# Sécurité contre les valeurs nulles

[Source](https://kotlinlang.org/docs/kotlin-tour-null-safety.html)

En Kotlin, il est possible d'avoir une valeur `null`. Pour aider à prévenir les problèmes avec les valeurs `null` dans
vos programmes, Kotlin a mis en place une sécurité contre les nullités. La sécurité contre les nullités détecte les
problèmes potentiels avec les valeurs `null` au moment de la compilation, plutôt qu'au moment de l'exécution.

La sécurité contre les nullités est une combinaison de fonctionnalités qui vous permettent de :

* déclarer explicitement quand les valeurs `null` sont autorisées dans votre programme.
* vérifier la présence de valeurs `null`.
* utiliser des appels sécurisés à des propriétés ou des fonctions qui peuvent contenir des valeurs `null`.
* déclarer des actions à prendre si des valeurs `null` sont détectées.

## Types Nullable

Kotlin supporte les types Nullable qui permettent la possibilité pour le type déclaré d'avoir des valeurs `null`. Par
défaut, un type n'est **pas** autorisé à accepter des valeurs `null`. Les types Nullable sont déclarés en ajoutant
explicitement `?` après la déclaration du type.

Par exemple :

```kotlin
fun main() {
    // neverNull a le type String
    var neverNull: String = "Ceci ne peut pas être null"
    
    // Lève une erreur de compilation
    neverNull = null
    
    // nullable a un type String nullable
    var nullable: String? = "Vous pouvez garder un null ici"
    
    // C'est OK
    nullable = null
    
    // Par défaut, les valeurs null ne sont pas acceptées
    var inferredNonNull = "Le compilateur suppose non-nullable"
    
    // Lève une erreur de compilation
    inferredNonNull = null
    
    // notNull n'accepte pas les valeurs null
    fun strLength(notNull: String): Int {                 
        return notNull.length
    }
    
    println(strLength(neverNull)) // 18
    println(strLength(nullable))  // Lève une erreur de compilation
}
```

> `length` est une propriété de la classe [String](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/) qui
> contient le nombre de caractères dans une chaîne.
>

## Vérifier les valeurs nulles

Vous pouvez vérifier la présence de valeurs `null` dans les expressions conditionnelles. Dans l'exemple suivant, la
fonction `describeString()` a une instruction `if` qui vérifie si `maybeString` n'est **pas** `null` et si sa `length`
est supérieure à zéro :

```kotlin
fun describeString(maybeString: String?): String {
    if (maybeString != null && maybeString.length > 0) {
        return "Chaîne de longueur ${maybeString.length}"
    } else {
        return "Chaîne vide ou null"
    }
}

fun main() {
    val nullString: String? = null
    println(describeString(nullString))
    // Chaîne vide ou null
}
```

## Utiliser des appels sécurisés

Pour accéder en toute sécurité aux propriétés d'un objet qui peut contenir une valeur `null`, utilisez l'opérateur 
d'appel sécurisé `?.`. L'opérateur d'appel sécurisé renvoie `null` si l'objet ou l'une de ses propriétés accédées
est `null`. Ceci est utile si vous voulez éviter que la présence de valeurs `null` déclenche des erreurs dans votre
code.

Dans l'exemple suivant, la fonction `lengthString()` utilise un appel sécurisé pour renvoyer soit la longueur de la
chaîne, soit `null` :

```kotlin
fun lengthString(maybeString: String?): Int? = maybeString?.length

fun main() { 
    val nullString: String? = null
    println(lengthString(nullString))
    // null
}
```

> Les appels sécurisés peuvent être chaînés de manière à ce que si une propriété d'un objet contient une valeur `null`,
> alors `null` est renvoyé sans qu'une erreur soit levée. Par exemple :
> ```kotlin
>   person.company?.address?.country
> ```
>


L'opérateur d'appel sécurisé peut également être utilisé pour appeler en toute sécurité une fonction d'extension ou de
membre. Dans ce cas, une vérification de nullité est effectuée avant l'appel de la fonction. Si la vérification détecte
une valeur `null`, alors l'appel est sauté et `null` est renvoyé.

Dans l'exemple suivant, `nullString` est `null` donc l'invocation
de [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html) est sautée et `null` est
renvoyé :

```kotlin
fun main() {
    val nullString: String? = null
    println(nullString?.uppercase())
    // null
}
```

## Utiliser l'opérateur Elvis

Vous pouvez fournir une valeur par défaut à retourner si une valeur `null` est détectée en utilisant 
l'**opérateur Elvis** `?:`.

Écrivez sur le côté gauche de l'opérateur Elvis ce qui doit être vérifié pour une valeur `null`.
Écrivez sur le côté droit de l'opérateur Elvis ce qui doit être renvoyé si une valeur `null` est détectée.

Dans l'exemple suivant, `nullString` est `null` donc l'appel sécurisé pour accéder à la propriété `length` renvoie une
valeur `null`.
En conséquence, l'opérateur Elvis renvoie `0` :

```kotlin
fun main() {
    val nullString: String? = null
    println(nullString?.length ?: 0)
    // 0
}
```

Pour plus d'informations sur la sécurité des nullités en Kotlin,
voir [Null safety](https://kotlinlang.org/docs/null-safety.html).

## Pratique

### Exercice

Vous avez la fonction `employeeById` qui vous donne accès à une base de données d'employés d'une entreprise.
Malheureusement, cette fonction renvoie une valeur du type `Employee?`, donc le résultat peut être `null`. Votre but est
d'écrire une fonction qui renvoie le salaire d'un employé quand son `id` est fourni, ou `0` si l'employé est manquant
dans la base de données.

```kotlin
data class Employee (val name: String, var salary: Int)

fun employeeById(id: Int) = when(id) {
    1 -> Employee("Mary", 20)
    2 -> null
    3 -> Employee("John", 21)
    4 -> Employee("Ann", 23)
    else -> null
}

fun salaryById(id: Int) = // Écrivez votre code ici

fun main() {
    println((1..5).sumOf { id -> salaryById(id) })
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
data class Employee (val name: String, var salary: Int)
fun employeeById(id: Int) = when(id) {
    1 -> Employee("Mary", 20)
    2 -> null
    3 -> Employee("John", 21)
    4 -> Employee("Ann", 23)
    else -> null
}
fun salaryById(id: Int) = employeeById(id)?.salary ?: 0
fun main() {
    println((1..5).sumOf { id -> salaryById(id) })
}
```
</details>
<br>

