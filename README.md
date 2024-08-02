            import random

            # Hangman ASCII art
            HANGMAN_ART = [
                '''
              _______
              |       |
                      |
                      |
                      |
                      |
            =========''',
                '''
              _______
              |       |
              O       |
                      |
                      |
                      |
            =========''',
                '''
              _______
              |       |
              O       |
              |       |
                      |
                      |
            =========''',
                '''
              _______
              |       |
              O       |
             /|       |
                      |
                      |
            =========''',
                '''
              _______
              |       |
              O       |
             /|\      |
                      |
                      |
            =========''',
                '''
              _______
              |       |
              O       |
             /|\      |
             /        |
                      |
            =========''',
                '''
              _______
              |       |
              O       |
             /|\      |
             / \      |
                      |
            ========='''
            ]

            # List of words
            WORD_LIST = [
                'ant', 'baboon', 'badger', 'bat', 'bear', 'beaver', 'camel', 'cat', 'clam', 'cobra', 'cougar', 'coyote', 'crow', 'deer', 'dog', 'donkey', 'duck', 'eagle', 'ferret', 'fox', 'frog', 'goat', 'goose', 'hawk', 'lion', 'lizard', 'llama', 'mole', 'monkey', 'moose', 'mouse', 'mule', 'newt', 'otter', 'owl', 'panda', 'parrot', 'pigeon', 'python', 'rabbit', 'ram', 'rat', 'raven', 'rhino', 'salmon', 'seal', 'shark', 'sheep', 'skunk', 'sloth', 'snake', 'spider', 'stork', 'swan', 'tiger', 'toad', 'trout', 'turkey', 'turtle', 'weasel', 'whale', 'wolf', 'wombat', 'zebra'
            ]

            def get_random_word(word_list):
                return random.choice(word_list)

            def display_board(hangman_art, missed_letters, correct_letters, secret_word):
                print(hangman_art[len(missed_letters)])
                print()

                print('Missed letters:', end=' ')
                for letter in missed_letters:
                    print(letter, end=' ')
                print()

                blanks = '_' * len(secret_word)

                for i in range(len(secret_word)):
                    if secret_word[i] in correct_letters:
                        blanks = blanks[:i] + secret_word[i] + blanks[i+1:]

                for letter in blanks:
                    print(letter, end=' ')
                print()

            def get_guess(already_guessed):
                while True:
                    print('Guess a letter.')
                    guess = input().lower()
                    if len(guess) != 1:
                        print('Please enter a single letter.')
                    elif guess in already_guessed:
                        print('You have already guessed that letter. Choose again.')
                    elif guess not in 'abcdefghijklmnopqrstuvwxyz':
                        print('Please enter a LETTER.')
                    else:
                        return guess

            def play_again():
                print('Do you want to play again? (yes or no)')
                return input().lower().startswith('y')

            print('H A N G M A N')
            missed_letters = ''
            correct_letters = ''
            secret_word = get_random_word(WORD_LIST)
            game_is_done = False

            while True:
                display_board(HANGMAN_ART, missed_letters, correct_letters, secret_word)

                guess = get_guess(missed_letters + correct_letters)

                if guess in secret_word:
                    correct_letters += guess

                    found_all_letters = True
                    for i in range(len(secret_word)):
                        if secret_word[i] not in correct_letters:
                            found_all_letters = False
                            break
                    if found_all_letters:
                        print(f'Yes! The secret word is "{secret_word}"! You have won!')
                        game_is_done = True
                else:
                    missed_letters += guess

                    if len(missed_letters) == len(HANGMAN_ART) - 1:
                        display_board(HANGMAN_ART, missed_letters, correct_letters, secret_word)
                        print(f'You have run out of guesses!\nAfter {len(missed_letters)} missed guesses and {len(correct_letters)} correct guesses, the word was "{secret_word}"')
                        game_is_done = True

                if game_is_done:
                    if play_again():
                        missed_letters = ''
                        correct_letters = ''
                        game_is_done = False
                        secret_word = get_random_word(WORD_LIST)
                    else:
                        break
