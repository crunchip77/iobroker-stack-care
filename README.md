# Systempflege für Normalnutzer – Vorschläge aus dem Forum

Zusammenfassung der Diskussion im ioBroker-Forum: [Updates & Systempflege – geht das für Normalnutzer einfacher?](https://forum.iobroker.net/topic/85889/updates-systempflege-geht-das-für-normalnutzer-einfacher)

> **Stand:** 24. November 2026 – die Diskussion läuft noch. Privat zusammengestellt, nichts Offizielles von ioBroker. Gedacht als Grundlage, falls die Punkte z. B. im Dev-Meeting besprochen werden.

## Worum es geht

Viele Nutzer wollen ihr Haus automatisieren, müssen sich aber nebenbei um Betriebssystem, Node.js, js-controller und Adapter kümmern. Im Thread hat sich gezeigt: ioBroker bringt dafür schon einiges mit (Updates und Controller-Upgrade aus dem Admin, automatische Adapter-Updates, Hinweise auf OS-Updates und Neustart in der Glocke, eine empfohlene Node-Version im Repository). Vieles ist aber verstreut oder nur im Terminal sichtbar, z. B. über `iob diag`.

Ein kuratiertes Gesamt-Release wie bei Debian oder Home Assistant passt nicht zu ioBroker, weil die Adapter bewusst unabhängig sind. Die Vorschläge unten bauen deshalb auf vorhandenen Teilen auf.

## Übersicht

| Thema | Stand | Begründung |
|------|------|-------------|
| Widerspruch in der Update-Doku (Punkt 6) | erledigt | [ioBroker.docs #1013](https://github.com/ioBroker/ioBroker.docs/issues/1013), behoben mit [PR #1014](https://github.com/ioBroker/ioBroker.docs/pull/1014) |
| Node nicht mehr empfohlen (Teil von Punkt 1) | Issue da, PR offen | [ioBroker.admin #3746](https://github.com/ioBroker/ioBroker.admin/issues/3746), [PR #3747](https://github.com/ioBroker/ioBroker.admin/pull/3747) noch nicht gemergt |
| OS-Version läuft aus (Teil von Punkt 1) | Issue da, noch nicht umgesetzt | [ioBroker.admin #3535](https://github.com/ioBroker/ioBroker.admin/issues/3535), Bluefox schlägt lokale Prüfung im js-controller vor |
| Warnung vor Major-Update (Punkt 2) | Issue da | [ioBroker.admin #3161](https://github.com/ioBroker/ioBroker.admin/issues/3161). Markierung in der Glocke wäre nur eine Ergänzung dazu |
| Letztes Backup anzeigen, Backup vor Update, Hinweis „nur lokal“ (Punkt 3) | Issue da | [ioBroker.admin #3750](https://github.com/ioBroker/ioBroker.admin/issues/3750), Kommentar zur Übersicht und zum zweiten Speicherort |
| Vorherige Adapter-Version markieren, Rückweg ohne Expertenmodus (Punkt 7) | Issue da | [ioBroker.admin #3751](https://github.com/ioBroker/ioBroker.admin/issues/3751), Kommentar: Dialog nach fehlgeschlagenem Start auch ohne Expertenmodus |
| Adapter-Update braucht neuere Node (Punkt 10) | Issue da | [ioBroker.admin #3752](https://github.com/ioBroker/ioBroker.admin/issues/3752) |
| Docker: Image-Tag statt `latest` (Punkt 8) | Issue da | [ioBroker.docs #1016](https://github.com/ioBroker/ioBroker.docs/issues/1016). Glocke hat schon `dockerUpdate` |
| Eine Übersicht statt vier Stellen (Teil von Punkt 1) | an vorhandene Issues gehängt | Kommentare an [#3750](https://github.com/ioBroker/ioBroker.admin/issues/3750), [#1914](https://github.com/ioBroker/ioBroker.admin/issues/1914), [#3746](https://github.com/ioBroker/ioBroker.admin/issues/3746) |
| Major-Updates in der Liste/Glocke gruppieren (Punkt 2) | an vorhandenes Issue gehängt | Kommentar an [#3161](https://github.com/ioBroker/ioBroker.admin/issues/3161) |
| `iob diag` in der Kachel „Systemstatus“ (Punkt 1) | ausstehend | Node und OS haben schon Issues, der Rest (root, 32 Bit, Vorabversionen, leeres Backup-Verzeichnis, ausdrückliches „alles in Ordnung“) nicht. Kachel prüft heute nur, ob der Host antwortet |
| Planbare Node-Empfehlung (Punkt 4) | ausstehend, eher Dev-Meeting | Thomas Braun vorgeschlagen, mcm1957 einverstanden als Empfehlung. Kein Code, sondern Absprache |
| Ablauf der Releases in der Doku (Punkt 5) | ausstehend, eher Dev-Meeting | Vorschlag von OliverIO, gehört ins Docs-Repo |
| Rücksprung auf die alte Controller-Version bei fehlgeschlagenem Update | noch klären | mcm1957: „muss man sich ansehen“. Technisch unklar, ob das mit npm so einfach ginge wie bei HA OS |
| Abkündigungen mit Datum | bewusst nicht als Issue | mcm1957: beim Core in den letzten Jahren kaum etwas weggefallen, bei Adaptern nur der Maintainer. Bleibt im Auge |
| Kuratiertes Gesamt-Release, fester Rhythmus, Reparaturen-Seite wie bei HA | bewusst nicht | Passt nicht zu unabhängigen Adaptern bzw. müsste jeder Adapter-Entwickler umsetzen |

## Vorschläge

### 1. Ergebnis von `iob diag` im Admin anzeigen

**Heute:** `iob diag` prüft schon vieles und markiert es farbig: Ausführung als root, 32-Bit-System, Abweichung von der empfohlenen Node-Hauptversion, nicht mehr unterstütztes OS (über `distro-info`), Vorabversionen von Node oder js-controller. In der nächsten Version kommen weitere Bereiche dazu, z. B. ein leeres Backup-Verzeichnis. Sichtbar ist das aber nur, wenn man `iob diag` im Terminal aufruft.

Die Übersicht im Admin hat schon eine Kachel „Systemstatus – OK – ioBroker läuft stabil“. Sie zeigt aber nur, ob der Host antwortet (`system.host.<host>.alive`), und steht deshalb auch auf OK, wenn Node veraltet ist oder lange kein Backup lief.

**Vorschlag:** Diese Ampel im Admin anzeigen, prominent, aber ohne dass man sie ständig wegklicken muss, z. B. in der vorhandenen Kachel „Systemstatus“. Die Regeln müssen nicht neu definiert werden, sie werden in `iob diag` schon gepflegt.

Dazu gehört, die verstreuten Infos an einer Stelle zu sammeln. Heute liegen letztes Backup, freier Speicher, neuer js-controller, Adapter-Updates und die Glocke auf verschiedenen Seiten. Die Übersicht kann das aufnehmen, ohne eine neue Seite: Systemstatus (dieser Punkt), letztes Backup ([#3750](https://github.com/ioBroker/ioBroker.admin/issues/3750)), sichtbares Controller-Update ([#1914](https://github.com/ioBroker/ioBroker.admin/issues/1914)). Kommentare dazu an den drei Issues.

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

**Status:** Gibt es schon als Issue: [ioBroker.admin #3161](https://github.com/ioBroker/ioBroker.admin/issues/3161) (Garfonso, offen). Dort geht es um eine Warnung mit Bestätigung vor einem Major-Update im Admin, auf der Kommandozeile gibt es die schon. Ergänzung als Kommentar dort: Major-Updates in der Update-Liste und in der Glocke markieren oder gruppieren, damit ein Sprung wie 4→5 nicht zwischen 40 Patches untergeht.

### 3. Backup vor dem Controller-Update per Knopf

**Heute:** Der Admin-Dialog zum Controller-Update empfiehlt `iob backup` als ersten Schritt, aber nur in der Anleitung fürs Terminal. Wer das Update per Knopf im Admin startet, bekommt keine Sicherung: Der Upgrade-Manager des js-controllers stoppt den Controller, installiert die neue Version und startet neu, ein Backup-Schritt ist nicht dabei. Automatisch gesichert werden sonst nur Objekte und States (bei der Standard-Datenbank jsonl alle 2 Stunden, bei Redis nicht).

**Vorschlag:** Vor dem Update per Knopf anbieten, `iob backup` auszuführen. Einrichten muss der Nutzer dafür nichts, der Befehl ist eingebaut.

**Aus dem Thread:** mcm1957 ([#44](https://forum.iobroker.net/post/1364681)), OliverIO ([#45](https://forum.iobroker.net/post/1364684))

**Status:** → [ioBroker.admin #3750](https://github.com/ioBroker/ioBroker.admin/issues/3750) (letztes Backup im Update-Dialog und in der Übersicht anzeigen, Backup vor dem Update anbieten). mcm1957 hat darum gebeten ([#56](https://forum.iobroker.net/post/1364786)). Kommentar dort: Hinweis, wenn das Backup nur auf demselben Gerät liegt (BackItUp sichert ab Werk lokal, `iob backup` ebenfalls).

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

**Status:** erledigt → [ioBroker.docs #1013](https://github.com/ioBroker/ioBroker.docs/issues/1013), behoben mit [PR #1014](https://github.com/ioBroker/ioBroker.docs/pull/1014) (gemergt am 23.11.2026). Beide Seiten nennen jetzt Adapter → js-controller → Node.js. Neu dazugekommen sind ein Hinweis für Docker (neues Image statt `iobroker nodejs-update` bzw. `apt`) und, auf Anregung von mcm1957 im Issue, ein Abschnitt für den Fall, dass ein Adapter schon eine neuere Node-Version verlangt. Was im Admin daraus folgt, steht unter Punkt 10. Was beim Docker-Weg noch fehlt, unter Punkt 8.

### 7. Rückweg nach einem Adapter-Update

**Heute:** Absturzschleifen werden gestoppt und in der Glocke gemeldet. Eine ältere Version installiert man nur im Expertenmodus. Die vorher installierte Version merkt sich der Admin nicht.

**Vorschlag:** Nach dem Update kurz sagen, ob die Instanz wieder läuft. Wenn nicht: vorherige Version nennen und den Dialog „bestimmte Version installieren“ anbieten, auch ohne Expertenmodus. Der Code kommt zurück, Objekte und States nicht – das muss im Dialog stehen.

**Status:** an [#3751](https://github.com/ioBroker/ioBroker.admin/issues/3751) als Kommentar gehängt (Markierung der vorherigen Version plus Dialog ohne Expertenmodus nach fehlgeschlagenem Start)

### 8. Docker: Image-Tag statt `latest`

**Heute:** Die Glocke kann `dockerUpdate` melden, wenn ein neues offizielles Image da ist. Die Doku sagt seit #1013, dass Node und OS mit dem Image kommen. Viele Compose-Dateien nutzen trotzdem `latest`. Dann ist unklar, auf welchem Stand man ist, und zurück geht schwer.

**Vorschlag:** In der Doku die Schritte festhalten: Tag pinnen, neues Image ziehen, Container neu anlegen (Daten im Volume bleiben). In der Übersicht oder in der Glocke den aktuellen und den neuen Tag nennen, wenn die Meldung schon kommt.

**Status:** → [ioBroker.docs #1016](https://github.com/ioBroker/ioBroker.docs/issues/1016)

### 9. Adapter-Updates nach Größe gruppieren

Siehe Punkt 2. Nur die Liste/Glocke, nicht der Bestätigungsdialog – der liegt schon in [#3161](https://github.com/ioBroker/ioBroker.admin/issues/3161).

### 10. Adapter-Update braucht neuere Node.js

**Heute:** Die Doku erklärt seit #1013, was man tun soll, wenn ein Adapter schon eine neuere Node-Version verlangt. Im Admin kann das Update trotzdem wie ein normales Update aussehen.

**Vorschlag:** Solche Updates nicht als normalen Ein-Klick-Update anbieten (ausgrauen oder erklären): Adapter braucht Node x, Host hat y, entweder Node zuerst oder eine passende ältere Adapterversion.

**Status:** → [ioBroker.admin #3752](https://github.com/ioBroker/ioBroker.admin/issues/3752)

## Gesehen bei Home Assistant (noch nicht diskutiert)

Verglichen mit einer HA-OS-Installation (Stand 2026.11) und der HA-Doku. Vieles davon hat ioBroker auch: Changelog im Update-Dialog, Version ignorieren, Warnung vor Major-Updates auf der Konsole, BackItUp bei der Neuinstallation über den Installer mit täglichem Minimal-Backup ab Werk (02:48 Uhr, lokal, 10 Tage aufbewahrt), letztes und nächstes Backup im BackItUp-Reiter, freier Speicherplatz und neuer js-controller unter „Hosts“, Meldungen in der Glocke z. B. bei wenig Speicherplatz, bei einer ständig abstürzenden Instanz (wird gestoppt) oder einer zurückgezogenen Version (Instanz wird deaktiviert).

Was HA anders macht:

| Bei Home Assistant | Bei ioBroker heute |
|------|-------------|
| Im Update-Dialog steht „Letztes automatisches Backup vor 6 Stunden“, daneben ein Schalter „Automatisches Backup vor dem Update“ (Voreinstellung wählbar, pro Update änderbar) | BackItUp kennt das letzte Backup (`backitup.0.history.iobrokerLastTime`), im Update-Dialog wird es nicht angezeigt. Siehe auch Punkt 3. → [ioBroker.admin #3750](https://github.com/ioBroker/ioBroker.admin/issues/3750) |
| Bei App-Updates „Backup der letzten Version behalten“ als Rückweg per Knopf | Einen solchen Rückweg gibt es nicht. Eine ältere Version lässt sich im Expertenmodus installieren („bestimmte Version installieren“), die Daten des Adapters kommen dabei aber nicht mit zurück. Die Liste dort markiert „aktuell“, „latest“ und „stable“, nicht die vorher installierte Version. Idee von mcm1957: die letzte Version dort markieren ([#56](https://forum.iobroker.net/post/1364786)). → [ioBroker.admin #3751](https://github.com/ioBroker/ioBroker.admin/issues/3751). Verwandt, aber nicht dasselbe: [ioBroker.admin #2973](https://github.com/ioBroker/ioBroker.admin/issues/2973) (Downgrade auf die Version im Repository, z. B. nach Wechsel von latest auf stable) |
| Startet HA nach einem Core-Update nicht, spielt der Supervisor die alte Version zurück, sichert das Log und meldet das | Das Controller-Update per Knopf stoppt den Controller, installiert und startet neu. Ein Fehler wird gemeldet, ein Rücksprung auf die alte Version passiert nicht. mcm1957: „muss man sich ansehen“ ([#56](https://forum.iobroker.net/post/1364786)) |
| Der Supervisor prüft stündlich, u. a. „kein aktuelles Backup“, Speicherplatz, Neustart nötig. Behobene Probleme verschwinden von selbst, ohne Probleme steht dort „Aktuell stehen keine Reparaturen an“ | Speicherplatz und „Neustart nötig“ meldet der js-controller auch in der Glocke. Eine zentrale Prüfung auf ein aktuelles Backup gibt es nicht (`iob diag` merkt in der noch nicht veröffentlichten Version ein leeres Backup-Verzeichnis an). Die Kachel „Systemstatus OK“ prüft nur, ob der Host antwortet. Siehe Punkt 1 |
| Wichtige Infos gesammelt in der Systemübersicht, z. B. „Letzte Datensicherung vor 6 Stunden“, „39 % belegt“ | Auch vorhanden, aber verteilt auf BackItUp-Reiter, Hosts und Glocke |
| Regel für die Entwickler: Was für Nutzer wegfällt, muss mindestens 6 Monate vorher angekündigt werden, mit Meldung, ab welcher Version es nicht mehr geht ([Doku](https://contributors.home-assistant.io/core/development/deprecating/)) | Siehe „Abkündigungen mit Datum“ unten. mcm1957: Beim Core ist in den letzten Jahren kaum etwas weggefallen, bei Adaptern kann nur der Maintainer ankündigen. Weil niemand eine neue Major-Version sofort einspielen muss, ist die Ankündigung im Grunde gegeben ([#56](https://forum.iobroker.net/post/1364786)) |

Zur Einordnung:

- Rücksprung und die stündlichen Systemprüfungen macht bei HA der Supervisor. Den gibt es nur auf HA OS (und der Supervised-Installation), nicht bei der reinen Docker-Installation.
- Auf der Reparaturen-Seite landen zwei Arten von Meldungen: zentrale vom System (Supervisor und HA-Core, z. B. Backup, Speicherplatz, Neustart, veraltete Konfiguration) und Meldungen der Integrationen selbst (laut HA-Doku z. B. zu alte Gerätefirmware, abgeschaltete API, nicht mehr unterstützte Konfiguration). Letztere müssen die Integrationen tatsächlich selbst umsetzen, wie mcm1957 angemerkt hat ([#44](https://forum.iobroker.net/post/1364681)).
- Bei ioBroker gibt es für beides schon Gegenstücke: Adapter können eigene Meldungen in der Glocke anlegen (BackItUp z. B. bei fehlgeschlagenen Backups), zentrale Meldungen kommen vom js-controller. Außerdem kann ein Entwickler über die Admin-Meldungen gezielt alle Installationen mit einer bestimmten Adapterversion anschreiben, z. B. wenn eine API abgeschaltet wurde. Das wird laut mcm1957 aber kaum genutzt, und API-Abschaltungen sind meist Reaktion statt Ankündigung ([#57](https://forum.iobroker.net/post/1364787)). Was fehlt, sind vor allem die Prüfungen aus `iob diag` im Admin und eine Prüfung auf ein aktuelles Backup (Punkt 1).

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
