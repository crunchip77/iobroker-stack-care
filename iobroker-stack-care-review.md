# Stack Care – Review: Sinn, Machbarkeit, Aufwand (inkl. Image-Historie)

**Stand:** November 2026  
**Bezug:** `iobroker-stack-care-konzept.md`, Forum-Entwurf  
**Zweck:** Gesamtüberblick über die Gedankengänge – nicht erneut technische Spezifikation.

---

## 1. Kurzfassung (Executive Summary)

| Kriterium | Bewertung | Kommentar |
|-----------|-----------|-----------|
| **Problem echt?** | **Ja, stark** | Forum, Umsteiger, mehrschichtige Pflege – Kernthema trifft |
| **Richtung sinnvoll?** | **Ja, bedingt** | Nur mit **enge** Supported-Matrix und **klarer Governance** |
| **Neuheit?** | **Gering bis mittel** | Image/Docker/Auto-Upgrade gibt es; **neu** wäre Produkt-Schicht (Manifest + Admin + Abmachung) |
| **Machbarkeit technisch** | **Mittel–hoch (schrittweise)** | Viel baut auf js-controller 6/7 + Image + BackItUp auf |
| **Machbarkeit organisatorisch** | **Der Engpass** | Historisch scheiterte Image-Pflege oft an **„wer wartet?“**, nicht an Technik |
| **Aufwand Minimal (P1)** | **Niedrig–mittel** | Release-Empfehlung + Doku + Tags, kaum Core-Code |
| **Aufwand Vollvision** | **Hoch, dauerhaft** | Admin-UI, Manifest-CI, zwei Profile, Assistent, Governance |
| **KI** | **Optional, Phase 2+** | Erklärung ja; Autopilot nein |
| **Empfehlung** | **RFC → Minimal-Pilot mit buanet/Core** | Ohne deren Commit: Konzept als Community-Papier, kein „Versprechen“ |

---

## 2. Was wir in der Diskussion zusammengetragen haben

### 2.1 Problem (unverändert zentral)

- Nutzer wollen **Automation**, nicht **Node/OS/Reihenfolge-Pflege**.
- Mehrschichtigkeit (Basis, Node, Controller, Adapter, ggf. Image am Host).
- Folgen: Aufschieben, Wild-Update, Forum-Stau, Abwanderung.

### 2.2 Lösungsidee (kein „zweites Docker“)

- **Stack Care** = Betriebsmodell: versionierte **Stack-Releases**, **eine Admin-Wartungssicht**, **Backup-Gate**, **Supported vs. Best effort**.
- Docker-Image ist oft **Hülle**, nicht die ganze Lösung.
- Parallel denkbar: **Debian-Installer-Profil** im selben Manifest (ohne buanet, mit Core/Community).
- Adapter: **live** patch/minor auto; beta/GitHub **außerhalb** des Stable-Versprechens.
- KI: **Dolmetscher**, kein root.

### 2.3 Rollen

| Rolle | Pflicht für was |
|--------|------------------|
| **buanet / Image** | Docker-Stack-Basis, Tags, Release = Stack-Referenz |
| **Core** | Admin/Controller, Policies, Host-Integration, Debian-Profil |
| **Community** | Test, Forum-RFC, Übersetzung |
| **User** | Backup, Bestätigung bei Major, kein `latest` |

---

## 3. Geschichte: Images und „wer wartet?“

Das ist **kein neues Problem** – es erklärt, warum Vorsicht angebracht ist.

### 3.1 Alte SBC-/Website-Images

- Früher manuell gebaute Images für viele Boards; in Dev-Meeting: lieber **keine** veralteten Images als **hoffnungslos veraltete** (Aufwand, Pflege) – Einstieg über **vereinfachten Installer** (Forum-Diskussion zu Raspberry Image v2, [Topic 70852](https://forum.iobroker.net/topic/70852/iobroker-raspberry-os-image-v2)).

### 3.2 buanet Raspberry Pi OS Image (2021–2024/25)

- Automatisierte Builds, Ziel: aktuelles SBC-Image ([Doku](https://docs.buanet.de/iobroker-raspberrypios-image/)).
- v2 Beta 2023, Community-Test, Ziel Pi-Imager.
- **Einstellung der Weiterentwicklung** (André/buanet, persönliche Kapazität, vermisster Fortschritt bei Onboarding/Wartung vs. Wettbewerb) – Angebot an Nachfolger für Build+Hosting, sonst: **offizielles Raspberry Pi OS + manuelle Installation** ([Forum, Seite 3](https://forum.iobroker.net/topic/70852/iobroker-raspberry-os-image-v2?page=3)).

### 3.3 Docker-Image (heute)

- **Weiter aktiv** (Major-Releases, z. B. v12), community- und doku-seitig **de facto Standard** für viele.
- Löst **Basis-Bündelung**, löst **nicht**:
  - User-Pflege-Gefühl (Tags, Host-Recreate, Major),
  - eine **eine** Antwort in Admin,
  - gebündelte **Produkt**-Freigabe „Stack stable“.

### 3.4 Lektion für Stack Care

| Lessons Learned | Konsequenz |
|-----------------|------------|
| Image ohne **Nachfolger + Prozess** stirbt | Governance **vor** Marketing |
| Ein Maintainer macht irgendwann **Schluss** | **Institution** (Core+Image+Release-Rhythmus), nicht Held |
| Installer ≠ wartungsarmes Produkt | Image/Installer + **Manifest + UX** |
| „Nur Doku“ reicht nicht | Aber **Minimalpilot** kann **nur** Tag+Doku+Ablauf sein |
| Wettbewerber: Onboarding + Wartung als Produkt | Stack Care adressiert genau die Lücke – **wenn** betreut |

**Stack Care ist in der Pflicht, die Image-Debatte zu beantworten:** Nicht noch ein Artifact, sondern **wer** jedes Quartal sagt „das ist stable“ und **was passiert**, wenn Image-Pflege fehlt (ehrlich degradieren auf Best effort).

---

## 4. Sinnhaftigkeit

### 4.1 Pro

- Trifft **Nutzer-Schmerz**, nicht Entwickler-Schönheit.
- Nutzt **vorhandene** Bausteine (Auto-Upgrade, Host, BackItUp, Docker-Releases).
- **Eng** gehalten (Supported vs. Rest) vermeidet unmögliche Testmatrix.
- **Forum-Narrativ** („Pflege-Last“) besser adressierbar als „noch ein Adapter“.
- Wettbewerbsfeld **Wartung/Onboarding** – strategisch relevant.

### 4.2 Contra / Risiken

- **Organisatorisch** ungelöst ohne buanet/Core – wird wieder **Einzelprojekt**.
- **Erwartungsmanagement:** „pflegefrei“ vs. „wartungsarm“ – bei Misserfolg mehr Frust.
- **Zwei Profile** (Docker + Debian) **verdoppelt** Testaufwand – Pilot daher mit **einem** Profil (das, für das zuerst eine Zusage kommt).
- **Host-Grenze** (Unraid, Synology): Assistent ohne Host-Helfer bleibt **geführt**, nicht magisch.
- **GitHub/beta-Nutzer** fühlen sich „zweite Klasse“ – UI muss das **ehrlich** zeigen.
- **KI-Hype:** Ablenkung, wenn Regelwerk noch fehlt.

### 4.3 Gesamturteil Sinn

**Sinnvoll als gemeinsames Produktversprechen + schrittweise Umsetzung.**  
**Nicht sinnvoll** als großer Wurf ohne Maintainer-Vertrag und ohne Minimalerfolg (P1).

---

## 5. Machbarkeit

### 5.1 Technisch (bereits da / nah)

| Baustein | Reife |
|----------|--------|
| Adapter Auto-Upgrade (6+) | vorhanden |
| Notifications, Host-Messages | vorhanden |
| Admin Host-Tab, Controller-Update | vorhanden |
| BackItUp | vorhanden |
| Docker-Image mit Tags, maintenance | vorhanden |
| `upgradeOsPackages` (Linux) | vorhanden, begrenzt |

### 5.2 Neu / aufwendig

| Baustein | Aufwand |
|----------|---------|
| Manifest-Format + Publikation | mittel (einmal Schema, pflege pro Release) |
| Admin „Systempflege“ + Manifest-Abgleich | **hoch** (Core/Admin) |
| Stack-Marker (Erkennung Supported) | mittel |
| BackItUp-Gate (API) | mittel |
| Geführter Image-Upgrade (Host) | hoch (Plattformen heterogen) |
| Debian-Profil + CI | hoch |
| KI-Erklärungsschicht | optional, mittel |

### 5.3 Organisatorisch

| Bedingung | Ohne | Mit |
|-----------|------|-----|
| buanet benennt Stack pro Image-Release | Docker-Stack nur Theorie | Minimal pilotierbar |
| Core nimmt 1 Admin-Ansicht in Roadmap | bleibt Forum+Doku | echtes Produkt |
| 3–5 Tester pro Release | Blindflug | wie Beta-Adapter heute |

**Fazit Machbarkeit:** Technisch **machbar in Wellen**; **kritisch** ist **verbindliche Mitwirkung** (wie bei jedem Image-Projekt).

---

## 6. Aufwandsschätzung (grobe Größenordnung)

Annahme: Freiwillige + bestehende Maintainer, keine Vollzeit-Stelle.

| Phase | Inhalt | Aufwand (einmalig) | Laufend / Release |
|-------|--------|---------------------|-------------------|
| **P0 RFC** | Forum, Abstimmung, Manifest-Schema | 1–2 Personen × wenige Wochen | – |
| **P1 Minimal** | Feste Tags, eine Upgrade-Story, Release-Note „Stack“, keine Admin-UI | gering, **buanet+1** | **1–3 Tage** pro Image-Major, wenige Stunden pro Minor |
| **P2 Admin read-only** | Ist/Soll, Backup-Alter, 4 Zustände | **Core:** Wochen–Monate dev | Pflege Texte |
| **P3 Innen-Auto + Assistent Controller** | Defaults, Gates | Core + Konzept | gering |
| **P4 Image-Assistent Host** | Unraid/Compose-Doku, optional Agent | Community hoch | Support |
| **P5 Debian-Profil** | Zweite CI-Matrix | Community + Core | **+50–100 %** Release-Aufwand |
| **KI optional** | Opt-in Text | Adapter-Community | Betrieb/Kosten User |

**Realistischer Einstieg mit kleinstem ROI:** **P1** – kostet am wenigsten, prüft ob **Zusammenarbeit** funktioniert (Lesson Raspberry-Image).

**Vollvision:** **dauerhaft** mehrere Rollen; vergleichbar mit **Image-Pflege heute**, plus **Core-UI** – nicht Nebenprojekt für einen Abend.

---

## 7. „Wer wartet das?“ – konkrete Antwortmodelle

### 7.1 Was **nicht** wieder passieren darf

- Ein **buanet/André**-Modell: Jahre Engagement, dann Stopp ohne Nachfolge.
- Behauptung „offiziell wartungsarm“ ohne **Release-Rhythmus**.

### 7.2 Modelle (absteigend Minimalismus)

1. **Release-Koordination (Minimal)**  
   - **Wer:** Image-Maintainer schreibt Release; Core bestätigt js-controller-Min; Forenpost „empfohlen“.  
   - **Kein** neues UI.  
   - **Wartet:** 2 Parteien, Stunden/Release.

2. **Stack-Manifest + Admin read-only (Mittel)**  
   - **Wer:** wie oben + Core pflegt Admin-Anzeige; Community testet.  
   - **Wartet:** kleines **Release-Team** (namentlich), Quartalsrhythmus `stable`.

3. **Voll Stack Care**  
   - **Wer:** Verein/Core Arbeitskreis, Image, Tester, Doku.  
   - **Formalisierung** (wer darf `stable` setzen).

4. **Foundation-/Vereinsfinanzierung**  
   - Nur wenn Pflege wirklich Vollzeit nötig – derzeit **nicht** voraussetzen.

### 7.3 Exit / Degradation

- Image-Pflege stoppt → Stack Care **downgraden** auf Best effort, Manifest „eingefroren“, Admin Hinweis – **ehrlicher** als veraltetes Image (Lesson alte SBC-Images).

---

## 8. Abgleich der Gedankengänge (Checkliste)

| Thema | Konsistent? | Anmerkung |
|-------|-------------|-----------|
| User-first | ja | |
| Docker ≠ Lösung allein | ja | mit buanet abgestimmt |
| Nicht-Docker | ja | Phase 2, mehr Aufwand |
| Adapter live/beta/GitHub | ja | |
| KI | ja | optional, nicht Kern |
| Governance | **offen** | historisch größte Schwäche |
| Veraltete Versionsbeispiele | behoben | Manifest pro Release |

---

## 9. Empfehlung (für dich / Forum)

1. **Forum:** Problem + Frage Governance, **nicht** Vollvision verkaufen.  
2. **Gespräch buanet:** „Würdet ihr pro Image-Release einen **Stack-Absatz** + festes Tag mittragen?“ – **Ja/Nein** entscheidet Projekt.  
3. **Gespräch Core:** „Reicht P1 ohne Code?“ / „Wer owned P2?“  
4. **Raspberry-Lesson** explizit nennen: Nachfolge und Rhythmus **vor** großem Versprechen.  
5. **Zweites Profil** (Docker bzw. Debian-Installer) erst nach **erfolgreichem** P1 im ersten.  
6. **KI** im Forum nur als „später, Erklärung“.

---

## 10. Ein Absatz für Skeptiker im Forum

Es gab schon Images – vom manuell veralteten SBC-Image über das eingestellte Raspberry-Image bis zum heutigen Docker. Das Problem war selten die Technik, sondern **wer dauerhaft freigibt und nachzieht**. Stack Care schlägt vor, **nicht noch ein Artifact**, sondern **Release + eine Admin-Antwort + klare Zuständigkeit** zu koppeln – und klein anzufangen, damit es nicht wieder an einer Person hängt. Ohne Mitmachen von Image-Pflege und Core bleibt es eine Analyse mit Recht; **mit** Mitmachen wäre es der fehlende Onboarding-/Wartungslayer, den Wettbewerber schon als Produkt verkaufen.

---

*Verweise: [Raspberry Image Forum](https://forum.iobroker.net/topic/70852/iobroker-raspberry-os-image-v2), [Raspberry Image Doku](https://docs.buanet.de/iobroker-raspberrypios-image/), [Docker-Doku](https://docs.buanet.de/iobroker-docker-image/docs/), [js-controller Auto-Upgrade](https://github.com/ioBroker/ioBroker.js-controller).*

---

## 11. Review Runde 2 (kurz)

**Vollständige Konstellationen-Matrix:** `iobroker-stack-care-konzept.md` § 15.

| Bereich | Neu bedacht? |
|---------|----------------|
| Docker auf NAS/Unraid/LXC, NFS-Volumes, Watchtower | § 15.1 |
| Multi-Host, große DB, History/Redis, nur Telegram | § 15.2 |
| Offline, Firewall ohne iobroker.net, Stromausfall | § 15.3 |
| Migration Synology→Docker, Cold Standby | § 15.4 |
| Abgrenzung `iob fix` / `nodejs-update` | § 14 |

**Urteil:** Für **Forum + P1** ist der Satz Dokumente **vollständig genug**. Für **Produktbau** bleibt Backlog (Gates, Hosts, Sidecars) – bewusst nicht alles in v1.

**Drei Top-Risiken v1:** (1) Watchtower/`latest` parallel zum Stack, (2) Backup-Gate zu weich oder zu hart bei NFS, (3) Docker-Nutzer führt trotzdem `iob nodejs-update` auf dem Host aus.