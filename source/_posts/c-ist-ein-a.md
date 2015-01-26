title: C ist ein A

tags:
  - programming
  - c
  - errors
categories:
  - Programming
date: 2008-02-01 23:02:00
---

Einerseits ist so ein C-Compiler ja immer fuer eine Uberraschung gut. Man bekommt zB staendig neue Meldungen die man vorher noch nie gehoert hat:

`     12 [main] a 2436 _cygtls::handle_exceptions: Error while dumping state (probably corrupted stack)
Segmentation fault (core dumped)`

…prima…

Das Problem ist diese wunderschoene Zeile Code:

``` C
  for (int i=0; i<max+1; sort[*(ptr+i++)]++);
```

bei der sort ein Array darstellt und ptr auf ein Array aus Integern zeigt… gut, die ist n bissl kryptisch, aber an sich doch einfach, oder?

Ach ich hab Kopfschmerzen… fuer heute ists genug glaub ich -.-
