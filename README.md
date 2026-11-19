# Pocho-Lab-
Aplicación para usar datos reologicos de la harina de trigo 
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pocho Lab - Reología</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
  <style>
    :root {
      --bg-void: #050505;
      --bg-surface: #0b0b0b;
      --bg-elevated: #121212;
      --border-subtle: #242424;
      --border-strong: #3f3f46;
      --ink-white: #ffffff;
      --ink-muted: #a3a3a3;
    }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background-color: var(--bg-void);
      color: var(--ink-white);
    }

    .font-poster {
      font-family: 'Bebas Neue', Impact, 'Arial Narrow', sans-serif;
      letter-spacing: 0.04em;
    }

    /* Estilo limpio para sliders personalizados en tema negro de alto contraste */
    input[type="range"].alveo-slider {
      -webkit-appearance: none;
      appearance: none;
      width: 100%;
      height: 8px;
      border-radius: 9999px;
      background: #262626;
      border: 1px solid #3a3a3a;
      outline: none;
      cursor: pointer;
    }

    input[type="range"].alveo-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 22px;
      height: 22px;
      border-radius: 50%;
      background: #ffffff;
      border: 3px solid #10b981;
      box-shadow: 0 0 0 2px #050505, 0 2px 8px rgba(0, 0, 0, 0.8);
      cursor: pointer;
      transition: transform 0.15s ease, border-color 0.15s ease;
    }

    input[type="range"].alveo-slider::-webkit-slider-thumb:hover {
      transform: scale(1.12);
      border-color: #34d399;
    }

    input[type="range"].alveo-slider::-moz-range-thumb {
      width: 22px;
      height: 22px;
      border-radius: 50%;
      background: #ffffff;
      border: 3px solid #10b981;
      box-shadow: 0 0 0 2px #050505, 0 2px 8px rgba(0, 0, 0, 0.8);
      cursor: pointer;
      transition: transform 0.15s ease, border-color 0.15s ease;
    }

    input[type="number"]::-webkit-inner-spin-button,
    input[type="number"]::-webkit-outer-spin-button {
      opacity: 0.8;
    }
  </style>
</head>
<body class="bg-[#050505] text-white antialiased selection:bg-white selection:text-[#050505]">
  <!-- Contenedor principal -->
  <div class="min-h-screen flex flex-col bg-[#050505]">
    <!-- Encabezado técnico-profesional estilo póster -->
    <header class="bg-[#0b0b0b] border-b border-[#262626] sticky top-0 z-20">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex flex-col md:flex-row md:items-center md:justify-between gap-3">
        <div class="flex items-start sm:items-center gap-3.5">
          <div class="w-11 h-11 rounded-xl bg-white text-[#050505] flex items-center justify-center shrink-0 border border-white/20">
            <!-- Icono SVG de curva reológica -->
            <svg class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="2.2" viewBox="0 0 24 24" aria-hidden="true">
              <path stroke-linecap="round" stroke-linejoin="round" d="M3 19h18M4 17c2-9 5-12 8-7 2.5 4.2 4 6.5 8 7" />
            </svg>
          </div>
          <div>
            <div class="flex flex-wrap items-center gap-2.5">
              <h1 class="font-poster text-3xl sm:text-4xl text-white tracking-wider leading-none">
                Pocho Lab - Reología
              </h1>
              <span class="text-[11px] font-semibold uppercase tracking-wider px-2.5 py-0.5 rounded-full bg-[#141414] text-white border border-[#383838]">
                Alveógrafo Chopin · ISO 27971
              </span>
            </div>
            <p class="text-xs sm:text-sm text-neutral-300 mt-1">
              Controla <strong class="text-white">W</strong>, <strong class="text-white">P</strong>, <strong class="text-white">L</strong>, <strong class="text-white">G</strong> y <strong class="text-white">P/L</strong> y observa cómo sincronizan la curva y sus parámetros derivados (<strong class="text-white">P200</strong>, <strong class="text-white">G</strong>, <strong class="text-white">P/L</strong> y <strong class="text-white">W</strong>).
            </p>
          </div>
        </div>

        <!-- Barra de estado / aviso de validación -->
        <div id="status-banner" class="hidden text-xs font-medium px-3 py-2 rounded-lg border bg-amber-950/80 text-amber-200 border-amber-500/50">
          <!-- Mensajes no bloqueantes -->
        </div>
      </div>
    </header>

    <!-- Cuerpo principal -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">
      <!-- Preajustes rápidos -->
      <section aria-label="Perfiles preestablecidos de harina" class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-4 sm:p-5 space-y-3.5">
        <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2 border-b border-[#1f1f1f] pb-3">
          <div>
            <h2 class="font-poster text-2xl text-white tracking-wide">Perfiles de Referencia</h2>
            <p class="text-xs text-neutral-400">Selecciona una ficha o perfil para cargar valores de <span class="text-white font-medium">P</span> y <span class="text-white font-medium">L</span> al instante.</p>
          </div>
          <span id="active-preset-label" class="text-xs font-semibold text-white bg-[#151515] px-3 py-1 rounded-full border border-[#3a3a3a] self-start sm:self-auto">
            Perfil activo: Ficha de referencia
          </span>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-2.5" id="presets-container">
          <!-- Botones generados desde JS -->
        </div>
      </section>

      <!-- Grid principal: Controles a la izquierda + Gráfico y métricas a la derecha -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">
        <!-- Columna izquierda: 5 Parámetros controlados (W, P, L, G, P/L) -->
        <section aria-label="Controles de entrada reológica" class="lg:col-span-5 bg-[#0b0b0b] rounded-2xl border border-[#262626] p-5 space-y-5">
          <div class="border-b border-[#1f1f1f] pb-3 flex items-start justify-between gap-2">
            <div>
              <h2 class="font-poster text-2xl text-white tracking-wide">Parámetros Controlados</h2>
              <p class="text-xs text-neutral-400 mt-0.5">Ajusta cualquiera de los 5 parámetros: se sincronizan <code class="font-mono text-emerald-300">G</code>, <code class="font-mono text-indigo-300">P/L</code> y la curva de <code class="font-mono text-white">W</code>.</p>
            </div>
          </div>

          <!-- Retroalimentación visual de sincronización -->
          <div id="sync-feedback" class="p-3 rounded-xl bg-[#111111] border border-[#2d2d2d] text-xs space-y-1.5">
            <div class="flex items-center justify-between gap-2">
              <span class="font-semibold text-white uppercase tracking-wider text-[11px]">Sincronización Reológica</span>
              <span id="sync-badge" class="px-2 py-0.5 rounded-full text-[10px] font-semibold bg-emerald-950/90 text-emerald-300 border border-emerald-500/40">
                Curva calibrada
              </span>
            </div>
            <p id="sync-detail" class="text-neutral-300 leading-relaxed">
              G = 2.226·√L · P/L = P/L · W integrado bajo la curva desde 0 hasta L.
            </p>
          </div>

          <!-- 1. W (10^-4 J) -->
          <div id="ctrl-wrap-W" class="space-y-2 pt-1 rounded-xl transition-colors">
            <div class="flex items-center justify-between gap-2">
              <label for="input-w-num" class="text-sm font-semibold text-white">
                W — Fuerza / Trabajo (10⁻⁴J)
              </label>
              <div class="flex items-center gap-1.5">
                <input
                  id="input-w-num"
                  type="number"
                  min="50"
                  max="800"
                  step="1"
                  value="405"
                  class="w-20 px-2.5 py-1.5 text-sm font-bold text-right text-white bg-[#121212] border border-[#3a3a3a] rounded-lg focus:outline-none focus:ring-2 focus:ring-white focus:border-white"
                />
                <span class="text-xs font-medium text-neutral-400 w-12">10⁻⁴J</span>
              </div>
            </div>
            <input
              id="input-w-range"
              type="range"
              min="50"
              max="800"
              step="1"
              value="405"
              class="alveo-slider"
              aria-label="Deslizador de fuerza W en 10^-4 J"
            />
            <div class="flex justify-between text-[11px] text-neutral-400">
              <span>50 (Floja)</span>
              <span>Rango: 50–800</span>
              <span>800 (Extra fuerte)</span>
            </div>
          </div>

          <!-- 2. P (mmH2O) -->
          <div id="ctrl-wrap-P" class="space-y-2 pt-3 border-t border-[#1f1f1f] rounded-xl transition-colors">
            <div class="flex items-center justify-between gap-2">
              <label for="input-p-num" class="text-sm font-semibold text-white">
                P — Tenacidad (mmH₂O)
              </label>
              <div class="flex items-center gap-1.5">
                <input
                  id="input-p-num"
                  type="number"
                  min="25"
                  max="160"
                  step="1"
                  value="105"
                  class="w-20 px-2.5 py-1.5 text-sm font-bold text-right text-white bg-[#121212] border border-[#3a3a3a] rounded-lg focus:outline-none focus:ring-2 focus:ring-white focus:border-white"
                />
                <span class="text-xs font-medium text-neutral-400 w-12">mmH₂O</span>
              </div>
            </div>
            <input
              id="input-p-range"
              type="range"
              min="25"
              max="160"
              step="1"
              value="105"
              class="alveo-slider"
              aria-label="Deslizador de tenacidad P en mmH2O"
            />
            <div class="flex justify-between text-[11px] text-neutral-400">
              <span>25 mmH₂O</span>
              <span>Rango: 25–160</span>
              <span>160 mmH₂O</span>
            </div>
          </div>

          <!-- 3. L (mm) -->
          <div id="ctrl-wrap-L" class="space-y-2 pt-3 border-t border-[#1f1f1f] rounded-xl transition-colors">
            <div class="flex items-center justify-between gap-2">
              <label for="input-l-num" class="text-sm font-semibold text-white">
                L — Extensibilidad (mm)
              </label>
              <div class="flex items-center gap-1.5">
                <input
                  id="input-l-num"
                  type="number"
                  min="20"
                  max="220"
                  step="1"
                  value="100"
                  class="w-20 px-2.5 py-1.5 text-sm font-bold text-right text-white bg-[#121212] border border-[#3a3a3a] rounded-lg focus:outline-none focus:ring-2 focus:ring-white focus:border-white"
                />
                <span class="text-xs font-medium text-neutral-400 w-12">mm</span>
              </div>
            </div>
            <input
              id="input-l-range"
              type="range"
              min="20"
              max="220"
              step="1"
              value="100"
              class="alveo-slider"
              aria-label="Deslizador de extensibilidad L en mm"
            />
            <div class="flex justify-between text-[11px] text-neutral-400">
              <span>20 mm</span>
              <span>Rango: 20–220</span>
              <span>220 mm</span>
            </div>
          </div>

          <!-- 4. G -->
          <div id="ctrl-wrap-G" class="space-y-2 pt-3 border-t border-[#1f1f1f] rounded-xl transition-colors">
            <div class="flex items-center justify-between gap-2">
              <label for="input-g-num" class="text-sm font-semibold text-white">
                G — Índice de hinchamiento
              </label>
              <div class="flex items-center gap-1.5">
                <input
                  id="input-g-num"
                  type="number"
                  min="10.0"
                  max="33.0"
                  step="0.1"
                  value="22.3"
                  class="w-20 px-2.5 py-1.5 text-sm font-bold text-right text-white bg-[#121212] border border-[#3a3a3a] rounded-lg focus:outline-none focus:ring-2 focus:ring-white focus:border-white"
                />
                <span class="text-xs font-medium text-neutral-400 w-12">cm³</span>
              </div>
            </div>
            <input
              id="input-g-range"
              type="range"
              min="10.0"
              max="33.0"
              step="0.1"
              value="22.3"
              class="alveo-slider"
              aria-label="Deslizador de índice de hinchamiento G"
            />
            <div class="flex justify-between text-[11px] text-neutral-400">
              <span>10.0 (L ≈ 20 mm)</span>
              <span>G = 2.226·√L</span>
              <span>33.0 (L ≈ 220 mm)</span>
            </div>
          </div>

          <!-- 5. P/L -->
          <div id="ctrl-wrap-PL" class="space-y-2 pt-3 border-t border-[#1f1f1f] rounded-xl transition-colors">
            <div class="flex items-center justify-between gap-2">
              <label for="input-pl-num" class="text-sm font-semibold text-white">
                P/L — Relación de configuración
              </label>
              <div class="flex items-center gap-1.5">
                <input
                  id="input-pl-num"
                  type="number"
                  min="0.15"
                  max="4.00"
                  step="0.01"
                  value="1.05"
                  class="w-20 px-2.5 py-1.5 text-sm font-bold text-right text-white bg-[#121212] border border-[#3a3a3a] rounded-lg focus:outline-none focus:ring-2 focus:ring-white focus:border-white"
                />
                <span class="text-xs font-medium text-neutral-400 w-12">ratio</span>
              </div>
            </div>
            <input
              id="input-pl-range"
              type="range"
              min="0.15"
              max="4.00"
              step="0.01"
              value="1.05"
              class="alveo-slider"
              aria-label="Deslizador de relación P/L"
            />
            <div class="flex justify-between text-[11px] text-neutral-400">
              <span>0.15 (Extensible)</span>
              <span>P/L = P ÷ L</span>
              <span>4.00 (Tenaz)</span>
            </div>
          </div>

          <!-- Nota técnica sobre integración y conversión -->
          <div class="pt-3 border-t border-[#1f1f1f] text-xs text-neutral-300 space-y-1.5">
            <div class="font-poster text-lg text-white tracking-wide">Acoplamiento Reológico</div>
            <p>
              • <strong class="text-white">G ↔ L:</strong> <code class="font-mono text-sky-300">G = 2.226 × √L</code> y <code class="font-mono text-sky-300">L = (G / 2.226)²</code>.
            </p>
            <p>
              • <strong class="text-white">P/L ↔ P, L:</strong> <code class="font-mono text-indigo-300">P/L = P / L</code>. Ajustar <code class="font-mono text-white">W</code> adapta el perfil de energía de la curva integrando con Simpson (1.000 pasos) y <code class="font-mono text-emerald-300">k = 0.062</code>.
            </p>
          </div>
        </section>

        <!-- Columna derecha: Simulación gráfica + Parámetros derivados en vivo -->
        <section aria-label="Curva simulada y parámetros derivados" class="lg:col-span-7 space-y-6">
          <!-- Tarjetas de parámetros derivados y seguimiento desde la curva -->
          <div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">
            <!-- P200 -->
            <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-4 flex flex-col justify-between">
              <div class="flex items-center justify-between">
                <span class="text-xs font-semibold uppercase tracking-wider text-neutral-300">P200 (200 cm³)</span>
                <span class="text-[11px] font-medium px-2 py-0.5 rounded-md bg-amber-950/80 text-amber-300 border border-amber-500/40">
                  x = 40 mm
                </span>
              </div>
              <div id="metric-p200" class="font-poster text-3xl sm:text-4xl text-white tracking-wide mt-2">
                60.3 mmH₂O
              </div>
              <p id="metric-p200-sub" class="text-xs text-neutral-400 mt-1">
                Presión en la curva a 40 mm
              </p>
            </div>

            <!-- G -->
            <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-4 flex flex-col justify-between">
              <div class="flex items-center justify-between">
                <span class="text-xs font-semibold uppercase tracking-wider text-neutral-300">G (Curva / L)</span>
                <span class="text-[11px] font-medium px-2 py-0.5 rounded-md bg-sky-950/80 text-sky-300 border border-sky-500/40">
                  2.226·√L
                </span>
              </div>
              <div id="metric-g" class="font-poster text-3xl sm:text-4xl text-white tracking-wide mt-2">
                22.3
              </div>
              <p id="metric-g-sub" class="text-xs text-neutral-400 mt-1">
                Índice de hinchamiento de burbuja
              </p>
            </div>

            <!-- P/L -->
            <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-4 flex flex-col justify-between">
              <div class="flex items-center justify-between">
                <span class="text-xs font-semibold uppercase tracking-wider text-neutral-300">P/L (Curva)</span>
                <span class="text-[11px] font-medium px-2 py-0.5 rounded-md bg-indigo-950/80 text-indigo-300 border border-indigo-500/40">
                  P / L
                </span>
              </div>
              <div id="metric-pl" class="font-poster text-3xl sm:text-4xl text-white tracking-wide mt-2">
                1.05
              </div>
              <p id="metric-pl-sub" class="text-xs text-neutral-400 mt-1">
                Relación tenacidad / extensibilidad
              </p>
            </div>

            <!-- W -->
            <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-4 flex flex-col justify-between">
              <div class="flex items-center justify-between">
                <span class="text-xs font-semibold uppercase tracking-wider text-neutral-300">W (Área Curva)</span>
                <span class="text-[11px] font-medium px-2 py-0.5 rounded-md bg-emerald-950/80 text-emerald-300 border border-emerald-500/40">
                  10⁻⁴ J
                </span>
              </div>
              <div id="metric-w" class="font-poster text-3xl sm:text-4xl text-emerald-300 tracking-wide mt-2">
                405
              </div>
              <p id="metric-w-sub" class="text-xs text-neutral-400 mt-1">
                Área integrada: 0 mm·mmH₂O
              </p>
            </div>
          </div>

          <!-- Gráfica reológica interactiva del alveógrafo Chopin -->
          <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-4 sm:p-5 space-y-4">
            <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2 border-b border-[#1f1f1f] pb-3">
              <div>
                <h2 class="font-poster text-2xl text-white tracking-wide">
                  Curva Alveográfica Simulada (Chopin)
                </h2>
                <p class="text-xs text-neutral-400">
                  Pasa el cursor o desliza el dedo sobre la curva para inspeccionar presión y extensión en cualquier punto.
                </p>
              </div>
              <div class="flex flex-wrap items-center gap-3 text-[11px] text-neutral-300">
                <span class="inline-flex items-center gap-1.5">
                  <span class="w-3 h-1 rounded-full bg-emerald-400 inline-block"></span>
                  Curva y(x) / Área W
                </span>
                <span class="inline-flex items-center gap-1.5">
                  <span class="w-2.5 h-2.5 rounded-full bg-amber-400 inline-block"></span>
                  P200 (40 mm)
                </span>
                <span class="inline-flex items-center gap-1.5">
                  <span class="w-2.5 h-2.5 rounded-full bg-rose-500 inline-block"></span>
                  Rotura L
                </span>
              </div>
            </div>

            <div class="relative w-full h-72 sm:h-80 md:h-96 rounded-xl overflow-hidden border border-[#222222] bg-[#050505]">
              <canvas
                id="alveo-canvas"
                class="w-full h-full block cursor-crosshair touch-none"
                aria-label="Gráfico interactivo de la curva del alveógrafo Chopin"
              ></canvas>
              <div
                id="curve-tooltip"
                class="hidden pointer-events-none absolute z-10 px-3 py-2 rounded-lg bg-[#141414]/95 border border-[#3f3f46] text-xs text-white shadow-xl"
              ></div>
            </div>
          </div>

          <!-- Idoneidad de producto y consejos técnicos de mejora -->
          <div class="grid grid-cols-1 xl:grid-cols-2 gap-6">
            <!-- Idoneidad de producto -->
            <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-5 space-y-4">
              <div class="border-b border-[#1f1f1f] pb-3">
                <h2 class="font-poster text-2xl text-white tracking-wide">Idoneidad de Producto</h2>
                <p class="text-xs text-neutral-400 mt-0.5">
                  Evaluación orientativa según fuerza <span class="text-white font-medium">W</span>, tenacidad <span class="text-white font-medium">P</span>, extensión <span class="text-white font-medium">L</span> y relación <span class="text-white font-medium">P/L</span>.
                </p>
              </div>
              <div id="suitability-list" class="space-y-3">
                <!-- Tarjetas de idoneidad generadas dinámicamente -->
              </div>
            </div>

            <!-- Consejos técnicos de mejora -->
            <div class="bg-[#0b0b0b] rounded-2xl border border-[#262626] p-5 space-y-4">
              <div class="border-b border-[#1f1f1f] pb-3">
                <h2 class="font-poster text-2xl text-white tracking-wide">Consejos Técnicos de Mejora</h2>
                <p class="text-xs text-neutral-400 mt-0.5">
                  Recomendaciones prácticas de panificación y molinería para ajustar el comportamiento de la masa.
                </p>
              </div>
              <div id="advice-list" class="space-y-3">
                <!-- Recomendaciones generadas dinámicamente -->
              </div>
            </div>
          </div>
        </section>
      </div>
    </main>

    <!-- Pie de página -->
    <footer class="bg-[#0b0b0b] border-t border-[#262626] mt-8">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 text-xs text-neutral-400 flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
        <span class="text-neutral-300 font-medium">Pocho Lab - Reología · Simulador de Alveógrafo Chopin.</span>
        <span>Los valores de laboratorio reales pueden variar según humedad, granulometría y protocolo de hidratación.</span>
      </div>
    </footer>
  </div>

  <script>
    /**
     * =========================================================================
     * CONFIGURACIÓN Y CONSTANTES DEL SIMULADOR REOLÓGICO CHOPIN
     * =========================================================================
     */
    const RANGES = {
      W: { min: 50, max: 800, defaultVal: 405 },
      P: { min: 25, max: 160, defaultVal: 105 },
      L: { min: 20, max: 220, defaultVal: 100 },
      G: { min: 10.0, max: 33.0, defaultVal: 22.3 },
      PL: { min: 0.15, max: 4.00, defaultVal: 1.05 }
    };

    // Factor de calibración desde el área integrada (mm * mmH2O) a W (en 10^-4 J)
    const WORK_CONVERSION_FACTOR = 0.062;
    const WORK_GAIN_BOUNDS = { min: 0.35, max: 2.6 };

    // Preajustes rápidos calibrados con los 5 parámetros (W, P, L, G, P/L)
    const PRESETS = [
      {
        id: 'ficha-referencia',
        name: 'Ficha de referencia',
        badge: 'W ~405 · P/L 1.05 · G ~22.3',
        summary: 'P 105 · L 100',
        P: 105,
        L: 100,
        W: null
      },
      {
        id: 'pan-corriente',
        name: 'Pan corriente',
        badge: 'P/L ~0.69',
        summary: 'P 72 · L 105',
        P: 72,
        L: 105,
        W: null
      },
      {
        id: 'alta-fuerza-masa-madre',
        name: 'Alta fuerza / Masa madre',
        badge: 'Panes estructurados',
        summary: 'P 118 · L 150',
        P: 118,
        L: 150,
        W: null
      },
      {
        id: 'bolleria-pizza',
        name: 'Bollería / pizza',
        badge: 'Alta extensibilidad',
        summary: 'P 78 · L 145',
        P: 78,
        L: 145,
        W: null
      },
      {
        id: 'galletas-reposteria',
        name: 'Galletas',
        badge: 'Baja tenacidad',
        summary: 'P 36 · L 115',
        P: 36,
        L: 115,
        W: null
      }
    ];

    // Estado central de la aplicación (P y L gobiernan la geometría base; workGain ajusta energía W)
    const state = {
      P: 105,
      L: 100,
      workGain: 1.0,
      activePresetId: 'ficha-referencia',
      lastChangedParam: 'P',
      hoverXmm: null,
      pointerPos: null
    };

    /**
     * =========================================================================
     * FUNCIONES DE VALIDACIÓN Y SIMULACIÓN DE LA CURVA ALVEOGRÁFICA
     * =========================================================================
     */

    function clampValue(value, min, max, fallback) {
      const num = Number(value);
      if (!Number.isFinite(num)) return fallback;
      return Math.min(max, Math.max(min, num));
    }

    function computeGFromL(L) {
      return Number((2.226 * Math.sqrt(Math.max(0, L))).toFixed(1));
    }

    function computeLFromG(G) {
      const rawL = Math.pow(G / 2.226, 2);
      return Math.round(clampValue(rawL, RANGES.L.min, RANGES.L.max, RANGES.L.defaultVal));
    }

    function computePLFromPAndL(P, L) {
      if (L <= 0) return 0;
      return Number((P / L).toFixed(2));
    }

    /**
     * Evalúa la curva simulada del alveógrafo Chopin y(x) en mmH2O para una abscisa x (en mm),
     * dado el estado actual (P, L) y el factor de ajuste energético workGain.
     */
    function evaluateCurveAtX(x, P, L, workGain = 1.0) {
      if (x < 0 || x > L || L <= 0) return 0;

      // Posición de abscisa del pico de presión inicial (en mm)
      const xPeak = Math.min(10, Math.max(2.5, 0.22 * L));
      let baseY = 0;

      // Tramo 1: Ascenso rápido desde base hasta el pico P en x = xPeak
      if (x <= xPeak) {
        const u = x / xPeak;
        const baseRatio = 0.25;
        const smoothRise = 1 - Math.pow(1 - u, 2);
        baseY = P * (baseRatio + (1 - baseRatio) * smoothRise);
      } else {
        const has40mm = L > 40;
        const xMid = has40mm ? 40 : Math.max(xPeak + 0.5, 0.82 * L);
        const extFactor = Math.min(1, L / 140);
        const yMid = has40mm
          ? P * (0.46 + 0.16 * extFactor)
          : P * (0.42 + 0.14 * extFactor) * (1 - 0.12 * (1 - L / 40));

        // Tramo 2: Desde xPeak hasta xMid
        if (x <= xMid) {
          const span = Math.max(0.001, xMid - xPeak);
          const t = (x - xPeak) / span;
          const factor = Math.pow(t, 1.25);
          baseY = P - (P - yMid) * factor;
        } else {
          // Tramo 3: Desde xMid hasta L (extensión de la burbuja y rotura final)
          const remainingSpan = Math.max(0.001, L - xMid);
          const r = (x - xMid) / remainingSpan;

          const ruptureStartR = 0.96;
          const sustainRatio = 0.45 + 0.25 * extFactor;
          const yPreRupture = Math.max(6, yMid * sustainRatio);

          if (r <= ruptureStartR) {
            const s = r / ruptureStartR;
            baseY = yMid - (yMid - yPreRupture) * Math.pow(s, 1.15);
          } else {
            const dropProgress = (r - ruptureStartR) / (1 - ruptureStartR);
            baseY = Math.max(0, yPreRupture * (1 - Math.pow(dropProgress, 1.8)));
          }
        }
      }

      // Aplicar ajuste de energía de deformación W sin alterar la rotura final en x = L
      const ruptureFade = x > 0.96 * L ? Math.max(0, (L - x) / Math.max(0.001, 0.04 * L)) : 1;
      const effectiveGain = 1 + (workGain - 1) * Math.min(1, ruptureFade);
      return Math.max(0, baseY * effectiveGain);
    }

    /**
     * Integra y(x) de 0 a L mediante regla de Simpson compuesta.
     */
    function integrateCurveArea(P, L, workGain = 1.0) {
      const n = 1000;
      const h = L / n;
      let sumOdd = 0;
      let sumEven = 0;

      for (let i = 1; i < n; i++) {
        const x = i * h;
        const y = evaluateCurveAtX(x, P, L, workGain);
        if (i % 2 === 1) {
          sumOdd += y;
        } else {
          sumEven += y;
        }
      }

      const y0 = evaluateCurveAtX(0, P, L, workGain);
      const yN = evaluateCurveAtX(L, P, L, workGain);
      return (h / 3) * (y0 + yN + 4 * sumOdd + 2 * sumEven);
    }

    /**
     * Calcula los parámetros derivados directamente desde la curva simulada:
     * - P200: valor en x = 40 mm si L > 40
     * - G: 2.226 * sqrt(L)
     * - P/L: P / L
     * - area y W: integrados desde la curva y(x)
     */
    function computeDerivedMetrics(P, L, workGain = 1.0) {
      const p200 = L > 40 ? evaluateCurveAtX(40, P, L, workGain) : null;
      const g = computeGFromL(L);
      const pl = computePLFromPAndL(P, L);
      const area = integrateCurveArea(P, L, workGain);
      const w = area * WORK_CONVERSION_FACTOR;

      return {
        p200,
        g,
        pl,
        area,
        w
      };
    }

    /**
     * =========================================================================
     * EVALUACIÓN DE IDONEIDAD DE PRODUCTO Y CONSEJOS TÉCNICOS
     * =========================================================================
     */

    function evaluateProductSuitability(P, L, metrics) {
      const { pl, w } = metrics;

      function scoreToStatus(score) {
        if (score >= 75) {
          return {
            label: 'Óptima',
            badgeClass: 'bg-emerald-950/90 text-emerald-300 border-emerald-500/40',
            barClass: 'bg-emerald-400'
          };
        }
        if (score >= 45) {
          return {
            label: 'Adecuada',
            badgeClass: 'bg-sky-950/90 text-sky-300 border-sky-500/40',
            barClass: 'bg-sky-400'
          };
        }
        return {
          label: 'Limitada',
          badgeClass: 'bg-amber-950/90 text-amber-300 border-amber-500/40',
          barClass: 'bg-amber-400'
        };
      }

      const products = [];

      // 1. Panificación de alta fuerza / masa madre / panes estructurados
      let forceScore = 20;
      if (w >= 310) forceScore += 35;
      else if (w >= 230) forceScore += 18;
      if (L >= 100) forceScore += 25;
      if (P >= 85 && pl >= 0.65 && pl <= 1.35) forceScore += 20;
      forceScore = Math.min(100, Math.max(10, forceScore));
      products.push({
        name: 'Panificación de alta fuerza / masa madre / panes estructurados',
        detail: 'Exige elevada fuerza (W alto), tenacidad firme y buena extensibilidad para sostener largas fermentaciones y alveolado estructurado.',
        score: forceScore,
        ...scoreToStatus(forceScore)
      });

      // 2. Pan corriente
      let panScore = 30;
      if (w >= 170 && w <= 340) panScore += 30;
      else if (w > 340 && w <= 430) panScore += 20;
      if (pl >= 0.45 && pl <= 1.15) panScore += 25;
      if (L >= 75 && L <= 150) panScore += 15;
      panScore = Math.min(100, Math.max(10, panScore));
      products.push({
        name: 'Pan corriente',
        detail: 'Requiere tenacidad moderada-alta y balance equilibrado entre P y L para un desarrollo uniforme en horno.',
        score: panScore,
        ...scoreToStatus(panScore)
      });

      // 3. Bollería / pizza
      let pastryPizzaScore = 25;
      if (w >= 220 && w <= 400) pastryPizzaScore += 25;
      if (L >= 105) pastryPizzaScore += 30;
      if (pl <= 1.0) pastryPizzaScore += 20;
      pastryPizzaScore = Math.min(100, Math.max(10, pastryPizzaScore));
      products.push({
        name: 'Bollería / pizza',
        detail: 'Favorece buena extensibilidad (L) y relación P/L contenida para laminar o estirar sin retracción excesiva.',
        score: pastryPizzaScore,
        ...scoreToStatus(pastryPizzaScore)
      });

      // 4. Galletas
      let cookieScore = 20;
      if (w <= 180) cookieScore += 35;
      else if (w <= 240) cookieScore += 15;
      if (P <= 55) cookieScore += 30;
      if (pl <= 0.65) cookieScore += 15;
      cookieScore = Math.min(100, Math.max(10, cookieScore));
      products.push({
        name: 'Galletas',
        detail: 'Prefiere harinas flojas de baja tenacidad (P) y menor fuerza (W) para conservar dimensiones y evitar encogimiento.',
        score: cookieScore,
        ...scoreToStatus(cookieScore)
      });

      return products;
    }

    function generateTechnicalAdvice(P, L, metrics) {
      const { pl, w, p200 } = metrics;
      const tips = [];

      // Hidratación
      if (P >= 110 || pl > 1.15) {
        tips.push({
          category: 'Hidratación',
          tagClass: 'bg-sky-950/90 text-sky-300 border-sky-500/40',
          text: `Con P = ${P} mmH₂O y P/L = ${pl.toFixed(2)}, la masa muestra tenacidad notable. Incrementar la hidratación (+2% a +4% según absorción real) ayuda a reducir P y ganar extensibilidad.`
        });
      } else if (P <= 45 && pl < 0.45) {
        tips.push({
          category: 'Hidratación',
          tagClass: 'bg-sky-950/90 text-sky-300 border-sky-500/40',
          text: `Con P bajo (${P} mmH₂O), controla el porcentaje de agua para no debilitar la red gluténica ni dificultar el manejo en mesa.`
        });
      } else {
        tips.push({
          category: 'Hidratación',
          tagClass: 'bg-sky-950/90 text-sky-300 border-sky-500/40',
          text: `Ajusta la hidratación al nivel de fuerza (W ≈ ${Math.round(w)} × 10⁻⁴ J) incorporando el agua de forma gradual hasta lograr una masa homogénea.`
        });
      }

      // Amasado y reposo
      if (pl > 1.25 && L < 90) {
        tips.push({
          category: 'Amasado y Reposo',
          tagClass: 'bg-purple-950/90 text-purple-300 border-purple-500/40',
          text: `Relación P/L elevada (${pl.toFixed(2)}) con L = ${L} mm. Aplica reposo en bloque o autólisis (15–30 min) para relajar tensiones y mejorar la extensibilidad antes del formado.`
        });
      } else if (L < 65) {
        tips.push({
          category: 'Amasado y Reposo',
          tagClass: 'bg-purple-950/90 text-purple-300 border-purple-500/40',
          text: `Extensibilidad corta (L = ${L} mm). Evita sobre-amasar y vigila la temperatura final de masa (24–26 °C) para reducir roturas durante el estirado.`
        });
      } else {
        tips.push({
          category: 'Amasado y Reposo',
          tagClass: 'bg-purple-950/90 text-purple-300 border-purple-500/40',
          text: `Desarrolla el gluten hasta punto elástico-extensible y respeta tiempos de relajación entre divisiones y boleado para evitar desgarros.`
        });
      }

      // Mejorantes / enzimas / oxidantes
      if (pl > 1.2) {
        tips.push({
          category: 'Mejorantes / Enzimas',
          tagClass: 'bg-amber-950/90 text-amber-300 border-amber-500/40',
          text: `Con P/L = ${pl.toFixed(2)}, una dosificación controlada de enzimas pentosanasas/xilanasas puede suavizar el exceso de tenacidad y ampliar la extensibilidad.`
        });
      } else if (pl < 0.5 && w >= 180) {
        tips.push({
          category: 'Oxidantes / Estructura',
          tagClass: 'bg-amber-950/90 text-amber-300 border-amber-500/40',
          text: `Con P/L bajo (${pl.toFixed(2)}), un ajuste fino de ácido ascórbico puede reforzar la tenacidad P y mejorar la retención de gas en fermentación.`
        });
      } else {
        tips.push({
          category: 'Mejorantes / Enzimas',
          tagClass: 'bg-amber-950/90 text-amber-300 border-amber-500/40',
          text: `Adapta enzimas y agentes oxidantes al perfil de fermentación y al tiempo de reposo en frío o cámara para estabilizar la burbuja.`
        });
      }

      // Mezclado de harinas
      if (w < 175) {
        tips.push({
          category: 'Mezclado de Harinas',
          tagClass: 'bg-emerald-950/90 text-emerald-300 border-emerald-500/40',
          text: `Fuerza baja (W ≈ ${Math.round(w)} × 10⁻⁴ J). Combina con un 15%–40% de harina de fuerza (W > 320) si buscas mayor volumen o fermentaciones prolongadas.`
        });
      } else if (w > 410 && pl > 1.0) {
        tips.push({
          category: 'Mezclado de Harinas',
          tagClass: 'bg-emerald-950/90 text-emerald-300 border-emerald-500/40',
          text: `Harina de alta fuerza y tenacidad (W ≈ ${Math.round(w)} × 10⁻⁴ J). Mezclar con una harina más extensible o floja permite equilibrar P/L según el producto final.`
        });
      } else {
        tips.push({
          category: 'Mezclado de Harinas',
          tagClass: 'bg-emerald-950/90 text-emerald-300 border-emerald-500/40',
          text: `Perfil consistente (W ≈ ${Math.round(w)} × 10⁻⁴ J${p200 !== null ? `, P200 ≈ ${p200.toFixed(1)} mmH₂O` : ''}), apto como base o componente de blend para estandarizar lotes.`
        });
      }

      return tips;
    }

    /**
     * =========================================================================
     * RENDERIZADO DE INTERFAZ, GRÁFICO CANVAS E INTERACCIÓN
     * =========================================================================
     */

    const dom = {
      wNum: document.getElementById('input-w-num'),
      wRange: document.getElementById('input-w-range'),
      pNum: document.getElementById('input-p-num'),
      pRange: document.getElementById('input-p-range'),
      lNum: document.getElementById('input-l-num'),
      lRange: document.getElementById('input-l-range'),
      gNum: document.getElementById('input-g-num'),
      gRange: document.getElementById('input-g-range'),
      plNum: document.getElementById('input-pl-num'),
      plRange: document.getElementById('input-pl-range'),
      syncBadge: document.getElementById('sync-badge'),
      syncDetail: document.getElementById('sync-detail'),
      presetsContainer: document.getElementById('presets-container'),
      activePresetLabel: document.getElementById('active-preset-label'),
      statusBanner: document.getElementById('status-banner'),
      canvas: document.getElementById('alveo-canvas'),
      tooltip: document.getElementById('curve-tooltip'),
      metricP200: document.getElementById('metric-p200'),
      metricP200Sub: document.getElementById('metric-p200-sub'),
      metricG: document.getElementById('metric-g'),
      metricGSub: document.getElementById('metric-g-sub'),
      metricPl: document.getElementById('metric-pl'),
      metricPlSub: document.getElementById('metric-pl-sub'),
      metricW: document.getElementById('metric-w'),
      metricWSub: document.getElementById('metric-w-sub'),
      suitabilityList: document.getElementById('suitability-list'),
      adviceList: document.getElementById('advice-list')
    };

    const CONTROL_WRAPS = {
      W: document.getElementById('ctrl-wrap-W'),
      P: document.getElementById('ctrl-wrap-P'),
      L: document.getElementById('ctrl-wrap-L'),
      G: document.getElementById('ctrl-wrap-G'),
      PL: document.getElementById('ctrl-wrap-PL')
    };

    // Referencia geométrica del último trazado en el canvas para tooltip e interacción táctil/ratón
    let chartLayout = null;

    function showStatusMessage(message) {
      if (!dom.statusBanner) return;
      if (message && message.trim().length > 0) {
        dom.statusBanner.textContent = message;
        dom.statusBanner.classList.remove('hidden');
      } else {
        dom.statusBanner.textContent = '';
        dom.statusBanner.classList.add('hidden');
      }
    }

    function renderPresetButtons() {
      if (!dom.presetsContainer) return;
      dom.presetsContainer.innerHTML = '';

      PRESETS.forEach((preset) => {
        const isActive = state.activePresetId === preset.id;
        const btn = document.createElement('button');
        btn.type = 'button';
        btn.setAttribute('aria-pressed', String(isActive));
        btn.className = isActive
          ? 'text-left p-3 rounded-xl border border-white bg-white text-[#050505] transition-all cursor-pointer space-y-1'
          : 'text-left p-3 rounded-xl border border-[#262626] bg-[#101010] hover:bg-[#181818] hover:border-[#404040] text-white transition-all cursor-pointer space-y-1';

        btn.innerHTML = `
          <div class="flex items-center justify-between gap-1.5">
            <span class="text-xs font-bold leading-snug">${preset.name}</span>
          </div>
          <div class="text-[11px] font-medium ${isActive ? 'text-neutral-800' : 'text-emerald-300'}">${preset.summary}</div>
          <div class="text-[10px] ${isActive ? 'text-neutral-700' : 'text-neutral-400'}">${preset.badge}</div>
        `;

        btn.addEventListener('click', () => applyPreset(preset.id));
        dom.presetsContainer.appendChild(btn);
      });
    }

    function applyPreset(presetId) {
      const preset = PRESETS.find((p) => p.id === presetId);
      if (!preset) return;

      state.P = preset.P;
      state.L = preset.L;
      state.workGain = 1.0;
      state.activePresetId = preset.id;
      state.lastChangedParam = 'P';

      showStatusMessage('');
      updateAll();
    }

    function syncInputsFromState(metrics) {
      const gVal = computeGFromL(state.L);
      const plVal = computePLFromPAndL(state.P, state.L);
      const wVal = Math.round(metrics.w);

      if (document.activeElement !== dom.wNum) dom.wNum.value = wVal;
      dom.wRange.value = clampValue(wVal, RANGES.W.min, RANGES.W.max, RANGES.W.defaultVal);

      if (document.activeElement !== dom.pNum) dom.pNum.value = state.P;
      dom.pRange.value = state.P;

      if (document.activeElement !== dom.lNum) dom.lNum.value = state.L;
      dom.lRange.value = state.L;

      if (document.activeElement !== dom.gNum) dom.gNum.value = gVal.toFixed(1);
      dom.gRange.value = clampValue(gVal, RANGES.G.min, RANGES.G.max, RANGES.G.defaultVal);

      if (document.activeElement !== dom.plNum) dom.plNum.value = plVal.toFixed(2);
      dom.plRange.value = clampValue(plVal, RANGES.PL.min, RANGES.PL.max, RANGES.PL.defaultVal);

      const activePreset = PRESETS.find((p) => p.id === state.activePresetId);
      dom.activePresetLabel.textContent = activePreset
        ? `Perfil activo: ${activePreset.name}`
        : 'Perfil activo: Personalizado';
    }

    function highlightSynchronizedControls(primaryKey, dependentKeys) {
      Object.entries(CONTROL_WRAPS).forEach(([key, el]) => {
        if (!el) return;
        el.classList.remove('ring-1', 'ring-emerald-400/70', 'bg-emerald-950/15', 'px-2', 'py-1.5');
        if (key === primaryKey) {
          el.classList.add('ring-1', 'ring-emerald-400/70', 'bg-emerald-950/15', 'px-2', 'py-1.5');
        }
      });

      const namesMap = {
        W: 'W',
        P: 'P',
        L: 'L',
        G: 'G',
        PL: 'P/L'
      };
      const depLabels = dependentKeys.map((k) => namesMap[k] || k).join(', ');
      dom.syncBadge.textContent = `Editando ${namesMap[primaryKey] || primaryKey}`;
      dom.syncDetail.textContent = depLabels
        ? `Sincronizados automáticamente: ${depLabels} y área integrada de la curva.`
        : 'Curva y métricas recalculadas.';
    }

    /**
     * Sincroniza los 5 parámetros controlados (W, P, L, G, PL) cuando el usuario
     * modifica cualquiera de ellos mediante slider o entrada numérica.
     */
    function handleParamChange(key, rawValue, fromNumberInput) {
      const cfg = RANGES[key];
      const parsed = Number(rawValue);

      if (fromNumberInput) {
        if (rawValue === '' || !Number.isFinite(parsed)) {
          showStatusMessage(`Introduce un valor numérico válido para ${key} (${cfg.min}–${cfg.max}).`);
          return;
        }
        if (parsed < cfg.min || parsed > cfg.max) {
          showStatusMessage(`${key} ajustado al rango permitido [${cfg.min} a ${cfg.max}].`);
        } else {
          showStatusMessage('');
        }
      } else {
        showStatusMessage('');
      }

      const clamped = clampValue(parsed, cfg.min, cfg.max, cfg.defaultVal);
      state.activePresetId = null;
      state.lastChangedParam = key;

      if (key === 'P') {
        state.P = Math.round(clamped);
        highlightSynchronizedControls('P', ['PL', 'W']);
      } else if (key === 'L') {
        state.L = Math.round(clamped);
        highlightSynchronizedControls('L', ['G', 'PL', 'W']);
      } else if (key === 'G') {
        const roundedG = Number(clamped.toFixed(1));
        state.L = computeLFromG(roundedG);
        highlightSynchronizedControls('G', ['L', 'PL', 'W']);
      } else if (key === 'PL') {
        const targetPL = Number(clamped.toFixed(2));
        const desiredP = targetPL * state.L;
        if (desiredP > RANGES.P.max) {
          state.P = RANGES.P.max;
          state.L = Math.round(clampValue(state.P / targetPL, RANGES.L.min, RANGES.L.max, state.L));
          showStatusMessage(`P limitado a ${RANGES.P.max} mmH₂O; L ajustado a ${state.L} mm para aproximar P/L = ${targetPL.toFixed(2)}.`);
          highlightSynchronizedControls('PL', ['P', 'L', 'G', 'W']);
        } else if (desiredP < RANGES.P.min) {
          state.P = RANGES.P.min;
          state.L = Math.round(clampValue(state.P / targetPL, RANGES.L.min, RANGES.L.max, state.L));
          showStatusMessage(`P limitado a ${RANGES.P.min} mmH₂O; L ajustado a ${state.L} mm para aproximar P/L = ${targetPL.toFixed(2)}.`);
          highlightSynchronizedControls('PL', ['P', 'L', 'G', 'W']);
        } else {
          state.P = Math.round(desiredP);
          highlightSynchronizedControls('PL', ['P', 'W']);
        }
      } else if (key === 'W') {
        const targetW = Math.round(clamped);
        const baseArea = integrateCurveArea(state.P, state.L, 1.0);
        const baseW = Math.max(0.01, baseArea * WORK_CONVERSION_FACTOR);
        const rawGain = targetW / baseW;
        const clampedGain = clampValue(rawGain, WORK_GAIN_BOUNDS.min, WORK_GAIN_BOUNDS.max, 1.0);
        state.workGain = clampedGain;

        const actualW = Math.round(baseW * clampedGain);
        if (actualW !== targetW) {
          showStatusMessage(`Curva ajustada a W ≈ ${actualW} (límite físico de forma para P=${state.P}, L=${state.L}; solicitado ${targetW}).`);
        }
        highlightSynchronizedControls('W', ['P200']);
      }

      updateAll();
    }

    /**
     * Dibuja la curva del alveógrafo Chopin en el elemento <canvas> de forma nítida (HiDPI)
     * y responsiva al tamaño de su contenedor.
     */
    function drawAlveographCurve(metrics) {
      const canvas = dom.canvas;
      const rect = canvas.getBoundingClientRect();
      const width = Math.max(260, rect.width);
      const height = Math.max(200, rect.height);
      const dpr = window.devicePixelRatio || 1;

      if (canvas.width !== Math.round(width * dpr) || canvas.height !== Math.round(height * dpr)) {
        canvas.width = Math.round(width * dpr);
        canvas.height = Math.round(height * dpr);
      }

      const ctx = canvas.getContext('2d');
      ctx.save();
      ctx.scale(dpr, dpr);

      // Fondo negro profundo para alto contraste
      ctx.clearRect(0, 0, width, height);
      ctx.fillStyle = '#050505';
      ctx.fillRect(0, 0, width, height);

      // Márgenes del área de trazado
      const isCompact = width < 480;
      const pad = {
        top: 24,
        right: isCompact ? 16 : 26,
        bottom: isCompact ? 42 : 48,
        left: isCompact ? 48 : 60
      };

      const plotW = Math.max(80, width - pad.left - pad.right);
      const plotH = Math.max(80, height - pad.top - pad.bottom);

      const xMax = Math.max(220, Math.ceil(state.L / 20) * 20);
      const peakVisual = Math.max(state.P, state.P * state.workGain);
      const yMax = Math.max(160, Math.ceil((peakVisual * 1.12) / 20) * 20);

      const xToPx = (xVal) => pad.left + (xVal / xMax) * plotW;
      const yToPx = (yVal) => pad.top + plotH - (yVal / yMax) * plotH;
      const pxToX = (px) => ((px - pad.left) / plotW) * xMax;

      chartLayout = {
        width,
        height,
        pad,
        plotW,
        plotH,
        xMax,
        yMax,
        xToPx,
        yToPx,
        pxToX
      };

      // Cuadrícula horizontal y etiquetas eje Y (mmH2O)
      const yStep = yMax <= 180 ? 20 : 40;
      ctx.font = `${isCompact ? 10 : 11}px Inter, sans-serif`;
      ctx.textAlign = 'right';
      ctx.textBaseline = 'middle';

      for (let yVal = 0; yVal <= yMax; yVal += yStep) {
        const py = yToPx(yVal);
        ctx.strokeStyle = yVal === 0 ? '#525252' : '#1c1c1c';
        ctx.lineWidth = yVal === 0 ? 1.3 : 0.9;
        ctx.beginPath();
        ctx.moveTo(pad.left, py);
        ctx.lineTo(pad.left + plotW, py);
        ctx.stroke();

        ctx.fillStyle = '#d4d4d4';
        ctx.fillText(String(yVal), pad.left - 8, py);
      }

      // Cuadrícula vertical y etiquetas eje X (mm)
      const xStep = isCompact ? 40 : 20;
      ctx.textAlign = 'center';
      ctx.textBaseline = 'top';

      for (let xVal = 0; xVal <= xMax; xVal += xStep) {
        const px = xToPx(xVal);
        ctx.strokeStyle = xVal === 0 ? '#525252' : '#161616';
        ctx.lineWidth = xVal === 0 ? 1.3 : 0.9;
        ctx.beginPath();
        ctx.moveTo(px, pad.top);
        ctx.lineTo(px, pad.top + plotH);
        ctx.stroke();

        if (!isCompact || xVal % 40 === 0) {
          ctx.fillStyle = '#d4d4d4';
          ctx.fillText(String(xVal), px, pad.top + plotH + 8);
        }
      }

      // Línea de referencia vertical en x = 40 mm (correspondiente a 200 cm³ / P200)
      const x40Px = xToPx(40);
      ctx.save();
      ctx.setLineDash([5, 4]);
      ctx.strokeStyle = '#fbbf24';
      ctx.lineWidth = 1.4;
      ctx.beginPath();
      ctx.moveTo(x40Px, pad.top);
      ctx.lineTo(x40Px, pad.top + plotH);
      ctx.stroke();
      ctx.restore();

      ctx.fillStyle = '#fcd34d';
      ctx.font = `600 ${isCompact ? 10 : 11}px Inter, sans-serif`;
      ctx.textAlign = 'left';
      ctx.textBaseline = 'top';
      ctx.fillText('200 cm³ (40 mm)', Math.min(pad.left + plotW - 92, x40Px + 5), pad.top + 4);

      // Muestreo de puntos de la curva desde x = 0 hasta x = L
      const samples = 240;
      const points = [];
      for (let i = 0; i <= samples; i++) {
        const xVal = (i / samples) * state.L;
        const yVal = evaluateCurveAtX(xVal, state.P, state.L, state.workGain);
        points.push({ x: xVal, y: yVal, px: xToPx(xVal), py: yToPx(yVal) });
      }

      // Sombreado del área bajo la curva (trabajo W)
      if (points.length > 1) {
        const areaGrad = ctx.createLinearGradient(0, pad.top, 0, pad.top + plotH);
        areaGrad.addColorStop(0, 'rgba(52, 211, 153, 0.28)');
        areaGrad.addColorStop(1, 'rgba(52, 211, 153, 0.03)');

        ctx.fillStyle = areaGrad;
        ctx.beginPath();
        ctx.moveTo(xToPx(0), yToPx(0));
        for (const pt of points) {
          ctx.lineTo(pt.px, pt.py);
        }
        ctx.lineTo(xToPx(state.L), yToPx(0));
        ctx.closePath();
        ctx.fill();

        // Trazo principal de la curva y(x)
        ctx.strokeStyle = '#34d399';
        ctx.lineWidth = 2.8;
        ctx.lineJoin = 'round';
        ctx.lineCap = 'round';
        ctx.beginPath();
        points.forEach((pt, idx) => {
          if (idx === 0) ctx.moveTo(pt.px, pt.py);
          else ctx.lineTo(pt.px, pt.py);
        });
        ctx.stroke();
      }

      // Marcador del pico máximo P
      const xPeak = Math.min(10, Math.max(2.5, 0.22 * state.L));
      const yPeak = evaluateCurveAtX(xPeak, state.P, state.L, state.workGain);
      const peakPx = xToPx(xPeak);
      const peakPy = yToPx(yPeak);

      ctx.fillStyle = '#ffffff';
      ctx.beginPath();
      ctx.arc(peakPx, peakPy, 4.8, 0, Math.PI * 2);
      ctx.fill();
      ctx.strokeStyle = '#10b981';
      ctx.lineWidth = 2;
      ctx.stroke();

      ctx.fillStyle = '#ffffff';
      ctx.font = `700 ${isCompact ? 11 : 12}px Inter, sans-serif`;
      ctx.textAlign = 'left';
      ctx.textBaseline = 'bottom';
      const peakLabelX = Math.min(pad.left + plotW - 84, peakPx + 8);
      const peakLabelY = Math.max(pad.top + 14, peakPy - 6);
      ctx.fillText(`P = ${state.P}`, peakLabelX, peakLabelY);

      // Marcador de P200 en x = 40 mm si L > 40
      if (state.L > 40 && metrics.p200 !== null) {
        const p200Py = yToPx(metrics.p200);
        ctx.fillStyle = '#fbbf24';
        ctx.beginPath();
        ctx.arc(x40Px, p200Py, 5.2, 0, Math.PI * 2);
        ctx.fill();
        ctx.strokeStyle = '#050505';
        ctx.lineWidth = 1.8;
        ctx.stroke();

        ctx.fillStyle = '#fde68a';
        ctx.font = `600 ${isCompact ? 10 : 11}px Inter, sans-serif`;
        ctx.textAlign = 'left';
        ctx.textBaseline = 'bottom';
        ctx.fillText(
          `P200 = ${metrics.p200.toFixed(1)}`,
          Math.min(pad.left + plotW - 92, x40Px + 8),
          Math.max(pad.top + 26, p200Py - 6)
        );
      }

      // Marcador de rotura en x = L
      const endPx = xToPx(state.L);
      const endPy = yToPx(0);
      ctx.fillStyle = '#f43f5e';
      ctx.beginPath();
      ctx.arc(endPx, endPy, 4.8, 0, Math.PI * 2);
      ctx.fill();
      ctx.strokeStyle = '#ffffff';
      ctx.lineWidth = 1.6;
      ctx.stroke();

      ctx.fillStyle = '#fda4af';
      ctx.font = `700 ${isCompact ? 10 : 11}px Inter, sans-serif`;
      ctx.textAlign = endPx > pad.left + plotW - 45 ? 'right' : 'left';
      ctx.textBaseline = 'bottom';
      ctx.fillText(`L = ${state.L}`, endPx, Math.max(pad.top + 14, endPy - 8));

      // Guía interactiva si el usuario está inspeccionando el gráfico
      if (state.hoverXmm !== null && state.hoverXmm >= 0 && state.hoverXmm <= xMax) {
        const hoverPx = xToPx(state.hoverXmm);
        const hoverY = evaluateCurveAtX(state.hoverXmm, state.P, state.L, state.workGain);
        const hoverPy = yToPx(hoverY);

        ctx.save();
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 1;
        ctx.setLineDash([3, 3]);
        ctx.beginPath();
        ctx.moveTo(hoverPx, pad.top);
        ctx.lineTo(hoverPx, pad.top + plotH);
        ctx.stroke();
        ctx.restore();

        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(hoverPx, hoverPy, 5, 0, Math.PI * 2);
        ctx.fill();
        ctx.strokeStyle = '#050505';
        ctx.lineWidth = 1.8;
        ctx.stroke();
      }

      // Etiquetas de los ejes
      ctx.fillStyle = '#ffffff';
      ctx.font = `600 ${isCompact ? 11 : 12}px Inter, sans-serif`;
      ctx.textAlign = 'center';
      ctx.textBaseline = 'bottom';
      ctx.fillText('Extensión / Abscisa x (mm)', pad.left + plotW / 2, height - 4);

      ctx.save();
      ctx.translate(14, pad.top + plotH / 2);
      ctx.rotate(-Math.PI / 2);
      ctx.textAlign = 'center';
      ctx.textBaseline = 'top';
      ctx.fillText('Presión (mmH₂O)', 0, 0);
      ctx.restore();

      ctx.restore();
    }

    function renderMetrics(metrics) {
      if (state.L > 40 && metrics.p200 !== null) {
        dom.metricP200.textContent = `${metrics.p200.toFixed(1)} mmH₂O`;
        dom.metricP200.className = 'font-poster text-3xl sm:text-4xl text-white tracking-wide mt-2';
        const pctOfP = state.P > 0 ? Math.round((metrics.p200 / state.P) * 100) : 0;
        dom.metricP200Sub.textContent = `Equivale al ${pctOfP}% de P (${state.P} mmH₂O)`;
      } else {
        dom.metricP200.textContent = 'N/A — ROTURA < 40 MM';
        dom.metricP200.className = 'font-poster text-xl sm:text-2xl text-amber-300 tracking-wide mt-2';
        dom.metricP200Sub.textContent = `La masa se rompe en L = ${state.L} mm antes de 40 mm`;
      }

      dom.metricG.textContent = metrics.g.toFixed(1);
      dom.metricGSub.textContent = `Calculado desde L = ${state.L} mm`;

      dom.metricPl.textContent = metrics.pl.toFixed(2);
      if (metrics.pl < 0.55) {
        dom.metricPlSub.textContent = 'Perfil extensible / tenacidad moderada';
      } else if (metrics.pl <= 1.25) {
        dom.metricPlSub.textContent = 'Perfil equilibrado de tenacidad y extensión';
      } else {
        dom.metricPlSub.textContent = 'Perfil tenaz / predominio de P sobre L';
      }

      dom.metricW.textContent = Math.round(metrics.w).toString();
      dom.metricWSub.textContent = `Área curva: ${Math.round(metrics.area).toLocaleString('es-ES')} mm·mmH₂O`;
    }

    function renderSuitability(metrics) {
      const items = evaluateProductSuitability(state.P, state.L, metrics);
      dom.suitabilityList.innerHTML = '';

      items.forEach((item) => {
        const card = document.createElement('div');
        card.className = 'p-3.5 rounded-xl border border-[#242424] bg-[#101010] space-y-2';
        card.innerHTML = `
          <div class="flex items-center justify-between gap-2">
            <h3 class="text-sm font-semibold text-white">${item.name}</h3>
            <span class="text-xs font-semibold px-2.5 py-0.5 rounded-full border ${item.badgeClass}">
              ${item.label} (${item.score}%)
            </span>
          </div>
          <p class="text-xs text-neutral-300 leading-relaxed">${item.detail}</p>
          <div class="w-full h-1.5 bg-[#242424] rounded-full overflow-hidden">
            <div class="h-full ${item.barClass} rounded-full transition-all duration-300" style="width: ${item.score}%"></div>
          </div>
        `;
        dom.suitabilityList.appendChild(card);
      });
    }

    function renderAdvice(metrics) {
      const tips = generateTechnicalAdvice(state.P, state.L, metrics);
      dom.adviceList.innerHTML = '';

      tips.forEach((tip) => {
        const card = document.createElement('div');
        card.className = 'p-3.5 rounded-xl border border-[#242424] bg-[#101010] space-y-1.5';
        card.innerHTML = `
          <div class="flex items-center justify-between gap-2">
            <span class="text-xs font-bold px-2.5 py-0.5 rounded-md border ${tip.tagClass}">
              ${tip.category}
            </span>
          </div>
          <p class="text-xs text-neutral-200 leading-relaxed">${tip.text}</p>
        `;
        dom.adviceList.appendChild(card);
      });
    }

    function updateAll() {
      try {
        const metrics = computeDerivedMetrics(state.P, state.L, state.workGain);
        syncInputsFromState(metrics);
        renderPresetButtons();
        renderMetrics(metrics);
        drawAlveographCurve(metrics);
        renderSuitability(metrics);
        renderAdvice(metrics);
      } catch (err) {
        showStatusMessage('Error al recalcular la simulación reológica. Revisa los valores introducidos.');
        console.error('Error en simulación reológica:', err);
      }
    }

    /**
     * =========================================================================
     * EVENTOS DE INTERACCIÓN (SLIDERS, INPUTS, HOVER/TOUCH EN CANVAS, RESIZE)
     * =========================================================================
     */

    function updateTooltipFromClientPoint(clientX, clientY) {
      if (!chartLayout) return;
      const rect = dom.canvas.getBoundingClientRect();
      const px = clientX - rect.left;
      const py = clientY - rect.top;

      const { pad, plotW, plotH, xMax, pxToX, width, height } = chartLayout;
      if (px < pad.left || px > pad.left + plotW || py < pad.top - 10 || py > pad.top + plotH + 10) {
        clearTooltip();
        return;
      }

      const xMm = Math.min(xMax, Math.max(0, pxToX(px)));
      const inCurve = xMm <= state.L;
      const yVal = inCurve ? evaluateCurveAtX(xMm, state.P, state.L, state.workGain) : 0;

      state.hoverXmm = xMm;
      const metrics = computeDerivedMetrics(state.P, state.L, state.workGain);
      drawAlveographCurve(metrics);

      dom.tooltip.innerHTML = `
        <div class="font-semibold text-emerald-300">x = ${xMm.toFixed(1)} mm</div>
        <div>Presión: <span class="font-bold">${yVal.toFixed(1)} mmH₂O</span></div>
        <div class="text-[11px] text-slate-300">
          ${inCurve ? 'Dentro de extensibilidad L' : `Más allá de rotura (L = ${state.L} mm)`}
        </div>
      `;
      dom.tooltip.classList.remove('hidden');

      const tooltipW = dom.tooltip.offsetWidth || 150;
      const tooltipH = dom.tooltip.offsetHeight || 60;
      let leftPos = px + 12;
      if (leftPos + tooltipW > width - 8) {
        leftPos = Math.max(8, px - tooltipW - 12);
      }
      let topPos = py - tooltipH - 10;
      if (topPos < 8) {
        topPos = Math.min(height - tooltipH - 8, py + 14);
      }

      dom.tooltip.style.left = `${leftPos}px`;
      dom.tooltip.style.top = `${topPos}px`;
    }

    function clearTooltip() {
      if (state.hoverXmm !== null) {
        state.hoverXmm = null;
        dom.tooltip.classList.add('hidden');
        const metrics = computeDerivedMetrics(state.P, state.L);
        drawAlveographCurve(metrics);
      }
    }

    function bindEvents() {
      // W
      dom.wRange.addEventListener('input', (e) => handleParamChange('W', e.target.value, false));
      dom.wNum.addEventListener('input', (e) => handleParamChange('W', e.target.value, true));
      dom.wNum.addEventListener('blur', (e) => {
        handleParamChange('W', e.target.value, true);
        if (document.activeElement !== dom.wNum) {
          const metrics = computeDerivedMetrics(state.P, state.L, state.workGain);
          dom.wNum.value = Math.round(metrics.w);
        }
      });

      // P
      dom.pRange.addEventListener('input', (e) => handleParamChange('P', e.target.value, false));
      dom.pNum.addEventListener('input', (e) => handleParamChange('P', e.target.value, true));
      dom.pNum.addEventListener('blur', (e) => {
        const clamped = Math.round(clampValue(e.target.value, RANGES.P.min, RANGES.P.max, state.P));
        state.P = clamped;
        showStatusMessage('');
        updateAll();
      });

      // L
      dom.lRange.addEventListener('input', (e) => handleParamChange('L', e.target.value, false));
      dom.lNum.addEventListener('input', (e) => handleParamChange('L', e.target.value, true));
      dom.lNum.addEventListener('blur', (e) => {
        const clamped = Math.round(clampValue(e.target.value, RANGES.L.min, RANGES.L.max, state.L));
        state.L = clamped;
        showStatusMessage('');
        updateAll();
      });

      // G
      dom.gRange.addEventListener('input', (e) => handleParamChange('G', e.target.value, false));
      dom.gNum.addEventListener('input', (e) => handleParamChange('G', e.target.value, true));
      dom.gNum.addEventListener('blur', (e) => {
        const clamped = clampValue(e.target.value, RANGES.G.min, RANGES.G.max, computeGFromL(state.L));
        state.L = computeLFromG(Number(clamped.toFixed(1)));
        showStatusMessage('');
        updateAll();
      });

      // P/L
      dom.plRange.addEventListener('input', (e) => handleParamChange('PL', e.target.value, false));
      dom.plNum.addEventListener('input', (e) => handleParamChange('PL', e.target.value, true));
      dom.plNum.addEventListener('blur', (e) => {
        handleParamChange('PL', e.target.value, true);
        if (document.activeElement !== dom.plNum) {
          dom.plNum.value = computePLFromPAndL(state.P, state.L).toFixed(2);
        }
      });

      // Ratón y táctil sobre el canvas
      dom.canvas.addEventListener('mousemove', (e) => {
        updateTooltipFromClientPoint(e.clientX, e.clientY);
      });
      dom.canvas.addEventListener('mouseleave', clearTooltip);

      dom.canvas.addEventListener(
        'touchstart',
        (e) => {
          if (e.touches.length > 0) {
            const t = e.touches[0];
            updateTooltipFromClientPoint(t.clientX, t.clientY);
          }
        },
        { passive: true }
      );

      dom.canvas.addEventListener(
        'touchmove',
        (e) => {
          if (e.touches.length > 0) {
            const t = e.touches[0];
            updateTooltipFromClientPoint(t.clientX, t.clientY);
          }
        },
        { passive: true }
      );

      dom.canvas.addEventListener('touchend', clearTooltip);
      dom.canvas.addEventListener('touchcancel', clearTooltip);

      // Redimensionamiento responsivo
      window.addEventListener('resize', () => {
        const metrics = computeDerivedMetrics(state.P, state.L, state.workGain);
        drawAlveographCurve(metrics);
      });
    }

    // Inicialización al cargar la página
    window.addEventListener('DOMContentLoaded', () => {
      bindEvents();
      updateAll();
    });
  </script>
</body>
</html>