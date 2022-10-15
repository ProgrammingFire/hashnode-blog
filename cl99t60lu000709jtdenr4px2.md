# picoCTF 2022: Cryptography: credstuff

## Introduction
**Challenge:** [credstuff](https://play.picoctf.org/practice/challenge/261?originalEvent=70&page=1)

**Category:** Cryptography

**Description:**

We found a leak of a black-market website's login credentials. Can you find the password of the user `cultiris` and successfully decrypt it? Download the leak [here](https://artifacts.picoctf.net/c/534/leak.tar).
The first user in `usernames.txt` corresponds to the first password in `passwords.txt`. The second user corresponds to the second password, and so on.

## Solution
In this challenge, we have been provided with a `.tar` file. If we extract that `.tar` file then we will get two files, `usernames.txt` and `passwords.txt`. Here we have to find out the password of the user `cultiris`. So let's take a look at `usernames.txt` file:
```txt
engineerrissoles
icebunt
fruitfultry
celebritypentathlon
galoshesopinion
favorboeing
bindingcouch
entersalad
ruthlessconfidence
coupleelevator
remotesword
researchfall
alertborn
....................
```
Now, let's see `passwords.txt`:
```txt
CMPTmLrgfYCexGzJu6TbdGwZa
GK73YKE2XD2TEnvJeHRBdfpt2
UukmEk5NCPGUSfs5tGWPK26gG
kaL36YJtvZMdbTdLuQRx84t85
K9gzHFpwF2azPayAUSrcL8fJ9
rYrtRbkHvJzPmDwzD6gSDbAE3
kfcVXjcFkvNQQPpATErx6eVDd
kDrPVvMakUsNd7BvmJtK3ACY4
dvDvWjzXNk8WwqEzJ5P2FP5YH
86L5w4sH9ZXTCPAa5ExMSPFNh
qXFEg8ZasLxQhUYWnhTemgqxh
gd7panTqNpUvBXBxpGpcqP9X7
Y3KcHyg7kSf6RgX5THyjrw3g1
.........................
```
Let's find for the `cultiris` user:
```txt
378 | cultiris
``` 
I have found the `cultiris` user on line 378. So according to the challenge [description](#heading-introduction), we should have the `cultiris` password on line 378.
```txt
378 | cvpbPGS{P7e1S_54I35_71Z3}
```
The first thing that I noticed is that this looks like a flag, but then, I saw that the letter `c` comes 13 characters before the letter `p`. That clearly means that this is a `ROT13` decrypted message. Let's decrypt it using the [CyberChef](https://gchq.github.io/CyberChef/) tool. It is the tool that I use to decrypt most of the messages.

![CyberChef](https://cdn.hashnode.com/res/hashnode/image/upload/v1665831872832/leGhRnWPd.png align="left")

Now, we have decrypted the message, and the output that we got is `picoCTF{C7r1F_54V35_71M3}`

## Conclusion
This challenge is all about identifying the decryption algorithm and using it to decrypt the message.

**Flag:** `picoCTF{C7r1F_54V35_71M3}`