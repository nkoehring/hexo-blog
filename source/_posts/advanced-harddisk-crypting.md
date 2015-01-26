title: Advanced Harddisk Crypting
tags:
  - tutorials
  - linux
  - security
categories:
  - Tutorials
date: 2009-05-06 00:34:40
tags:
---

In einer meiner üblichen Kurzschlussreaktionen beschloss ich vor ein paar Tagen, meine Laptop-Festplatte zu verschlüsseln. Alles kein Problem, laut der Stimme in meinem Kopf. Einfach Daten sichern, Platte neu partitionieren, System installieren, Daten zurueck holen, fertig.

Aber darin enthalten waren zwei Punkte, die mich gestoert haben:

1.  System neu installieren - Nein! Das wollte ich dann doch vermeiden.
2.  Platte partitionieren und dann verschluesseln - Ich will das sicherer haben... Was? Paranoia? Ja hey, natürlich ist das paranoid. Na und?
Also, was wollen wir nun genau erreichen?

1.  Das bestehende System sollte moeglichst unveraendert bleiben.
2.  Die bestehende Partitionierung wird von Grund auf modernisiert.
3.  Bootloader, Kernel, InitramFS, etc werden ausgelagert.
4.  Die eigentliche Festplatte sollte von außen aussehen, als bestuende sie nur aus Datenmuell. Weder Partitionierungsinformationen, noch MBR, noch irgendwas sollten darauf erkennbar sein.
5.  Das ganze sollte den Bootvorgang nicht sonderlich durch Passwortabfragen stoeren.Wie wird das umgesetzt?

1.  Erstmal muss das bestehende System gesichert werden.
2.  Die bestehende Partitionierung wird entfernt.
3.  Die komplette Platte wird verschluesselt. Der Schluessel ist in einem KeyFile.

4.  Die neue logische Partitionierung (LVM) kommt auf die verschluesselte Platte.
5.  MBR sowie Bootpartition mit Kernel und InitramFS werden auf einen USB-Stick ausgelagert.Im Detail dauerte das alles ca anderthalb Tage. Rechne ich aber die Backup- und Wiederherstellungszeiten sowie die unnoetigen Probleme (die ich hatte) weg, sind es nur noch wenige Stunden handarbeit.

* * *

Wenn dich der Vorgang im Detail interessiert... bist du nun endlich an der entsprechenden Stelle angekommen:

<big><u>**Beginnen wir mit dem Sichern:**</u></big>
Je nach Möglichkeiten ist die Sicherung mehr oder weniger trivial. Der einfachste und wohl auch schnellste Weg ist eine zweite Festplatte dafuer in den Rechner zu schieben. Leuten mit Laptops oder einer schnellen NAS-Loesung empfehle ich eine Mischung aus TAR und NetCat (war auch meine Variante). Dabei ist aber unbedingt darauf zu achten, dass die Dateirechte beibehalten werden. Dazu hat _tar_ den Parameter _-p_, welcher fuer "preserve permissions" steht. Bei mir hat dieser Teil leider nicht geklappt, was einige Stunden zusaetzlicher Arbeit bedeutete.

Meine netcat&amp;tar-Lösung bestand nur aus zwei Arten von Kommandos:
> nc -l -p &lt;port&gt; &gt; backup.tar<div align="right">zum "lauschen" auf der Serverseite.
&nbsp;
</div>
> tar pc /files/for/backup | pv -trb | nc &lt;target-ip&gt; &lt;target-port&gt;<div align="right">zum versenden der Daten.
&nbsp;
</div>

_pv (PipeView) _ist uebrigens ein nuetzliches kleines Tool zum beobachten der Datenuebertragung. So kann man zB sehen, wie schnell die Daten uebertragen werden, wie lang die Uebertragung schon laeuft, wie viel schon Uebertragen wurde, oder wenn man die Groesse der zu uebertragenen Daten kennt, das ganze auch als Progressbar und/oder ETA. Zusaetzlich koennte das Programm auch die maximale Uebertragungsgeschwindigkeit bestimmen, was fuer ein Backup aber eher unnuetz waere.

**Noch ein Tipp:** Komprimierung bringt keinen Geschwindigkeitsvorteil! Die Dateiuebertragung durch pures tar gepiped lief bei mir mit ~11,2MB/s (100Mbit eben). GZip bremste das ganze schon auf unter 3MB/s. BZip2 schickte es in die Knie, naemlich auf ~250kb/s.

<u><big>**Die Verschluesselung**</big></u>
Jetzt, wo alle Daten gesichert sind, schmeißen wir erstmal alles restliche von der Platte. Das heißt, nicht nur die Daten, sondern auch die komplette Partitionierung. Das ist also der Punkt, an dem man spaetestens eine LiveCD oder aehnliches in petto haben sollte. Am besten eine, die nuetzliche Dinge wie Cryptsetup und LVM schon mitbringt. Ich als Arch-Linux-User bin da mit einer normalen Arch-LiveCD gut bedient.

Von der LiveCD aus loeschen wir nun also die komplette Festplattenstruktur. Am besten macht man das mit _dd_. So kann man gleich die komplette Festplatte mit Zufallsdaten ueberschreiben, was die Sicherheit zusaetzlich erhoeht.

> dd if=/dev/urandom of=/dev/&lt;harddisk&gt;
<div align="right">...dauert lange. Zeit einen Kaffee zu trinken und sich allgemein ueber Festplattungverschluesselung zu informieren: [http://en.wikipedia.org/wiki/Disk_encryption_theory](http://en.wikipedia.org/wiki/Disk_encryption_theory)
&nbsp;
</div>

...*schluerf* Wow, interessant. *les...les...les...* Man, wer haette das gedacht... *schluerf*

Nachdem die Festplatte vollstaendig mit Zufallsbrei ueberschuettet ist, koennen wir nun die Verschluesselung ansetzen. Ich verwendete dazu _cryptsetup_. In der Kaffeepause habe ich mir dann Gedanken gemacht, welchen Algorithmus ich zur Verschluesselung verwende... okay, ich gebs zu. In Wahrheit habe ich ne Runde Kicker gespielt und Equi gefragt. Er hat gemeint, ich soll AES-XTS-PLAIN benutzen. Aber ich habe mich zumindest nachtraeglich mal informiert, was daran so toll ist, ehrlich!

Vorher erstellte ich noch ein Keyfile. Der echte Nerd will auch echte Sicherheit. Also erstellt er ein 512 Bit (bzw 64byte) langes Passwort, bestehend aus (pseudo-)Zufall. Warum diese Groesse? Der Wikipedia-Artikel, den du wohl nicht gelesen hast, wird dir helfen. Soviel sei verraten: Es muss sich um eine durch acht teilbare Groesse handeln.

> dd if=/dev/urandom of=crypto.key bs=1 count=64

Die Datei _crypto.key_ enthaelt nun den zukuenftigen Schluessel zur Festplatte und allen ihren Daten. Ein Verlust dieser Datei ist mit dem Verlust eines Autoschluessels gleichzusetzen. Nur mit einem Unterschied: Das Auto kann man mit ein wenig Geduld knacken, fuer die Festplattenverschluesselung reicht keine Geduld der Welt.

Nun kommen wir zur eigentlichen Verschluesselung der Festplatte:

> cryptsetup -c &lt;cipher&gt; -d &lt;keyfile&gt; create &lt;name&gt; /dev/&lt;device&gt;

Nach gewissenhaftem Lesen des von mir Vorgeschlagenen Wikipedia-Artikels und zusaetzlicher ausgiebiger Recherche, solltest du den fuer dich am besten geeigneten Cipher bereits gefunden haben. Falls nicht, nimm AES-XTS-PLAIN.

Das Geraet sollte nun unter /dev/mapper/&lt;name&gt; zur Verfuegung stehen.

<u><big>**Nun zur Partitionierung**</big></u>
Dank dem guten Equi habe ich mich fuer eine moderne Variante der Partitionierung entschieden. LVM oder auch LogicalVolumeManager ist das Zauberwort. Die Vorteile von LVM im Gegensatz zu statischer Partitionierung sind im Detail beim entsprechenden [Wikipedia-Artikel](http://de.wikipedia.org/wiki/Logical_Volume_Manager) beschrieben.

Ueber die Aufteilung der Partitionierung sollte man sich immer selbst Gedanken gemacht haben. Meine 200GB-Laptopfestplatte habe ich zB so aufgeteilt:

> &nbsp; 1 '/dev/platte/root'&nbsp;&nbsp;&nbsp; [ 20,00 GB]> 
> &nbsp; 2 '/dev/platte/var'&nbsp;&nbsp;&nbsp;&nbsp; [&nbsp; 1,00 GB]> 
> &nbsp; 3 '/dev/platte/swap'&nbsp;&nbsp;&nbsp; [&nbsp; 2,00 GB]> 
> &nbsp; 4 '/dev/platte/home'&nbsp;&nbsp;&nbsp; [160,00 GB]> 
> &nbsp; 5 '/dev/platte/private' [&nbsp; 3,31 GB]
Wie das alles nun genau geht, werde ich hier lieber auslassen. Ich will schließlich nicht die Schuld am Scheitern des ganzen bekommen. Gute Ansaetze zur Verwendung von LVM findet man aber zB bei [SelfLinux](http://www.selflinux.org/selflinux/html/lvm01.html). Dabei ist zu beachten, dass meist davon gesprochen wird, erst eine Partition zu erstellen, die dann mit dem Typ-Code 8e gekennzeichnet wird. Das habe ich *nicht* gemacht, sondern LVM direkt auf das Geraet schreiben lassen. Da ich diesen Blog-Artikel hier auf dem Laptop verfasse, kann ich mit großer Sicherheit sagen, dass es zumindest bei mir keine Probleme gab. Ich verwendete dafuer das auf der LVM2 auf der Archlinux-LiveCD 2008.4.

<big><span style="font-weight:bold;text-decoration:underline;">BootStick und InitRAMFS</span></big>
Um das bootpartitionslose System hochfahren zu koennen, lagerte ich alles dafuer benoetigte auf einen USB-Stick aus. Man sollte sich selbstverstaendlich sicher sein, dass das BIOS des Systems keinerlei Probleme damit hat, von einem USB-Stick zu booten. Falls jemand jetzt alles bisher beschriebene bereits durchexzerzierte um nun festzustellen, dass das System garnicht von einem Stick booten kann, hat noch ein paar Moeglichkeiten:

*   CD - also anstelle des BootSticks ein CD-Image erstellen und brennen.*   Bootpartition - nicht ganz so schoen wie kompletter Datenbrei auf der Platte ist eine unverschluesselte Boot-Partition mit zweiter Partition fuer das verschluesselte LVM.
Fuer alle, deren System von einem USB-Stick bootbar ist, erklaere ich nun grob, was zu tun ist um das ganze auch funktionstuechtig zu gestalten.

Da ArchLinux LVM und Cryptsetup out-of-the-Box unterstuetzt, brauchte ich keinerlei Kernel-Tweaks oder Patches. Das kann bei anderen Systemen aber der Fall sein. Dazu bitte die entsprechenden Foren, Wikis, How-Tos und/oder Manuals konsultieren.

Ich partitionierte meinen USB-Stick so, dass eine kleine (128MB) Ext2-Partition fuer Kernel und InitRAMFS zur Verfuegung steht. Den Inhalt der Bootpartition kopierte ich einfach in die neue Partition und fuehrte danach <span style="font-style:italic;">grub-setup</span> dafuer aus. Sobald der Stick bootfaehig und mit Grub bestueckt war, kam ich zum eigentlichen Teil: InitRAMFS.

Diese - bei Arch-Linux kernel26.img - genannte Datei enthaelt ein kleines Dateisystem, welches direkt in den Speicher geladen wird und wichtige Programme enthaelt, die die Entschluesselung der Festplatte und aktivierung der logischen Partitionen ermoeglichen. ArchLinux bringt fuer die Erstellung ein Hilfsprogramm namens <span style="font-style:italic;">mkinitcpio</span> mit. Dieses verwendet sogenannte <span style="font-style:italic;">Hooks</span> - kleine Scripte die bestimmte Aufgaben beim hochfahren des Systems erledigen. Einen guten Artikel zu diesem tollen Werkzeug findet man im [ArchWiki](http://wiki.archlinux.de/title/Mkinitcpio).

Da ich keine Lust hatte, herauszufinden, wie dem bestehenden Hook "encrypt" gesagt wird, was er zu tun hat, schrieb ich ihn einfach entsprechend um, so das er in einem Kommando genau das macht, was er soll: Die Festplatte entschluesseln und dazu das angegebene Keyfile benutzen.

Das Keyfile habe ich dem InitRAMFS mitgegeben. Das klingt vielleicht unsicher, ist es aber nur bedingt. Ein USB-Stick ist in diesem Moment so sicher wie ein Schluessel zum Tresor. Solang ihn niemand in die Haende bekommt, ist alles gut. Zusaetzlich kann man natuerlich den Schluessel auch gern mit einem Passwort sichern. Dazu wird eben zusaetzliche Software zur Entschluesselung im InitRAMFS benoetigt. Damit naehert man sich dann schon dem Konzept von LUKS, welches es ermoeglicht, den Schluessel zu einer Festplatte zu veraendern, indem es die eigentlichen Schluessel generiert und mit dem angegebenen Passwort mit großen Bitlaengen verschluesselt. Will man das Passwort aendern, brauchen nur die Schluessel neu verschluesselt zu werden und nicht die gesamte Festplatte.

Nach der Erstellung des InitRAMFS machte ich mich noch ueber die noetigen Kernel-Parameter schlau. Die sehen nun so aus (GRUB):

> title Arch Linux> 
> root (hd0, 0)> 
> kernel /vmlinuz26 root=/dev/platte/root cryptdevice=/dev/sda:platte ro> 
> initrd /kernel26.imgMeiner Vermutung nach ist der cryptdevice-Parameter in meinem Fall ueberfluessig, da ich genau diesen Part fest geschrieben habe. Ich habe aber nie damit rumgespielt um das herauszufinden.

<big><span style="font-weight:bold;text-decoration:underline;">System zurueck holen</span></big>
Jetzt wo die Festplatte komplett verschluesselt, die Partitionierung von Grund auf modernisiert und die Boot-Partition durch einen USB-Stick ersetzt wurde, wird es Zeit, die Systemsicherung zurueck zu spielen. Dazu dreht man den Backup-Prozess einfach um:
> nc -l -p &lt;port&gt; &lt; backup.tar<div align="right">auf der Serverseite.
&nbsp;
</div>
> nc &lt;target-ip&gt; &lt;target-port&gt; | pv -trb | tar px /file/location<div style="text-align:right;">zum zurueckspielen der Daten.
&nbsp;
</div>
Der Parameter <span style="font-style:italic;">p</span> ist in diesem Falle wie anfangs erwaehnt aeusserst wichtig! Es macht wirklich keinen Spaß die ganzen Dateirechte von Hand wiederherzustellen. Ich spreche da leider aus Erfahrung.
