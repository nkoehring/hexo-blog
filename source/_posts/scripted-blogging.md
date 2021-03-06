title: scripted blogging
tags:
  - programming
  - usability
  - weblog
categories:
  - Weblog
date: 2012-08-19 22:13:28
---

Gefühlt blogge ich etwa so oft, wie die <span class="caps">NASA</span> Rover auf den Mars schickt. Ich sollte das wirklich ändern. Aber es hilft ja alles nix. Mein Elan ist einfach zu selten vorhanden und diesen webLog ließt ja eh <span title="&quot;Falls du das hier ließt, freue ich mich über eine Nachricht. Ich benutze keinerlei Webanalyse-Kram a.k.a. Google-Analytics etc.&quot;">niemand</span>.

Ich weiß auch niemanden, der meine <span title="*hust*">großartige</span> Blogging-Engine verwendet, die ich für diesen webLog verwende, aber Weiterentwicklung betreibe ich trotzdem. So gibt es nun zwei Ruby-Progrämmchen, die das Blogger-Leben ein wenig erleichtern.

Zum einen einen Feed-Generator, der einen <span class="caps">RSS2</span>.0-Feed mit allen Artikeln ausspuckt, zum anderen ein Script, dass das Erstellen eines Artikels ein wenig automatisiert. Beide sind super einfach zu verwenden und benötigen keine externen Ruby-Bibliotheken, wenn man Ruby19 verwendet. Bei Ruby18 muss man <span class="caps">JSON</span> nachrüsten.

Es gibt auch noch eine Änderung: Ich habe seit einiger Zeit zwei virtuelle Server parallel, lasse den ersten, bei Server4You gehosteten aber auslaufen.

Auf ihm laufen derzeit noch alle Dienste, die irgendwie mit nkoehring.de verknüpft sind (E-Mail, Jabber, Blog, Homepage). Seit einigen Wochenenden ziehe ich all das nach und nach auf meinen zweiten Server um. Mein webBlog ist schon umgezogen und ist deshalb für einige Zeit nur auf der Adresse [https://log.koehr.in](https://log.koehr.in) erreichbar. Das <span class="caps">HTTPS</span> in in der Adresse ist nun übrigens verpflichtend. Alle <span class="caps">HTTP</span>-Anfragen auf diesen Webserver enden auf ein und derselben Landing-Page.

Ein kleiner Aufruf noch an alle, die es bis hierher geschafft haben: Wenn wirklich jemand meine webLog-Engine verwendet oder auch nur damit herumgespielt hat, so melde er sich ruhig bei mir.
