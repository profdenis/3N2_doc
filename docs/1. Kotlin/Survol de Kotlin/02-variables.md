# Variables

[Source](https://kotlinlang.org/docs/kotlin-tour-hello-world.html)

Voici un programme simple qui imprime "Hello, world!" :

```kotlin
fun main() {
    println("Hello, world!")
    // Hello, world!
}
```

En Kotlin :

* `fun` est utilisé pour déclarer une fonction
* la fonction `main()` est le point de départ de votre programme
* le corps d'une fonction est écrit entre accolades `{}`
* les fonctions [`println()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/println.html)
  et [`print()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/print.html) affichent leurs arguments en sortie
  standard

> Les fonctions sont discutées plus en détail dans quelques chapitres. Jusque-là, tous les exemples utilisent la
> fonction `main()`.

## Variables

Tous les programmes doivent être capables de stocker des données, et les variables vous aident à faire justement cela.
En Kotlin, vous pouvez déclarer :

* des variables en lecture seule avec `val`
* des variables modifiables avec `var`

Pour attribuer une valeur, utilisez l'opérateur d'affectation `=`.

Par exemple :

```kotlin
fun main() { 
//sampleStart
    val popcorn = 5    // Il y a 5 boîtes de popcorn
    val hotdog = 7     // Il y a 7 hot-dogs
    var clients = 10   // Il y a 10 clients dans la queue
    
    // Certains clients quittent la queue
    clients = 8
    println(clients)
    // 8
//sampleEnd
}
```

> Les variables peuvent être déclarées en dehors de la fonction `main()` au début de votre programme. On dit que les
> variables déclarées de cette manière sont déclarées au **niveau supérieur**.
>

Comme `clients` est une variable modifiable, sa valeur peut être réattribuée après la déclaration.

> Nous recommandons que vous déclariez toutes les variables en lecture seule (`val`) par défaut. Déclarez des variables
> modifiables (`var`) seulement si nécessaire.
>

## Gabarits de chaîne

Il est utile de savoir comment imprimer le contenu des variables en sortie standard. Vous pouvez le faire avec les
**gabarits de chaîne**. Vous pouvez utiliser des expressions de modèle pour accéder aux données stockées dans les
variables et d'autres objets, et les convertir en chaînes. Une valeur de chaîne est une séquence de caractères entre
guillemets doubles `"`. Les expressions de gabarit commencent toujours par un signe dollar `$`.

Pour évaluer un bout de code dans une expression de gabarit, placez le code entre accolades `{}` après le signe
dollar `$`.

Par exemple :

```kotlin
fun main() { 
//sampleStart
    val clients = 10
    println("Il y a $clients clients")
    // Il y a 10 clients
    
    println("Il y a ${clients + 1} clients")
    // Il y a 11 clients
//sampleEnd
}
```

Pour plus d'informations, voir [Gabarits de chaîne](https://kotlinlang.org/docs/strings.html).

Vous remarquerez qu'il n'y a pas de types déclarés pour les variables. Kotlin a inféré le type lui-même : `Int`. Ce
parcours explique les différents types de base de Kotlin et comment les déclarer dans
le [chapitre suivant](03-types.md).

## Pratique

### Exercice

Complétez le code pour que le programme imprime `"Mary a 20 ans"` en sortie standard :

```kotlin
fun main() {
    val nom = "Mary"
    val age = 20
    // Ecrivez votre code ici
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
fun main() {
    val nom = "Mary"
    val age = 20
    println("$nom a $age ans")
}
```
</details>
<br>
