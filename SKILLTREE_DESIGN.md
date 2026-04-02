# SKILLTREE_DESIGN.md — Ilzarah Station Defender

> Skill-Tree System — Design-Referenz
> Letzte Aktualisierung: Konzeptphase

---

## Grundprinzip

### Zwei Node-Typen

| Typ | Funktion | Limit |
|-----|----------|-------|
| **Kleiner Node** | Passive Boni, steigerbar | ♾️ Unendlich wiederholbar |
| **Großer Node** | Neue Einheit / Fähigkeit freischalten | Einmalig |

Kleine Nodes geben bei jeder Stufe einen kleinen Bonus — kontinuierliche EP-Investition über viele Runs. Kein hartes Cap, aber steigende Kosten pro Stufe (abnehmende Returns ab einem Schwellenwert sinnvoll).

Große Nodes sind einmalige Meilensteine — einmal gekauft, dauerhaft verfügbar in jedem Run.

---

## Darstellung (UI-Konzept)

**Konstellations-/Netz-Layout** — keine klassische Baum-Hierarchie.

- Große Nodes = leuchtende Kerne (größer, pulsierend)
- Kleine Nodes = umlaufende Satelliten-Punkte um die großen Nodes
- Verbindungslinien leuchten auf wenn Vorgänger freigeschaltet
- Gesperrt: dunkel, schemenhaft sichtbar aber nicht lesbar
- Freischaltbar: pulsierend (Amber-Glow)
- Freigeschaltet: voll beleuchtet, Stufenzahl sichtbar

```
Farbkodierung:
  Attack     = Rot / Orange
  Def        = Cyan / Blau
  Ressourcen = Grün / Gelb
```

```
        ●  ●
       /    \
  ●───◉ GROSS ◉───●
       \    /
        ●  ●

Kleine Nodes (●) als Satelliten um große Nodes (◉)
Verbindungslinie zeigt Freischaltungs-Voraussetzung
```

---

## ⚔️ ATTACK-TREE

### Kleine Nodes (unendlich steigerbar)

| Node | Effekt pro Stufe | Notiz |
|------|-----------------|-------|
| **Basisschaden** | +X% Schaden alle Türme | Gilt für Gun + Rocket |
| **Feuerrate** | +X% Angriffsgeschwindigkeit | Soft-Cap denkbar ab ~+200% |
| **Durchschuss-Chance** | +X% Chance auf Durchschuss | Trifft mehrere Ziele in einer Linie |
| **Reichweite** | +X% Turmreichweite | Global für alle Angriffs-Türme |

### Durchschuss → Kritischer Systemtreffer (Kaskaden-Mechanic)

```
Schuss trifft
    │
    ├── normaler Treffer (Basis)
    │
    └── [Durchschuss-Chance] → Durchschuss trifft mehrere Ziele
              │
              └── [Systemtreffer-Chance] → Kritischer Systemtreffer
                        │
                        ├── Antrieb ausschalten
                        │     → Gegner verlangsamt / gestoppt (X Sek.)
                        │
                        ├── Waffen ausschalten
                        │     → Gegner macht keinen Schaden (X Sek.)
                        │
                        └── Schild direkt zerstören
                              → Schild sofort weg, HP-Schaden beginnt
```

> Systemtreffer-Chance ist initial fix, später eigener kleiner Node (Phase 2).

---

### Große Nodes Attack (einmalig)

#### Waffen-Tiers — Freischaltungskette

```
[Gun1] ── immer verfügbar (Start)
   │
   └──► [Rocket Tower T1]    Splash-Damage, langsamer als Gun
              │
              └──► [Gun Tower T2]    Schnellfeuer, Piercing
                        │
                        └──► [Gun Tower T3]    Railgun, hoher Single-Target
                                  │
                                  └──► [Rocket Tower T2]    Clustermunition, Multi-Target
```

#### Angriffswinkel-Erweiterung

| Node | Winkel | Voraussetzung |
|------|--------|---------------|
| **Angriffsradius I** | 180° → 200° | Gun T2 freigeschaltet |
| **Angriffsradius II** | 200° → 225° | Angriffsradius I |
| **Angriffsradius III** | 225° → 250° (Max) | Angriffsradius II |

> Standard: Türme feuern in 180° Halbkreis zur Gegnerseite.
> Max-Skillung: 250° — fast Rundum-Feuer, nur kleiner toter Winkel hinter der Station.

#### Spezialangriff

| Node | Effekt |
|------|--------|
| **Hangar** | Schickt eigenes Kampfraumschiff los. Autonomes Bekämpfen für X Sekunden, dann Rückkehr. Raumschiff hat eigene HP — kann abgeschossen werden. Cooldown zwischen Einsätzen. |

---

## 🛡️ DEF-TREE

### Kleine Nodes (unendlich steigerbar)

| Node | Effekt pro Stufe | Notiz |
|------|-----------------|-------|
| **Hülle verstärken** | +X% Station-HP | Basisschutz |
| **Schild verstärken** | +X% Schild-Kapazität | Benötigt Schild-Generator gebaut |
| **Energie erhöhen** | +X% Energie-Pool | Energie versorgt Schild + aktive Systeme |
| **Notreserve** | +X% Chance: bei tödlichem Treffer 1 HP übrig | Einmal pro Welle auslösbar |

> Notreserve: Nicht stapelbar pro Welle — höhere Stufe erhöht die Auslöse-Chance, nicht die Häufigkeit.

---

### Große Nodes Def (einmalig)

| Node | Freischaltet | Details |
|------|-------------|---------|
| **Raketenabwehr** | Gun T1 + Rocket T1 können feindliche Raketen abschießen | Türme priorisieren Raketen automatisch wenn aktiv. Separater Toggle sinnvoll. |
| **Energy Core T2** | Erhöhter Energie-Pool + Regen | Voraussetzung für Schild T2 |
| **Schild T2** | Stärkerer Schild-Generator baubar | Benötigt Energy Core T2 |

#### Freischaltungskette Def

```
[Raketenabwehr]    [Energy Core T2]
                          │
                          └──► [Schild T2]
```

---

## ⚙️ RESSOURCEN-TREE

### Kleine Nodes (unendlich steigerbar)

| Node | Effekt pro Stufe | Wann |
|------|-----------------|------|
| **VSU bei Gegner-Abschuss** | +X% VSU pro eliminiertem Gegner | Direkt beim Tod |
| **VSU bei Wellenabschluss** | +X% VSU-Bonus am Wellenende | Bonus-Ausschüttung |
| **EP bei Gegner-Abschuss** | +X% EP pro eliminiertem Gegner (Meilenstein-Bonus) | Erhöht EP aus Kills |
| **EP bei Wellenabschluss** | +X% EP-Bonus am Wellen-Meilenstein | Erhöht Meilenstein-EP |
| **Reparatureffizienz** | +X% HP pro ausgegebenem VSU | Mehr Hülle für gleiche VSU-Menge |
| **Reparaturkosten** | -X% VSU-Grundkosten für Hüllenreparatur | Spart VSU für Upgrades in der Vorbereitungsphase |

> Reparatur funktioniert als einmaliger Button zwischen Wellen — kein Zeitfaktor.
> Beide Nodes zusammen ermöglichen verschiedene Spielstile: maximale HP-Rückgewinnung vs. VSU für Gebäude-Upgrades sparen.

> EP-Nodes machen Meilensteine wertvoller — Spieler der länger überlebt wird stärker belohnt.

---

### Große Nodes Ressourcen (einmalig)

| Node | Freischaltet | Details |
|------|-------------|---------|
| **Sammel-Drohne** | Drohne fliegt zu Gegner-Wracks | Nach Eliminierung kann ein Wrack entstehen (Chance-basiert). Drohne fliegt hin, sammelt Bonus-VSU. Ohne Drohne: kein Wrack-Loot. Mehrere Drohnen durch weitere Investition denkbar (Phase 2). |

#### Wrack-System (abhängig von Sammel-Drohne)

```
Gegner eliminiert
    │
    └── [Wrack-Chance %] → Wrack entsteht an Position
              │
              └── Sammel-Drohne fliegt los
                        │
                        ├── Drohne kommt an → VSU einsammeln → zurück
                        └── Drohne wird beschossen → Wrack verfällt nach X Sek.
```

---

## EP-Kosten Struktur (Richtwerte, anpassbar)

| Node-Typ | Kosten erste Stufe | Kostensteigerung pro Stufe |
|----------|-------------------|---------------------------|
| Kleiner Node | 10 EP | +5 EP pro Stufe (linear) oder +10% (exponentiell) |
| Großer Node (niedrig) | 50 EP | Einmalig |
| Großer Node (mittel) | 150 EP | Einmalig |
| Großer Node (hoch) | 300–500 EP | Einmalig |

> Kostenmodell muss durch Playtesting kalibriert werden — hier nur Ausgangswerte.

---

## Offene Fragen / Phase 2 Erweiterungen

| Thema | Idee |
|-------|------|
| Systemtreffer-Chance skalierbar? | Eigener kleiner Node in Phase 2 |
| Drohnen-Anzahl erhöhen | Weiterer großer Node: "Drohnen-Bay T2" → 2 Drohnen gleichzeitig |
| Weitere große Nodes Ressourcen | Recycling-Core (zerstörte eigene Gebäude → VSU) |
| Weitere große Nodes Def | Neue Station (mehr Bauplätze, dickere Hülle) |
| Weitere große Nodes Attack | Neue Station (zweiter Stationsring) |
| Waffenspezifische kleine Nodes | Separate Nodes nur für Gun / nur für Rocket statt global |

---

*Projekt: Ilzarah Station Defender | Engine: Godot 4 | Genre: Idle Autobattler Roguelite*
