title: foobar2k masstagging
tags:
  - foobar2k
  - masstagging
  - tutorials
categories:
  - Tutorials
date: 2008-03-17 23:57:24
---

Hallo...

mit diesem Text leite ich meine neue Tutorial-Kategorie ein! Immer dann wenn ich etwas fuer interessant befinde und zu all dem Spaß nicht nur Zeit, sondern auch noch Lust habe, werde ich hier ein kleines Tutorial veroeffentlichen - in der Hoffnung, dass es jemanden interessiert und nuetzt ;)

**Fangen wir an...**

Wer fuer Windows einen reinen Audioplayer sucht, der schlank, effizient und nuetzlich ist, sollte sich unbedingt Foobar2000 anschauen! Zu finden und downloaden auf [http://www.foobar2000.org](http://www.foobar2000.org "FooBar2k") ist dieser Player meiner Meinung nach der mit Abstand beste **ueberhaupt**!

Dieser Player kann mit diversen Plugins um nuetzliche Funktionalitaeten erweitert werden. Das macht ihn schlank und funktional zugleich. Um nun zum eigentlichen Thema zu kommen:

Foo_masstag ist ein Plugin, mit dem Scriptartige Tagging-Funktionen erstellt, gespeichert und angewendet werden koennen. So ist es zB sehr einfach damit moeglich die Tags aus den Dateinamen zu erzeugen, solange diese nach einem Muster benannt sind.

Zum Beispiel habe ich mir von YouTube ein paar (70) Musikvideos mit dem YoutubeVideoDownloader ins MP3-Format gebracht. Nun geht es ans Tagging.

70 Stuecke... einzeln taggen? Da gibts doch auch was von Ratiofa... ich mein, dafuer gibts ja FreeDB! Nein... leider nicht. die FreeDB erkennt keinerlei Muster in den erzeugten MP3s. Die Mp3s sind allerdings schon nach den Original-Videodateien benannt. Da hilft der Masstagger.

Man nehme: Eine Playlist mit den vermeintlichen Videos... dann markiere man alle und mache den guten alten Rechtsklick! Ist das Masstagging-Plugin installiert findet sich nun im Abschnitt "tagging" der Punkt "manage scripts...". Dort klicken wir fleißig drauf und schauen was da kommt.

Heureka, ein Fenster!

Rechts ist eine Liste mit unseren markierten Liedern zu sehen. Links eine Liste mit den Buttons "Add", "Remove", "Clear", "Up" und "Down" darunter. Mit einem Klick auf "Add" werden wir aufgefordert den "action type" auszuwaehlen. Die Auswahlmoeglichkeiten reichen vom hinzufuegen/aendern/loeschen von Elementen, ueber das Eingeben/Kopieren von Werten bis hin zum Schaetzen von Werten.

Schaetzen? Sagte ich Schaetzen? Genau: "Guess value from..." - so beginnt es.

Und mit "Guess value from filename..." ist es moeglich, den Dateinamen als Vorlage fuer die Musiktags zu verwenden. Nach der Auswahl dieses Elementes sollen wir nun das"Scheme", also Namensschema der Datei angeben. Die Videodateien von Youtube sind alle in etwa so benannt:

**Interpret - Titel(YoutubeVideoID)**

Einfach uebersetzt also "artist", "title" und eben jene ID... Variablen werden in %-Zeichen gefasst.Bei Foobar2000 sind solche Variablen meist sehr intuitiv vergeben. Auf Gut Glueck versuchte ich also:

**%artist% - %title%**

...halt. Bleibt noch diese VideoID... das kennt Foobar2k natuerlich nicht. %trash% wird es sicherlich auch nicht geben... wir brauchen also ein existierendes Feld, welches moeglichst wenig Einfluss hat. Meine Wahl:

**%artist% - %title%(%comment%)**

Und es klappte auf Anhieb! Tolles Ding :-P

Nun kann man solche Scripte benennen, speichern, als Datei speichern und auch direkt unter dem Tagging-Menue anwaehlen.

Ich hoffe das hilft bei den zukuenftigen Massentagging-Versuchen! Bevor ich es vergesse, hier noch ein Link:

[http://www.foobar2000.org/components/foo_masstag.zip](http://www.foobar2000.org/components/foo_masstag.zip "FooBar2k Masstag")
