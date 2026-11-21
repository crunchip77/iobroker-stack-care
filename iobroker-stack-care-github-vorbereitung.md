# Stack Care – GitHub-Repo (Checkliste vor Veröffentlichung)

**Zweck:** Wenn du die Dokumente **öffentlich** legst (eigenes Repo, kein offizielles ioBroker-Projekt), damit nichts Wichtiges fehlt.  
**Forum:** Kurzer Post + **ein** Link hierher; lange Texte nur im Repo.

---

## Öffentlich vs. privat (Empfehlung)

| | **Öffentliches GitHub-Repo** | **Privates Repo** |
|---|------------------------------|-------------------|
| **Ziel RFC / Forum** | ✓ Jeder kann lesen, verlinken, Issues | ✗ Leser brauchen Einladung |
| **Image-Pflege / Core** | Link reicht | extra Zugriff pflegen |
| **Forum-Zitat** | „siehe GitHub …“ ohne Hürde | wirkt geschlossen |
| **Geheimnisse** | **trotzdem** nicht committen | gleiches Risiko bei falschem Push |

**Empfehlung für Stack Care:** **public** auf GitHub. Cursor-Hosted-Origin ist ohnehin nicht öffentlich – für diese Diskussion **GitHub public** wählen.

**Nicht ins Repo:** persönliche Notizen, unveröffentlichte Mail-Entwürfe an Maintainer, Zugangsdaten. Die Forum-Master-Checkliste **darf** öffentlich sein (nur Taktik, keine Geheimnisse).

---

## A. Pflicht vor erstem Push

| # | Punkt | Erledigt |
|---|--------|----------|
| A1 | **README.md:** Vorlage `iobroker-stack-care-README.md` → beim Push als `README.md` ins Repo-Root |
| A2 | **Haftungs-/Status-Hinweis:** Diskussionsentwurf, keine offizielle ioBroker-Position, keine Garantie |
| A3 | **Dokumenten-Karte** (welche Datei wofür) – siehe Tabelle unten | ☐ |
| A4 | **Lizenz** CC BY-SA 4.0 im Repo (`iobroker-stack-care/LICENSE`) | ☐ |
| A5 | Betreff im Forum-Entwurf: Repo-URL eintragen (`forum-einstieg.md`) | ☐ |

---

## B. Empfohlen (Feedback & Diskussion)

| # | Punkt | Erledigt |
|---|--------|----------|
| B1 | **GitHub Discussions** oder Issues mit Label `feedback` / `p1-pilot` | ☐ |
| B2 | Kurz: „Feedback hier im Issue/Discussion, Forum-Thread verlinken“ | ☐ |
| B3 | **CONTRIBUTING.md:** nur Markdown-Fixes vs. inhaltliche Änderungen (wer merged?) | ☐ |
| B4 | Optional: **Manifest-Beispiel** als `examples/manifest-stable.example.json` aus Konzept §5 | ☐ |

---

## C. Dokumenten-Karte (Repo-Inhalt)

| Datei | Publikum |
|--------|----------|
| `iobroker-stack-care-ausarbeitung.md` | Einstieg, Argumentation (~15 Min.) |
| `iobroker-stack-care-konzept.md` | Vollspec |
| `iobroker-stack-care-review.md` | Sinn/Aufwand/Historie |
| `iobroker-stack-care-admin-mockup.md` | Optional, P2-Visualisierung |
| `iobroker-stack-care-core-checkliste.md` | Core-Gespräch |
| `iobroker-stack-care-forum-einstieg.md` | Forum-Kurztext |
| `iobroker-stack-care-forum-master-checkliste.md` | Deine Pre-Post-Liste |
| Diese Datei | Repo-Go-Live |

**Nicht zwingend ins öffentliche Repo:** interne Notizen, die du nur lokal brauchst.

---

## D. Bewusst später / Backlog (im Konzept, nicht „vergessen“)

| Thema | Wo |
|--------|-----|
| Englische Kurzfassung (RFC international) | Backlog |
| Manifest-Signatur, APIs, ENV-Namen final | P0 mit Core/Image |
| Sidecars, Multi-Host, NFS-Backup | Konzept §13.2, §15 |
| Synology/Windows-Native-Profil v1 | bewusst Best effort |
| P1 **Erfolgskriterium** schriftlich festhalten (z. B. „1 Image-Release mit Stack-Absatz + 3 Tester“) | noch ergänzen wenn Pilot startet |
| Koordinator-Rolle fürs Repo (du vs. suchen) | Forum/GitHub-About |

---

## E. Forum ↔ GitHub Workflow (dein Plan)

1. Repo veröffentlichen (README + Ausarbeitung prominent).  
2. Forum: Problem + 3–4 Fragen + **Link Repo** (kein Roman).  
3. Feedback: **Issues/Discussions** für konkrete Änderungen; Forum für Stimmung & Maintainer-Stimmen.  
4. Änderungen: Commit im Repo, im Forum optional „Stand aktualisiert“.  
5. Privat-Mail buanet/Core: **wann du willst**, unabhängig vom Repo – siehe Ausarbeitung FAQ.

---

*Interne Arbeitsliste – kann mit ins Repo als Transparency.*