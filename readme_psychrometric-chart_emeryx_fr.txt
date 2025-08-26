Psychrometric Chart by EMERYX
README (utilisation rapide + théorie)

==================================================
1) QU’EST‑CE QUE C’EST ?
==================================================
Application de calcul et de tracé psychrométrique pour créer des ÉTATS d’air, enchaîner des PROCESSUS (chauffer, refroidir, mélanger, récupérer la chaleur, humidifier) et exporter les résultats (PDF, CSV et JSON). Pensée pour un travail technique agile et vérifiable.

Exigences : ouvrir le fichier HTML dans un navigateur moderne (Chrome recommandé). Aucun serveur nécessaire.

==================================================
2) COMMENT OUVRIR
==================================================
1. Enregistrez le fichier : psychro_mvp_desktop_v2f.html
2. Ouvrez‑le par double‑clic ou glissez‑déposez‑le dans un onglet du navigateur.
3. Vous verrez une barre supérieure (commutateur des courbes HR) et deux zones :
   - À gauche : cartes de contrôle (« Conditions », « États », « Processus », « Projet »).
   - À droite : diagramme psychrométrique interactif.

==================================================
3) FLUX DE TRAVAIL EN 3 ÉTAPES
==================================================
1) Conditions
   • Définir Altitude (m) et/ou Pression (kPa).
     - Si vous indiquez la Pression, elle prime sur l’Altitude.
   • Définir le Débit d’air (m³/h). Ce débit est utilisé dans les processus pour la puissance (kW), l’humidité (L/h), etc.
2) États
   • Dans « + Nouvel état » : saisir Nom, Tbs (°C) et φ (%). Ajouter.
   • Les points sont tracés sur le graphique avec leur nom. Au survol, vous voyez tous les paramètres.
   • Vous pouvez supprimer des états depuis la carte « États ».
3) Processus
   • Dans « + Nouveau processus » : choisir le Type, sélectionner l’Entrée, définir les paramètres et saisir le nom de Sortie.
   • Cliquer sur « Appliquer » : le nouvel état est créé et la ligne du processus apparaît sur le graphique.
   • Chaque processus s’affiche dans sa carte avec ΔT, Δx, Δh, ṁ_humidité, Q total et Q sensible.

4) Exportation (carte « Projet »)
   • Enregistrer JSON : enregistre le « projet » pour l’ouvrir ultérieurement.
   • Exporter CSV : tableau lisible (noms des états/processus + grandeurs).
   • Exporter JSON (données) : JSON de rapport avec états et processus dérivés.
   • Exporter PDF : 3 pages épurées (1 : graphique, 2 : tableau des états, 3 : tableau des processus).

==================================================
4) CARTES (FONCTIONS)
==================================================
CONDITIONS
• Altitude (m) → convertie en pression barométrique standard si P n’est pas indiquée.
• Pression (kPa) → si renseignée, elle prévaut sur l’Altitude.
• Débit d’air (m³/h) → utilisé pour convertir les différences spécifiques en kW et L/h.

+ NOUVEL ÉTAT
• Saisir Nom, Tbs (°C) et φ (%). Le nom doit être unique.
• Calculs automatiques : x (g/kg_da), h (kJ/kg_da), Twb (°C), ρ (kg/m³).

+ NOUVEAU PROCESSUS
• Type : Chauffer, Refroidir, Mélange, Récupération de chaleur, Humidifier.
• Entrée : choisir l’état de départ.
• Paramètres : varient selon le Type (voir section 5).
• Sortie : nom de l’état résultant.
• Appliquer : crée l’état et ajoute le processus.

ÉTATS
• Liste des états avec leurs grandeurs calculées. Bouton « Supprimer » par état.

PROCESSUS
• Liste des processus avec :
  Processus_#, Type (badge), connexions (par nom) et métriques :
  ΔT (°C), Δx (g/kg_da), Δh (kJ/kg_da), ṁ_humidité (L/h), Q total (kW), Q sensible (kW).

PROJET
• Enregistrer/Charger le JSON du projet, Exporter CSV, Exporter JSON (données) et Exporter PDF.

==================================================
5) DIAGRAMME PSYCHROMÉTRIQUE (CE QUE VOUS VOYEZ)
==================================================
• Axe X : Tbs (°C)
• Axe Y : x (g/kg_da)
• Courbes de fond : isohumidités relatives (10–100 %) + étiquettes 10/30/50/70/90.
• Lignes diagonales en pointillés : enthalpie (h, kJ/kg_da).
• Points bleus : états (étiquette fixe = nom ; infobulle = T, φ, x, h, Twb, ρ).
• Lignes de processus :
  - Mélange : vert foncé ; repère gris du « nouveau » débit + ligne grise discontinue.
  - Récupération (HRV/ERV) : orange ; repère gris du secondaire + ligne grise discontinue.
  - Refroidissement avec condensation : repère gris marquant l’ADP + ligne grise discontinue.
• Clic sur le graphique : sélectionne l’état le plus proche (utile pour éditer/supprimer).

==================================================
6) PROCESSUS ET THÉORIE (RÉSUMÉ OPÉRATIONNEL)
==================================================
Notes communes
• P = pression en kPa (depuis Altitude ou champ Pression).
• Débit → ṁ ≈ (m³/h / 3600) · ρ (kg/s) de l’état d’entrée.
• Dans tous les bilans, x est en kg/kg_da (à l’écran en g/kg_da).

CHAUFFER (heat)
• Modes (« Description ») :
  1) Puissance (Q) : vous ajoutez une puissance (kW). Elle se transforme en Δh = Q/ṁ → h2 = h1 + Δh et T2 via h–w.
  2) ΔT (dT) : T2 = T1 + ΔT ; w2 = w1 (échauffement sensible).
  3) Δh (dH) : h2 = h1 + Δh ; T2 via h–w.
  4) à T (Ttarget) : objectif de température ; w constant.
  5) T paroi (Twall, η) : échange vers la paroi → T2 = T1 + η·(Twall − T1).
  6) Caract. fluide (Tin, Tout, η) : utilise Tmoy du fluide et η → T2 = T1 + η·(Tmoy − T1).
• Humidité : pas d’apport/retrait d’eau → w constant.

REFROIDIR (cool)
• Identique à Chauffer dans les descriptions (Q, ΔT, Δh, à T, paroi, fluide) avec un « Rendement batterie (%) » (η_total).
• On calcule d’abord un point « sensible » (même w). Si HR>100 % à ce point :
  – Condensation activée via modèle ADP + BF :
    • ADP ≈ Tsensible − ΔADP (ajustable ou lié à paroi/eau).
    • BF ~ 1 − η_total (peut être écrasé).
    • État final = mélange : h2 = BF·h1 + (1−BF)·h_ADP, w2 = BF·w1 + (1−BF)·w_ADP.
• Sorties : Q total (kW) = ṁ·(h2 − h1), Q sensible (kW) = ṁ·(h(T2, w1) − h1), ṁ_eau (L/h) = 3600·(w2 − w1)·ṁ.

HUMIDIFIER (hum_adiab ↔ hum_isot)
• Type = Adiabatique (h constante) ou Isotherme (T constante).
• Description :
  1) DX (Δx) : incrément direct de x (contrôlé pour ne pas dépasser la saturation).
  2) à Hr% : objectif de φ ; résout T et w selon le type (adiabatique : h maintenue ; isotherme : T maintenue).
  3) Débit d’eau (L/h) : convertit ṁ_eau en Δx = (ṁ_eau / 3600)/ṁ (avec coupure à la saturation).
• Le système limite à φ ≤ 100 % (saturation).

RÉCUPÉRATION DE CHALEUR (HRV/ERV)
• Paramètres : T secondaire, Hr secondaire, efficacité thermique (ηS), récupération d’humidité (ηL).
• Formules :
  – T2 = T1 + ηS·(Tsec − T1)
  – w2 = w1 (HRV) ou w1 + ηL·(wsec − w1) (ERV)

MÉLANGE (mix)
• Deux variantes :
  – Mélanger deux états existants avec la fraction fA.
  – Mélanger un état existant avec un « nouveau débit » (Tn, RHn, q_nouveau et % du débit existant utilisé).
• Bilan massique et énergétique :
  – h_mix = fE·h_exist + fN·h_nouveau  ;  w_mix = fE·w_exist + fN·w_nouveau (avec fE proportionnel à ṁ utilisé).

==================================================
7) EXPORTATIONS
==================================================
• CSV (lisible) : noms (pas d’IDs), états complets et processus avec : ΔT, Δx, Δh, ṁ_humidité (L/h), Q total (kW), Q sensible (kW).
• JSON (données) : structure « rapport » avec noms et grandeurs dérivées (idéal pour post‑traitement).
• PDF : 3 pages → 1) Graphique, 2) États, 3) Processus.

==================================================
8) VALIDATIONS ET LIMITES
==================================================
• φ entre 0 et 100 %. Le système recadre si nécessaire.
• Pour les processus qui utilisent le débit, définir d’abord le Débit d’air (m³/h) dans Conditions.
• Mélange avec « % du débit existant pour mélange » accepte 0–100 % (inclus).
• Saturation : φ > 100 % évité ; en refroidissement, la condensation est calculée automatiquement.
• Noms d’états uniques (alerte en cas de doublon).

==================================================
9) CONSEILS PRATIQUES
==================================================
• Activez/masquez les courbes HR dans la barre supérieure pour mieux voir les processus.
• Utilisez « à T » ou « à Hr% » pour fixer des objectifs directs et valider des projets.
• Exportez en PDF pour la documentation (graphique propre + tableaux). CSV/JSON pour calculs et traçabilité.
• Ajustez ΔADP et BF pour affiner la prédiction sur des batteries réelles.

==================================================
10) GLOSSAIRE RAPIDE
==================================================
Tbs : température de bulbe sec (°C)
Twb : température de bulbe humide (°C)
φ : humidité relative (%)
x : teneur en humidité (kg/kg_da) [à l’écran en g/kg_da]
h : enthalpie spécifique (kJ/kg_da)
ρ : densité de l’air humide (kg/m³)
ADP : Apparatus Dew Point (point de rosée de la batterie)
BF : Bypass Factor (fraction qui « n’atteint pas » l’ADP)
η_total : rendement global de batterie (sensible + approche)

Fin.
