# Systempflege für Normalnutzer – Vorschläge aus dem Forum

Zusammenfassung der Diskussion im ioBroker-Forum: [Updates & Systempflege – geht das für Normalnutzer einfacher?](https://forum.iobroker.net/topic/85889/updates-systempflege-geht-das-für-normalnutzer-einfacher)

> **Stand:** 23. November 2026 – die Diskussion läuft noch. Privat zusammengestellt, nichts Offizielles von ioBroker. Gedacht als Grundlage, falls die Punkte z. B. im Dev-Meeting besprochen werden.

## Worum es geht

Viele Nutzer wollen ihr Haus automatisieren, müssen sich aber nebenbei um Betriebssystem, Node.js, js-controller und Adapter kümmern. Im Thread hat sich gezeigt: ioBroker bringt dafür schon einiges mit (Updates und Controller-Upgrade aus dem Admin, automatische Adapter-Updates, Hinweise auf OS-Updates und Neustart in der Glocke, eine empfohlene Node-Version im Repository). Vieles ist aber verstreut oder nur im Terminal sichtbar, z. B. über `iob diag`.

Ein kuratiertes Gesamt-Release wie bei Debian oder Home Assistant passt nicht zu ioBroker, weil die Adapter bewusst unabhängig sind. Die Vorschläge unten bauen deshalb auf vorhandenen Teilen auf.

## Vorschläge

### 1. Ergebnis von `iob diag` im Admin anzeigen

**Heute:** `iob diag` prüft schon vieles und markiert es farbig: Ausführung als root, 32-Bit-System, Abweichung von der empfohlenen Node-Hauptversion, nicht mehr unterstütztes OS (über `distro-info`), Vorabversionen von Node oder js-controller. In der nächsten Version kommen weitere Bereiche dazu, z. B. ein leeres Backup-Verzeichnis. Sichtbar ist das aber nur, wenn man `iob diag` im Terminal aufruft.

**Vorschlag:** Diese Ampel im Admin anzeigen, prominent, aber ohne dass man sie ständig wegklicken muss. Die Regeln müssen nicht neu definiert werden, sie werden in `iob diag` schon gepflegt.

**Aus dem Thread:** Thomas Braun ([#52](https://forum.iobroker.net/post/1364694), [#30](https://forum.iobroker.net/post/1364633)), mcm1957: umsetzbar, wenn die Regeln definierbar sind ([#46](https://forum.iobroker.net/post/1364687)), OliverIO: prominent, ohne wiederholtes Wegklicken ([#44](https://forum.iobroker.net/post/1364684))

**Status:** offen

Zwei Teile davon sind schon als Issue eingetragen:

- **Node-Version nicht mehr empfohlen:** Das Repository nennt `nodeJsRecommended`, der Admin schreibt `versions.nodeNewestNext` in die Host-Datenpunkte, eine Meldung gibt es aber nicht. Vorschlag: wiederkehrende Erinnerung plus Log-Eintrag, kein hartes Abschalten. Bei Docker Hinweis auf ein neues Image statt `iobroker nodejs-update`. → [ioBroker.admin #3746](https://github.com/ioBroker/ioBroker.admin/issues/3746), unterstützt von mcm1957 ([#34](https://forum.iobroker.net/post/1364641))
- **OS-Version läuft aus:** Es gibt eine Meldung für Paket-Updates, aber keine, wenn die Debian-Version selbst ausläuft. → [ioBroker.admin #3535](https://github.com/ioBroker/ioBroker.admin/issues/3535) (von mcm1957), ergänzt um den Hinweis auf `distro-info` von Thomas Braun ([#30](https://forum.iobroker.net/post/1364633))

### 2. Große Versionssprünge in der Glocke kennzeichnen

**Heute:** Die Meldung „Adapter-Updates verfügbar“ ist eine einfache Liste. Ein Major-Update (z. B. BackItUp 4 → 5) steht genauso drin wie ein kleiner Patch. Nur der js-controller bekommt einen eigenen Text.

**Vorschlag:** Major-Updates markieren und darauf hinweisen, vorher den Changelog zu lesen.

**Aus dem Thread:** Vorschlag von mcm1957 ([#43](https://forum.iobroker.net/post/1364681))

**Status:** offen, Issue geplant

### 3. Backup vor dem Controller-Update per Knopf

**Heute:** Der Admin-Dialog zum Controller-Update empfiehlt `iob backup` als ersten Schritt, aber nur in der Anleitung fürs Terminal. Wer das Update per Knopf im Admin startet, bekommt keine Sicherung, der Upgrade-Prozess des js-controllers enthält keinen Backup-Schritt. Automatisch gesichert werden sonst nur Objekte und States (bei der Standard-Datenbank jsonl alle 2 Stunden, bei Redis nicht).

**Vorschlag:** Vor dem Update per Knopf anbieten, `iob backup` auszuführen. Einrichten muss der Nutzer dafür nichts, der Befehl ist eingebaut.

**Aus dem Thread:** mcm1957 ([#43](https://forum.iobroker.net/post/1364681)), OliverIO ([#44](https://forum.iobroker.net/post/1364684))

**Status:** offen, Issue geplant

### 4. Planbare Node-Empfehlung

**Heute:** Die empfohlene Node-Version ändert sich „irgendwann, wenn das Bauchgefühl sagt, es ist langsam an der Zeit“.

**Vorschlag:** Die Empfehlung an den Release-Plan von Node.js koppeln, z. B. immer zum gleichen Zeitpunkt im Jahr. Adapter-Entwickler und Nutzer könnten den Wechsel dann einplanen. Ausdrücklich als Empfehlung, nicht als Pflicht.

**Aus dem Thread:** Thomas Braun ([#32](https://forum.iobroker.net/post/1364638)), mcm1957 einverstanden, solange es eine Empfehlung bleibt ([#39](https://forum.iobroker.net/post/1364659))

**Status:** offen

### 5. Ablauf der Releases in der Doku beschreiben

**Heute:** Für den js-controller gibt es im Forum Ankündigungen wie „jetzt für alle User im STABLE“, für Adapter legt ein Automatismus ein Issue an, wenn eine Version reif für stable ist. Nachlesen kann man den Ablauf aber nirgends.

**Vorschlag:** Eine Seite „Releases“ in der Doku: wie js-controller und Admin von beta nach stable kommen, ungefähre Mindestzeit, wo angekündigt wird. Für Adapter eine kurze Beschreibung des Wegs von latest nach stable bzw. eine Empfehlung im README-Template.

**Aus dem Thread:** OliverIO ([#42](https://forum.iobroker.net/post/1364680), [#49](https://forum.iobroker.net/post/1364691))

**Status:** offen

### 6. Widerspruch in der Update-Doku beheben

**Heute:** [Updaten](https://www.iobroker.com/docs/install/update.md) nennt die Reihenfolge Adapter → js-controller → Node.js, [Updates einspielen](https://www.iobroker.com/docs/tutorial/updates.md) genau umgekehrt. Docker kommt auf beiden Seiten nicht vor.

**Status:** → [ioBroker.docs #1013](https://github.com/ioBroker/ioBroker.docs/issues/1013)

## Bewusst nicht weiterverfolgt

| Idee | Warum nicht |
|------|-------------|
| Kuratiertes Gesamt-Release (wie Debian oder Home Assistant) | Adapter sind bewusst unabhängig, zu viel „Wildwuchs“, den man nicht beeinflussen kann – Thomas Braun ([#28](https://forum.iobroker.net/post/1364616)) |
| Fester Release-Rhythmus | Bei Freiwilligen kann niemand Termine garantieren – mcm1957 ([#43](https://forum.iobroker.net/post/1364681)). Besser: angekündigter Termin mit Vorlauf, siehe Punkt 5 |
| Reparaturen-Seite wie bei Home Assistant | Müsste von jedem Adapter-Entwickler umgesetzt werden – mcm1957 ([#43](https://forum.iobroker.net/post/1364681)), OliverIO ([#44](https://forum.iobroker.net/post/1364684)) |
| Abkündigungen mit Datum | Für js-controller und Admin denkbar, im Auge behalten – mcm1957 ([#43](https://forum.iobroker.net/post/1364681)) |

## Herkunft

Ursprünglich stand hier der Entwurf „Stack Care“, ein Konzept für versionierte Gesamt-Releases und eine eigene Admin-Seite „Systempflege“. Im Thread hat sich gezeigt, dass das nicht zu ioBroker passt und vieles schon vorhanden ist. Der alte Entwurf liegt unter [`archiv/`](archiv/).

## Lizenz

Texte: [CC BY-SA 4.0](LICENSE)
