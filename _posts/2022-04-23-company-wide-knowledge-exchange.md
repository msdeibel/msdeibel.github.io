---
layout: post
title:  "Firmenweiter Wissensaustausch"
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
Es ist jetzt schon einen Weile her, da wurde ich mit der Aufgabe betraut, die Verschluesselung
in einem existierenden Projekt zu verbessern.

Die Aufgabe hat zunaechst in mir kein Hochgefuehl ausgeloest, da ich seit dem Studium (und das ist
schon eine Weile her), Verschluessellung praktisch nur als Endnutzer verwendet habe, aber mit den Details
der Implementierung eher weniger bewandert bin.

Da es meinen KollegInnen im Projekt aber aehnlich ging, war es dann auch egal auf wen das Los fiel.

## Hintergrund

Der Verschluessellungscode, um den es ging war bei einem PEN Test aufgefallen und als erhoehtes Risiko
fuer einen Angriff eingestuft worden. Die besagte Software ist zwar nicht direkt mit dem Backend im Web verbunden,
kann aber zur Datenmanipulation verwendet werden. Die Daten die in dem Programm verarbeitet werden, werden danach
per Import in das eigentliche Produkt importiert.

Die bis dahin verwendete Verschluessellungsmethode ist allerdings nicht aus versehen implementiert worden,
sondern basiert auf spezifischen Anforderung der fachlichen Seite des Systems. Man kann also den vorherigen
Entwicklern hier keinen Vorwurf machen.

## Erstmal Code lesen

Ich bin dann damit gestartet mir den eigentlichen Code anzusehen. Wie gesagt, ich bin keine Experte fuer
Software-Sicherheit, aber vielleicht koennte ich ja Anknuepfungspunkte finden, die sogar fuer mich
offensichtlich sein muessten.

Tatsaechlich bin ich auf diesem Weg auch fuendig geworden (s. [PBKDF2 und Argon2]({% link de/_posts/2021-06-03-pbkdf2-and-argon2.md %})

## Infos sammeln

Mit dem was ich im Code vorgefunden hatte, machte ich mich auf die Suche im Internet.

Doch wie wird man fuendig, wenn mach nicht weiss wonach man wirklich sucht? Ich musste die gefundenen
Informationen also irgendwie einordnen. Doch das ist ohne Basiswissen zu Verschluesselungsalgorithmen zum einen schwer und zum anderen gefaehrlich.
Man will ja nicht noch groessere Sicherheitsluecken aufreissen.

## Firmennetzwerk anzapfen

Es gibt mit Reddit, Slack, Stackoverflow etc. jede Menge Seiten und Dienste auf denen man Wissen von
Experten geboten bekommt, aber manchmal eben nicht zeitnah und qualitativ doch fragwuerdig.

Wenn man, so wie ich, bei einer Software-Firma angestellt ist, sollte man auch hier alle Hebel in Bewegung setzen,
um evtl. an das benoetigte Wissen zu kommen. Unfassbarer Weise ist es naemlich so, dass die KollegInnen in der
Regel auch helfen wollen wo sie nur koennen. Selbst wenn man sich vorher noch nie persoenlich kennengelernt hat.

Hierzu muss man natuerlich die Kommunikationswege im Unternehmen kennen. Bei uns wird [Rocket Chat](https://rocket.chat/) als
Slack-Alternative verwendet. Es gibt einen grossen Kanal, in dem fast alle MitarbeiterInnen vertreten sind und
mindestens ein Dutzend ist auch immer online.

## Ich weiss wer dir helfen kann

Natuerlich kann nicht jede/r die/der in diesem Chat mitliest direkt helfen, aber es koennen auf diesem Weg
schnell ein paar Personen angetriggert werden. Unter dem Motto, "ich kann dir nicht helfen, aber schreib mal Simona an,
die hat das in einem vorherigen Projekt auf jeden Fall schon mal gemacht."

Auf dem Weg habe ich dann einen unserer Geschaeftsfuehrer ans Telefon bekommen, der mir in 15 Minuten, alles erklaert
hat was ich zur Loesung des Problems brauchte.

In so einer Umgebung/Firmenkultur schlagen kurze Kommunkationswege fast alles was das Internet zu bieten hat.
