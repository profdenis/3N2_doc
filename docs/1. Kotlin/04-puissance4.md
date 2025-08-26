# Exercice formatif : Jeu de Puissance 4 en Kotlin

## Objectif

Développer une version console du jeu "Puissance 4" en Kotlin. Ce travail pratique vous permettra de vous familiariser
avec la syntaxe et les concepts de base de Kotlin, sans utiliser encore les fonctionnalités spécifiques à Android.

## Description du jeu

Puissance 4 est un jeu de stratégie pour deux joueurs. Le but est d'aligner 4 jetons de sa couleur horizontalement,
verticalement ou en diagonale sur une grille de 6 lignes et 7 colonnes.

## Fonctionnalités requises

1. Afficher la grille de jeu dans la console
2. Permettre aux joueurs de placer leurs jetons à tour de rôle
3. Vérifier la victoire après chaque coup
4. Gérer les erreurs (ex : colonne pleine, entrée invalide)
5. Proposer de rejouer une partie à la fin

## Structure du projet

Voici la structure de base du projet avec le code de départ :

```kotlin
// Fichier: Game.kt
// package ???

enum class CellState(val symbol: Char) {
    EMPTY(' '),
    PLAYER_X('X'),
    PLAYER_O('O');

    override fun toString(): String = symbol.toString()
}

class Game {
    private val board: Array<Array<CellState>> = Array(6) { Array(7) { CellState.EMPTY } }
    private var currentPlayer: CellState = CellState.PLAYER_X

    fun play() {
        // TODO: Implémenter la logique principale du jeu
    }

    fun displayBoard() {
        // TODO: Afficher la grille de jeu
    }

    fun makeMove(column: Int): Boolean {
        // TODO: Placer un jeton dans la colonne spécifiée
        return false
    }

    fun checkWin(): Boolean {
        // TODO: Vérifier s'il y a un gagnant
        return false
    }

    fun isBoardFull(): Boolean {
        // TODO: Vérifier si la grille est pleine
        return false
    }

    fun switchPlayer() {
        currentPlayer = if (currentPlayer == CellState.PLAYER_X) CellState.PLAYER_O else CellState.PLAYER_X
    }

    fun getBoardCell(row: Int, col: Int): CellState {
        return board[row][col]
    }

    fun getCurrentPlayer(): CellState {
        return currentPlayer
    }
}

// Fichier: Main.kt

fun main() {
    val game = Game()
    game.play()
}
```

## Tests unitaires

Voici quelques tests unitaires utilisant `kotlin.test` :

```kotlin
// Fichier: GameTest.kt

import kotlin.test.*
// importer Game et CellState

class GameTest {

    @Test
    fun testInitialBoardIsEmpty() {
        val game = Game()
        for (row in 0..5) {
            for (col in 0..6) {
                assertEquals(CellState.EMPTY, game.getBoardCell(row, col))
            }
        }
    }

    @Test
    fun testMakeValidMove() {
        val game = Game()
        assertTrue(game.makeMove(3))
        assertEquals(CellState.PLAYER_X, game.getBoardCell(5, 3))
    }

    @Test
    fun testMakeInvalidMove() {
        val game = Game()
        assertFalse(game.makeMove(7))  // Colonne invalide
        assertFalse(game.makeMove(-1)) // Colonne invalide
    }

    @Test
    fun testMakeMoveInFullColumn() {
        val game = Game()
        repeat(6) { game.makeMove(0) }
        assertFalse(game.makeMove(0))
    }

    @Test
    fun testSwitchPlayer() {
        val game = Game()
        assertEquals(CellState.PLAYER_X, game.getCurrentPlayer())
        game.switchPlayer()
        assertEquals(CellState.PLAYER_O, game.getCurrentPlayer())
        game.switchPlayer()
        assertEquals(CellState.PLAYER_X, game.getCurrentPlayer())
    }

    @Test
    fun testCheckWinHorizontal() {
        val game = Game()
        repeat(4) { col ->
            game.makeMove(col)
            if (col < 3) game.makeMove(col)
        }
        assertTrue(game.checkWin())
    }

    @Test
    fun testCheckWinVertical() {
        val game = Game()
        repeat(4) {
            game.makeMove(0)
            if (it < 3) game.makeMove(1)
        }
        assertTrue(game.checkWin())
    }

    @Test
    fun testCheckWinDiagonalAscending() {
        val game = Game()
        game.makeMove(0); game.makeMove(1)
        game.makeMove(1); game.makeMove(2)
        game.makeMove(2); game.makeMove(3)
        game.makeMove(2); game.makeMove(3)
        game.makeMove(3); game.makeMove(0)
        game.makeMove(3)
        assertTrue(game.checkWin())
    }

    @Test
    fun testCheckWinDiagonalDescending() {
        val game = Game()
        game.makeMove(3); game.makeMove(2)
        game.makeMove(2); game.makeMove(1)
        game.makeMove(1); game.makeMove(0)
        game.makeMove(1); game.makeMove(0)
        game.makeMove(0); game.makeMove(3)
        game.makeMove(0)
        assertTrue(game.checkWin())
    }

    @Test
    fun testNoWinYet() {
        val game = Game()
        game.makeMove(0); game.makeMove(1)
        game.makeMove(2); game.makeMove(3)
        assertFalse(game.checkWin())
    }

    @Test
    fun testIsBoardFullWhenFull() {
        val game = Game()
        for (col in 0..6) {
            repeat(6) { game.makeMove(col) }
        }
        assertTrue(game.isBoardFull())
    }

    @Test
    fun testIsBoardFullWhenNotFull() {
        val game = Game()
        game.makeMove(0); game.makeMove(1)
        assertFalse(game.isBoardFull())
    }
}
```

## Consignes

1. Créez un projet kotlin dans IntelliJ.
   - Choisissez "Gradle" comme "Build System", et "Kotlin" comme "Gradle DSL".
2. Complétez les fonctions de la classe `Game`.
3. Implémentez la logique du jeu dans la fonction `play()`.
4. Assurez-vous que le jeu gère correctement les entrées utilisateur.
5. Si nécessaire, complétez et ajoutez des tests unitaires pour couvrir toutes les fonctionnalités.
6. Commentez votre code de manière claire et concise.
   - Au début de chaque fichier kotlin (extension `.kt`), écrivez votre nom et numéro de DA dans un commentaire.

## Critères d'évaluation

- Exercice formatif

## Fonctionnalités supplémentaires

- Implémenter un mode contre l'ordinateur avec une IA simple
- Permettre de sauvegarder et charger une partie
