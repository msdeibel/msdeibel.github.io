---
layout: post
title:  "Multilingual blog in Jekyll"
date:   2022-05-03 19:04:00 +0100
categories: blogging technology jekyll hacking
postLink: multilingual
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

## tl;dr

A multilingual blog with Jekyll is possible, but needs a bit of manual work.

## Root cause

When I restarted blogging in 2020 I wrote exclusively in English. However there are always people who come into the field for whom this might be another fence that's keeping them out.

I changed my mind and started translating my posts to at least one other language.
I [wrote about how easy it is to translate a few lines of prose]({% post_url 2021-07-10-posting-in-two-languages %}) back in 2021.

## Sources for Jekyll

While the [Jekyll documentation](https://jekyllrb.com/docs/) is not at all bad, for someone who is new to the ecosystem,
you might know where to start searching - and for what.

Luckily there is almost always "someone who's done it before". In this case this someone is
[Anthony Granger](http://chocanto.me) who [wrote about this very topic](http://chocanto.me/2016/04/16/jekyll-multilingual.html) in 2016.

So this was my starting point.

## Piecing things together

I just took what Anthony used as code and blatantly copied it over to my instance of Jekyll and tried to build the thing.

Unfortunately without much success. So I backed out all the changes and tried them step by step.

### Configuration

Luckily the first thing I tried already worked, or at least let me build the blog without errors.
It was the change to `defaults` section of the `_config.yml` which for me now looks like this

```yaml
defaults:
    -
     scope:
         path: ""
     values:
         author: "msdeibel"
         layout: Layout
         language: en
         base-url: "/"
    -
     scope:
         path: de
     values:
         author: "msdeibel"
         layout: Layout
         language: de
         base-url: "/de/"
```

This basically tells Jekyll to look for markdown files to compile in the `/_posts` and `/de/_posts` path.

### Post head

Also the head of my posts now contains a fixed start to automagically find and link to the corresponding post in the other language, should it exists.

My markdown for this is

{% raw %}

```markdown
---
layout: post
title:  "Multilingual blog in Jekyll"
date:   2022-05-03 19:04:00 +0100
categories: blogging technology jekyll hacking
postLink: multilingual
---
{% assign relatedPosts=site.documents | where:"postLink", page.postLink | sort: 'language' %}

<div class="language">
|
    {{% for p in relatedPosts %}}
      <a class="{{ p.language }}" href="{{ site.base-url }}{{ p.url }}">{{ p.language }}</a> |
    {% endfor %}
</div><br/>
<hr>
<br/>
```

{% endraw %}

The `postLink` is something I copied from Anthony. The part below the frontmatter I had to slightly modify compared to his code. This is due to the different versions of Jekyll we use - I guess.

With this part in place the compiler checks for related posts with the same `postLink` entry in other scopes of the instance and creates the link based on the attributes in the other post.

