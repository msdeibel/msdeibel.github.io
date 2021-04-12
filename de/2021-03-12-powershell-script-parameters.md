#---
#layout: post
#title:  "Pflichtparameter mit Standardwerten in Powershell-Skripten"
#date:   2021-03-12 07:40:42 +0100
#categories: powershell coding de
#---

## Komfort und Funktionssicherung
Diese Woche hatte ich die Anforderung ein Powershell-Skript zu erweitern. Dabei ging es um einen Bugfix, aber auch darum, den Aufruf anwenderfreundlicher zu gestalten.

Vorher hat das Skript eine einzelne Datei bearbeitet, deren kompletter Pfad vom Benutzer abgefragt wurde. Allerdings war in der Originalanforderung nicht festgehalten, dass der Benutzer das Skript fuer knappe 200 Dateien aufrufen muss. Und wenn ich von "Benutzer" rede, denkt man direkt an jemanden, der nicht unbedingt weiss, dass er/sie sich relativ leicht eine Schleife in Powershell bauen kann, die das automatisiert.

Daher hiess es, den Aufruf zu verbessern.

## Parameterabsicherung bei Powershell
Nachdem die inhaltlichen Fehler relativ schnell abgearbeitet waren, ging es an die Komfortfunktionen.

Allerdings darf mit dem Komfort natuerlich die Absicherung der Funktionalitaet nicht leiden. In diesem Fall hiess das, dass das Skript auf jeden Fall ohne Fehler starten muss - auch in einer automatisierten Umgebung. Daraus ergaben sich folgende Eckpunkte
- Uebergabe des Verzeichnisses mit den Dateien, statt eine einzelne Datei als Kommandozeilenparameter
- Benutzerabfrage, falls kein Kommandozeilenparameter vorliegt
- Start mit leerer Benutzereingabe erlauben
- Erlauben von Pfaden mit Leerzeichen

Parameter lassen sich bereits mit Bordmitteln der Powershell in gewissem Masse absichern. Dazu gehoeren z.B. Typ, Null-Sicherung und Pflichtparameter. Details dazu finden sich in der Microsoft-eigenen Doku auf [about_parameters](https://docs.microsoft.com/de-de/powershell/module/microsoft.powershell.core/about/about_parameters).

# Die Basis ist unflexibel
Leider sind die Absicherungsmoeglichkeiten etwas unflexibel und passen nicht zum o.g. Szenario.

Ein Parameter kann eine Pflichtangabe sein
```powershell
param ([Parameter(Mandatory)]$servername)
```
aber in diesem Fall kann man keinen Standardwert mehr zuweisen.
D.h. man kann bei fehlendem Kommandozeilenparameter nicht einfach einen Standardwert setzen wie es mit
```powershell
param ($servername = 'server01')
```
moeglich ist.

Auch eine laengere Suche im Netz liess mich immer wieder zum selben Ergebnis kommen: Selber programmieren ist angesagt.

## Komfortabel und abgesichert
Der Code bei dem ich am Ende gelandet bin, umfasst nur einige Zeilen, die das Ziel erreichen.

```powershell
Param ([string]$parameterKommandoZeile)

[string]$parameter = $parameterKommandoZeile

# Bei fehlendem Parameter, Benutzereingabe anfordern
if($parameter -eq "") {
  $parameter = (Read-Host -prompt "Parameter eingeben (leer - aktuelles Verzeichnis")
}

# Falls die Benutzereingabe auch leer ist, aktuelles Verzeichnis verwenden
if($parameter -eq "") {
  $parameter = '.\'
}

# Leerzeichen escapen
$parameter -replace ' ', '` '

# Sicherstellen, dass der Pfad mit \ endet
if(!$paramert.EndsWith('\')) {
  $parameter = $parameter + '\'
}
```

Mit wenigen Handgriffen, haben wir damit ein flexibles Szenario, was die Parameterauswertung angeht.