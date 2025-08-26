Psychrometric Chart by EMERYX
README (uso rapido + teoria)

==================================================
1) CHE COS’È?
==================================================
Applicazione per il calcolo e il tracciamento psicrometrico per creare STATI dell’aria, concatenare PROCESSI (riscaldare, raffreddare, miscelare, recuperare calore, umidificare) ed esportare i risultati (PDF, CSV e JSON). Progettata per un lavoro tecnico agile e verificabile.

Requisiti: aprire il file HTML in un browser moderno (consigliato Chrome). Non serve alcun server.

==================================================
2) COME APRIRE
==================================================
1. Salvare il file: psychro_mvp_desktop_v2f.html
2. Aprirlo con doppio clic o trascinarlo in una scheda del browser.
3. Vedrete una barra superiore (switch delle curve di UR) e due aree:
   - Sinistra: schede di controllo (“Condizioni”, “Stati”, “Processi”, “Progetto”).
   - Destra: diagramma psicrometrico interattivo.

==================================================
3) FLUSSO DI LAVORO IN 3 PASSI
==================================================
1) Condizioni
   • Impostare Altitudine (m) e/o Pressione (kPa).
     - Se si inserisce la Pressione, ha priorità sull’Altitudine.
   • Impostare la Portata d’aria (m³/h). Questa portata è usata nei processi per potenza (kW), umidità (L/h), ecc.
2) Stati
   • In “+ Nuovo stato”: indicare Nome, Tbs (°C) e φ (%). Aggiungere.
   • I punti vengono disegnati con il loro nome. Al passaggio del mouse si vedono tutti i parametri.
   • È possibile eliminare stati dalla scheda “Stati”.
3) Processi
   • In “+ Nuovo processo”: scegliere il Tipo, selezionare l’Ingresso, definire i parametri e scrivere il nome di Uscita.
   • Premere “Applica”: viene creato il nuovo stato e la linea del processo si disegna nel grafico.
   • Ogni processo appare nella sua scheda con ΔT, Δx, Δh, ṁ_umidità, Q totale e Q sensibile.

4) Esportazione (scheda “Progetto”)
   • Salvare JSON: salva il “progetto” per riaprirlo in seguito.
   • Esportare CSV: tabella leggibile (nomi degli stati/processi + grandezze).
   • Esportare JSON (dati): JSON di report con stati e processi derivati.
   • Esportare PDF: 3 pagine pulite (1: grafico, 2: tabella stati, 3: tabella processi).

==================================================
4) SCHEDE (FUNZIONALITÀ)
==================================================
CONDIZIONI
• Altitudine (m) → convertita in pressione barometrica standard se P non è indicata.
• Pressione (kPa) → se compilata, prevale sull’Altitudine.
• Portata d’aria (m³/h) → usata per convertire differenze specifiche in kW e L/h.

+ NUOVO STATO
• Inserire Nome, Tbs (°C) e φ (%). Il nome deve essere univoco.
• Calcoli automatici: x (g/kg_da), h (kJ/kg_da), Twb (°C), ρ (kg/m³).

+ NUOVO PROCESSO
• Tipo: Riscaldare, Raffreddare, Miscelare, Recupero di calore, Umidificare.
• Ingresso: selezionare lo stato di partenza.
• Parametri: variano in base al Tipo (vedere sezione 5).
• Uscita: nome dello stato risultante.
• Applica: crea lo stato e aggiunge il processo.

STATI
• Elenco degli stati con le grandezze calcolate. Pulsante “Elimina” per singolo stato.

PROCESSI
• Elenco dei processi con:
  Processo_#, Tipo (badge), connessioni (per nome) e metriche:
  ΔT (°C), Δx (g/kg_da), Δh (kJ/kg_da), ṁ_umidità (L/h), Q totale (kW), Q sensibile (kW).

PROGETTO
• Salva/Carica JSON del progetto, Esporta CSV, Esporta JSON (dati) ed Esporta PDF.

==================================================
5) DIAGRAMMA PSICROMETRICO (COSA SI VEDE)
==================================================
• Asse X: Tbs (°C)
• Asse Y: x (g/kg_da)
• Curve di sfondo: umidità relativa (10–100%) + etichette 10/30/50/70/90.
• Linee diagonali puntinate: entalpia (h, kJ/kg_da).
• Punti blu: stati (etichetta fissa = nome; tooltip = T, φ, x, h, Twb, ρ).
• Linee di processo:
  - Miscelazione: verde scuro; ausilio grigio per il “nuovo” flusso + linea grigia tratteggiata.
  - Recupero calore (HRV/ERV): arancione; ausilio grigio per il secondario + linea grigia tratteggiata.
  - Raffreddamento con condensazione: ausilio grigio che indica l’ADP + linea grigia tratteggiata.
• Clic sul grafico: seleziona lo stato più vicino (utile per modifica/eliminazione).

==================================================
6) PROCESSI E TEORIA (SINTESI OPERATIVA)
==================================================
Note comuni
• P = pressione in kPa (da Altitudine o campo Pressione).
• Portata → ṁ ≈ (m³/h / 3600) · ρ (kg/s) dello stato di ingresso.
• In tutti i bilanci, x è in kg/kg_da (a schermo in g/kg_da).

RISCALDARE (heat)
• Modalità (“Descrizione”):
  1) Potenza (Q): aggiungi potenza (kW). Si traduce in Δh = Q/ṁ → h2 = h1 + Δh e T2 da h–w.
  2) ΔT (dT): T2 = T1 + ΔT; w2 = w1 (riscaldamento sensibile).
  3) Δh (dH): h2 = h1 + Δh; T2 da h–w.
  4) a T (Ttarget): obiettivo di temperatura; w costante.
  5) T parete (Twall, η): scambio verso la parete → T2 = T1 + η·(Twall − T1).
  6) Fluido medio (Tin, Tout, η): usa Tmedia del fluido e η → T2 = T1 + η·(Tmedia − T1).
• Umidità: nessun apporto/prelievo di massa d’acqua → w costante.

RAFFRESCARE (cool)
• Stesse descrizioni di Riscaldare (Q, ΔT, Δh, a T, parete, fluido) con un «Rendimento batteria (%)» (η_total).
• Si calcola prima un punto “sensibile” (stesso w). Se lì UR>100%:
  – Si attiva la condensazione con modello ADP + BF:
    • ADP ≈ Tsensibile − ΔADP (regolabile o legato a parete/acqua).
    • BF ~ 1 − η_total (puoi sovrascriverlo).
    • Stato finale = miscela: h2 = BF·h1 + (1−BF)·h_ADP, w2 = BF·w1 + (1−BF)·w_ADP.
• Uscite: Q totale (kW) = ṁ·(h2 − h1), Q sensibile (kW) = ṁ·(h(T2, w1) − h1), ṁ_acqua (L/h) = 3600·(w2 − w1)·ṁ.

UMIDIFICARE (hum_adiab ↔ hum_isot)
• Tipo = Adiabatico (h costante) o Isotermo (T costante).
• Descrizione:
  1) DX (Δx): incremento diretto di x (limitato per evitare supersaturazione).
  2) a UR%: obiettivo φ; risolve T e w secondo il tipo (adiabatico: h costante; isotermo: T costante).
  3) Portata acqua (L/h): converte ṁ_acqua in Δx = (ṁ_acqua / 3600)/ṁ (con limitazione a saturazione).
• Il sistema limita a φ ≤ 100% (saturazione).

RECUPERO DI CALORE (HRV/ERV)
• Parametri: T secondaria, UR secondaria, efficienza sensibile (ηS), recupero latente (ηL).
• Formule:
  – T2 = T1 + ηS·(Tsec − T1)
  – w2 = w1 (HRV) oppure w1 + ηL·(wsec − w1) (ERV)

MISCELARE (mix)
• Due varianti implementate:
  – Miscelare due stati esistenti con frazione fA.
  – Miscelare uno stato esistente con un “nuovo flusso” (Tn, RHn, q_nuovo e % del flusso esistente utilizzato).
• Bilanci di massa ed energia:
  – h_mix = fE·h_esist + fN·h_nuovo  ;  w_mix = fE·w_esist + fN·w_nuovo (con fE proporzionale al ṁ usato).

==================================================
7) ESPORTAZIONI
==================================================
• CSV (leggibile): nomi (non ID), stati completi e processi con: ΔT, Δx, Δh, ṁ_umidità (L/h), Q totale (kW), Q sensibile (kW).
• JSON (dati): struttura “di report” con nomi e grandezze derivate (ideale per post‑processing).
• PDF: 3 pagine → 1) Grafico, 2) Stati, 3) Processi.

==================================================
8) VALIDAZIONI E LIMITI
==================================================
• φ tra 0 e 100%. Il sistema limita se necessario.
• Per i processi che usano la portata, impostare prima la Portata d’aria (m³/h) in Condizioni.
• Miscelazione con «% del flusso esistente per miscelare» accetta 0–100% (inclusi).
• Saturazione: il sistema evita φ > 100%; in raffreddamento, la condensazione è calcolata automaticamente.
• Nomi degli stati univoci (avviso in caso di duplicati).

==================================================
9) CONSIGLI PRATICI
==================================================
• Attivare/nascondere le curve di UR dalla barra superiore per visualizzare meglio i processi.
• Usare «a T» o «a UR%» per fissare target diretti e validare i progetti.
• Esportare PDF per documentazione (grafico pulito + tabelle). CSV/JSON per calcoli e tracciabilità.
• Regolare ΔADP e BF per affinare la previsione sulle batterie reali.

==================================================
10) GLOSSARIO RAPIDO
==================================================
Tbs: temperatura a bulbo secco (°C)
Twb: temperatura a bulbo umido (°C)
φ: umidità relativa (%)
x: umidità assoluta / rapporto di umidità (kg/kg_da) [a schermo in g/kg_da]
h: entalpia specifica (kJ/kg_da)
ρ: densità dell’aria umida (kg/m³)
ADP: Apparatus Dew Point (punto di rugiada della batteria)
BF: Bypass Factor (frazione che «non tocca» l’ADP)
η_total: rendimento globale della batteria (sensibile + approccio)

Fine.
