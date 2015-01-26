title: so richtig sichere Passworthashes für Linux
tags:
  - linux
  - security
  - tutorials
categories:
  - Tutorials
date: 2013-03-08 09:21:01
---

In /etc/pam.d/passwd (Arch Linux):

`password  required  pam_unix.so sha512 shadow nullok rounds=125000`

Wichtig hier sind

*   sha512: Ordentliche Hashes
*   rounds: Die Anzahl an Hash-Runden beträgt standardmäßig 5000.

Ich habe den Roundswert soweit erhöht, dass es eine sehr kurze, aber spürbare Verzögerung gibt. Je nach Power deines Prozessors, solltest du den Wert evtl verringern oder erhöhen.

Es gibt als Algorithmus auch **bigcrypt**. Ich konnte aber nicht herausfinden, wie man den konfiguriert (die Rundenanzahl anpasst, etc).
