+++
date = "2015-03-28T14:05:38+01:00"
draft = true
title = "Randomness"
categories = [ "Security", "Cryptography" ]

+++

What makes somethings random? Which of the following sequences is more random?

<pre>
O <b>X</b> O <b>X</b> <b>X</b> O <b>X</b> <b>X</b> O <b>X</b> O <b>X</b> <b>X</b> O <b>X</b> O O <b>X</b> O <b>X</b> O
</pre>

<pre>
<b>X</b> <b>X</b> <b>X</b> O O <b>X</b> O O O <b>X</b> <b>X</b> <b>X</b> <b>X</b> O O O <b>X</b> <b>X</b> <b>X</b> <b>X</b> <b>X</b>
</pre>

Most of us would probably say the first sequence looks more random than the
second. This is because we as human beings are notoriously bad at judging
randomness, which is the source of many fallacies and biases (such as the
gamers fallacy).

I *made up* the first sequence to make it appear random. The second one was
obtained by flipping a real 2 Euro coin and writing down the results.

Why does the first sequence look more random? Because we don't trust long
sequences of the same element. For example consider the final 5 Xs in the
second sequence. If I were to flip the coin again how much would you bet on it
being another X (tails) or O (heads). Our intuition tells us that O 'is due'
(a.k.a. the gamer's fallacy). But the truth is that the coin has no memory and
both results are equally likely *no matter what has happened before*.

If you were to flip a coin 10 times it is not unlikely for you to get a very
uneven result (e.g. 7 heads and 3 tails) even though the probability of each on
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
intuitively don't look random (as seen in the article at the beginning). Thus
assume someone gives you the sequence 11111111111111111 and asks you if it is
random. You cannot tell. It does not look random, but possibly it came out of a
true random number generator, or someone flipping a coin. 

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
the key will be bad and can potentially be guesses/calculated. (see How I Met
Your Girlfriend)

## PRNG (Pseudo random number generator)

blah blah

## True random generators

asdf
