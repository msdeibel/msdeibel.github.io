---
layout: post
title:  "Blogging in two languages"
date:   2021-07-10 07:29:00 +0100
categories: blogging reach blastradius
postLink: 2languages
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
# One language is not enough
When I started blogging again in November 2020, the question of language didn't arise for me at all.
I work in the IT world, it's mostly about IT topics, so the blog language is English.

However, when I published the post [Mandatory parameters with default values in Powershell scripts]({% post_url 2021-03-12-powershell-script-parameters %}) in March, that changed.

In the course of searching for the appropriate code, I basically didn't come across _any_ up-to-date German-language information on the subject and that got me thinking.

Of course, the IT world is dominated by the English language, but especially young beginners, whose technical vocabulary is not yet so well-developed,
are faced with a relatively high hurdle. Not only do they not know what to look for, they also have to find the right search term in a different language.

# Translate everything yourself? No, thank you.
As I said, I'm an IT professional and it's about IT topics. And although my English skills are sufficient to translate back and forth, I just don't want to spend the time to find the correct wording for _everything_ and then type it again.

In the 21st century, there are online services that do this automatically in seconds.
Then all that's left are small corrections by hand and instead of 20 minutes, the translation then only takes five.

## The translation service of my choice
Since I had already used [DeepL](https://deepl.com) professionally, I also tried it out for my blog and was again impressed by its performance.

The above blog post was the first one I had translated from German into English and almost everything was right on the first try.

Most of the technical terms were translated correctly or retained. The choice of words was very good in my opinion and even most ambiguous terms were translated correctly.

And although the AI of DeepL does not yet translate all idioms correctly, it minimises the remaining effort for head and hand work immensely.

## Try it out
To try it out for yourself, I recommend simply copying the content of the above blog post as it is from the HTML page and pasting it into DeepL. If you have sufficient knowledge of the target language, you will then be able to judge relatively quickly whether the team there has managed a good development.

# What comes next?
Having the translation done by a computer is, of course, only one half. The other half is parallel publication. This is possible with a static tool like Jekyll, but not easy. Unfortunately, there was again only English-language information on the net and then there were the small but subtle differences between the versions, but more on that in a future post.