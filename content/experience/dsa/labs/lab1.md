---
title: "DSA lab 1"
date: Labs
mainSectionTitle: "ptc"
---

[Download problem set](https://dk3156.github.io/experience/dsa/labs/dsa_lab1.docx)
## Answer for Coding Questions
```
#QUESTION 1

Apples = 
[a = 1, p = 2, l =1, e = 1, s =1]
Apls
[a = 1, p=1, l=1, s=1]

def can_construct(word, letters):
	letters_lst = [-1] * 26 #We create a list with a length of 26 with each index representing one character
    
	for char in letters:
    		if letters_lst[ord(char)-97] == -1: #We offset by 97 because the ascii values of lowercase letters start from 97
        			letters_lst[ord(char)-97] = 0
    		 letters_lst[ord(char)-97] += 1

	for char in word:
    		if letters_lst[ord(char)-97] == -1: #a letter of the word is not in letters
        			return False
    		if letters_lst[ord(char)-97] < 1: #not enough of the same letters for the word
        			return False
    		letters_lst[ord(char)-97] -= 1 #character is in the word so we "use" it by reducing the count by 1
	return True #confirmed that each the letters are enough to create the word

#QUESTION 2

class Complex:
    #a + bi
    def __init__(self, a, b):
        self.a = a #constant
        self.b = b #coefficient for i

    def __add__(self, other):
        return Complex(self.a + other.a, self.b + other.b)
    
    def __sub__(self, other):
        return Complex(self.a - other.a, self.b - other.b)

    def __mul__(self, other): #FOIL Method
        return Complex(self.a * other.a - self.b * other.b, self.a * other.b + self.b * other.a)
    
    def __str__(self):
        return str(self.a) + " + " + str(self.b) + "i"
    
    def __iadd__(self, other):
        self = self + other
        return self


#TEST CODE

'''
def __add__(self, other): 
cplx1 + cplx2
In this example, self refers to cplx1 since it is the first argument and other would refer cplx2 since it is the second argument.
'''


#constructor, output
cplx1 = Complex(5, 2)
print(cplx1) 	#5 + 2i

cplx2 = Complex(3, 3)
print(cplx2)	#3 + 3i

#addition
print(cplx1 + cplx2)	#8 + 5i

#subtraction
print(cplx1 - cplx2)	#2 - 1i

#multiplication
print(cplx1 * cplx2)	#9 + 21i

#original objects remain unchanged
print(cplx1) 	#5 + 2i
print(cplx2)	#3 + 3i





# QUESTION 3

import random

# Part A
# decreases the given number by 1 each loop and places it at a random index in the list. 
# Guarantees the numbers in the list are not repeated.

def create_permutation(n):
    perm = [-1]*n #-1 is used to signify that the spot has not been taken
    counter = n - 1
    while counter >= 0:
        index = random.randint(0, n-1)
        if perm[index] == -1:
            perm[index] = counter
            counter -= 1
            
    return perm



# Part B
# Calls the create_permutation function which returns a list. 
# Uses the values of the list to index the letters in the word and places them in the list. 

def scramble_word(word):
    lst = create_permutation(len(word))
    for i in range(len(lst)):
        lst[i] = word[lst[i]]
    return " ".join(lst)


# Part C
word_bank = ["Pokemon", "Digimon"]

def choose_word(words):
    return words[random.randint(0, len(word_bank)-1)]

def guessing_game(word):
    print("Unscramble the word " + scramble_word(word))
    tries = 3
    correct_guess = False

    while tries and not correct_guess:
        guess = input("Try #" + str(4 - tries) +":" )
        if word == guess:
            correct_guess = True
        else:
            print("Wrong!")
            tries -= 1

    if tries:
        print("Yay you got it!")
    else:
        print("Out of tries!")


pick = choose_word(word_bank)

guessing_game(pick)



#QUESTION 1 
def add_bin(bin_num1, bin_num2):
    result = ""
    carry = 0

    max_length = max(len(bin_num1), len(bin_num2)) 
    
    #sign extension so both will have same length
    b1 = (max_length - len(bin_num1))*'0' + bin_num1
    b2 = (max_length - len(bin_num2))*'0' + bin_num2


    for i in range(1, max_length + 1): 
        #we will use the negative indices since we evaluate from right to left
        sum_bits = int(b1[-i]) + int(b2[-i]) + carry
        result = str(sum_bits % 2) + result

        #carry = sum_bits >= 2  #can shorten the following if/else like with this line via implicit bool conversion
        if sum_bits < 2: 
            carry = 0
        else:
            carry = 1 #if sum of the bits + carry >= 2, then carry = 1

    return carry*'1' + result
```