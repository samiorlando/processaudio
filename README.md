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
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg-dark);
    color: var(--text-primary);
    font-family: 'Segoe UI', 'Roboto', 'Helvetica Neue', Arial, sans-serif;
    overflow-x: hidden;
    min-height: 100vh;
  }

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

  .header-left { display: flex; align-items: center; gap: 15px; }
  .logo-text { font-size: 11px; letter-spacing: 3px; color: var(--text-secondary); text-transform: uppercase; }
  .logo-main { font-size: 24px; font-weight: 700; letter-spacing: 4px; background: linear-gradient(135deg, var(--accent-green), var(--accent-cyan)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .logo-sub { font-size: 10px; letter-spacing: 2px; color: var(--text-dim); }

  .power-btn {
    width: 50px; height: 50px; border-radius: 50%;
    border: 2px solid #444; background: radial-gradient(circle, #1a1a1a, #111);
    cursor: pointer; display: flex; align-items: center; justify-content: center;
    transition: all 0.3s ease; position: relative;
  }
  .power-btn:hover { border-color: #666; }
  .power-btn.on { border-color: var(--accent-green); box-shadow: 0 0 15px rgba(0,255,136,0.4), inset 0 0 10px rgba(0,255,136,0.1); }
  .power-icon { width: 20px; height: 20px; border: 2px solid #555; border-radius: 50%; position: relative; }
  .power-icon::after { content: ''; position: absolute; top: -4px; left: 50%; transform: translateX(-50%); width: 2px; height: 10px; background: #555; border-radius: 1px; }
  .power-btn.on .power-icon { border-color: var(--accent-green); box-shadow: 0 0 5px var(--accent-green); }
  .power-btn.on .power-icon::after { background: var(--accent-green); box-shadow: 0 0 5px var(--accent-green); }
  .power-label { font-size: 9px; letter-spacing: 2px; text-transform: uppercase; margin-top: 4px; color: var(--text-dim); text-align: center; }

  .header-right { display: flex; align-items: center; gap: 15px; }
  .sample-rate-display { font-size: 11px; color: var(--text-dim); font-family: 'Courier New', monospace; background: #0a0a0f; padding: 4px 8px; border-radius: 4px; border: 1px solid #222; }
  .status-dot { width: 8px; height: 8px; border-radius: 50%; background: #333; display: inline-block; margin-right: 5px; }
  .status-dot.active { background: var(--accent-green); box-shadow: 0 0 5px var(--accent-green); animation: pulse 2s infinite; }
  @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.5; } }

  .main-container { padding: 10px; display: flex; flex-direction: column; gap: 10px; }
  .panel { background: var(--bg-panel); border: 1px solid var(--border-color); border-radius: 6px; overflow: hidden; }
  .panel-header { background: linear-gradient(180deg, #1e1e2a, #16161f); padding: 8px 15px; display: flex; align-items: center; justify-content: space-between; border-bottom: 1px solid var(--border-color); }
  .panel-title { font-size: 11px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: var(--accent-green-dim); }
  .panel-body { padding: 15px; }
  .disabled { opacity: 0.3; pointer-events: none; }

  .knob-container { display: flex; flex-direction: column; align-items: center; gap: 5px; cursor: pointer; user-select: none; }
  .knob-wrapper { width: 56px; height: 56px; position: relative; }
  .knob-svg { width: 100%; height: 100%; transform: rotate(-90deg); }
  .knob-bg { fill: none; stroke: #1a1a2a; stroke-width: 3; }
  .knob-fill { fill: none; stroke: var(--accent-green); stroke-width: 3; stroke-linecap: round; transition: stroke-dashoffset 0.1s ease; filter: drop-shadow(0 0 3px rgba(0,255,136,0.4)); }
  .knob-fill.amber { stroke: var(--accent-amber); filter: drop-shadow(0 0 3px rgba(255,170,0,0.4)); }
  .knob-fill.blue { stroke: var(--accent-blue); filter: drop-shadow(0 0 3px rgba(68,136,255,0.4)); }
  .knob-fill.cyan { stroke: var(--accent-cyan); filter: drop-shadow(0 0 3px rgba(0,204,255,0.4)); }
  .knob-fill.red { stroke: var(--accent-red); filter: drop-shadow(0 0 3px rgba(255,51,68,0.4)); }
  .knob-inner { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 40px; height: 40px; border-radius: 50%; background: radial-gradient(circle at 35% 35%, #333344, #1a1a22); border: 1px solid #333; }
  .knob-indicator { position: absolute; top: 4px; left: 50%; transform: translateX(-50%); width: 2px; height: 8px; background: var(--accent-green); border-radius: 1px; box-shadow: 0 0 4px var(--accent-green); }
  .knob-label { font-size: 9px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 1px; }
  .knob-value { font-size: 10px; color: var(--accent-green); font-family: 'Courier New', monospace; min-width: 50px; text-align: center; }

  input[type="range"] { -webkit-appearance: none; appearance: none; background: transparent; cursor: pointer; }
  input[type="range"]::-webkit-slider-runnable-track { height: 4px; border-radius: 2px; background: #1a1a2a; }
  input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; width: 16px; height: 16px; border-radius: 50%; background: radial-gradient(circle at 35% 35%, #555566, #333344); border: 2px solid var(--accent-green); box-shadow: 0 0 5px rgba(0,255,136,0.3); margin-top: -6px; }
  .slider-horizontal { width: 100%; }
  .slider-vertical { writing-mode: vertical-lr; direction: rtl; height: 100px; width: 20px; }
  .slider-label { font-size: 9px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 1px; text-align: center; }
  .slider-value { font-size: 10px; color: var(--accent-green); font-family: 'Courier New', monospace; }

  .meter-container { display: flex; flex-direction: column; align-items: center; gap: 3px; }
  .meter-bar { width: 24px; height: 200px; background: #0a0a12; border-radius: 3px; position: relative; overflow: hidden; border: 1px solid #222; }
  .meter-fill { position: absolute; bottom: 0; left: 0; width: 100%; height: 0%; border-radius: 2px; transition: height 0.05s linear; background: linear-gradient(0deg, var(--meter-green) 0%, var(--meter-green) 60%, var(--meter-yellow) 80%, var(--meter-red) 100%); box-shadow: 0 0 8px rgba(0,255,102,0.3); }
  .meter-scale { position: absolute; right: 2px; top: 0; height: 100%; display: flex; flex-direction: column; justify-content: space-between; padding: 2px 0; }
  .meter-scale span { font-size: 7px; color: var(--text-dim); font-family: 'Courier New', monospace; }
  .meter-label { font-size: 9px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 1px; }

  .eq-bands { display: flex; justify-content: space-around; align-items: flex-end; gap: 8px; flex-wrap: wrap; }
  .eq-band { display: flex; flex-direction: column; align-items: center; gap: 4px; flex: 1; min-width: 60px; }
  .graphic-eq { display: flex; justify-content: space-around; align-items: flex-end; gap: 4px; padding: 10px 0; }
  .geq-band { display: flex; flex-direction: column; align-items: center; gap: 4px; flex: 1; }
  .geq-slider { height: 130px; width: 18px; }
  .geq-freq { font-size: 8px; color: var(--text-dim); font-family: 'Courier New', monospace; }

  .mb-comp-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 12px; }
  .mb-band { background: var(--bg-section); border: 1px solid var(--border-color); border-radius: 5px; padding: 10px; }
  .mb-band-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; padding-bottom: 5px; border-bottom: 1px solid #222; }
  .mb-band-name { font-size: 10px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; }
  .mb-band-name.sub { color: #aa44ff; }
  .mb-band-name.bass { color: var(--accent-cyan); }
  .mb-band-name.mid { color: var(--accent-green); }
  .mb-band-name.presence { color: var(--accent-amber); }
  .mb-band-name.air { color: #ff88aa; }
  .mb-band-params { display: grid; grid-template-columns: 1fr 1fr; gap: 6px; }
  .mb-param { display: flex; flex-direction: column; gap: 2px; }
  .mb-param label { font-size: 8px; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; }
  .mb-param input[type="range"] { width: 100%; height: 4px; }
  .mb-param .param-value { font-size: 9px; color: var(--accent-green); font-family: 'Courier New', monospace; text-align: right; }

  .analyzer-container { width: 100%; height: 80px; background: #080810; border-radius: 4px; border: 1px solid #1a1a2a; overflow: hidden; }
  .analyzer-canvas { width: 100%; height: 100%; }
  .row { display: flex; gap: 10px; align-items: flex-start; }
  .col { flex: 1; display: flex; flex-direction: column; gap: 10px; }
  .bass-section, .stereo-section, .output-section { display: flex; align-items: center; gap: 15px; flex-wrap: wrap; }
  .output-meters { display: flex; gap: 10px; }
  
  .btn, .preset-btn { padding: 6px 14px; border: 1px solid var(--border-color); background: linear-gradient(180deg, #1e1e2a, #16161f); color: var(--text-secondary); font-size: 10px; letter-spacing: 1px; text-transform: uppercase; border-radius: 4px; cursor: pointer; transition: all 0.2s; }
  .btn:hover, .preset-btn:hover { border-color: var(--accent-green-dim); color: var(--accent-green); }
  .btn.active, .preset-btn.active { border-color: var(--accent-green); color: var(--accent-green); box-shadow: 0 0 8px rgba(0,255,136,0.2); background: rgba(0,255,136,0.05); }
  
  select { background: #0d0d15; border: 1px solid var(--border-color); color: var(--text-primary); padding: 5px 10px; font-size: 11px; border-radius: 4px; cursor: pointer; }
  select:focus { outline: none; border-color: var(--accent-green-dim); }
  .toggle-switch { position: relative; width: 40px; height: 20px; background: #1a1a22; border-radius: 10px; cursor: pointer; border: 1px solid #333; transition: all 0.3s; }
  .toggle-switch.on { background: rgba(0,255,136,0.15); border-color: var(--accent-green); }
  .toggle-switch::after { content: ''; position: absolute; top: 2px; left: 2px; width: 14px; height: 14px; border-radius: 50%; background: #444; transition: all 0.3s; }
  .toggle-switch.on::after { left: 22px; background: var(--accent-green); box-shadow: 0 0 5px var(--accent-green); }

  .loudness-display { font-family: 'Courier New', monospace; font-size: 28px; font-weight: 700; color: var(--accent-green); text-shadow: 0 0 10px rgba(0,255,136,0.3); text-align: center; min-width: 120px; }
  .loudness-unit { font-size: 12px; color: var(--text-secondary); margin-left: 3px; }
  .loudness-label { font-size: 9px; color: var(--text-dim); text-transform: uppercase; letter-spacing: 1px; text-align: center; }

  @media (max-width: 768px) { .header { flex-direction: column; gap: 10px; } .mb-comp-grid { grid-template-columns: 1fr; } .eq-bands, .graphic-eq { flex-direction: column; align-items: center; } .row { flex-direction: column; } }
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <div>
      <div class="logo-text">Professional Audio Processing</div>
      <div class="logo-main">OPTIMOD-PC 1600</div>
      <div class="logo-sub">Multiband Audio Processor — Web Edition</div>
    </div>
  </div>
  <div class="header-right">
    <div style="text-align:center;">
      <div class="power-btn" id="powerBtn" onclick="togglePower()"><div class="power-icon"></div></div>
      <div class="power-label">POWER</div>
    </div>
    <div class="sample-rate-display" id="sampleRateDisplay">-- kHz</div>
    <div>
      <div class="status-dot" id="statusDot"></div>
      <span style="font-size:10px;color:var(--text-dim);">PROCESSING</span>
    </div>
  </div>
</div>

<div class="main-container" id="mainContainer">
  <!-- 1. INPUT SECTION -->
  <div class="panel disabled" id="inputPanel">
    <div class="panel-header">
      <span class="panel-title">🔊 Input Section</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <select id="inputSelect" onchange="changeInput()"><option value="">Solicitando dispositivos...</option></select>
        <button class="btn" onclick="refreshDevices()">🔄 Refresh</button>
      </div>
    </div>
    <div class="panel-body">
      <div class="row">
        <div class="col" style="flex:0 0 auto;align-items:center;">
          <div class="knob-container" id="inputGainKnob" data-param="inputGain" data-min="0" data-max="24" data-value="12" data-unit="dB">
            <div class="knob-wrapper">
              <svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/></svg>
              <div class="knob-inner"><div class="knob-indicator"></div></div>
            </div>
            <span class="knob-label">Gain</span><span class="knob-value">12.0 dB</span>
          </div>
        </div>
        <div class="col" style="flex:1;">
          <div style="display:flex;gap:10px;align-items:flex-start;">
            <div class="meter-container"><div class="meter-label">L</div><div class="meter-bar"><div class="meter-fill" id="inputMeterL"></div></div></div>
            <div class="meter-container"><div class="meter-label">R</div><div class="meter-bar"><div class="meter-fill" id="inputMeterR"></div></div></div>
            <div style="flex:1;"><div class="analyzer-container"><canvas class="analyzer-canvas" id="inputAnalyzer"></canvas></div></div>
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
        <button class="preset-btn" onclick="loadEqPreset('flat', this)">Flat</button>
        <button class="preset-btn" onclick="loadEqPreset('bassBoost', this)">Bass Boost</button>
        <button class="preset-btn" onclick="loadEqPreset('vocalClarity', this)">Vocal Clarity</button>
        <button class="preset-btn" onclick="loadEqPreset('radio', this)">Radio</button>
      </div>
    </div>
    <div class="panel-body"><div class="eq-bands" id="paramEqBands"></div></div>
  </div>

  <!-- 3. MULTIBAND COMPRESSOR -->
  <div class="panel disabled" id="mbCompPanel">
    <div class="panel-header">
      <span class="panel-title">🎛️ Multiband Compressor</span>
      <div class="toggle-switch on" id="mbCompToggle" onclick="toggleModule('mbComp')"></div>
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
        <div class="toggle-switch on" id="geqToggle" onclick="toggleModule('geq')"></div>
      </div>
    </div>
    <div class="panel-body"><div class="graphic-eq" id="geqBands"></div></div>
  </div>

  <!-- 5. DUAL BAND COMPRESSOR + LIMITER -->
  <div class="panel disabled" id="dualCompPanel">
    <div class="panel-header">
      <span class="panel-title">⚡ Dual Band Compressor + Limiter</span>
      <div class="toggle-switch on" id="dualCompToggle" onclick="toggleModule('dualComp')"></div>
    </div>
    <div class="panel-body">
      <div class="row">
        <div class="mb-band" style="flex:1;">
          <div class="mb-band-header"><span class="mb-band-name bass">LOW BAND</span><span style="font-size:9px;color:var(--text-dim);">&lt; 2kHz</span></div>
          <div class="mb-band-params">
            <div class="mb-param"><label>Threshold</label><input type="range" min="-60" max="0" value="-20" id="dualLowThresh" oninput="updateDualComp('low','thresh',this.value)"><span class="param-value" id="dualLowThreshVal">-20 dB</span></div>
            <div class="mb-param"><label>Ratio</label><input type="range" min="1" max="20" value="4" id="dualLowRatio" oninput="updateDualComp('low','ratio',this.value)"><span class="param-value" id="dualLowRatioVal">4:1</span></div>
            <div class="mb-param"><label>Attack</label><input type="range" min="0.1" max="100" value="10" step="0.1" id="dualLowAttack" oninput="updateDualComp('low','attack',this.value)"><span class="param-value" id="dualLowAttackVal">10 ms</span></div>
            <div class="mb-param"><label>Makeup</label><input type="range" min="0" max="18" value="3" id="dualLowMakeup" oninput="updateDualComp('low','makeup',this.value)"><span class="param-value" id="dualLowMakeupVal">3 dB</span></div>
          </div>
        </div>
        <div class="mb-band" style="flex:1;">
          <div class="mb-band-header"><span class="mb-band-name air">HIGH BAND</span><span style="font-size:9px;color:var(--text-dim);">&gt; 2kHz</span></div>
          <div class="mb-band-params">
            <div class="mb-param"><label>Threshold</label><input type="range" min="-60" max="0" value="-18" id="dualHighThresh" oninput="updateDualComp('high','thresh',this.value)"><span class="param-value" id="dualHighThreshVal">-18 dB</span></div>
            <div class="mb-param"><label>Ratio</label><input type="range" min="1" max="20" value="3" id="dualHighRatio" oninput="updateDualComp('high','ratio',this.value)"><span class="param-value" id="dualHighRatioVal">3:1</span></div>
            <div class="mb-param"><label>Attack</label><input type="range" min="0.1" max="100" value="5" step="0.1" id="dualHighAttack" oninput="updateDualComp('high','attack',this.value)"><span class="param-value" id="dualHighAttackVal">5 ms</span></div>
            <div class="mb-param"><label>Makeup</label><input type="range" min="0" max="18" value="2" id="dualHighMakeup" oninput="updateDualComp('high','makeup',this.value)"><span class="param-value" id="dualHighMakeupVal">2 dB</span></div>
          </div>
        </div>
        <div class="mb-band" style="flex:1;">
          <div class="mb-band-header"><span class="mb-band-name" style="color:var(--accent-red);">BRICKWALL LIMITER</span></div>
          <div class="mb-band-params">
            <div class="mb-param"><label>Threshold</label><input type="range" min="-12" max="0" value="-1" step="0.5" id="limiterThresh" oninput="updateLimiter('thresh',this.value)"><span class="param-value" id="limiterThreshVal">-1 dB</span></div>
            <div class="mb-param"><label>Ratio</label><input type="range" min="10" max="100" value="20" id="limiterRatio" oninput="updateLimiter('ratio',this.value)"><span class="param-value" id="limiterRatioVal">20:1</span></div>
            <div class="mb-param"><label>Release</label><input type="range" min="10" max="500" value="100" id="limiterRelease" oninput="updateLimiter('release',this.value)"><span class="param-value" id="limiterReleaseVal">100 ms</span></div>
            <div class="mb-param"><label>Output</label><input type="range" min="-3" max="0" value="-0.1" step="0.1" id="limiterCeiling" oninput="updateLimiter('ceiling',this.value)"><span class="param-value" id="limiterCeilingVal">-0.1 dB</span></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 6. BASS EQ -->
  <div class="panel disabled" id="bassPanel">
    <div class="panel-header"><span class="panel-title">🔥 Bass EQ Enhancement</span><div class="toggle-switch" id="bassToggle" onclick="toggleModule('bass')"></div></div>
    <div class="panel-body">
      <div class="bass-section">
        <div class="knob-container" id="bassFreqKnob" data-param="bassFreq" data-min="30" data-max="120" data-value="60" data-unit="Hz"><div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="80"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div><span class="knob-label">Frequency</span><span class="knob-value">60 Hz</span></div>
        <div class="knob-container" id="bassGainKnob" data-param="bassGain" data-min="0" data-max="12" data-value="4" data-unit="dB"><div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="90"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div><span class="knob-label">Boost</span><span class="knob-value">4.0 dB</span></div>
        <div style="margin-left:10px;"><div style="font-size:9px;color:var(--text-dim);margin-bottom:5px;">ANTI-CLIP</div><div class="toggle-switch on" id="antiDistToggle"></div><div style="font-size:8px;color:var(--text-dim);margin-top:3px;">Protection</div></div>
      </div>
    </div>
  </div>

  <!-- 7. STEREO ENHANCER -->
  <div class="panel disabled" id="stereoPanel">
    <div class="panel-header"><span class="panel-title">🌐 Stereo Enhancer</span><div class="toggle-switch on" id="stereoToggle" onclick="toggleModule('stereo')"></div></div>
    <div class="panel-body">
      <div class="stereo-section">
        <div class="knob-container" id="stereoWidthKnob" data-param="stereoWidth" data-min="0" data-max="200" data-value="100" data-unit="%"><div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill cyan" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div><span class="knob-label">Width</span><span class="knob-value">100%</span></div>
        <div class="knob-container" id="midLevelKnob" data-param="midLevel" data-min="-12" data-max="6" data-value="0" data-unit="dB"><div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill cyan" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div><span class="knob-label">Mid</span><span class="knob-value">0.0 dB</span></div>
        <div class="knob-container" id="sideLevelKnob" data-param="sideLevel" data-min="-24" data-max="6" data-value="0" data-unit="dB"><div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill cyan" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="56"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div><span class="knob-label">Side</span><span class="knob-value">0.0 dB</span></div>
      </div>
    </div>
  </div>

  <!-- 8. OUTPUT SECTION -->
  <div class="panel disabled" id="outputPanel">
    <div class="panel-header">
      <span class="panel-title">📤 Output Section</span>
      <select id="outputSelect" onchange="changeOutput(this.value)"><option value="">Default Output</option></select>
    </div>
    <div class="panel-body">
      <div class="output-section">
        <div class="knob-container" id="outputVolKnob" data-param="outputVol" data-min="-30" data-max="6" data-value="0" data-unit="dB"><div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 56 56"><circle class="knob-bg" cx="28" cy="28" r="24"/><circle class="knob-fill red" cx="28" cy="28" r="24" stroke-dasharray="113" stroke-dashoffset="0"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div><span class="knob-label">Output</span><span class="knob-value">0.0 dB</span></div>
        <div class="output-meters">
          <div class="meter-container"><div class="meter-label">L</div><div class="meter-bar"><div class="meter-fill" id="outputMeterL"></div></div></div>
          <div class="meter-container"><div class="meter-label">R</div><div class="meter-bar"><div class="meter-fill" id="outputMeterR"></div></div></div>
        </div>
        <div style="text-align:center;"><div class="loudness-display" id="loudnessDisplay">--<span class="loudness-unit">LUFS</span></div><div class="loudness-label">Integrated Loudness</div></div>
        <div style="text-align:center;"><div class="loudness-display" id="truePeakDisplay" style="color:var(--accent-amber);font-size:22px;">--<span class="loudness-unit">dBTP</span></div><div class="loudness-label">True Peak</div></div>
        <div class="analyzer-container" style="flex:1;min-width:200px;"><canvas class="analyzer-canvas" id="outputAnalyzer"></canvas></div>
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
let animFrameId = null;

// Audio Chain Nodes (Pre-created for stability)
let inputGainNode, paramEqNodes = [], mbCompNodes = [], geqNodes = [];
let dualLowComp, dualHighComp, dualSplitter, dualMerger, dualLowGain, dualHighGain;
let bassNode, limiterNode, outputGainNode;
let stereoMerger, stereoSplitter, midGainNode, sideGainNode, widthNodeL, widthNodeR;
let analyserInput, analyserOutput, masterAnalyser;

// State
let modules = { mbComp: true, geq: true, dualComp: true, bass: false, stereo: true };

// ==================== POWER & INIT ====================
async function togglePower() {
  const btn = document.getElementById('powerBtn');
  if (!isPowerOn) await powerOn(), btn.classList.add('on'), isPowerOn = true;
  else powerOff(), btn.classList.remove('on'), isPowerOn = false;
}

async function powerOn() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  await audioCtx.resume();
  document.getElementById('sampleRateDisplay').textContent = (audioCtx.sampleRate / 1000).toFixed(1) + ' kHz';
  document.getElementById('statusDot').classList.add('active');
  
  // Initialize nodes only once
  if (!inputGainNode) initNodes();
  
  enablePanels();
  refreshDevices();
  buildAudioChain();
  startMeters();
}

function powerOff() {
  if (inputStream) { inputStream.getTracks().forEach(t => t.stop()); inputStream = null; }
  if (sourceNode) { try { sourceNode.disconnect(); } catch(e){} sourceNode = null; }
  if (audioCtx) { audioCtx.close(); audioCtx = null; }
  if (animFrameId) cancelAnimationFrame(animFrameId);
  document.getElementById('statusDot').classList.remove('active');
  disablePanels();
}

function enablePanels() { document.querySelectorAll('.panel').forEach(p => p.classList.remove('disabled')); }
function disablePanels() { document.querySelectorAll('.panel').forEach(p => p.classList.add('disabled')); }

// ==================== NODE INITIALIZATION ====================
function initNodes() {
  inputGainNode = audioCtx.createGain();
  inputGainNode.gain.value = dbToGain(12);

  // Param EQ
  paramEqNodes = [
    { type: 'lowshelf', freq: 80, gain: 0, q: 0.7 },
    { type: 'peaking', freq: 250, gain: 0, q: 1 },
    { type: 'peaking', freq: 1000, gain: 0, q: 1 },
    { type: 'peaking', freq: 4000, gain: 0, q: 1 },
    { type: 'highshelf', freq: 12000, gain: 0, q: 0.7 }
  ].map((p, i) => {
    const n = audioCtx.createBiquadFilter();
    n.type = p.type; n.frequency.value = p.freq; n.gain.value = p.gain; n.Q.value = p.q;
    return n;
  });

  // Multiband Comp
  const mbSettings = [
    { range: '20-80', low: 20, high: 80, th: -24, r: 4, at: 0.015, rel: 0.2, mk: 4 },
    { range: '80-250', low: 80, high: 250, th: -22, r: 3.5, at: 0.012, rel: 0.18, mk: 3 },
    { range: '250-2k', low: 250, high: 2000, th: -20, r: 3, at: 0.008, rel: 0.15, mk: 2 },
    { range: '2k-6k', low: 2000, high: 6000, th: -22, r: 3.5, at: 0.005, rel: 0.12, mk: 2 },
    { range: '6k-16k', low: 6000, high: 16000, th: -24, r: 2.5, at: 0.003, rel: 0.1, mk: 3 }
  ];
  mbCompNodes = mbSettings.map(s => {
    const lp = audioCtx.createBiquadFilter(); lp.type = 'lowpass'; lp.frequency.value = s.high;
    const hp = audioCtx.createBiquadFilter(); hp.type = 'highpass'; hp.frequency.value = s.low;
    const comp = audioCtx.createDynamicsCompressor();
    comp.threshold.value = s.th; comp.ratio.value = s.r; comp.attack.value = s.at; comp.release.value = s.rel; comp.knee.value = 6;
    const mk = audioCtx.createGain(); mk.gain.value = dbToGain(s.mk);
    return { lp, hp, comp, mk, range: s.range };
  });

  // Graphic EQ
  geqNodes = [31,63,125,250,500,1000,2000,4000,6300,8000,10000,12500,14000,16000,18000].map(f => {
    const n = audioCtx.createBiquadFilter();
    n.type = 'peaking'; n.frequency.value = f; n.gain.value = 0; n.Q.value = 1.4;
    return n;
  });

  // Dual Band
  dualSplitter = audioCtx.createChannelSplitter(2);
  dualLowComp = audioCtx.createDynamicsCompressor();
  dualHighComp = audioCtx.createDynamicsCompressor();
  dualLowGain = audioCtx.createGain();
  dualHighGain = audioCtx.createGain();
  dualMerger = audioCtx.createChannelMerger(2);
  
  // Bass
  bassNode = audioCtx.createBiquadFilter();
  bassNode.type = 'lowshelf'; bassNode.frequency.value = 60; bassNode.gain.value = 0; bassNode.Q.value = 1.4;

  // Stereo
  stereoSplitter = audioCtx.createChannelSplitter(2);
  midGainNode = audioCtx.createGain(); midGainNode.gain.value = 1;
  sideGainNode = audioCtx.createGain(); sideGainNode.gain.value = 1;
  widthNodeL = audioCtx.createGain(); widthNodeL.gain.value = 1;
  widthNodeR = audioCtx.createGain(); widthNodeR.gain.value = 1;
  stereoMerger = audioCtx.createChannelMerger(2);

  // Limiter & Output
  limiterNode = audioCtx.createDynamicsCompressor();
  limiterNode.threshold.value = -1; limiterNode.ratio.value = 20; limiterNode.attack.value = 0.0005; limiterNode.release.value = 0.1; limiterNode.knee.value = 0;
  outputGainNode = audioCtx.createGain();
  outputGainNode.gain.value = dbToGain(0);

  // Analyzers
  analyserInput = audioCtx.createAnalyser(); analyserInput.fftSize = 2048;
  analyserOutput = audioCtx.createAnalyser(); analyserOutput.fftSize = 2048;
  masterAnalyser = audioCtx.createAnalyser(); masterAnalyser.fftSize = 4096;

  // UI Generation
  renderParamEqUI();
  renderMbCompUI(mbSettings);
  renderGeqUI();
  initKnobs();
}

// ==================== AUDIO CHAIN BUILDER ====================
function buildAudioChain() {
  if (!audioCtx || !sourceNode) return;

  // Disconnect all existing connections to rebuild safely
  try {
    sourceNode.disconnect();
    inputGainNode.disconnect();
    paramEqNodes.forEach(n => n.disconnect());
    mbCompNodes.forEach(n => { n.lp.disconnect(); n.hp.disconnect(); n.comp.disconnect(); n.mk.disconnect(); });
    geqNodes.forEach(n => n.disconnect());
    dualSplitter.disconnect(); dualLowComp.disconnect(); dualHighComp.disconnect(); dualLowGain.disconnect(); dualHighGain.disconnect(); dualMerger.disconnect();
    bassNode.disconnect();
    stereoSplitter.disconnect(); midGainNode.disconnect(); sideGainNode.disconnect(); widthNodeL.disconnect(); widthNodeR.disconnect(); stereoMerger.disconnect();
    limiterNode.disconnect(); outputGainNode.disconnect();
  } catch(e){}

  // 1. Input -> Analyser -> Input Gain
  sourceNode.connect(analyserInput);
  analyserInput.connect(inputGainNode);

  let current = inputGainNode;

  // 2. Parametric EQ
  paramEqNodes.forEach((node, i) => {
    current.connect(node);
    current = node;
  });

  // 3. Multiband Comp (Parallel Routing)
  if (modules.mbComp) {
    mbCompNodes.forEach((band, i) => {
      // Bandpass: Highpass -> Lowpass
      current.connect(band.lp);
      current.connect(band.hp);
      band.lp.connect(band.hp); // Series for bandpass effect
      band.hp.connect(band.comp);
      band.comp.connect(band.mk);
    });
    // Connect all band outputs to next stage (simple merge)
    const merger = audioCtx.createChannelMerger(2);
    mbCompNodes.forEach((band, i) => {
      band.mk.connect(merger, 0, i % 2);
      band.mk.connect(merger, 0, 1); // Duplicate to both channels for stability
    });
    current = merger;
  }

  // 4. Graphic EQ
  if (modules.geq) {
    geqNodes.forEach(node => {
      current.connect(node);
      current = node;
    });
  }

  // 5. Dual Band Comp
  if (modules.dualComp) {
    const lpFilter = audioCtx.createBiquadFilter();
    lpFilter.type = 'lowpass'; lpFilter.frequency.value = 2000;
    const hpFilter = audioCtx.createBiquadFilter();
    hpFilter.type = 'highpass'; hpFilter.frequency.value = 2000;
    
    current.connect(lpFilter); lpFilter.connect(dualLowComp); dualLowComp.connect(dualLowGain);
    current.connect(hpFilter); hpFilter.connect(dualHighComp); dualHighComp.connect(dualHighGain);
    
    dualLowGain.connect(dualMerger, 0, 0); dualLowGain.connect(dualMerger, 0, 1);
    dualHighGain.connect(dualMerger, 0, 0); dualHighGain.connect(dualMerger, 0, 1);
    current = dualMerger;
  }

  // 6. Bass EQ
  current.connect(bassNode);
  current = bassNode;

  // 7. Stereo Enhancer
  if (modules.stereo) {
    current.connect(stereoSplitter);
    // Mid: (L+R)/2
    const mid = audioCtx.createGain(); mid.gain.value = 0.5;
    stereoSplitter.connect(mid, 0); stereoSplitter.connect(mid, 1);
    mid.connect(midGainNode);

    // Side: (L-R)/2
    const side = audioCtx.createGain(); side.gain.value = 0.5;
    const invert = audioCtx.createGain(); invert.gain.value = -1;
    stereoSplitter.connect(side, 0);
    stereoSplitter.connect(invert, 1); invert.connect(side, 0);
    side.connect(sideGainNode);

    midGainNode.connect(stereoMerger, 0, 0); midGainNode.connect(stereoMerger, 0, 1);
    sideGainNode.connect(widthNodeL); widthNodeL.connect(stereoMerger, 0, 0);
    sideGainNode.connect(widthNodeR); widthNodeR.connect(stereoMerger, 0, 1);
    current = stereoMerger;
  }

  // 8. Limiter & Output
  current.connect(limiterNode);
  limiterNode.connect(outputGainNode);
  outputGainNode.connect(analyserOutput);
  analyserOutput.connect(audioCtx.destination);
  masterAnalyser.connect();
}

// ==================== DEVICE HANDLING ====================
async function refreshDevices() {
  try {
    // Request permission to get labels
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    stream.getTracks().forEach(t => t.stop());
    
    const devices = await navigator.mediaDevices.enumerateDevices();
    const inSelect = document.getElementById('inputSelect');
    const outSelect = document.getElementById('outputSelect');
    inSelect.innerHTML = '<option value="">Select Input...</option>';
    outSelect.innerHTML = '<option value="">Default Output</option>';
    
    devices.filter(d => d.kind === 'audioinput').forEach(d => {
      const opt = document.createElement('option');
      opt.value = d.deviceId;
      opt.textContent = d.label || `Micrófono ${inSelect.options.length}`;
      inSelect.appendChild(opt);
    });

    // Output devices (Chrome/Edge only via setSinkId)
    if (audioCtx.setSinkId) {
      devices.filter(d => d.kind === 'audiooutput').forEach(d => {
        const opt = document.createElement('option');
        opt.value = d.deviceId;
        opt.textContent = d.label || `Salida ${outSelect.options.length}`;
        outSelect.appendChild(opt);
      });
    } else {
      const opt = document.createElement('option');
      opt.value = "none";
      opt.textContent = "🔊 Salida predeterminada (Navegador)";
      outSelect.appendChild(opt);
    }
  } catch(e) {
    console.warn("Permiso de audio denegado o no soportado:", e);
    alert("Se requiere permiso de micrófono para reconocer dispositivos de audio.");
  }
}

async function changeInput() {
  if (!audioCtx) return;
  const deviceId = document.getElementById('inputSelect').value;
  if (inputStream) inputStream.getTracks().forEach(t => t.stop());
  
  try {
    inputStream = await navigator.mediaDevices.getUserMedia({
      audio: deviceId ? { deviceId: { exact: deviceId }, echoCancellation: false, noiseSuppression: false } : true
    });
    if (sourceNode) sourceNode.disconnect();
    sourceNode = audioCtx.createMediaStreamSource(inputStream);
    buildAudioChain();
  } catch(e) {
    alert("No se pudo acceder al dispositivo de entrada.");
  }
}

async function changeOutput(deviceId) {
  if (!deviceId || deviceId === "none") return;
  if (audioCtx.setSinkId) {
    try { await audioCtx.setSinkId(deviceId); } catch(e) { console.warn("setSinkId falló:", e); }
  }
}

// ==================== UI & CONTROLS ====================
function toggleModule(mod) {
  modules[mod] = !modules[mod];
  const btn = document.getElementById(mod === 'mbComp' ? 'mbCompToggle' : mod === 'geq' ? 'geqToggle' : mod === 'dualComp' ? 'dualCompToggle' : mod === 'bass' ? 'bassToggle' : 'stereoToggle');
  btn.classList.toggle('on', modules[mod]);
  if (sourceNode) buildAudioChain();
}

function renderParamEqUI() {
  const container = document.getElementById('paramEqBands');
  container.innerHTML = '';
  const labels = ['Low', 'Low-Mid', 'Mid', 'Presence', 'Air'];
  paramEqNodes.forEach((node, i) => {
    container.innerHTML += `
      <div class="eq-band">
        <div style="font-size:10px;color:var(--accent-green);margin-bottom:4px;">${labels[i]}</div>
        <div class="slider-container"><span style="font-size:9px;color:var(--text-dim);">Gain</span>
          <input type="range" class="slider-horizontal" min="-12" max="12" value="0" step="0.5" oninput="updateParamEq(${i},'gain',this.value)" style="width:80px;">
          <span class="slider-value" id="paramEqGain${i}">0dB</span></div>
        <div class="slider-container" style="margin-top:5px;"><span style="font-size:9px;color:var(--text-dim);">Freq</span>
          <input type="range" class="slider-horizontal" min="${node.type==='lowshelf'?20:node.type==='highshelf'?2000:50}" max="${node.type==='lowshelf'?500:node.type==='highshelf'?16000:16000}" value="${node.frequency.value}" step="1" oninput="updateParamEq(${i},'freq',this.value)" style="width:80px;">
          <span class="slider-value" id="paramEqFreq${i}">${node.frequency.value}Hz</span></div>
        <div class="slider-container" style="margin-top:5px;"><span style="font-size:9px;color:var(--text-dim);">Q</span>
          <input type="range" class="slider-horizontal" min="0.1" max="10" value="${node.Q.value}" step="0.1" oninput="updateParamEq(${i},'q',this.value)" style="width:80px;">
          <span class="slider-value" id="paramEqQ${i}">${node.Q.value}</span></div>
      </div>`;
  });
}

function updateParamEq(i, p, v) {
  v = parseFloat(v);
  if(p==='gain'){ paramEqNodes[i].gain.value=v; document.getElementById(`paramEqGain${i}`).textContent=v+'dB'; }
  else if(p==='freq'){ paramEqNodes[i].frequency.value=v; document.getElementById(`paramEqFreq${i}`).textContent=v+'Hz'; }
  else if(p==='q'){ paramEqNodes[i].Q.value=v; document.getElementById(`paramEqQ${i}`).textContent=v; }
}

function loadEqPreset(name, btn) {
  document.querySelectorAll('.preset-btn').forEach(b=>b.classList.remove('active'));
  if(btn) btn.classList.add('active');
  const p = {
    flat: [[0,0,0],[0,0,0],[0,0,0],[0,0,0],[0,0,0]],
    bassBoost: [[6,0,0],[2,0,0],[0,0,0],[1,0,0],[2,0,0]],
    vocalClarity: [[-3,0,0],[-2,0,0],[4,1.5,0],[5,1,0],[3,0,0]],
    radio: [[4,0,0],[1,0,0],[3,1.5,0],[4,1,0],[3,0,0]]
  }[name] || [[0,0,0],[0,0,0],[0,0,0],[0,0,0],[0,0,0]];
  p.forEach((v,i)=>updateParamEq(i,'gain',v[0]));
}

function renderMbCompUI(settings) {
  const container = document.getElementById('mbCompBands');
  const names = ['Sub','Bass','Mid','Presence','Air'];
  const classes = ['sub','bass','mid','presence','air'];
  settings.forEach((s, i) => {
    container.innerHTML += `
      <div class="mb-band">
        <div class="mb-band-header"><span class="mb-band-name ${classes[i]}">${names[i]}</span><span style="font-size:9px;color:var(--text-dim);">${s.range}Hz</span></div>
        <div class="mb-band-params">
          <div class="mb-param"><label>Threshold</label><input type="range" min="-60" max="0" value="${s.th}" oninput="mbCompNodes[${i}].comp.threshold.value=parseFloat(this.value);this.nextElementSibling.textContent=this.value+'dB'"><span class="param-value">${s.th} dB</span></div>
          <div class="mb-param"><label>Ratio</label><input type="range" min="1" max="20" value="${s.r}" step="0.5" oninput="mbCompNodes[${i}].comp.ratio.value=parseFloat(this.value);this.nextElementSibling.textContent=this.value+':1'"><span class="param-value">${s.r}:1</span></div>
          <div class="mb-param"><label>Attack</label><input type="range" min="0.1" max="100" value="${s.at*1000}" step="0.1" oninput="mbCompNodes[${i}].comp.attack.value=parseFloat(this.value)/1000;this.nextElementSibling.textContent=this.value+'ms'"><span class="param-value">${(s.at*1000).toFixed(0)} ms</span></div>
          <div class="mb-param"><label>Makeup</label><input type="range" min="0" max="18" value="${s.mk}" oninput="mbCompNodes[${i}].mk.gain.value=dbToGain(parseFloat(this.value));this.nextElementSibling.textContent=this.value+'dB'"><span class="param-value">${s.mk} dB</span></div>
        </div>
      </div>`;
  });
}

function renderGeqUI() {
  const container = document.getElementById('geqBands');
  container.innerHTML = '';
  geqNodes.forEach((node, i) => {
    container.innerHTML += `
      <div class="geq-band">
        <input type="range" class="slider-vertical geq-slider" min="-12" max="12" value="0" step="0.5" oninput="geqNodes[${i}].gain.value=parseFloat(this.value);this.nextElementSibling.textContent=this.value">
        <span style="font-size:9px;color:var(--accent-green);font-family:'Courier New',monospace;">0</span>
        <span class="geq-freq">${node.frequency.value >= 1000 ? (node.frequency.value/1000)+'k' : node.frequency.value}</span>
      </div>`;
  });
}

function resetGeq() { geqNodes.forEach((n,i)=>{ n.gain.value=0; document.querySelectorAll('.geq-slider')[i].value=0; document.querySelectorAll('.geq-band span')[i*2].textContent='0'; }); }

function updateDualComp(band, param, val) {
  val = parseFloat(val);
  const comp = band==='low' ? dualLowComp : dualHighComp;
  const gain = band==='low' ? dualLowGain : dualHighGain;
  if(param==='thresh'){ comp.threshold.value=val; document.getElementById(`dual${band==='low'?'Low':'High'}ThreshVal`).textContent=val+' dB'; }
  else if(param==='ratio'){ comp.ratio.value=val; document.getElementById(`dual${band==='low'?'Low':'High'}RatioVal`).textContent=val+':1'; }
  else if(param==='attack'){ comp.attack.value=val/1000; document.getElementById(`dual${band==='low'?'Low':'High'}AttackVal`).textContent=val+' ms'; }
  else if(param==='makeup'){ gain.gain.value=dbToGain(val); document.getElementById(`dual${band==='low'?'Low':'High'}MakeupVal`).textContent=val+' dB'; }
}

function updateLimiter(param, val) {
  val = parseFloat(val);
  if(param==='thresh'){ limiterNode.threshold.value=val; document.getElementById('limiterThreshVal').textContent=val+' dB'; }
  else if(param==='ratio'){ limiterNode.ratio.value=val; document.getElementById('limiterRatioVal').textContent=val+':1'; }
  else if(param==='release'){ limiterNode.release.value=val/1000; document.getElementById('limiterReleaseVal').textContent=val+' ms'; }
  else if(param==='ceiling'){ outputGainNode.gain.value=dbToGain(val); document.getElementById('limiterCeilingVal').textContent=val+' dB'; }
}

// ==================== KNOB LOGIC ====================
function initKnobs() {
  document.querySelectorAll('.knob-container').forEach(k => updateKnobDisplay(k, parseFloat(k.dataset.value)));
  
  let activeKnob = null;
  document.addEventListener('mousedown', e => {
    const k = e.target.closest('.knob-container');
    if(!k) return;
    activeKnob = k;
    activeKnob._startY = e.clientY;
    activeKnob._startVal = parseFloat(k.dataset.value);
  });
  document.addEventListener('mouseup', () => activeKnob = null);
  document.addEventListener('mousemove', e => {
    if(!activeKnob) return;
    const delta = (activeKnob._startY - e.clientY) * 0.5;
    const min = parseFloat(activeKnob.dataset.min), max = parseFloat(activeKnob.dataset.max);
    let val = activeKnob._startVal + (delta / 100) * (max - min);
    val = Math.max(min, Math.min(max, val));
    activeKnob.dataset.value = val;
    updateKnobDisplay(activeKnob, val);
    applyKnobValue(activeKnob, val);
  });
}

function updateKnobDisplay(k, val) {
  const min = parseFloat(k.dataset.min), max = parseFloat(k.dataset.max);
  const norm = (val - min) / (max - min);
  const offset = 113 - (norm * 113 * 0.75 + 113 * 0.125);
  k.querySelector('.knob-fill').style.strokeDashoffset = offset;
  const angle = -90 + (norm * 270 - 135);
  const rad = (angle * Math.PI) / 180;
  const ind = k.querySelector('.knob-indicator');
  if(ind){ ind.style.left = (50 + 40 * Math.cos(rad)) + '%'; ind.style.top = (50 + 40 * Math.sin(rad)) + '%'; ind.style.transform = 'translate(-50%, -50%)'; }
  const unit = k.dataset.unit || '';
  const disp = k.querySelector('.knob-value');
  if(disp) {
    if(unit==='%') disp.textContent = val.toFixed(0)+'%';
    else if(unit==='Hz') disp.textContent = val.toFixed(0)+' Hz';
    else if(unit==='dB') disp.textContent = val.toFixed(1)+' dB';
    else disp.textContent = val.toFixed(1);
  }
}

function applyKnobValue(k, val) {
  const p = k.dataset.param; val = parseFloat(val);
  switch(p) {
    case 'inputGain': if(inputGainNode) inputGainNode.gain.value=dbToGain(val); break;
    case 'bassFreq': if(bassNode) bassNode.frequency.value=val; break;
    case 'bassGain': if(bassNode && modules.bass) bassNode.gain.value=val; break;
    case 'stereoWidth': { const w=val/100; if(widthNodeL) widthNodeL.gain.value=w; if(widthNodeR) widthNodeR.gain.value=w; break; }
    case 'midLevel': if(midGainNode) midGainNode.gain.value=dbToGain(val); break;
    case 'sideLevel': if(sideGainNode) sideGainNode.gain.value=dbToGain(val); break;
    case 'outputVol': if(outputGainNode) outputGainNode.gain.value=dbToGain(val); break;
  }
}

// ==================== METERS & ANALYZERS ====================
function startMeters() {
  const inC = document.getElementById('inputAnalyzer');
  const outC = document.getElementById('outputAnalyzer');
  if(inC){ inC.width=inC.offsetWidth*2; inC.height=inC.offsetHeight*2; }
  if(outC){ outC.width=outC.offsetWidth*2; outC.height=outC.offsetHeight*2; }

  function animate() {
    animFrameId = requestAnimationFrame(animate);
    if(!audioCtx || audioCtx.state!=='running') return;
    
    drawWaveform(inC, analyserInput, '#00ff88');
    drawWaveform(outC, analyserOutput, '#00ccff');
    
    const inData = new Uint8Array(analyserInput.frequencyBinCount);
    analyserInput.getByteFrequencyData(inData);
    updateMeter('inputMeterL', calculateLevel(inData)-3); updateMeter('inputMeterR', calculateLevel(inData)-3);
    
    const outData = new Uint8Array(analyserOutput.frequencyBinCount);
    analyserOutput.getByteFrequencyData(outData);
    const level = calculateLevel(outData);
    updateMeter('outputMeterL', level); updateMeter('outputMeterR', level-1);
    
    const lufs = -60 + (level/255)*60;
    document.getElementById('loudnessDisplay').innerHTML = lufs.toFixed(1)+'<span class="loudness-unit">LUFS</span>';
    document.getElementById('truePeakDisplay').innerHTML = (-60 + (Math.max(...outData)/255)*60).toFixed(1)+'<span class="loudness-unit">dBTP</span>';
  }
  animate();
}

function calculateLevel(d) { return d ? Array.from(d).reduce((a,b)=>a+b,0)/d.length : 0; }
function updateMeter(id, lvl) {
  const el = document.getElementById(id);
  if(!el) return;
  const pct = Math.min(100, Math.max(0, (lvl/255)*100));
  el.style.height = pct+'%';
  el.style.background = pct>85?'var(--meter-red)':pct>65?'var(--meter-yellow)':'var(--meter-green)';
  el.style.boxShadow = `0 0 8px ${pct>85?'rgba(255,51,51,0.5)':pct>65?'rgba(255,204,0,0.5)':'rgba(0,255,102,0.3)'}`;
}

function drawWaveform(canvas, analyser, color) {
  if(!canvas || !analyser) return;
  const ctx = canvas.getContext('2d');
  const w = canvas.width, h = canvas.height;
  ctx.clearRect(0,0,w,h);
  ctx.strokeStyle = 'rgba(255,255,255,0.03)'; ctx.lineWidth=1;
  for(let i=0;i<10;i++){ ctx.beginPath(); ctx.moveTo(0,h/10*i); ctx.lineTo(w,h/10*i); ctx.stroke(); }
  ctx.strokeStyle='rgba(255,255,255,0.08)'; ctx.beginPath(); ctx.moveTo(0,h/2); ctx.lineTo(w,h/2); ctx.stroke();
  
  const data = new Uint8Array(analyser.frequencyBinCount);
  analyser.getByteTimeDomainData(data);
  ctx.lineWidth=2; ctx.strokeStyle=color; ctx.shadowColor=color; ctx.shadowBlur=5; ctx.beginPath();
  let x=0; const slice=w/data.length;
  for(let i=0;i<data.length;i++){ const v=data[i]/128; const y=(v*h)/2; i===0?ctx.moveTo(x,y):ctx.lineTo(x,y); x+=slice; }
  ctx.stroke(); ctx.shadowBlur=0;
}

function dbToGain(db) { return Math.pow(10, db/20); }

window.addEventListener('load', () => refreshDevices());
window.addEventListener('resize', () => {
  const inC=document.getElementById('inputAnalyzer'), outC=document.getElementById('outputAnalyzer');
  if(inC){ inC.width=inC.offsetWidth*2; inC.height=inC.offsetHeight*2; }
  if(outC){ outC.width=outC.offsetWidth*2; outC.height=outC.offsetHeight*2; }
});
</script>
</body>
</html>
