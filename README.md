<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Broadcast Audio Processor Pro</title>
<style>
/* ======================== RESET & BASE ======================== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --bg-dark: #0a0a0f;
  --bg-panel: #12121a;
  --bg-panel-light: #1a1a25;
  --bg-control: #0d0d14;
  --border: #2a2a3a;
  --border-light: #3a3a4a;
  --text: #e0e0e8;
  --text-dim: #888899;
  --accent: #00d4ff;
  --accent-glow: rgba(0, 212, 255, 0.3);
  --green: #00ff88;
  --green-glow: rgba(0, 255, 136, 0.3);
  --yellow: #ffcc00;
  --red: #ff3344;
  --red-glow: rgba(255, 51, 68, 0.3);
  --orange: #ff8800;
  --knob-size: 60px;
}
body {
  background: var(--bg-dark);
  color: var(--text);
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  overflow-x: hidden;
  min-height: 100vh;
}
/* ======================== SCROLLBAR ======================== */
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: var(--bg-dark); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: var(--border-light); }

/* ======================== HEADER ======================== */
.header {
  background: linear-gradient(180deg, #15152a 0%, var(--bg-dark) 100%);
  border-bottom: 1px solid var(--border);
  padding: 12px 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: sticky;
  top: 0;
  z-index: 100;
}
.logo {
  display: flex;
  align-items: center;
  gap: 12px;
}
.logo svg { filter: drop-shadow(0 0 8px var(--accent-glow)); }
.logo h1 {
  font-size: 1.2rem;
  font-weight: 700;
  background: linear-gradient(135deg, var(--accent), #8866ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  letter-spacing: 1px;
}
.logo span {
  font-size: 0.65rem;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 2px;
}
.header-controls {
  display: flex;
  align-items: center;
  gap: 16px;
}

/* ======================== POWER BUTTON ======================== */
.power-btn {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: 2px solid var(--red);
  background: rgba(255, 51, 68, 0.1);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
}
.power-btn:hover { box-shadow: 0 0 20px var(--red-glow); }
.power-btn.active {
  border-color: var(--green);
  background: rgba(0, 255, 136, 0.1);
  box-shadow: 0 0 20px var(--green-glow);
}
.power-btn svg { width: 24px; height: 24px; }
.power-btn.active svg { color: var(--green); }

/* ======================== MAIN LAYOUT ======================== */
.main-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 16px;
}
.top-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  margin-bottom: 16px;
}
.bottom-section {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  margin-bottom: 16px;
}
.full-section {
  margin-bottom: 16px;
}

/* ======================== PANELS ======================== */
.panel {
  background: var(--bg-panel);
  border: 1px solid var(--border);
  border-radius: 12px;
  overflow: hidden;
}
.panel-header {
  background: linear-gradient(180deg, var(--bg-panel-light) 0%, var(--bg-panel) 100%);
  padding: 10px 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid var(--border);
}
.panel-title {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--accent);
  font-weight: 600;
}
.panel-badge {
  font-size: 0.6rem;
  padding: 2px 8px;
  border-radius: 10px;
  background: rgba(0, 212, 255, 0.15);
  color: var(--accent);
  border: 1px solid rgba(0, 212, 255, 0.3);
}
.panel-body {
  padding: 16px;
}

/* ======================== BYPASS TOGGLE ======================== */
.bypass-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
}
.bypass-toggle label {
  font-size: 0.65rem;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 1px;
}
.toggle-switch {
  position: relative;
  width: 36px;
  height: 18px;
  cursor: pointer;
}
.toggle-switch input { display: none; }
.toggle-slider {
  position: absolute;
  inset: 0;
  background: #333;
  border-radius: 9px;
  transition: 0.3s;
}
.toggle-slider::before {
  content: '';
  position: absolute;
  width: 14px;
  height: 14px;
  left: 2px;
  top: 2px;
  background: #666;
  border-radius: 50%;
  transition: 0.3s;
}
.toggle-switch input:checked + .toggle-slider {
  background: rgba(0, 255, 136, 0.3);
}
.toggle-switch input:checked + .toggle-slider::before {
  transform: translateX(18px);
  background: var(--green);
  box-shadow: 0 0 6px var(--green-glow);
}

/* ======================== INPUT/OUTPUT SECTION ======================== */
.io-section {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 16px;
}
.io-group label {
  display: block;
  font-size: 0.65rem;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 6px;
}
.io-group select, .io-group input[type="range"] {
  width: 100%;
}
.io-group select {
  background: var(--bg-control);
  color: var(--text);
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 8px 10px;
  font-size: 0.8rem;
  outline: none;
  cursor: pointer;
}
.io-group select:focus { border-color: var(--accent); }

/* ======================== RANGE SLIDER ======================== */
input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  height: 6px;
  background: var(--bg-control);
  border-radius: 3px;
  outline: none;
  cursor: pointer;
}
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--accent);
  border: 2px solid #fff;
  box-shadow: 0 0 8px var(--accent-glow);
  cursor: pointer;
}
input[type="range"].green::-webkit-slider-thumb {
  background: var(--green);
  box-shadow: 0 0 8px var(--green-glow);
}
input[type="range"].red::-webkit-slider-thumb {
  background: var(--red);
  box-shadow: 0 0 8px var(--red-glow);
}
.range-value {
  text-align: center;
  font-size: 0.75rem;
  color: var(--accent);
  margin-top: 4px;
  font-family: 'Courier New', monospace;
  font-weight: 600;
}

/* ======================== VU METERS ======================== */
.meter-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}
.meter-container canvas {
  border-radius: 4px;
  border: 1px solid var(--border);
}
.meter-label {
  font-size: 0.6rem;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 1px;
}
.meter-db {
  font-size: 0.7rem;
  font-family: 'Courier New', monospace;
  color: var(--green);
  font-weight: 600;
}

/* ======================== KNOB CONTROL ======================== */
.knob-control {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
}
.knob-container {
  position: relative;
  width: var(--knob-size);
  height: var(--knob-size);
}
.knob-svg {
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
}
.knob-bg {
  fill: none;
  stroke: var(--bg-control);
  stroke-width: 4;
}
.knob-fill {
  fill: none;
  stroke: var(--accent);
  stroke-width: 4;
  stroke-linecap: round;
  transition: stroke-dashoffset 0.1s;
  filter: drop-shadow(0 0 4px var(--accent-glow));
}
.knob-fill.green {
  stroke: var(--green);
  filter: drop-shadow(0 0 4px var(--green-glow));
}
.knob-fill.red {
  stroke: var(--red);
  filter: drop-shadow(0 0 4px var(--red-glow));
}
.knob-fill.orange {
  stroke: var(--orange);
}
.knob-center-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 0.65rem;
  font-family: 'Courier New', monospace;
  font-weight: 700;
  color: var(--text);
}
.knob-label {
  font-size: 0.6rem;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 1px;
  text-align: center;
}
.knob-unit {
  font-size: 0.55rem;
  color: var(--accent);
}

/* ======================== EQ VISUALIZER ======================== */
.eq-display {
  background: var(--bg-control);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 8px;
  margin-bottom: 12px;
}
.eq-display canvas {
  width: 100%;
  height: 100px;
  border-radius: 4px;
}

/* ======================== BAND CONTROLS ======================== */
.bands-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 8px;
}
.band-strip {
  background: var(--bg-control);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 10px 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}
.band-name {
  font-size: 0.55rem;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 1px;
  text-align: center;
}
.band-freq {
  font-size: 0.6rem;
  color: var(--accent);
  font-family: 'Courier New', monospace;
}
.band-control {
  width: 100%;
}
.band-control label {
  display: block;
  font-size: 0.5rem;
  color: var(--text-dim);
  margin-bottom: 2px;
}
.band-control input[type="range"] {
  height: 4px;
}
.band-control input[type="range"]::-webkit-slider-thumb {
  width: 12px;
  height: 12px;
}
.band-control .range-value {
  font-size: 0.55rem;
}

/* ======================== GRAPHIC EQ ======================== */
.geq-container {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 4px;
  padding: 10px 0;
}
.geq-band {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  min-width: 30px;
}
.geq-band input[type="range"] {
  writing-mode: vertical-lr;
  direction: rtl;
  height: 120px;
  width: 6px;
}
.geq-band input[type="range"]::-webkit-slider-thumb {
  width: 14px;
  height: 14px;
}
.geq-freq {
  font-size: 0.5rem;
  color: var(--text-dim);
  font-family: 'Courier New', monospace;
}
.geq-db {
  font-size: 0.5rem;
  color: var(--accent);
  font-family: 'Courier New', monospace;
}

/* ======================== COMPRESSOR DISPLAY ======================== */
.comp-band {
  background: var(--bg-control);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 8px;
}
.comp-band-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.comp-band-title {
  font-size: 0.65rem;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 600;
}
.comp-controls {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 8px;
}
.comp-control {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}
.comp-control label {
  font-size: 0.5rem;
  color: var(--text-dim);
  text-transform: uppercase;
}
.comp-control .range-value {
  font-size: 0.55rem;
}
.comp-control input[type="range"] {
  height: 4px;
}

/* ======================== SPECTRUM DISPLAY ======================== */
.spectrum-container {
  background: var(--bg-control);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 8px;
}
.spectrum-container canvas {
  width: 100%;
  height: 150px;
  border-radius: 4px;
}

/* ======================== STEREO ENHANCER ======================== */
.stereo-display {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  align-items: center;
}
.stereo-knobs {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
}

/* ======================== PRESETS ======================== */
.presets-row {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin-bottom: 12px;
}
.preset-btn {
  padding: 6px 16px;
  border: 1px solid var(--border);
  background: var(--bg-control);
  color: var(--text-dim);
  border-radius: 20px;
  font-size: 0.7rem;
  cursor: pointer;
  transition: all 0.3s;
  text-transform: uppercase;
  letter-spacing: 1px;
}
.preset-btn:hover {
  border-color: var(--accent);
  color: var(--accent);
  background: rgba(0, 212, 255, 0.1);
}
.preset-btn.active {
  border-color: var(--accent);
  background: rgba(0, 212, 255, 0.15);
  color: var(--accent);
  box-shadow: 0 0 12px var(--accent-glow);
}

/* ======================== CLIP INDICATOR ======================== */
.clip-indicator {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #333;
  transition: all 0.05s;
}
.clip-indicator.active {
  background: var(--red);
  box-shadow: 0 0 12px var(--red-glow);
  animation: clipPulse 0.1s;
}
@keyframes clipPulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.5); }
  100% { transform: scale(1); }
}

/* ======================== STATUS BAR ======================== */
.status-bar {
  background: var(--bg-panel);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
  font-size: 0.65rem;
  color: var(--text-dim);
}
.status-item {
  display: flex;
  align-items: center;
  gap: 6px;
}
.status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--green);
  animation: pulse 2s infinite;
}
.status-dot.off {
  background: #555;
  animation: none;
}
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

/* ======================== RESPONSIVE ======================== */
@media (max-width: 1200px) {
  .bottom-section { grid-template-columns: 1fr; }
  .top-row { grid-template-columns: 1fr; }
  .bands-grid { grid-template-columns: repeat(3, 1fr); }
  .comp-controls { grid-template-columns: repeat(3, 1fr); }
}
@media (max-width: 768px) {
  .io-section { grid-template-columns: 1fr; }
  .bands-grid { grid-template-columns: repeat(2, 1fr); }
  .comp-controls { grid-template-columns: repeat(2, 1fr); }
  .geq-container { overflow-x: auto; }
}

/* ======================== MODULE SECTION SPACING ======================== */
.module-section {
  margin-bottom: 16px;
}
.module-section:last-child { margin-bottom: 0; }

/* ======================== DUAL BAND COMPRESSOR ======================== */
.dual-band {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}
.dual-band-strip {
  background: var(--bg-control);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 12px;
}
.dual-band-strip h4 {
  font-size: 0.7rem;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 10px;
  text-align: center;
}
.dual-controls {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 8px;
}
.dual-control {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}
.dual-control label {
  font-size: 0.5rem;
  color: var(--text-dim);
  text-transform: uppercase;
}
.dual-control .range-value {
  font-size: 0.55rem;
}

/* ======================== BASS ENHANCER ======================== */
.bass-controls {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
}

/* ======================== LIMITER DISPLAY ======================== */
.limiter-meter {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 8px;
}
.limiter-bar {
  flex: 1;
  height: 8px;
  background: var(--bg-control);
  border-radius: 4px;
  overflow: hidden;
  border: 1px solid var(--border);
}
.limiter-fill {
  height: 100%;
  border-radius: 3px;
  transition: width 0.05s;
}

/* ======================== LOUDNESS METER ======================== */
.loudness-display {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 0;
}
.loudness-value {
  font-size: 1.2rem;
  font-family: 'Courier New', monospace;
  font-weight: 700;
  color: var(--accent);
}
.loudness-label {
  font-size: 0.6rem;
  color: var(--text-dim);
}

/* ======================== TABS ======================== */
.tabs {
  display: flex;
  gap: 2px;
  margin-bottom: 12px;
  background: var(--bg-control);
  border-radius: 8px;
  padding: 3px;
}
.tab-btn {
  flex: 1;
  padding: 6px 12px;
  border: none;
  background: transparent;
  color: var(--text-dim);
  font-size: 0.65rem;
  cursor: pointer;
  border-radius: 6px;
  transition: all 0.3s;
  text-transform: uppercase;
  letter-spacing: 1px;
}
.tab-btn.active {
  background: var(--bg-panel-light);
  color: var(--accent);
}
.tab-content { display: none; }
.tab-content.active { display: block; }
</style>
</head>
<body>

<!-- ======================== HEADER ======================== -->
<header class="header">
  <div class="logo">
    <svg width="36" height="36" viewBox="0 0 36 36" fill="none">
      <circle cx="18" cy="18" r="16" stroke="url(#grad1)" stroke-width="2"/>
      <path d="M18 8v20M12 14v8M24 10v16M8 16v4M28 12v12" stroke="url(#grad1)" stroke-width="2" stroke-linecap="round"/>
      <defs><linearGradient id="grad1" x1="0" y1="0" x2="36" y2="36"><stop stop-color="#00d4ff"/><stop offset="1" stop-color="#8866ff"/></linearGradient></defs>
    </svg>
    <div>
      <h1>BROADCAST PROCESSOR PRO</h1>
      <span>Real-Time Audio Engine v2.0</span>
    </div>
  </div>
  <div class="header-controls">
    <div style="display:flex;align-items:center;gap:8px;">
      <span style="font-size:0.6rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px;">DSP</span>
      <div class="clip-indicator" id="clipIndicator"></div>
    </div>
    <div class="power-btn" id="powerBtn" title="Power ON/OFF">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
        <path d="M12 3v9M18.36 6.64a9 9 0 1 1-12.73 0"/>
      </svg>
    </div>
  </div>
</header>

<!-- ======================== STATUS BAR ======================== -->
<div class="main-container">
  <div class="status-bar">
    <div class="status-item">
      <div class="status-dot off" id="statusDot"></div>
      <span id="statusText">DSP OFF — Click power to start</span>
    </div>
    <div class="status-item">
      <span>Sample Rate: <b id="sampleRateDisplay">—</b></span>
    </div>
    <div class="status-item">
      <span>Latency: <b id="latencyDisplay">—</b></span>
    </div>
    <div class="status-item">
      <span id="timeDisplay">00:00:00</span>
    </div>
  </div>

  <!-- ======================== INPUT / OUTPUT ======================== -->
  <div class="top-row">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🎤 Input</span>
        <span class="panel-badge">SIGNAL</span>
      </div>
      <div class="panel-body">
        <div class="io-section">
          <div class="io-group">
            <label>Input Device</label>
            <select id="inputDevice"><option value="">— Select Input —</option></select>
          </div>
          <div class="io-group">
            <label>Input Gain</label>
            <input type="range" id="inputGain" min="0" max="2" step="0.01" value="1">
            <div class="range-value" id="inputGainVal">0.0 dB</div>
          </div>
          <div class="meter-container">
            <canvas id="inputMeter" width="60" height="200"></canvas>
            <div class="meter-db" id="inputDb">-∞ dB</div>
            <div class="meter-label">Input Level</div>
          </div>
        </div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🔊 Output</span>
        <span class="panel-badge">MASTER</span>
      </div>
      <div class="panel-body">
        <div class="io-section">
          <div class="io-group">
            <label>Output Device</label>
            <select id="outputDevice"><option value="">Default</option></select>
          </div>
          <div class="io-group">
            <label>Master Volume</label>
            <input type="range" id="masterVolume" class="green" min="0" max="1.5" step="0.01" value="0.8">
            <div class="range-value" id="masterVolumeVal">-1.9 dB</div>
          </div>
          <div class="meter-container">
            <canvas id="outputMeter" width="60" height="200"></canvas>
            <div class="meter-db" id="outputDb">-∞ dB</div>
            <div class="meter-label">Output Level</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ======================== PRESETS ======================== -->
  <div class="full-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">⚡ Presets</span>
        <span class="panel-badge">QUICK LOAD</span>
      </div>
      <div class="panel-body">
        <div class="presets-row">
          <button class="preset-btn active" data-preset="flat">Flat</button>
          <button class="preset-btn" data-preset="radioFM">Radio FM</button>
          <button class="preset-btn" data-preset="voiceClear">Voz Clara</button>
          <button class="preset-btn" data-preset="musicPower">Música Potente</button>
          <button class="preset-btn" data-preset="ultraBass">Ultra Bass</button>
          <button class="preset-btn" data-preset="brilliant">Brillante</button>
          <button class="preset-btn" data-preset="podcast">Podcast</button>
          <button class="preset-btn" data-preset="liveBand">Live Band</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ======================== PARAMETRIC EQ ======================== -->
  <div class="module-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🎚️ EQ Multibanda Paramétrico</span>
        <div class="bypass-toggle">
          <label>Bypass</label>
          <label class="toggle-switch">
            <input type="checkbox" id="paramEqBypass">
            <span class="toggle-slider"></span>
          </label>
        </div>
      </div>
      <div class="panel-body">
        <div class="eq-display">
          <canvas id="paramEqCanvas" height="100"></canvas>
        </div>
        <div class="bands-grid" id="paramEqBands"></div>
      </div>
    </div>
  </div>

  <!-- ======================== GRAPHIC EQ ======================== -->
  <div class="module-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📊 Ecualizador Gráfico</span>
        <div class="bypass-toggle">
          <label>Bypass</label>
          <label class="toggle-switch">
            <input type="checkbox" id="geqBypass">
            <span class="toggle-slider"></span>
          </label>
        </div>
      </div>
      <div class="panel-body">
        <div class="geq-container" id="geqBands"></div>
      </div>
    </div>
  </div>

  <!-- ======================== MULTIBAND COMPRESSOR ======================== -->
  <div class="module-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🔗 Compresor Multibanda</span>
        <div class="bypass-toggle">
          <label>Bypass</label>
          <label class="toggle-switch">
            <input type="checkbox" id="mbCompBypass">
            <span class="toggle-slider"></span>
          </label>
        </div>
      </div>
      <div class="panel-body">
        <div id="mbCompBands"></div>
      </div>
    </div>
  </div>

  <!-- ======================== DUAL BAND COMPRESSOR + LIMITER ======================== -->
  <div class="module-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">⚡ Dual Band Compressor / Limiter</span>
        <div class="bypass-toggle">
          <label>Bypass</label>
          <label class="toggle-switch">
            <input type="checkbox" id="dualCompBypass">
            <span class="toggle-slider"></span>
          </label>
        </div>
      </div>
      <div class="panel-body">
        <div class="dual-band">
          <div class="dual-band-strip">
            <h4>🔊 Low Band (20–2kHz)</h4>
            <div class="dual-controls">
              <div class="dual-control">
                <label>Threshold</label>
                <input type="range" id="dualLowThresh" min="-60" max="0" step="0.5" value="-24">
                <div class="range-value" id="dualLowThreshVal">-24 dB</div>
              </div>
              <div class="dual-control">
                <label>Ratio</label>
                <input type="range" id="dualLowRatio" min="1" max="20" step="0.5" value="4">
                <div class="range-value" id="dualLowRatioVal">4:1</div>
              </div>
              <div class="dual-control">
                <label>Attack</label>
                <input type="range" id="dualLowAttack" min="0.001" max="0.1" step="0.001" value="0.01">
                <div class="range-value" id="dualLowAttackVal">10ms</div>
              </div>
              <div class="dual-control">
                <label>Release</label>
                <input type="range" id="dualLowRelease" min="0.01" max="1" step="0.01" value="0.1">
                <div class="range-value" id="dualLowReleaseVal">100ms</div>
              </div>
              <div class="dual-control">
                <label>Makeup</label>
                <input type="range" id="dualLowMakeup" min="0" max="18" step="0.5" value="3">
                <div class="range-value" id="dualLowMakeupVal">3 dB</div>
              </div>
              <div class="dual-control">
                <label>Crossover</label>
                <input type="range" id="dualLowCrossover" min="500" max="4000" step="50" value="2000">
                <div class="range-value" id="dualLowCrossoverVal">2000 Hz</div>
              </div>
            </div>
          </div>
          <div class="dual-band-strip">
            <h4>🎵 High Band (2k–20kHz)</h4>
            <div class="dual-controls">
              <div class="dual-control">
                <label>Threshold</label>
                <input type="range" id="dualHighThresh" min="-60" max="0" step="0.5" value="-18">
                <div class="range-value" id="dualHighThreshVal">-18 dB</div>
              </div>
              <div class="dual-control">
                <label>Ratio</label>
                <input type="range" id="dualHighRatio" min="1" max="20" step="0.5" value="3">
                <div class="range-value" id="dualHighRatioVal">3:1</div>
              </div>
              <div class="dual-control">
                <label>Attack</label>
                <input type="range" id="dualHighAttack" min="0.001" max="0.1" step="0.001" value="0.005">
                <div class="range-value" id="dualHighAttackVal">5ms</div>
              </div>
              <div class="dual-control">
                <label>Release</label>
                <input type="range" id="dualHighRelease" min="0.01" max="1" step="0.01" value="0.08">
                <div class="range-value" id="dualHighReleaseVal">80ms</div>
              </div>
              <div class="dual-control">
                <label>Makeup</label>
                <input type="range" id="dualHighMakeup" min="0" max="18" step="0.5" value="2">
                <div class="range-value" id="dualHighMakeupVal">2 dB</div>
              </div>
              <div class="dual-control">
                <label>Presence</label>
                <input type="range" id="dualHighPresence" min="0" max="6" step="0.5" value="2">
                <div class="range-value" id="dualHighPresenceVal">2 dB</div>
              </div>
            </div>
          </div>
        </div>
        <div style="margin-top:12px;">
          <div class="dual-band-strip">
            <h4>🛡️ Final Limiter</h4>
            <div class="dual-controls" style="grid-template-columns: repeat(3,1fr);">
              <div class="dual-control">
                <label>Ceiling</label>
                <input type="range" id="limiterCeiling" min="-3" max="0" step="0.1" value="-0.3">
                <div class="range-value" id="limiterCeilingVal">-0.3 dB</div>
              </div>
              <div class="dual-control">
                <label>Release</label>
                <input type="range" id="limiterRelease" min="0.01" max="0.5" step="0.01" value="0.05">
                <div class="range-value" id="limiterReleaseVal">50ms</div>
              </div>
              <div class="dual-control">
                <label>Output Gain</label>
                <input type="range" id="limiterOutputGain" min="0" max="12" step="0.5" value="4">
                <div class="range-value" id="limiterOutputGainVal">4 dB</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ======================== BASS EQ + STEREO ENHANCER ======================== -->
  <div class="bottom-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🔊 Bass Enhancer</span>
        <div class="bypass-toggle">
          <label>Bypass</label>
          <label class="toggle-switch">
            <input type="checkbox" id="bassBypass">
            <span class="toggle-slider"></span>
          </label>
        </div>
      </div>
      <div class="panel-body">
        <div class="bass-controls">
          <div class="knob-control">
            <div class="knob-container" data-knob="bassFreq" data-min="20" data-max="200" data-value="80" data-unit="Hz">
              <svg class="knob-svg" viewBox="0 0 60 60">
                <circle class="knob-bg" cx="30" cy="30" r="26"/>
                <circle class="knob-fill green" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="81.68"/>
              </svg>
              <div class="knob-center-text" id="bassFreqText">80</div>
            </div>
            <div class="knob-label">Frequency</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="bassGain" data-min="-12" data-max="12" data-value="4" data-unit="dB">
              <svg class="knob-svg" viewBox="0 0 60 60">
                <circle class="knob-bg" cx="30" cy="30" r="26"/>
                <circle class="knob-fill green" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="65.34"/>
              </svg>
              <div class="knob-center-text" id="bassGainText">4</div>
            </div>
            <div class="knob-label">Boost</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="bassQ" data-min="0.1" data-max="3" data-value="0.7" data-unit="">
              <svg class="knob-svg" viewBox="0 0 60 60">
                <circle class="knob-bg" cx="30" cy="30" r="26"/>
                <circle class="knob-fill green" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="100.74"/>
              </svg>
              <div class="knob-center-text" id="bassQText">0.7</div>
            </div>
            <div class="knob-label">Q / Width</div>
          </div>
        </div>
        <div style="margin-top:12px;text-align:center;">
          <label class="toggle-switch" style="margin-right:8px;">
            <input type="checkbox" id="bassBoostPro">
            <span class="toggle-slider"></span>
          </label>
          <span style="font-size:0.65rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px;">Bass Boost Pro</span>
        </div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🎧 Stereo Enhancer</span>
        <div class="bypass-toggle">
          <label>Bypass</label>
          <label class="toggle-switch">
            <input type="checkbox" id="stereoBypass">
            <span class="toggle-slider"></span>
          </label>
        </div>
      </div>
      <div class="panel-body">
        <div class="stereo-knobs">
          <div class="knob-control">
            <div class="knob-container" data-knob="stereoWidth" data-min="0" data-max="200" data-value="100" data-unit="%">
              <svg class="knob-svg" viewBox="0 0 60 60">
                <circle class="knob-bg" cx="30" cy="30" r="26"/>
                <circle class="knob-fill orange" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="81.68"/>
              </svg>
              <div class="knob-center-text" id="stereoWidthText">100</div>
            </div>
            <div class="knob-label">Width</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="stereoBalance" data-min="-100" data-max="100" data-value="0" data-unit="">
              <svg class="knob-svg" viewBox="0 0 60 60">
                <circle class="knob-bg" cx="30" cy="30" r="26"/>
                <circle class="knob-fill orange" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="81.68"/>
              </svg>
              <div class="knob-center-text" id="stereoBalanceText">C</div>
            </div>
            <div class="knob-label">Balance</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="stereoPhase" data-min="0" data-max="180" data-value="0" data-unit="°">
              <svg class="knob-svg" viewBox="0 0 60 60">
                <circle class="knob-bg" cx="30" cy="30" r="26"/>
                <circle class="knob-fill orange" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="163.36"/>
              </svg>
              <div class="knob-center-text" id="stereoPhaseText">0°</div>
            </div>
            <div class="knob-label">Phase</div>
          </div>
        </div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📈 Spectral Analyzer</span>
        <span class="panel-badge">FFT</span>
      </div>
      <div class="panel-body">
        <div class="spectrum-container">
          <canvas id="spectrumCanvas"></canvas>
        </div>
        <div class="loudness-display">
          <div>
            <div class="loudness-value" id="loudnessVal">-∞</div>
            <div class="loudness-label">Integrated LUFS</div>
          </div>
          <div>
            <div class="loudness-value" id="peakVal">-∞</div>
            <div class="loudness-label">True Peak dBTP</div>
          </div>
          <div>
            <div class="loudness-value" id="dynamicVal">—</div>
            <div class="loudness-label">Dynamic Range</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
// ======================== AUDIO ENGINE ========================
class BroadcastProcessor {
  constructor() {
    this.ctx = null;
    this.active = false;
    this.startTime = 0;

    // Nodes
    this.inputGain = null;
    this.outputGain = null;
    this.source = null;
    this.stream = null;

    // Parametric EQ
    this.paramEqFilters = [];
    this.paramEqBypass = false;

    // Graphic EQ
    this.geqFilters = [];
    this.geqBypass = false;

    // Multiband Compressor
    this.mbCompressorBands = [];
    this.mbCompBypass = false;

    // Dual Band Compressor
    this.dualLowSplit = null;
    this.dualHighSplit = null;
    this.dualLowComp = null;
    this.dualHighComp = null;
    this.dualLowGain = null;
    this.dualHighGain = null;
    this.dualCompBypass = false;

    // Bass Enhancer
    this.bassFilter = null;
    this.bassBypass = false;

    // Stereo Enhancer
    this.stereoMerger = null;
    this.stereoSplitter = null;
    this.stereoLeftGain = null;
    this.stereoRightGain = null;
    this.stereoBypass = false;

    // Final Limiter
    this.limiter = null;
    this.limiterGain = null;

    // Analyzers
    this.inputAnalyser = null;
    this.outputAnalyser = null;
    this.spectrumAnalyser = null;

    // Metering
    this.inputLevel = 0;
    this.outputLevel = 0;
    this.clipTimeout = null;

    // Parametric EQ bands config
    this.paramEqBandsConfig = [
      { freq: 40, gain: 0, Q: 1.0, type: 'lowshelf', name: 'Sub Bass', range: '20-60Hz' },
      { freq: 150, gain: 0, Q: 1.0, type: 'peaking', name: 'Bass', range: '60-250Hz' },
      { freq: 1000, gain: 0, Q: 1.0, type: 'peaking', name: 'Mid', range: '250-2kHz' },
      { freq: 4000, gain: 0, Q: 1.0, type: 'peaking', name: 'High Mid', range: '2k-6kHz' },
      { freq: 10000, gain: 0, Q: 1.0, type: 'highshelf', name: 'Treble', range: '6k-16kHz' }
    ];

    // Graphic EQ frequencies
    this.geqFreqs = [31, 62, 125, 250, 500, 1000, 2000, 4000, 8000, 12000, 16000];

    // Multiband compressor config
    this.mbCompConfig = [
      { name: 'Sub Bass', lowFreq: 20, highFreq: 80, threshold: -30, ratio: 4, attack: 0.015, release: 0.15, makeup: 3 },
      { name: 'Bass', lowFreq: 80, highFreq: 250, threshold: -24, ratio: 4, attack: 0.01, release: 0.12, makeup: 2 },
      { name: 'Mid', lowFreq: 250, highFreq: 2000, threshold: -20, ratio: 3, attack: 0.008, release: 0.08, makeup: 1 },
      { name: 'High Mid', lowFreq: 2000, highFreq: 6000, threshold: -18, ratio: 3, attack: 0.005, release: 0.06, makeup: 1 },
      { name: 'Treble', lowFreq: 6000, highFreq: 16000, threshold: -20, ratio: 2.5, attack: 0.003, release: 0.05, makeup: 1 }
    ];
  }

  async init() {
    this.ctx = new (window.AudioContext || window.webkitAudioContext)({
      latencyHint: 'interactive',
      sampleRate: 48000
    });

    document.getElementById('sampleRateDisplay').textContent = this.ctx.sampleRate + ' Hz';

    // Create nodes
    this.inputGain = this.ctx.createGain();
    this.outputGain = this.ctx.createGain();

    this.inputAnalyser = this.ctx.createAnalyser();
    this.inputAnalyser.fftSize = 2048;
    this.outputAnalyser = this.ctx.createAnalyser();
    this.outputAnalyser.fftSize = 2048;
    this.spectrumAnalyser = this.ctx.createAnalyser();
    this.spectrumAnalyser.fftSize = 4096;
    this.spectrumAnalyser.smoothingTimeConstant = 0.8;

    // Create Parametric EQ filters
    this.paramEqFilters = this.paramEqBandsConfig.map(band => {
      const filter = this.ctx.createBiquadFilter();
      filter.type = band.type;
      filter.frequency.value = band.freq;
      filter.gain.value = band.gain;
      filter.Q.value = band.Q;
      return filter;
    });

    // Create Graphic EQ filters
    this.geqFilters = this.geqFreqs.map(freq => {
      const filter = this.ctx.createBiquadFilter();
      filter.type = 'peaking';
      filter.frequency.value = freq;
      filter.gain.value = 0;
      filter.Q.value = 1.4;
      return filter;
    });

    // Create Multiband Compressor bands
    this.mbCompressorBands = this.mbCompConfig.map(config => {
      const input = this.ctx.createGain();
      const lowpass = this.ctx.createBiquadFilter();
      lowpass.type = 'lowpass';
      lowpass.frequency.value = config.highFreq;
      lowpass.Q.value = 0.707;

      const highpass = this.ctx.createBiquadFilter();
      highpass.type = 'highpass';
      highpass.frequency.value = config.lowFreq;
      highpass.Q.value = 0.707;

      const compressor = this.ctx.createDynamicsCompressor();
      compressor.threshold.value = config.threshold;
      compressor.ratio.value = config.ratio;
      compressor.attack.value = config.attack;
      compressor.release.value = config.release;
      compressor.knee.value = 6;

      const makeupGain = this.ctx.createGain();
      makeupGain.gain.value = Math.pow(10, config.makeup / 20);

      const output = this.ctx.createGain();
      output.gain.value = 0; // Will be set when bypass is off

      return { ...config, input, lowpass, highpass, compressor, makeupGain, output };
    });

    // Dual Band Compressor
    this.dualLowSplit = this.ctx.createBiquadFilter();
    this.dualLowSplit.type = 'lowpass';
    this.dualLowSplit.frequency.value = 2000;

    this.dualHighSplit = this.ctx.createBiquadFilter();
    this.dualHighSplit.type = 'highpass';
    this.dualHighSplit.frequency.value = 2000;

    this.dualLowComp = this.ctx.createDynamicsCompressor();
    this.dualLowComp.threshold.value = -24;
    this.dualLowComp.ratio.value = 4;
    this.dualLowComp.attack.value = 0.01;
    this.dualLowComp.release.value = 0.1;

    this.dualHighComp = this.ctx.createDynamicsCompressor();
    this.dualHighComp.threshold.value = -18;
    this.dualHighComp.ratio.value = 3;
    this.dualHighComp.attack.value = 0.005;
    this.dualHighComp.release.value = 0.08;

    this.dualLowGain = this.ctx.createGain();
    this.dualLowGain.gain.value = Math.pow(10, 3/20);

    this.dualHighGain = this.ctx.createGain();
    this.dualHighGain.gain.value = Math.pow(10, 2/20);

    // Bass Enhancer
    this.bassFilter = this.ctx.createBiquadFilter();
    this.bassFilter.type = 'lowshelf';
    this.bassFilter.frequency.value = 80;
    this.bassFilter.gain.value = 4;
    this.bassFilter.Q.value = 0.7;

    // Stereo Enhancer
    this.stereoSplitter = this.ctx.createChannelSplitter(2);
    this.stereoMerger = this.ctx.createChannelMerger(2);
    this.stereoLeftGain = this.ctx.createGain();
    this.stereoLeftGain.gain.value = 1;
    this.stereoRightGain = this.ctx.createGain();
    this.stereoRightGain.gain.value = 1;

    // Final Limiter
    this.limiter = this.ctx.createDynamicsCompressor();
    this.limiter.threshold.value = -0.3;
    this.limiter.ratio.value = 20;
    this.limiter.attack.value = 0.001;
    this.limiter.release.value = 0.05;
    this.limiter.knee.value = 0;

    this.limiterGain = this.ctx.createGain();
    this.limiterGain.gain.value = Math.pow(10, 4/20);

    // Connect master output
    this.outputGain.connect(this.inputAnalyser);
    this.inputAnalyser.connect(this.ctx.destination);

    // Connect spectrum analyser
    this.outputGain.connect(this.spectrumAnalyser);

    // Connect input analyser to source side
    this.inputGain.connect(this.inputAnalyser);

    this.active = true;
    this.startTime = Date.now();
    document.getElementById('statusDot').classList.remove('off');
    document.getElementById('statusText').textContent = 'DSP ACTIVE — Processing audio';
  }

  async startAudio() {
    if (!this.ctx) await this.init();
    if (this.ctx.state === 'suspended') await this.ctx.resume();

    try {
      const deviceId = document.getElementById('inputDevice').value;
      const constraints = {
        audio: deviceId ? { deviceId: { exact: deviceId } } : {
          echoCancellation: false,
          noiseSuppression: false,
          autoGainControl: false
        }
      };

      this.stream = await navigator.mediaDevices.getUserMedia(constraints);
      this.source = this.ctx.createMediaStreamSource(this.stream);
      this.source.connect(this.inputGain);

      // Build the full audio chain
      this.buildAudioChain();

      this.startTime = Date.now();
      return true;
    } catch (err) {
      console.error('Error starting audio:', err);
      alert('Error al acceder al micrófono: ' + err.message);
      return false;
    }
  }

  buildAudioChain() {
    // Disconnect everything first
    this.disconnectChain();

    // Chain: inputGain → [ParamEQ] → [MBCompressor] → [GraphicEQ] → [DualBand] → [Bass] → [Stereo] → [Limiter] → outputGain
    let chainEnd = this.inputGain;

    // Parametric EQ
    if (!this.paramEqBypass) {
      chainEnd.connect(this.paramEqFilters[0]);
      for (let i = 0; i < this.paramEqFilters.length - 1; i++) {
        this.paramEqFilters[i].connect(this.paramEqFilters[i + 1]);
      }
      chainEnd = this.paramEqFilters[this.paramEqFilters.length - 1];
    }

    // Multiband Compressor
    if (!this.mbCompBypass) {
      this.buildMultibandCompressor(chainEnd);
      chainEnd = this.mbCompressorOutput;
    }

    // Graphic EQ
    if (!this.geqBypass) {
      chainEnd.connect(this.geqFilters[0]);
      for (let i = 0; i < this.geqFilters.length - 1; i++) {
        this.geqFilters[i].connect(this.geqFilters[i + 1]);
      }
      chainEnd = this.geqFilters[this.geqFilters.length - 1];
    }

    // Dual Band Compressor
    if (!this.dualCompBypass) {
      this.buildDualBandCompressor(chainEnd);
      chainEnd = this.dualBandOutput;
    }

    // Bass Enhancer
    if (!this.bassBypass) {
      chainEnd.connect(this.bassFilter);
      chainEnd = this.bassFilter;
    }

    // Stereo Enhancer
    if (!this.stereoBypass) {
      this.buildStereoEnhancer(chainEnd);
      chainEnd = this.stereoOutput;
    }

    // Final Limiter
    chainEnd.connect(this.limiter);
    this.limiter.connect(this.limiterGain);
    this.limiterGain.connect(this.outputGain);
  }

  buildMultibandCompressor(inputNode) {
    // Create a gain node to sum all bands
    this.mbCompressorOutput = this.ctx.createGain();
    this.mbCompressorOutput.gain.value = 0;

    this.mbCompressorBands.forEach((band, i) => {
      inputNode.connect(band.lowpass);
      band.lowpass.connect(band.highpass);
      band.highpass.connect(band.compressor);
      band.compressor.connect(band.makeupGain);
      band.makeupGain.connect(this.mbCompressorOutput);
    });
  }

  buildDualBandCompressor(inputNode) {
    this.dualBandOutput = this.ctx.createGain();
    this.dualBandOutput.gain.value = 0;

    inputNode.connect(this.dualLowSplit);
    inputNode.connect(this.dualHighSplit);

    this.dualLowSplit.connect(this.dualLowComp);
    this.dualLowComp.connect(this.dualLowGain);
    this.dualLowGain.connect(this.dualBandOutput);

    this.dualHighSplit.connect(this.dualHighComp);
    this.dualHighComp.connect(this.dualHighGain);
    this.dualHighGain.connect(this.dualBandOutput);
  }

  buildStereoEnhancer(inputNode) {
    this.stereoOutput = this.ctx.createGain();
    this.stereoOutput.gain.value = 0;

    inputNode.connect(this.stereoSplitter);
    this.stereoSplitter.connect(this.stereoLeftGain, 0);
    this.stereoSplitter.connect(this.stereoRightGain, 1);
    this.stereoLeftGain.connect(this.stereoMerger, 0, 0);
    this.stereoRightGain.connect(this.stereoMerger, 0, 1);
    this.stereoMerger.connect(this.stereoOutput);
  }

  disconnectChain() {
    // Disconnect inputGain
    try { this.inputGain.disconnect(); } catch(e) {}

    // Disconnect param EQ
    this.paramEqFilters.forEach(f => { try { f.disconnect(); } catch(e) {} });

    // Disconnect MB compressor
    this.mbCompressorBands.forEach(band => {
      try { band.input.disconnect(); } catch(e) {}
      try { band.lowpass.disconnect(); } catch(e) {}
      try { band.highpass.disconnect(); } catch(e) {}
      try { band.compressor.disconnect(); } catch(e) {}
      try { band.makeupGain.disconnect(); } catch(e) {}
      try { band.output.disconnect(); } catch(e) {}
    });
    if (this.mbCompressorOutput) { try { this.mbCompressorOutput.disconnect(); } catch(e) {} }

    // Disconnect GEQ
    this.geqFilters.forEach(f => { try { f.disconnect(); } catch(e) {} });

    // Disconnect dual band
    try { this.dualLowSplit.disconnect(); } catch(e) {}
    try { this.dualHighSplit.disconnect(); } catch(e) {}
    try { this.dualLowComp.disconnect(); } catch(e) {}
    try { this.dualHighComp.disconnect(); } catch(e) {}
    try { this.dualLowGain.disconnect(); } catch(e) {}
    try { this.dualHighGain.disconnect(); } catch(e) {}
    if (this.dualBandOutput) { try { this.dualBandOutput.disconnect(); } catch(e) {} }

    // Disconnect bass
    try { this.bassFilter.disconnect(); } catch(e) {}

    // Disconnect stereo
    try { this.stereoSplitter.disconnect(); } catch(e) {}
    try { this.stereoMerger.disconnect(); } catch(e) {}
    try { this.stereoLeftGain.disconnect(); } catch(e) {}
    try { this.stereoRightGain.disconnect(); } catch(e) {}
    if (this.stereoOutput) { try { this.stereoOutput.disconnect(); } catch(e) {} }

    // Disconnect limiter
    try { this.limiter.disconnect(); } catch(e) {}
    try { this.limiterGain.disconnect(); } catch(e) {}

    // Disconnect output gain from input analyser
    try { this.outputGain.disconnect(); } catch(e) {}
    this.outputGain.connect(this.inputAnalyser);
    this.outputGain.connect(this.spectrumAnalyser);
    this.outputGain.connect(this.ctx.destination);
  }

  stopAudio() {
    if (this.stream) {
      this.stream.getTracks().forEach(track => track.stop());
      this.stream = null;
    }
    if (this.source) {
      try { this.source.disconnect(); } catch(e) {}
      this.source = null;
    }
    this.active = false;
    document.getElementById('statusDot').classList.add('off');
    document.getElementById('statusText').textContent = 'DSP OFF — Click power to start';
  }

  togglePower() {
    if (!this.active) {
      this.startAudio().then(success => {
        if (success) {
          document.getElementById('powerBtn').classList.add('active');
        }
      });
    } else {
      this.stopAudio();
      document.getElementById('powerBtn').classList.remove('active');
    }
  }

  setInputGain(value) {
    if (this.inputGain) {
      this.inputGain.gain.value = value;
    }
  }

  setMasterVolume(value) {
    if (this.outputGain) {
      this.outputGain.gain.value = value;
    }
  }

  // Parametric EQ
  setParamEqBand(index, freq, gain, Q) {
    if (this.paramEqFilters[index]) {
      const filter = this.paramEqFilters[index];
      filter.frequency.value = freq;
      filter.gain.value = gain;
      filter.Q.value = Q;
      this.paramEqBandsConfig[index].freq = freq;
      this.paramEqBandsConfig[index].gain = gain;
      this.paramEqBandsConfig[index].Q = Q;
    }
  }

  // Graphic EQ
  setGeqBand(index, gain) {
    if (this.geqFilters[index]) {
      this.geqFilters[index].gain.value = gain;
    }
  }

  // Bass
  setBassFreq(freq) {
    if (this.bassFilter) this.bassFilter.frequency.value = freq;
  }
  setBassGain(gain) {
    if (this.bassFilter) this.bassFilter.gain.value = gain;
  }
  setBassQ(Q) {
    if (this.bassFilter) this.bassFilter.Q.value = Q;
  }

  // Stereo
  setStereoWidth(width) {
    // width: 0-200 (100 = normal, 0 = mono, 200 = max width)
    if (this.stereoLeftGain && this.stereoRightGain) {
      const w = width / 100;
      // Mid/Side encoding approximation
      const mid = 0.5;
      const side = 0.5 * w;
      this.stereoLeftGain.gain.value = mid + side;
      this.stereoRightGain.gain.value = mid + side;
    }
  }
  setStereoBalance(balance) {
    if (this.stereoLeftGain && this.stereoRightGain) {
      const b = balance / 100;
      if (b >= 0) {
        this.stereoLeftGain.gain.value = 1;
        this.stereoRightGain.gain.value = 1 - b;
      } else {
        this.stereoLeftGain.gain.value = 1 + b;
        this.stereoRightGain.gain.value = 1;
      }
    }
  }

  // Dual Band Compressor
  setDualLowThresh(v) { if (this.dualLowComp) this.dualLowComp.threshold.value = v; }
  setDualLowRatio(v) { if (this.dualLowComp) this.dualLowComp.ratio.value = v; }
  setDualLowAttack(v) { if (this.dualLowComp) this.dualLowComp.attack.value = v; }
  setDualLowRelease(v) { if (this.dualLowComp) this.dualLowComp.release.value = v; }
  setDualLowMakeup(v) { if (this.dualLowGain) this.dualLowGain.gain.value = Math.pow(10, v/20); }
  setDualLowCrossover(v) {
    if (this.dualLowSplit) this.dualLowSplit.frequency.value = v;
    if (this.dualHighSplit) this.dualHighSplit.frequency.value = v;
  }

  setDualHighThresh(v) { if (this.dualHighComp) this.dualHighComp.threshold.value = v; }
  setDualHighRatio(v) { if (this.dualHighComp) this.dualHighComp.ratio.value = v; }
  setDualHighAttack(v) { if (this.dualHighComp) this.dualHighComp.attack.value = v; }
  setDualHighRelease(v) { if (this.dualHighComp) this.dualHighComp.release.value = v; }
  setDualHighMakeup(v) { if (this.dualHighGain) this.dualHighGain.gain.value = Math.pow(10, v/20); }
  setDualHighPresence(v) {
    // Presence is implemented as a gentle high-shelf boost
    // We'll add it to the dualHighGain as an approximation
    if (this.dualHighGain) {
      const base = parseFloat(document.getElementById('dualHighMakeup').value) || 0;
      this.dualHighGain.gain.value = Math.pow(10, (base + v) / 20);
    }
  }

  // Limiter
  setLimiterCeiling(v) { if (this.limiter) this.limiter.threshold.value = v; }
  setLimiterRelease(v) { if (this.limiter) this.limiter.release.value = v; }
  setLimiterOutputGain(v) { if (this.limiterGain) this.limiterGain.gain.value = Math.pow(10, v/20); }

  // Multiband Compressor
  setMbCompBand(index, threshold, ratio, attack, release, makeup) {
    const band = this.mbCompressorBands[index];
    if (band) {
      band.compressor.threshold.value = threshold;
      band.compressor.ratio.value = ratio;
      band.compressor.attack.value = attack;
      band.compressor.release.value = release;
      band.makeupGain.gain.value = Math.pow(10, makeup / 20);
    }
  }

  // Presets
  applyPreset(name) {
    const presets = {
      flat: {
        paramEq: [{f:40,g:0,q:1},{f:150,g:0,q:1},{f:1000,g:0,q:1},{f:4000,g:0,q:1},{f:10000,g:0,q:1}],
        geq: [0,0,0,0,0,0,0,0,0,0,0],
        bass: { f: 80, g: 0, q: 0.7 },
        dualLow: { th: -24, r: 4, a: 0.01, rel: 0.1, mk: 3 },
        dualHigh: { th: -18, r: 3, a: 0.005, rel: 0.08, mk: 2 },
        limiter: { ceiling: -0.3, rel: 0.05, gain: 4 },
        stereo: { width: 100, balance: 0 },
        mbComp: [
          { th: -30, r: 4, a: 0.015, rel: 0.15, mk: 3 },
          { th: -24, r: 4, a: 0.01, rel: 0.12, mk: 2 },
          { th: -20, r: 3, a: 0.008, rel: 0.08, mk: 1 },
          { th: -18, r: 3, a: 0.005, rel: 0.06, mk: 1 },
          { th: -20, r: 2.5, a: 0.003, rel: 0.05, mk: 1 }
        ]
      },
      radioFM: {
        paramEq: [{f:40,g:2,q:1.2},{f:150,g:1,q:1},{f:1000,g:3,q:0.8},{f:4000,g:4,q:1},{f:10000,g:2,q:1}],
        geq: [1,2,1,0,1,3,3,2,1,1,0],
        bass: { f: 80, g: 3, q: 0.8 },
        dualLow: { th: -28, r: 6, a: 0.008, rel: 0.08, mk: 5 },
        dualHigh: { th: -22, r: 4, a: 0.003, rel: 0.05, mk: 4 },
        limiter: { ceiling: -0.3, rel: 0.03, gain: 8 },
        stereo: { width: 120, balance: 0 },
        mbComp: [
          { th: -28, r: 6, a: 0.01, rel: 0.1, mk: 5 },
          { th: -24, r: 5, a: 0.008, rel: 0.08, mk: 4 },
          { th: -20, r: 4, a: 0.005, rel: 0.06, mk: 3 },
          { th: -18, r: 4, a: 0.003, rel: 0.04, mk: 3 },
          { th: -16, r: 3, a: 0.002, rel: 0.03, mk: 2 }
        ]
      },
      voiceClear: {
        paramEq: [{f:40,g:-6,q:1},{f:150,g:-3,q:1},{f:1000,g:4,q:0.7},{f:4000,g:5,q:0.8},{f:10000,g:2,q:1}],
        geq: [-4,-2,0,2,4,5,4,2,0,-1,-2],
        bass: { f: 80, g: -4, q: 0.7 },
        dualLow: { th: -30, r: 3, a: 0.015, rel: 0.15, mk: 2 },
        dualHigh: { th: -20, r: 4, a: 0.005, rel: 0.06, mk: 5 },
        limiter: { ceiling: -0.5, rel: 0.05, gain: 5 },
        stereo: { width: 80, balance: 0 },
        mbComp: [
          { th: -30, r: 3, a: 0.015, rel: 0.15, mk: 2 },
          { th: -26, r: 3, a: 0.01, rel: 0.12, mk: 1 },
          { th: -18, r: 4, a: 0.005, rel: 0.06, mk: 4 },
          { th: -16, r: 4, a: 0.003, rel: 0.04, mk: 4 },
          { th: -20, r: 2.5, a: 0.003, rel: 0.05, mk: 2 }
        ]
      },
      musicPower: {
        paramEq: [{f:40,g:5,q:1.2},{f:150,g:3,q:1},{f:1000,g:1,q:0.8},{f:4000,g:2,q:1},{f:10000,g:4,q:1}],
        geq: [4,3,2,1,0,1,2,3,3,2,1],
        bass: { f: 60, g: 6, q: 0.8 },
        dualLow: { th: -26, r: 5, a: 0.01, rel: 0.1, mk: 4 },
        dualHigh: { th: -18, r: 3.5, a: 0.005, rel: 0.07, mk: 3 },
        limiter: { ceiling: -0.3, rel: 0.04, gain: 7 },
        stereo: { width: 140, balance: 0 },
        mbComp: [
          { th: -26, r: 5, a: 0.01, rel: 0.1, mk: 4 },
          { th: -22, r: 4.5, a: 0.008, rel: 0.08, mk: 3 },
          { th: -18, r: 3, a: 0.005, rel: 0.06, mk: 2 },
          { th: -16, r: 3, a: 0.003, rel: 0.04, mk: 2 },
          { th: -18, r: 2.5, a: 0.003, rel: 0.05, mk: 2 }
        ]
      },
      ultraBass: {
        paramEq: [{f:40,g:10,q:1.4},{f:150,g:6,q:1.2},{f:1000,g:-2,q:0.8},{f:4000,g:-1,q:1},{f:10000,g:1,q:1}],
        geq: [8,6,4,2,0,-1,0,1,1,0,0],
        bass: { f: 50, g: 10, q: 0.9 },
        dualLow: { th: -32, r: 8, a: 0.015, rel: 0.12, mk: 8 },
        dualHigh: { th: -16, r: 2.5, a: 0.005, rel: 0.08, mk: 2 },
        limiter: { ceiling: -0.5, rel: 0.05, gain: 6 },
        stereo: { width: 100, balance: 0 },
        mbComp: [
          { th: -32, r: 8, a: 0.015, rel: 0.12, mk: 8 },
          { th: -26, r: 6, a: 0.01, rel: 0.1, mk: 6 },
          { th: -20, r: 3, a: 0.005, rel: 0.06, mk: 2 },
          { th: -18, r: 2.5, a: 0.003, rel: 0.04, mk: 1 },
          { th: -20, r: 2, a: 0.003, rel: 0.05, mk: 1 }
        ]
      },
      brilliant: {
        paramEq: [{f:40,g:0,q:1},{f:150,g:0,q:1},{f:1000,g:1,q:0.8},{f:4000,g:5,q:0.7},{f:10000,g:8,q:1}],
        geq: [0,0,0,0,1,2,3,5,6,5,4],
        bass: { f: 80, g: 0, q: 0.7 },
        dualLow: { th: -24, r: 4, a: 0.01, rel: 0.1, mk: 2 },
        dualHigh: { th: -20, r: 3, a: 0.003, rel: 0.05, mk: 5 },
        limiter: { ceiling: -0.3, rel: 0.04, gain: 5 },
        stereo: { width: 130, balance: 0 },
        mbComp: [
          { th: -28, r: 4, a: 0.012, rel: 0.12, mk: 3 },
          { th: -24, r: 4, a: 0.01, rel: 0.1, mk: 2 },
          { th: -20, r: 3, a: 0.006, rel: 0.07, mk: 2 },
          { th: -16, r: 3.5, a: 0.003, rel: 0.04, mk: 4 },
          { th: -14, r: 3, a: 0.002, rel: 0.03, mk: 5 }
        ]
      },
      podcast: {
        paramEq: [{f:40,g:-4,q:1},{f:150,g:-2,q:1},{f:1000,g:3,q:0.8},{f:4000,g:4,q:0.9},{f:10000,g:1,q:1}],
        geq: [-3,-2,-1,1,3,4,3,1,0,-1,-2],
        bass: { f: 100, g: -2, q: 0.7 },
        dualLow: { th: -30, r: 3, a: 0.015, rel: 0.15, mk: 2 },
        dualHigh: { th: -22, r: 4, a: 0.005, rel: 0.06, mk: 4 },
        limiter: { ceiling: -0.5, rel: 0.05, gain: 6 },
        stereo: { width: 60, balance: 0 },
        mbComp: [
          { th: -30, r: 3, a: 0.015, rel: 0.15, mk: 2 },
          { th: -26, r: 3, a: 0.012, rel: 0.12, mk: 1 },
          { th: -20, r: 4, a: 0.006, rel: 0.07, mk: 3 },
          { th: -18, r: 4, a: 0.004, rel: 0.05, mk: 3 },
          { th: -20, r: 2.5, a: 0.003, rel: 0.05, mk: 1 }
        ]
      },
      liveBand: {
        paramEq: [{f:40,g:4,q:1.2},{f:150,g:2,q:1},{f:1000,g:2,q:0.8},{f:4000,g:3,q:0.9},{f:10000,g:3,q:1}],
        geq: [3,2,1,1,0,1,2,2,3,2,1],
        bass: { f: 60, g: 4, q: 0.8 },
        dualLow: { th: -26, r: 5, a: 0.01, rel: 0.1, mk: 4 },
        dualHigh: { th: -20, r: 3.5, a: 0.005, rel: 0.07, mk: 3 },
        limiter: { ceiling: -0.3, rel: 0.04, gain: 6 },
        stereo: { width: 150, balance: 0 },
        mbComp: [
          { th: -26, r: 5, a: 0.01, rel: 0.1, mk: 4 },
          { th: -22, r: 4, a: 0.008, rel: 0.08, mk: 3 },
          { th: -20, r: 3.5, a: 0.006, rel: 0.07, mk: 2 },
          { th: -18, r: 3.5, a: 0.004, rel: 0.05, mk: 2 },
          { th: -18, r: 3, a: 0.003, rel: 0.04, mk: 2 }
        ]
      }
    };

    const preset = presets[name];
    if (!preset) return;

    // Apply Parametric EQ
    preset.paramEq.forEach((b, i) => {
      this.setParamEqBand(i, b.f, b.g, b.q);
    });
    updateParamEqUI(preset.paramEq);

    // Apply Graphic EQ
    preset.geq.forEach((g, i) => {
      this.setGeqBand(i, g);
    });
    updateGeqUI(preset.geq);

    // Apply Bass
    this.setBassFreq(preset.bass.f);
    this.setBassGain(preset.bass.g);
    this.setBassQ(preset.bass.q);
    updateKnob('bassFreq', preset.bass.f);
    updateKnob('bassGain', preset.bass.g);
    updateKnob('bassQ', preset.bass.q);

    // Apply Dual Band
    this.setDualLowThresh(preset.dualLow.th);
    this.setDualLowRatio(preset.dualLow.r);
    this.setDualLowAttack(preset.dualLow.a);
    this.setDualLowRelease(preset.dualLow.rel);
    this.setDualLowMakeup(preset.dualLow.mk);
    this.setDualHighThresh(preset.dualHigh.th);
    this.setDualHighRatio(preset.dualHigh.r);
    this.setDualHighAttack(preset.dualHigh.a);
    this.setDualHighRelease(preset.dualHigh.rel);
    this.setDualHighMakeup(preset.dualHigh.mk);

    // Update dual band UI
    updateDualBandUI(preset);

    // Apply Limiter
    this.setLimiterCeiling(preset.limiter.ceiling);
    this.setLimiterRelease(preset.limiter.rel);
    this.setLimiterOutputGain(preset.limiter.gain);

    // Apply Stereo
    this.setStereoWidth(preset.stereo.width);
    this.setStereoBalance(preset.stereo.balance);
    updateKnob('stereoWidth', preset.stereo.width);
    updateKnob('stereoBalance', preset.stereo.balance);

    // Apply MB Compressor
    preset.mbComp.forEach((c, i) => {
      this.setMbCompBand(i, c.th, c.r, c.a, c.rel, c.mk);
    });
    updateMbCompUI(preset.mbComp);

    // Rebuild chain to apply all changes
    if (this.active) {
      this.buildAudioChain();
    }
  }
}

// ======================== GLOBAL INSTANCE ========================
const processor = new BroadcastProcessor();

// ======================== UI INITIALIZATION ========================
function initUI() {
  // Enumerate audio devices
  enumerateDevices();
  navigator.mediaDevices.addEventListener('devicechange', enumerateDevices);

  // Build Parametric EQ UI
  buildParamEqUI();

  // Build Graphic EQ UI
  buildGeqUI();

  // Build Multiband Compressor UI
  buildMbCompUI();

  // Initialize knobs
  initKnobs();

  // Setup event listeners
  setupEventListeners();

  // Start animation loop
  requestAnimationFrame(animateMeters);
  requestAnimationFrame(animateSpectrum);
  updateClock();
  setInterval(updateClock, 1000);
}

async function enumerateDevices() {
  try {
    // Need permission first
    await navigator.mediaDevices.getUserMedia({ audio: true }).then(s => s.getTracks().forEach(t => t.stop()));
  } catch(e) {}

  const devices = await navigator.mediaDevices.enumerateDevices();
  const audioInputs = devices.filter(d => d.kind === 'audioinput');
  const audioOutputs = devices.filter(d => d.kind === 'audiooutput');

  const inputSelect = document.getElementById('inputDevice');
  const outputSelect = document.getElementById('outputDevice');

  inputSelect.innerHTML = '<option value="">— Default Input —</option>';
  audioInputs.forEach(d => {
    const opt = document.createElement('option');
    opt.value = d.deviceId;
    opt.textContent = d.label || `Input ${d.deviceId.slice(0,8)}`;
    inputSelect.appendChild(opt);
  });

  outputSelect.innerHTML = '<option value="">Default Output</option>';
  audioOutputs.forEach(d => {
    const opt = document.createElement('option');
    opt.value = d.deviceId;
    opt.textContent = d.label || `Output ${d.deviceId.slice(0,8)}`;
    outputSelect.appendChild(opt);
  });
}

// ======================== PARAMETRIC EQ UI ========================
function buildParamEqUI() {
  const container = document.getElementById('paramEqBands');
  container.innerHTML = '';

  processor.paramEqBandsConfig.forEach((band, i) => {
    const strip = document.createElement('div');
    strip.className = 'band-strip';
    strip.innerHTML = `
      <div class="band-name">${band.name}</div>
      <div class="band-freq">${band.range}</div>
      <div class="band-control">
        <label>Gain</label>
        <input type="range" class="param-eq-gain" data-band="${i}" min="-12" max="12" step="0.5" value="${band.gain}">
        <div class="range-value" id="peqGain${i}">${band.gain > 0 ? '+' : ''}${band.gain} dB</div>
      </div>
      <div class="band-control">
        <label>Frequency</label>
        <input type="range" class="param-eq-freq" data-band="${i}" min="20" max="20000" step="1" value="${band.freq}">
        <div class="range-value" id="peqFreq${i}">${formatFreq(band.freq)}</div>
      </div>
      <div class="band-control">
        <label>Q</label>
        <input type="range" class="param-eq-q" data-band="${i}" min="0.1" max="10" step="0.1" value="${band.Q}">
        <div class="range-value" id="peqQ${i}">${band.Q}</div>
      </div>
    `;
    container.appendChild(strip);
  });
}

function updateParamEqUI(bands) {
  bands.forEach((b, i) => {
    processor.setParamEqBand(i, b.f, b.g, b.q);
    document.querySelector(`.param-eq-gain[data-band="${i}"]`).value = b.g;
    document.querySelector(`.param-eq-freq[data-band="${i}"]`).value = b.f;
    document.querySelector(`.param-eq-q[data-band="${i}"]`).value = b.q;
    document.getElementById(`peqGain${i}`).textContent = `${b.g > 0 ? '+' : ''}${b.g} dB`;
    document.getElementById(`peqFreq${i}`).textContent = formatFreq(b.f);
    document.getElementById(`peqQ${i}`).textContent = b.q;
  });
  drawParamEqCurve();
}

function updateGeqUI(gains) {
  gains.forEach((g, i) => {
    processor.setGeqBand(i, g);
    document.querySelectorAll('.geq-slider')[i].value = g;
    document.querySelectorAll('.geq-db')[i].textContent = `${g > 0 ? '+' : ''}${g} dB`;
  });
  drawParamEqCurve();
}

function updateDualBandUI(preset) {
  const ids = ['dualLowThresh','dualLowRatio','dualLowAttack','dualLowRelease','dualLowMakeup',
               'dualHighThresh','dualHighRatio','dualHighAttack','dualHighRelease','dualHighMakeup'];
  const vals = [preset.dualLow.th, preset.dualLow.r, preset.dualLow.a, preset.dualLow.rel, preset.dualLow.mk,
                preset.dualHigh.th, preset.dualHigh.r, preset.dualHigh.a, preset.dualHigh.rel, preset.dualHigh.mk];
  ids.forEach((id, i) => {
    document.getElementById(id).value = vals[i];
  });
  // Update display values
  document.getElementById('dualLowThreshVal').textContent = `${vals[0]} dB`;
  document.getElementById('dualLowRatioVal').textContent = `${vals[1]}:1`;
  document.getElementById('dualLowAttackVal').textContent = `${Math.round(vals[2]*1000)}ms`;
  document.getElementById('dualLowReleaseVal').textContent = `${Math.round(vals[3]*1000)}ms`;
  document.getElementById('dualLowMakeupVal').textContent = `${vals[4]} dB`;
  document.getElementById('dualHighThreshVal').textContent = `${vals[5]} dB`;
  document.getElementById('dualHighRatioVal').textContent = `${vals[6]}:1`;
  document.getElementById('dualHighAttackVal').textContent = `${Math.round(vals[7]*1000)}ms`;
  document.getElementById('dualHighReleaseVal').textContent = `${Math.round(vals[8]*1000)}ms`;
  document.getElementById('dualHighMakeupVal').textContent = `${vals[9]} dB`;
}

function updateMbCompUI(bands) {
  bands.forEach((b, i) => {
    processor.setMbCompBand(i, b.th, b.r, b.a, b.rel, b.mk);
    const container = document.querySelectorAll('.comp-band')[i];
    if (container) {
      container.querySelector('.comp-threshold').value = b.th;
      container.querySelector('.comp-ratio').value = b.r;
      container.querySelector('.comp-attack').value = b.a;
      container.querySelector('.comp-release').value = b.rel;
      container.querySelector('.comp-makeup').value = b.mk;
      container.querySelector('.comp-thresh-val').textContent = `${b.th} dB`;
      container.querySelector('.comp-ratio-val').textContent = `${b.r}:1`;
      container.querySelector('.comp-attack-val').textContent = `${Math.round(b.a*1000)}ms`;
      container.querySelector('.comp-release-val').textContent = `${Math.round(b.rel*1000)}ms`;
      container.querySelector('.comp-makeup-val').textContent = `${b.mk} dB`;
    }
  });
}

// ======================== GRAPHIC EQ UI ========================
function buildGeqUI() {
  const container = document.getElementById('geqBands');
  container.innerHTML = '';

  processor.geqFreqs.forEach((freq, i) => {
    const band = document.createElement('div');
    band.className = 'geq-band';
    band.innerHTML = `
      <input type="range" class="geq-slider" data-band="${i}" min="-12" max="12" step="0.5" value="0">
      <div class="geq-db" id="geqDb${i}">0 dB</div>
      <div class="geq-freq">${formatFreq(freq)}</div>
    `;
    container.appendChild(band);
  });
}

// ======================== MULTIBAND COMPRESSOR UI ========================
function buildMbCompUI() {
  const container = document.getElementById('mbCompBands');
  container.innerHTML = '';

  processor.mbCompConfig.forEach((band, i) => {
    const div = document.createElement('div');
    div.className = 'comp-band';
    div.innerHTML = `
      <div class="comp-band-header">
        <span class="comp-band-title">${band.name} (${formatFreq(band.lowFreq)}–${formatFreq(band.highFreq)})</span>
      </div>
      <div class="comp-controls">
        <div class="comp-control">
          <label>Threshold</label>
          <input type="range" class="comp-threshold" data-band="${i}" min="-60" max="0" step="0.5" value="${band.threshold}">
          <div class="range-value comp-thresh-val">${band.threshold} dB</div>
        </div>
        <div class="comp-control">
          <label>Ratio</label>
          <input type="range" class="comp-ratio" data-band="${i}" min="1" max="20" step="0.5" value="${band.ratio}">
          <div class="range-value comp-ratio-val">${band.ratio}:1</div>
        </div>
        <div class="comp-control">
          <label>Attack</label>
          <input type="range" class="comp-attack" data-band="${i}" min="0.001" max="0.1" step="0.001" value="${band.attack}">
          <div class="range-value comp-attack-val">${Math.round(band.attack*1000)}ms</div>
        </div>
        <div class="comp-control">
          <label>Release</label>
          <input type="range" class="comp-release" data-band="${i}" min="0.01" max="1" step="0.01" value="${band.release}">
          <div class="range-value comp-release-val">${Math.round(band.release*1000)}ms</div>
        </div>
        <div class="comp-control">
          <label>Makeup</label>
          <input type="range" class="comp-makeup" data-band="${i}" min="0" max="18" step="0.5" value="${band.makeup}">
          <div class="range-value comp-makeup-val">${band.makeup} dB</div>
        </div>
      </div>
    `;
    container.appendChild(div);
  });
}

// ======================== KNOB SYSTEM ========================
function initKnobs() {
  document.querySelectorAll('.knob-container').forEach(knob => {
    setupKnob(knob);
  });
}

function setupKnob(knob) {
  const min = parseFloat(knob.dataset.min);
  const max = parseFloat(knob.dataset.max);
  const value = parseFloat(knob.dataset.value);
  const unit = knob.dataset.unit;

  updateKnobVisual(knob, value, min, max, unit);

  let isDragging = false;
  let startY = 0;
  let startValue = value;

  knob.addEventListener('mousedown', (e) => {
    isDragging = true;
    startY = e.clientY;
    startValue = parseFloat(knob.dataset.value);
    e.preventDefault();
  });

  document.addEventListener('mousemove', (e) => {
    if (!isDragging) return;
    const delta = (startY - e.clientY) * ((max - min) / 150);
    let newValue = Math.max(min, Math.min(max, startValue + delta));
    newValue = Math.round(newValue * 10) / 10;
    knob.dataset.value = newValue;
    updateKnobVisual(knob, newValue, min, max, unit);
    onKnobChange(knob.dataset.knob, newValue);
  });

  document.addEventListener('mouseup', () => {
    isDragging = false;
  });

  // Touch support
  knob.addEventListener('touchstart', (e) => {
    isDragging = true;
    startY = e.touches[0].clientY;
    startValue = parseFloat(knob.dataset.value);
    e.preventDefault();
  });

  document.addEventListener('touchmove', (e) => {
    if (!isDragging) return;
    const delta = (startY - e.touches[0].clientY) * ((max - min) / 150);
    let newValue = Math.max(min, Math.min(max, startValue + delta));
    newValue = Math.round(newValue * 10) / 10;
    knob.dataset.value = newValue;
    updateKnobVisual(knob, newValue, min, max, unit);
    onKnobChange(knob.dataset.knob, newValue);
  });

  document.addEventListener('touchend', () => {
    isDragging = false;
  });
}

function updateKnob(knobId, value) {
  const knob = document.querySelector(`[data-knob="${knobId}"]`);
  if (knob) {
    knob.dataset.value = value;
    updateKnobVisual(knob, value, parseFloat(knob.dataset.min), parseFloat(knob.dataset.max), knob.dataset.unit);
  }
}

function updateKnobVisual(knob, value, min, max, unit) {
  const circumference = 2 * Math.PI * 26; // r=26
  const fraction = (value - min) / (max - min);
  const offset = circumference * (1 - fraction * 0.75); // 75% of circle for range

  const fill = knob.querySelector('.knob-fill');
  fill.setAttribute('stroke-dashoffset', offset);

  const centerText = knob.querySelector('.knob-center-text');
  if (centerText) {
    if (knob.dataset.knob === 'stereoBalance') {
      centerText.textContent = value === 0 ? 'C' : (value > 0 ? `R${value}` : `L${Math.abs(value)}`);
    } else if (knob.dataset.knob === 'stereoPhase') {
      centerText.textContent = `${Math.round(value)}°`;
    } else {
      centerText.textContent = unit ? `${Math.round(value)}` : `${value}`;
    }
  }
}

function onKnobChange(knobId, value) {
  switch(knobId) {
    case 'bassFreq':
      processor.setBassFreq(value);
      document.getElementById('bassFreqText').textContent = Math.round(value);
      break;
    case 'bassGain':
      processor.setBassGain(value);
      document.getElementById('bassGainText').textContent = value;
      break;
    case 'bassQ':
      processor.setBassQ(value);
      document.getElementById('bassQText').textContent = value;
      break;
    case 'stereoWidth':
      processor.setStereoWidth(value);
      document.getElementById('stereoWidthText').textContent = Math.round(value);
      break;
    case 'stereoBalance':
      processor.setStereoBalance(value);
      document.getElementById('stereoBalanceText').textContent = value === 0 ? 'C' : (value > 0 ? `R${Math.round(value)}` : `L${Math.round(Math.abs(value))}`);
      break;
    case 'stereoPhase':
      document.getElementById('stereoPhaseText').textContent = `${Math.round(value)}°`;
      break;
  }
}

// ======================== EVENT LISTENERS ========================
function setupEventListeners() {
  // Power button
  document.getElementById('powerBtn').addEventListener('click', () => {
    processor.togglePower();
  });

  // Input gain
  document.getElementById('inputGain').addEventListener('input', (e) => {
    const v = parseFloat(e.target.value);
    processor.setInputGain(v);
    const db = v > 0 ? 20 * Math.log10(v) : -Infinity;
    document.getElementById('inputGainVal').textContent = db === -Infinity ? '-∞ dB' : `${db.toFixed(1)} dB`;
  });

  // Master volume
  document.getElementById('masterVolume').addEventListener('input', (e) => {
    const v = parseFloat(e.target.value);
    processor.setMasterVolume(v);
    const db = v > 0 ? 20 * Math.log10(v) : -Infinity;
    document.getElementById('masterVolumeVal').textContent = db === -Infinity ? '-∞ dB' : `${db.toFixed(1)} dB`;
  });

  // Parametric EQ events
  document.querySelectorAll('.param-eq-gain').forEach(el => {
    el.addEventListener('input', (e) => {
      const i = parseInt(e.target.dataset.band);
      const gain = parseFloat(e.target.value);
      const freq = parseFloat(document.querySelector(`.param-eq-freq[data-band="${i}"]`).value);
      const Q = parseFloat(document.querySelector(`.param-eq-q[data-band="${i}"]`).value);
      processor.setParamEqBand(i, freq, gain, Q);
      document.getElementById(`peqGain${i}`).textContent = `${gain > 0 ? '+' : ''}${gain} dB`;
      drawParamEqCurve();
    });
  });

  document.querySelectorAll('.param-eq-freq').forEach(el => {
    el.addEventListener('input', (e) => {
      const i = parseInt(e.target.dataset.band);
      const freq = parseFloat(e.target.value);
      const gain = parseFloat(document.querySelector(`.param-eq-gain[data-band="${i}"]`).value);
      const Q = parseFloat(document.querySelector(`.param-eq-q[data-band="${i}"]`).value);
      processor.setParamEqBand(i, freq, gain, Q);
      document.getElementById(`peqFreq${i}`).textContent = formatFreq(freq);
      drawParamEqCurve();
    });
  });

  document.querySelectorAll('.param-eq-q').forEach(el => {
    el.addEventListener('input', (e) => {
      const i = parseInt(e.target.dataset.band);
      const Q = parseFloat(e.target.value);
      const freq = parseFloat(document.querySelector(`.param-eq-freq[data-band="${i}"]`).value);
      const gain = parseFloat(document.querySelector(`.param-eq-gain[data-band="${i}"]`).value);
      processor.setParamEqBand(i, freq, gain, Q);
      document.getElementById(`peqQ${i}`).textContent = Q;
      drawParamEqCurve();
    });
  });

  // Graphic EQ events
  document.querySelectorAll('.geq-slider').forEach(el => {
    el.addEventListener('input', (e) => {
      const i = parseInt(e.target.dataset.band);
      const gain = parseFloat(e.target.value);
      processor.setGeqBand(i, gain);
      document.querySelectorAll('.geq-db')[i].textContent = `${gain > 0 ? '+' : ''}${gain} dB`;
      drawParamEqCurve();
    });
  });

  // Multiband Compressor events
  document.querySelectorAll('.comp-threshold').forEach(el => {
    el.addEventListener('input', (e) => {
      const i = parseInt(e.target.dataset.band);
      updateMbCompFromUI(i);
    });
  });
  document.querySelectorAll('.comp-ratio').forEach(el => {
    el.addEventListener('input', (e) => { updateMbCompFromUI(parseInt(e.target.dataset.band)); });
  });
  document.querySelectorAll('.comp-attack').forEach(el => {
    el.addEventListener('input', (e) => { updateMbCompFromUI(parseInt(e.target.dataset.band)); });
  });
  document.querySelectorAll('.comp-release').forEach(el => {
    el.addEventListener('input', (e) => { updateMbCompFromUI(parseInt(e.target.dataset.band)); });
  });
  document.querySelectorAll('.comp-makeup').forEach(el => {
    el.addEventListener('input', (e) => { updateMbCompFromUI(parseInt(e.target.dataset.band)); });
  });

  // Dual Band Compressor events
  ['dualLowThresh','dualLowRatio','dualLowAttack','dualLowRelease','dualLowMakeup','dualLowCrossover'].forEach(id => {
    document.getElementById(id).addEventListener('input', (e) => {
      const v = parseFloat(e.target.value);
      const fnMap = {
        'dualLowThresh': v => { processor.setDualLowThresh(v); e.target.previousElementSibling.textContent = `${v} dB`; },
        'dualLowRatio': v => { processor.setDualLowRatio(v); e.target.previousElementSibling.textContent = `${v}:1`; },
        'dualLowAttack': v => { processor.setDualLowAttack(v); e.target.previousElementSibling.textContent = `${Math.round(v*1000)}ms`; },
        'dualLowRelease': v => { processor.setDualLowRelease(v); e.target.previousElementSibling.textContent = `${Math.round(v*1000)}ms`; },
        'dualLowMakeup': v => { processor.setDualLowMakeup(v); e.target.previousElementSibling.textContent = `${v} dB`; },
        'dualLowCrossover': v => { processor.setDualLowCrossover(v); e.target.previousElementSibling.textContent = `${formatFreq(v)}`; }
      };
      if (fnMap[id]) fnMap[id](v);
    });
  });

  ['dualHighThresh','dualHighRatio','dualHighAttack','dualHighRelease','dualHighMakeup','dualHighPresence'].forEach(id => {
    document.getElementById(id).addEventListener('input', (e) => {
      const v = parseFloat(e.target.value);
      const fnMap = {
        'dualHighThresh': v => { processor.setDualHighThresh(v); e.target.previousElementSibling.textContent = `${v} dB`; },
        'dualHighRatio': v => { processor.setDualHighRatio(v); e.target.previousElementSibling.textContent = `${v}:1`; },
        'dualHighAttack': v => { processor.setDualHighAttack(v); e.target.previousElementSibling.textContent = `${Math.round(v*1000)}ms`; },
        'dualHighRelease': v => { processor.setDualHighRelease(v); e.target.previousElementSibling.textContent = `${Math.round(v*1000)}ms`; },
        'dualHighMakeup': v => { processor.setDualHighMakeup(v); e.target.previousElementSibling.textContent = `${v} dB`; },
        'dualHighPresence': v => { processor.setDualHighPresence(v); e.target.previousElementSibling.textContent = `${v} dB`; }
      };
      if (fnMap[id]) fnMap[id](v);
    });
  });

  // Limiter events
  document.getElementById('limiterCeiling').addEventListener('input', (e) => {
    const v = parseFloat(e.target.value);
    processor.setLimiterCeiling(v);
    e.target.previousElementSibling.textContent = `${v} dB`;
  });
  document.getElementById('limiterRelease').addEventListener('input', (e) => {
    const v = parseFloat(e.target.value);
    processor.setLimiterRelease(v);
    e.target.previousElementSibling.textContent = `${Math.round(v*1000)}ms`;
  });
  document.getElementById('limiterOutputGain').addEventListener('input', (e) => {
    const v = parseFloat(e.target.value);
    processor.setLimiterOutputGain(v);
    e.target.previousElementSibling.textContent = `${v} dB`;
  });

  // Bypass toggles
  document.getElementById('paramEqBypass').addEventListener('change', (e) => {
    processor.paramEqBypass = e.target.checked;
    if (processor.active) processor.buildAudioChain();
  });
  document.getElementById('geqBypass').addEventListener('change', (e) => {
    processor.geqBypass = e.target.checked;
    if (processor.active) processor.buildAudioChain();
  });
  document.getElementById('mbCompBypass').addEventListener('change', (e) => {
    processor.mbCompBypass = e.target.checked;
    if (processor.active) processor.buildAudioChain();
  });
  document.getElementById('dualCompBypass').addEventListener('change', (e) => {
    processor.dualCompBypass = e.target.checked;
    if (processor.active) processor.buildAudioChain();
  });
  document.getElementById('bassBypass').addEventListener('change', (e) => {
    processor.bassBypass = e.target.checked;
    if (processor.active) processor.buildAudioChain();
  });
  document.getElementById('stereoBypass').addEventListener('change', (e) => {
    processor.stereoBypass = e.target.checked;
    if (processor.active) processor.buildAudioChain();
  });

  // Presets
  document.querySelectorAll('.preset-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.preset-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      processor.applyPreset(btn.dataset.preset);
    });
  });
}

function updateMbCompFromUI(index) {
  const container = document.querySelectorAll('.comp-band')[index];
  if (!container) return;
  const th = parseFloat(container.querySelector('.comp-threshold').value);
  const r = parseFloat(container.querySelector('.comp-ratio').value);
  const a = parseFloat(container.querySelector('.comp-attack').value);
  const rel = parseFloat(container.querySelector('.comp-release').value);
  const mk = parseFloat(container.querySelector('.comp-makeup').value);

  processor.setMbCompBand(index, th, r, a, rel, mk);

  container.querySelector('.comp-thresh-val').textContent = `${th} dB`;
  container.querySelector('.comp-ratio-val').textContent = `${r}:1`;
  container.querySelector('.comp-attack-val').textContent = `${Math.round(a*1000)}ms`;
  container.querySelector('.comp-release-val').textContent = `${Math.round(rel*1000)}ms`;
  container.querySelector('.comp-makeup-val').textContent = `${mk} dB`;
}

// ======================== HELPERS ========================
function formatFreq(f) {
  if (f >= 1000) return (f/1000).toFixed(f >= 10000 ? 0 : 1) + 'k';
  return Math.round(f).toString();
}

function updateClock() {
  if (processor.active && processor.startTime) {
    const elapsed = Math.floor((Date.now() - processor.startTime) / 1000);
    const h = String(Math.floor(elapsed / 3600)).padStart(2, '0');
    const m = String(Math.floor((elapsed % 3600) / 60)).padStart(2, '0');
    const s = String(elapsed % 60).padStart(2, '0');
    document.getElementById('timeDisplay').textContent = `${h}:${m}:${s}`;
  } else {
    document.getElementById('timeDisplay').textContent = '00:00:00';
  }
}

// ======================== VU METERS ========================
function animateMeters() {
  if (processor.active && processor.inputAnalyser && processor.outputAnalyser) {
    // Input meter
    const inputData = new Uint8Array(processor.inputAnalyser.fftSize);
    processor.inputAnalyser.getByteTimeDomainData(inputData);
    const inputRMS = calculateRMS(inputData);
    processor.inputLevel = inputRMS;
    drawMeter('inputMeter', inputRMS, 'inputDb');

    // Output meter
    const outputData = new Uint8Array(processor.outputAnalyser.fftSize);
    processor.outputAnalyser.getByteTimeDomainData(outputData);
    const outputRMS = calculateRMS(outputData);
    processor.outputLevel = outputRMS;
    drawMeter('outputMeter', outputRMS, 'outputDb');

    // Clip detection
    if (outputRMS > 0.95) {
      document.getElementById('clipIndicator').classList.add('active');
      clearTimeout(processor.clipTimeout);
      processor.clipTimeout = setTimeout(() => {
        document.getElementById('clipIndicator').classList.remove('active');
      }, 150);
    }
  } else {
    drawMeter('inputMeter', 0, 'inputDb');
    drawMeter('outputMeter', 0, 'outputDb');
  }

  // Update latency display
  if (processor.ctx) {
    const baseLatency = processor.ctx.baseLatency || 0;
    const outputLatency = processor.ctx.outputLatency || 0;
    document.getElementById('latencyDisplay').textContent = `${((baseLatency + outputLatency) * 1000).toFixed(1)} ms`;
  }

  requestAnimationFrame(animateMeters);
}

function calculateRMS(data) {
  let sum = 0;
  for (let i = 0; i < data.length; i++) {
    const sample = (data[i] - 128) / 128;
    sum += sample * sample;
  }
  return Math.sqrt(sum / data.length);
}

function drawMeter(canvasId, rms, dbId) {
  const canvas = document.getElementById(canvasId);
  const ctx = canvas.getContext('2d');
  const w = canvas.width;
  const h = canvas.height;

  ctx.clearRect(0, 0, w, h);

  // Background
  ctx.fillStyle = '#0a0a0f';
  ctx.fillRect(0, 0, w, h);

  // Scale
  const db = rms > 0 ? 20 * Math.log10(rms) : -Infinity;
  const level = Math.max(0, (db + 60) / 60); // Map -60dB to 0dB

  // Gradient segments
  const segments = 60;
  const segH = h / segments;

  for (let i = 0; i < segments; i++) {
    const y = h - (i + 1) * segH;
    const threshold = i / segments;

    if (level >= threshold) {
      if (i < segments * 0.6) {
        ctx.fillStyle = '#00ff88';
      } else if (i < segments * 0.8) {
        ctx.fillStyle = '#ffcc00';
      } else {
        ctx.fillStyle = '#ff3344';
      }
    } else {
      ctx.fillStyle = '#1a1a25';
    }

    ctx.fillRect(2, y, w - 4, segH - 1);
  }

  // Peak indicator
  if (rms > 0) {
    const peakY = h - level * h;
    ctx.fillStyle = '#ffffff';
    ctx.fillRect(2, peakY, w - 4, 2);
  }

  // dB label
  const dbEl = document.getElementById(dbId);
  if (dbEl) {
    dbEl.textContent = db === -Infinity ? '-∞ dB' : `${db.toFixed(1)} dB`;
    if (db > -3) {
      dbEl.style.color = '#ff3344';
    } else if (db > -12) {
      dbEl.style.color = '#ffcc00';
    } else {
      dbEl.style.color = '#00ff88';
    }
  }
}

// ======================== SPECTRUM ANALYZER ========================
function animateSpectrum() {
  const canvas = document.getElementById('spectrumCanvas');
  const ctx = canvas.getContext('2d');

  // Set canvas size
  canvas.width = canvas.offsetWidth * 2;
  canvas.height = canvas.offsetHeight * 2;

  const w = canvas.width;
  const h = canvas.height;

  ctx.clearRect(0, 0, w, h);

  if (processor.active && processor.spectrumAnalyser) {
    const bufferLength = processor.spectrumAnalyser.frequencyBinCount;
    const dataArray = new Uint8Array(bufferLength);
    processor.spectrumAnalyser.getByteFrequencyData(dataArray);

    // Draw spectrum
    const barCount = 128;
    const barWidth = w / barCount;
    const step = Math.floor(bufferLength / barCount);

    for (let i = 0; i < barCount; i++) {
      let sum = 0;
      for (let j = 0; j < step; j++) {
        sum += dataArray[i * step + j];
      }
      const avg = sum / step;
      const barHeight = (avg / 255) * h * 0.9;

      // Color based on frequency
      const hue = 200 - (i / barCount) * 160; // Blue to pink
      const saturation = 80;
      const lightness = 40 + (avg / 255) * 30;

      ctx.fillStyle = `hsl(${hue}, ${saturation}%, ${lightness}%)`;
      ctx.fillRect(i * barWidth, h - barHeight, barWidth - 1, barHeight);

      // Glow effect
      ctx.fillStyle = `hsla(${hue}, ${saturation}%, ${lightness + 20}%, 0.3)`;
      ctx.fillRect(i * barWidth, h - barHeight, barWidth - 1, 4);
    }

    // Grid lines
    ctx.strokeStyle = 'rgba(255,255,255,0.05)';
    ctx.lineWidth = 1;
    for (let i = 0; i < 5; i++) {
      const y = (h / 5) * i;
      ctx.beginPath();
      ctx.moveTo(0, y);
      ctx.lineTo(w, y);
      ctx.stroke();
    }

    // Update loudness display
    let totalEnergy = 0;
    for (let i = 0; i < bufferLength; i++) {
      totalEnergy += dataArray[i];
    }
    const avgEnergy = totalEnergy / bufferLength;
    const estimatedLUFS = -60 + (avgEnergy / 255) * 60;
    document.getElementById('loudnessVal').textContent = estimatedLUFS.toFixed(1);
    document.getElementById('loudnessVal').style.color = estimatedLUFS > -14 ? '#ff8800' : '#00d4ff';

    // Peak
    let peak = 0;
    for (let i = 0; i < bufferLength; i++) {
      if (dataArray[i] > peak) peak = dataArray[i];
    }
    const peakDb = peak > 0 ? 20 * Math.log10(peak / 255) : -Infinity;
    document.getElementById('peakVal').textContent = peakDb === -Infinity ? '-∞' : peakDb.toFixed(1);
    document.getElementById('peakVal').style.color = peakDb > -3 ? '#ff3344' : '#00ff88';

    // Dynamic range
    const dynamicRange = estimatedLUFS - (peakDb === -Infinity ? -60 : peakDb);
    document.getElementById('dynamicVal').textContent = `${dynamicRange.toFixed(1)} dB`;
  } else {
    // Draw flat spectrum when inactive
    ctx.fillStyle = '#1a1a25';
    ctx.fillRect(0, 0, w, h);
    ctx.fillStyle = '#333';
    ctx.font = '24px monospace';
    ctx.textAlign = 'center';
    ctx.fillText('SPECTRUM ANALYZER', w/2, h/2);
  }

  // Draw Parametric EQ curve
  drawParamEqCurve();

  requestAnimationFrame(animateSpectrum);
}

// ======================== PARAMETRIC EQ CURVE DRAWING ========================
function drawParamEqCurve() {
  const canvas = document.getElementById('paramEqCanvas');
  if (!canvas) return;

  const ctx = canvas.getContext('2d');
  canvas.width = canvas.offsetWidth * 2;
  canvas.height = canvas.offsetHeight * 2;

  const w = canvas.width;
  const h = canvas.height;
  const padding = { top: 10, bottom: 10, left: 10, right: 10 };

  ctx.clearRect(0, 0, w, h);

  // Background
  ctx.fillStyle = '#0a0a0f';
  ctx.fillRect(0, 0, w, h);

  // Grid
  ctx.strokeStyle = 'rgba(255,255,255,0.05)';
  ctx.lineWidth = 1;

  // Horizontal grid (dB lines)
  for (let db = -12; db <= 12; db += 6) {
    const y = padding.top + (h - padding.top - padding.bottom) * (1 - (db + 12) / 24);
    ctx.beginPath();
    ctx.moveTo(padding.left, y);
    ctx.lineTo(w - padding.right, y);
    ctx.stroke();

    ctx.fillStyle = 'rgba(255,255,255,0.2)';
    ctx.font = '16px monospace';
    ctx.textAlign = 'right';
    ctx.fillText(`${db > 0 ? '+' : ''}${db}dB`, padding.left + 30, y + 4);
  }

  // Vertical grid (frequency lines)
  const freqMin = 20;
  const freqMax = 20000;
  const logFreqMin = Math.log10(freqMin);
  const logFreqMax = Math.log10(freqMax);

  const freqLabels = [20, 50, 100, 200, 500, 1000, 2000, 5000, 10000, 20000];
  freqLabels.forEach(freq => {
    const logFreq = Math.log10(freq);
    const x = padding.left + ((logFreq - logFreqMin) / (logFreqMax - logFreqMin)) * (w - padding.left - padding.right);
    ctx.beginPath();
    ctx.moveTo(x, padding.top);
    ctx.lineTo(x, h - padding.bottom);
    ctx.stroke();

    ctx.fillStyle = 'rgba(255,255,255,0.2)';
    ctx.font = '14px monospace';
    ctx.textAlign = 'center';
    ctx.fillText(formatFreq(freq), x, h - padding.bottom + 16);
  });

  // Center line (0 dB)
  ctx.strokeStyle = 'rgba(0, 212, 255, 0.2)';
  ctx.beginPath();
  const centerY = padding.top + (h - padding.top - padding.bottom) * 0.5;
  ctx.moveTo(padding.left, centerY);
  ctx.lineTo(w - padding.right, centerY);
  ctx.stroke();

  // Draw EQ curve
  ctx.beginPath();
  ctx.strokeStyle = '#00d4ff';
  ctx.lineWidth = 3;
  ctx.shadowColor = 'rgba(0, 212, 255, 0.5)';
  ctx.shadowBlur = 8;

  const plotWidth = w - padding.left - padding.right;
  const plotHeight = h - padding.top - padding.bottom;

  for (let px = 0; px <= plotWidth; px++) {
    const logFreq = logFreqMin + (px / plotWidth) * (logFreqMax - logFreqMin);
    const freq = Math.pow(10, logFreq);

    let totalGain = 0;

    // Parametric EQ
    if (!processor.paramEqBypass) {
      processor.paramEqFilters.forEach(filter => {
        const biquadGain = getBiquadGainAtFreq(filter, freq);
        totalGain += biquadGain;
      });
    }

    // Graphic EQ
    if (!processor.geqBypass) {
      processor.geqFilters.forEach(filter => {
        const g = getBiquadGainAtFreq(filter, freq);
        totalGain += g;
      });
    }

    // Bass
    if (!processor.bassBypass && processor.bassFilter) {
      totalGain += getBiquadGainAtFreq(processor.bassFilter, freq);
    }

    // Map to Y
    const clampedGain = Math.max(-12, Math.min(12, totalGain));
    const y = padding.top + plotHeight * (1 - (clampedGain + 12) / 24);

    if (px === 0) {
      ctx.moveTo(padding.left + px, y);
    } else {
      ctx.lineTo(padding.left + px, y);
    }
  }

  ctx.stroke();
  ctx.shadowBlur = 0;

  // Fill under curve
  ctx.lineTo(w - padding.right, centerY);
  ctx.lineTo(padding.left, centerY);
  ctx.closePath();
  ctx.fillStyle = 'rgba(0, 212, 255, 0.05)';
  ctx.fill();

  // Band markers
  processor.paramEqBandsConfig.forEach((band, i) => {
    if (processor.paramEqBypass) return;
    const logFreq = Math.log10(band.freq);
    const x = padding.left + ((logFreq - logFreqMin) / (logFreqMax - logFreqMin)) * plotWidth;
    const y = padding.top + plotHeight * (1 - (Math.max(-12, Math.min(12, band.gain)) + 12) / 24);

    ctx.beginPath();
    ctx.arc(x, y, 6, 0, Math.PI * 2);
    ctx.fillStyle = '#00d4ff';
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
  });
}

function getBiquadGainAtFreq(filter, freq) {
  // Calculate the gain of a biquad filter at a specific frequency
  const magResponse = filter.getFrequencyResponse
    ? (() => {
        const freqs = new Float32Array([freq]);
        const mag = new Float32Array(1);
        const phase = new Float32Array(1);
        filter.getFrequencyResponse(freqs, mag, phase);
        return 20 * Math.log10(mag[0]);
      })()
    : 0;

  return magResponse;
}

// ======================== INIT ========================
window.addEventListener('DOMContentLoaded', initUI);
window.addEventListener('resize', () => {
  drawParamEqCurve();
});
</script>
</body>
</html>
