---
layout: post
title:  "Company wide knowledge exchange"
date:   2022-04-23 07:51:53 +0100
categories: knowledge information exchange culture
postLink: knowledgeexchange
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
It's been a while now since I was given the task of improving the encryption in an existing project.

The task didn't initially make me feel very good, because since university (and that was a while ago) I had used encryption practically only as an end-user, but I am less familiar with the details of the implementation.

Since my colleagues in the project felt the same way, it didn't matter who tackled the task.

## Background

The encryption code in question had come to light during a PEN test and had been classified as an increased risk for an attack.
The software in question is not directly connected to the backend on the web,
but can be used to manipulate data. The data processed in the programme is then imported into the actual product.

The encryption method used up to this point was not implemented by accident, however,
but is based on specific requirements of the technical side of the system. So one cannot blame the previous developers for this.

## First read the code

I then started to look at the actual code. As I said, I am not an expert in software security, but maybe I could find some points of contact that should be obvious even to me.

In fact, this is how I found it (see [PBKDF2 and Argon2]({% post_url 2021-06-03-pbkdf2-and-argon2 %}))

## Gather info

With what I had found in the code, I started searching the internet.

But how do you find what you are looking for if you don't know what you are really looking for? I had to sort out the information I had found. But without basic knowledge of encryption algorithms, this is both difficult and dangerous.
You don't want to open up even bigger security gaps.

## Tapping into the corporate network

With Reddit, Slack, Stackoverflow etc. there are lots of sites and services where you can get knowledge from experts, but sometimes not in a timely manner and of questionable quality.

If, like me, you are employed by a software company, you should pull out all the strings here too, to get the knowledge you need. Unbelievably, colleagues usually want to help wherever they can. Even if you have never met in person before.

Of course, you have to know the communication channels within the company. We use [Rocket Chat](https://rocket.chat/) as a Slack alternative. There is one big channel in which almost all employees are present and at least a dozen are always online.

## I know who can help you

Of course, not everyone who reads this chat can help directly, but it can be a quick way to trigger a few people. Under the motto, "I can't help you, but write to Simona, she's definitely done this before in a previous project."

On the way I got one of our managers on the phone, who explained everything I needed to know to solve the problem in 15 minutes.

In such an environment/corporate culture, short lines of communication beat almost anything the internet has to offer.
