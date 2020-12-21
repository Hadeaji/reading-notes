# Cryptography

## Caesar cipher

In cryptography, a Caesar cipher, also known as Caesar's cipher, the shift cipher, Caesar's code or Caesar shift is one of the simplest and most widely known encryption techniques

It is a type of substitution cipher in which each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet

Example

The transformation can be represented by aligning two alphabets; the cipher alphabet is the plain alphabet rotated left or right by some number of positions. For instance, here is a Caesar cipher using a left rotation of three places, equivalent to a right shift of 23 (the shift parameter is used as the key):

Plain:    ABCDEFGHIJKLMNOPQRSTUVWXYZ
Cipher:   XYZABCDEFGHIJKLMNOPQRSTUVW

When encrypting, a person looks up each letter of the message in the "plain" line and writes down the corresponding letter in the "cipher" line.

Plaintext:  THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
Ciphertext: QEB NRFZH YOLTK CLU GRJMP LSBO QEB IXWV ALD

Deciphering is done in reverse, with a right shift of 3.

## Breaking the cipher

The Caesar cipher can be easily broken even in a ciphertext-only scenario. Two situations can be considered:

1. an attacker knows (or guesses) that some sort of simple substitution cipher has been used, but not specifically that it is a Caesar scheme;
2. an attacker knows that a Caesar cipher is in use, but does not know the shift value.

In the first case, the cipher can be broken using the same techniques as for a general simple substitution cipher, such as frequency analysis or pattern words.[16] While solving, it is likely that an attacker will quickly notice the regularity in the solution and deduce that a Caesar cipher is the specific algorithm employed.

In the second instance, breaking the scheme is even more straightforward. Since there are only a limited number of possible shifts (25 in English), they can each be tested in turn in a brute force attack.[17] One way to do this is to write out a snippet of the ciphertext in a table of all possible shifts[18] – a technique sometimes known as **"completing the plain component"**.

## Cryptography

Cryptography camed from Crypto + graphy which means secret text

the process of making text secret is called encryption and the reverse is decryption

it is used to send share or conect between users in order to protect thier privacy

tere lots of ways used such as the exchange table where the message is being devided then placed in a table of columns then took the columns and formed a words of the same size and could be done multiply times to add to the complixity

there is also the one way Cryptography where both sides decied on shared kay that they applay to the info before sending it

there is some cases where the key is privet and allow the user from encrypting and the other can only decrypt or the it is the reverse using the public kay