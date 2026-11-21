# Forum-Beitrag (nur diesen Block posten)

**Betreff:** Updates & Systempflege – geht das für Normalnutzer einfacher?

**Kategorie:** Vorschlag an ioBroker.de (oder „Allgemein“, je nach Forum)

---

Guten Tag,

dieser Beitrag betrifft keinen einzelnen Adapter-Fehler, sondern etwas, das hier oft wiederkehrt: **Viele wollen das Zuhause automatisieren, müssen aber nebenbei ständig am „Unterbau“ schrauben** – Betriebssystem oder fertiges Installationspaket (Image), Node (die Laufzeit für ioBroker), js-controller, viele Adapter-Updates, und oft ohne klare Reihenfolge.

Für jeden Schritt gibt es **gute Anleitungen**. Was vielen fehlt, ist **eine einfache Gesamtantwort:** *Läuft bei mir alles normal – und wenn nicht, was soll ich als Nächstes tun, ohne Tage im Forum zu verbringen?* Typische Themen: nach einem Update geht nichts mehr, welche Node-Version gilt, Docker mit `latest` statt fester Version, große Sprünge nach Jahren ohne Pflege, dutzende Adapter-Updates auf einmal.

Auch in Rückmeldungen und Wechselberichten taucht immer wieder auf, dass **Nutzer ioBroker abbrechen oder wechseln** – häufig nicht wegen fehlender Funktionen, sondern weil der **Einstieg oder die laufende Pflege** als zu aufwendig empfunden wird. Ein Vergleich mit anderen Systemen will ich hier nicht führen; mir geht es nur darum: **Wie erleben Einsteiger und Bestandsnutzer ioBroker im Alltag** – eher als Automatisierung oder eher als IT-Projekt?

Es gab schon **fertige Images** (SD-Karte fürs Raspberry, heute oft Docker). Die Technik war oft da; unklar blieb: **Wer kümmert sich dauerhaft darum, welche Versionen zusammenpassen?** Ohne feste Zuständigkeiten enden solche Angebote irgendwann – Stichwort **eingestelltes Raspberry-Image**. Mir geht es nicht um „noch ein Image bauen“, sondern darum, **gemeinsam festzulegen, was „aktuell und empfohlen“ ist**, **wer das mitpflegt** und **dem Nutzer eine verständliche Rückmeldung** zu geben – und **klein anzufangen**, bevor man Großes verspricht.

Als **Diskussionsentwurf** habe ich unter dem Namen **„Stack Care“** etwas skizziert – **meine Idee**, nichts von ioBroker offiziell, auch noch **nichts umgesetzt**. Wer mag, könnte einen **klar beschriebenen, möglichst wartungsarmen Weg** mitgehen, zuerst ganz bescheiden: z. B. im Docker-Image **eine feste Version** statt `latest` und **eine verständliche Update-Anleitung pro Release** – **ohne** sofort eine neue Seite in der Verwaltung zu bauen. Später denkbar: in der **Admin-Oberfläche** eine Übersicht in wenigen Worten – *alles ok* / *kleine Updates möglich* / *größere Wartung mit Anleitung* / *deine Installation weicht ab*. Adapter aus dem normalen Repository (`live`) anders behandeln als Test-Versionen (`beta`) oder Selbstinstalliertes von GitHub – ehrlich sagen, was automatisch mitläuft und was nicht.

**Kernfrage:** Kennt ihr dieses Pflege-Problem auch – und lohnt es sich, **gemeinsam** eine Richtung zu suchen?

1. Wie oft seht ihr im Forum Hilfeanfragen, die eigentlich „wie pflege ich mein System?“ sind – oder Wechsel/Abbruch, weil es zu technisch wirkt?
2. Was wäre für euch ein akzeptabler **offiziell mitgepflegter Weg** – Docker-Image, Installer auf Debian, beides?
3. Wo soll **Automatik aufhören** (z. B. große Sprung-Updates, neues Image) – reicht ein **Assistent mit klaren Schritten und Backup davor**?
4. Was wäre ein **kleiner erster Schritt** (z. B. kurzer Hinweis im Image-Release + feste Version + eine Anleitung) – und **wer hätte dafür Zeit** (Docker-Image-Pflege, js-controller/Admin-Team)?

Ausführlich (normaler Text, online lesbar): **https://github.com/crunchip77/iobroker-stack-care**  
Diskussion dort: **https://github.com/crunchip77/iobroker-stack-care/discussions/1**  
– oder einfach hier im Thread.

Viele Grüße