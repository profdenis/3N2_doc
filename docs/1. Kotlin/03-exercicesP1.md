# Exercices

[GitHub](https://github.com/profdenis/3N2_exercices)

!!! note "Note"
    Le dernier *commit* contient les réponses aux exercices. Faites un *checkout* de l'avant-dernière révision pour avoir la
    structure du code sans les réponses. Vous pouvez créer une branche différente pour y placer vos réponses.

## Exercice 1 : Calculer la somme des chiffres d'un nombre

Écrivez une fonction qui prend un nombre entier positif en entrée et retourne la somme de ses chiffres.

```kotlin
fun sumOfDigits(number: Int): Int {
    // Votre code ici
}
```

Tests unitaires :

```kotlin
import org.junit.jupiter.api.Test
import kotlin.test.assertEquals

class Exercise1Test {
    @Test
    fun testSumOfDigits() {
        assertEquals(6, sumOfDigits(123))
        assertEquals(10, sumOfDigits(1234))
        assertEquals(15, sumOfDigits(54321))
        assertEquals(1, sumOfDigits(1000))
        assertEquals(0, sumOfDigits(0))

        assertEquals(6, sumOfDigits(-123))
        assertEquals(10, sumOfDigits(-1234))
    }
}
```

## Exercice 2 : Vérifier si une chaîne est un palindrome

Écrivez une fonction qui prend une chaîne en entrée et retourne true si c'est un palindrome, false sinon. Ignorez la
casse et les espaces.

```kotlin
fun isPalindrome(str: String): Boolean {
    // Votre code ici
}
```

Tests unitaires :

```kotlin
import org.junit.jupiter.api.Test
import kotlin.test.assertEquals

class Exercise2Test {
    @Test
    fun testIsPalindrome() {
        assertEquals(true, isPalindrome("A man a plan a canal Panama"))
        assertEquals(false, isPalindrome("race a car"))
        assertEquals(false, isPalindrome("hello"))
        assertEquals(true, isPalindrome("Able was I ere I saw Elba"))
        assertEquals(true, isPalindrome(""))

        assertEquals(true, isPalindrome("!@#$%^&*()"))
        assertEquals(true, isPalindrome("  kayak     "))
        assertEquals(false, isPalindrome("Almost a palindrome"))
        assertEquals(true, isPalindrome("A Santa at NASA"))
    }
}
```

## Exercice 3 : Trouver le n-ième nombre de Fibonacci

Écrivez une fonction qui prend un entier n en entrée et retourne le n-ième nombre de la suite de Fibonacci. Supposez que
F(0) = 0 et F(1) = 1.

```kotlin
fun fibonacci(n: Int): Int {
    // Votre code ici
}
```

Tests unitaires :

```kotlin
import org.junit.jupiter.api.Test
import kotlin.test.assertEquals

class Exercise3Test {
    @Test
    fun testFibonacci() {
        assertEquals(0, fibonacci(0))
        assertEquals(1, fibonacci(1))
        assertEquals(1, fibonacci(2))
        assertEquals(2, fibonacci(3))
        assertEquals(5, fibonacci(5))
        assertEquals(55, fibonacci(10))

        assertFailsWith<IllegalArgumentException> {
            fibonacci(-1)
        }
        assertFailsWith<IllegalArgumentException> {
            fibonacci(-10)
        }
    }
}
```

## Exercice 4 : Compter les occurrences d'un caractère dans une chaîne

Écrivez une fonction qui prend une chaîne et un caractère en entrée, et retourne le nombre d'occurrences de ce caractère
dans la chaîne. La recherche doit être sensible à la casse.

```kotlin
fun countOccurrences(str: String, char: Char): Int {
    // Votre code ici
}
```

Tests unitaires :

```kotlin
import org.junit.jupiter.api.Test
import kotlin.test.assertEquals

class Exercise4Test {
    @Test
    fun testCountOccurrences() {
        assertEquals(3, countOccurrences("Hello, World!", 'l'))
        assertEquals(1, countOccurrences("Hello, World!", 'H'))
        assertEquals(0, countOccurrences("Hello, World!", 'h'))
        assertEquals(0, countOccurrences("Hello, World!", 'z'))
        assertEquals(2, countOccurrences("Hello, World!", 'o'))
        assertEquals(0, countOccurrences("", 'a'))

        assertEquals(0, countOccurrences("", ' '))
        assertEquals(1, countOccurrences("Special @#$%^&*() characters", '@'))
        assertEquals(1, countOccurrences("Case Sensitive", 'S'))
        assertEquals(1, countOccurrences("Newline\ncharacter", '\n'))
    }
}
```

## Exercice 5 : Inverser les mots d'une phrase

Écrivez une fonction qui prend une phrase en entrée et retourne une nouvelle phrase avec les mots inversés, tout en
conservant l'ordre des mots.

```kotlin
fun reverseWords(sentence: String): String {
    // Votre code ici
}
```

Tests unitaires :

```kotlin
import org.junit.jupiter.api.Test
import kotlin.test.assertEquals

class Exercise5Test {
    @Test
    fun testReverseWords() {
        assertEquals("olleH dlroW", reverseWords("Hello World"))
        assertEquals("ehT kciuq nworb xof", reverseWords("The quick brown fox"))
        assertEquals("", reverseWords(""))
        assertEquals("a", reverseWords("a"))
        assertEquals("cba fed ihg", reverseWords("abc def ghi"))

        assertEquals("  ", reverseWords("  "))
        assertEquals("olleH, dlroW!", reverseWords("Hello, World!"))
        assertEquals("elpitluM   secaps", reverseWords("Multiple   spaces"))
        assertEquals("noitautcnuP, dna secaps .", reverseWords("Punctuation, and spaces ."))
    }
}
```

## Exercice 6 : Conversion de nombres romains

Créez un programme qui convertit les nombres romains en nombres arabes (entre 1 et 3999 inclusivement). Vous devrez
probablement créer au moins deux fonctions : une pour valider l'entrée et une pour effectuer la conversion.

```kotlin
fun convertRomanToArabic(roman: String): Int {
    // Votre code ici
}

fun isValidRomanNumeral(roman: String): Boolean {
    // Votre code ici
}
```

Tests unitaires :

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals
import kotlin.test.assertFailsWith

class Exercise6Test {
    @Test
    fun testConvertRomanToArabic() {
        assertEquals(1, convertRomanToArabic("I"))
        assertEquals(4, convertRomanToArabic("IV"))
        assertEquals(9, convertRomanToArabic("IX"))
        assertEquals(1994, convertRomanToArabic("MCMXCIV"))
        assertEquals(3999, convertRomanToArabic("MMMCMXCIX"))

        assertFailsWith<IllegalArgumentException> {
            convertRomanToArabic("IIII")
        }
        assertFailsWith<IllegalArgumentException> {
            convertRomanToArabic("ABC")
        }
    }

    @Test
    fun testIsValidRomanNumeral() {
        assertEquals(true, isValidRomanNumeral("MCMLIV"))
        assertEquals(false, isValidRomanNumeral("IIII"))
        assertEquals(false, isValidRomanNumeral("XYZ"))
        assertEquals(true, isValidRomanNumeral("MMMCMXCIX"))
        assertEquals(false, isValidRomanNumeral("IC"))
    }
}
```

## Exercice 7 : Jeu du morpion (Tic-Tac-Toe)

Implémentez un jeu de morpion. Vous aurez besoin de plusieurs fonctions pour gérer l'état du jeu, les mouvements des
joueurs et vérifier s'il y a un gagnant.

```kotlin
class TicTacToe {
    // Définissez ici les propriétés nécessaires

    fun makeMove(player: Char, row: Int, col: Int): Boolean {
        // Votre code ici
    }

    fun checkWinner(): Char? {
        // Votre code ici
    }

    fun isBoardFull(): Boolean {
        // Votre code ici
    }

    fun printBoard() {
        // Votre code ici
    }
}
```

Tests unitaires :

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals
import kotlin.test.assertFalse
import kotlin.test.assertTrue

class Exercise7Test {
    @Test
    fun testTicTacToe() {
        val game = TicTacToe()

        assertTrue(game.makeMove('X', 0, 0))
        assertFalse(game.makeMove('O', 0, 0))
        assertTrue(game.makeMove('O', 1, 1))

        assertEquals(null, game.checkWinner())

        game.makeMove('X', 0, 1)
        game.makeMove('X', 0, 2)

        assertEquals('X', game.checkWinner())

        assertTrue(game.isBoardFull())
    }
}
```

## Exercice 8 : Calculatrice de notation polonaise inverse (RPN)

Implémentez une calculatrice qui utilise la notation polonaise inverse. Vous aurez besoin de fonctions pour analyser
l'entrée, effectuer les opérations et gérer une pile.

```kotlin
class RPNCalculator {
    private val stack = mutableListOf<Double>()

    fun evaluate(expression: String): Double {
        // Votre code ici
    }

    private fun performOperation(operator: String) {
        // Votre code ici
    }
}
```

Tests unitaires :

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals
import kotlin.test.assertFailsWith

class Exercise8Test {
    @Test
    fun testRPNCalculator() {
        val calculator = RPNCalculator()

        assertEquals(3.0, calculator.evaluate("1 2 +"))
        assertEquals(23.0, calculator.evaluate("5 4 * 3 +"))
        assertEquals(141.0, calculator.evaluate("3 4 + 5 * 3 -"))

        assertFailsWith<IllegalArgumentException> {
            calculator.evaluate("1 +")
        }
        assertFailsWith<IllegalArgumentException> {
            calculator.evaluate("1 2 3 +")
        }
    }
}
```

## Exercice 9 : Générateur de mots de passe

Créez un générateur de mots de passe qui prend en compte différents critères (longueur, inclusion de chiffres, de
symboles, etc.). Vous aurez besoin de fonctions pour générer différents types de caractères et vérifier la force du mot
de passe.

```kotlin
class PasswordGenerator {
    fun generatePassword(length: Int, includeDigits: Boolean, includeSymbols: Boolean): String {
        // Votre code ici
    }

    fun checkPasswordStrength(password: String): String {
        // Retourne "Faible", "Moyen" ou "Fort"
        // Votre code ici
    }

    private fun generateRandomChar(charPool: List<Char>): Char {
        // Votre code ici
    }
}
```

Tests unitaires :

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals
import kotlin.test.assertTrue

class Exercise9Test {
    @Test
    fun testPasswordGenerator() {
        val generator = PasswordGenerator()

        val password1 = generator.generatePassword(10, true, true)
        assertEquals(10, password1.length)
        assertTrue(password1.any { it.isDigit() })
        assertTrue(password1.any { !it.isLetterOrDigit() })

        val password2 = generator.generatePassword(8, false, false)
        assertEquals(8, password2.length)
        assertTrue(password2.all { it.isLetter() })

        assertEquals("Fort", generator.checkPasswordStrength("P@ssw0rd123!"))
        assertEquals("Faible", generator.checkPasswordStrength("password"))
    }
}
```

## Exercice 10 : Analyseur de texte

Créez un analyseur de texte qui peut compter les mots, les phrases, et fournir des statistiques sur le texte. Vous aurez
besoin de plusieurs fonctions pour traiter différents aspects du texte.

```kotlin
class TextAnalyzer {
    fun countWords(text: String): Int {
        // Votre code ici
    }

    fun countSentences(text: String): Int {
        // Votre code ici
    }

    fun getMostFrequentWord(text: String): String {
        // Votre code ici
    }

    fun getAverageWordLength(text: String): Double {
        // Votre code ici
    }
}
```

Tests unitaires :

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals

class Exercise10Test {
    @Test
    fun testTextAnalyzer() {
        val analyzer = TextAnalyzer()
        val text = "Hello world! This is a test. This test has multiple sentences."

        assertEquals(10, analyzer.countWords(text))
        assertEquals(3, analyzer.countSentences(text))
        assertEquals("this", analyzer.getMostFrequentWord(text))
        assertEquals(4.0, analyzer.getAverageWordLength(text))
    }
}
```
