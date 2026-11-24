# Systempflege für Normalnutzer – Vorschläge aus dem Forum

Zusammenfassung der Diskussion im ioBroker-Forum: [Updates & Systempflege – geht das für Normalnutzer einfacher?](https://forum.iobroker.net/topic/85889/updates-systempflege-geht-das-für-normalnutzer-einfacher)

> **Stand:** 24. November 2026 – die Diskussion läuft noch. Privat zusammengestellt, nichts Offizielles von ioBroker. Gedacht als Grundlage, falls die Punkte z. B. im Dev-Meeting besprochen werden.

## Worum es geht

Viele Nutzer wollen ihr Haus automatisieren, müssen sich aber nebenbei um Betriebssystem, Node.js, js-controller und Adapter kümmern. Im Thread hat sich gezeigt: ioBroker bringt dafür schon einiges mit (Updates und Controller-Upgrade aus dem Admin, automatische Adapter-Updates, Hinweise auf OS-Updates und Neustart in der Glocke, eine empfohlene Node-Version im Repository). Vieles ist aber verstreut oder nur im Terminal sichtbar, z. B. über `iob diag`.

Ein kuratiertes Gesamt-Release wie bei Debian oder Home Assistant passt nicht zu ioBroker, weil die Adapter bewusst unabhängig sind. Die Vorschläge unten bauen deshalb auf vorhandenen Teilen auf.

## Vorschläge

### 1. Ergebnis von `iob diag` im Admin anzeigen

**Heute:** `iob diag` prüft schon vieles und markiert es farbig: Ausführung als root, 32-Bit-System, Abweichung von der empfohlenen Node-Hauptversion, nicht mehr unterstütztes OS (über `distro-info`), Vorabversionen von Node oder js-controller. In der nächsten Version kommen weitere Bereiche dazu, z. B. ein leeres Backup-Verzeichnis. Sichtbar ist das aber nur, wenn man `iob diag` im Terminal aufruft.

Die Übersicht im Admin hat schon eine Kachel „Systemstatus – OK – ioBroker läuft stabil“. Sie zeigt aber nur, ob der Host antwortet (`system.host.<host>.alive`), und steht deshalb auch auf OK, wenn Node veraltet ist oder lange kein Backup lief.

**Vorschlag:** Diese Ampel im Admin anzeigen, prominent, aber ohne dass man sie ständig wegklicken muss, z. B. in der vorhandenen Kachel „Systemstatus“. Die Regeln müssen nicht neu definiert werden, sie werden in `iob diag` schon gepflegt.

**Aus dem Thread:** Thomas Braun ([#53](https://forum.iobroker.net/post/1364694), [#31](https://forum.iobroker.net/post/1364633)), mcm1957: umsetzbar, wenn die Regeln definierbar sind ([#47](https://forum.iobroker.net/post/1364687)), OliverIO: prominent, ohne wiederholtes Wegklicken ([#45](https://forum.iobroker.net/post/1364684))

**Status:** offen

Zwei Teile davon sind schon als Issue eingetragen:

- **Node-Version nicht mehr empfohlen:** Das Repository nennt `nodeJsRecommended`, eine Meldung gab es bisher aber nicht. Idee aus dem Thread: wiederkehrende Erinnerung plus Log-Eintrag, kein hartes Abschalten – mcm1957 ([#35](https://forum.iobroker.net/post/1364641)). → [ioBroker.admin #3746](https://github.com/ioBroker/ioBroker.admin/issues/3746). Umsetzung liegt als [PR #3747](https://github.com/ioBroker/ioBroker.admin/pull/3747) vor (noch nicht gemergt): Warnung im Log und Meldung in der Glocke, höchstens alle 7 Tage, bei Docker mit Hinweis auf ein neueres Image.
- **OS-Version läuft aus:** Es gibt eine Meldung für Paket-Updates, aber keine, wenn die Debian-Version selbst ausläuft. → [ioBroker.admin #3535](https://github.com/ioBroker/ioBroker.admin/issues/3535) (von mcm1957), ergänzt um den Hinweis auf `distro-info` von Thomas Braun ([#31](https://forum.iobroker.net/post/1364633)). Im Issue schlägt Bluefox eine lokale Prüfung mit `distro-info` vor, als Meldung des js-controllers, die der Admin dann anzeigt. mcm1957 ist einverstanden, die Meldung soll wiederkommen (mindestens bei jedem Start), aber nicht täglich nerven. Noch nicht umgesetzt.

Verwandt, zum Node-Update aus dem Admin: [ioBroker.admin #2553](https://github.com/ioBroker/ioBroker.admin/issues/2553) (Dialogtext missverständlich, der Knopf aktualisiert nur innerhalb der installierten Hauptversion) und [#3152](https://github.com/ioBroker/ioBroker.admin/issues/3152) (bei einer neuen Hauptversion zeigt der Knopf ein Update an, führt es aber nicht aus)

### 2. Große Versionssprünge in der Glocke kennzeichnen

**Heute:** Die Meldung „Adapter-Updates verfügbar“ ist eine einfache Liste. Ein Major-Update (z. B. BackItUp 4 → 5) steht genauso drin wie ein kleiner Patch. Nur der js-controller bekommt einen eigenen Text.

**Vorschlag:** Major-Updates markieren und darauf hinweisen, vorher den Changelog zu lesen.

**Aus dem Thread:** Vorschlag von mcm1957 ([#44](https://forum.iobroker.net/post/1364681))

**Status:** Gibt es schon als Issue: [ioBroker.admin #3161](https://github.com/ioBroker/ioBroker.admin/issues/3161) (Garfonso, offen). Dort geht es um eine Warnung mit Bestätigung vor einem Major-Update im Admin, auf der Kommandozeile gibt es die schon. Die Markierung in der Glocke wäre eine Ergänzung dazu.

### 3. Backup vor dem Controller-Update per Knopf

**Heute:** Der Admin-Dialog zum Controller-Update empfiehlt `iob backup` als ersten Schritt, aber nur in der Anleitung fürs Terminal. Wer das Update per Knopf im Admin startet, bekommt keine Sicherung: Der Upgrade-Manager des js-controllers stoppt den Controller, installiert die neue Version und startet neu, ein Backup-Schritt ist nicht dabei. Automatisch gesichert werden sonst nur Objekte und States (bei der Standard-Datenbank jsonl alle 2 Stunden, bei Redis nicht).

**Vorschlag:** Vor dem Update per Knopf anbieten, `iob backup` auszuführen. Einrichten muss der Nutzer dafür nichts, der Befehl ist eingebaut.

**Aus dem Thread:** mcm1957 ([#44](https://forum.iobroker.net/post/1364681)), OliverIO ([#45](https://forum.iobroker.net/post/1364684))

**Status:** offen, bisher kein Issue dazu gefunden (weder beim Admin noch beim js-controller)

**Verwandt:** [ioBroker.admin #1914](https://github.com/ioBroker/ioBroker.admin/issues/1914) – ein verfügbares js-controller-Update auffälliger im Admin anzeigen

### 4. Planbare Node-Empfehlung

**Heute:** Die empfohlene Node-Version ändert sich „irgendwann, wenn das Bauchgefühl sagt, es ist langsam an der Zeit“ – Thomas Braun ([#38](https://forum.iobroker.net/post/1364646)).

**Vorschlag:** Die Empfehlung an den Release-Plan von Node.js koppeln, z. B. immer zum gleichen Zeitpunkt im Jahr. Adapter-Entwickler und Nutzer könnten den Wechsel dann einplanen. Ausdrücklich als Empfehlung, nicht als Pflicht.

**Aus dem Thread:** Thomas Braun ([#33](https://forum.iobroker.net/post/1364638)), mcm1957 einverstanden, solange es eine Empfehlung bleibt ([#40](https://forum.iobroker.net/post/1364659))

**Status:** offen

### 5. Ablauf der Releases in der Doku beschreiben

**Heute:** Für den js-controller gibt es im Forum Ankündigungen wie „jetzt für alle User im STABLE“, für Adapter legt ein Automatismus ein Issue an, wenn eine Version reif für stable ist. Nachlesen kann man den Ablauf aber nirgends.

**Vorschlag:** Eine Seite „Releases“ in der Doku: wie js-controller und Admin von beta nach stable kommen, ungefähre Mindestzeit, wo angekündigt wird. Für Adapter eine kurze Beschreibung des Wegs von latest nach stable bzw. eine Empfehlung im README-Template.

**Aus dem Thread:** OliverIO ([#43](https://forum.iobroker.net/post/1364680), [#50](https://forum.iobroker.net/post/1364691))

**Status:** offen

### 6. Widerspruch in der Update-Doku beheben

**Vorher:** [Updaten](https://www.iobroker.com/docs/install/update.md) nannte die Reihenfolge Adapter → js-controller → Node.js, [Updates einspielen](https://www.iobroker.com/docs/tutorial/updates.md) genau umgekehrt. Docker kam auf beiden Seiten nicht vor.

**Status:** erledigt → [ioBroker.docs #1013](https://github.com/ioBroker/ioBroker.docs/issues/1013), behoben mit [PR #1014](https://github.com/ioBroker/ioBroker.docs/pull/1014) (gemergt am 23.11.2026). Beide Seiten nennen jetzt Adapter → js-controller → Node.js. Neu dazugekommen sind ein Hinweis für Docker (neues Image statt `iobroker nodejs-update` bzw. `apt`) und, auf Anregung von mcm1957 im Issue, ein Abschnitt für den Fall, dass ein Adapter schon eine neuere Node-Version verlangt.

## Gesehen bei Home Assistant (noch nicht diskutiert)

Verglichen mit einer HA-OS-Installation (Stand 2026.11) und der HA-Doku. Vieles davon hat ioBroker auch: Changelog im Update-Dialog, Version ignorieren, tägliches Minimal-Backup von BackItUp ab Werk (02:48 Uhr, lokal, 10 Tage), letztes und nächstes Backup im BackItUp-Reiter, freier Speicherplatz und neuer js-controller unter „Hosts“, Meldungen in der Glocke z. B. bei wenig Speicherplatz, bei einer ständig abstürzenden Instanz (wird gestoppt) oder einer zurückgezogenen Version (Instanz wird deaktiviert).

Was HA anders macht:

| Bei Home Assistant | Bei ioBroker heute |
|------|-------------|
| Im Update-Dialog steht „Letztes automatisches Backup vor 6 Stunden“, daneben ein Schalter „Automatisches Backup vor dem Update“ (Voreinstellung wählbar, pro Update änderbar) | BackItUp kennt das letzte Backup (`backitup.0.history.iobrokerLastTime`), im Update-Dialog wird es nicht angezeigt. Siehe auch Punkt 3 |
| Bei App-Updates „Backup der letzten Version behalten“ als Rückweg per Knopf | Zurück nur über den Expertenmodus („bestimmte Version installieren“). Verwandt, aber nicht dasselbe: [ioBroker.admin #2973](https://github.com/ioBroker/ioBroker.admin/issues/2973) (Downgrade auf die Version im Repository, z. B. nach Wechsel von latest auf stable) |
| Startet HA nach einem Core-Update nicht, spielt der Supervisor die alte Version zurück, sichert das Log und meldet das | Beim Controller-Update per Knopf wird ein Fehler nur ins Log geschrieben, kein Rücksprung |
| Der Supervisor prüft stündlich zentral, u. a. „kein aktuelles Backup“, Speicherplatz, Neustart nötig. Behobene Probleme verschwinden von selbst, ohne Probleme steht dort „Aktuell stehen keine Reparaturen an“ | Einzelne Meldungen kommen in der Glocke, eine ausdrückliche Rückmeldung „alles in Ordnung“ gibt es nicht. Siehe Punkt 1 |
| Wichtige Infos gesammelt in der Systemübersicht, z. B. „Letzte Datensicherung vor 6 Stunden“, „39 % belegt“ | Auch vorhanden, aber verteilt auf BackItUp-Reiter, Hosts und Glocke |
| Was für Nutzer wegfällt, muss mindestens 6 Monate vorher angekündigt werden, mit Meldung, ab welcher Version es nicht mehr geht ([Doku](https://contributors.home-assistant.io/core/development/deprecating/)) | Keine feste Regel, siehe „Abkündigungen mit Datum“ unten |

Zur Einordnung: Rücksprung und Systemprüfungen macht bei HA der Supervisor, den gibt es nur auf HA OS, nicht bei der reinen Docker-Installation. Der System-Teil der Reparaturen-Seite kommt dort also zentral und nicht von den Integrationen. Bei ioBroker entspricht das am ehesten `iob diag` (Punkt 1).

## Bewusst nicht weiterverfolgt

| Idee | Warum nicht |
|------|-------------|
| Kuratiertes Gesamt-Release (wie Debian oder Home Assistant) | Adapter sind bewusst unabhängig, zu viel „Wildwuchs“, den man nicht beeinflussen kann – Thomas Braun ([#29](https://forum.iobroker.net/post/1364616)) |
| Fester Release-Rhythmus | Bei Freiwilligen kann niemand Termine garantieren – mcm1957 ([#44](https://forum.iobroker.net/post/1364681)). Besser: angekündigter Termin mit Vorlauf – OliverIO ([#43](https://forum.iobroker.net/post/1364680)), siehe Punkt 5 |
| Reparaturen-Seite wie bei Home Assistant | Müsste von den Adapter-Entwicklern umgesetzt werden – mcm1957 ([#44](https://forum.iobroker.net/post/1364681)). Alle Zustände und Kombinationen vorauszudenken wird schwierig – OliverIO ([#45](https://forum.iobroker.net/post/1364684)) |
| Abkündigungen mit Datum | Für js-controller und Admin denkbar, im Auge behalten – mcm1957 ([#44](https://forum.iobroker.net/post/1364681)) |

## Herkunft

Ursprünglich stand hier der Entwurf „Stack Care“, ein Konzept für versionierte Gesamt-Releases und eine eigene Admin-Seite „Systempflege“. Im Thread hat sich gezeigt, dass das nicht zu ioBroker passt und vieles schon vorhanden ist. Der alte Entwurf liegt unter [`archiv/`](archiv/).

## Lizenz

Texte: [CC BY-SA 4.0](LICENSE)
