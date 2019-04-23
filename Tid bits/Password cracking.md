Ideas:
Show hashcat installation
Discuss identifying hashes
Explain salts
Explain difficulties of cracking certain passwords
Link to ExRex


Notes:
Brutefore is simply doing aaaaaa aaaaab

Number of char in char set to power of lenght of password.
26 lower case digits to the power of 7
6 char passwords 2 digits are 26 to the power of 6 multiplied by 10 to the power of 2.

Show salted hash example and blowfish

How passwords work is that you take a password that the user enters,  hash that password using whatever algorithm/encryption method you wish and put it in the database as random crap.
Then when the user comes along again, the user puts their password in and the system does the same process and checks the database. If the random crap matches, you're let in.

When cracking passwords you're doing the same process. You have the end result which is the hash and you're taking your own random password which you're trying to guess and you're hashing it using the same method used originally and you're checking if the random crap matches what you had down for that specific user's password.

Bruteforcing is a way of ensuring not a single password is missed. It literally starts from the bottom with just the letter a and increments b c then aa ab ac and adding upper and lower case and numbers and special characters massively increases the wordspace and exponentially becomes difficult

----------------

Password security
- all about trust. Everything on the internet is run by humans. You trust the person on the other end's ability to
