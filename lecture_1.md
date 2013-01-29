#Crypto B

##Lecture 1

No problem class, 2 lectures.

* Follow the book
* [Introduction to modern cryptography](http://www.amazon.co.uk/Introduction-Modern-Cryptography-Principles-Protocols/dp/1584885513)
* Scribe a lecture: get 5%
* Office hours: 2-3 on thursdays
* Some lectures won't need a scribe, material on the web

* Why aren't lectures recorded?
   * it won't help, what's on the board is what matters
   * university hasn't got it set up yet
   * reading lecture notes > other things
   * Ian is doing it at the moment

* Course relies on Crypto A
   * Crypto B uses lots of knowledge from Crypto A, make sure you know it

* RSA mathematics required in Crypto A, in the exam.
* Mathematics not in this course, less mathematics (except for today's lecture)

* In this course we will study
* pyramid of construction

```
      /   ecommerce, e-voting                                                             \
     /   protocols: tls, key exchange                                                      \
    /   more advanced primitives                                                            \
   /   Simple cryptographic constructions -> sym enc, assy enc, digital sig, hashing, MAC    \
  /   Basic mathematical problems -> RSA, Factoring, DLP, AES, DH                             \

```

* TLS -> know who you are talking to, communication is private and authenticated (messages are authenticated)
* encryption -> hiding
* MAC/hash -> ensuring method is valid

* study some parts of the pyramid
* start with basic primitives

* rsa digital signature: full domain hash: hash and encrypt with private key

###Classifying primitives

* classify by hardness to solve?
* if rsa is hard, I can construct digital signatures
* if factoring is hard you can do signatures and assymetric
* AES is a symmetric primitive, encryption and mac
* other primitives are asymmetric (dlog, rsa, factoring and dh)
* A closer look at AES's uses will happen
* Below RSA is a "one way function"
* If one way functions don't exist, then nothing exists in cryptography (apart
  from one time pad)
* What makes modern cryptography modern?
    * been proved
    * used to work on language instead of computers
    * cerchov's principle
    * (bogdan's answer) when you look at the primitive, you have to define what
      the primitive does, the syntax of the primitive. What does security mean
      for the primitives. Come up with the protocol and someone would broke it.
      Redesign the broken protocol. Interactively until no-one can attack your system.

###Security Models

```
 +------------------------+
 |                        | <--------> Adversary  Execution Model
 |     crypto system      |
 |                        |
 +------------------------+

```

* What is a break?
* Adversary can interact with the scheme
* Chosen plaintext attacks
    * Choose messages, primtives return encryption of a chosen message
    * Broke when he can guess which message is encrypted with better than
      one in two chance.

* Digital signature model
    * Adversary can interact with the digital scheme, gets verification key
    * syntax: Key generation, how to sign, how to verify.


####Security model

1. System does key generation V<sub>k</sub> S<sub>k</sub> (verification and
   signing key)
2. Signature system sends verification key to adversary
3. Adversary sends m to signature system
4. System sends signature sigma back to adversary
5. Adversary wins if he can create a valid signature for a message which has
   not been signed by the system (no one can create valid signatures on
   messages I have not signed).

Scheme is called EUF (exestential unforgability) CMA (chosen message attack).
Schemes are different from security models (AES is different from IND-CPA).

EUF is the goal of the adversary, and CMA is the attack model. PASS is a weaker
model because he can't chose the messages.

If a scheme is secure under EUF-PASS does it mean it is secure under EUF-CMA?
No, because in PASS the attacker has significantly less power. However the
inverse applies, CMA gives the attacker more power than PASS


####Negligible functions

Keys have some sizes. If keys are 2 bits long, is there any hope for security?
2 bits is 4 tries, 80 bits is 2<sup>80</sup> tries.

What we would like with large n is

![eq1](http://mathbin.net/equations/150955_1.png).

However this is impossible in most crypto systems so instead we go for

![eq2](http://mathbin.net/equations/150955_2.png).

(that is the probability of breaking is very small with sufficiently large n)

We equate this to negligible functions:

![eq3](http://mathbin.net/equations/150955_3.png)

An informal definition of a negligible function is:

```
a function f is negligible if it is smaller than the inverse of any polynomial.
```

![graph1](http://samphippen.com/graph.jpg)

Blue is 1/x, Green is 1/x<sup>2</sup> and Red is 1/x<sup>3</sup>

Formal definition of a negligible function:

![eq4](http://mathbin.net/equations/150971_4.png)

1/n<sup>2</sup> is not negligible as some other polynomial functions beat it.
An example of a negligible function is 1/2<sup>n</sup>.

If the adversary is polynomial he can run in polynomial time. Given A we construct
B that does the following:


g(n) is some polynomial

```
for i = 1 to g(n)
    run a against the system
```

the chance that B beats the system may be better than A.

```
Prob(BREAK<sub>n</sub>(B)) <= prob of A breaking many times <= g(n) * Prob(BREAK<sub>n</sub>(A))
```

Negligibility comes in because if the probability of A breaking the system is
negligible then the probability of g(n) times it is also negligible. In other words
repeating the attack will not work.

###Exercises for next week

* Prove that if f is negligible then
    1. 2f is neglible
    2. g(n)f(n) is negligible
* f and g are negligible functions then f+g is negligible (challenge)
