# ioBroker Stack Care (RFC-Entwurf)

**Diskussionsentwurf** für ein verbindlicheres **Betriebsmodell** rund um Systempflege (Image, Node, js-controller, Adapter) – nicht ein neues Docker-Produkt, sondern eine Schicht aus **Stack-Releases**, klarer Nutzersprache und (perspektivisch) einer Admin-Ansicht „Systempflege“.

> **Status (November 2026):** Diskussionsentwurf aus der Community-Runde – **nicht** offiziell von ioBroker, **keine** Produktzusage, **keine** Garantie auf Umsetzung. Marken „ioBroker“ gehören den jeweiligen Rechteinhabern.

**Sichtbarkeit:** Dieses Repository ist **öffentlich** (GitHub Public), damit Forum, Image-Pflege und Core alles ohne Zugang lesen können. Nur **credentials, private Mails oder interne Notizen** gehören nicht ins Repo (siehe [CONTRIBUTING.md](CONTRIBUTING.md)).

---

## Kurzfassung

Viele Nutzer wollen automatisieren, nicht dauernd Linux/Node/Docker pflegen. Bausteine (offizielles Image, `iob fix`, BackItUp, Admin) gibt es – es fehlt oft ein **gemeinsames Release-Versprechen** und **eine Antwort**: *Ist meine Installation in Ordnung – und wenn nicht, was ist der nächste Schritt?*

**Stack Care** schlägt vor, **klein mit P1** zu beginnen (Release-Text + festes Tag + Upgrade-Story, ohne Core-Code) und UX **erst nach** Mitwirkung von Image-Pflege und Core zu bauen.

---

## Einstieg lesen

| Zuerst | Inhalt |
|--------|--------|
| **[Ausarbeitung](iobroker-stack-care-ausarbeitung.md)** | Argumentation in ~15–20 Minuten – **hier starten** |
| [Review](iobroker-stack-care-review.md) | Sinn, Aufwand, Image-Historie |
| [Konzept](iobroker-stack-care-konzept.md) | Vollständige Spezifikation (§ 1–15) |

---

## Weitere Dokumente

| Datei | Für wen |
|--------|---------|
| [Admin-Mockup](iobroker-stack-care-admin-mockup.md) | Optional: UI-Skizze für Phase P2 |
| [Manifest-Beispiel](examples/manifest-stable.example.json) | Struktur P2+ (Platzhalter) |

Forum-Kurztext nur im [ioBroker-Forum-Thread](https://forum.iobroker.net/topic/85889/updates-systempflege-geht-das-für-normalnutzer-einfacher) – nicht als Duplikat im Repo.

---

## Feedback

- **ioBroker-Forum:** https://forum.iobroker.net/topic/85889/updates-systempflege-geht-das-für-normalnutzer-einfacher  
- **[Discussions](https://github.com/crunchip77/iobroker-stack-care/discussions/1)** – GitHub-Diskussion (RFC-Feedback)  
- **[Issues](https://github.com/crunchip77/iobroker-stack-care/issues)** – konkrete Punkte (Labels: `feedback`, `p1-pilot`)  

Pull Requests willkommen; größere Richtungsänderungen bitte vorher kurz in einer Discussion oder einem Issue skizzieren.

---

## Phasen (Kurz)

| Phase | Inhalt |
|-------|--------|
| **P0** | RFC – dieses Repo |
| **P1** | Image-Release mit Stack-Absatz, ohne neue Software |
| **P2+** | Admin liest Manifest, Assistent, Gates |

Details: [Ausarbeitung §7](iobroker-stack-care-ausarbeitung.md#7-phasen--was-wann-sinnvoll-ist).

---

## Risiken (v1)

Docker-Pfad: kein blindes `latest`, Watchtower abschalten, kein Host-`iob nodejs-update` statt Image-Tag, Backup vor riskanten Schritten. Siehe [Konzept §15](iobroker-stack-care-konzept.md), [Review](iobroker-stack-care-review.md).

---

## Lizenz

Texte und Beispiele: [CC BY-SA 4.0](LICENSE) – Weitergabe mit Namensnennung; Bearbeitungen unter gleicher Lizenz.