# MTech - Information Security

**Date** : Aug 2022 - 2025

## [[Cryptography]] Basics

**Date** : Aug - Dec - 2022
**Tag** : #crypto

### Assignments

- Assignment 1 - Shift Cipher
- Assignment 2 - Implement Feistel Cipher Network
- Assignment 3 - Implement [[ElGamal]] [[digital signature]] algorithm using Shamir trick for exponentiation
	- Shamir trick for exponentiation is actually called Straus-Shamir Trick. Reference paper 
		- use it for two power only nothing more than that
	- You have to implement verification algorithm using double-exponentiation
	- Open source implementation
		- 
		- [ElGamal Implementation](https://github.com/crysqrlkys/ipm/tree/master/elgamal)
			- Implemented in cpp and in small number only
		- https://github.com/D35YNC/D35CryptoLib/blob/master/lib/public_key/elgamal.cpp
	- Reference Shared by Prof.
		- Finding primitive root of very large prime number [link](https://crypto.stackexchange.com/questions/56155/primitive-root-of-a-very-big-prime-number-elgamal-ds)
		- Shamir trick for exponentiation
			- [Algorithm for Multi-exponentation](https://www.bmoeller.de/pdf/multiexp-sac2001.pdf)
			- http://www1.spms.ntu.edu.sg/~ccrg/documents/chienning-multiexponentiation.pdf
- Optional Assignment
	- Implement ECC Digital Signature
	- Ref
		- ECC library [link](https://github.com/ANSSI-FR/libecc)
- Assignment 4 - Implement HMAC


### Notes

- [Lecture Recording & Notes](https://drive.google.com/drive/u/6/folders/1yYscKjogEhK7eLMWR9nRvh8r33BAf5k-)
- Representing large number using small number is the trick using CRT
	- https://github.com/mortendahl/mortendahl.github.io/blob/master/_drafts/the-chinese-remainder-theorem.md
	- https://github.com/e-maxx-eng/e-maxx-eng/blob/master/src/algebra/chinese-remainder-theorem.md
- Open Source implementation of various components needed by cryptography class
	- [[Arbitrary Precision]] Library implementation in C [link](https://github.com/kokke/tiny-bignum-c)
	- RSA Implementation in C [link](https://github.com/Biggy54321/crypto-rsa-C/blob/master/rsa_key_gen.c)
	- [[ElGamal]] encryption scheme
		- Fixed size implementation [link](https://github.com/crysqrlkys/ipm/blob/master/elgamal/main.cpp)
		- ElGamal signing for Big Number implementation in C [link](https://github.com/D35YNC/D35CryptoLib/blob/master/lib/public_key/elgamal.cpp)
		- python implementation [link](https://github.com/MJVL/CSCI-462-Tools/blob/main/scripts/elgamal_digital_signature.py)
		- Implementation in using GNU GMP lib large number integers [link](https://github.com/MichaelND/Elgamal-Encryption/blob/master/encrypt.c)
		- Finding primitive root of very large prime number [link](https://github.com/e-maxx-eng/e-maxx-eng/blob/master/src/algebra/primitive-root.md)
	- [[Chinese Remainder Theorem]] [[CRT]] implementation and explanation [link](https://github.com/e-maxx-eng/e-maxx-eng/blob/master/src/algebra/chinese-remainder-theorem.md)
	- Factorizing algorithm [link](https://github.com/e-maxx-eng/e-maxx-eng/blob/master/src/algebra/factorization.md)
		
### Video Lecture Index

- Shift Cipher
- Symmetric Key crypto
- DSA
- Random Number Generator
- Lect 6
	- Block Cipher
- Lect 7
	- 22:00 - DES
- Lect 10
- Lect 11
	- Asymmetric cryptography concepts
	- 43:00 - RSA algorithm intro
	- Explains RSA algorithm
	- CRT for representing very large number and do operations on it
- Lect 12
	- How to optimize [[RSA]] secret key math using Remainder tuple representation
	- and how to further optimize calculation using Fermat little theorem
	- RSA algorithm is Deterministic and for very small message space brute-forcing can be done. That's why we need some randomization in the cipher text
- Lect 13
	- RSA Cont.
	- 47:00 [[ElGamal]] encryption 
	- explain cyclic groups and prime order sub groups
	- How ElGamal public private keys chosen such that cipher text is not reveal
- Lect 14
	- [[Trap Door function]] / One-way function are the function which is easy to calculate in one direction but computationally infeasible to calculate the reverse value. Some examples are [[Discrete Log]], [[prime factorization]] and [[Elliptic Curve]] which we will be discussed in this lecture.
	- these [[trap door functions]] form the basis of [[public key]] cryptography.
	- Elliptic Curve Cryptography [[ECC]]
	- Co-ordinate points of size 80 - 160 bits will be of same difficult as 1024 bits (ElGamal) Discrete log Problems
	- Definition of Elliptic Curve 
	- Intrusion of how elliptic curve forms [[abel group]] and how it can be used as [[one-way function]] and proved some properties the group
	- Bitcoin protocol uses ECC
- Lect 15
	- ECC cont
	- 25:00 [[Diffie-Hellman]] key Exchange
	- Explains a little bit about ECC parameters used by bitcoin protocol
- Lect 16
	- [[Hash Function]]
	- Discusses security requirement of hash functions
	- Doesn't discusses any particular algorithm
	- discussion about what should be the padding of the message before computing hash because we need to have message aligned to particular value.
- Lect 17
	- Digital Signature Scheme
	- 41:00 - ElGamal Digital Signature Scheme till the end
- Lect 18
	- continuation of previous Lect on digital signature
	- Explains Schnarr's algorithm
- Lect 19
	- Digital Signature Standards [[DSS]]
	- Parameters of public key crypto systems
	- Shamir's trick for multi-exponentiation from 55:00 to 59:00 
		- it is similar to square and multiply algorithm done for two numbers, go through this video [link](https://www.youtube.com/watch?v=cbGB__V8MNk&t=904s&ab_channel=Computerphile)
- Lect 20
	- Message Authentication Codes - [[MAC]]
	- [[HMAC]]
- Lect 21
	- Key Management
- Lect 22
	- 33:00 - Kerberos
	- 54:00 - Identification


### Reference

1. Understanding Cryptography: A Textbook for Students and Practitioners - Christof Paar and Jan Pelzl
2. Cryptography Theory and Practice - Doug Stinson
3. Cryptography and Network Security: Principles and Practice - William Stallings
4. [Good Crypto Labs Doc](http://syllabus.cs.manchester.ac.uk/ugt/2019/COMP26120/2019labs/lab10-cryptosystems.pdf)


## Logic & Combinatorics

### Task

### Notes

- [Lecture Recording & Notes](https://drive.google.com/drive/folders/15Lri6eOKSXKxbIn4A5h4GMv5btHErxFL)
- [Meeting Link](https://us02web.zoom.us/j/88518063234?pwd=S0VZckZVcmlBUEI2VkhnZWptdXhRUT09 "https://us02web.zoom.us/j/88518063234?pwd=s0vzckzvcmlbuei2vkhnzwptdxhrut09")
	- Meeting ID: 88518063234  
	- Passcode: Cs6030@22

### References

- Discrete Mathematics and Its Applications (SIE) | 8th Edition - by KENNETH H. ROSEN and DR. KAMALA KRITHIVASAN