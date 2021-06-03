---
layout: post
title:  "PBKDF2 und Argon2"
date:   2021-06-03 07:08:00 +0100
categories: security encryption .Net
postLink: argon2
---
{% assign relatedPosts=site.documents | where:"postLink", page.postLink | sort: 'language' %}

<div class="language">
|
    {% for p in relatedPosts %}
      <a class="{{ p.language }}" href="{{ site.base-url }}{{ p.url }}">{{ p.language }}</a> |
    {% endfor %}
</div><br/>
<hr>
<br/>
# or: how can I generate secure(r) keys from insecure passwords?
In the history of cryptology, one piece of wisdom has prevailed: even if the encryption method is known, it must not be possible for an attacker to decrypt the data.
The only point of attack must be the key and this must be kept secret by the sender and receiver.

However, this requirement only makes sense under one condition, namely if it is not possible for an attacker to find out the key in a period of time in which it is useful for them to decrypt the message.

## Long passwords are better than complex ones
Computers have made finding keys (passwords) extremely fast because they are good at counting (increasing values), comparing (detecting equality) and performing operations repeatedly and in parallel (speed) - in short, combinatorics.

This makes it clear that I can counter a computer well by increasing the number of possible combinations for passwords as much as possible.

### A comparison
Let's take an alphabet with ten characters, e.g. the numbers 0-9, and compare it with an alphabet of 26 characters, e.g. A-Z.

Then we get the following (approximate) values for the number of possible combinations:

<table>
    <tr><th>Number of characters<br/>in the password</th><th colspan="2">alphabet size</th></tr>
    <tr><td></td><th>10</th><th>26</th></tr>
    <tr><th>5</th><td>10<sup>5</sup></td><td>10<sup>7</sup></td></tr>
    <tr><th>15</th><td>10<sup>15</sup></td><td>10<sup>18</sup></td></tr>
</table>

So, from a combinatorial point of view, the number of characters in the password is much more important than the number of characters in the alphabet from which the password is formed.

## They forced me ...
But what happens when the general conditions force us to use bad passwords? As a result, the passwords and thus the system they protect become easy targets for bruteforce attacks.

This is what happened in a project I worked on.

One requirement implied the use of insecure passwords (alphabet with 8 characters and password length 8 - ~16.7 million combinations) as a prerequisite, because it could not be implemented otherwise.

The problem here was that a key is generated from the password, which is used to encrypt important, sometimes personal, data. So it's fair to say that this is not entirely unimportant data.

### Creating artificial complexity
In order to be able to generate secure keys for encryption tasks even with relatively weak passwords, clever people have invented the so-called _Password Based Key Derivation Functions_ (PBKDFs).

These functions are then run x times and generate a hash of the password, which depends on the number of runs.

#### The simple variant
With the variant [PBKDF2 (Wikipedia)](https://de.wikipedia.org/wiki/PBKDF2), this takes one millisecond for a few runs, and ten (or more) seconds for many, i.e > 100,000.

On the one hand, this slows down the software during encryption and decryption, but it also makes life difficult for an attacker.

Even the small number of 8<sup>8</sup> possible combinations becomes an insurmountable obstacle, because it no longer takes 16,800s (~4.5 hours) to try out all combinations, but 1,680,000,000s (450,000 hours, ~19,400 days).

#### With inert gas against special attacks
However, just increasing the number of runs is vulnerable to two special attacks.
1. extreme parallelisation, as is feasible with graphics cards
2. mapping the algorithm in hardware, as is possible with _Field Prgrammable Grid Arrays_ (FPGAs).

This is where [Argon2 (Wikipedia)](https://de.wikipedia.org/wiki/Argon2) comes in. The winner of the _Password Hashing Competition 2015_ works fundamentally like the other algorithms - more passes lead to longer waiting times.

However, it has two more tricks up its sleeve that can protect against the above-mentioned special attacks.

One of the call parameters is the maximum number of cores to be used in parallel during encryption/decryption, which destroys the possibility of a faster attack via the approach with many cores of a GPU.

Another parameter determines how much RAM the algorithm should allocate during the calculation.
Since FPGAs are usually used for embedded and hardware-related programming, they come with memory in the range of 256 or 512MB. FPGAs with high memory (>2GB) are rare and/or very expensive.

So if the algorithm wants to use 4GB but doesn't get it, it won't work. Thus, this attack vector is also severely hampered.

## Everything has a price
As already mentioned above, the weak passwords cause additional effort. This effort is reflected in computing time and thus _waiting time_ on the part of the users.
When using the corresponding algorithms, it must therefore be determined in the project what waiting time is acceptable for the users and the settings adjusted accordingly.

## PBKDF2 and Argon2 in C#
While PBKDF2 is available as [System.Security.Cryptography.Rfc2898DeriveBytes (MS Docs)](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rfc2898derivebytes?view=net-5.0) in C#, Argon2 has to be added via a Nuget package. From the few available implementations, we implemented a POC using [Konscious.Security.Cryptography (Github)](https://github.com/kmaragon/Konscious.Security.Cryptography).
