import random
import os
import time


class SecurityBreachGame:
    def __init__(self):
        # Configuration
        self.words = ["FIREWALL", "PROTOCOL", "BANDWIDTH", "INTERFACE", "COMPILER"]
        self.max_attempts = 6

        # Game State
        self.secret_word = ""
        self.display_word = []
        self.attempts_left = 0
        self.guessed_letters = []

        # Colors (ANSI Escape Codes for standard terminals)
        self.RED = "\033[91m"
        self.GREEN = "\033[92m"
        self.CYAN = "\033[96m"
        self.YELLOW = "\033[93m"
        self.RESET = "\033[0m"

    def clear_screen(self):
        # Clears terminal for a cleaner UI
        os.system('cls' if os.name == 'nt' else 'clear')

    def get_hangman_art(self):
        stages = [
            f"""
               {self.RED}--------
               |      |
               |      X
               |     \\|/
               |      |
               |     / \\
               -{self.RESET}
            """,
            f"""
               {self.YELLOW}--------
               |      |
               |      O
               |     \\|/
               |      |
               |     / 
               -{self.RESET}
            """,
            """
               --------
               |      |
               |      O
               |     \\|/
               |      |
               |      
               -
            """,
            """
               --------
               |      |
               |      O
               |     \\|
               |      |
               |     
               -
            """,
            """
               --------
               |      |
               |      O
               |      |
               |      |
               |     
               -
            """,
            """
               --------
               |      |
               |      O
               |    
               |      
               |     
               -
            """,
            f"""
               {self.GREEN}--------
               |      |
               |      
               |    
               |      
               |     
               -{self.RESET}
            """
        ]
        return stages[self.attempts_left]

    def print_header(self):
        self.clear_screen()
        print(f"{self.CYAN}=========================================={self.RESET}")
        print(f"{self.CYAN}    SYSTEM BREACH PROTOCOL: INITIATED     {self.RESET}")
        print(f"{self.CYAN}=========================================={self.RESET}")

    def start_game(self):
        self.secret_word = random.choice(self.words)
        self.display_word = ["_"] * len(self.secret_word)
        self.attempts_left = self.max_attempts
        self.guessed_letters = []

        print("Target System Identified...")
        time.sleep(1)
        self.play_loop()

    def play_loop(self):
        while self.attempts_left > 0:
            self.print_header()
            print(self.get_hangman_art())

            # Dashboard stats
            print(f"PASSWORD:  {' '.join(self.display_word)}")
            print(f"ATTEMPTS:  {self.attempts_left}")
            print(f"LOGS:      {', '.join(self.guessed_letters)}")
            print("-" * 42)

            guess = input(f"{self.YELLOW}>> Enter Decryption Key (Letter): {self.RESET}").upper()

            if not self.validate_input(guess):
                continue

            self.process_guess(guess)

            # Win Check
            if "_" not in self.display_word:
                self.print_header()
                print(f"\n{self.GREEN}>>> ACCESS GRANTED. SYSTEM UNLOCKED.{self.RESET}")
                print(f"PASSWORD: {self.secret_word}")
                return

        # Loss Check
        self.print_header()
        print(self.get_hangman_art())
        print(f"\n{self.RED}>>> ACCESS DENIED. SYSTEM LOCKDOWN.{self.RESET}")
        print(f"PASSWORD: {self.secret_word}")

    def validate_input(self, guess):
        if len(guess) != 1 or not guess.isalpha():
            print(f"{self.RED}(!) Invalid Format.{self.RESET}")
            time.sleep(1)
            return False
        if guess in self.guessed_letters:
            print(f"{self.RED}(!) Key already used.{self.RESET}")
            time.sleep(1)
            return False
        return True

    def process_guess(self, guess):
        self.guessed_letters.append(guess)
        if guess in self.secret_word:
            print(f"{self.GREEN}>>> Processing... Match Found.{self.RESET}")
            for i, letter in enumerate(self.secret_word):
                if letter == guess:
                    self.display_word[i] = guess
        else:
            print(f"{self.RED}>>> Processing... No Match.{self.RESET}")
            self.attempts_left -= 1
        time.sleep(0.8)  # Small delay to read the feedback


if __name__ == "__main__":
    game = SecurityBreachGame()
    game.start_game()
