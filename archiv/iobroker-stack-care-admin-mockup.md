# Admin „Systempflege“ – UI-Mockup (Entwurf, P2+)

**Zweck:** Zeigen, **was Nutzer in Phase P2 sehen könnten** – nicht heutiges Admin, sondern **Diskussionsmaterial** für Core und Forum.  
**Kein Pflichtteil für P0/P1:** In P1 reicht Release-Text im Image; **kein** neuer Tab nötig.

**Bezug:** Konzept §3 (vier Zustände), §9.8 (Adapter), `iobroker-stack-care-konzept.md` §11.1.

---

## Warum dieses Dokument?

| Frage | Antwort |
|--------|---------|
| Ist das „echtes“ Design? | **Nein** – Layout-Vorschlag; Farben/Icons orientieren sich lose an Admin, ohne Screenshot. |
| Braucht man das fürs Forum? | **Nein** – Ausarbeitung + Konzept reichen. |
| Wann hilft es? | Wenn jemand sagt: „Was meint ihr mit einer Admin-Ansicht?“ – oder wenn Core P2 plant. |

---

## Gemeinsame Seitenstruktur (P2 read-only)

```
┌─────────────────────────────────────────────────────────────────┐
│ ioBroker Admin                    [☰ Menü]                      │
├──────────┬──────────────────────────────────────────────────────┤
│ Adapter  │  Systempflege                                        │
│ Instanzen│  ─────────────────────────────────────────────────   │
│ Host     │  Dein Stack                                          │
│ BackItUp │  Profil: Docker (buanet) · Kanal: stable             │
│ ►System- │  Empfohlen: Stack docker-v12-2026.11-stable          │
│  pflege  │                                                      │
│          │  ┌ Status ( einer der vier Zustände unten ) ─────┐  │
│          │  └────────────────────────────────────────────────┘  │
│          │                                                      │
│          │  Details ▾                                           │
│          │  · Letztes Backup (BackItUp): …                      │
│          │  · Image-Tag / Controller / Adapter-Policy           │
│          │  · Link: Runbook für dieses Release                  │
│          │                                                      │
│          │  Außerhalb des Stack (falls zutreffend)              │
│          │  · Adapter von GitHub, beta-Kanal, …                 │
└──────────┴──────────────────────────────────────────────────────┘
```

**Warum ein eigener Tab?** Heute sind Pflege-Hinweise auf Host, Adapter und Notifications verteilt. Eine Seite bündelt: *Muss ich heute etwas tun?*

---

## Zustand 1 – Alles in Ordnung

```
│  ● Alles in Ordnung                                               │
│    Deine Installation entspricht dem empfohlenen Stack stable.    │
│    Du musst nichts tun.                                           │
│                                                                   │
│    [Experten: Versionen anzeigen]                                 │
```

**Warum wichtig:** Viele Nutzer öffnen Admin nur aus Angst. Ein klares „nichts tun“ reduziert Fehl-Updates.

---

## Zustand 2 – Unkritische Updates

```
│  ◐ Unkritische Updates verfügbar (8)                              │
│    Patch-/Minor-Adapter gemäß Stack-Policy – unkritisch.          │
│                                                                   │
│    [ Empfohlen: jetzt anwenden ]   [ Später erinnern ]            │
│    Hinweis: Auto-Upgrade js-controller kann das im Hintergrund. │
```

**Warum wichtig:** Trennt „Kosmetik/Minor“ von Majors – entlastet Forum und Nutzer-Entscheidung.

---

## Zustand 3 – Wartung mit Plan

```
│  ◆ Wartung empfohlen                                              │
│    Neuer Stack-Release: Image-Tag v12.0.1 (minor).                │
│    Reihenfolge: Backup → Wartung → Checkliste.                    │
│                                                                   │
│    Letztes Backup: vor 45 Tagen  ⚠ Backup zuerst empfohlen         │
│    [ Backup mit BackItUp ]   [ Assistent starten ]  (P3)          │
│    [ Runbook anzeigen (nur Anleitung, P2) ]                         │
```

**Warum wichtig:** Geführter Schritt statt verstreuter Doku; Backup-Gate erst ab P3, in P2 nur Warnung.

---

## Zustand 4 – Nicht im Supported Stack

```
│  ◇ Abweichende Installation (Best effort)                         │
│    Kein Stack-Marker oder Profil nicht freigegeben.               │
│    ioBroker funktioniert – ohne Versprechen „wartungsarm“.        │
│                                                                   │
│    Abweichungen: unbekanntes Image-Tag · Node ≠ Manifest (Profil) │
│    [ Hinweise zu deinem Profil ]   [ Was ist Supported? ]         │
```

**Warum wichtig:** Ehrlichkeit statt falsches Grün; Migration zum offiziellen Pfad bleibt freiwillig.

---

## Spezialfall – In Ordnung, aber Adapter ausgenommen

```
│  ● In Ordnung (2 Adapter ausgenommen)                             │
│    Stack stable aktiv; 2 Adapter von GitHub nicht im Prüfpfad.    │
│    [ Details ]  [ Auf live wechseln – Hinweis, kein Auto ]        │
```

---

## Was bewusst **nicht** im Mockup

- Wizard-Schritte, Fortschrittsbalken (P3)  
- Host recreate / Unraid-Klickpfad (P4, nur Links)  
- KI-Erklärfenster (optional später)  
- Sidecars (Z2M, Influx): nur Hinweis „nicht Teil des Stack“

---

*Skizze November 2026 – Diskussionsmaterial für P2 (Admin).*