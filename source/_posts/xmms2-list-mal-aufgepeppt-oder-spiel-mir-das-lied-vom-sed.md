title: "xmms2 list mal aufgepeppt oder: Spiel mir das Lied vom Sed"
tags:
  - programming
  - shell
  - sed
  - xmms2
  - tools
  - music
  - linux
categories:
  - Tools
date: 2010-11-26 16:33:42
---

Nach langem mal wieder ein Blogartikel von mir. Warum? Mir ist langweilig, ganz klar. Außerdem hat die wundervolle @wunder_lich (ja, ich benutze hier mal rigoros Twitter-Pseudonyme) jetzt auch einen Blog. Da muss ich doch mithalten!

Und weil die liebe Claudia und ich herrlich verschieden sind, wird mein Artikel hier auch ueberhaupt nichts mit ihr oder ihren Themen zu tun haben.

[![](http://oldblog.nkoehring.de/images/20101126/thumb2_playlist.png)](http://blog.nkoehring.de/images/20101126/playlist.png)

Ich habe heute naemlich mal aus lange Weile die Ausgabe von `xmms2 list` aufgepeppt und mir eine Playlist-Anzeige gefrickelt. Benoetigte Werkzeuge: Ein vernuenftiges Betriebssystem (vermutlich klappts aber auch unter Cygwin) und dessen Standard-Shellwerkzeuge watch und sed.

Dabei herausgekommen ist eine huebsch bunte Playlist, die auf die Sekunde aktuell ist. Mit ein bisschen Bastelei waere das ganze auch schnell zu einem kleinen, voll konfigurierbaren Script aufgepeppt, was ich aber gern der Allgemeinheit ueberlasse. Hier also der Einzeiler, auf den es ankommt:

``` sh
watch --color 'tput setaf 1; xmms2 list |
               sed "s/^--&gt;\(.*\)$/ $(tput bold; tput setaf 4) \1 $(tput sgr0; tput setaf 2)/";
               tput sgr0'
```

Die Aufrufe von tput koennte man sicher durch direkte Farbcode-Ausgaben ersetzen und ueberhaupt geht das sicher alles schoener, aber zur Veranschaulichung reicht es.

Ach ja: watch unterstuetzt das Argument &#8212;color erst mit Version 0.3.0\. Auf meinem Laptop mit Arch Linux funktioniert das zB noch nicht.

Viel Spaß mit der bunten Playlist. Ich trink jetzt Kaffee.
