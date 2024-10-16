# karatavas
import random

# Vārdi latviski
word_list = ['Dejas', 'Suns', 'Lauva', 'Skola', 'Mamma', 'Pirts', 'Sals', 'Saule', 'Ozols', 'Salaspils']

# Izvēlas nejaušu vārdu no saraksta
def izveleties_vardu():
    word = random.choice(word_list)
    return word.upper()

# 2. Lietotāja informācijas ievadīšana
# Pieprasa lietotājam ievadīt burtu vai vārdu
def ievadit_gajienu():
    return input("Uzmini burtu vai vārdu: ").upper()

# 3. Iegūtās informācijas salīdzināšana ar vārdu
# Pārbauda, vai ievadītais burts vai vārds ir pareizs, un atjaunina progresu
def salidzinat_vardu(guess, word, guessed_letters, guessed_words, word_completion, tries):
    if len(guess) == 1 and guess.isalpha():
        if guess in guessed_letters:
            print("Tu jau minēji šo burtu:", guess)
        elif guess not in word:
            print(guess, "nav vārdā.")
            tries -= 1
            guessed_letters.append(guess)
        else:
            print("Lieliski,", guess, "ir vārdā!")
            guessed_letters.append(guess)
            word_as_list = list(word_completion)
            indices = [i for i, letter in enumerate(word) if letter == guess]
            for index in indices:
                word_as_list[index] = guess
            word_completion = "".join(word_as_list)
            if "_" not in word_completion:
                return word_completion, guessed, tries
    elif len(guess) == len(word) and guess.isalpha():
        if guess in guessed_words:
            print("Tu jau minēji šo vārdu:", guess)
        elif guess != word:
            print(guess, "nav pareizais vārds.")
            tries -= 1
            guessed_words.append(guess)
        else:
            return word, True, tries
    else:
        print("Nepareizs minējums.")
    return word_completion, False, tries

# 4. Informācijas attēlojums
# Parāda karātavu attēlu un pašreizējo vārda progresu
def attelot_speeli(tries, word_completion):
    print(display_hangman(tries))
    print(word_completion)
    print("\n")

# 5. Spēles beigas un atkārtošana
# Noslēdz spēli, dod iespēju spēlēt atkārtoti
def speles_beigas(guessed, word):
    if guessed:
        print("Apsveicu, tu uzminēji vārdu!")
    else:
        print("Žēl, tu zaudēji. Vārds bija " + word + ". Mēģini vēlreiz!")

def display_hangman(tries):
    stages = [  
                """
                   --------
                   |      |
                   |      O
                   |     \\|/
                   |      |
                   |     / \\
                   -
                """,
                """
                   --------
                   |      |
                   |      O
                   |     \\|/
                   |      |
                   |     / 
                   -
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
                """
                   --------
                   |      |
                   |      
                   |    
                   |      
                   |     
                   -
                """
    ]
    return stages[tries]

def spelet_karatavas():
    word = izveleties_vardu()
    word_completion = "_" * len(word)
    guessed = False
    guessed_letters = []
    guessed_words = []
    tries = 6
    print("Spēlēsim Karatavas!")
    
    attelot_speeli(tries, word_completion)
    
    while not guessed and tries > 0:
        guess = ievadit_gajienu()
        word_completion, guessed, tries = salidzinat_vardu(guess, word, guessed_letters, guessed_words, word_completion, tries)
        attelot_speeli(tries, word_completion)
    
    speles_beigas(guessed, word)

    if input("Vai spēlēt vēlreiz? (Y/N) ").upper() == "Y":
        spelet_karatavas()

if __name__ == "__main__":
    spelet_karatavas()
