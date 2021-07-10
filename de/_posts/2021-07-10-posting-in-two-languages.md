---
layout: post
title:  "Blogposts in zwei Sprachen"
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
# Eine Sprache ist nicht genug
Als ich im November 2020 wieder angefangen habe zu bloggen hat sich für mich die Frage nach der Sprache überhaupt nicht gestellt.
Ich arbeite in der IT Welt, es geht hautsächlich um IT Themen, also ist die Blogsprache Englisch.

Als ich allerdings im März das Post [Pflichtparameter mit Standardwerten in Powershell-Skripten]({% post_url 2021-03-12-powershell-script-parameters %}) veröffentlichte, hat sich das geändert.

Im Laufe der Suche nach dem passenden Code bin ich praktisch keiner _aktuellen_ deutschsprachigen Information zu dem Thema begegnet 
und das hat mich irgendwie nachdenklich gestimmt.

Klar ist die IT Welt englisch geprägt, aber vor allem junge Einsteiger, deren technisches Vokabular noch nicht so ausgeprägt ist,
werden hier vor eine relativ hohe Hürde gestellt. Nicht nur, dass sie nicht wissen wonach sie suchen müssen, nein, sie müssen auch
noch den richtigen Suchbegriff in einer anderen Sprache finden.

# Alles selber übersetzen? Nein danke.
Wie schon gesagt, ich bin ITler und es geht um IT Themen. Und obwohl meine Englischkenntnisse ausreichend sind, um hin und her zu übersetzen möchte ich einfach nicht die Zeit dafür aufwenden, um für _alles_ die korrekte Formulierung zu finden und es dann auch nochmal zu tippen.

Hierfür gibt es im 21. Jahrhundert online Dienste, die das in Sekundenschnelle automatisch erledigen.
Dann bleiben nur noch kleine Korrekturen von Hand und statt 20 Minuten dauert die Übersetzung dann nur noch fünf.

## Der Übersetzungsservice meiner Wahl
Da ich beruflich bereits mit [DeepL](https://deepl.com) zu tun hatte, habe ich es auch für mein Blog ausprobiert und war sofort wieder begeistert von der Leistungsfähigkeit.

Das o.g. Blogpost war das erste, das ich vom Deutschen ins Englische übersetzen ließ und fast alles hat auf anhieb gepasst.

Die meisten technischen Begriffe wurden korrekt übersetzt oder beibehalten. Die Wortwahl war meiner Meinung nach sehr gut und auch die meisten mehrdeutigen Begriffe wurden richtig übersetzt.

Und obwohl die KI von DeepL noch nicht alle Idiome korrekt übersetzt, minimiert sie den Restaufwand für die Kopf- und Handarbeit immens.

## Ausprobieren
Zum selber Ausprobieren empfehle ich, einfach mal den Inhalt des o.g. Blogposts so wie er ist von der HTML Seite zu kopieren und in DeepL einzufügen. Wer ausreichende Kenntnis in der Zielsprache hat wird dann relativ schnell beurteilen können, ob dem dortigen Team eine guten Entwicklung gelungen ist.

# Was kommt als nächstes?
Die Übersetzung von einem Computer erstellen zu lassen, ist natürlich nur die eine Hälfte. Die andere Hälfte ist die parallele Veröffentlichung. Das ist mit einem statischen Werkzeug wie Jekyll zwar möglich, aber nicht ganz einfach. Leider gab es auch hier wieder nur englischsprachige Infos im Netz und dann waren da noch die kleinen aber feinen Unterschiede zwischen den Versionen, aber dazu mehr in einem der nächsten Beiträge.