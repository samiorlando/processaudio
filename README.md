<!DOCTYPE html>
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
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-track { background: var(--bg-dark); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: var(--border-light); }

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
.logo { display: flex; align-items: center; gap: 12px; }
.logo svg { filter: drop-shadow(0 0 8px var(--accent-glow)); }
.logo h1 {
  font-size: 1.2rem; font-weight: 700;
  background: linear-gradient(135deg, var(--accent), #8866ff);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; letter-spacing: 1px;
}
.logo span { font-size: 0.65rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 2px; }
.header-controls { display: flex; align-items: center; gap: 16px; }

.power-btn {
  width: 48px; height: 48px; border-radius: 50%; border: 2px solid var(--red);
  background: rgba(255, 51, 68, 0.1); cursor: pointer; display: flex;
  align-items: center; justify-content: center; transition: all 0.3s;
}
.power-btn:hover { box-shadow: 0 0 20px var(--red-glow); }
.power-btn.active { border-color: var(--green); background: rgba(0, 255, 136, 0.1); box-shadow: 0 0 20px var(--green-glow); }
.power-btn svg { width: 24px; height: 24px; }
.power-btn.active svg { color: var(--green); }

.main-container { max-width: 1600px; margin: 0 auto; padding: 16px; }
.top-row { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 16px; }
.bottom-section { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-bottom: 16px; }
.full-section { margin-bottom: 16px; }

.panel { background: var(--bg-panel); border: 1px solid var(--border); border-radius: 12px; overflow: hidden; }
.panel-header {
  background: linear-gradient(180deg, var(--bg-panel-light) 0%, var(--bg-panel) 100%);
  padding: 10px 16px; display: flex; align-items: center; justify-content: space-between;
  border-bottom: 1px solid var(--border);
}
.panel-title { font-size: 0.75rem; text-transform: uppercase; letter-spacing: 2px; color: var(--accent); font-weight: 600; }
.panel-badge {
  font-size: 0.6rem; padding: 2px 8px; border-radius: 10px;
  background: rgba(0, 212, 255, 0.15); color: var(--accent); border: 1px solid rgba(0, 212, 255, 0.3);
}
.panel-body { padding: 16px; }

.bypass-toggle { display: flex; align-items: center; gap: 8px; }
.bypass-toggle label { font-size: 0.65rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; }
.toggle-switch { position: relative; width: 36px; height: 18px; cursor: pointer; }
.toggle-switch input { display: none; }
.toggle-slider {
  position: absolute; inset: 0; background: #333; border-radius: 9px; transition: 0.3s;
}
.toggle-slider::before {
  content: ''; position: absolute; width: 14px; height: 14px; left: 2px; top: 2px;
  background: #666; border-radius: 50%; transition: 0.3s;
}
.toggle-switch input:checked + .toggle-slider { background: rgba(0, 255, 136, 0.3); }
.toggle-switch input:checked + .toggle-slider::before { transform: translateX(18px); background: var(--green); box-shadow: 0 0 6px var(--green-glow); }

.io-section { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; }
.io-group label { display: block; font-size: 0.65rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; }
.io-group select {
  width: 100%; background: var(--bg-control); color: var(--text); border: 1px solid var(--border);
  border-radius: 6px; padding: 8px 10px; font-size: 0.8rem; outline: none; cursor: pointer;
}
.io-group select:focus { border-color: var(--accent); }
.io-group select:disabled { opacity: 0.5; cursor: not-allowed; }

input[type="range"] {
  -webkit-appearance: none; appearance: none; height: 6px; background: var(--bg-control);
  border-radius: 3px; outline: none; cursor: pointer; width: 100%;
}
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none; width: 16px; height: 16px; border-radius: 50%;
  background: var(--accent); border: 2px solid #fff; box-shadow: 0 0 8px var(--accent-glow); cursor: pointer;
}
input[type="range"].green::-webkit-slider-thumb { background: var(--green); box-shadow: 0 0 8px var(--green-glow); }
input[type="range"].red::-webkit-slider-thumb { background: var(--red); box-shadow: 0 0 8px var(--red-glow); }
.range-value { text-align: center; font-size: 0.75rem; color: var(--accent); margin-top: 4px; font-family: 'Courier New', monospace; font-weight: 600; }

.meter-container { display: flex; flex-direction: column; align-items: center; gap: 4px; }
.meter-container canvas { border-radius: 4px; border: 1px solid var(--border); }
.meter-label { font-size: 0.6rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; }
.meter-db { font-size: 0.7rem; font-family: 'Courier New', monospace; color: var(--green); font-weight: 600; }

.knob-control { display: flex; flex-direction: column; align-items: center; gap: 6px; }
.knob-container { position: relative; width: var(--knob-size); height: var(--knob-size); cursor: ns-resize; }
.knob-svg { width: 100%; height: 100%; transform: rotate(-90deg); }
.knob-bg { fill: none; stroke: var(--bg-control); stroke-width: 4; }
.knob-fill { fill: none; stroke: var(--accent); stroke-width: 4; stroke-linecap: round; transition: stroke-dashoffset 0.1s; filter: drop-shadow(0 0 4px var(--accent-glow)); }
.knob-fill.green { stroke: var(--green); filter: drop-shadow(0 0 4px var(--green-glow)); }
.knob-fill.orange { stroke: var(--orange); }
.knob-center-text { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); font-size: 0.65rem; font-family: 'Courier New', monospace; font-weight: 700; color: var(--text); }
.knob-label { font-size: 0.6rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; text-align: center; }

.eq-display { background: var(--bg-control); border: 1px solid var(--border); border-radius: 8px; padding: 8px; margin-bottom: 12px; }
.eq-display canvas { width: 100%; height: 100px; border-radius: 4px; }

.bands-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; }
.band-strip { background: var(--bg-control); border: 1px solid var(--border); border-radius: 8px; padding: 10px 8px; display: flex; flex-direction: column; align-items: center; gap: 8px; }
.band-name { font-size: 0.55rem; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; text-align: center; }
.band-freq { font-size: 0.6rem; color: var(--accent); font-family: 'Courier New', monospace; }
.band-control { width: 100%; }
.band-control label { display: block; font-size: 0.5rem; color: var(--text-dim); margin-bottom: 2px; }
.band-control input[type="range"] { height: 4px; }
.band-control input[type="range"]::-webkit-slider-thumb { width: 12px; height: 12px; }
.band-control .range-value { font-size: 0.55rem; }

.geq-container { display: flex; align-items: flex-end; justify-content: space-between; gap: 4px; padding: 10px 0; }
.geq-band { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 4px; min-width: 30px; }
.geq-band input[type="range"] { writing-mode: vertical-lr; direction: rtl; height: 120px; width: 6px; }
.geq-freq { font-size: 0.5rem; color: var(--text-dim); font-family: 'Courier New', monospace; }
.geq-db { font-size: 0.5rem; color: var(--accent); font-family: 'Courier New', monospace; }

.comp-band { background: var(--bg-control); border: 1px solid var(--border); border-radius: 8px; padding: 12px; margin-bottom: 8px; }
.comp-band-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
.comp-band-title { font-size: 0.65rem; color: var(--accent); text-transform: uppercase; letter-spacing: 1px; font-weight: 600; }
.comp-controls { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; }
.comp-control { display: flex; flex-direction: column; align-items: center; gap: 4px; }
.comp-control label { font-size: 0.5rem; color: var(--text-dim); text-transform: uppercase; }

.spectrum-container { background: var(--bg-control); border: 1px solid var(--border); border-radius: 8px; padding: 8px; }
.spectrum-container canvas { width: 100%; height: 150px; border-radius: 4px; }
.presets-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 12px; }
.preset-btn {
  padding: 6px 16px; border: 1px solid var(--border); background: var(--bg-control);
  color: var(--text-dim); border-radius: 20px; font-size: 0.7rem; cursor: pointer;
  transition: all 0.3s; text-transform: uppercase; letter-spacing: 1px;
}
.preset-btn:hover { border-color: var(--accent); color: var(--accent); background: rgba(0, 212, 255, 0.1); }
.preset-btn.active { border-color: var(--accent); background: rgba(0, 212, 255, 0.15); color: var(--accent); box-shadow: 0 0 12px var(--accent-glow); }

.status-bar {
  background: var(--bg-panel); border: 1px solid var(--border); border-radius: 8px;
  padding: 8px 16px; display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 16px; font-size: 0.65rem; color: var(--text-dim); flex-wrap: wrap; gap: 8px;
}
.status-item { display: flex; align-items: center; gap: 6px; }
.status-dot { width: 8px; height: 8px; border-radius: 50%; background: #555; transition: 0.3s; }
.status-dot.on { background: var(--green); box-shadow: 0 0 8px var(--green-glow); }
.clip-indicator { width: 10px; height: 10px; border-radius: 50%; background: #333; transition: all 0.05s; }
.clip-indicator.active { background: var(--red); box-shadow: 0 0 12px var(--red-glow); }

.dual-band { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.dual-band-strip { background: var(--bg-control); border: 1px solid var(--border); border-radius: 8px; padding: 12px; }
.dual-band-strip h4 { font-size: 0.7rem; color: var(--accent); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 10px; text-align: center; }
.dual-controls { display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; }
.dual-control { display: flex; flex-direction: column; align-items: center; gap: 4px; }
.dual-control label { font-size: 0.5rem; color: var(--text-dim); text-transform: uppercase; }
.bass-controls { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
.stereo-knobs { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
.loudness-display { display: flex; align-items: center; gap: 12px; padding: 8px 0; flex-wrap: wrap; }
.loudness-value { font-size: 1.2rem; font-family: 'Courier New', monospace; font-weight: 700; color: var(--accent); }
.loudness-label { font-size: 0.6rem; color: var(--text-dim); }

@media (max-width: 1200px) { .bottom-section { grid-template-columns: 1fr; } .top-row { grid-template-columns: 1fr; } .bands-grid { grid-template-columns: repeat(3, 1fr); } .comp-controls { grid-template-columns: repeat(3, 1fr); } }
@media (max-width: 768px) { .io-section { grid-template-columns: 1fr; } .bands-grid { grid-template-columns: repeat(2, 1fr); } .comp-controls { grid-template-columns: repeat(2, 1fr); } .geq-container { overflow-x: auto; } }
</style>
</head>
<body>

<header class="header">
  <div class="logo">
    <svg width="36" height="36" viewBox="0 0 36 36" fill="none">
      <circle cx="18" cy="18" r="16" stroke="url(#grad1)" stroke-width="2"/>
      <path d="M18 8v20M12 14v8M24 10v16M8 16v4M28 12v12" stroke="url(#grad1)" stroke-width="2" stroke-linecap="round"/>
      <defs><linearGradient id="grad1" x1="0" y1="0" x2="36" y2="36"><stop stop-color="#00d4ff"/><stop offset="1" stop-color="#8866ff"/></linearGradient></defs>
    </svg>
    <div>
      <h1>BROADCAST PROCESSOR PRO</h1>
      <span>Real-Time Audio Engine v2.1</span>
    </div>
  </div>
  <div class="header-controls">
    <div style="display:flex;align-items:center;gap:8px;">
      <span style="font-size:0.6rem;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px;">DSP</span>
      <div class="clip-indicator" id="clipIndicator"></div>
    </div>
    <div class="power-btn" id="powerBtn" title="Iniciar / Detener DSP">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
        <path d="M12 3v9M18.36 6.64a9 9 0 1 1-12.73 0"/>
      </svg>
    </div>
  </div>
</header>

<div class="main-container">
  <div class="status-bar">
    <div class="status-item">
      <div class="status-dot" id="statusDot"></div>
      <span id="statusText">Esperando inicio...</span>
    </div>
    <div class="status-item">Sample Rate: <b id="sampleRateDisplay">—</b></div>
    <div class="status-item">Latencia: <b id="latencyDisplay">—</b></div>
    <div class="status-item" id="timeDisplay">00:00:00</div>
  </div>

  <div class="top-row">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🎤 Entrada de Audio</span>
        <span class="panel-badge">INPUT</span>
      </div>
      <div class="panel-body">
        <div class="io-section">
          <div class="io-group">
            <label>Dispositivo</label>
            <select id="inputDevice" disabled><option>Cargando...</option></select>
          </div>
          <div class="io-group">
            <label>Ganancia</label>
            <input type="range" id="inputGain" min="0" max="2" step="0.01" value="1">
            <div class="range-value" id="inputGainVal">0.0 dB</div>
          </div>
          <div class="meter-container">
            <canvas id="inputMeter" width="60" height="200"></canvas>
            <div class="meter-db" id="inputDb">-∞ dB</div>
            <div class="meter-label">Nivel Entrada</div>
          </div>
        </div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">🔊 Salida de Audio</span>
        <span class="panel-badge">OUTPUT</span>
      </div>
      <div class="panel-body">
        <div class="io-section">
          <div class="io-group">
            <label>Dispositivo</label>
            <select id="outputDevice" disabled><option>Cargando...</option></select>
          </div>
          <div class="io-group">
            <label>Volumen Master</label>
            <input type="range" id="masterVolume" class="green" min="0" max="1.5" step="0.01" value="0.8">
            <div class="range-value" id="masterVolumeVal">-1.9 dB</div>
          </div>
          <div class="meter-container">
            <canvas id="outputMeter" width="60" height="200"></canvas>
            <div class="meter-db" id="outputDb">-∞ dB</div>
            <div class="meter-label">Nivel Salida</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="full-section">
    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">⚡ Presets Rápidos</span>
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
        <div class="eq-display"><canvas id="paramEqCanvas" height="100"></canvas></div>
        <div class="bands-grid" id="paramEqBands"></div>
      </div>
    </div>
  </div>

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
      <div class="panel-body"><div id="mbCompBands"></div></div>
    </div>
  </div>

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
              <svg class="knob-svg" viewBox="0 0 60 60"><circle class="knob-bg" cx="30" cy="30" r="26"/><circle class="knob-fill green" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="81.68"/></svg>
              <div class="knob-center-text" id="bassFreqText">80</div>
            </div>
            <div class="knob-label">Frequency</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="bassGain" data-min="-12" data-max="12" data-value="4" data-unit="dB">
              <svg class="knob-svg" viewBox="0 0 60 60"><circle class="knob-bg" cx="30" cy="30" r="26"/><circle class="knob-fill green" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="65.34"/></svg>
              <div class="knob-center-text" id="bassGainText">4</div>
            </div>
            <div class="knob-label">Boost</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="bassQ" data-min="0.1" data-max="3" data-value="0.7" data-unit="">
              <svg class="knob-svg" viewBox="0 0 60 60"><circle class="knob-bg" cx="30" cy="30" r="26"/><circle class="knob-fill green" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="100.74"/></svg>
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
              <svg class="knob-svg" viewBox="0 0 60 60"><circle class="knob-bg" cx="30" cy="30" r="26"/><circle class="knob-fill orange" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="81.68"/></svg>
              <div class="knob-center-text" id="stereoWidthText">100</div>
            </div>
            <div class="knob-label">Width</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="stereoBalance" data-min="-100" data-max="100" data-value="0" data-unit="">
              <svg class="knob-svg" viewBox="0 0 60 60"><circle class="knob-bg" cx="30" cy="30" r="26"/><circle class="knob-fill orange" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="81.68"/></svg>
              <div class="knob-center-text" id="stereoBalanceText">C</div>
            </div>
            <div class="knob-label">Balance</div>
          </div>
          <div class="knob-control">
            <div class="knob-container" data-knob="stereoPhase" data-min="0" data-max="180" data-value="0" data-unit="°">
              <svg class="knob-svg" viewBox="0 0 60 60"><circle class="knob-bg" cx="30" cy="30" r="26"/><circle class="knob-fill orange" cx="30" cy="30" r="26" stroke-dasharray="163.36" stroke-dashoffset="163.36"/></svg>
              <div class="knob-center-text" id="stereoPhaseText">0°</div>
            </div>
            <div class="knob-label">Phase</div>
          </div>
        </div>
      </div>
    </div>

    <div class="panel">
      <div class="panel-header">
        <span class="panel-title">📈 Analizador de Espectro</span>
        <span class="panel-badge">FFT</span>
      </div>
      <div class="panel-body">
        <div class="spectrum-container"><canvas id="spectrumCanvas"></canvas></div>
        <div class="loudness-display">
          <div><div class="loudness-value" id="loudnessVal">-∞</div><div class="loudness-label">Estimación LUFS</div></div>
          <div><div class="loudness-value" id="peakVal">-∞</div><div class="loudness-label">True Peak dBFS</div></div>
          <div><div class="loudness-value" id="dynamicVal">—</div><div class="loudness-label">Rango Dinámico</div></div>
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
    this.stream = null;
    this.source = null;

    this.inputGain = null;
    this.outputGain = null;

    this.paramEqFilters = [];
    this.geqFilters = [];
    this.mbCompressorBands = [];
    this.mbCompBypass = false;

    this.dualLowSplit = null; this.dualHighSplit = null;
    this.dualLowComp = null; this.dualHighComp = null;
    this.dualLowGain = null; this.dualHighGain = null;
    this.dualCompBypass = false;

    this.bassFilter = null; this.bassBypass = false;
    this.stereoSplitter = null; this.stereoMerger = null;
    this.stereoLeftGain = null; this.stereoRightGain = null;
    this.stereoBypass = false;

    this.limiter = null; this.limiterGain = null;
    this.paramEqBypass = false; this.geqBypass = false;

    this.inputAnalyser = null; this.outputAnalyser = null; this.spectrumAnalyser = null;
    this.mbCompressorOutput = null; this.dualBandOutput = null; this.stereoOutput = null;

    this.paramEqBandsConfig = [
      { freq: 40, gain: 0, Q: 1.0, type: 'lowshelf', name: 'Sub Bass', range: '20-60Hz' },
      { freq: 150, gain: 0, Q: 1.0, type: 'peaking', name: 'Bass', range: '60-250Hz' },
      { freq: 1000, gain: 0, Q: 1.0, type: 'peaking', name: 'Mid', range: '250-2kHz' },
      { freq: 4000, gain: 0, Q: 1.0, type: 'peaking', name: 'High Mid', range: '2k-6kHz' },
      { freq: 10000, gain: 0, Q: 1.0, type: 'highshelf', name: 'Treble', range: '6k-16kHz' }
    ];
    this.geqFreqs = [31, 62, 125, 250, 500, 1000, 2000, 4000, 8000, 12000, 16000];
    this.mbCompConfig = [
      { name: 'Sub Bass', lowFreq: 20, highFreq: 80, threshold: -30, ratio: 4, attack: 0.015, release: 0.15, makeup: 3 },
      { name: 'Bass', lowFreq: 80, highFreq: 250, threshold: -24, ratio: 4, attack: 0.01, release: 0.12, makeup: 2 },
      { name: 'Mid', lowFreq: 250, highFreq: 2000, threshold: -20, ratio: 3, attack: 0.008, release: 0.08, makeup: 1 },
      { name: 'High Mid', lowFreq: 2000, highFreq: 6000, threshold: -18, ratio: 3, attack: 0.005, release: 0.06, makeup: 1 },
      { name: 'Treble', lowFreq: 6000, highFreq: 16000, threshold: -20, ratio: 2.5, attack: 0.003, release: 0.05, makeup: 1 }
    ];
  }

  async init() {
    this.ctx = new (window.AudioContext || window.webkitAudioContext)({ latencyHint: 'interactive', sampleRate: 48000 });
    document.getElementById('sampleRateDisplay').textContent = `${this.ctx.sampleRate} Hz`;

    this.inputGain = this.ctx.createGain();
    this.outputGain = this.ctx.createGain();
    this.inputAnalyser = this.ctx.createAnalyser(); this.inputAnalyser.fftSize = 2048;
    this.outputAnalyser = this.ctx.createAnalyser(); this.outputAnalyser.fftSize = 2048;
    this.spectrumAnalyser = this.ctx.createAnalyser(); this.spectrumAnalyser.fftSize = 4096; this.spectrumAnalyser.smoothingTimeConstant = 0.8;

    this.paramEqFilters = this.paramEqBandsConfig.map(b => {
      const f = this.ctx.createBiquadFilter(); f.type = b.type; f.frequency.value = b.freq; f.gain.value = b.gain; f.Q.value = b.Q; return f;
    });
    this.geqFilters = this.geqFreqs.map(f => {
      const filter = this.ctx.createBiquadFilter(); filter.type = 'peaking'; filter.frequency.value = f; filter.gain.value = 0; filter.Q.value = 1.4; return filter;
    });
    this.mbCompressorBands = this.mbCompConfig.map(cfg => {
      const lp = this.ctx.createBiquadFilter(); lp.type = 'lowpass'; lp.frequency.value = cfg.highFreq; lp.Q.value = 0.707;
      const hp = this.ctx.createBiquadFilter(); hp.type = 'highpass'; hp.frequency.value = cfg.lowFreq; hp.Q.value = 0.707;
      const comp = this.ctx.createDynamicsCompressor(); comp.threshold.value = cfg.threshold; comp.ratio.value = cfg.ratio; comp.attack.value = cfg.attack; comp.release.value = cfg.release; comp.knee.value = 6;
      const mg = this.ctx.createGain(); mg.gain.value = Math.pow(10, cfg.makeup / 20);
      return { ...cfg, lowpass: lp, highpass: hp, compressor: comp, makeupGain: mg };
    });

    this.dualLowSplit = this.ctx.createBiquadFilter(); this.dualLowSplit.type = 'lowpass'; this.dualLowSplit.frequency.value = 2000;
    this.dualHighSplit = this.ctx.createBiquadFilter(); this.dualHighSplit.type = 'highpass'; this.dualHighSplit.frequency.value = 2000;
    this.dualLowComp = this.ctx.createDynamicsCompressor(); Object.assign(this.dualLowComp, { threshold: -24, ratio: 4, attack: 0.01, release: 0.1 });
    this.dualHighComp = this.ctx.createDynamicsCompressor(); Object.assign(this.dualHighComp, { threshold: -18, ratio: 3, attack: 0.005, release: 0.08 });
    this.dualLowGain = this.ctx.createGain(); this.dualLowGain.gain.value = Math.pow(10, 3/20);
    this.dualHighGain = this.ctx.createGain(); this.dualHighGain.gain.value = Math.pow(10, 2/20);

    this.bassFilter = this.ctx.createBiquadFilter(); this.bassFilter.type = 'lowshelf'; this.bassFilter.frequency.value = 80; this.bassFilter.gain.value = 4; this.bassFilter.Q.value = 0.7;

    this.stereoSplitter = this.ctx.createChannelSplitter(2); this.stereoMerger = this.ctx.createChannelMerger(2);
    this.stereoLeftGain = this.ctx.createGain(); this.stereoRightGain = this.ctx.createGain(); this.stereoLeftGain.gain.value = 1; this.stereoRightGain.gain.value = 1;

    this.limiter = this.ctx.createDynamicsCompressor(); Object.assign(this.limiter, { threshold: -0.3, ratio: 20, attack: 0.001, release: 0.05, knee: 0 });
    this.limiterGain = this.ctx.createGain(); this.limiterGain.gain.value = Math.pow(10, 4/20);

    this.outputGain.connect(this.inputAnalyser);
    this.inputAnalyser.connect(this.ctx.destination);
    this.outputGain.connect(this.spectrumAnalyser);

    this.active = true;
    this.startTime = Date.now();
    this.updateStatus(true);
  }

  async startAudio() {
    if (!this.ctx) await this.init();
    if (this.ctx.state === 'suspended') await this.ctx.resume();

    const deviceId = document.getElementById('inputDevice').value;
    const constraints = deviceId ? { audio: { deviceId: { exact: deviceId }, echoCancellation: false, noiseSuppression: false, autoGainControl: false } } : { audio: { echoCancellation: false, noiseSuppression: false, autoGainControl: false } };

    try {
      this.stream = await navigator.mediaDevices.getUserMedia(constraints);
      this.source = this.ctx.createMediaStreamSource(this.stream);
      this.source.connect(this.inputGain);
      this.buildAudioChain();
      this.startTime = Date.now();
      return true;
    } catch (err) {
      console.error('Error audio input:', err);
      alert('No se pudo acceder al micrófono. Verifica los permisos del navegador.');
      return false;
    }
  }

  async switchInput(deviceId) {
    if (!this.active || !this.ctx) return;
    if (this.source) this.source.disconnect();
    if (this.stream) this.stream.getTracks().forEach(t => t.stop());

    const constraints = deviceId ? { audio: { deviceId: { exact: deviceId }, echoCancellation: false, noiseSuppression: false, autoGainControl: false } } : { audio: { echoCancellation: false, noiseSuppression: false, autoGainControl: false } };
    this.stream = await navigator.mediaDevices.getUserMedia(constraints);
    this.source = this.ctx.createMediaStreamSource(this.stream);
    this.source.connect(this.inputGain);
    this.buildAudioChain();
  }

  buildAudioChain() {
    if (!this.active) return;
    this.disconnectChain();

    let chain = this.inputGain;

    if (!this.paramEqBypass) {
      chain.connect(this.paramEqFilters[0]);
      for (let i = 0; i < this.paramEqFilters.length - 1; i++) this.paramEqFilters[i].connect(this.paramEqFilters[i + 1]);
      chain = this.paramEqFilters[this.paramEqFilters.length - 1];
    }

    if (!this.mbCompBypass) {
      this.mbCompressorOutput = this.ctx.createGain();
      this.mbCompressorBands.forEach(band => {
        chain.connect(band.lowpass);
        band.lowpass.connect(band.highpass);
        band.highpass.connect(band.compressor);
        band.compressor.connect(band.makeupGain);
        band.makeupGain.connect(this.mbCompressorOutput);
      });
      chain = this.mbCompressorOutput;
    }

    if (!this.geqBypass) {
      chain.connect(this.geqFilters[0]);
      for (let i = 0; i < this.geqFilters.length - 1; i++) this.geqFilters[i].connect(this.geqFilters[i + 1]);
      chain = this.geqFilters[this.geqFilters.length - 1];
    }

    if (!this.dualCompBypass) {
      this.dualBandOutput = this.ctx.createGain();
      chain.connect(this.dualLowSplit); chain.connect(this.dualHighSplit);
      this.dualLowSplit.connect(this.dualLowComp).connect(this.dualLowGain).connect(this.dualBandOutput);
      this.dualHighSplit.connect(this.dualHighComp).connect(this.dualHighGain).connect(this.dualBandOutput);
      chain = this.dualBandOutput;
    }

    if (!this.bassBypass) { chain.connect(this.bassFilter); chain = this.bassFilter; }

    if (!this.stereoBypass) {
      this.stereoOutput = this.ctx.createGain();
      chain.connect(this.stereoSplitter);
      this.stereoSplitter.connect(this.stereoLeftGain, 0); this.stereoSplitter.connect(this.stereoRightGain, 1);
      this.stereoLeftGain.connect(this.stereoMerger, 0, 0); this.stereoRightGain.connect(this.stereoMerger, 0, 1);
      this.stereoMerger.connect(this.stereoOutput);
      chain = this.stereoOutput;
    }

    chain.connect(this.limiter).connect(this.limiterGain).connect(this.outputGain);
  }

  disconnectChain() {
    const safeDisconnect = (node) => { try { node?.disconnect(); } catch(e){} };
    [this.mbCompressorOutput, this.dualBandOutput, this.stereoOutput].forEach(safeDisconnect);
    this.paramEqFilters.forEach(safeDisconnect);
    this.geqFilters.forEach(safeDisconnect);
    this.mbCompressorBands.forEach(b => { safeDisconnect(b.lowpass); safeDisconnect(b.highpass); safeDisconnect(b.compressor); safeDisconnect(b.makeupGain); });
    safeDisconnect(this.dualLowSplit); safeDisconnect(this.dualHighSplit); safeDisconnect(this.dualLowComp); safeDisconnect(this.dualHighComp);
    safeDisconnect(this.dualLowGain); safeDisconnect(this.dualHighGain);
    safeDisconnect(this.bassFilter);
    safeDisconnect(this.stereoSplitter); safeDisconnect(this.stereoMerger); safeDisconnect(this.stereoLeftGain); safeDisconnect(this.stereoRightGain);
    safeDisconnect(this.limiter); safeDisconnect(this.limiterGain);

    this.outputGain.disconnect();
    this.outputGain.connect(this.inputAnalyser).connect(this.ctx.destination);
    this.outputGain.connect(this.spectrumAnalyser);
  }

  stopAudio() {
    if (this.source) { try { this.source.disconnect(); } catch(e){} this.source = null; }
    if (this.stream) { this.stream.getTracks().forEach(t => t.stop()); this.stream = null; }
    this.active = false;
    this.updateStatus(false);
  }

  togglePower() {
    if (!this.active) this.startAudio().then(ok => { document.getElementById('powerBtn').classList.toggle('active', ok); });
    else { this.stopAudio(); document.getElementById('powerBtn').classList.remove('active'); }
  }

  updateStatus(isOn) {
    const dot = document.getElementById('statusDot');
    const txt = document.getElementById('statusText');
    if (isOn) { dot.classList.add('on'); dot.classList.remove('off'); txt.textContent = 'DSP ACTIVO — Procesando señal'; }
    else { dot.classList.remove('on'); txt.textContent = 'DSP OFF — Haz clic en Power para iniciar'; }
  }

  setInputGain(v) { if (this.inputGain) this.inputGain.gain.value = v; }
  setMasterVolume(v) { if (this.outputGain) this.outputGain.gain.value = v; }

  setParamEqBand(i, f, g, q) {
    if (this.paramEqFilters[i]) { const fl = this.paramEqFilters[i]; fl.frequency.value = f; fl.gain.value = g; fl.Q.value = q; this.paramEqBandsConfig[i] = { ...this.paramEqBandsConfig[i], freq: f, gain: g, Q: q }; }
  }
  setGeqBand(i, g) { if (this.geqFilters[i]) this.geqFilters[i].gain.value = g; }

  setBassFreq(f) { if (this.bassFilter) this.bassFilter.frequency.value = f; }
  setBassGain(g) { if (this.bassFilter) this.bassFilter.gain.value = g; }
  setBassQ(q) { if (this.bassFilter) this.bassFilter.Q.value = q; }

  setStereoWidth(w) {
    if (this.stereoLeftGain && this.stereoRightGain) {
      const s = w / 100; this.stereoLeftGain.gain.value = 0.5 + 0.5 * s; this.stereoRightGain.gain.value = 0.5 + 0.5 * s;
    }
  }
  setStereoBalance(b) {
    if (this.stereoLeftGain && this.stereoRightGain) {
      this.stereoLeftGain.gain.value = b <= 0 ? 1 : 1 - (b/100); this.stereoRightGain.gain.value = b >= 0 ? 1 : 1 + (b/100);
    }
  }

  setDualLowThresh(v) { if(this.dualLowComp) this.dualLowComp.threshold.value = v; }
  setDualLowRatio(v) { if(this.dualLowComp) this.dualLowComp.ratio.value = v; }
  setDualLowAttack(v) { if(this.dualLowComp) this.dualLowComp.attack.value = v; }
  setDualLowRelease(v) { if(this.dualLowComp) this.dualLowComp.release.value = v; }
  setDualLowMakeup(v) { if(this.dualLowGain) this.dualLowGain.gain.value = Math.pow(10, v/20); }
  setDualLowCrossover(v) { if(this.dualLowSplit) this.dualLowSplit.frequency.value = v; if(this.dualHighSplit) this.dualHighSplit.frequency.value = v; }

  setDualHighThresh(v) { if(this.dualHighComp) this.dualHighComp.threshold.value = v; }
  setDualHighRatio(v) { if(this.dualHighComp) this.dualHighComp.ratio.value = v; }
  setDualHighAttack(v) { if(this.dualHighComp) this.dualHighComp.attack.value = v; }
  setDualHighRelease(v) { if(this.dualHighComp) this.dualHighComp.release.value = v; }
  setDualHighMakeup(v) { if(this.dualHighGain) this.dualHighGain.gain.value = Math.pow(10, v/20); }

  setLimiterCeiling(v) { if(this.limiter) this.limiter.threshold.value = v; }
  setLimiterRelease(v) { if(this.limiter) this.limiter.release.value = v; }
  setLimiterOutputGain(v) { if(this.limiterGain) this.limiterGain.gain.value = Math.pow(10, v/20); }

  setMbCompBand(i, th, r, a, rel, mk) {
    const b = this.mbCompressorBands[i]; if(!b) return;
    b.compressor.threshold.value = th; b.compressor.ratio.value = r; b.compressor.attack.value = a; b.compressor.release.value = rel; b.makeupGain.gain.value = Math.pow(10, mk/20);
  }

  applyPreset(name) {
    const P = {
      flat: { paramEq: [{f:40,g:0,q:1},{f:150,g:0,q:1},{f:1000,g:0,q:1},{f:4000,g:0,q:1},{f:10000,g:0,q:1}], geq: Array(11).fill(0), bass:{f:80,g:0,q:0.7}, dualL:{th:-24,r:4,a:0.01,rel:0.1,mk:3,c:2000}, dualH:{th:-18,r:3,a:0.005,rel:0.08,mk:2}, lim:{ce:-0.3,rel:0.05,g:4}, stereo:{w:100,b:0}, mb: [{th:-30,r:4,a:0.015,rel:0.15,mk:3},{th:-24,r:4,a:0.01,rel:0.12,mk:2},{th:-20,r:3,a:0.008,rel:0.08,mk:1},{th:-18,r:3,a:0.005,rel:0.06,mk:1},{th:-20,r:2.5,a:0.003,rel:0.05,mk:1}] },
      radioFM: { paramEq: [{f:40,g:2,q:1.2},{f:150,g:1,q:1},{f:1000,g:3,q:0.8},{f:4000,g:4,q:1},{f:10000,g:2,q:1}], geq: [1,2,1,0,1,3,3,2,1,1,0], bass:{f:80,g:3,q:0.8}, dualL:{th:-28,r:6,a:0.008,rel:0.08,mk:5,c:2000}, dualH:{th:-22,r:4,a:0.003,rel:0.05,mk:4}, lim:{ce:-0.3,rel:0.03,g:8}, stereo:{w:120,b:0}, mb: [{th:-28,r:6,a:0.01,rel:0.1,mk:5},{th:-24,r:5,a:0.008,rel:0.08,mk:4},{th:-20,r:4,a:0.005,rel:0.06,mk:3},{th:-18,r:4,a:0.003,rel:0.04,mk:3},{th:-16,r:3,a:0.002,rel:0.03,mk:2}] },
      voiceClear: { paramEq: [{f:40,g:-6,q:1},{f:150,g:-3,q:1},{f:1000,g:4,q:0.7},{f:4000,g:5,q:0.8},{f:10000,g:2,q:1}], geq: [-4,-2,0,2,4,5,4,2,0,-1,-2], bass:{f:80,g:-4,q:0.7}, dualL:{th:-30,r:3,a:0.015,rel:0.15,mk:2,c:2000}, dualH:{th:-20,r:4,a:0.005,rel:0.06,mk:5}, lim:{ce:-0.5,rel:0.05,g:5}, stereo:{w:80,b:0}, mb: [{th:-30,r:3,a:0.015,rel:0.15,mk:2},{th:-26,r:3,a:0.01,rel:0.12,mk:1},{th:-18,r:4,a:0.005,rel:0.06,mk:4},{th:-16,r:4,a:0.003,rel:0.04,mk:4},{th:-20,r:2.5,a:0.003,rel:0.05,mk:2}] },
      musicPower: { paramEq: [{f:40,g:5,q:1.2},{f:150,g:3,q:1},{f:1000,g:1,q:0.8},{f:4000,g:2,q:1},{f:10000,g:4,q:1}], geq: [4,3,2,1,0,1,2,3,3,2,1], bass:{f:60,g:6,q:0.8}, dualL:{th:-26,r:5,a:0.01,rel:0.1,mk:4,c:2000}, dualH:{th:-18,r:3.5,a:0.005,rel:0.07,mk:3}, lim:{ce:-0.3,rel:0.04,g:7}, stereo:{w:140,b:0}, mb: [{th:-26,r:5,a:0.01,rel:0.1,mk:4},{th:-22,r:4.5,a:0.008,rel:0.08,mk:3},{th:-18,r:3,a:0.005,rel:0.06,mk:2},{th:-16,r:3,a:0.003,rel:0.04,mk:2},{th:-18,r:2.5,a:0.003,rel:0.05,mk:2}] },
      ultraBass: { paramEq: [{f:40,g:10,q:1.4},{f:150,g:6,q:1.2},{f:1000,g:-2,q:0.8},{f:4000,g:-1,q:1},{f:10000,g:1,q:1}], geq: [8,6,4,2,0,-1,0,1,1,0,0], bass:{f:50,g:10,q:0.9}, dualL:{th:-32,r:8,a:0.015,rel:0.12,mk:8,c:2000}, dualH:{th:-16,r:2.5,a:0.005,rel:0.08,mk:2}, lim:{ce:-0.5,rel:0.05,g:6}, stereo:{w:100,b:0}, mb: [{th:-32,r:8,a:0.015,rel:0.12,mk:8},{th:-26,r:6,a:0.01,rel:0.1,mk:6},{th:-20,r:3,a:0.005,rel:0.06,mk:2},{th:-18,r:2.5,a:0.003,rel:0.04,mk:1},{th:-20,r:2,a:0.003,rel:0.05,mk:1}] },
      brilliant: { paramEq: [{f:40,g:0,q:1},{f:150,g:0,q:1},{f:1000,g:1,q:0.8},{f:4000,g:5,q:0.7},{f:10000,g:8,q:1}], geq: [0,0,0,0,1,2,3,5,6,5,4], bass:{f:80,g:0,q:0.7}, dualL:{th:-24,r:4,a:0.01,rel:0.1,mk:2,c:2000}, dualH:{th:-20,r:3,a:0.003,rel:0.05,mk:5}, lim:{ce:-0.3,rel:0.04,g:5}, stereo:{w:130,b:0}, mb: [{th:-28,r:4,a:0.012,rel:0.12,mk:3},{th:-24,r:4,a:0.01,rel:0.1,mk:2},{th:-20,r:3,a:0.006,rel:0.07,mk:2},{th:-16,r:3.5,a:0.003,rel:0.04,mk:4},{th:-14,r:3,a:0.002,rel:0.03,mk:5}] },
      podcast: { paramEq: [{f:40,g:-4,q:1},{f:150,g:-2,q:1},{f:1000,g:3,q:0.8},{f:4000,g:4,q:0.9},{f:10000,g:1,q:1}], geq: [-3,-2,-1,1,3,4,3,1,0,-1,-2], bass:{f:100,g:-2,q:0.7}, dualL:{th:-30,r:3,a:0.015,rel:0.15,mk:2,c:2000}, dualH:{th:-22,r:4,a:0.005,rel:0.06,mk:4}, lim:{ce:-0.5,rel:0.05,g:6}, stereo:{w:60,b:0}, mb: [{th:-30,r:3,a:0.015,rel:0.15,mk:2},{th:-26,r:3,a:0.012,rel:0.12,mk:1},{th:-20,r:4,a:0.006,rel:0.07,mk:3},{th:-18,r:4,a:0.004,rel:0.05,mk:3},{th:-20,r:2.5,a:0.003,rel:0.05,mk:1}] },
      liveBand: { paramEq: [{f:40,g:4,q:1.2},{f:150,g:2,q:1},{f:1000,g:2,q:0.8},{f:4000,g:3,q:0.9},{f:10000,g:3,q:1}], geq: [3,2,1,1,0,1,2,2,3,2,1], bass:{f:60,g:4,q:0.8}, dualL:{th:-26,r:5,a:0.01,rel:0.1,mk:4,c:2000}, dualH:{th:-20,r:3.5,a:0.005,rel:0.07,mk:3}, lim:{ce:-0.3,rel:0.04,g:6}, stereo:{w:150,b:0}, mb: [{th:-26,r:5,a:0.01,rel:0.1,mk:4},{th:-22,r:4,a:0.008,rel:0.08,mk:3},{th:-20,r:3.5,a:0.006,rel:0.07,mk:2},{th:-18,r:3.5,a:0.004,rel:0.05,mk:2},{th:-18,r:3,a:0.003,rel:0.04,mk:2}] }
    };
    const p = P[name]; if (!p) return;
    p.paramEq.forEach((b,i) => this.setParamEqBand(i, b.f, b.g, b.q));
    p.geq.forEach((g,i) => this.setGeqBand(i, g));
    this.setBassFreq(p.bass.f); this.setBassGain(p.bass.g); this.setBassQ(p.bass.q);
    this.setDualLowThresh(p.dualL.th); this.setDualLowRatio(p.dualL.r); this.setDualLowAttack(p.dualL.a); this.setDualLowRelease(p.dualL.rel); this.setDualLowMakeup(p.dualL.mk); this.setDualLowCrossover(p.dualL.c);
    this.setDualHighThresh(p.dualH.th); this.setDualHighRatio(p.dualH.r); this.setDualHighAttack(p.dualH.a); this.setDualHighRelease(p.dualH.rel); this.setDualHighMakeup(p.dualH.mk);
    this.setLimiterCeiling(p.lim.ce); this.setLimiterRelease(p.lim.rel); this.setLimiterOutputGain(p.lim.g);
    this.setStereoWidth(p.stereo.w); this.setStereoBalance(p.stereo.b);
    p.mb.forEach((b,i) => this.setMbCompBand(i, b.th, b.r, b.a, b.rel, b.mk));
    updateAllUI(p);
    if (this.active) this.buildAudioChain();
  }
}

const processor = new BroadcastProcessor();

// ======================== DEVICE ENUMERATION ========================
async function updateAudioDevices() {
  const inSel = document.getElementById('inputDevice');
  const outSel = document.getElementById('outputDevice');
  try {
    // Request temp permission for labels
    const temp = await navigator.mediaDevices.getUserMedia({ audio: true });
    temp.getTracks().forEach(t => t.stop());

    const devices = await navigator.mediaDevices.enumerateDevices();
    const inputs = devices.filter(d => d.kind === 'audioinput');
    const outputs = devices.filter(d => d.kind === 'audiooutput');

    inSel.innerHTML = '<option value="">— Entrada Predeterminada —</option>';
    inputs.forEach(d => { const o = document.createElement('option'); o.value = d.deviceId; o.textContent = d.label || `Micrófono ${d.deviceId.slice(0,8)}`; inSel.appendChild(o); });
    inSel.disabled = false;

    outSel.innerHTML = '<option value="">— Salida Predeterminada —</option>';
    outputs.forEach(d => { const o = document.createElement('option'); o.value = d.deviceId; o.textContent = d.label || `Salida ${d.deviceId.slice(0,8)}`; outSel.appendChild(o); });
    outSel.disabled = false;
  } catch(e) {
    console.warn('Permisos requeridos para ver nombres de dispositivos');
    inSel.innerHTML = '<option value="">No hay permisos</option>'; inSel.disabled = true;
    outSel.innerHTML = '<option value="">No hay permisos</option>'; outSel.disabled = true;
  }
}

// ======================== UI BUILDERS ========================
function buildParamEqUI() {
  const c = document.getElementById('paramEqBands'); c.innerHTML = '';
  processor.paramEqBandsConfig.forEach((b, i) => {
    const d = document.createElement('div'); d.className = 'band-strip';
    d.innerHTML = `<div class="band-name">${b.name}</div><div class="band-freq">${b.range}</div>
      <div class="band-control"><label>Ganancia</label><input type="range" class="peq-gain" data-i="${i}" min="-12" max="12" step="0.5" value="${b.gain}"><div class="range-value" id="peqG${i}">${b.gain>0?'+':''}${b.gain} dB</div></div>
      <div class="band-control"><label>Frecuencia</label><input type="range" class="peq-freq" data-i="${i}" min="20" max="20000" step="1" value="${b.freq}"><div class="range-value" id="peqF${i}">${formatFreq(b.freq)}</div></div>
      <div class="band-control"><label>Q</label><input type="range" class="peq-q" data-i="${i}" min="0.1" max="10" step="0.1" value="${b.Q}"><div class="range-value" id="peqQ${i}">${b.Q}</div></div>`;
    c.appendChild(d);
  });
}
function buildGeqUI() {
  const c = document.getElementById('geqBands'); c.innerHTML = '';
  processor.geqFreqs.forEach((f, i) => {
    c.innerHTML += `<div class="geq-band"><input type="range" class="geq-slider" data-i="${i}" min="-12" max="12" step="0.5" value="0"><div class="geq-db" id="geqD${i}">0 dB</div><div class="geq-freq">${formatFreq(f)}</div></div>`;
  });
}
function buildMbCompUI() {
  const c = document.getElementById('mbCompBands'); c.innerHTML = '';
  processor.mbCompConfig.forEach((b, i) => {
    c.innerHTML += `<div class="comp-band"><div class="comp-band-header"><span class="comp-band-title">${b.name} (${formatFreq(b.lowFreq)}–${formatFreq(b.highFreq)})</span></div>
      <div class="comp-controls">
        <div class="comp-control"><label>Threshold</label><input type="range" class="mc-th" data-i="${i}" min="-60" max="0" step="0.5" value="${b.threshold}"><div class="range-value" id="mcTh${i}">${b.threshold} dB</div></div>
        <div class="comp-control"><label>Ratio</label><input type="range" class="mc-r" data-i="${i}" min="1" max="20" step="0.5" value="${b.ratio}"><div class="range-value" id="mcR${i}">${b.ratio}:1</div></div>
        <div class="comp-control"><label>Attack</label><input type="range" class="mc-a" data-i="${i}" min="0.001" max="0.1" step="0.001" value="${b.attack}"><div class="range-value" id="mcA${i}">${Math.round(b.attack*1000)}ms</div></div>
        <div class="comp-control"><label>Release</label><input type="range" class="mc-rel" data-i="${i}" min="0.01" max="1" step="0.01" value="${b.release}"><div class="range-value" id="mcRel${i}">${Math.round(b.release*1000)}ms</div></div>
        <div class="comp-control"><label>Makeup</label><input type="range" class="mc-mk" data-i="${i}" min="0" max="18" step="0.5" value="${b.makeup}"><div class="range-value" id="mcMk${i}">${b.makeup} dB</div></div>
      </div></div>`;
  });
}

function updateAllUI(p) {
  p.paramEq.forEach((b,i) => { document.querySelector(`.peq-gain[data-i="${i}"]`).value=b.g; document.querySelector(`.peq-freq[data-i="${i}"]`).value=b.f; document.querySelector(`.peq-q[data-i="${i}"]`).value=b.q; document.getElementById(`peqG${i}`).textContent=`${b.g>0?'+':''}${b.g} dB`; document.getElementById(`peqF${i}`).textContent=formatFreq(b.f); document.getElementById(`peqQ${i}`).textContent=b.q; });
  p.geq.forEach((g,i) => { document.querySelectorAll('.geq-slider')[i].value=g; document.getElementById(`geqD${i}`).textContent=`${g>0?'+':''}${g} dB`; });
  updateKnobVisual(document.querySelector('[data-knob="bassFreq"]'), p.bass.f, 20, 200, 'Hz'); document.getElementById('bassFreqText').textContent=p.bass.f;
  updateKnobVisual(document.querySelector('[data-knob="bassGain"]'), p.bass.g, -12, 12, 'dB'); document.getElementById('bassGainText').textContent=p.bass.g;
  updateKnobVisual(document.querySelector('[data-knob="bassQ"]'), p.bass.q, 0.1, 3, ''); document.getElementById('bassQText').textContent=p.bass.q;
  document.getElementById('dualLowThresh').value=p.dualL.th; document.getElementById('dualLowThreshVal').textContent=`${p.dualL.th} dB`;
  document.getElementById('dualLowRatio').value=p.dualL.r; document.getElementById('dualLowRatioVal').textContent=`${p.dualL.r}:1`;
  document.getElementById('dualLowAttack').value=p.dualL.a; document.getElementById('dualLowAttackVal').textContent=`${Math.round(p.dualL.a*1000)}ms`;
  document.getElementById('dualLowRelease').value=p.dualL.rel; document.getElementById('dualLowReleaseVal').textContent=`${Math.round(p.dualL.rel*1000)}ms`;
  document.getElementById('dualLowMakeup').value=p.dualL.mk; document.getElementById('dualLowMakeupVal').textContent=`${p.dualL.mk} dB`;
  document.getElementById('dualLowCrossover').value=p.dualL.c; document.getElementById('dualLowCrossoverVal').textContent=`${formatFreq(p.dualL.c)} Hz`;
  document.getElementById('dualHighThresh').value=p.dualH.th; document.getElementById('dualHighThreshVal').textContent=`${p.dualH.th} dB`;
  document.getElementById('dualHighRatio').value=p.dualH.r; document.getElementById('dualHighRatioVal').textContent=`${p.dualH.r}:1`;
  document.getElementById('dualHighAttack').value=p.dualH.a; document.getElementById('dualHighAttackVal').textContent=`${Math.round(p.dualH.a*1000)}ms`;
  document.getElementById('dualHighRelease').value=p.dualH.rel; document.getElementById('dualHighReleaseVal').textContent=`${Math.round(p.dualH.rel*1000)}ms`;
  document.getElementById('dualHighMakeup').value=p.dualH.mk; document.getElementById('dualHighMakeupVal').textContent=`${p.dualH.mk} dB`;
  document.getElementById('limiterCeiling').value=p.lim.ce; document.getElementById('limiterCeilingVal').textContent=`${p.lim.ce} dB`;
  document.getElementById('limiterRelease').value=p.lim.rel; document.getElementById('limiterReleaseVal').textContent=`${Math.round(p.lim.rel*1000)}ms`;
  document.getElementById('limiterOutputGain').value=p.lim.g; document.getElementById('limiterOutputGainVal').textContent=`${p.lim.g} dB`;
  updateKnobVisual(document.querySelector('[data-knob="stereoWidth"]'), p.stereo.w, 0, 200, '%'); document.getElementById('stereoWidthText').textContent=p.stereo.w;
  updateKnobVisual(document.querySelector('[data-knob="stereoBalance"]'), p.stereo.b, -100, 100, ''); document.getElementById('stereoBalanceText').textContent=p.stereo.b===0?'C':(p.stereo.b>0?`R${p.stereo.b}`:`L${Math.abs(p.stereo.b)}`);
  p.mb.forEach((b,i) => { document.querySelector(`.mc-th[data-i="${i}"]`).value=b.th; document.querySelector(`.mc-r[data-i="${i}"]`).value=b.r; document.querySelector(`.mc-a[data-i="${i}"]`).value=b.a; document.querySelector(`.mc-rel[data-i="${i}"]`).value=b.rel; document.querySelector(`.mc-mk[data-i="${i}"]`).value=b.mk; document.getElementById(`mcTh${i}`).textContent=`${b.th} dB`; document.getElementById(`mcR${i}`).textContent=`${b.r}:1`; document.getElementById(`mcA${i}`).textContent=`${Math.round(b.a*1000)}ms`; document.getElementById(`mcRel${i}`).textContent=`${Math.round(b.rel*1000)}ms`; document.getElementById(`mcMk${i}`).textContent=`${b.mk} dB`; });
}

// ======================== KNOBS ========================
function initKnobs() {
  document.querySelectorAll('.knob-container').forEach(k => setupKnob(k));
}
function setupKnob(knob) {
  const min = parseFloat(knob.dataset.min), max = parseFloat(knob.dataset.max), unit = knob.dataset.unit;
  updateKnobVisual(knob, parseFloat(knob.dataset.value), min, max, unit);
  let drag=false, y0=0, v0=0;
  const move = (cy) => { const d=(y0-cy)*((max-min)/150); let nv=Math.max(min,Math.min(max,v0+d)); nv=Math.round(nv*10)/10; knob.dataset.value=nv; updateKnobVisual(knob,nv,min,max,unit); onKnobChange(knob.dataset.knob,nv,unit); };
  knob.addEventListener('mousedown', e => { drag=true; y0=e.clientY; v0=parseFloat(knob.dataset.value); e.preventDefault(); });
  window.addEventListener('mousemove', e => drag && move(e.clientY));
  window.addEventListener('mouseup', () => drag=false);
  knob.addEventListener('touchstart', e => { drag=true; y0=e.touches[0].clientY; v0=parseFloat(knob.dataset.value); e.preventDefault(); }, {passive:false});
  window.addEventListener('touchmove', e => drag && move(e.touches[0].clientY), {passive:false});
  window.addEventListener('touchend', () => drag=false);
}
function updateKnob(kid, val) { const k=document.querySelector(`[data-knob="${kid}"]`); if(k){k.dataset.value=val; updateKnobVisual(k,val,parseFloat(k.dataset.min),parseFloat(k.dataset.max),k.dataset.unit);} }
function updateKnobVisual(k, val, min, max, unit) {
  const c=2*Math.PI*26, f=(val-min)/(max-min), off=c*(1-f*0.75);
  k.querySelector('.knob-fill').setAttribute('stroke-dashoffset', off);
  const t=k.querySelector('.knob-center-text');
  if(k.dataset.knob==='stereoBalance') t.textContent=val===0?'C':(val>0?`R${Math.round(val)}`:`L${Math.round(Math.abs(val))}`);
  else if(k.dataset.knob==='stereoPhase') t.textContent=`${Math.round(val)}°`;
  else t.textContent=unit?`${Math.round(val)}`:`${val}`;
}
function onKnobChange(id,val,unit) {
  switch(id){
    case 'bassFreq': processor.setBassFreq(val); document.getElementById('bassFreqText').textContent=Math.round(val); break;
    case 'bassGain': processor.setBassGain(val); document.getElementById('bassGainText').textContent=val; break;
    case 'bassQ': processor.setBassQ(val); document.getElementById('bassQText').textContent=val; break;
    case 'stereoWidth': processor.setStereoWidth(val); document.getElementById('stereoWidthText').textContent=Math.round(val); break;
    case 'stereoBalance': processor.setStereoBalance(val); document.getElementById('stereoBalanceText').textContent=val===0?'C':(val>0?`R${Math.round(val)}`:`L${Math.round(Math.abs(val))}`); break;
  }
}

// ======================== EVENTS ========================
function setupEvents() {
  document.getElementById('powerBtn').onclick = () => processor.togglePower();
  document.getElementById('inputGain').oninput = e => { const v=parseFloat(e.target.value); processor.setInputGain(v); document.getElementById('inputGainVal').textContent=v>0?`${(20*Math.log10(v)).toFixed(1)} dB`:'-∞ dB'; };
  document.getElementById('masterVolume').oninput = e => { const v=parseFloat(e.target.value); processor.setMasterVolume(v); document.getElementById('masterVolumeVal').textContent=v>0?`${(20*Math.log10(v)).toFixed(1)} dB`:'-∞ dB'; };

  document.querySelectorAll('.peq-gain').forEach(el=>el.oninput=e=>{const i=+e.target.dataset.i; const f=+document.querySelector(`.peq-freq[data-i="${i}"]`).value,q=+document.querySelector(`.peq-q[data-i="${i}"]`).value,g=+e.target.value; processor.setParamEqBand(i,f,g,q); document.getElementById(`peqG${i}`).textContent=`${g>0?'+':''}${g} dB`; drawParamEqCurve();});
  document.querySelectorAll('.peq-freq').forEach(el=>el.oninput=e=>{const i=+e.target.dataset.i; const f=+e.target.value,q=+document.querySelector(`.peq-q[data-i="${i}"]`).value,g=+document.querySelector(`.peq-gain[data-i="${i}"]`).value; processor.setParamEqBand(i,f,g,q); document.getElementById(`peqF${i}`).textContent=formatFreq(f); drawParamEqCurve();});
  document.querySelectorAll('.peq-q').forEach(el=>el.oninput=e=>{const i=+e.target.dataset.i; const q=+e.target.value,f=+document.querySelector(`.peq-freq[data-i="${i}"]`).value,g=+document.querySelector(`.peq-gain[data-i="${i}"]`).value; processor.setParamEqBand(i,f,g,q); document.getElementById(`peqQ${i}`).textContent=q; drawParamEqCurve();});

  document.querySelectorAll('.geq-slider').forEach(el=>el.oninput=e=>{const i=+e.target.dataset.i; processor.setGeqBand(i,+e.target.value); document.getElementById(`geqD${i}`).textContent=`${+e.target.value>0?'+':''}${e.target.value} dB`; drawParamEqCurve();});

  ['th','r','a','rel','mk'].forEach((s,idx) => {
    const map = {th:'threshold',r:'ratio',a:'attack',rel:'release',mk:'makeup'};
    document.querySelectorAll(`.mc-${s}`).forEach(el=>el.oninput=e=>{
      const i=+e.target.dataset.i, v=+e.target.value;
      processor.setMbCompBand(i, +document.querySelector(`.mc-th[data-i="${i}"]`).value, +document.querySelector(`.mc-r[data-i="${i}"]`).value, +document.querySelector(`.mc-a[data-i="${i}"]`).value, +document.querySelector(`.mc-rel[data-i="${i}"]`).value, +document.querySelector(`.mc-mk[data-i="${i}"]`).value);
      const ids = {th:`mcTh${i}`,r:`mcR${i}`,a:`mcA${i}`,rel:`mcRel${i}`,mk:`mcMk${i}`};
      document.getElementById(ids[s]).textContent = s==='th'?`${v} dB`:s==='r'?`${v}:1`:s==='a'||s==='rel'?`${Math.round(v*1000)}ms`:`${v} dB`;
    });
  });

  const bindDual = (id, valId, fn, fmt) => document.getElementById(id).oninput = e => { const v=+e.target.value; fn(v); document.getElementById(valId).textContent=fmt(v); };
  bindDual('dualLowThresh','dualLowThreshVal', v=>processor.setDualLowThresh(v), v=>`${v} dB`);
  bindDual('dualLowRatio','dualLowRatioVal', v=>processor.setDualLowRatio(v), v=>`${v}:1`);
  bindDual('dualLowAttack','dualLowAttackVal', v=>processor.setDualLowAttack(v), v=>`${Math.round(v*1000)}ms`);
  bindDual('dualLowRelease','dualLowReleaseVal', v=>processor.setDualLowRelease(v), v=>`${Math.round(v*1000)}ms`);
  bindDual('dualLowMakeup','dualLowMakeupVal', v=>processor.setDualLowMakeup(v), v=>`${v} dB`);
  bindDual('dualLowCrossover','dualLowCrossoverVal', v=>processor.setDualLowCrossover(v), v=>`${formatFreq(v)} Hz`);
  bindDual('dualHighThresh','dualHighThreshVal', v=>processor.setDualHighThresh(v), v=>`${v} dB`);
  bindDual('dualHighRatio','dualHighRatioVal', v=>processor.setDualHighRatio(v), v=>`${v}:1`);
  bindDual('dualHighAttack','dualHighAttackVal', v=>processor.setDualHighAttack(v), v=>`${Math.round(v*1000)}ms`);
  bindDual('dualHighRelease','dualHighReleaseVal', v=>processor.setDualHighRelease(v), v=>`${Math.round(v*1000)}ms`);
  bindDual('dualHighMakeup','dualHighMakeupVal', v=>processor.setDualHighMakeup(v), v=>`${v} dB`);
  bindDual('limiterCeiling','limiterCeilingVal', v=>processor.setLimiterCeiling(v), v=>`${v} dB`);
  bindDual('limiterRelease','limiterReleaseVal', v=>processor.setLimiterRelease(v), v=>`${Math.round(v*1000)}ms`);
  bindDual('limiterOutputGain','limiterOutputGainVal', v=>processor.setLimiterOutputGain(v), v=>`${v} dB`);

  document.getElementById('inputDevice').onchange = e => { if(processor.active) processor.switchInput(e.target.value); };
  document.getElementById('outputDevice').onchange = async e => {
    if(processor.ctx && e.target.value) {
      try { if(typeof processor.ctx.setSinkId==='function') await processor.ctx.setSinkId(e.target.value); else alert('Tu navegador no soporta cambio de salida. Usa Chrome/Edge 110+.'); } catch(err){ console.error(err); }
    }
  };

  ['paramEqBypass','geqBypass','mbCompBypass','dualCompBypass','bassBypass','stereoBypass'].forEach(id => {
    document.getElementById(id).onchange = e => {
      const map = {paramEqBypass:'paramEqBypass', geqBypass:'geqBypass', mbCompBypass:'mbCompBypass', dualCompBypass:'dualCompBypass', bassBypass:'bassBypass', stereoBypass:'stereoBypass'};
      processor[map[id]] = e.target.checked; if(processor.active) processor.buildAudioChain();
    };
  });

  document.querySelectorAll('.preset-btn').forEach(btn => btn.onclick = () => {
    document.querySelectorAll('.preset-btn').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); processor.applyPreset(btn.dataset.preset);
  });
}

// ======================== METERS & VISUALS ========================
function animateMeters() {
  if (processor.active && processor.inputAnalyser && processor.outputAnalyser) {
    const id = new Uint8Array(processor.inputAnalyser.fftSize); processor.inputAnalyser.getByteTimeDomainData(id); drawMeter('inputMeter', calcRMS(id), 'inputDb');
    const od = new Uint8Array(processor.outputAnalyser.fftSize); processor.outputAnalyser.getByteTimeDomainData(od); drawMeter('outputMeter', calcRMS(od), 'outputDb');
    if(calcRMS(od)>0.95) { document.getElementById('clipIndicator').classList.add('active'); setTimeout(()=>document.getElementById('clipIndicator').classList.remove('active'),150); }
  } else { drawMeter('inputMeter',0,'inputDb'); drawMeter('outputMeter',0,'outputDb'); }
  if(processor.ctx) { const bl=processor.ctx.baseLatency||0, ol=processor.ctx.outputLatency||0; document.getElementById('latencyDisplay').textContent=`${((bl+ol)*1000).toFixed(1)} ms`; }
  requestAnimationFrame(animateMeters);
}
function calcRMS(d) { let s=0; for(let i=0;i<d.length;i++){const x=(d[i]-128)/128; s+=x*x;} return Math.sqrt(s/d.length); }
function drawMeter(cid, rms, did) {
  const cv=document.getElementById(cid), cx=cv.getContext('2d'), w=cv.width, h=cv.height;
  cx.clearRect(0,0,w,h); cx.fillStyle='#0a0a0f'; cx.fillRect(0,0,w,h);
  const db=rms>0?20*Math.log10(rms):-Infinity, lvl=Math.max(0,(db+60)/60), seg=60, sh=h/seg;
  for(let i=0;i<seg;i++){const y=h-(i+1)*sh; if(lvl>=i/seg) cx.fillStyle=i<seg*0.6?'#00ff88':i<seg*0.8?'#ffcc00':'#ff3344'; else cx.fillStyle='#1a1a25'; cx.fillRect(2,y,w-4,sh-1);}
  if(rms>0){cx.fillStyle='#fff'; cx.fillRect(2, h-lvl*h, w-4, 2);}
  const el=document.getElementById(did); if(el){ el.textContent=db===-Infinity?'-∞ dB':`${db.toFixed(1)} dB`; el.style.color=db>-3?'#ff3344':db>-12?'#ffcc00':'#00ff88'; }
}

function animateSpectrum() {
  const cv=document.getElementById('spectrumCanvas'); if(!cv) return;
  const cx=cv.getContext('2d'); cv.width=cv.offsetWidth*2; cv.height=cv.offsetHeight*2;
  const w=cv.width, h=cv.height; cx.clearRect(0,0,w,h);
  if(processor.active && processor.spectrumAnalyser) {
    const n=processor.spectrumAnalyser.frequencyBinCount, d=new Uint8Array(n); processor.spectrumAnalyser.getByteFrequencyData(d);
    const bars=128, bw=w/bars, step=Math.floor(n/bars);
    for(let i=0;i<bars;i++){let s=0; for(let j=0;j<step;j++) s+=d[i*step+j]; const avg=s/step, bh=(avg/255)*h*0.9, hue=200-(i/bars)*160;
      cx.fillStyle=`hsl(${hue},80%,${40+(avg/255)*30}%)`; cx.fillRect(i*bw, h-bh, bw-1, bh);
      cx.fillStyle=`hsla(${hue},80%,${60+(avg/255)*20}%,0.3)`; cx.fillRect(i*bw, h-bh, bw-1, 4);
    }
    cx.strokeStyle='rgba(255,255,255,0.05)'; cx.lineWidth=1; for(let i=0;i<5;i++){const y=(h/5)*i; cx.beginPath(); cx.moveTo(0,y); cx.lineTo(w,y); cx.stroke();}
    let tot=0, pk=0; for(let i=0;i<n;i++){tot+=d[i]; if(d[i]>pk) pk=d[i];}
    const lu=-60+(tot/n/255)*60, pDb=pk>0?20*Math.log10(pk/255):-Infinity;
    document.getElementById('loudnessVal').textContent=lu.toFixed(1); document.getElementById('loudnessVal').style.color=lu>-14?'#ff8800':'#00d4ff';
    document.getElementById('peakVal').textContent=pDb===-Infinity?'-∞':pDb.toFixed(1); document.getElementById('peakVal').style.color=pDb>-3?'#ff3344':'#00ff88';
    document.getElementById('dynamicVal').textContent=`${(lu-(pDb===-Infinity?-60:pDb)).toFixed(1)} dB`;
  } else { cx.fillStyle='#1a1a25'; cx.fillRect(0,0,w,h); cx.fillStyle='#333'; cx.font='24px monospace'; cx.textAlign='center'; cx.fillText('ESPECTRO',w/2,h/2); }
  drawParamEqCurve();
  requestAnimationFrame(animateSpectrum);
}

function drawParamEqCurve() {
  const cv=document.getElementById('paramEqCanvas'); if(!cv) return;
  const cx=cv.getContext('2d'); cv.width=cv.offsetWidth*2; cv.height=cv.offsetHeight*2;
  const w=cv.width, h=cv.height, p={t:10,b:10,l:10,r:10};
  cx.clearRect(0,0,w,h); cx.fillStyle='#0a0a0f'; cx.fillRect(0,0,w,h);
  cx.strokeStyle='rgba(255,255,255,0.05)'; cx.lineWidth=1;
  for(let db=-12;db<=12;db+=6){const y=p.t+(h-p.t-p.b)*(1-(db+12)/24); cx.beginPath(); cx.moveTo(p.l,y); cx.lineTo(w-p.r,y); cx.stroke(); cx.fillStyle='rgba(255,255,255,0.2)'; cx.font='16px monospace'; cx.textAlign='right'; cx.fillText(`${db>0?'+':''}${db}dB`,p.l+30,y+4);}
  const fMin=20,fMax=20000,logMin=Math.log10(fMin),logMax=Math.log10(fMax);
  [20,50,100,200,500,1000,2000,5000,10000,20000].forEach(f=>{const x=p.l+((Math.log10(f)-logMin)/(logMax-logMin))*(w-p.l-p.r); cx.beginPath(); cx.moveTo(x,p.t); cx.lineTo(x,h-p.b); cx.stroke(); cx.fillStyle='rgba(255,255,255,0.2)'; cx.font='14px monospace'; cx.textAlign='center'; cx.fillText(formatFreq(f),x,h-p.b+16);});
  cx.strokeStyle='rgba(0,212,255,0.2)'; cx.beginPath(); cx.moveTo(p.l, p.t+(h-p.t-p.b)*0.5); cx.lineTo(w-p.r, p.t+(h-p.t-p.b)*0.5); cx.stroke();

  cx.beginPath(); cx.strokeStyle='#00d4ff'; cx.lineWidth=3; cx.shadowColor='rgba(0,212,255,0.5)'; cx.shadowBlur=8;
  const pw=w-p.l-p.r, ph=h-p.t-p.b;
  for(let px=0;px<=pw;px++){const logF=logMin+(px/pw)*(logMax-logMin), f=Math.pow(10,logF); let g=0;
    if(!processor.paramEqBypass) processor.paramEqFilters.forEach(fl=>{const m=new Float32Array(1),ph=new Float32Array(1),ff=new Float32Array([f]); fl.getFrequencyResponse(ff,m,ph); g+=20*Math.log10(m[0]);});
    if(!processor.geqBypass) processor.geqFilters.forEach(fl=>{const m=new Float32Array(1),ph=new Float32Array(1),ff=new Float32Array([f]); fl.getFrequencyResponse(ff,m,ph); g+=20*Math.log10(m[0]);});
    if(!processor.bassBypass && processor.bassFilter) {const m=new Float32Array(1),ph=new Float32Array(1),ff=new Float32Array([f]); processor.bassFilter.getFrequencyResponse(ff,m,ph); g+=20*Math.log10(m[0]);}
    const cg=Math.max(-12,Math.min(12,g)), y=p.t+ph*(1-(cg+12)/24);
    px===0?cx.moveTo(p.l+px,y):cx.lineTo(p.l+px,y);
  }
  cx.stroke(); cx.shadowBlur=0;
  cx.lineTo(w-p.r, p.t+ph*0.5); cx.lineTo(p.l, p.t+ph*0.5); cx.closePath(); cx.fillStyle='rgba(0,212,255,0.05)'; cx.fill();
}

function formatFreq(f){ return f>=1000?(f/1000).toFixed(f>=10000?0:1)+'k':Math.round(f).toString(); }
function updateClock(){ const e=processor.active?Math.floor((Date.now()-processor.startTime)/1000):0; const h=String(Math.floor(e/3600)).padStart(2,'0'), m=String(Math.floor((e%3600)/60)).padStart(2,'0'), s=String(e%60).padStart(2,'0'); document.getElementById('timeDisplay').textContent=`${h}:${m}:${s}`; }

window.addEventListener('DOMContentLoaded', () => { updateAudioDevices(); buildParamEqUI(); buildGeqUI(); buildMbCompUI(); initKnobs(); setupEvents(); requestAnimationFrame(animateMeters); requestAnimationFrame(animateSpectrum); updateClock(); setInterval(updateClock, 1000); });
</script>
</body>
</html>
