Psychrometric Chart by EMERYX
README (Schnellstart + Theorie)

==================================================
1) WAS IST DAS?
==================================================
Psychrometrische Berechnungs‑ und Plot‑App zum Erstellen von LUFTZUSTÄNDEN, zum Verketten von PROZESSEN (Heizen, Kühlen, Mischen, Wärmerückgewinnung, Befeuchten) und zum Export der Ergebnisse (PDF, CSV und JSON). Für agile und überprüfbare technische Arbeit konzipiert.

Voraussetzungen: HTML‑Datei in einem modernen Browser öffnen (Chrome empfohlen). Kein Server nötig.

==================================================
2) ÖFFNEN
==================================================
1. Datei speichern: psychro_mvp_desktop_v2f.html
2. Mit Doppelklick öffnen oder in einen Browser‑Tab ziehen.
3. Sie sehen eine obere Leiste (Umschalter für rF‑Kurven) und zwei Bereiche:
   - Links: Steuerkarten („Bedingungen“, „Zustände“, „Prozesse“, „Projekt“).
   - Rechts: interaktives psychrometrisches Diagramm.

==================================================
3) ARBEITSABLAUF IN 3 SCHRITTEN
==================================================
1) Bedingungen
   • Höhe (m) und/oder Druck (kPa) festlegen.
     - Ein eingegebener Druck hat Vorrang vor der Höhe.
   • Luftvolumenstrom (m³/h) festlegen. Dieser wird in Prozessen für Leistung (kW), Feuchte (L/h) usw. verwendet.
2) Zustände
   • In „+ Neuer Zustand“: Name, Tbs (°C) und φ (%) eingeben. Hinzufügen.
   • Punkte werden mit ihrem Namen gezeichnet. Beim Überfahren erscheinen alle Parameter.
   • Zustände können in der Karte „Zustände“ gelöscht werden.
3) Prozesse
   • In „+ Neuer Prozess“: Typ wählen, Eingangszustand auswählen, Parameter setzen und den Namen des Ausgangszustands eingeben.
   • „Übernehmen“ drücken: Der neue Zustand wird erstellt und die Prozesslinie im Diagramm gezeichnet.
   • Jede Prozesskarte zeigt ΔT, Δx, Δh, ṁ_Feuchte, Gesamt‑Q und Sensibel‑Q.

4) Export (Karte „Projekt“)
   • JSON speichern: speichert das „Projekt“ zum späteren Öffnen.
   • CSV exportieren: gut lesbare Tabelle (Namen der Zustände/Prozesse + Größen).
   • JSON (Daten) exportieren: Bericht‑JSON mit abgeleiteten Zuständen und Prozessen.
   • PDF exportieren: 3 klare Seiten (1: Diagramm, 2: Zustands‑Tabelle, 3: Prozess‑Tabelle).

==================================================
4) KARTEN (FUNKTIONEN)
==================================================
BEDINGUNGEN
• Höhe (m) → wird in Standardluftdruck umgerechnet, wenn P nicht angegeben ist.
• Druck (kPa) → hat Vorrang vor der Höhe.
• Luftvolumenstrom (m³/h) → zur Umrechnung spezifischer Differenzen in kW und L/h.

+ NEUER ZUSTAND
• Name, Tbs (°C) und φ (%) eingeben. Der Name muss eindeutig sein.
• Automatisch berechnet: x (g/kg_da), h (kJ/kg_da), Twb (°C), ρ (kg/m³).

+ NEUER PROZESS
• Typ: Heizen, Kühlen, Mischen, Wärmerückgewinnung, Befeuchten.
• Eingang: Startzustand wählen.
• Parameter: je nach Typ (siehe Abschnitt 5).
• Ausgang: Name des resultierenden Zustands.
• Übernehmen: erstellt den Zustand und fügt den Prozess hinzu.

ZUSTÄNDE
• Liste der Zustände mit berechneten Größen. „Löschen“ pro Zustand.

PROZESSE
• Liste der Prozesse mit:
  Prozess_#, Typ (Badge), Verknüpfungen (per Name) und Kennwerten:
  ΔT (°C), Δx (g/kg_da), Δh (kJ/kg_da), ṁ_Feuchte (L/h), Gesamt‑Q (kW), Sensibel‑Q (kW).

PROJEKT
• Projekt‑JSON speichern/laden, CSV exportieren, JSON (Daten) exportieren und PDF exportieren.

==================================================
5) PSYCHROMETRISCHES DIAGRAMM (ANZEIGE)
==================================================
• X‑Achse: Tbs (°C)
• Y‑Achse: x (g/kg_da)
• Hintergrundkurven: relative Feuchte (10–100 %) + Labels 10/30/50/70/90.
• Diagonale Punktlinien: Enthalpie (h, kJ/kg_da).
• Blaue Punkte: Zustände (feste Beschriftung = Name; Tooltip = T, φ, x, h, Twb, ρ).
• Prozesslinien:
  - Mischen: dunkelgrün; grauer Helfer für den „neuen“ Volumenstrom + graue gestrichelte Linie.
  - Wärmerückgewinnung (HRV/ERV): orange; grauer Helfer für den Sekundärstrom + graue gestrichelte Linie.
  - Kühlen mit Kondensation: grauer Helfer markiert den ADP + graue gestrichelte Linie.
• Klick ins Diagramm: wählt den nächsten Zustand (nützlich zum Bearbeiten/Löschen).

==================================================
6) PROZESSE & THEORIE (BETRIEBLICHE ZUSAMMENFASSUNG)
==================================================
Gemeinsame Hinweise
• P = Druck in kPa (aus Höhe oder Druckfeld).
• Volumenstrom → ṁ ≈ (m³/h / 3600) · ρ (kg/s) des Eingangszustands.
• In allen Bilanzen ist x in kg/kg_da (Anzeige in g/kg_da).

HEIZEN (heat)
• Modi („Beschreibung“):
  1) Leistung (Q): Leistung (kW) hinzufügen. Daraus folgt Δh = Q/ṁ → h2 = h1 + Δh und T2 aus h–w.
  2) ΔT (dT): T2 = T1 + ΔT; w2 = w1 (fühlbares Heizen).
  3) Δh (dH): h2 = h1 + Δh; T2 aus h–w.
  4) auf T (Ttarget): Temperatursollwert; w konstant.
  5) Wand‑T (Twall, η): Austausch zur Wand → T2 = T1 + η·(Twall − T1).
  6) Fluid‑Mittel (Tin, Tout, η): verwendet mittlere Fluidtemperatur und η → T2 = T1 + η·(Tmittel − T1).
• Feuchte: keine Wasserzugabe/‑entnahme → w konstant.

KÜHLEN (cool)
• Gleiche Beschreibungen wie Heizen (Q, ΔT, Δh, auf T, Wand, Fluid) plus „Registerwirkungsgrad (%)“ (η_total).
• Zuerst wird ein „fühlbarer“ Punkt (gleiches w) berechnet. Wenn dort rF>100 %:
  – Kondensation mit ADP‑ + BF‑Modell:
    • ADP ≈ Tsensibel − ΔADP (anpassbar oder an Wand/Wasser gekoppelt).
    • BF ~ 1 − η_total (kann überschrieben werden).
    • Endzustand = Mischung: h2 = BF·h1 + (1−BF)·h_ADP, w2 = BF·w1 + (1−BF)·w_ADP.
• Ausgaben: Gesamt‑Q (kW) = ṁ·(h2 − h1), Sensibel‑Q (kW) = ṁ·(h(T2, w1) − h1), ṁ_Wasser (L/h) = 3600·(w2 − w1)·ṁ.

BEFEUCHTEN (hum_adiab ↔ hum_isot)
• Typ = Adiabatisch (h konstant) oder Isotherm (T konstant).
• Beschreibung:
  1) DX (Δx): direkter Anstieg von x (mit Begrenzung gegen Übersättigung).
  2) auf rF%: Ziel‑φ; löst T und w je nach Typ (adiabatisch: h konstant; isotherm: T konstant).
  3) Wasserstrom (L/h): wandelt ṁ_Wasser in Δx = (ṁ_Wasser / 3600)/ṁ um (mit Sättigungsbegrenzung).
• Das System begrenzt auf φ ≤ 100 % (Sättigung).

WÄRMERÜCKGEWINNUNG (HRV/ERV)
• Parameter: Sekundär‑T, Sekundär‑rF, fühlbarer Wirkungsgrad (ηS), latente Rückgewinnung (ηL).
• Formeln:
  – T2 = T1 + ηS·(Tsec − T1)
  – w2 = w1 (HRV) oder w1 + ηL·(wsec − w1) (ERV)

MISCHEN (mix)
• Zwei Varianten:
  – Zwei vorhandene Zustände mit Anteil fA mischen.
  – Einen vorhandenen Zustand mit einem „neuen Strom“ mischen (Tn, RHn, q_neu und % des genutzten bestehenden Stroms).
• Massen‑ und Energiebilanz:
  – h_mix = fE·h_exist + fN·h_neu  ;  w_mix = fE·w_exist + fN·w_neu (fE proportional zum verwendeten ṁ).

==================================================
7) EXPORTE
==================================================
• CSV (lesbar): Namen (keine IDs), vollständige Zustände und Prozesse mit: ΔT, Δx, Δh, ṁ_Feuchte (L/h), Gesamt‑Q (kW), Sensibel‑Q (kW).
• JSON (Daten): „Berichts“‑Struktur mit Namen und abgeleiteten Größen (ideal für die Weiterverarbeitung).
• PDF: 3 Seiten → 1) Diagramm, 2) Zustände, 3) Prozesse.

==================================================
8) VALIDIERUNGEN & GRENZEN
==================================================
• φ zwischen 0 und 100 %. System begrenzt bei Bedarf.
• Für prozessabhängige Ströme zuerst den Luftvolumenstrom (m³/h) unter Bedingungen setzen.
• Mischen mit „% des bestehenden Stroms für Mischung“ akzeptiert 0–100 % (inklusive).
• Sättigung: φ > 100 % wird vermieden; beim Kühlen wird Kondensation automatisch berechnet.
• Eindeutige Zustandsnamen (Warnung bei Duplikaten).

==================================================
9) PRAXISTIPPS
==================================================
• rF‑Kurven in der oberen Leiste ein‑/ausblenden, um Prozesse besser zu sehen.
• „auf T“ oder „auf rF%“ nutzen, um Ziele direkt zu setzen und Projekte zu validieren.
• PDF für Dokumentation (sauberes Diagramm + Tabellen). CSV/JSON für Berechnungen und Nachvollziehbarkeit.
• ΔADP und BF anpassen, um reale Register besser abzubilden.

==================================================
10) SCHNELLGLOSSAR
==================================================
Tbs: Trockenkugeltemperatur (°C)
Twb: Feuchtkugeltemperatur (°C)
φ: relative Feuchte (%)
x: Feuchteverhältnis (kg/kg_da) [Anzeige in g/kg_da]
h: spezifische Enthalpie (kJ/kg_da)
ρ: Dichte der feuchten Luft (kg/m³)
ADP: Apparatus‑Taupunkt (Register‑Taupunkt)
BF: Bypass‑Faktor (Anteil, der den ADP „umgeht“)
η_total: Gesamtwirkungsgrad des Registers (fühlbar + Annäherung)

Ende.
