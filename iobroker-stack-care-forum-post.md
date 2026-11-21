# Forum-Beitrag (nur diesen Block posten)

**Betreff:** Dauerhafte Systempflege – strukturelles Thema für Nutzer?

**Kategorie:** Vorschlag an ioBroker.de (oder „Allgemein“, je nach Forum)

---

Guten Tag,

dieser Beitrag betrifft keinen konkreten Adapter-Fehler, sondern ein Muster, das hier immer wieder auftaucht: **Viele wollen Hausautomatisierung, müssen aber faktisch ständig Systempflege mitbetreiben** – Node, OS oder Image, js-controller, Adapter, oft ohne klare Reihenfolge.

Die Einzelschritte sind in Doku und Image gut beschrieben. **Es fehlt aus Nutzersicht oft ein roter Faden:** *Ist meine Installation in Ordnung – und wenn nicht, was ist der nächste Schritt?* Das zeigt sich in wiederkehrenden Threads (Reihenfolge nach Updates, Docker-`latest`, Node, große Sprünge, viele gleichrangige Adapter-Updates). Umsteiger nennen selten „zu wenig Adapter“, eher: zu technisch, Updates zu riskant.

Es gab schon verschiedene **Images** (SBC, Raspberry, heute viel Docker). Technisch war oft etwas da; strittig blieb **wer dauerhaft freigibt und nachzieht**. Ohne klare Zuständigkeit landen wir wieder bei veralteten Erwartungen oder abgebrochenen Angeboten – Stichwort eingestelltes Raspberry-Image. Mir geht es nicht um „noch ein Image bauen“, sondern um **Release-Absprache, Governance und eine verständliche Antwort** – und einen **kleinen Anfang**, bevor man groß verspricht.

Als **Diskussionsentwurf** habe ich unter dem Namen **„Stack Care“** etwas skizziert – **meine Idee**, nichts von ioBroker offiziell beschlossen oder bereits umgesetzt. Gedacht ist ein **wartungsarmer, klar beschriebener Installationsweg** für die, die ihn freiwillig mitgehen wollen – zuerst z. B. festes Image-Tag und eine Upgrade-Story pro Release (**ohne** sofort neue Admin-UI). Perspektivisch: eine Admin-Übersicht mit wenigen Zuständen (in Ordnung / unkritisch / Wartung / abweichende Installation), Adapter vor allem aus `live`, beta/GitHub ehrlich ausgenommen.

**Kernfrage:** Erkennt ihr das Pflege-Problem in dieser Schärfe – und lohnt sich eine gemeinsame Richtung?

1. Wie dominant ist Pflege in eurer Beobachtung (Forum, Support)?
2. Was wäre ein akzeptabler „offizieller“ Weg (Docker-Image, Installer, beides)?
3. Wo soll Automatik enden (Major, Image) – reicht ein geführter Assistent mit Backup-Pflicht?
4. Was wäre ein **Minimum vor großer UX** (z. B. Release-Absatz + festes Tag + eine Upgrade-Anleitung) – und **wer könnte** das mittragen (Image-Pflege, Core)?

Ausführliche Ausarbeitung (Markdown, CC BY-SA): **https://github.com/crunchip77/iobroker-stack-care**  
Diskussion dort: **https://github.com/crunchip77/iobroker-stack-care/discussions/1**  
Gerne aber auch hier im Thread.

**Ehrlich:** Ohne Mitwirkung von Image-Maintainer und Core bleibt es eine Analyse – dann bitte auch so sagen.

Viele Grüße