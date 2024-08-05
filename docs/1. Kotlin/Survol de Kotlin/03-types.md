# Types de base

[Source](https://kotlinlang.org/docs/kotlin-tour-basic-types.html)

Chaque variable et structure de données en Kotlin ont un type de données. Les types de données sont importants, car ils
indiquent au compilateur ce que vous avez le droit de faire avec cette variable ou structure de données. En d'autres
termes, quelles fonctions et propriétés, elles possèdent.

Dans le dernier chapitre, Kotlin a pu déduire dans l'exemple précédent que `customers` est de
type : [`Int`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-int/).
La capacité de Kotlin à **déduire** le type de données est appelée **inférence de type**. `customers` se voit attribuer
une valeur entière. De cela, Kotlin déduit que `customers` a un type de données numérique : `Int`. Par conséquent, le
compilateur sait que vous pouvez effectuer des opérations arithmétiques avec `customers` :

```kotlin
fun main() {
//sampleStart
    var customers = 10

    // Certains clients quittent la file d'attente
    customers = 8

    customers = customers + 3 // Exemple d'addition : 11
    customers += 7            // Exemple d'addition : 18
    customers -= 3            // Exemple de soustraction : 15
    customers *= 2            // Exemple de multiplication : 30
    customers /= 3            // Exemple de division : 10

    println(customers) // 10
//sampleEnd
}
```

> `+=`, `-=`, `*=`, `/=`, et `%=` sont des opérateurs d'affectation augmentés. Pour plus d'informations,
> voir [Affectations augmentées](https://kotlinlang.org/docs/operator-overloading.html#augmented-assignments).

Au total, Kotlin a les types de base suivants :

| **Catégorie**               | **Types de base**                  |
|-----------------------------|------------------------------------|
| Entiers                     | `Byte`, `Short`, `Int`, `Long`     |
| Entiers non signés          | `UByte`, `UShort`, `UInt`, `ULong` |
| Nombres à virgule flottante | `Float`, `Double`                  |
| Booléens                    | `Boolean`                          |
| Caractères                  | `Char`                             |
| Chaînes                     | `String`                           |

Pour plus d'informations sur les types de base et leurs propriétés,
voir [Types de base](https://kotlinlang.org/docs/basic-types.html).

Avec ces connaissances, vous pouvez déclarer des variables et les initialiser plus tard. Kotlin peut gérer cela tant que
les variables sont initialisées avant la première lecture.

Pour déclarer une variable sans l'initialiser, spécifiez son type avec `:`.

Par exemple :

```kotlin
fun main() {
//sampleStart
    // Variable déclarée sans initialisation
    val d: Int
    // Variable initialisée
    d = 3

    // Variable de type explicite et initialisée
    val e: String = "hello"

    // Les variables peuvent être lues car elles ont été initialisées
    println(d) // 3
    println(e) // hello
//sampleEnd
}
```

Maintenant que vous savez comment déclarer des types de base, il est temps d'en savoir plus sur
les [collections](04-collections.md).

## Pratique

### Exercice

Déclarez explicitement le type correct pour chaque variable :

```kotlin
fun main() {
    val a = 1000
    val b = "log message"
    val c = 3.14
    val d = 100_000_000_000_000
    val e = false
    val f = '\n'
}
```

<details>
    <summary>Réponse</summary>

```kotlin
fun main() {
    val a: Int = 1000
    val b: String = "log message"
    val c: Double = 3.14
    val d: Long = 100_000_000_000
    val e: Boolean = false
    val f: Char = '\n'
}
```
</details>
<br>
