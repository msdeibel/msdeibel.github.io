---
layout: post
title:  "Mehrsprachiges Blog mit Jekyll"
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

Ein mehrsprachiger Blog mit Jekyll ist möglich, erfordert aber ein wenig Handarbeit.

## Root cause

Als ich 2020 wieder mit dem Bloggen anfing, schrieb ich ausschließlich auf Englisch. Es gibt jedoch immer wieder Leute, die in das Feld einsteigen, für die dies ein weiteres Hinderniss sein könnte.

Ich habe meine Meinung geändert und angefangen, meine Beiträge in mindestens eine weitere Sprache zu übersetzen.
Ich [schrieb darüber, wie einfach es ist, ein paar Zeilen Prosa zu übersetzen]({% post_url 2021-07-10-posting-in-two-languages %}) im Jahr 2021.

## Quellen für Jekyll

Während die [Jekyll-Dokumentation](https://jekyllrb.com/docs/) gar nicht so schlecht ist, weiss jemand der neu im Ökosystem ist, eher nicht wo er mit der Suche anfangen soll - und wonach.

Glücklicherweise gibt es fast immer "jemanden, der es schon mal gemacht hat". In diesem Fall ist dieser Jemand
[Anthony Granger](http://chocanto.me), der 2016 [über genau dieses Thema](http://chocanto.me/2016/04/16/jekyll-multilingual.html) geschrieben hat.

Das war also mein Ausgangspunkt.

## Bastelei

Ich nahm einfach das, was Anthony als Code verwendete, kopierte es einfach in meine Jekyll-Instanz und versuchte, das Ding zu bauen.

Leider ohne großen Erfolg. Also habe ich alle Änderungen rückgängig gemacht und sie Schritt für Schritt ausprobiert.

### Konfiguration

Glücklicherweise funktionierte die erste Sache, die ich ausprobierte, direkt, oder ließ mich zumindest den Blog ohne Fehler erstellen.
Es war die Änderung der `defaults` Sektion der `_config.yml`, die für mich nun wie folgt aussieht

```yaml
defaults:
    -
     scope:
         path: ""
     Werte:
         author: "msdeibel"
         Layout: Layout:
         Sprache: de
         base-url: "/"
    -
     Bereich:
         Pfad: de
     Werte:
         author: "msdeibel"
         Layout: Layout:
         Sprache: de
         base-url: "/de/"
```

Dies weist Jekyll an, nach Markdown-Dateien zu suchen, die in den Pfaden `/_posts` und `/de/_posts` kompiliert werden sollen.

### Post head

Auch der Kopf meiner Beiträge enthält jetzt einen festen Anfang, um automatisch den entsprechenden Beitrag in der anderen Sprache zu finden und zu verlinken, falls er existiert.

Mein Markdown dafür sieht wie folgt aus

{% raw %}

```markdown
---
layout: post
title:  "Multilingual blog in Jekyll"
date: 2022-05-03 19:04:00 +0100
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

Den `postLink` habe ich von Anthony kopiert. Den Teil unter dem Frontmatter musste ich im Vergleich zu seinem Code leicht abändern. Das liegt an den unterschiedlichen Versionen von Jekyll, die wir verwenden - denke ich.

Wenn dieser Teil vorhanden ist, prüft der Compiler auf verwandte Beiträge mit dem gleichen `postLink` Eintrag in anderen Bereichen der Instanz und erstellt den Link basierend auf den Attributen im anderen Beitrag.
