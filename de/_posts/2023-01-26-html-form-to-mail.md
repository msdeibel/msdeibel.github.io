---
layout: post
title:  "mailto links FTW"
date:   2023-01-23 20:55:00 +0100
categories: html form mailto 
postLink: mailto
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

**tl;dr**

Manchmal ist es besser, E-Mail-Inhalte aus einem Formular zu generieren, als alle Daten in einer DB auf dem Server zu speichern.

## Schönes neues Blazor

Letztes Jahr wurde ich gebeten, ein neues Anfrageformular für einen lokalen gemeinnützigen Verein zu erstellen. Sie besitzen einen Grillplatz mit einer Hütte, die sie an die Öffentlichkeit vermieten. Die Anfragen dafür folgten keiner Vorlage und daher fehlten meist wichtige Informationen, um die Anfrage zeitnah zu bearbeiten, was zu einem Hin und Her von E-Mails und SMS-Nachrichten führte, um die fehlenden Informationen zu sammeln.

Als C#-Entwickler war ich auf der Suche nach einem kleinen Projekt, um Blazor auszuprobieren, und hier sah ich eine Chance, das "neue" Framework nicht nur auszuprobieren, sondern auch etwas Nützliches damit zu entwickeln.

Als ich einmal angefangen hatte, brauchte ich etwa vier oder fünf Abende, um eine funktionierende Lösung zu entwickeln, die Folgendes ermöglichte

- die Auswahl von Daten aus dem Datepicker
- das Deaktivieren von Terminen auf der Grundlage von Daten in einer Textdatei auf dem Server
- das Versenden von E-Mails an den Antragsteller und an die Mitarbeiter, die den Vermietungsprozess bearbeiten

Es funktionierte auf meinem Rechner und ich konnte es sogar ohne große Probleme auf Azure bereitstellen.
Zu meiner Überraschung funktionierte sogar das Aktualisieren der Textdatei auf der bereitgestellten Version, ohne dass ich obskure Azure-Datevolumes oder ähnliches erstellen musste.

Nach etwas Styling mit Bootstrap sah es sogar so aus, als würde man es benutzen wollen, wenn man versucht, einen lokalen Grillplatz zu mieten.

## Blazor als Sackgasse

Und dann kam der Alltag und die Lösung stand da und wartete darauf, live zu gehen. Einen Monat lang, zwei Monate, drei... . Als ich damit wieder anfing, fragte ich mich schließlich, wie ich das Ganze in einen produktionsreifen Zustand bringen könnte.

Aufgrund der [DSGVO] (https://en.wikipedia.org/wiki/General_Data_Protection_Regulation) Überlegungen war es keine Option mehr, es einfach in die Azure-Cloud zu stellen. Wenn man außerdem bedenkt, dass die Nutzung einer eigenen Domäne bei Azure jeden Monat Kosten verursacht, war das der letzte Sargnagel für die Blazor-Lösung.

Aber was könnte sonst noch funktionieren?

## "Low"-Tech-Lösung

Da Blazor aus dem Rennen war, begann ich, über die "einfachste" technische Lösung nachzudenken, die der Herausforderung gewachsen sein würde.

An diesem Punkt sah ich mir die Einschränkungen an, mit denen ich konfrontiert war:

1. Verbesserung der Qualität der Daten, die in der Anfrage beim ersten Kontakt enthalten sind
2. Der Server bietet nur PHP-Scripting-Unterstützung (und ich wollte _diesen_ Weg wirklich nicht einschlagen)
3. Keine persönlichen Daten auf einem Server, der 24/7 im Internet haengt 

Die nächste Lösung, die mir in den Sinn kam, war die Verwendung von JavaScript zur Änderung eines "Mailto"-Links, und diesen Weg schlug ich ein.

Diese Lösung, so hoffte ich, würde innerhalb der oben genannten Grenzen funktionieren _und_ hätte den zusätzlichen Vorteil, dass der Benutzer das letzte Wort darüber hätte, welche Daten er senden würde.

## HTML und JS FTW

Ich begann mit der Implementierung eines Formulars mit HTML, JS, ein wenig JQuery UI und PureCSS.

Für die Funktionalität habe ich zwei Abende lang an der bestehenden Seite herumgefeilt. Dann habe ich drei weitere Abende damit verbracht, die Seite mit PureCSS umzugestalten und die gesamte Seite responsiver zu machen.

Die Seite ist jetzt live und ich warte auf Rückmeldungen zum Prozess. Die Vorstandsmitglieder waren bereits mit dem ersten Versuch zufrieden, aber einige Praxistests werden zeigen, ob die Lösung gut genug ist und wo noch nachgebessert werden muss.

Übersetzt mit www.DeepL.com/Translator (kostenlose Version)
