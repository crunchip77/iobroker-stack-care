# Forum-Entwurf (zum Posten)

**Betreff-Idee:** Dauerhafte Systempflege – strukturelles Thema für Nutzer und Community?

---

Guten Tag,

mit diesem Beitrag geht es nicht um einen konkreten Adapter-Fehler, sondern um ein Muster, das im Forum, in Rückmeldungen von Umsteigern und in der Diskussion um Verbreitung von ioBroker immer wieder auftaucht: **Viele Nutzer wollen Hausautomatisierung, müssen aber faktisch ständig Systemadministration mitbetreiben** (Node, Betriebssystem oder Image, js-controller, Adapter – oft ohne klare Reihenfolge).

Das ist nicht nur eine Einzelmeinung. Man sieht es **an den wiederkehrenden Forum-Themen** – nach Updates funktioniert nichts mehr, welche Node-Version gilt, was zuerst aktualisiert werden muss, Docker-`latest`, OS-Update und danach Ausfall, Angst vor großen Sprüngen nach Jahren ohne Pflege, dutzende Adapter-Updates ohne Priorisierung. Die Doku zu den Einzelschritten ist oft gut; **es fehlt ein verbindlicher roter Faden**: *Ist meine Installation in Ordnung – und wenn nein, was ist der nächste Schritt?*

Parallel liest man **häufig von Wechseln zu anderen Plattformen**. Begründungen sind selten „zu wenig Adapter“, sondern eher: zu technisch, Updates zu riskant, zu viel Zeit für Pflege. Das steht im Widerspruch zum Bild reiner Hausautomatisierung, mit dem ioBroker bei Einsteigern ankommt.

In den **Jahresrückblicken** (z. B. Forum, ioBroker-Blog) wird weiterhin von wachsender Zahl gemeldeter Installationen gesprochen; die **öffentliche Statistik** ([iobroker.com/statistics](https://www.iobroker.com/statistics?lang=de), freiwillige Meldung) zeigt den Installationsbestand. Gleichzeitig deutet die Kombination aus Forum, Umsteiger-Berichten und der Pflege-Diskussion darauf hin, dass **Neu- und Bestandsnutzer an genau dieser Hürde hängen** – nicht nur an fehlenden Funktionen. Das wäre aus meiner Sicht eine Frage für die Community, nicht bloß eine Randnotiz.

**Kernpunkt:** Nicht „noch ein Adapter“, sondern ein **verständliches Betriebsmodell** – ein offiziell gepflegter Installationsweg mit klarer Erwartung; in Admin **eine** Wartungsübersicht (in Ordnung / unkritische Updates / Wartung mit Plan / abweichende Installation); unkritisches Update optional automatisch; kritisches Update geführt, mit Backup davor; Node und OS im Hintergrund, sichtbar nur „läuft“ oder „hier handeln“.

Dazu ein historischer Punkt: Es gab schon **Images** – früher SBC-Images auf der Website, später das **Raspberry-Pi-Image** (buanet), heute sehr viel das **Docker-Image**. Das technische Angebot war oft da; strittig war regelmäßig **wer dauerhaft freigibt und nachzieht**, wenn Maintainer-Kapazität fehlt. Ohne diese Klärung bringt auch das beste Image auf Dauer wieder veraltete Erwartungen oder ein wieder abgebrochenes Angebot – siehe die Diskussion um das eingestellte Raspberry-Image im Forum. Deshalb geht es mir weniger um „noch ein Image bauen“, sondern um **Release, Zuständigkeit und eine verständliche Antwort für Nutzer** – und um einen **kleinen Anfang**, bevor man groß verspricht.

Das ist noch **kein fertiger Lösungsvorschlag**, sondern die Frage: **Erkennt ihr das Problem in dieser Schärfe mit – und lohnt sich eine gemeinsame Richtung?**

Kurz zur Diskussion:
1. Wie dominant ist die Pflege-Thematik in eurer Beobachtung (Forum, Support für andere)?
2. Was wäre ein akzeptabler „offizieller“ Weg (Image, Installer, beides)?
3. Wo sollte Automatik enden (Major, Node, Image) – reicht ein Assistent mit Backup-Pflicht?
4. Wer könnte einen **Release-Rhythmus** mittragen (Image-Pflege, Core) – und was wäre ein **Minimum** vor größerer UX (z. B. festes Tag + eine Upgrade-Story pro Release)?

Ausführliche Ausarbeitung (Entwurf, kein offizieller Beschluss): **https://github.com/crunchip77/iobroker-stack-care** – Feedback in [Discussions/Issues](https://github.com/crunchip77/iobroker-stack-care/discussions) oder in diesem Thread.

---

## Rückfrage vorbereiten: „Wer wartet das?“ (nicht mitposten)

Kurzantwort fürs Forum:

Heute „wartet“ im Prinzip **jeder Einzelnutzer** sein eigenes Konglomerat aus OS, Node, Image, Controller und Adaptern – schlecht, unterschiedlich, oft im Forum. Die Frage ist nicht ob Pflege entsteht, sondern ob sie **zentral, klein und getestet** statt **tausendfach und chaotisch** anfällt.

**Was überhaupt gepflegt werden müsste** (nicht alles von einer Person):

| Teil | Sinnvoller Ort heute |
|------|----------------------|
| Basis (Debian/Node im Container) | ohnehin **Image-Pflege** (buanet / offizielles Docker-Image) |
| js-controller, Host-Funktionen | **Core-Team**, wenn Admin/Controller das Nutzerversprechen mittragen |
| „Welche Versionen gehören zusammen“ | kleines **Stack-Manifest** pro Image-Release (Release Notes + feste Tags, kein `latest`) |
| Admin-Anzeige „in Ordnung / Wartung“ | Core oder Admin; kein zweites Parallel-Ökosystem nötig |
| Test vor Freigabe | **Freiwillige** aus Forum/Discord (wie bei Beta-Adaptern) |

**Minimalanfang ohne neuen Vereinsapparat:** Kein neues Produkt tag eins, sondern abgestimmte **Release-Empfehlung** zum bestehenden Image (festes Tag, dokumentierte Kombination, klare Upgrade-Anleitung als *ein* Ablauf). Das wäre weniger Arbeit als die Endlosschleife identischer Forum-Hilfen.

**Ehrlich:** Ohne Mitmachen von Core und Image-Maintainer geht das nicht – dann bleibt alles beim Status quo. Der Beitrag soll klären, ob sich das Problem lohnt und wer *bereit* wäre, welchen Teil zu tragen – nicht einen fertigen Betreiber verkünden.

---

## Rückfrage: stable / beta / GitHub-Adapter (Antwort fürs Forum, kopierbar)

Dazu kurz meine gedachte Linie – offen zur Diskussion:

Das „wartungsarm“-Versprechen soll **nicht** heißen: jeden Adapter jeder Quelle wird nachts automatisch mitgezogen. Im Alltag gibt es drei Fälle:

**`live` (stable Repo)** – das ist der Normalfall für Laien: kleine Updates (patch/minor) können automatisch oder gebündelt laufen; **Major-Versionen** bewusst im Wartungs-Assistenten, nicht im Blindflug. So nutzt man ohnehin schon die Auto-Upgrade-Logik des js-controller – nur mit klaren Defaults.

**`beta`-Repo** – bleibt möglich für Tester, gehört aber **nicht** zum stable-Stack. Wer dauerhaft auf beta installiert, ist auf einem Tester-Pfad: anderer Anspruch, weniger Automatik.

**GitHub / eigene URL** – bleibt für Fixes und Entwicklung wie heute; wird **nicht** in die Auto-Pflege einbezogen. Wer einen GitHub-Adapter hat, bekommt eher einen Hinweis „außerhalb des geprüften Stack“ – der Rest des Systems kann trotzdem grün sein („in Ordnung, 2 Adapter ausgenommen“).

Gemischte Bestände sind realistisch: 95 % live, ein paar Sonderfälle – dann wäre ehrlicher Status besser als so zu tun, der ganze Kasten sei „offiziell geprüft“, obwohl drei Adapter aus GitHub kommen.

Details: im Konzept `iobroker-stack-care-konzept.md`, Abschnitt 9.8.

---

## Rückfrage: KI / AI (Antwort fürs Forum, kopierbar)

Kommt sicher: „Warum nicht einfach KI, die pflegt?“ – meine Einordnung:

**KI ersetzt keinen Stack.** `stable` muss weiter aus getestetem Image/Manifest, Runbook und Backup vor dem Gefährlichen kommen – mit Mitwirkung von Image-Pflege und Core. Ohne das wird auch KI nur eine schnellere Anleitung in die falsche Richtung.

**Sinnvoll** wäre KI vor allem als **Erklärungsschicht**: Meldungen in Admin verständlich machen, aus Ist-Version + offizieller Empfehlung einen klaren Satz („alles ok“ / „unkritisch“ / „Wartung mit Plan“), bekannte Fehlerbilder aus Logs zuordnen und auf den **festen** Assistenten verweisen – nicht auf improvisierte Terminal-Kommandos.

**Nicht sinnvoll** für einen offiziellen Pflegepfad: dass ein Modell selbst Image zieht, Node wechselt, `apt` ausführt oder Adapter-Majors auslöst. Das ist nicht reproduzierbar, datenschutzrechtlich heikel (Logs, Geräte, Orte) und bei Heizung & Co. ein Risiko, das wir mit Stack Care eigentlich verkleinern wollen.

Praktisch: erst **regelbasiert** (Manifest, Zustände, js-controller-Automation für patch/minor), **KI optional** später und Opt-in – Dolmetscher, nicht root.

Konzeptdetail: `iobroker-stack-care-konzept.md`, Abschnitt 9.10.

---

*(Interne Notiz: Technik → `iobroker-stack-care-konzept.md`)*