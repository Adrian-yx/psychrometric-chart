Psychrometric Chart by EMERYX
README (uso rápido + teoría)

==================================================
1) ¿QUÉ ES?
==================================================
Aplicación de cálculo y trazado psicrométrico para crear ESTADOS de aire, encadenar PROCESOS (calentar, enfriar, mezclar, recuperar calor, humidificar) y exportar resultados (PDF, CSV y JSON). Pensada para trabajo técnico ágil y verificable.

Requisitos: abrir el archivo HTML en un navegador moderno (Chrome recomendado). No necesita servidor.

==================================================
2) CÓMO ABRIR
==================================================
1. Guarda el archivo: psychro_mvp_desktop_v2f.html
2. Ábrelo con doble clic o arrástralo a una pestaña de tu navegador.
3. Verás una barra superior (conmutador de curvas RH) y dos zonas:
   - Izquierda: “tarjetas” de control (Condiciones, Estados, Procesos, Proyecto).
   - Derecha: diagrama psicrométrico interactivo.

==================================================
3) FLUJO DE TRABAJO EN 3 PASOS
==================================================
1) Condiciones
   • Define Altitud (m) y/o Presión (kPa).
     - Si introduces Presión, tiene prioridad sobre Altitud.
   • Define Caudal aire (m³/h). Este caudal se usa en procesos para potencia (kW), humedad (L/h), etc.
2) Estados
   • En “+ Nuevo estado”: pon Nombre, Tbs (°C) y φ (%). Añadir.
   • Los puntos se dibujan en la gráfica con su nombre. Al pasar el ratón ves todos los parámetros.
   • Puedes borrar estados desde la tarjeta “Estados”.
3) Procesos
   • En “+ Nuevo proceso”: elige Tipo, selecciona Entrada, define parámetros y escribe el nombre de Salida.
   • Pulsa “Aplicar”: se crea el nuevo estado y la línea del proceso en la gráfica.
   • Cada proceso aparece en su tarjeta con ΔT, Δx, Δh, ṁ_humedad, Q total y Q sensible.

4) Exportación (tarjeta “Proyecto”)
   • Guardar JSON: guarda el “proyecto” para abrirlo después.
   • Exportar CSV: tabla legible (nombres de estados/procesos + magnitudes).
   • Exportar JSON (datos): JSON de informe con estados y procesos derivados.
   • Exportar PDF: 3 páginas limpias (1: gráfica, 2: tabla de estados, 3: tabla de procesos).

==================================================
4) TARJETAS (QUÉ HACE CADA UNA)
==================================================
CONDICIONES
• Altitud (m) → se convierte a Presión barométrica estándar si no indicas P.
• Presión (kPa) → si se rellena, manda sobre Altitud.
• Caudal aire (m³/h) → se usa para convertir diferencias específicas a kW y L/h.

+ NUEVO ESTADO
• Introduce Nombre, Tbs (°C) y φ (%). El nombre debe ser único.
• Se calculan automáticamente: x (g/kg_da), h (kJ/kg_da), Twb (°C), ρ (kg/m³).

+ NUEVO PROCESO
• Tipo: Calentar, Enfriar, Mezcla, Recuperación de calor, Humidificar.
• Entrada: elige estado de partida.
• Parámetros: cambian según el Tipo (ver sección 5).
• Salida: nombre del estado resultante.
• Aplicar: crea el estado y añade el proceso.

ESTADOS
• Lista de estados con sus magnitudes calculadas. Botón “Borrar” por estado.

PROCESOS
• Lista de procesos con:
  Proceso_#, Tipo (badge), conexiones (por nombre), y métricas:
  ΔT (°C), Δx (g/kg_da), Δh (kJ/kg_da), ṁ_humedad (L/h), Q total (kW), Q sensible (kW).

PROYECTO
• Guardar/Cargar JSON del proyecto, Exportar CSV, Exportar JSON (datos) y Exportar PDF.

==================================================
5) DIAGRAMA PSICROMÉTRICO (QUÉ VES)
==================================================
• Eje X: Tbs (°C)
• Eje Y: x (g/kg_da)
• Curvas de fondo: isohumedades relativas (10–100%) + etiquetas 10/30/50/70/90.
• Líneas diagonales punteadas: entalpía (h, kJ/kg_da).
• Puntos azules: estados (etiqueta fija = nombre; tooltip = T, φ, x, h, Twb, ρ).
• Líneas de proceso:
  - Mezcla: verde oscura; helper gris del caudal “nuevo” + línea gris discontinua.
  - Recuperación (HRV/ERV): naranja; helper gris del secundario + línea gris discontinua.
  - Enfriar con condensación: helper gris marca el ADP + línea gris discontinua.
• Click en la gráfica: selecciona el estado más cercano (útil para edición/borrado).

==================================================
6) PROCESOS Y TEORÍA (RESUMEN OPERATIVO)
==================================================
Notas comunes
• P = presión en kPa (de Altitud o campo Presión).
• Caudal → ṁ ≈ (m³/h / 3600) · ρ (kg/s) del estado de entrada.
• En todos los balances, x está en kg/kg_da (en pantalla se ve g/kg_da).

CALENTAR (heat)
• Modos (“Descripción”):
  1) Potencia (Q): añades una potencia (kW). Se transforma en Δh = Q/ṁ → h2 = h1 + Δh y T2 por h–w.
  2) ΔT (dT): T2 = T1 + ΔT; w2 = w1 (calentamiento sensible).
  3) Δh (dH): h2 = h1 + Δh; T2 por h–w.
  4) a T (Ttarget): objetivo de temperatura; w constante.
  5) T pared (Twall, η): aproximación con intercambio hacia la pared → T2 = T1 + η·(Twall − T1).
  6) Caract. medio hyd. (Tin, Tout, η): usa Tmedio del fluido y η → T2 = T1 + η·(Tmedio − T1).
• Humedad: sin aporte/retirada de masa de agua → w constante.

ENFRIAR (cool)
• Igual que Calentar en descripciones (Q, ΔT, Δh, a T, pared, medio hyd.) con un “Rend. batería (%)” (η_total).
• Primero se calcula un punto “sensible” (misma w). Si allí la HR>100%:
  – Se activa la condensación con un modelo ADP + BF:
    • ADP ≈ Tsensible − ΔADP (puede ajustarse o ligarse a pared/agua).
    • BF ~ 1 − η_total (puedes sobreescribirlo).
    • Estado final = mezcla: h2 = BF·h1 + (1−BF)·h_ADP, w2 = BF·w1 + (1−BF)·w_ADP.
• Salidas: Q total (kW) = ṁ·(h2 − h1), Q sensible (kW) = ṁ·(h(T2, w1) − h1), ṁ_agua (L/h) = 3600·(w2 − w1)·ṁ.

HUMIDIFICAR (hum_adiab ↔ hum_isot)
• Tipo = Adiabática (constante h) o Isotérmica (constante T).
• Descripción:
  1) DX (Δx): incremento directo de x (controlado para no superar saturación).
  2) a Hr%: objetivo de φ; resuelve T y w según tipo (adiabática: mantiene h; isotérmica: mantiene T).
  3) Caudal de agua (L/h): convierte ṁ_agua a Δx = (ṁ_agua / 3600)/ṁ (con corte a saturación).
• El sistema limita a φ ≤ 100% (saturación).

RECUPERACIÓN DE CALOR (HRV/ERV)
• Parámetros: T secundaria, Hr secundaria, eficiencia térmica (ηS), recup. de humedad (ηL).
• Fórmulas:
  – T2 = T1 + ηS·(Tsec − T1)
  – w2 = w1 (HRV) o w1 + ηL·(wsec − w1) (ERV)

MEZCLA (mix)
• Dos variantes implementadas:
  – Mezclar dos estados existentes con fracción fA.
  – Mezclar un estado existente con un “caudal nuevo” (Tn, RHn, q_nuevo y % del caudal existente que usas).
• Balance másico y energético:
  – h_mix = fE·h_exist + fN·h_nuevo  ;  w_mix = fE·w_exist + fN·w_nuevo (con fE proporcional a ṁ usado).

==================================================
7) EXPORTACIONES
==================================================
• CSV (legible): nombres (no IDs), estados completos y procesos con: ΔT, Δx, Δh, ṁ_humedad (L/h), Q total (kW), Q sensible (kW).
• JSON (datos): estructura “de informe” con nombres y magnitudes derivadas (ideal para postprocesado).
• PDF: 3 páginas → 1) Gráfica, 2) Estados, 3) Procesos.

==================================================
8) VALIDACIONES Y LÍMITES
==================================================
• φ entre 0 y 100 %. El sistema recorta si hace falta.
• En procesos que usan caudal, define primero el Caudal aire (m³/h) en Condiciones.
• Mezcla con “% caudal existente para mezcla” admite 0–100 % (incluidos).
• Saturación: el sistema evita φ > 100 %; con enfriamiento, la condensación se calcula automáticamente.
• Nombres de estados únicos (se avisa si hay duplicados).

==================================================
9) CONSEJOS PRÁCTICOS
==================================================
• Activa/oculta curvas RH desde la barra superior para ver mejor los procesos.
• Usa “a T” o “a Hr%” para fijar objetivos directos y validar proyectos.
• Exporta PDF para documentación (gráfica limpia + tablas). CSV/JSON para cálculos y trazabilidad.
• Ajusta ΔADP y BF cuando quieras afinar la predicción en baterías reales.

==================================================
10) GLOSARIO RÁPIDO
==================================================
Tbs: temperatura de bulbo seco (°C)
Twb: temperatura de bulbo húmedo (°C)
φ: humedad relativa (%)
x: humedad absoluta (kg/kg_da) [en pantalla se muestra g/kg_da]
h: entalpía específica (kJ/kg_da)
ρ: densidad del aire húmedo (kg/m³)
ADP: Apparatus Dew Point (punto de rocío de la batería)
BF: Bypass Factor (fracción que “no toca” el ADP)
η_total: rendimiento global de batería (sensible + aproximación)

Fin.
