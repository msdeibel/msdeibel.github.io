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
# oder: wie kann ich aus unsicheren Passworten sichere(re) Schlüssel erzeugen
In der Geschichte der Kryptologie hat sich eine Weisheit durchgesetzt: Selbst wenn das Verfahren zur Verschlüsselung bekannt ist, darf es einem Angreifer nicht möglich sein, die Daten zu entschlüsseln.
Der einzige Angriffspunkt darf der Schlüssel sein und dieser muss von Sender und Empfänger geheim gehalten werden.

Diese Voraussetzung hat aber nur unter einer Voraussetzung einen Sinn, nämlich dann wenn es einem Angreifer nicht möglich ist, den Schlüssel in einem Zeitraum herauszufinden, in dem ihm die Entschlüsselung der Nachricht von Nutzen ist.

## Lange Passwörter sind besser als komplexe
Computer haben das Auffinden von Schlüsseln (Passworten) extrem beschleunigt was daran liegt, dass sie gut darin sind zu zählen (Werte zu erhöhen), zu vergleichen (Erkennung von Gleichheit) und Operationen wiederholt und parallel auszuführen (Geschwindigkeit) - kurz Kombinatorik.

Damit wird klar, dass ich einem Computer gut entgegentreten kann wenn ich die Anzahl der möglichen Kombinationen für Passwörter so weit wie möglich erhöhe.

### Ein Vergleich
Nehmen wir ein Alphabet mit zehn Zeichen, z.B. die Zahlen 0-9, und vergleichen es mit einem Alphabet aus 26 Zeichen, z.B. A-Z.

Dann ergeben sich daraus folgende (ungefähre) Werte für die Anzahl möglicher Kombinationen:

<table>
    <tr><th>Anzahl Zeichen</br>im Passwort</th><th colspan="2">Alphabetgröße</th></tr>
    <tr><td></td><th>10</th><th>26</th</tr>
    <tr><th>5</th><td>10<sup>5</sup></td><td>10<sup>7</sup></td</tr>
    <tr><th>15</th><td>10<sup>15</sup></td><td>10<sup>18</sup></td</tr>
</table>

Die Anzahl der Zeichen im Passwort ist also aus kombinatorischer Sicht wesentlich wichtiger als die Anzahl der Zeichen des Alphabetes aus dem das Passwort gebildet wird.

## Sie haben mich gezwungen ...
Was aber passiert wenn die Rahmenbedingungen uns zur Verwendung von schlechten Passwörtern zwingen? In der Folge werden die Passwörter und damit das System, dass sie schützen zu leichten Zielen für Bruteforce Attacken.

So passiert im Rahmen eines Projekts an dem ich arbeite.

Eine Anforderung hat die Verwendung von unsicheren Passworten (Alphabet mit 8 Zeichen und Passwortlänge 8 - ~16,7 Mio Kombinationen) zur Voraussetzung, da sie sich anders nicht umsetzen lässt.

Das Problem hierbei ist, dass aus dem Passwort ein Schlüssel generiert wird, mit dem wichtige, zum Teil persönliche, Daten verschlüsselt werden. Man kann also durchaus sagen, dass das keine ganz unwichtigen Daten sind.

### Künstliche Komplexität schaffen
Damit man selbst mit relativ schwachen Passwörten sichere Schlüssel für Verschlüsselungsaufgaben erzeugen kann, haben gewiefte Menschen die sog. _Password Based Key Derivation Functions_ (PBKDFs) erfunden.

Diese Funktionen werden dann x mal durchlaufen und erzeugen einen Hash des Passwortes, der von der Durchlaufzahl abhängt.

#### Die einfache Variante
Mit der Variante [PBKDF2 (Wikipedia)](https://de.wikipedia.org/wiki/PBKDF2) dauert das bei wenigen Durchläufen eine Millisekunde, bei vielen - > 100.000 - dann eben zehn (oder mehr) Sekunden.

Auf der einen Seite verlangsamt das zwar die Software beim Ver- und Entschlüsseln, allerdings macht es auch das Leben für einen Angreifer schwer.

Dann wird selbst die niedrige Zahl von 8<sup>8</sup> möglichen Kombinationen zu einem unüberwindlichen Hinderniss, denn es dauert nicht mehr 16.800s (~4,5 Stunden) alle Kombinationen durchzuprobieren sondern, 1.680.000.000s (450.000 Stunden, ~19.400 Tage).

#### Mit Edelgas gegen Spezialangriffe
Nur die Anzahl der Durchläufe zu erhöhen ist aber anfällig gegen zwei Spezialangriffe
1. Extreme Parallelisierung, wie sie mit Grafikkarten machbar ist
2. Abbildung des ALgorithmus in Hardware, wie es mit _Field Prgrammable Grid Arrays_ (FPGAs) möglich ist

Hier kommt [Argon2 (Wikipedia)](https://de.wikipedia.org/wiki/Argon2) ins Spiel. Der Gewinner der _Password Hashing Competition 2015_ funktioniert grundlegend wie die anderen Algorithmen - mehr Durchläufe führen zu längeren Wartezeiten.

Allerdings hat er noch zwei weitere Tricks auf Lager, die gegen die o.g. Spezialangriffe schützen können.

Einer der Aufrufparameter ist die maximale Anzahl parallel zu verwendender Kerne bei der Ver-/Entschlüsselung, was die Möglichkeit eines schnelleren Angriffs über den Ansatz mit vielen Kernen einer GPU zunichte macht.

Mit einem weiteren Parameter wird festgelegt wieviel RAM der Algorithmus während der Berechnung allokieren soll.
Da FPGAs in der Regel für Embedded- und Hardwarenahe Programmierung eingesetzt werden kommen sie meist mit Speicher im Bereich von 256 oder 512MB. FPGAs mit hohem Speicher (>2 GB) sind selten und/oder sehr teuer.

Wenn der Algorithmus also 4GB verwenden will, diese aber nicht bekommt, funktioniert er nicht. Somit ist auch dieser Angriffsvektorstark erschwert.

## Alles hat seinen Preis
Wie bereits weiter oben erwähnt, verursachen die schwachen Passwörter zusätzlichen Aufwand. Dieser Aufwand schlägt sich in Rechenzeit und damit _Wartezeit_ auf Seiten der Benutzer nieder.
Bei Verwendung der entsprechenden Algorithmen muss also im Projekt ermittelt werden, welche Wartezeit für die Benutzer akzeptabel ist und die Einstellungen daraufhin angepasst werden.

## PBKDF2 und Argon2 in C#
Während PBKDF2 als [System.Security.Cryptography.Rfc2898DeriveBytes (MS Docs)](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rfc2898derivebytes?view=net-5.0) in C# verfügbar ist, muss Argon2 über ein Nuget Paket hinzugefügt werden. Aus den wenigen verfügbaren Implementierung haben wir in der LUSD einen POC mit [Konscious.Security.Cryptography (Github)](https://github.com/kmaragon/Konscious.Security.Cryptography) umgesetzt.

