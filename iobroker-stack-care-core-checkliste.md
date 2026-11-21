# Stack Care – Checkliste Gespräch mit Core / Admin (P2-Vorbereitung)

**Zweck:** Punkte aus dem Konzept, die **vor** Umsetzung in js-controller/Admin geklärt werden sollten.  
**Bezug:** `iobroker-stack-care-konzept.md` § 12–13.  
**Kontext:** P1 (nur Release + Doku) kann ohne Core starten; diese Liste betrifft **P2+**.

---

## A. Scope und Versprechen

- [ ] **Nutzerversprechen:** nur `wartungsarm` auf **offiziellem Profil** (Docker zuerst) – kein SLA?
- [ ] **Supported vs. Best effort:** wer entscheidet die Erkennungsregeln (Marker, Manifest)?
- [ ] **Kanäle:** `stable` vs. `beta` – eigenes Manifest, weniger Automatik für beta?

---

## B. Architektur-Entscheidungen

- [ ] **Umsetzung UI:** Admin-Erweiterung **bevorzugt** vs. separater Adapter?
- [ ] **Manifest-Hosting:** URL, HTTPS, wer publiziert, Update-Frequenz?
- [ ] **Stack-Marker:** ENV im Image, Datei `/opt/iobroker/.stack`, beides – wer setzt bei Install/Upgrade?
- [ ] **Manifest-Abgleich:** nur lesen (P2) oder auch **Runbook** ausführen (P3)?

---

## C. js-controller

- [ ] **Host-States** `system.host.<host>.stack.*` (Profil, Kanal, letzte Manifest-ID, Abweichung ja/nein)?
- [ ] **Host-Messages:** `getStackStatus`, optional `compareManifest` – oder alles nur in Admin?
- [ ] **Erstsetup:** Defaults `adapterAutoUpgrade` (live, minor) setzen – Wizard vs. silent bei Stack-Marker?
- [ ] **Multi-Host:** Stack-Status **pro Host** oder nur Master; Slaves ohne Docker?
- [ ] **Notifications:** neue Kategorie `stackMaintenance` im system-Scope?
- [ ] **Offline:** kein Manifest erreichbar → letzte gecachte Empfehlung vs. „unbekannt“?

---

## D. Admin

- [ ] Neuer Tab **„Systempflege“** vs. Ausbau **Host** – Platz, Rechte, Expertenmodus?
- [ ] **Vier Zustände** – feste Texte DE/EN, Verlinkung profilspezifische Doku?
- [ ] **BackItUp-Gate (P3):** welcher State/API = „Backup ok“; max. Alter (Tage)?
- [ ] **Block „Außerhalb Stack“:** Zähler GitHub/beta-Adapter – Datenquelle (Instanz-Objekte, Install-Quelle)?
- [ ] **Expertenmodus:** Hinweis wenn User Policies/Marker überschreibt?

---

## E. BackItUp (Adapter-Team)

- [ ] Stabiler, lesbarer **Zeitstempel letztes erfolgreiches Backup** für Gate?
- [ ] Restore als **offizieller Rollback** im Runbook verlinkt?

---

## F. Image-Pflege (buanet, parallel zu Core)

- [ ] Pro Image-Release: Manifest-Zeile oder verlinkte JSON – **Mitwirkung ja/nein**?
- [ ] ENV Stack-Marker im Container – Abstimmung mit Admin-Erkennung?

---

## G. Randthemen (nicht vergessen)

| Thema | Frage an Core |
|--------|----------------|
| **Native Adapter / Node** | Runbook-Hinweis „Adapter neu installieren“ – reicht Doku oder Schritt im Wizard? |
| **Sidecars** (Z2M, Influx, …) | Explizit **nicht** im Stack – Hinweis in UI ja/nein? |
| **EOL** | `min_supported_stack` im Manifest – Admin-Warnung ab wann? |
| **Wartungsfenster** | geplanter Start (Zeitzone) – Core oder nur Admin-UI? |
| **Proxy** | Manifest-Fetch wie Repo-Updates (Proxy-Settings)? |
| **Recht** | Standard-Hinweis „keine Garantie“ im UI? |

---

## H. Phasen und Aufwand (Erwartung abstimmen)

| Phase | Core-Aufwand (grob) | Commitment |
|-------|---------------------|------------|
| **P1** | 0 Code | Doku-Link optional |
| **P2** | Admin read-only + fetch, optional Host-States | **Ja/Nein, wer owned?** |
| **P3** | Gate + Runbook | nach P2-Erfolg |

- [ ] **P2 als GitHub-Issue** gewünscht – mit Verweis auf RFC/Forum-Thread?
- [ ] **Tester:** 3–5 Freiwillige pro Stack-Release – Core nutzt Feedback wie bei Beta?

---

## I. Bewusst nicht an Core (v1)

Vollauto Docker-Pull, KI-Autopilot, Synology/Windows-Profil, Pflege externer Container, Vereins-Vollzeit.

---

## J. Minimale Ergebnisse aus dem Gespräch (Definition of Done)

1. **Ja/Nein** zu P2 in Roadmap (auch wenn „später“).  
2. **Owner** Admin vs. Controller für Manifest/Marker.  
3. **P1** ohne Code: Image + Foren-„Stack“ – Core verlinkt oder nicht.  
4. **Top 3** aus Abschnitt G, die Core in v1 UI berücksichtigen will.

---

*Einzelseite zum Mitnehmen ins Dev-Meeting oder als GitHub-Discussion-Body.*