# Hangman Game

## Background

You will be building the classic hangman game in Python. In hangman, a secret word is chosen and the user tries to guess each letter. Each correct guess reveals all instances of that letter. Each incorrect guess draws another body part on the poor guy getting hanged. The user wins if they reveal the entire word. They lose if the whole person is drawn on the noose.

## Requirements

* Store a list of 5 to 10 words in your script.
* Randomly choose a word from this list as the secret word.
* Display the unrevealed word as underscores (with the same length.)
* Prompt the user to enter a letter.
* If the letter is in the word, mark it as revealed and visually display that letter in the word.
* If the letter is incorrect, draw another part onto the stick person.
* YOU MUST NOT WRITE FUNCTIONS! WRITE THIS ENTIRELY IMPERATIVELY! We will cover functions later. This is an exercise in writing imperative code. Imperative code is harder to reason about than functional code. You may use built-in functions but do not yet write your own.

## Hints

* Your game will need some state to keep track of the word, how many letters are yet to be guessed, and the current state of the hangman.
* You can initially represent the hangman as a decreasing number of guesses remaining.
* You can use the `in` keyword to test to see if a letter is in the secret word.
* Remember to account for case differences.

## Notes

* You decide how many body parts the person gets before completion.
* 