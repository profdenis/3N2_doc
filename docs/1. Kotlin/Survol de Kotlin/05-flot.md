# Flot de contrôle

[Source](https://kotlinlang.org/docs/kotlin-tour-control-flow.html)

Comme les autres langages de programmation, Kotlin est capable de prendre des décisions en fonction de l'évaluation d'un
fragment de code à vrai. De tels fragments de code sont appelés **expressions conditionnelles**. Kotlin est également
capable de créer et naviguer à travers des boucles.

## Expressions conditionnelles

Kotlin propose `if` et `when` pour vérifier des expressions conditionnelles.

> Si vous devez choisir entre `if` et `when`, nous vous recommandons d'utiliser `when`, car cela conduit à des
> programmes plus robustes et plus sûrs.
>

### If

Pour utiliser `if`, ajoutez l'expression conditionnelle entre parenthèses `()` et l'action à effectuer si le résultat
est vrai à l'intérieur des accolades `{}` :

```kotlin
fun main() {
//sampleStart
    val d: Int
    val check = true

    if (check) {
        d = 1
    } else {
        d = 2
    }

    println(d)
    // 1
//sampleEnd
}
```

Il n'y a pas d'opérateur ternaire `condition ? then : else` en Kotlin. En revanche, `if` peut être utilisé comme une
expression. S'il n'y a qu'une seule ligne de code par action, les accolades `{}` sont facultatives :

```kotlin
fun main() {
//sampleStart
    val a = 1
    val b = 2

    println(if (a > b) a else b) // Returns a value: 2
//sampleEnd
}
```

### When

Utilisez `when` lorsque vous avez une expression conditionnelle avec plusieurs branches. `when` peut être utilisé soit
comme une instruction, soit comme une expression.

Voici un exemple d'utilisation de `when` en tant qu'instruction :

* Placez l'expression conditionnelle entre parenthèses `()` et les actions à effectuer
  à l'intérieur des accolades `{}`.
* Utilisez `->` dans chaque branche pour séparer chaque condition de chaque action.

```kotlin
fun main() {
//sampleStart
    val obj = "Hello"

    when (obj) {
        // Vérifie si obj est égal à "1"
        "1" -> println("Un")
        // Vérifie si obj est égal à "Hello"
        "Hello" -> println("Salutation")
        // Instruction par défaut
        else -> println("Inconnu")
    }
    // Salutation
//sampleEnd
}
```

> Notez que toutes les conditions des branches sont vérifiées séquentiellement jusqu'à ce que l'une d'entre elles soit
> satisfaite. Seule la première branche adéquate est donc exécutée.
>

Voici un exemple d'utilisation de `when` en tant qu'expression. La syntaxe `when` est immédiatement assignée à une
variable :

```kotlin
fun main() {
//sampleStart    
    val obj = "Hello"

    val result = when (obj) {
        // Si obj est égal à "1", assigne "Un" à result
        "1" -> "Un"
        // Si obj est égal à "Hello", assigne "Salutation" à result
        "Hello" -> "Salutation"
        // Assign "Inconnu" à result si aucune des conditions précédentes n'est satisfaite
        else -> "Inconnu"
    }
    println(result)
    // Salutation
//sampleEnd
}
```

Si `when` est utilisé comme une expression, la branche `else` est obligatoire, à moins que le compilateur puisse
détecter
que tous les cas possibles sont couverts par les conditions des branches.

L'exemple précédent a montré que `when` est utile pour faire correspondre une variable. `when` est également utile
lorsque vous avez besoin de vérifier une chaîne d'expressions booléennes :

```kotlin
fun main() {
//sampleStart
    val temp = 18

    val description = when {
        // Si temp < 0 est vrai, assigne "voir froid" à description
        temp < 0 -> "très froid"
        // Si temp < 10 est vrai, assigne "un peu froid" à description
        temp < 10 -> "un peu froid"
        // Si temp < 20 est vrai, assigne "chaud" à description
        temp < 20 -> "chaud"
        // Assigne "très chaud" à description si aucune des conditions précédentes n'est satisfaite
        else -> "très chaud"
    }
    println(description)
    // chaud
//sampleEnd
}
```

## Intervalles

Avant de parler des boucles, il est utile de savoir comment construire des intervalles pour que les boucles puissent les
parcourir.

La façon la plus courante de créer un intervalle en Kotlin est d'utiliser l'opérateur `..`. Par exemple, `1..4` équivaut
à `1, 2, 3, 4`.

Pour déclarer un intervalle qui n'inclut pas la valeur de fin, utilisez l'opérateur `..<`. Par exemple, `1..<4` équivaut
à `1, 2, 3`.

Pour déclarer un intervalle en ordre inverse, utilisez `downTo.`. Par exemple, `4 downTo 1` équivaut à `4, 3, 2, 1`.

Pour déclarer un intervalle qui augmente en pas qui n'est pas de 1, utilisez `step` et la valeur d'incrément souhaitée.
Par exemple, `1..5 step 2` équivaut à `1, 3, 5`.

Il est également possible de faire la même chose avec des intervalles de caractères `Char` :

* `'a'..'d'` équivaut à `'a', 'b', 'c', 'd'`
* `'z' downTo 's' step 2` équivaut à `'z', 'x', 'v', 't'`

## Boucles

Les deux structures de boucle les plus courantes en programmation sont `for` et `while`. Utilisez `for` pour itérer sur
une série de valeurs et effectuer une action. Utilisez `while` pour poursuivre une action jusqu'à ce qu'une condition
particulière soit satisfaite.

### Pour

En utilisant votre nouvelle connaissance des intervalles, vous pouvez créer une boucle `for` qui itère sur les numéros 1
à 5 et imprime le numéro à chaque fois.

Placez l'itérateur et l'intervalle entre parenthèses `()` avec le mot-clé `in`. Ajoutez l'action que vous voulez
terminer à l'intérieur des accolades `{}` :

```kotlin
fun main() {
//sampleStart
    for (number in 1..5) {
        // number est l'itérateur et 1..5 est l'intervalle
        print(number)
    }
    // 12345
//sampleEnd
}
```

Les collections peuvent également être parcourues par des boucles :

```kotlin
fun main() {
//sampleStart
    val cakes = listOf("carotte", "fromage", "chocolat")

    for (cake in cakes) {
        println("Miam, c'est un gâteau à la $cake!")
    }
    // Miam, c'est un gâteau à la carotte!
    // Miam, c'est un gâteau au fromage!
    // Miam, c'est un gâteau au chocolat!
//sampleEnd
}
```

### Tant que

`while` peut être utilisé de deux façons :

* Pour exécuter un bloc de code tant qu'une expression conditionnelle est vraie. (`while`)
* Pour exécuter le bloc de code en premier, puis vérifier l'expression conditionnelle. (`do-while`)

Dans le premier cas d'utilisation (`while`) :

* Déclarez l'expression conditionnelle pour que votre boucle continue entre parenthèses `()`.
* Ajoutez l'action que vous voulez effectuer à l'intérieur des accolades `{}`.

> Les exemples suivants utilisent
> l'[opérateur d'incrémentation](https://kotlinlang.org/docs/operator-overloading.html#increments-and-decrements) `++`
> pour incrémenter la valeur de la variable `gâteauxMangés`.
>

```kotlin
fun main() {
//sampleStart
    var gâteauxMangés = 0
    while (gâteauxMangés < 3) {
        println("Mange un gâteau")
        gâteauxMangés++
    }
    // Mange un gâteau
    // Mange un gâteau
    // Mange un gâteau
//sampleEnd
}
```

Dans le second cas (`do-while`) :

* Déclarez l'expression conditionnelle pour que votre boucle continue entre parenthèses `()`.
* Définissez l'action que vous voulez effectuer entre les accolades `{}` avec le mot-clé `do`.

```kotlin
fun main() {
//sampleStart
    var gâteauxMangés = 0
    var gâteauxCuits = 0
    while (gâteauxMangés < 3) {
        println("Mange un gâteau")
        gâteauxMangés++
    }
    do {
        println("Cuit un gâteau")
        gâteauxCuits++
    } while (gâteauxCuits < gâteauxMangés)
    // Mange un gâteau
    // Mange un gâteau
    // Mange un gâteau
    // Cuit un gâteau
    // Cuit un gâteau
    // Cuit un gâteau
//sampleEnd
}
```

Pour plus d'informations et d'exemples sur les expressions conditionnelles et les boucles,
voir [Conditions et boucles](https://kotlinlang.org/docs/control-flow.html).

Maintenant que vous connaissez les fondamentaux du flux de contrôle de Kotlin, il est temps d'apprendre comment écrire
vos propres [fonctions](06-fonctions.md).

## Pratique

### Exercice 1

En utilisant une expression `when`, mettez à jour le programme suivant de sorte que lorsque vous saisissez les noms des
boutons du GameBoy, les actions soient affichées en sortie.

| **Bouton** | **Action**                         |
|------------|------------------------------------|
| A          | Oui                                |
| B          | Non                                |
| X          | Menu                               |
| Y          | Rien                               |
| Autre      | Il n'y a pas de bouton de ce genre |

```kotlin
fun main() {
    val bouton = "A"

    println(
        // Écrivez votre code ici
    )
}
```

<details>
    <summary>Réponse</summary>

```kotlin
fun main() {
    val bouton = "A"
    println(
        when (bouton) {
            "A" -> "Oui"
            "B" -> "Non"
            "X" -> "Menu"
            "Y" -> "Rien"
            else -> "Il n'y a pas de bouton de ce genre"
        }
    )
}
```

</details>
<br>

### Exercice 2

Vous avez un programme qui compte les tranches de pizza jusqu'à ce qu'il y ait une pizza entière avec 8 tranches.
Refactorisez ce programme de deux façons :

* Utilisez une boucle `while`.
* Utilisez une boucle `do-while`.

```kotlin
fun main() {
    var tranchesPizza = 0
    // Commencez le refactoring ici
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    println("Il n'y a que $tranchesPizza tranche/s de pizza :(")
    tranchesPizza++
    // Terminez le refactoring ici
    println("Il y a $tranchesPizza tranches de pizza. Hourra! Nous avons une pizza entière! :D")
}
```

<details>
    <summary>Réponse</summary>

```kotlin
fun main() {
    var tranchesPizza = 0
    while (tranchesPizza < 7 ) {
        tranchesPizza++
        println("Il n 'y a que $tranchesPizza tranche/s de pizza :(")
    }
    tranchesPizza++
    println("Il y a $tranchesPizza tranches de pizza. Hourra!Nous avons une pizza entière!:D")
}
```

</details>
<br>

<details>
    <summary>Réponse</summary>

```kotlin
fun main() {
    var tranchesPizza = 0
    tranchesPizza++
    do {
        println("Il n 'y a que $tranchesPizza tranche/s de pizza :(")
        tranchesPizza++
    } while (tranchesPizza < 8 )
    println("Il y a $tranchesPizza tranches de pizza. Hourra!Nous avons une pizza entière!:D")
}
```

</details>
<br>

### Exercice 3

Écrivez un programme qui simule le jeu [Fizz buzz](https://fr.wikipedia.org/wiki/Fizz_buzz). Votre tâche est d'imprimer
les nombres de 1 à 100 de manière incrémentielle, en remplaçant tout nombre divisible par trois par le mot "fizz", et
tout nombre divisible par cinq par le mot "buzz". Tout nombre divisible à la fois par 3 et 5 doit être remplacé par le
mot "fizzbuzz".

<details>
    <summary>Indice</summary>

Utilisez une boucle <code>for</code> pour compter les numéros et une expression <code>when</code> pour décider de ce
qu'il faut imprimer à chaque étape.
</details>
<br>

```kotlin
fun main() {
    // Écrivez votre code ici
}
```

<details>
    <summary>Réponse</summary>

```kotlin
fun main() {
    for (number in 1..100) {
        println(
            when {
                number % 15 == 0 -> "fizzbuzz"
                number % 3 == 0 -> "fizz"
                number % 5 == 0 -> "buzz"
                else -> "$number"
            }
        )
    }
}
```

</details>
<br>

### Exercice 4

Vous avez une liste de mots. Utilisez `for` et `if` pour imprimer seulement les mots qui commencent par la lettre `l`.

<details>
    <summary>Indice</summary>

Utilisez la fonction <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/starts-with.html">
<code>.startsWith()</code></a> pour le type <code>String</code>.
</details>
<br>

```kotlin
fun main() {
    val mots = listOf("dinosaure", "limousine", "magazine", "langue")
    // Écrivez votre code ici
}
```

<details>
    <summary>Réponse</summary>
    
```kotlin
fun main() {
    val mots = listOf("dinosaure", "limousine", "magazine", "langue")
    for (m in mots) {
        if (m.startsWith("l"))
            println(m)
    }
}
```
</details>
<br>