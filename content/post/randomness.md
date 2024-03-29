+++
date = "2015-09-03T14:05:38+01:00"
title = "Randomness"
tags = [ "security", "cryptography" ]

+++

What makes somethings random? Which of the following sequences is more random?

<!--more-->

<pre>
O <b>X</b> O <b>X</b> <b>X</b> O <b>X</b> <b>X</b> O <b>X</b> O <b>X</b> <b>X</b> O <b>X</b> O O <b>X</b> O <b>X</b> O
</pre>

<pre>
<b>X</b> <b>X</b> <b>X</b> O O <b>X</b> O O O <b>X</b> <b>X</b> <b>X</b> <b>X</b> O O O <b>X</b> <b>X</b> <b>X</b> <b>X</b> <b>X</b>
</pre>

Most of us would probably say the first sequence looks more random than the
second. This is because we as human beings are notoriously bad at judging
randomness, which is the source of many fallacies and biases (such as the
gambler's fallacy).

I *made up* the first sequence to make it appear random. The second one was
obtained by flipping a real 2 Euro coin and writing down the results.

Why does the first sequence look more random? Because we don't trust long
sequences of the same element. For example consider the final 5 Xs in the
second sequence. If I were to flip the coin again how much would you bet on it
being another X (tails) or O (heads). Our intuition tells us that O 'is due'
(a.k.a. the gambler's fallacy). But the truth is that the coin has no memory and
both results are equally likely *no matter what has happened before*.

If you were to flip a coin 10 times it is not unlikely for you to get a very
uneven result (e.g. 7 heads and 3 tails) even though the probability of each one
is 50%. But if you flip a coin 10.000.000 times then it is almost impossible
that you'd get 7.000.000 heads and 3.000.000 tails. You would get around
5.000.000 each. This is called the law of big numbers. Things tend to even out
in the long run. You will have very long runs of only heads but then you will
also have long runs of only tails, and in the end they even out (more or less).

This is the reason why statistically speaking small samples are unreliable. It
is more likely to get extreme results!

Imagine a country where people like to eat 10 types of candy. Each has a
different flavour (e.g. chocolate, strawberry, ...). And imagine the preference
for a certain flavour was evenly distributed across the whole country. 10% of
people love chocolate, 10% strawberry etc. You are the representative of the
candy-manufacturing company and you want to know if production should favour
one flavour over the other. Therefore you want to conduct a survey. You choose
the smallest of samples: 1 person. You ask just one person. This person tells
you he loves pineapple flavoured candy. You deduce from your data that 100% of
people eat pineapple candy. Now imagine you choose a bigger sample of 10
people. Do you think it likely that there will be exactly one person per
flavour leading to the correct 10% each flavour result? It is unlikely that
this will happen, therefore you will still get results that don't match the
reality of what people like.

## How to tell if something is random?

Short answer: You can't. At least not with certainty. Why is that? Because in a
random sequence every possible outcome is possible, even the ones that
intuitively don't look random (as seen at the beginning). Thus assume someone
gives you the sequence 11111111111111111 and asks you if it is random. You
cannot tell. It does not look random, but possibly it came out of a true random
number generator, or it is the result of someone flipping a coin. 

What you can do is perform some mathematical tests to see if the sequence you
got looks random. But it is both possible to generate a sequence that looks
random but isn't (you just need to know the rules used for checking) and it is
possible to get a random sequence that does not pass the tests and does not
look random.

The only thing you can do is choose your random number generator properly and
trust it.

## Why does it matter?

Randomness is hugely important for computer cryptography. Very often secret
keys are generated using a random number generator. If the generator is bad,
the key will be bad and can potentially be guesses/calculated (see link below
*How I Met Your Girlfriend*).

## PRNG (Pseudo random number generator)

A PRNG generates *random* numbers in some arithmetic (mathematical) way and is
repeatable. One basic example is the *Middle square method* that was described
in the middle ages and re-invented by John von Neumann.

We take some number (we call it the *seed*) and multiply it by itself. Then we
take the middle digits of the result as the first *random* number. Repeat the
process with this new *random* number.

For instance if the seed is 42:

    42 * 42 = 1765
    The middle digits are 76
    76 * 76 = 5776
    The middle digits are 77
    77 * 77 = 5929
    The middle digits are 92
    etc.

This gives us following sequence of numbers: 76, 77, 92, 46, 11, 12, 14, 19, 36, 29, 84, 5, 2

They look pretty random don't they?

The main problem with PRNGs is that they tend to form *circles* where values repeat themselves.

For instance if we take 79 as the seed:

    79 * 79 = 6241
    The middle digits are 24
    24 * 24 = 576
    The middle digits are 57 (0576)
    57 * 57 = 3249
    The middle digits are 24
    24 * 24 = 576
    The middle digits are 57 (0576)
    57 * 57 = 3249
    The middle digits are 24
    24 * 24 = 576
    The middle digits are 57 (0576)
    etc.

The resulting sequence is therefore: 79, 24, 57, 24, 57, 24, ...

An attacker who observes the output of the PRNG will be able to predict the
next value before it happens. These values are no longer random.

Another problem with the *Middle square method* is that if the middle digits
are 00 then all following values will be 00 as well (because 0 * 0 = 0).

This is what happens with the first sequence above (the one that started with
42 * 42 = 76). If we continue:

    2 * 2 = 4
    The middle digits are 00 (0004)
    0 * 0 = 0
    The middle digits are 00 (0000)
    0 * 0 = 0
    etc.

Kevin Mitnick describes an amusing story in *The Art of Intrusion* about
hacking a Casino machine by timing the exact moment to hit the *Play* button.
They had found out what kind of PRNG (a LFSR - Linear Feedback Shift Register)
the machine used and by playing once and then entering the cards displayed by
the machine into their computer they were able to calculate how long it would
take the machine to make the full *circle* and get back to *Royal Flush*.

The big advantage of PRNGs is that they are very cheap and very fast. And being
able to repeat the values (by supplying the same seed as the first time) is
also an advantage when you only want to test your machine or software.


## True Random Number Generator

A TRNG (True Random Number Generator) is a hardware device that observes some
unpredictable physical event and records the results. For example you might
build a machine that observes a casino roulette and outputs the results as
random numbers. In practice this machine would be very slow and therefore
expensive but it would work.

Very often microscopic events such as thermal noise, radioactive decay,
photoelectric effect etc. are used because of their unpredictable nature.

It is possible to do this in software by measuring (in theory) unpredictable
events such as the time between keystrokes, mouse movements, arrival time of
network packets etc. In theory a very sophisticated attacker could manipulate a
lot of these events but in practice it seems to be good enough.


# More info

* [Gambler's fallacy](https://en.wikipedia.org/wiki/Gambler%27s_fallacy)
* [Pseudorandom Bits and Sequences](http://cs.ucsb.edu/~koc/ns/docs/slides/14-pgp/chap5-prng.pdf)
* [Hardware random number generator](https://en.wikipedia.org/wiki/Hardware_random_number_generator)
* [Cryptographically secure pseudorandom number generator](https://en.wikipedia.org/wiki/Cryptographically_secure_pseudorandom_number_generator)
* [DEFCON 18: How I Met Your Girlfriend - Part 1](https://www.youtube.com/watch?v=fEmO7wQKCMw),
  [Part 2](https://www.youtube.com/watch?v=2ctRfWnisSk),
  [Part 3](https://www.youtube.com/watch?v=vJtmZZGcR54)
