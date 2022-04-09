---
layout: post
title:  "Rebooting the child"
date:   2021-04-11 20:17:13 +0100
categories: realLife brain psychology
postLink: childreboot
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
A few weeks back my child had a really hard time finding sleep.

It was their mum's turn to bring them to bed, which basically _never_ posed a
a problem before but that evening was a spectacular fail.

## The way to disaster

They already woke up half an hour before the usual time, but this is not something
parents get concerned about right away. Very young children have an biological clock
but it's not as fine tuned yet and prone to be disturbed by only small events - at least 
from our experience.

Breakfast went down as usual and everything went OK for the first half of the morning.
We went through our routine of food, brushing the teeth, playing, reading, etc. .
At some point later - I only realized that on the next day or the day after - they got slightly
grumpy. Also nothing that worries me right away, if kids don't get their way, they react like that.

However, this slighty bad mood lasted till nap time in the afternoon and made it nearly impossible
to put them to sleep and even then the real time they was asleep can't have been more than
15 minutes. Which is really useless.

Well no sleep in the afternoon means earlier to bed in the evening and more mom-and-dad-time, w00t.

## Past the tipping point

So on went the afternoon with a grumpy child that only had kind of slept. The longer the day lastet
the more we were looking forward to getting them to bed.

With dinner complete and teeth brushed, my wife started out with the evening routine (changing clothes, saying
goodnight, reading and then off to bed).

When changing the clothes already became problematic and led to crying an tears we realized, we had
passed the tipping point. Putting the kid to sleep would be a long journey this evening.
Well, happened before and will happen again.

We didn't know what we were in for yet.

## Just a restart doesn't do it

I sat in the living room and had already found a movie to watch and was waiting for my wife to
get out of the kids room as soon as they fell asleep.

After twenty minutes I started listening to my current audio book.

40 minutes later my better half walked into the room, crying child in her arms and said "I give up".

I took the little one and asked her what she had tried, was met with a blank stare and an "Everything".

After getting a few more details - and she really had tried _a lot_ - I said, let's reboot them.

## "Ctrl+Alt+Del"-ish

Off I went with the child that was falling from sobbing into crying and back again. Looking at them
and feeling their reactions I saw that they was fully in crying mode. I had to distract them.
So I went through the whole going-to-be routine once more starting at changing their nappy.

Once this was done I was facing a child that was merely sniffling from time to time.
After saying goodnight again and having read another book, they had calmed down.

I put them in their bed and it only took another five minutes and they was fast asleep - no wonder after more or less an hour of crying.

## Breaking the pattern

The deeper the problem the larger the break in the pattern that causes it, needs to be.
Seasoned IT people generally know this as "restart, reboot, reinstall" ;)

It works for software, young children and developers who have retried a lot to fix a particular piece of code.

Sometimes a five minute break is enough, somestimes your brain can work it out over night and sometimes it needs
rubber ducky or a human colleague to point out the (black magic) obvious error.

The real important question is how much time you use up trying on your own. Because also in here is a tipping
point: it takes time to learn and try out tested approaches on one side and wasting time on the other.

Finding and adjusting this tipping point (dynamically) is one of the bigger challenges for developers.
For me it needed lots of trial and error as well as feedback from colleagues to get to a stage that is
not good yet, but already acceptable.
