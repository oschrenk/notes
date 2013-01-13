# Klassen von Algorithmen #

## Was ist ein algorithmisches Problem? ##

*   **Endlichkeitsbedingung** Jede einzelne erlaubte Eingabe und jede
    einzelne erwartete Ausgabe muss (durch) eine _endliche_
    Zeichenkette (darstellbar) sein
*   **Unendlichkeitsbdingung** Es müssen (abzählbar) _unendlich_
    viele verschiedene Eingaben erlaubt sein
*   **Lösbarkeitsbedingung**

Die Endlichkeitsbedingung, schließt folgendes Beispiel aus:

> Gesucht wird ein Algorithmus ohne Eingabe, der die Zahl Pi in
> Dezimalbruchdarstellung ausgibt.

Da Pi aber eine irrationale Zahl ist, und demzufolge unendlich viele
Ziffern (und nicht periodisch ist), ist dieses Problem nicht
algorithmischer Natur.

Mit der Unendlichkeitsbedingung wollen wir Probleme ausschließen, die
sich durch einen _Trick_ effizient berechnen lassen. Bei endlich
vielen Eingaben kann man auf eine Lösungstabelle zurückgreifen. Da
wir aber Algorithmen bezüglich ihrer Schwierigkeit klassifizieren
wollen, müssen wir solche Probleme ausschließen, da dort die
Schwierigkeit in Richtung des Entwicklungsprozesses verschoben werden
kann. Als sehr einfaches Beispiel kann man einen "Algorithmus""
entwickeln, der uns die 12. Stelle von Pi zurückgibt, indem man
einfach eine Methode schreibt die direkt `8` zurückgibt. Die
&#8220;Berechnung&#8221; ist sicher schnell, liefert aber keine
zuverlässige Aussage über die Komplexität des Algorithmus, da die
eigentliche Schwierigkeit in der Entwicklung lag.

Bedingung 3 schließt folgendes Beispiel aus:

> Gesucht wird ein Algorithmus, mit einer ganzen Zahl `z` als Eingabe,
> und als Ausgabe eine ganze Zahl `w` für die gilt `w * w = z`

Da viele ganze Zahlen keine ganzzahlige Wurzel haben (z.B. 2, 3, 5,
alle negativen Zahlen) ist dieses Problem kein _algorithmisches
Problem_.

## Theoretische Lösbarkeit von Problemen ##

Die Klasse aller algorithmischen Probleme kann man in zwei Klassen
einteilen:

*   algorithmische Probleme, die aus theoretischen Gründen nicht
    lösbar sind
*   algorithmische Probleme, die (zumindest theoretisch) lösbar sind.

Theoretisch lösbar zu sein heißt, das ein Lösungsalgorithmus
existiert (ob er praktisch ausführbar ist, ist dann ein anderes
Problem)
Um zu zeigen, dass ein Problem theoretisch unlösbar ist, muss man
einen Beweis führen. Nur weil kein Algorithmus bekannt ist, heißt
nicht das das Problem nicht lösbar ist.

Gegeben sei folgende Behauptung:

> Es gibt keine _ganze_ Zahl, deren Quadrat gleicht 5 ist.

Dies kann man beweisen in dem man sich bewusst macht, dass `4 < 5 < 9`
und dass `2^2 < n^2 < 3^2`. Da aber keine ganze Zahl zwischen 2 und 3
liegt, kann keine Lösung existieren.

Berühmte Beispiele für unlösbare Probleme sind unter anderem das
Kachelproblem, das Paligrammproblem und das Halteproblem.

### Das Halteproblem ###

Gesucht wird ein Algorithmus der als Eingabe ein Tupel bestehend aus
Programm P und einer Eingabe E (für das Programm P) bekommt und
`true` ausgibt, falls P(E) hält und sonst `false`.

Das Problem ist ein Entscheidungsproblem und erfüllt alle drei
Kriterien als algorithmisches Problem.

Dieses Problem ist nicht lösbar.

## Praktische Lösbarkeit ##

Quelle:Grundkurs Algorithmen und Datenstrukturen in JAVA
Andreas Solymosi und Ulrich Grude
Vieweg+Teubner Verlag
ISBN 978-3-8348-0350-4
