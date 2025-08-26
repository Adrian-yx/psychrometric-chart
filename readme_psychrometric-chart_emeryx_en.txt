Psychrometric Chart by EMERYX
README (quick use + theory)

==================================================
1) WHAT IS IT?
==================================================
Psychrometric calculation and plotting app to create air STATES, chain PROCESS steps (heating, cooling, mixing, heat recovery, humidification) and export results (PDF, CSV and JSON). Built for agile, verifiable technical work.

Requirements: open the HTML file in a modern browser (Chrome recommended). No server needed.

==================================================
2) HOW TO OPEN
==================================================
1. Save the file: psychro_mvp_desktop_v2f.html
2. Open it by double‑click or drag it into a browser tab.
3. You’ll see a top bar (RH curves toggle) and two areas:
   - Left: control cards (“Conditions”, “States”, “Processes”, “Project”).
   - Right: interactive psychrometric chart.

==================================================
3) 3‑STEP WORKFLOW
==================================================
1) Conditions
   • Set Altitude (m) and/or Pressure (kPa).
     - If you enter Pressure, it overrides Altitude.
   • Set Airflow (m³/h). This flow is used in processes for power (kW), moisture (L/h), etc.
2) States
   • In “+ New state”: enter Name, DBT (°C) and RH φ (%). Add.
   • Points are plotted with their name. On hover you see all parameters.
   • You can delete states from the “States” card.
3) Processes
   • In “+ New process”: choose Type, select the Input, set parameters and type the Output name.
   • Press “Apply”: the new state is created and the process line is drawn on the chart.
   • Each process shows in its card with ΔT, Δx, Δh, ṁ_moisture, Total Q and Sensible Q.

4) Export (card “Project”)
   • Save JSON: saves the “project” to reopen later.
   • Export CSV: human‑readable table (state/process names + magnitudes).
   • Export JSON (data): report‑style JSON with derived states and processes.
   • Export PDF: 3 clean pages (1: chart, 2: states table, 3: processes table).

==================================================
4) CARDS (WHAT EACH ONE DOES)
==================================================
CONDITIONS
• Altitude (m) → converted to standard barometric pressure if P not provided.
• Pressure (kPa) → when filled, it takes precedence over Altitude.
• Airflow (m³/h) → used to convert specific differences to kW and L/h.

+ NEW STATE
• Enter Name, DBT (°C) and RH φ (%). The name must be unique.
• Automatically computed: x (g/kg_da), h (kJ/kg_da), WBT (°C), ρ (kg/m³).

+ NEW PROCESS
• Type: Heat, Cool, Mix, Heat Recovery, Humidify.
• Input: select the starting state.
• Parameters: vary by Type (see section 5).
• Output: name of the resulting state.
• Apply: creates the state and adds the process.

STATES
• List of states with their computed magnitudes. “Delete” button per state.

PROCESSES
• List of processes with:
  Process_#, Type (badge), connections (by name), and metrics:
  ΔT (°C), Δx (g/kg_da), Δh (kJ/kg_da), ṁ_moisture (L/h), Total Q (kW), Sensible Q (kW).

PROJECT
• Save/Load project JSON, Export CSV, Export JSON (data) and Export PDF.

==================================================
5) PSYCHROMETRIC CHART (WHAT YOU SEE)
==================================================
• X‑axis: DBT (°C)
• Y‑axis: x (g/kg_da)
• Background curves: relative humidity (10–100%) + labels 10/30/50/70/90.
• Diagonal dotted lines: enthalpy (h, kJ/kg_da).
• Blue points: states (fixed label = name; tooltip = T, φ, x, h, WBT, ρ).
• Process lines:
  - Mixing: dark green; grey helper for the “new” flow + grey dashed line.
  - Heat recovery (HRV/ERV): orange; grey helper for the secondary + grey dashed line.
  - Cooling with condensation: grey helper marks the ADP + grey dashed line.
• Click on the chart: selects the nearest state (useful for edit/delete).

==================================================
6) PROCESSES & THEORY (OPERATIONAL SUMMARY)
==================================================
Common notes
• P = pressure in kPa (from Altitude or Pressure field).
• Flow → ṁ ≈ (m³/h / 3600) · ρ (kg/s) of the input state.
• In all balances, x is in kg/kg_da (displayed as g/kg_da).

HEAT (heat)
• Modes (“Description”):
  1) Power (Q): add power (kW). It becomes Δh = Q/ṁ → h2 = h1 + Δh and T2 from h–w.
  2) ΔT (dT): T2 = T1 + ΔT; w2 = w1 (sensible heating).
  3) Δh (dH): h2 = h1 + Δh; T2 from h–w.
  4) to T (Ttarget): temperature setpoint; w constant.
  5) Wall T (Twall, η): exchange toward the wall → T2 = T1 + η·(Twall − T1).
  6) Fluid mean (Tin, Tout, η): uses fluid mean T and η → T2 = T1 + η·(Tmean − T1).
• Humidity: no mass added/removed → w constant.

COOL (cool)
• Same descriptions as Heat (Q, ΔT, Δh, to T, wall, fluid) plus “Coil efficiency (%)” (η_total).
• First compute a “sensible” point (same w). If RH>100% there:
  – Condensation model ADP + BF is activated:
    • ADP ≈ Tsensible − ΔADP (tunable or linked to wall/water).
    • BF ~ 1 − η_total (you can override it).
    • Final state = mix: h2 = BF·h1 + (1−BF)·h_ADP, w2 = BF·w1 + (1−BF)·w_ADP.
• Outputs: Total Q (kW) = ṁ·(h2 − h1), Sensible Q (kW) = ṁ·(h(T2, w1) − h1), ṁ_water (L/h) = 3600·(w2 − w1)·ṁ.

HUMIDIFY (hum_adiab ↔ hum_isot)
• Type = Adiabatic (constant h) or Isothermal (constant T).
• Description:
  1) DX (Δx): direct increase of x (clamped to avoid supersaturation).
  2) to RH%: target φ; solves T and w by type (adiabatic: hold h; isothermal: hold T).
  3) Water flow (L/h): converts ṁ_water to Δx = (ṁ_water / 3600)/ṁ (with saturation clamp).
• The system limits to φ ≤ 100% (saturation).

HEAT RECOVERY (HRV/ERV)
• Parameters: Secondary T, Secondary RH, sensible efficiency (ηS), latent recovery (ηL).
• Formulae:
  – T2 = T1 + ηS·(Tsec − T1)
  – w2 = w1 (HRV) or w1 + ηL·(wsec − w1) (ERV)

MIX (mix)
• Two variants implemented:
  – Mix two existing states with fraction fA.
  – Mix an existing state with a “new flow” (Tn, RHn, q_new and % of the existing flow you use).
• Mass and energy balances:
  – h_mix = fE·h_exist + fN·h_new  ;  w_mix = fE·w_exist + fN·w_new (with fE proportional to the ṁ used).

==================================================
7) EXPORTS
==================================================
• CSV (readable): names (not IDs), full states and processes with: ΔT, Δx, Δh, ṁ_moisture (L/h), Total Q (kW), Sensible Q (kW).
• JSON (data): “report” structure with names and derived magnitudes (ideal for post‑processing).
• PDF: 3 pages → 1) Chart, 2) States, 3) Processes.

==================================================
8) VALIDATION & LIMITS
==================================================
• φ between 0 and 100%. The system clamps if needed.
• For processes using flow, first set Airflow (m³/h) in Conditions.
• Mixing with “% of existing flow for mixing” accepts 0–100% (inclusive).
• Saturation: the system avoids φ > 100%; with cooling, condensation is calculated automatically.
• Unique state names (warning on duplicates).

==================================================
9) PRACTICAL TIPS
==================================================
• Toggle RH curves in the top bar to better visualise processes.
• Use “to T” or “to RH%” to set direct targets and validate projects.
• Export PDF for documentation (clean chart + tables). CSV/JSON for calculations and traceability.
• Tune ΔADP and BF when you want more realistic coil predictions.

==================================================
10) QUICK GLOSSARY
==================================================
DBT: dry‑bulb temperature (°C)
WBT: wet‑bulb temperature (°C)
φ: relative humidity (%)
x: humidity ratio (kg/kg_da) [shown as g/kg_da on screen]
h: specific enthalpy (kJ/kg_da)
ρ: humid‑air density (kg/m³)
ADP: Apparatus Dew Point (coil dew point)
BF: Bypass Factor (fraction that “doesn’t touch” the ADP)
η_total: overall coil efficiency (sensible + approach)

End.
