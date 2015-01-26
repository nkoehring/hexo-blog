title: Spaß mit Passwörtern
tags:
  - thoughts
  - passwords
  - security
categories:
  - Thoughts
date: 2011-01-25 23:13:26
---

Wie wir alle benutze auch ich Passwörter oder evtl. besser Mantras zum Schutze meiner Privatsphäre, der Einstellungen diverser (Web-)Dienste und manchmal auch nur aus purer Paranoidität. Und gerade wegen letzterem habe ich mal geschaut, ob es Übersichtstabellen gibt, in denen Passwörter (bzw eben Mantras) nach ihrere Komplexität aufgereiht sind.

Et voilà: [Password Recovery Speeds](http://www.lockdown.co.uk/?pg=combi) – eine Tabelle mit genau den gewünschten Informationen und sogar den approximierten Zeitspannen für sechs verschiedene Leistungsklassen, die Bruteforce-Attacken zum knacken eines entsprechenden Passworts benötigen.

Hauptausschlaggebend für die Schätzung der Passwortkomplexität sind vorallem zwei Parameter: Die Länge des Passwortes, sowie die Anzahl der möglichen Zeichen. Daraus ergibt sich ganz einfach die Anzahl der möglichen Kombinationen. Soweit so gut und vorallem sicher auch bekannt.

Nun habe ich mich aber mal gefragt, was passiert, wenn man ein <span class="caps">UTF</span>-8-Zeichen in seinem Passwort hat. Wie erhöht sich dadurch der Zeichenraum?

Als Beispiel ziehe ich mal die Definition im [Kerberos-Protokoll](http://de.wikipedia.org/wiki/Kerberos_(Informatik)) heran. Da gibt es folgende Gruppen:
* Lowercase letters (26)
* Uppercase letters (26)
* Numbers (10)
* Punctuation (34?)
* Other Characters (??)

Nun sind sicher schnell ein paar Fragezeichen ins Auge gestochen. Die Punktuierung mit 34 Zeichen anzusetzen halte ich für realistisch, man kann ansonsten ja einfach mal durchprobieren. Aber was bedeutet &#8220;Other Characters&#8221;? Alles ungleich dem Rest? Oder alles, was in den erweiterten <span class="caps">ASCII</span>-Zeichensatz passt?

Nehmen wir letzteres an, kann man von bis zu 128 weiteren Zeichen ausgehen. Damit wäre der gesamte Zeichenraum bei 26+26+10+34+128 = 224 Zeichen. Mein bisheriges Passwort deckte diesen Raum ab. Bei angenommenen zehn Zeichen erreicht man damit eine ordentliche Komplexität von 224**10, was etwas mehr als 78 Bit entspricht. Zum Vergleich: Ein gleich langes Passwort bestehend aus Zahlen, Groß- und Kleinbuchstaben liegt irgendwo zwischen 59 und 60 Bit.

Aber um nun endlich mal zurück zu den <span class="caps">UTF</span>-8-Zeichen zu kommen. Denken wir uns eine weitere Kategorie &#8220;Special Characters&#8221; dazu, die alle &#8220;üblicherweise&#8221; Unterstützen <span class="caps">UTF</span>-8-Zeichen enthält. Dies entspricht allen Zeichen, die mit drei der vier Möglichen Bytes abgedeckt werden und somit &#8220;virtuell alle allgemein genutzten Zeichen/Symbole&#8221; enthält. Dieser Bereich erstreckt sich damit auf 2**24 = 16777216 Möglichkeiten. Mit diesem Zeichenraum hat man bei eine minimalen Passwortlänge von zwei Zeichen bereits nahezu 300Mrd. Mögliche Kombinationen, die es zu testen gilt. Bei drei Zeichen sind es bereits 4.7 Trilliarden(!).

Aus purer Geilheit an den riesigen Zahlen hier mal eine Tabelle im Stile von [Password Recovery Speeds](http://www.lockdown.co.uk/?pg=combi), aber mit moderneren und vorallem paranoideren Angriffsszenarien:

Klasse A: <span class="caps">AMD</span> Phenom II X4 ~3GHz – **~16,7 <span class="caps">MPW</span>/s**

(recht erschwinglicher Heim-PC)

Klasse B: Cell Broadband Engine ~3,2GHz – **~24,4 <span class="caps">MPW</span>/s**

(<span class="caps">IBM</span> Server Prozessor, in kleinerer Form in der <span class="caps">PS3</span> verbaut)

Klasse C: <span class="caps">ATI</span> Radeon HD 5970 X2 – **~3,5 <span class="caps">GPW</span>/s**

(Gamer-PC, Highend-Grafikkarte)

Klasse D: <span class="caps">ATI</span> Radeon HD 6970 &#215;4 – **~8 <span class="caps">GPW</span>/s**

(Gamer-PC, 4 Highend-Grafikkarten)

Klasse E: Distributed.net – **~76,1 <span class="caps">GPW</span>/s**

(Bovine <span class="caps">RC5</span>-64 Project, distributed)

Die Werte habe ich aus den Distributed.net-Statistiken.

Und die Abkürzungen, die selbstverständlich jedem Standard trotzen, habe ich mir selbst ersponnen:

* <span class="caps">MPW</span>/s – &#8220;Megapasswords per Second&#8221;, Millionen Passwörter pro Sekunde

* <span class="caps">GPW</span>/s – &#8220;Gigapasswords per Second&#8221;, Milliarden Passwörter pro Sekunde

Und nun endlich das, was alle Zahlenfetischisten so sehnsüchtig erwarten:


### Zahlen (10<sup>n</sup> Kombinationen)

Beispiel      | Klasse A | Klasse B | Klasse C | Klasse D | Klasse E
--------------|----------|----------|----------|----------|---------
74392478      | 6s       | 5s       | <1s      | <1s      | <1s
3627163777    | 10m      | 6m50s    | <3s      | <2s      | <1s
676372777266  | 16h38m   | 11h23m   | 4m46s    | 2m5s     | 14s


### Zahlen und Buchstaben (62<sup>n</sup> Kombinationen)

Beispiel      | Klasse A | Klasse B | Klasse C | Klasse D | Klasse E
--------------|----------|----------|----------|----------|---------
hxHH24dD      | 5 Monat  | 0.25a    | 17h20m   | 7h35m    | 48m
hT2dD63jk7    | 1500a    | 1090a    | >7,5a    | 3,3a     | 130d
jjD3Dd77sdW6  | 6·10⁶a   | 4·10⁶a   | 29210a   | 12780a   | 1343a

### Zahlen, Buchstaben, Punktuierung (96<sup>n</sup> Kombinationen)

Beispiel      | Klasse A | Klasse B | Klasse C | Klasse D | Klasse E
--------------|----------|----------|----------|----------|---------
gf6!j?d7      | 13,6a    | 9,3a     | 23d 20h  | 10d 10h  | 26h 20m
js7?8+23l9    | 126153a  | 86343a   | 600a     | 263a	  | 27,6a
p.k,s-jY3Dn?  | 1,2·10⁹a | 796·10⁶a | 5,5·10⁶a | 2,5·10⁶a | 255137a

### Erweiterter <span class="caps">ASCII</span>-Raum (224<sup>n</sup> Kombinationen)

Beispiel      | Klasse A | Klasse B | Klasse C | Klasse D | Klasse E
--------------|----------|----------|----------|----------|---------
RG!µẼ°3D      | 12028a   | 8232a    | 57.5a    | 25a      | 2.6a
éT7ADä#€k8    | 600·10⁶a | 413·10⁶a | 2,9·10⁶a | 1,3·10⁶a | 132434a
djÿ§§d42sd]6  | 30·10⁶a  | 2·10⁶a   | 145·10⁹a | 23·10⁹a  | 6,6·10⁹a


Und nun für die wirklich hartgesottenen unter euch:
### 3byte-<span class="caps">UTF</span>-8-Raum (16777216<sup>n</sup> Kombinationen)

Beispiel      | Klasse A | Klasse B | Klasse C | Klasse D | Klasse E
--------------|----------|----------|----------|----------|---------
øגק	      | 9·10⁶a   | 6·10⁶a   | 42755a   | 18705a   | 1966a
føøβαr        | 410<sup>29</sup>a   | 310<sup>29</sup>a   | 310<sup>27</sup>a | 910<sup>26</sup>a   | 910<sup>25</sup>a
føøβαr23      |  10<sup>44</sup>a   | 810<sup>43</sup>a   | 510<sup>41</sup>a   | 210<sup>41</sup>a   | 310<sup>40</sup>a
wøø┼ a p4β    | 310<sup>58</sup>a   | 210<sup>58</sup>a   |  10<sup>56</sup>a   | 710<sup>55</sup>a   | 710<sup>54</sup>a
…m³gâþ4βwørd  | 910<sup>72</sup>a   | 610<sup>72</sup>a   | 410<sup>70</sup>a   | 210<sup>70</sup>a   | 210<sup>69</sup>a


Fazit? Sonderzeichen machen Passwörter sicherer, <span class="caps">UTF</span>-8-Symbole außerhalb jedes <span class="caps">ASCII</span>-Charsets unknackbar!

Unter unixoiden System hat man out-of-the-box schon einige solcher Zeichen direkt verfügbar. Einfach mal ausprobieren mit AltGR+Zeichen und AltGR+Shift+Zeichen. Außerdem lässt sich die Zeichentabelle komplett anpassen <sup class="footnote" id="fnr1">[1](#fn1)</sup>, oder per Modifier auf andere Zeichensätze (japanisch zB) umschalten.<sup class="footnote" id="fnr2">[2](#fn2)</sup>

Windows-User haben es da schwerer. Dort kann man virtuell zwar jedes <span class="caps">UTF</span>-8-Zeichen tippen, muss das aber mit einer Kombination aus Alt-Taste und einem Code auf dem Nummernblock machen.<sup class="footnote" id="fnr3">[3](#fn3)</sup> Auf einem Laptop zB wird das mindestens unhandlich, wenn es denn überhaupt geht.

Und jetzt fange ich endlich an, mit den wichtigeren Dingen des Lebens. Der Arbeit.

Zur Ergänzung:

Der [@techpriester](http://twitter.com/techpriester) meinte:

> ein schönes Repertoire an Sonderzeichen hat man auch auf dem Mac, mit [option]+[rest der tastatur]

[<sup id="fn1">1</sup>](#fnr1) [<span class="caps">XKB</span>-Config](http://www.xfree86.org/current/XKB-Config.html)

[<sup id="fn2">2</sup>](#fnr2) [<span class="caps">XKB</span> layout switching](http://www.linuxquestions.org/questions/linux-software-2/current-xkbmap-x-org-kb-layout-switching-470134/)

[<sup id="fn3">3</sup>](#fnr3) [<span class="caps">UTF</span>-8 unter Windows](http://www.5goldig.de/Text_Uebersetzer/Zeichen-Buchstaben.html)
