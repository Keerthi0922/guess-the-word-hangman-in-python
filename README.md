# guess-the-word-hangman-in-python
import random

# List of words to choose from
words = ["python", "developer", "hangman", "function", "variable", "algorithm", "data", "structure"]

# Hangman art representation for each stage of the game
hangman_art = [
    [
        "  +---+\n      |\n      |\n      |\n     ===",  # 0 wrong guesses
        "  +---+\n  O   |\n      |\n      |\n     ===",  # 1 wrong guess
        "  +---+\n  O   |\n  |   |\n      |\n     ===",  # 2 wrong guesses
        "  +---+\n  O   |\n /|   |\n      |\n     ===",  # 3 wrong guesses
        "  +---+\n  O   |\n /|\\  |\n      |\n     ===",  # 4 wrong guesses
        "  +---+\n  O   |\n /|\\  |\n /    |\n     ===",  # 5 wrong guesses
        "  +---+\n  O   |\n /|\\  |\n / \\  |\n     ==="   # 6 wrong guesses (lost)
    ]
]

def display_man(wrong_guesses):
    """Display the hangman figure based on the number of wrong guesses."""
    print("**********")
    print(hangman_art[0][wrong_guesses])
    print("**********")

def display_hint(hint):
    """Display the current hint (word progress)."""
    print(" ".join(hint))

def display_answer(answer):
    """Display the correct answer when the game ends."""
    print("The word was: " + " ".join(answer))

def main():
    """Main function to run the Hangman game."""
    answer = random.choice(words)  # Select a random word
    hint = ["_"] * len(answer)      # Initialize hint with underscores
    wrong_guesses = 0               # Initialize wrong guesses counter
    guessed_letters = set()         # Store guessed letters
    is_running = True               # Control the game loop

    while is_running:
        display_man(wrong_guesses)  # Display hangman
        display_hint(hint)          # Display current hint
        guess = input("Enter a letter: ").lower()  # Get user input

        # Validate user input
        if len(guess) != 1 or not guess.isalpha():
            print("Invalid input. Please enter a single alphabetic character.")
            continue

        if guess in guessed_letters:
            print(f"You have already guessed '{guess}'.")
            continue

        guessed_letters.add(guess)  # Add guess to the set of guessed letters

        # Check if the guessed letter is in the answer
        if guess in answer:
            for i in range(len(answer)):
                if answer[i] == guess:
                    hint[i] = guess  # Update hint with the correct letter
        else:
            wrong_guesses += 1  # Increment wrong guesses counter

        # Check win or lose conditions
        if "_" not in hint:
            display_man(wrong_guesses)
            display_answer(answer)
            print("YOU WIN!")
            is_running = False
        elif wrong_guesses >= len(hangman_art[0]) - 1:
            display_man(wrong_guesses)
            display_answer(answer)
            print("YOU LOSE!")
            is_running = False

if __name__ == "__main__":
    main()

