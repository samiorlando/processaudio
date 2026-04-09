<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Optimod-PC 1600 Clone - Audio Processor</title>
<style>
  :root {
    --bg-dark: #0a0a0f;
    --bg-panel: #12121a;
    --bg-section: #1a1a25;
    --bg-knob: #222233;
    --border-color: #2a2a3a;
    --text-primary: #e0e0e8;
    --text-secondary: #888899;
    --text-dim: #555566;
    --accent-green: #00ff88;
    --accent-green-dim: #00aa55;
    --accent-amber: #ffaa00;
    --accent-red: #ff3344;
    --accent-blue: #4488ff;
    --accent-cyan: #00ccff;
    --meter-green: #00ff66;
    --meter-yellow: #ffcc00;
    --meter-red: #ff3333;
    --glow-green: 0 0 10px rgba(0,255,136,0.3);
    --glow-blue: 0 0 10px rgba(68,136,255,0.3);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg-dark);
    color: var(--text-primary);
    font-family: 'Segoe UI', 'Roboto', 'Helvetica Neue', Arial, sans-serif;
    overflow-x: hidden;
    min-height: 100vh;
  }

  /* HEADER */
  .header {
    background: linear-gradient(180deg, #1a1a28 0%, #0d0d15 100%);
    border-bottom: 2px solid var(--accent-green-dim);
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 100;
  }

  .header-left {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  .logo-text {
    font-size: 11px;
    letter-spacing: 3px;
    color: var(--text-secondary);
    text-transform: uppercase;
  }

  .logo-main {
    font-size: 24px;
    font-weight: 700;
    letter-spacing: 4px;
    background: linear-gradient(135deg, var(--accent-green), var(--accent-cyan));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    text-shadow: none;
  }

  .logo-sub {
    font-size: 10px;
    letter-spacing: 2px;
    color: var(--text-dim);
  }

  /* POWER BUTTON */
  .power-btn {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    border: 2px solid #444;
    background: radial-gradient(circle, #1a1a1a, #111);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
    position: relative;
  }

  .power-btn:hover {
    border-color: #666;
  }

  .power-btn.on {
    border-color: var(--accent-green);
    box-shadow: 0 0 15px rgba(0,255,136,0.4), inset 0 0 10px rgba(0,255,136,0.1);
  }

  .power-btn .power-icon {
    width: 20px;
    height: 20px;
    border: 2px solid #555;
    border-radius: 50%;
    position: relative;
  }

  .power-btn .power-icon::after {
    content: '';
    position: absolute;
    top: -4px;
    left: 50%;
    transform: translateX(-50%);
    width: 2px;
    height: 10px;
    background: #555;
    border-radius: 1px;
  }

  .power-btn.on .power-icon {
    border-color: var(--accent-green);
    box-shadow: 0 0 5px var(--accent-green);
  }

  .power-btn.on .power-icon::after {
    background: var(--accent-green);
    box-shadow: 0 0 5px var(--accent-green);
  }

  .power-label {
    font-size: 9px;
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-top: 4px;
    color: var(--text-dim);
    text-align: center;
  }

  .power-btn.on ~ .power-label {
    color: var(--accent-green);
  }

  .header-right {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  .sample-rate-display {
    font-size: 11px;
    color: var(--text-dim);
    font-family: 'Courier New', monospace;
    background: #0a0a0f;
    padding: 4px 8px;
    border-radius: 4px;
    border: 1px solid #222;
  }

  /* MAIN LAYOUT */
  .main-container {
    padding: 10px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  /* SECTION PANELS */
  .panel {
    background: var(--bg-panel);
    border: 1px solid var(--border-color);
    border-radius: 6px;
    overflow: hidden;
  }

  .panel-header {
    background: linear-gradient(180deg, #1e1e2a, #16161f);
    padding: 8px 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid var(--border-color);
  }

  .panel-title {
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--accent-green-dim);
  }

  .panel-body {
    padding: 15px;
  }

  /* KNOB CONTROL */
  .knob-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
    cursor: pointer;
    user-select: none;
  }

  .knob-wrapper {
    width: 56px;
    height: 56px;
    position: relative;
  }

  .knob-svg {
    width: 100%;
    height: 100%;
    transform: rotate(-90deg);
  }

  .knob-bg {
    fill: none;
    stroke: #1a1a2a;
    stroke-width: 3;
  }

  .knob-fill {
    fill: none;
    stroke: var(--accent-green);
    stroke-width: 3;
    stroke-linecap: round;
    transition: stroke-dashoffset 0.1s ease;
    filter: drop-shadow(0 0 3px rgba(0,255,136,0.4));
  }

  .knob-fill.amber { stroke: var(--accent-amber); filter: drop-shadow(0 0 3px rgba(255,170,0,0.4)); }
  .knob-fill.blue { stroke: var(--accent-blue); filter: drop-shadow(0 0 3px rgba(68,136,255,0.4)); }
  .knob-fill.cyan { stroke: var(--accent-cyan); filter: drop-shadow(0 0 3px rgba(0,204,255,0.4)); }
  .knob-fill.red { stroke: var(--accent-red); filter: drop-shadow(0 0 3px rgba(255,51,68,0.4)); }

  .knob-inner {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #333344, #1a1a22);
    border: 1px solid #333;
  }

  .knob-indicator {
    position: absolute;
    top: 4px;
    left: 50%;
    transform: translateX(-50%);
    width: 2px;
    height: 8px;
    background: var(--accent-green);
    border-radius: 1px;
    box-shadow: 0 0 4px var(--accent-green);
  }

  .knob-label {
    font-size: 9px;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .knob-value {
    font-size: 10px;
    color: var(--accent-green);
    font-family: 'Courier New', monospace;
    min-width: 50px;
    text-align: center;
  }

  /* SLIDER CONTROL */
  .slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
  }

  .slider-vertical-wrapper {
    height: 120px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
    background: transparent;
    cursor: pointer;
  }

  input[type="range"]::-webkit-slider-runnable-track {
    height: 4px;
    border-radius: 2px;
    background: #1a1a2a;
  }

  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #555566, #333344);
    border: 2px solid var(--accent-green);
    box-shadow: 0 0 5px rgba(0,255,136,0.3);
    margin-top: -6px;
  }

  input[type="range"]::-moz-range-track {
    height: 4px;
    border-radius: 2px;
    background: #1a1a2a;
    border: none;
  }

  input[type="range"]::-moz-range-thumb {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #555566, #333344);
    border: 2px solid var(--accent-green);
    box-shadow: 0 0 5px rgba(0,255,136,0.3);
  }

  .slider-horizontal {
    width: 100%;
  }

  .slider-label {
    font-size: 9px;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 1px;
    text-align: center;
  }

  .slider-value {
    font-size: 10px;
    color: var(--accent-green);
    font-family: 'Courier New', monospace;
  }

  /* VERTICAL SLIDER */
  .slider-vertical {
    writing-mode: vertical-lr;
    direction: rtl;
    height: 100px;
    width: 20px;
  }

  /* METER */
  .meter-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 3px;
  }

  .meter-bar {
    width: 24px;
    height: 200px;
    background: #0a0a12;
    border-radius: 3px;
    position: relative;
    overflow: hidden;
    border: 1px solid #222;
  }

  .meter-fill {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 0%;
    border-radius: 2px;
    transition: height 0.05s linear;
    background: linear-gradient(0deg, var(--meter-green) 0%, var(--meter-green) 60%, var(--meter-yellow) 80%, var(--meter-red) 100%);
    box-shadow: 0 0 8px rgba(0,255,102,0.3);
  }

  .meter-scale {
    position: absolute;
    right: 2px;
    top: 0;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 2px 0;
  }

  .meter-scale span {
    font-size: 7px;
    color: var(--text-dim);
    font-family: 'Courier New', monospace;
  }

  .meter-label {
    font-size: 9px;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  /* HORIZONTAL LED METER */
  .led-meter {
    display: flex;
    align-items: center;
    gap: 2px;
    height: 12px;
  }

  .led-segment {
    width: 4px;
    height: 12px;
    background: #1a1a22;
    border-radius: 1px;
    transition: background 0.05s;
  }

  .led-segment.green { background: var(--meter-green); box-shadow: 0 0 3px rgba(0,255,102,0.5); }
  .led-segment.yellow { background: var(--meter-yellow); box-shadow: 0 0 3px rgba(255,204,0,0.5); }
  .led-segment.red { background: var(--meter-red); box-shadow: 0 0 3px rgba(255,51,51,0.5); }

  /* BUTTONS */
  .btn {
    padding: 6px 14px;
    border: 1px solid var(--border-color);
    background: linear-gradient(180deg, #1e1e2a, #16161f);
    color: var(--text-secondary);
    font-size: 10px;
    letter-spacing: 1px;
    text-transform: uppercase;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .btn:hover {
    border-color: var(--accent-green-dim);
    color: var(--accent-green);
  }

  .btn.active {
    border-color: var(--accent-green);
    color: var(--accent-green);
    box-shadow: 0 0 8px rgba(0,255,136,0.2);
    background: rgba(0,255,136,0.05);
  }

  .btn.danger { border-color: #441111; color: var(--accent-red); }
  .btn.danger:hover { border-color: var(--accent-red); box-shadow: 0 0 8px rgba(255,51,68,0.2); }

  /* SELECT */
  select {
    background: #0d0d15;
    border: 1px solid var(--border-color);
    color: var(--text-primary);
    padding: 5px 10px;
    font-size: 11px;
    border-radius: 4px;
    cursor: pointer;
  }

  select:focus {
    outline: none;
    border-color: var(--accent-green-dim);
  }

  /* EQ BANDS ROW */
  .eq-bands {
    display: flex;
    justify-content: space-around;
    align-items: flex-end;
    gap: 8px;
    flex-wrap: wrap;
  }

  .eq-band {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    flex: 1;
    min-width: 60px;
  }

  .eq-band-controls {
    display: flex;
    gap: 3px;
    align-items: center;
  }

  .eq-band-controls input[type="range"] {
    width: 50px;
  }

  /* GRAPHIC EQ */
  .graphic-eq {
    display: flex;
    justify-content: space-around;
    align-items: flex-end;
    gap: 4px;
    padding: 10px 0;
  }

  .geq-band {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    flex: 1;
  }

  .geq-slider {
    height: 130px;
    width: 18px;
  }

  .geq-freq {
    font-size: 8px;
    color: var(--text-dim);
    font-family: 'Courier New', monospace;
  }

  /* MULTIBAND COMPRESSOR */
  .mb-comp-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 12px;
  }

  .mb-band {
    background: var(--bg-section);
    border: 1px solid var(--border-color);
    border-radius: 5px;
    padding: 10px;
  }

  .mb-band-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 8px;
    padding-bottom: 5px;
    border-bottom: 1px solid #222;
  }

  .mb-band-name {
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  .mb-band-name.sub { color: #aa44ff; }
  .mb-band-name.bass { color: var(--accent-cyan); }
  .mb-band-name.mid { color: var(--accent-green); }
  .mb-band-name.presence { color: var(--accent-amber); }
  .mb-band-name.air { color: #ff88aa; }

  .mb-band-params {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6px;
  }

  .mb-param {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .mb-param label {
    font-size: 8px;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .mb-param input[type="range"] {
    width: 100%;
    height: 4px;
  }

  .mb-param .param-value {
    font-size: 9px;
    color: var(--accent-green);
    font-family: 'Courier New', monospace;
    text-align: right;
  }

  /* CANVAS ANALYZER */
  .analyzer-container {
    width: 100%;
    height: 80px;
    background: #080810;
    border-radius: 4px;
    border: 1px solid #1a1a2a;
    overflow: hidden;
  }

  .analyzer-canvas {
    width: 100%;
    height: 100%;
  }

  /* ROW LAYOUTS */
  .row {
    display: flex;
    gap: 10px;
    align-items: flex-start;
  }

  .col {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  /* BASS EQ SECTION */
  .bass-section {
    display: flex;
    align-items: center;
    gap: 15px;
    flex-wrap: wrap;
  }

  /* STEREO ENHANCER */
  .stereo-section {
    display: flex;
    align-items: center;
    gap: 20px;
    flex-wrap: wrap;
  }

  /* OUTPUT SECTION */
  .output-section {
    display: flex;
    align-items: center;
    gap: 15px;
    flex-wrap: wrap;
  }

  .output-meters {
    display: flex;
    gap: 10px;
  }

  /* TOGGLE SWITCH */
  .toggle-switch {
    position: relative;
    width: 40px;
    height: 20px;
    background: #1a1a22;
    border-radius: 10px;
    cursor: pointer;
    border: 1px solid #333;
    transition: all 0.3s;
  }

  .toggle-switch.on {
    background: rgba(0,255,136,0.15);
    border-color: var(--accent-green);
  }

  .toggle-switch::after {
    content: '';
    position: absolute;
    top: 2px;
    left: 2px;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background: #444;
    transition: all 0.3s;
  }

  .toggle-switch.on::after {
    left: 22px;
    background: var(--accent-green);
    box-shadow: 0 0 5px var(--accent-green);
  }

  /* PRESET BUTTONS */
  .preset-row {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }

  .preset-btn {
    padding: 4px 10px;
    font-size: 9px;
    letter-spacing: 1px;
    text-transform: uppercase;
    background: var(--bg-section);
    border: 1px solid var(--border-color);
    color: var(--text-secondary);
    border-radius: 3px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .preset-btn:hover {
    border-color: var(--accent-green-dim);
    color: var(--accent-green);
  }

  .preset-btn.active {
    background: rgba(0,255,136,0.1);
    border-color: var(--accent-green);
    color: var(--accent-green);
  }

  /* STATUS INDICATOR */
  .status-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #333;
    display: inline-block;
    margin-right: 5px;
  }

  .status-dot.active {
    background: var(--accent-green);
    box-shadow: 0 0 5px var(--accent-green);
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  /* TOOLTIP */
  .tooltip {
    position: relative;
  }

  .tooltip::after {
    content: attr(data-tip);
    position: absolute;
    bottom: 100%;
    left: 50%;
    transform: translateX(-50%);
    padding: 4px 8px;
    background: #111;
    border: 1px solid #333;
    border-radius: 3px;
    font-size: 9px;
    color: var(--text-primary);
    white-space: nowrap;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.2s;
  }

  .tooltip:hover::after {
    opacity: 1;
  }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg-dark); }
  ::-webkit-scrollbar-thumb { background: #333; border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: #444; }

  /* RESPONSIVE */
  @media (max-width: 768px) {
    .header { flex-direction: column; gap: 10px; }
    .mb-comp-grid { grid-template-columns: 1fr; }
    .eq-bands { flex-direction: column; align-items: center; }
    .graphic-eq { flex-wrap: wrap; }
    .row { flex-direction: column; }
  }

  /* DISABLED STATE */
  .disabled {
    opacity: 0.3;
    pointer-events: none;
  }

  /* GLOW ANIMATION */
  @keyframes glow {
    0%, 100% { box-shadow: 0 0 5px rgba(0,255,136,0.2); }
    50% { box-shadow: 0 0 15px rgba(0,255,136,0.4); }
  }

  .panel.glow {
    border-color: var(--accent-green-dim);
    animation: glow 3s infinite;
  }

  /* PHASE METER */
  .phase-display {
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 10px;
    color: var(--text-secondary);
  }

  .phase-bar {
    width: 60px;
    height: 6px;
    background: #1a1a22;
    border-radius: 3px;
    overflow: hidden;
  }

  .phase-fill {
    height: 100%;
    width: 50%;
    background: var(--accent-green);
    border-radius: 3px;
    transition: width 0.1s;
  }

  /* LOUDNESS DISPLAY */
  .loudness-display {
    font-family: 'Courier New', monospace;
    font-size: 28px;
    font-weight: 700;
    color: var(--accent-green);
    text-shadow: 0 0 10px rgba(0,255,136,0.3);
    text-align: center;
    min-width: 120px;
  }

  .loudness-unit {
    font-size: 12px;
    color: var(--text-secondary);
    margin-left: 3px;
  }

  .loudness-label {
    font-size: 9px;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 1px;
    text-align: center;
  }
</style>
</head>
<body>

<!-- HEADER -->
<div class="header">
  <div class="header-left">
    <div>
      <div class="logo-text">Professional Audio Processing</div>
      <div class="logo-main">OPTIMOD-PC 1600</div>
      <div class="logo-sub">Multiband Audio Processor — Web Edition</div>
    </div>
  </div>
  <div style="display:flex;align-items:center;gap:20px;">
    <div style="text-align:center;">
      <div class="power-btn" id="powerBtn" onclick="togglePower()">
        <div class="power-icon"></div>
      </div>
      <div class="power-label">POWER</div>
    </div>
    <div class="sample-rate-display" id="sampleRateDisplay">-- kHz</div>
    <div>
      <div class="status-dot" id="statusDot"></div>
      <span style="font-size:10px;color:var(--text-dim);">PROCESSING</span>
    </div>
  </div>
</div>

<!-- MAIN -->
<div class="main-container" id="mainContainer">

  <!-- 1. INPUT SECTION -->
  <div class="panel disabled" id="inputPanel">
    <div class="panel-header">
      <span class="panel-title">🔊 Input Section</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <select id="inputSelect" onchange="changeInput()">
          <option value="">Select Input Device...</option>
        </select>
        <button class="btn" id="refreshDevices" onclick="refreshDevices()">Refresh</button>
      </div>
    </div>
    <div class="panel-body">
      <div class="row">
        <div class="col" style="flex:0 0 auto;align-items:center;">
          <div class="knob-container" id="inputGainKnob" data-param="inputGain" data-min="0" data-max="24" data-value="12" data-unit="dB">
            <div class="knob-wrapper">
              <svg class="knob-svg" viewBox="0 0 56 56">
                <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
                <circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/>
              </svg>
              <div class="knob-inner"><div class="knob-indicator"></div></div>
            </div>
            <span class="knob-label">Gain</span>
            <span class="knob-value">12.0 dB</span>
          </div>
        </div>
        <div class="col" style="flex:1;">
          <div style="display:flex;gap:10px;align-items:flex-start;">
            <div class="meter-container">
              <div class="meter-label">L</div>
              <div class="meter-bar">
                <div class="meter-fill" id="inputMeterL"></div>
                <div class="meter-scale"><span>0</span><span>-6</span><span>-12</span><span>-24</span><span>-48</span></div>
              </div>
            </div>
            <div class="meter-container">
              <div class="meter-label">R</div>
              <div class="meter-bar">
                <div class="meter-fill" id="inputMeterR"></div>
                <div class="meter-scale"><span>0</span><span>-6</span><span>-12</span><span>-24</span><span>-48</span></div>
              </div>
            </div>
            <div style="flex:1;">
              <div class="analyzer-container">
                <canvas class="analyzer-canvas" id="inputAnalyzer"></canvas>
              </div>
              <div style="margin-top:5px;display:flex;justify-content:space-between;">
                <span style="font-size:8px;color:var(--text-dim);">20Hz</span>
                <span style="font-size:8px;color:var(--text-dim);">20kHz</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 2. PARAMETRIC EQ -->
  <div class="panel disabled" id="eqPanel">
    <div class="panel-header">
      <span class="panel-title">🎚️ Parametric EQ (5 Bands)</span>
      <div class="preset-row">
        <button class="preset-btn" onclick="loadEqPreset('flat')">Flat</button>
        <button class="preset-btn" onclick="loadEqPreset('bassBoost')">Bass Boost</button>
        <button class="preset-btn" onclick="loadEqPreset('vocalClarity')">Vocal Clarity</button>
        <button class="preset-btn" onclick="loadEqPreset('brightHighs')">Bright Highs</button>
        <button class="preset-btn" onclick="loadEqPreset('radio')">Radio</button>
      </div>
    </div>
    <div class="panel-body">
      <div class="eq-bands" id="paramEqBands"></div>
    </div>
  </div>

  <!-- 3. MULTIBAND COMPRESSOR -->
  <div class="panel disabled" id="mbCompPanel">
    <div class="panel-header">
      <span class="panel-title">🎛️ Multiband Compressor</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <span style="font-size:9px;color:var(--text-dim);">MASTER</span>
        <div class="toggle-switch on" id="mbCompToggle" onclick="toggleMbComp()"></div>
      </div>
    </div>
    <div class="panel-body">
      <div class="mb-comp-grid" id="mbCompBands"></div>
    </div>
  </div>

  <!-- 4. GRAPHIC EQ -->
  <div class="panel disabled" id="geqPanel">
    <div class="panel-header">
      <span class="panel-title">📊 Graphic Equalizer (15 Bands)</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <button class="btn" onclick="resetGeq()">Reset</button>
        <div class="toggle-switch on" id="geqToggle" onclick="toggleGeq()"></div>
      </div>
    </div>
    <div class="panel-body">
      <div class="graphic-eq" id="geqBands"></div>
    </div>
  </div>

  <!-- 5. DUAL BAND COMPRESSOR -->
  <div class="panel disabled" id="dualCompPanel">
    <div class="panel-header">
      <span class="panel-title">⚡ Dual Band Compressor + Limiter</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <span style="font-size:9px;color:var(--text-dim);">ACTIVE</span>
        <div class="toggle-switch on" id="dualCompToggle" onclick="toggleDualComp()"></div>
      </div>
    </div>
    <div class="panel-body">
      <div class="row">
        <div class="mb-band" style="flex:1;">
          <div class="mb-band-header">
            <span class="mb-band-name bass">LOW BAND</span>
            <span style="font-size:9px;color:var(--text-dim);">80-2000 Hz</span>
          </div>
          <div class="mb-band-params">
            <div class="mb-param">
              <label>Threshold</label>
              <input type="range" min="-60" max="0" value="-20" id="dualLowThresh" oninput="updateDualComp()">
              <span class="param-value" id="dualLowThreshVal">-20 dB</span>
            </div>
            <div class="mb-param">
              <label>Ratio</label>
              <input type="range" min="1" max="20" value="4" id="dualLowRatio" oninput="updateDualComp()">
              <span class="param-value" id="dualLowRatioVal">4:1</span>
            </div>
            <div class="mb-param">
              <label>Attack</label>
              <input type="range" min="0.1" max="100" value="10" step="0.1" id="dualLowAttack" oninput="updateDualComp()">
              <span class="param-value" id="dualLowAttackVal">10 ms</span>
            </div>
            <div class="mb-param">
              <label>Release</label>
              <input type="range" min="10" max="1000" value="150" id="dualLowRelease" oninput="updateDualComp()">
              <span class="param-value" id="dualLowReleaseVal">150 ms</span>
            </div>
            <div class="mb-param">
              <label>Makeup</label>
              <input type="range" min="0" max="18" value="3" id="dualLowMakeup" oninput="updateDualComp()">
              <span class="param-value" id="dualLowMakeupVal">3 dB</span>
            </div>
          </div>
        </div>
        <div class="mb-band" style="flex:1;">
          <div class="mb-band-header">
            <span class="mb-band-name air">HIGH BAND</span>
            <span style="font-size:9px;color:var(--text-dim);">2000-20000 Hz</span>
          </div>
          <div class="mb-band-params">
            <div class="mb-param">
              <label>Threshold</label>
              <input type="range" min="-60" max="0" value="-18" id="dualHighThresh" oninput="updateDualComp()">
              <span class="param-value" id="dualHighThreshVal">-18 dB</span>
            </div>
            <div class="mb-param">
              <label>Ratio</label>
              <input type="range" min="1" max="20" value="3" id="dualHighRatio" oninput="updateDualComp()">
              <span class="param-value" id="dualHighRatioVal">3:1</span>
            </div>
            <div class="mb-param">
              <label>Attack</label>
              <input type="range" min="0.1" max="100" value="5" step="0.1" id="dualHighAttack" oninput="updateDualComp()">
              <span class="param-value" id="dualHighAttackVal">5 ms</span>
            </div>
            <div class="mb-param">
              <label>Release</label>
              <input type="range" min="10" max="1000" value="120" id="dualHighRelease" oninput="updateDualComp()">
              <span class="param-value" id="dualHighReleaseVal">120 ms</span>
            </div>
            <div class="mb-param">
              <label>Makeup</label>
              <input type="range" min="0" max="18" value="2" id="dualHighMakeup" oninput="updateDualComp()">
              <span class="param-value" id="dualHighMakeupVal">2 dB</span>
            </div>
          </div>
        </div>
        <div class="mb-band" style="flex:1;">
          <div class="mb-band-header">
            <span class="mb-band-name" style="color:var(--accent-red);">BRICKWALL LIMITER</span>
          </div>
          <div class="mb-band-params">
            <div class="mb-param">
              <label>Threshold</label>
              <input type="range" min="-12" max="0" value="-0.5" step="0.1" id="limiterThresh" oninput="updateLimiter()">
              <span class="param-value" id="limiterThreshVal">-0.5 dB</span>
            </div>
            <div class="mb-param">
              <label>Ratio</label>
              <input type="range" min="10" max="100" value="20" id="limiterRatio" oninput="updateLimiter()">
              <span class="param-value" id="limiterRatioVal">20:1</span>
            </div>
            <div class="mb-param">
              <label>Attack</label>
              <input type="range" min="0.1" max="10" value="0.5" step="0.1" id="limiterAttack" oninput="updateLimiter()">
              <span class="param-value" id="limiterAttackVal">0.5 ms</span>
            </div>
            <div class="mb-param">
              <label>Release</label>
              <input type="range" min="10" max="500" value="100" id="limiterRelease" oninput="updateLimiter()">
              <span class="param-value" id="limiterReleaseVal">100 ms</span>
            </div>
            <div class="mb-param">
              <label>Ceiling</label>
              <input type="range" min="-3" max="0" value="-0.1" step="0.1" id="limiterCeiling" oninput="updateLimiter()">
              <span class="param-value" id="limiterCeilingVal">-0.1 dB</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 6. BASS EQ -->
  <div class="panel disabled" id="bassPanel">
    <div class="panel-header">
      <span class="panel-title">🔥 Bass EQ Enhancement</span>
      <div class="toggle-switch" id="bassToggle" onclick="toggleBass()"></div>
    </div>
    <div class="panel-body">
      <div class="bass-section">
        <div class="knob-container" id="bassFreqKnob" data-param="bassFreq" data-min="30" data-max="120" data-value="60" data-unit="Hz">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="80"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Frequency</span>
          <span class="knob-value">60 Hz</span>
        </div>
        <div class="knob-container" id="bassGainKnob" data-param="bassGain" data-min="0" data-max="12" data-value="4" data-unit="dB">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="90"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Boost</span>
          <span class="knob-value">4.0 dB</span>
        </div>
        <div class="knob-container" id="bassQKnob" data-param="bassQ" data-min="0.5" data-max="3" data-value="1.4" data-unit="">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="60"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Q</span>
          <span class="knob-value">1.4</span>
        </div>
        <div class="knob-container" id="subBassKnob" data-param="subBass" data-min="0" data-max="10" data-value="0" data-unit="dB">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill amber" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="113"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Sub Bass</span>
          <span class="knob-value">0.0 dB</span>
        </div>
        <div style="margin-left:10px;">
          <div style="font-size:9px;color:var(--text-dim);margin-bottom:5px;">ANTI-DISTORTION</div>
          <div class="toggle-switch" id="antiDistToggle" onclick="toggleAntiDist()"></div>
          <div style="font-size:8px;color:var(--text-dim);margin-top:3px;">Protection</div>
        </div>
      </div>
    </div>
  </div>

  <!-- 7. STEREO ENHANCER -->
  <div class="panel disabled" id="stereoPanel">
    <div class="panel-header">
      <span class="panel-title">🌐 Stereo Enhancer</span>
      <div class="toggle-switch on" id="stereoToggle" onclick="toggleStereo()"></div>
    </div>
    <div class="panel-body">
      <div class="stereo-section">
        <div class="knob-container" id="stereoWidthKnob" data-param="stereoWidth" data-min="0" data-max="200" data-value="100" data-unit="%">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill cyan" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Width</span>
          <span class="knob-value">100%</span>
        </div>
        <div class="knob-container" id="midLevelKnob" data-param="midLevel" data-min="-12" data-max="6" data-value="0" data-unit="dB">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill cyan" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Mid</span>
          <span class="knob-value">0.0 dB</span>
        </div>
        <div class="knob-container" id="sideLevelKnob" data-param="sideLevel" data-min="-24" data-max="6" data-value="0" data-unit="dB">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill cyan" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Side</span>
          <span class="knob-value">0.0 dB</span>
        </div>
        <div class="phase-display">
          <span>Phase:</span>
          <div class="phase-bar"><div class="phase-fill" id="phaseFill"></div></div>
          <span id="phaseValue">0°</span>
        </div>
        <div>
          <div style="font-size:9px;color:var(--text-dim);margin-bottom:5px;">MONO COMPATIBILITY</div>
          <div class="toggle-switch on" id="monoCompatToggle" onclick="toggleMonoCompat()"></div>
          <div style="font-size:8px;color:var(--text-dim);margin-top:3px;">Safe Mode</div>
        </div>
      </div>
    </div>
  </div>

  <!-- 8. MASTER MULTIBAND PROCESSOR -->
  <div class="panel disabled" id="masterPanel">
    <div class="panel-header">
      <span class="panel-title">🎚️ Master Multiband Processor (5 Bands)</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <div class="toggle-switch on" id="masterToggle" onclick="toggleMaster()"></div>
      </div>
    </div>
    <div class="panel-body">
      <div class="mb-comp-grid" id="masterBands"></div>
    </div>
  </div>

  <!-- 9. OUTPUT SECTION -->
  <div class="panel disabled" id="outputPanel">
    <div class="panel-header">
      <span class="panel-title">📤 Output Section</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <select id="outputSelect">
          <option value="">Default Output</option>
        </select>
      </div>
    </div>
    <div class="panel-body">
      <div class="output-section">
        <div class="knob-container" id="outputVolKnob" data-param="outputVol" data-min="-30" data-max="6" data-value="0" data-unit="dB">
          <div class="knob-wrapper">
            <svg class="knob-svg" viewBox="0 0 56 56">
              <circle class="knob-bg" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
              <circle class="knob-fill red" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/>
            </svg>
            <div class="knob-inner"><div class="knob-indicator"></div></div>
          </div>
          <span class="knob-label">Output</span>
          <span class="knob-value">0.0 dB</span>
        </div>
        <div class="output-meters">
          <div class="meter-container">
            <div class="meter-label">L</div>
            <div class="meter-bar">
              <div class="meter-fill" id="outputMeterL"></div>
              <div class="meter-scale"><span>0</span><span>-6</span><span>-12</span><span>-24</span><span>-48</span></div>
            </div>
          </div>
          <div class="meter-container">
            <div class="meter-label">R</div>
            <div class="meter-bar">
              <div class="meter-fill" id="outputMeterR"></div>
              <div class="meter-scale"><span>0</span><span>-6</span><span>-12</span><span>-24</span><span>-48</span></div>
            </div>
          </div>
        </div>
        <div style="text-align:center;">
          <div class="loudness-display" id="loudnessDisplay">--<span class="loudness-unit">LUFS</span></div>
          <div class="loudness-label">Integrated Loudness</div>
        </div>
        <div style="text-align:center;">
          <div class="loudness-display" id="truePeakDisplay" style="color:var(--accent-amber);font-size:22px;">--<span class="loudness-unit">dBTP</span></div>
          <div class="loudness-label">True Peak</div>
        </div>
        <div class="analyzer-container" style="flex:1;min-width:200px;">
          <canvas class="analyzer-canvas" id="outputAnalyzer"></canvas>
        </div>
      </div>
    </div>
  </div>

</div>

<script>
// ==================== GLOBAL STATE ====================
let audioCtx = null;
let isPowerOn = false;
let inputStream = null;
let sourceNode = null;

// Audio nodes chain
let inputGainNode = null;
let paramEqNodes = [];
let mbCompNodes = [];
let geqNodes = [];
let dualCompNodes = [];
let bassNode = null;
let subBassNode = null;
let stereoMerger = null;
let stereoSplitter = null;
let masterCompNodes = [];
let outputGainNode = null;
let limiterNode = null;
let analyserInput = null;
let analyserOutput = null;
let masterAnalyser = null;

// Meter data
let meterDataL = new Uint8Array(128);
let meterDataR = new Uint8Array(128);

// Animation frame
let animFrameId = null;

// ==================== POWER ON/OFF ====================
async function togglePower() {
  const btn = document.getElementById('powerBtn');
  if (!isPowerOn) {
    await powerOn();
    btn.classList.add('on');
    isPowerOn = true;
    enablePanels();
  } else {
    powerOff();
    btn.classList.remove('on');
    isPowerOn = false;
    disablePanels();
  }
}

async function powerOn() {
  audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  document.getElementById('sampleRateDisplay').textContent = (audioCtx.sampleRate / 1000).toFixed(1) + ' kHz';
  
  // Create nodes
  inputGainNode = audioCtx.createGain();
  inputGainNode.gain.value = dbToGain(12);
  
  // Analyzers
  analyserInput = audioCtx.createAnalyser();
  analyserInput.fftSize = 2048;
  analyserInput.smoothingTimeConstant = 0.8;
  
  analyserOutput = audioCtx.createAnalyser();
  analyserOutput.fftSize = 2048;
  analyserOutput.smoothingTimeConstant = 0.8;
  
  masterAnalyser = audioCtx.createAnalyser();
  masterAnalyser.fftSize = 4096;
  masterAnalyser.smoothingTimeConstant = 0.85;
  
  // Create parametric EQ bands
  createParamEqNodes();
  
  // Create multiband compressor nodes
  createMultibandCompNodes();
  
  // Create graphic EQ nodes
  createGeqNodes();
  
  // Create dual band compressor
  createDualBandNodes();
  
  // Bass enhancement
  bassNode = audioCtx.createBiquadFilter();
  bassNode.type = 'lowshelf';
  bassNode.frequency.value = 60;
  bassNode.gain.value = 0;
  
  subBassNode = audioCtx.createBiquadFilter();
  subBassNode.type = 'peaking';
  subBassNode.frequency.value = 40;
  subBassNode.gain.value = 0;
  subBassNode.Q.value = 1;
  
  // Stereo processing
  stereoMerger = audioCtx.createChannelMerger(2);
  stereoSplitter = audioCtx.createChannelSplitter(2);
  
  // Master multiband processor
  createMasterBandNodes();
  
  // Limiter
  limiterNode = audioCtx.createDynamicsCompressor();
  limiterNode.threshold.value = -0.5;
  limiterNode.ratio.value = 20;
  limiterNode.attack.value = 0.0005;
  limiterNode.release.value = 0.1;
  limiterNode.knee.value = 0;
  
  // Output gain
  outputGainNode = audioCtx.createGain();
  outputGainNode.gain.value = dbToGain(0);
  
  // Build audio chain
  buildAudioChain();
  
  // Start meter animation
  startMeters();
  
  document.getElementById('statusDot').classList.add('active');
}

function buildAudioChain() {
  // Disconnect everything first
  try {
    if (sourceNode) sourceNode.disconnect();
  } catch(e) {}
  
  // Chain: source -> inputGain -> analyserInput -> paramEQ -> mbComp -> geq -> dualComp -> bass -> stereo -> master -> limiter -> outputGain -> analyserOutput -> destination
  let chain = sourceNode || audioCtx.destination;
  
  if (sourceNode) {
    chain.connect(inputGainNode);
    inputGainNode.connect(analyserInput);
    
    // Parametric EQ
    let eqChain = analyserInput;
    for (let node of paramEqNodes) {
      eqChain.connect(node);
      eqChain = node;
    }
    
    // Multiband compressor
    let mbOut = routeThroughMbComp(eqChain);
    
    // Graphic EQ
    let geqChain = mbOut;
    for (let node of geqNodes) {
      geqChain.connect(node);
      geqChain = node;
    }
    
    // Dual band compressor
    let dualOut = routeThroughDualComp(geqChain);
    
    // Bass EQ
    dualOut.connect(bassNode);
    bassNode.connect(subBassNode);
    
    // Stereo enhancer
    subBassNode.connect(stereoSplitter);
    routeStereoEnhancer();
    
    // Master multiband processor
    let masterOut = routeThroughMasterBands(stereoMerger);
    
    // Limiter
    masterOut.connect(limiterNode);
    limiterNode.connect(outputGainNode);
    outputGainNode.connect(analyserOutput);
    analyserOutput.connect(audioCtx.destination);
    
    // Also connect master analyser
    masterOut.connect(masterAnalyser);
  }
}

function powerOff() {
  if (inputStream) {
    inputStream.getTracks().forEach(t => t.stop());
    inputStream = null;
  }
  if (sourceNode) {
    try { sourceNode.disconnect(); } catch(e) {}
    sourceNode = null;
  }
  if (audioCtx) {
    audioCtx.close();
    audioCtx = null;
  }
  if (animFrameId) {
    cancelAnimationFrame(animFrameId);
    animFrameId = null;
  }
  document.getElementById('statusDot').classList.remove('active');
}

function enablePanels() {
  ['inputPanel','eqPanel','mbCompPanel','geqPanel','dualCompPanel','bassPanel','stereoPanel','masterPanel','outputPanel'].forEach(id => {
    document.getElementById(id).classList.remove('disabled');
  });
}

function disablePanels() {
  ['inputPanel','eqPanel','mbCompPanel','geqPanel','dualCompPanel','bassPanel','stereoPanel','masterPanel','outputPanel'].forEach(id => {
    document.getElementById(id).classList.add('disabled');
  });
}

// ==================== INPUT DEVICE ====================
async function refreshDevices() {
  try {
    const devices = await navigator.mediaDevices.enumerateDevices();
    const select = document.getElementById('inputSelect');
    select.innerHTML = '<option value="">Select Input Device...</option>';
    devices.filter(d => d.kind === 'audioinput').forEach(d => {
      const opt = document.createElement('option');
      opt.value = d.deviceId;
      opt.textContent = d.label || `Input ${select.options.length}`;
      select.appendChild(opt);
    });
  } catch(e) {
    console.error('Error enumerating devices:', e);
  }
}

async function changeInput() {
  if (!audioCtx) return;
  const deviceId = document.getElementById('inputSelect').value;
  if (inputStream) {
    inputStream.getTracks().forEach(t => t.stop());
    if (sourceNode) { try { sourceNode.disconnect(); } catch(e) {} }
  }
  
  try {
    const constraints = {
      audio: deviceId ? { deviceId: { exact: deviceId } } : true,
      video: false
    };
    inputStream = await navigator.mediaDevices.getUserMedia(constraints);
    sourceNode = audioCtx.createMediaStreamSource(inputStream);
    buildAudioChain();
  } catch(e) {
    console.error('Error accessing microphone:', e);
    alert('No se pudo acceder al dispositivo de audio. Verifica los permisos.');
  }
}

// ==================== PARAMETRIC EQ ====================
function createParamEqNodes() {
  paramEqNodes = [];
  const bands = [
    { freq: 80, gain: 0, q: 0.7, type: 'lowshelf', label: 'Low' },
    { freq: 250, gain: 0, q: 1, type: 'peaking', label: 'Low-Mid' },
    { freq: 1000, gain: 0, q: 1, type: 'peaking', label: 'Mid' },
    { freq: 4000, gain: 0, q: 1, type: 'peaking', label: 'Presence' },
    { freq: 12000, gain: 0, q: 0.7, type: 'highshelf', label: 'Air' }
  ];
  
  bands.forEach((b, i) => {
    const node = audioCtx.createBiquadFilter();
    node.type = b.type;
    node.frequency.value = b.freq;
    node.gain.value = b.gain;
    node.Q.value = b.q;
    paramEqNodes.push(node);
  });
  
  renderParamEqUI(bands);
}

function renderParamEqUI(bands) {
  const container = document.getElementById('paramEqBands');
  container.innerHTML = '';
  
  bands.forEach((b, i) => {
    const div = document.createElement('div');
    div.className = 'eq-band';
    div.innerHTML = `
      <div style="font-size:10px;color:var(--accent-green);margin-bottom:4px;">${b.label}</div>
      <div class="slider-container">
        <span style="font-size:9px;color:var(--text-dim);">Gain</span>
        <input type="range" class="slider-horizontal" min="-12" max="12" value="${b.gain}" step="0.5" 
          oninput="updateParamEq(${i}, 'gain', this.value)" style="width:80px;">
        <span class="slider-value" id="paramEqGain${i}">${b.gain}dB</span>
      </div>
      <div class="slider-container" style="margin-top:5px;">
        <span style="font-size:9px;color:var(--text-dim);">Freq</span>
        <input type="range" class="slider-horizontal" min="${b.type==='lowshelf'?20:b.type==='highshelf'?2000:50}" max="${b.type==='lowshelf'?500:b.type==='highshelf'?16000:16000}" value="${b.freq}" step="1"
          oninput="updateParamEq(${i}, 'freq', this.value)" style="width:80px;">
        <span class="slider-value" id="paramEqFreq${i}">${b.freq}Hz</span>
      </div>
      <div class="slider-container" style="margin-top:5px;">
        <span style="font-size:9px;color:var(--text-dim);">Q</span>
        <input type="range" class="slider-horizontal" min="0.1" max="10" value="${b.q}" step="0.1"
          oninput="updateParamEq(${i}, 'q', this.value)" style="width:80px;">
        <span class="slider-value" id="paramEqQ${i}">${b.q}</span>
      </div>
    `;
    container.appendChild(div);
  });
}

function updateParamEq(band, param, value) {
  value = parseFloat(value);
  if (param === 'gain') {
    paramEqNodes[band].gain.value = value;
    document.getElementById(`paramEqGain${band}`).textContent = value.toFixed(1) + 'dB';
  } else if (param === 'freq') {
    paramEqNodes[band].frequency.value = value;
    document.getElementById(`paramEqFreq${band}`).textContent = value.toFixed(0) + 'Hz';
  } else if (param === 'q') {
    paramEqNodes[band].Q.value = value;
    document.getElementById(`paramEqQ${band}`).textContent = value.toFixed(1);
  }
}

function loadEqPreset(preset) {
  const presets = {
    flat: [
      { freq: 80, gain: 0, q: 0.7, type: 'lowshelf' },
      { freq: 250, gain: 0, q: 1, type: 'peaking' },
      { freq: 1000, gain: 0, q: 1, type: 'peaking' },
      { freq: 4000, gain: 0, q: 1, type: 'peaking' },
      { freq: 12000, gain: 0, q: 0.7, type: 'highshelf' }
    ],
    bassBoost: [
      { freq: 80, gain: 6, q: 0.7, type: 'lowshelf' },
      { freq: 250, gain: 2, q: 1, type: 'peaking' },
      { freq: 1000, gain: 0, q: 1, type: 'peaking' },
      { freq: 4000, gain: 1, q: 1, type: 'peaking' },
      { freq: 12000, gain: 2, q: 0.7, type: 'highshelf' }
    ],
    vocalClarity: [
      { freq: 80, gain: -3, q: 0.7, type: 'lowshelf' },
      { freq: 250, gain: -2, q: 1, type: 'peaking' },
      { freq: 1000, gain: 4, q: 1.5, type: 'peaking' },
      { freq: 4000, gain: 5, q: 1, type: 'peaking' },
      { freq: 12000, gain: 3, q: 0.7, type: 'highshelf' }
    ],
    brightHighs: [
      { freq: 80, gain: 0, q: 0.7, type: 'lowshelf' },
      { freq: 250, gain: 0, q: 1, type: 'peaking' },
      { freq: 1000, gain: 1, q: 1, type: 'peaking' },
      { freq: 4000, gain: 3, q: 1, type: 'peaking' },
      { freq: 12000, gain: 6, q: 0.7, type: 'highshelf' }
    ],
    radio: [
      { freq: 80, gain: 4, q: 0.7, type: 'lowshelf' },
      { freq: 250, gain: 1, q: 1, type: 'peaking' },
      { freq: 1000, gain: 3, q: 1.5, type: 'peaking' },
      { freq: 4000, gain: 4, q: 1, type: 'peaking' },
      { freq: 12000, gain: 3, q: 0.7, type: 'highshelf' }
    ]
  };
  
  const p = presets[preset];
  if (!p) return;
  
  p.forEach((b, i) => {
    paramEqNodes[i].frequency.value = b.freq;
    paramEqNodes[i].gain.value = b.gain;
    paramEqNodes[i].Q.value = b.q;
    paramEqNodes[i].type = b.type;
  });
  
  // Update UI
  document.querySelectorAll('.preset-btn').forEach(btn => btn.classList.remove('active'));
  event.target.classList.add('active');
}

// ==================== MULTIBAND COMPRESSOR ====================
let mbCompEnabled = true;

function createMultibandCompNodes() {
  const bands = [
    { name: 'Sub', class: 'sub', range: '20-80Hz', lowFreq: 20, highFreq: 80, threshold: -24, ratio: 4, attack: 15, release: 200, makeup: 4 },
    { name: 'Bass', class: 'bass', range: '80-250Hz', lowFreq: 80, highFreq: 250, threshold: -22, ratio: 3.5, attack: 12, release: 180, makeup: 3 },
    { name: 'Mid', class: 'mid', range: '250-2kHz', lowFreq: 250, highFreq: 2000, threshold: -20, ratio: 3, attack: 8, release: 150, makeup: 2 },
    { name: 'Presence', class: 'presence', range: '2-6kHz', lowFreq: 2000, highFreq: 6000, threshold: -22, ratio: 3.5, attack: 5, release: 120, makeup: 2 },
    { name: 'Air', class: 'air', range: '6-16kHz', lowFreq: 6000, highFreq: 16000, threshold: -24, ratio: 2.5, attack: 3, release: 100, makeup: 3 }
  ];
  
  mbCompNodes = bands.map(b => {
    const comp = audioCtx.createDynamicsCompressor();
    comp.threshold.value = b.threshold;
    comp.ratio.value = b.ratio;
    comp.attack.value = b.attack / 1000;
    comp.release.value = b.release / 1000;
    comp.knee.value = 6;
    return { ...b, compressor: comp };
  });
  
  renderMbCompUI(bands);
}

function renderMbCompUI(bands) {
  const container = document.getElementById('mbCompBands');
  container.innerHTML = '';
  
  bands.forEach((b, i) => {
    const div = document.createElement('div');
    div.className = 'mb-band';
    div.innerHTML = `
      <div class="mb-band-header">
        <span class="mb-band-name ${b.class}">${b.name}</span>
        <span style="font-size:9px;color:var(--text-dim);">${b.range}</span>
      </div>
      <div class="mb-band-params">
        <div class="mb-param">
          <label>Threshold</label>
          <input type="range" min="-60" max="0" value="${b.threshold}" id="mbThresh${i}" oninput="updateMbComp(${i},'threshold',this.value)">
          <span class="param-value" id="mbThresh${i}Val">${b.threshold} dB</span>
        </div>
        <div class="mb-param">
          <label>Ratio</label>
          <input type="range" min="1" max="20" value="${b.ratio}" step="0.5" id="mbRatio${i}" oninput="updateMbComp(${i},'ratio',this.value)">
          <span class="param-value" id="mbRatio${i}Val">${b.ratio}:1</span>
        </div>
        <div class="mb-param">
          <label>Attack</label>
          <input type="range" min="0.1" max="100" value="${b.attack}" step="0.1" id="mbAttack${i}" oninput="updateMbComp(${i},'attack',this.value)">
          <span class="param-value" id="mbAttack${i}Val">${b.attack} ms</span>
        </div>
        <div class="mb-param">
          <label>Release</label>
          <input type="range" min="10" max="1000" value="${b.release}" id="mbRelease${i}" oninput="updateMbComp(${i},'release',this.value)">
          <span class="param-value" id="mbRelease${i}Val">${b.release} ms</span>
        </div>
        <div class="mb-param">
          <label>Makeup</label>
          <input type="range" min="0" max="18" value="${b.makeup}" id="mbMakeup${i}" oninput="updateMbComp(${i},'makeup',this.value)">
          <span class="param-value" id="mbMakeup${i}Val">${b.makeup} dB</span>
        </div>
      </div>
    `;
    container.appendChild(div);
  });
}

function updateMbComp(band, param, value) {
  value = parseFloat(value);
  const node = mbCompNodes[band].compressor;
  if (param === 'threshold') { node.threshold.value = value; document.getElementById(`mbThresh${band}Val`).textContent = value + ' dB'; }
  else if (param === 'ratio') { node.ratio.value = value; document.getElementById(`mbRatio${band}Val`).textContent = value + ':1'; }
  else if (param === 'attack') { node.attack.value = value / 1000; document.getElementById(`mbAttack${band}Val`).textContent = value + ' ms'; }
  else if (param === 'release') { node.release.value = value / 1000; document.getElementById(`mbRelease${band}Val`).textContent = value + ' ms'; }
  else if (param === 'makeup') { document.getElementById(`mbMakeup${band}Val`).textContent = value + ' dB'; }
}

function routeThroughMbComp(inputNode) {
  if (!mbCompEnabled || mbCompNodes.length === 0) {
    return inputNode;
  }
  
  // Simple serial chain through each band's compressor
  let current = inputNode;
  mbCompNodes.forEach(band => {
    // Create bandpass filter for each band
    const bpLow = audioCtx.createBiquadFilter();
    bpLow.type = 'lowshelf';
    bpLow.frequency.value = band.highFreq;
    bpLow.gain.value = 6;
    
    const bpHigh = audioCtx.createBiquadFilter();
    bpHigh.type = 'highshelf';
    bpHigh.frequency.value = band.lowFreq;
    bpHigh.gain.value = -6;
    
    current.connect(bpLow);
    bpLow.connect(bpHigh);
    bpHigh.connect(band.compressor);
    
    // Makeup gain
    const makeup = audioCtx.createGain();
    makeup.gain.value = dbToGain(parseFloat(document.getElementById(`mbMakeup${mbCompNodes.indexOf(band)}`).value) || 0);
    band.compressor.connect(makeup);
    current = makeup;
  });
  
  return current;
}

function toggleMbComp() {
  mbCompEnabled = !mbCompEnabled;
  document.getElementById('mbCompToggle').classList.toggle('on', mbCompEnabled);
  if (sourceNode) buildAudioChain();
}

// ==================== GRAPHIC EQ ====================
let geqEnabled = true;
const geqFreqs = [31, 63, 125, 250, 500, 1000, 2000, 4000, 6300, 8000, 10000, 12500, 14000, 16000, 18000];

function createGeqNodes() {
  geqNodes = geqFreqs.map(freq => {
    const node = audioCtx.createBiquadFilter();
    node.type = 'peaking';
    node.frequency.value = freq;
    node.gain.value = 0;
    node.Q.value = 1.4;
    return node;
  });
  
  renderGeqUI();
}

function renderGeqUI() {
  const container = document.getElementById('geqBands');
  container.innerHTML = '';
  
  geqFreqs.forEach((freq, i) => {
    const div = document.createElement('div');
    div.className = 'geq-band';
    div.innerHTML = `
      <input type="range" class="slider-vertical geq-slider" min="-12" max="12" value="0" step="0.5"
        oninput="updateGeq(${i}, this.value)">
      <span style="font-size:9px;color:var(--accent-green);font-family:'Courier New',monospace;" id="geqVal${i}">0</span>
      <span class="geq-freq">${freq >= 1000 ? (freq/1000)+'k' : freq}</span>
    `;
    container.appendChild(div);
  });
}

function updateGeq(band, value) {
  geqNodes[band].gain.value = parseFloat(value);
  document.getElementById(`geqVal${band}`).textContent = value;
}

function resetGeq() {
  geqNodes.forEach((node, i) => {
    node.gain.value = 0;
    const slider = document.querySelectorAll('.geq-slider')[i];
    if (slider) slider.value = 0;
    document.getElementById(`geqVal${i}`).textContent = '0';
  });
}

function toggleGeq() {
  geqEnabled = !geqEnabled;
  document.getElementById('geqToggle').classList.toggle('on', geqEnabled);
  if (sourceNode) buildAudioChain();
}

// ==================== DUAL BAND COMPRESSOR ====================
let dualCompEnabled = true;
let dualLowComp, dualHighComp, dualLowFilter, dualHighFilter;

function createDualBandNodes() {
  dualLowComp = audioCtx.createDynamicsCompressor();
  dualHighComp = audioCtx.createDynamicsCompressor();
  
  dualLowFilter = audioCtx.createBiquadFilter();
  dualLowFilter.type = 'lowpass';
  dualLowFilter.frequency.value = 2000;
  dualLowFilter.Q.value = 0.7;
  
  dualHighFilter = audioCtx.createBiquadFilter();
  dualHighFilter.type = 'highpass';
  dualHighFilter.frequency.value = 2000;
  dualHighFilter.Q.value = 0.7;
  
  updateDualComp();
}

function updateDualComp() {
  if (!dualLowComp) return;
  
  dualLowComp.threshold.value = parseFloat(document.getElementById('dualLowThresh').value);
  dualLowComp.ratio.value = parseFloat(document.getElementById('dualLowRatio').value);
  dualLowComp.attack.value = parseFloat(document.getElementById('dualLowAttack').value) / 1000;
  dualLowComp.release.value = parseFloat(document.getElementById('dualLowRelease').value) / 1000;
  
  dualHighComp.threshold.value = parseFloat(document.getElementById('dualHighThresh').value);
  dualHighComp.ratio.value = parseFloat(document.getElementById('dualHighRatio').value);
  dualHighComp.attack.value = parseFloat(document.getElementById('dualHighAttack').value) / 1000;
  dualHighComp.release.value = parseFloat(document.getElementById('dualHighRelease').value) / 1000;
  
  document.getElementById('dualLowThreshVal').textContent = document.getElementById('dualLowThresh').value + ' dB';
  document.getElementById('dualLowRatioVal').textContent = document.getElementById('dualLowRatio').value + ':1';
  document.getElementById('dualLowAttackVal').textContent = document.getElementById('dualLowAttack').value + ' ms';
  document.getElementById('dualLowReleaseVal').textContent = document.getElementById('dualLowRelease').value + ' ms';
  document.getElementById('dualLowMakeupVal').textContent = document.getElementById('dualLowMakeup').value + ' dB';
  
  document.getElementById('dualHighThreshVal').textContent = document.getElementById('dualHighThresh').value + ' dB';
  document.getElementById('dualHighRatioVal').textContent = document.getElementById('dualHighRatio').value + ':1';
  document.getElementById('dualHighAttackVal').textContent = document.getElementById('dualHighAttack').value + ' ms';
  document.getElementById('dualHighReleaseVal').textContent = document.getElementById('dualHighRelease').value + ' ms';
  document.getElementById('dualHighMakeupVal').textContent = document.getElementById('dualHighMakeup').value + ' dB';
}

function routeThroughDualComp(inputNode) {
  if (!dualCompEnabled) return inputNode;
  
  // Split into low and high
  const splitter = audioCtx.createChannelSplitter(2);
  inputNode.connect(splitter);
  
  // Low path
  const lowFilter = audioCtx.createBiquadFilter();
  lowFilter.type = 'lowpass';
  lowFilter.frequency.value = 2000;
  
  const lowGain = audioCtx.createGain();
  lowGain.gain.value = dbToGain(parseFloat(document.getElementById('dualLowMakeup').value) || 0);
  
  splitter.connect(lowFilter, 0);
  lowFilter.connect(dualLowComp);
  dualLowComp.connect(lowGain);
  
  // High path
  const highFilter = audioCtx.createBiquadFilter();
  highFilter.type = 'highpass';
  highFilter.frequency.value = 2000;
  
  const highGain = audioCtx.createGain();
  highGain.gain.value = dbToGain(parseFloat(document.getElementById('dualHighMakeup').value) || 0);
  
  splitter.connect(highFilter, 0);
  highFilter.connect(dualHighComp);
  dualHighComp.connect(highGain);
  
  // Merge back
  const merger = audioCtx.createChannelMerger(2);
  lowGain.connect(merger, 0, 0);
  highGain.connect(merger, 0, 1);
  
  return merger;
}

function toggleDualComp() {
  dualCompEnabled = !dualCompEnabled;
  document.getElementById('dualCompToggle').classList.toggle('on', dualCompEnabled);
  if (sourceNode) buildAudioChain();
}

// ==================== LIMITER ====================
function updateLimiter() {
  if (!limiterNode) return;
  limiterNode.threshold.value = parseFloat(document.getElementById('limiterThresh').value);
  limiterNode.ratio.value = parseFloat(document.getElementById('limiterRatio').value);
  limiterNode.attack.value = parseFloat(document.getElementById('limiterAttack').value) / 1000;
  limiterNode.release.value = parseFloat(document.getElementById('limiterRelease').value) / 1000;
  
  document.getElementById('limiterThreshVal').textContent = document.getElementById('limiterThresh').value + ' dB';
  document.getElementById('limiterRatioVal').textContent = document.getElementById('limiterRatio').value + ':1';
  document.getElementById('limiterAttackVal').textContent = document.getElementById('limiterAttack').value + ' ms';
  document.getElementById('limiterReleaseVal').textContent = document.getElementById('limiterRelease').value + ' ms';
  document.getElementById('limiterCeilingVal').textContent = document.getElementById('limiterCeiling').value + ' dB';
}

// ==================== BASS EQ ====================
let bassEnabled = false;
let antiDistEnabled = false;

function toggleBass() {
  bassEnabled = !bassEnabled;
  document.getElementById('bassToggle').classList.toggle('on', bassEnabled);
  if (bassNode) {
    bassNode.gain.value = bassEnabled ? 4 : 0;
  }
}

function toggleAntiDist() {
  antiDistEnabled = !antiDistEnabled;
  document.getElementById('antiDistToggle').classList.toggle('on', antiDistEnabled);
}

// ==================== STEREO ENHANCER ====================
let stereoEnabled = true;
let monoCompatEnabled = true;

function toggleStereo() {
  stereoEnabled = !stereoEnabled;
  document.getElementById('stereoToggle').classList.toggle('on', stereoEnabled);
  if (sourceNode) buildAudioChain();
}

function toggleMonoCompat() {
  monoCompatEnabled = !monoCompatEnabled;
  document.getElementById('monoCompatToggle').classList.toggle('on', monoCompatEnabled);
}

function routeStereoEnhancer() {
  if (!stereoSplitter) return;
  
  const width = parseFloat(document.getElementById('stereoWidthKnob')?.dataset?.value || 100) / 100;
  const midGain = dbToGain(parseFloat(document.getElementById('midLevelKnob')?.dataset?.value || 0));
  const sideGain = dbToGain(parseFloat(document.getElementById('sideLevelKnob')?.dataset?.value || 0));
  
  // Mid/Side processing
  const midGainNode = audioCtx.createGain();
  midGainNode.gain.value = midGain;
  
  const sideGainNode = audioCtx.createGain();
  sideGainNode.gain.value = sideGain * width;
  
  // L+R = Mid, L-R = Side (simplified)
  stereoSplitter.connect(midGainNode, 0);
  stereoSplitter.connect(midGainNode, 1);
  
  // Side signal (simplified width control)
  const sideL = audioCtx.createGain();
  const sideR = audioCtx.createGain();
  sideL.gain.value = width;
  sideR.gain.value = width;
  
  stereoSplitter.connect(sideL, 0);
  stereoSplitter.connect(sideR, 1);
  
  // Merge back
  midGainNode.connect(stereoMerger, 0, 0);
  midGainNode.connect(stereoMerger, 0, 1);
  sideL.connect(stereoMerger, 0, 0);
  sideR.connect(stereoMerger, 0, 1);
}

// ==================== MASTER MULTIBAND PROCESSOR ====================
let masterEnabled = true;

function createMasterBandNodes() {
  const bands = [
    { name: 'Sub', class: 'sub', range: '20-60Hz', threshold: -30, ratio: 2, attack: 20, release: 250, makeup: 3 },
    { name: 'Bass', class: 'bass', range: '60-250Hz', threshold: -26, ratio: 2.5, attack: 15, release: 200, makeup: 2 },
    { name: 'Low-Mid', class: 'mid', range: '250-800Hz', threshold: -24, ratio: 2, attack: 10, release: 180, makeup: 1 },
    { name: 'Presence', class: 'presence', range: '800-5kHz', threshold: -22, ratio: 2.5, attack: 8, release: 150, makeup: 2 },
    { name: 'Air', class: 'air', range: '5-20kHz', threshold: -28, ratio: 1.5, attack: 5, release: 120, makeup: 3 }
  ];
  
  masterCompNodes = bands.map((b, i) => {
    const comp = audioCtx.createDynamicsCompressor();
    comp.threshold.value = b.threshold;
    comp.ratio.value = b.ratio;
    comp.attack.value = b.attack / 1000;
    comp.release.value = b.release / 1000;
    comp.knee.value = 8;
    return { ...b, compressor: comp };
  });
  
  renderMasterUI(bands);
}

function renderMasterUI(bands) {
  const container = document.getElementById('masterBands');
  container.innerHTML = '';
  
  bands.forEach((b, i) => {
    const div = document.createElement('div');
    div.className = 'mb-band';
    div.innerHTML = `
      <div class="mb-band-header">
        <span class="mb-band-name ${b.class}">${b.name}</span>
        <span style="font-size:9px;color:var(--text-dim);">${b.range}</span>
      </div>
      <div class="mb-band-params">
        <div class="mb-param">
          <label>Threshold</label>
          <input type="range" min="-60" max="0" value="${b.threshold}" id="mThresh${i}" oninput="updateMaster(${i},'threshold',this.value)">
          <span class="param-value" id="mThresh${i}Val">${b.threshold} dB</span>
        </div>
        <div class="mb-param">
          <label>Ratio</label>
          <input type="range" min="1" max="10" value="${b.ratio}" step="0.5" id="mRatio${i}" oninput="updateMaster(${i},'ratio',this.value)">
          <span class="param-value" id="mRatio${i}Val">${b.ratio}:1</span>
        </div>
        <div class="mb-param">
          <label>Attack</label>
          <input type="range" min="1" max="100" value="${b.attack}" id="mAttack${i}" oninput="updateMaster(${i},'attack',this.value)">
          <span class="param-value" id="mAttack${i}Val">${b.attack} ms</span>
        </div>
        <div class="mb-param">
          <label>Release</label>
          <input type="range" min="10" max="500" value="${b.release}" id="mRelease${i}" oninput="updateMaster(${i},'release',this.value)">
          <span class="param-value" id="mRelease${i}Val">${b.release} ms</span>
        </div>
        <div class="mb-param">
          <label>Makeup</label>
          <input type="range" min="0" max="12" value="${b.makeup}" id="mMakeup${i}" oninput="updateMaster(${i},'makeup',this.value)">
          <span class="param-value" id="mMakeup${i}Val">${b.makeup} dB</span>
        </div>
      </div>
    `;
    container.appendChild(div);
  });
}

function updateMaster(band, param, value) {
  value = parseFloat(value);
  const node = masterCompNodes[band].compressor;
  if (param === 'threshold') { node.threshold.value = value; document.getElementById(`mThresh${band}Val`).textContent = value + ' dB'; }
  else if (param === 'ratio') { node.ratio.value = value; document.getElementById(`mRatio${band}Val`).textContent = value + ':1'; }
  else if (param === 'attack') { node.attack.value = value / 1000; document.getElementById(`mAttack${band}Val`).textContent = value + ' ms'; }
  else if (param === 'release') { node.release.value = value / 1000; document.getElementById(`mRelease${band}Val`).textContent = value + ' ms'; }
  else if (param === 'makeup') { document.getElementById(`mMakeup${band}Val`).textContent = value + ' dB'; }
}

function toggleMaster() {
  masterEnabled = !masterEnabled;
  document.getElementById('masterToggle').classList.toggle('on', masterEnabled);
  if (sourceNode) buildAudioChain();
}

function routeThroughMasterBands(inputNode) {
  if (!masterEnabled) return inputNode;
  
  let current = inputNode;
  masterCompNodes.forEach((band, i) => {
    const makeup = audioCtx.createGain();
    makeup.gain.value = dbToGain(parseFloat(document.getElementById(`mMakeup${i}`).value) || 0);
    
    current.connect(band.compressor);
    band.compressor.connect(makeup);
    current = makeup;
  });
  
  return current;
}

// ==================== KNOB INTERACTION ====================
document.addEventListener('mousedown', function(e) {
  const knob = e.target.closest('.knob-container');
  if (!knob) return;
  
  e.preventDefault();
  const startY = e.clientY;
  const startVal = parseFloat(knob.dataset.value);
  const min = parseFloat(knob.dataset.min);
  const max = parseFloat(knob.dataset.max);
  const range = max - min;
  
  function onMove(e) {
    const delta = (startY - e.clientY) * 0.5;
    let newVal = startVal + (delta / 100) * range;
    newVal = Math.max(min, Math.min(max, newVal));
    
    knob.dataset.value = newVal;
    updateKnobDisplay(knob, newVal);
    applyKnobValue(knob, newVal);
  }
  
  function onUp() {
    document.removeEventListener('mousemove', onMove);
    document.removeEventListener('mouseup', onUp);
  }
  
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
});

function updateKnobDisplay(knob, value) {
  const min = parseFloat(knob.dataset.min);
  const max = parseFloat(knob.dataset.max);
  const normalized = (value - min) / (max - min);
  const circumference = 113;
  const offset = circumference - (normalized * circumference * 0.75 + circumference * 0.125);
  
  const fill = knob.querySelector('.knob-fill');
  if (fill) fill.style.strokeDashoffset = offset;
  
  const indicator = knob.querySelector('.knob-indicator');
  if (indicator) {
    const angle = -90 + (normalized * 270 - 135);
    const rad = (angle * Math.PI) / 180;
    const x = 50 + 40 * Math.cos(rad);
    const y = 50 + 40 * Math.sin(rad);
    indicator.style.left = x + '%';
    indicator.style.top = y + '%';
    indicator.style.transform = 'translate(-50%, -50%)';
  }
  
  const unit = knob.dataset.unit || '';
  const valDisplay = knob.querySelector('.knob-value');
  if (valDisplay) {
    if (unit === '%') valDisplay.textContent = value.toFixed(0) + '%';
    else if (unit === 'Hz') valDisplay.textContent = value.toFixed(0) + ' Hz';
    else if (unit === 'dB') valDisplay.textContent = value.toFixed(1) + ' dB';
    else valDisplay.textContent = value.toFixed(1);
  }
}

function applyKnobValue(knob, value) {
  const param = knob.dataset.param;
  value = parseFloat(value);
  
  switch(param) {
    case 'inputGain':
      if (inputGainNode) inputGainNode.gain.value = dbToGain(value);
      break;
    case 'bassFreq':
      if (bassNode) bassNode.frequency.value = value;
      break;
    case 'bassGain':
      if (bassNode) bassNode.gain.value = bassEnabled ? value : 0;
      break;
    case 'bassQ':
      if (bassNode) bassNode.Q.value = value;
      break;
    case 'subBass':
      if (subBassNode) subBassNode.gain.value = bassEnabled ? value : 0;
      break;
    case 'stereoWidth':
      if (sourceNode) buildAudioChain();
      break;
    case 'midLevel':
      if (sourceNode) buildAudioChain();
      break;
    case 'sideLevel':
      if (sourceNode) buildAudioChain();
      break;
    case 'outputVol':
      if (outputGainNode) outputGainNode.gain.value = dbToGain(value);
      break;
  }
}

function dbToGain(db) {
  return Math.pow(10, db / 20);
}

// ==================== METERING & ANALYZERS ====================
function startMeters() {
  const inputCanvas = document.getElementById('inputAnalyzer');
  const outputCanvas = document.getElementById('outputAnalyzer');
  
  if (inputCanvas) inputCanvas.width = inputCanvas.offsetWidth * 2;
  if (inputCanvas) inputCanvas.height = inputCanvas.offsetHeight * 2;
  if (outputCanvas) outputCanvas.width = outputCanvas.offsetWidth * 2;
  if (outputCanvas) outputCanvas.height = outputCanvas.offsetHeight * 2;
  
  function animate() {
    animFrameId = requestAnimationFrame(animate);
    
    if (!audioCtx || audioCtx.state !== 'running') return;
    
    // Input analyzer
    if (analyserInput) {
      const dataArray = new Uint8Array(analyserInput.frequencyBinCount);
      analyserInput.getByteTimeDomainData(dataArray);
      
      // Waveform
      drawWaveform(inputCanvas, dataArray, '#00ff88');
      
      // Meters
      const freqData = new Uint8Array(analyserInput.frequencyBinCount);
      analyserInput.getByteFrequencyData(freqData);
      const level = calculateLevel(freqData);
      updateMeter('inputMeterL', level - 3);
      updateMeter('inputMeterR', level - 3);
    }
    
    // Output analyzer
    if (analyserOutput) {
      const dataArray = new Uint8Array(analyserOutput.frequencyBinCount);
      analyserOutput.getByteTimeDomainData(dataArray);
      
      drawWaveform(outputCanvas, dataArray, '#00ccff');
      
      // Output meters
      const freqData = new Uint8Array(analyserOutput.frequencyBinCount);
      analyserOutput.getByteFrequencyData(freqData);
      const level = calculateLevel(freqData);
      updateMeter('outputMeterL', level);
      updateMeter('outputMeterR', level - 1);
      
      // Loudness estimate
      const loudness = -60 + (level / 255) * 60;
      document.getElementById('loudnessDisplay').innerHTML = loudness.toFixed(1) + '<span class="loudness-unit">LUFS</span>';
      
      const peak = -60 + (Math.max(...freqData) / 255) * 60;
      document.getElementById('truePeakDisplay').innerHTML = peak.toFixed(1) + '<span class="loudness-unit">dBTP</span>';
    }
    
    // Phase display
    if (stereoSplitter) {
      const phase = 0; // Simplified
      document.getElementById('phaseFill').style.width = '50%';
      document.getElementById('phaseValue').textContent = '0°';
    }
  }
  
  animate();
}

function calculateLevel(freqData) {
  if (!freqData || freqData.length === 0) return 0;
  let sum = 0;
  for (let i = 0; i < freqData.length; i++) {
    sum += freqData[i];
  }
  return sum / freqData.length;
}

function updateMeter(elementId, level) {
  const el = document.getElementById(elementId);
  if (!el) return;
  
  // Map 0-255 to 0-100%
  const pct = Math.min(100, Math.max(0, (level / 255) * 100));
  el.style.height = pct + '%';
  
  // Color based on level
  if (pct > 85) {
    el.style.background = 'var(--meter-red)';
    el.style.boxShadow = '0 0 8px rgba(255,51,51,0.5)';
  } else if (pct > 65) {
    el.style.background = 'var(--meter-yellow)';
    el.style.boxShadow = '0 0 8px rgba(255,204,0,0.5)';
  } else {
    el.style.background = 'var(--meter-green)';
    el.style.boxShadow = '0 0 8px rgba(0,255,102,0.3)';
  }
}

function drawWaveform(canvas, dataArray, color) {
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const width = canvas.width;
  const height = canvas.height;
  
  ctx.clearRect(0, 0, width, height);
  
  // Grid
  ctx.strokeStyle = 'rgba(255,255,255,0.03)';
  ctx.lineWidth = 1;
  for (let i = 0; i < 10; i++) {
    const y = (height / 10) * i;
    ctx.beginPath();
    ctx.moveTo(0, y);
    ctx.lineTo(width, y);
    ctx.stroke();
  }
  
  // Center line
  ctx.strokeStyle = 'rgba(255,255,255,0.08)';
  ctx.beginPath();
  ctx.moveTo(0, height / 2);
  ctx.lineTo(width, height / 2);
  ctx.stroke();
  
  // Waveform
  ctx.lineWidth = 2;
  ctx.strokeStyle = color;
  ctx.shadowColor = color;
  ctx.shadowBlur = 5;
  ctx.beginPath();
  
  const sliceWidth = width / dataArray.length;
  let x = 0;
  
  for (let i = 0; i < dataArray.length; i++) {
    const v = dataArray[i] / 128.0;
    const y = (v * height) / 2;
    
    if (i === 0) ctx.moveTo(x, y);
    else ctx.lineTo(x, y);
    
    x += sliceWidth;
  }
  
  ctx.stroke();
  ctx.shadowBlur = 0;
  
  // Frequency bars at bottom
  if (analyserOutput) {
    const freqData = new Uint8Array(analyserOutput.frequencyBinCount);
    analyserOutput.getByteFrequencyData(freqData);
    
    const barCount = 64;
    const barWidth = width / barCount;
    
    ctx.fillStyle = 'rgba(0, 204, 255, 0.15)';
    for (let i = 0; i < barCount; i++) {
      const idx = Math.floor(i * freqData.length / barCount);
      const barHeight = (freqData[idx] / 255) * height * 0.3;
      ctx.fillRect(i * barWidth, height - barHeight, barWidth - 1, barHeight);
    }
  }
}

// ==================== INITIALIZATION ====================
window.addEventListener('load', () => {
  refreshDevices();
  
  // Initialize knob displays
  document.querySelectorAll('.knob-container').forEach(knob => {
    updateKnobDisplay(knob, parseFloat(knob.dataset.value));
  });
});

// Handle window resize for canvases
window.addEventListener('resize', () => {
  const inputCanvas = document.getElementById('inputAnalyzer');
  const outputCanvas = document.getElementById('outputAnalyzer');
  if (inputCanvas) { inputCanvas.width = inputCanvas.offsetWidth * 2; inputCanvas.height = inputCanvas.offsetHeight * 2; }
  if (outputCanvas) { outputCanvas.width = outputCanvas.offsetWidth * 2; outputCanvas.height = outputCanvas.offsetHeight * 2; }
});
</script>
</body>
</html>
