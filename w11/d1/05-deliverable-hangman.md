# Hangman Game

## Background

You will be building the classic hangman game in Python. In hangman, a secret word is chosen and the user tries to guess each letter. Each correct guess reveals all instances of that letter. Each incorrect guess draws another body part on the poor guy getting hanged. The user wins if they reveal the entire word. They lose if the whole person is drawn on the noose.

## Functionality

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

## Pseudocode

1. Initialize the game: Initialize all variables to default values.
2. Display hangman or number of guesses remaining.
3. Randomly select a secret word.
4. Display the word as blanks.
5. Display the letters guessed so far.
6. Ask the user for a letter.
7. Determine if letter is correct or incorrect.
8. If incorrect, add the letter to the guessed list, decrement remaining guesses, and/or draw another bit of the hangman.
9. If correct, add the letter to the guessed list, redraw the secret word with the new letter(s) showing.
10. Loop back up to step 6 and continue until the word is fully revealed or guesses are used up.

## How to draw that secret word?

Being able to render a word as some underscores and some letters will be a little bit of a challenge. What I would recommend is the following:

1. Make one variable to hold the secret word.
2. Make one variable to hold every letter that the user guesses.
3. Make one variable to hold the word as it is displayed.
4. To decide if you should render a blank underscore or render a correct letter, you can iterate over the secret word checking to see if each letter is in the guessed letter list. If it is, use the actual letter character. If it is not, use an underscore to represent a letter not yet guessed.

## Notes

* You decide how many body parts the person gets before completion.
* You can display any kind of hangman that you want. Initially, you should just use a decrementing number. But if you feel like drawing a little person, you might try something that starts like this:

```
-
```

...and adds body parts until it looks like this:
```
-o-|-<
```

Or you could go all out and use Python's multi-line strings to make some breathtaking ASCII art:

```
 ____
|    |
|    O
|   -|-
|    /\
|
-
```