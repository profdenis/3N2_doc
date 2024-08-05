# Collections

[Source](https://kotlinlang.org/docs/kotlin-tour-collections.html)

Lors de la programmation, il est utile de pouvoir regrouper des données dans des structures pour un traitement
ultérieur. Kotlin fournit des collections pour cet exact propos.

Kotlin a les collections suivantes pour regrouper les éléments :

| **Type de collection** | **Description**                                                                         |
|------------------------|-----------------------------------------------------------------------------------------|
| Listes                 | Collections ordonnées d'éléments                                                        |
| Ensembles              | Collections non ordonnées uniques d'éléments                                            |
| Mappes                 | Ensembles de paires clé-valeur où les clés sont uniques et associent à une seule valeur |

Chaque type de collection peut être modifiable ou en lecture seule.

## Liste

Les listes stockent les éléments dans l'ordre dans lequel ils sont ajoutés, et autorisent des éléments en double.

Pour créer une liste en lecture
seule ([`List`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/)), utilisez la
fonction [`listOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/list-of.html).

Pour créer une liste
modifiable ([`MutableList`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list.html)),
utilisez la
fonction [`mutableListOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-list-of.html).

Lors de la création de listes, Kotlin peut inférer le type des éléments stockés. Pour déclarer le type explicitement,
ajoutez le type entre des crochets `<>` après la déclaration de liste :

```kotlin
fun main() { 
//sampleStart
    // Liste en lecture seule
    val formesLectureSeule = listOf("triangle", "carré", "cercle")
    println(formesLectureSeule)
    // [triangle, carré, cercle]
    
    // Liste modifiable avec déclaration de type explicite
    val formes: MutableList<String> = mutableListOf("triangle", "carré", "cercle")
    println(formes)
    // [triangle, carré, cercle]
//sampleEnd
}
```

> Pour prévenir les modifications indésirables, vous pouvez obtenir des vues en lecture seule de listes modifiables en
> les affectant à une `List` :
```kotlin
    val formes: MutableList<String> = mutableListOf("triangle", "carré", "cercle")
    val formesVerrouillées: List<String> = formes
```
> Ceci est également appelé **casting** en anglais.
>


Les listes sont ordonnées donc pour accéder à un élément dans une liste, utilisez
l'[opérateur d'accès indexé](https://kotlinlang.org/docs/operator-overloading.html#indexed-access-operator) `[]` :

```kotlin
fun main() { 
//sampleStart
    val formesLectureSeule = listOf("triangle", "carré", "cercle")
    println("Le premier élément de la liste est : ${formesLectureSeule[0]}")
    // Le premier élément de la liste est: triangle
//sampleEnd
}
```

Pour obtenir le premier ou le dernier élément d'une liste, utilisez respectivement les
fonctions [`.first()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first.html)
et [`.last()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/last.html) :

```kotlin
fun main() { 
//sampleStart
    val formesLectureSeule = listOf("triangle", "carré", "cercle")
    println("Le premier élément de la liste est : ${formesLectureSeule.first()}")
    // Le premier élément de la liste est: triangle
//sampleEnd
}
```

> Les fonctions [`.first()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first.html)
> et [`.last()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/last.html)
> sont des exemples de **fonctions d'extension**. Pour appeler une fonction d'extension sur un objet, écrivez le nom de
> la fonction
> après l'objet en l'appendant avec un point `.`
>
> Pour plus d'informations sur les fonctions d'extension,
> voir [Fonctions d'extensions](https://kotlinlang.org/docs/extensions.html#extension-functions).
> Pour les objectifs de cette visite, vous avez juste besoin de savoir comment les appeler.
>


Pour obtenir le nombre d'éléments dans une liste, utilisez la
fonction [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html) :

```kotlin
fun main() { 
//sampleStart
    val formesLectureSeule = listOf("triangle", "carré", "cercle")
    println("Cette liste contient ${formesLectureSeule.count()} éléments")
    // Cette liste contient 3 éléments
//sampleEnd
}
```

Pour vérifier qu'un élément se trouve dans une liste, utilisez
l'[opérateur `in`](https://kotlinlang.org/docs/operator-overloading.html#in-operatorr) :

```kotlin
fun main() {
//sampleStart
    val formesLectureSeule = listOf("triangle", "carré", "cercle")
    println("cercle" in formesLectureSeule)
    // true
//sampleEnd
}
```

Pour ajouter ou supprimer des éléments d'une liste modifiable, utilisez respectivement les
fonctions [`.add()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/add.html)
et [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) :

```kotlin
fun main() { 
//sampleStart
    val formes: MutableList<String> = mutableListOf("triangle", "carré", "cercle")
    // Ajoute "pentagone" à la liste
    formes.add("pentagone") 
    println(formes)  
    // [triangle, carré, cercle, pentagone]

    // Supprime le premier "pentagone" de la liste
    formes.remove("pentagone") 
    println(formes)  
    // [triangle, carré, cercle]
//sampleEnd
}
```

## Ensemble

Alors que les listes sont ordonnées et autorisent des éléments en double, les ensembles sont **non ordonnés** et
stockent uniquement des éléments **uniques**.

Pour créer un ensemble en lecture
seule ([`Set`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-set/)), utilisez la
fonction [`setOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/set-of.html).

Pour créer un ensemble
modifiable ([`MutableSet`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-set/)), utilisez la
fonction [`mutableSetOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-set-of.html).

Lors de la création des ensembles, Kotlin peut inférer le type des éléments stockés. Pour déclarer explicitement le
type, ajoutez le type entre des crochets `<>` après la déclaration de l'ensemble :

```kotlin
fun main() {
//sampleStart
    // Ensemble en lecture seule
    val fruitsLectureSeule = setOf("pomme", "banane", "cerise", "cerise")
    
    // Ensemble modifiable avec déclaration de type explicite
    val fruits: MutableSet<String> = mutableSetOf("pomme", "banane", "cerise", "cerise")
    
    println(fruitsLectureSeule)
    // [pomme, banane, cerise]
//sampleEnd
}
```

Vous pouvez voir dans l'exemple précédent que comme les ensembles ne contiennent que des éléments uniques,
l'élément `"cerise"` en double est supprimé.

> Pour prévenir les modifications indésirables, obtenez des vues en lecture seule des ensembles modifiables en les
> affectant à `Set` :
>```kotlin
>    val fruits: MutableSet<String> = mutableSetOf("pomme", "banane", "cerise", "cerise")
>    val fruitsVerrouillés: Set<String> = fruits
>```
>


> Comme les ensembles sont **non ordonnés**, vous ne pouvez pas accéder à un élément à un index particulier.
>


Pour obtenir le nombre d'éléments dans un ensemble, utilisez la
fonction [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html) :

```kotlin
fun main() { 
//sampleStart
    val fruitsLectureSeule = setOf("pomme", "banane", "cerise", "cerise")
    println("Cet ensemble contient ${fruitsLectureSeule.count()} éléments")
    // Cet ensemble contient 3 éléments
//sampleEnd
}
```

Pour vérifier qu'un élément se trouve dans un ensemble, utilisez
l'[opérateur `in`](https://kotlinlang.org/docs/operator-overloading.html#in-operator) :

```kotlin
fun main() {
//sampleStart
    val fruitsLectureSeule = setOf("pomme", "banane", "cerise", "cerise")
    println("banane" in fruitsLectureSeule)
    // true
//sampleEnd
}
```

Pour ajouter ou supprimer des éléments d'un ensemble modifiable, utilisez respectivement les
fonctions [`.add()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-set/add.html)
et [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) :

```kotlin
fun main() { 
//sampleStart
    val fruits: MutableSet<String> = mutableSetOf("pomme", "banane", "cerise", "cerise")
    fruits.add("dragonfruit")    // Ajoute "dragonfruit" à l'ensemble
    println(fruits)              // [pomme, banane, cerise, dragonfruit]
   
    fruits.remove("dragonfruit") // Supprime "dragonfruit" de l'ensemble
    println(fruits)              // [pomme, banane, cerise]
//sampleEnd
}
```

Pour obtenir une collection des clés ou des valeurs d'une mappe, utilisez les
propriétés [`keys`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/keys.html)
et [`values`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/values.html) respectivement :

```kotlin
fun main() {
//sampleStart
    val menuJusLectureSeule = mapOf("pomme" to 100, "kiwi" to 190, "orange" to 100)
    println(menuJusLectureSeule.keys)
    // [pomme, kiwi, orange]
    println(menuJusLectureSeule.values)
    // [100, 190, 100]
//sampleEnd
}
```

> [`keys`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/keys.html)
> et [`values`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/values.html) sont des exemples de *
*propriétés** d'un objet. Pour accéder à la propriété d'un objet, écrivez le nom de la propriété après l'objet, en
> ajoutant un point `.`.
> Les propriétés sont discutées plus en détail dans le chapitre [Classes](07-classes.md). À ce stade de la
> visite, vous devez seulement savoir comment y accéder.
>


Pour vérifier qu'une clé ou une valeur est dans une mappe, utilisez
l'[opérateur `in`](https://kotlinlang.org/docs/operator-overloading.html#in-operator) :

```kotlin
fun main() {
//sampleStart
    val menuJusLectureSeule = mapOf("pomme" to 100, "kiwi" to 190, "orange" to 100)
    println("orange" in menuJusLectureSeule.keys)
    // true
    println(200 in menuJusLectureSeule.values)
    // false
//sampleEnd
}
```

Pour plus d'informations sur ce que vous pouvez faire avec les collections,
voir [Collections](https://kotlinlang.org/docs/collections-overview.html).

Maintenant que vous connaissez les types de base et comment gérer les collections, il est temps d'explorer
la [logique de contrôle](05-flot.md) que vous pouvez utiliser dans vos programmes.

## Pratique

### Exercice 1

Vous disposez d'une liste de nombres "verts" et d'une liste de nombres "rouges". Complétez le code pour imprimer combien
de nombres il y a en tout.

```kotlin
fun main() {
    val nombresVerts = listOf(1, 4, 23)
    val nombresRouges = listOf(17, 2)
    // Écrivez votre code ici
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
fun main() {
    val nombresVerts = listOf(1, 4, 23)
    val nombresRouges = listOf(17, 2)
    val totalCount = nombresVerts.count() + nombresRouges.count()
    println(totalCount)
}
```
</details>
<br>

### Exercice 2

Vous avez un ensemble de protocoles pris en charge par votre serveur. Un utilisateur demande à utiliser un protocole
particulier. Complétez le programme pour vérifier si le protocole demandé est pris en charge ou non (`isSupported` doit
être une valeur booléenne).

```kotlin
fun main() {
    val SUPPORTED = setOf("HTTP", "HTTPS", "FTP")
    val requested = "smtp"
    val isSupported = // Écrivez votre code ici 
    println("Support pour $requested: $isSupported")
}
```

<details>
    <summary>Astuce</summary>
    
Assurez-vous de vérifier le protocole demandé en majuscule. Vous pouvez utiliser la fonction <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html"><code>.uppercase()</code></a> pour vous aider.
</details>
<br>

<details>
    <summary>Réponse</summary>
    
```kotlin
fun main() {
    val SUPPORTED = setOf("HTTP", "HTTPS", "FTP")
    val requested = "smtp"
    val isSupported = requested.uppercase() in SUPPORTED
    println("Support pour $requested: $isSupported")
}
```
</details>
<br>

### Exercice 3

Définissez une mappe qui relie les nombres entiers de 1 à 3 à leur orthographe correspondante. Utilisez cette mappe pour
épeler le nombre donné.

```kotlin
fun main() {
    val number2word = // Écrivez votre code ici
    val n = 2
    println("$n est épelé comme '${<Écrivez votre code ici>}'")
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
fun main() {
    val number2word = mapOf(1 to "un", 2 to "deux", 3 to "trois")
    val n = 2
    println("$n est épelé comme '${number2word[n]}'")
}
```
</details>
<br>
