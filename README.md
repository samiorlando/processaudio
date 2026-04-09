<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BROADCAST PRO™ Audio Processor v3.0</title>
  <style>
    :root {
      --bg: #0b0d12;
      --panel: #14171f;
      --panel-hover: #1a1e27;
      --border: #2a2f3a;
      --text: #e6e9ef;
      --text-dim: #8b92a5;
      --accent: #00d4ff;
      --accent-glow: rgba(0, 212, 255, 0.25);
      --success: #22c55e;
      --warning: #f59e0b;
      --danger: #ef4444;
      --radius: 8px;
      --font: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
      --font-mono: 'JetBrains Mono', 'Fira Code', ui-monospace, monospace;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font);
      line-height: 1.5;
      padding: 20px;
      min-height: 100vh;
    }

    .broadcast-pro { max-width: 1280px; margin: 0 auto; }
    
    /* HEADER */
    .app-header {
      display: flex; justify-content: space-between; align-items: center;
      margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid var(--border);
      flex-wrap: wrap; gap: 10px;
    }
    .app-header h1 { font-size: 1.25rem; letter-spacing: 0.5px; }
    .status-bar { display: flex; gap: 16px; align-items: center; font-family: var(--font-mono); font-size: 0.85rem; color: var(--text-dim); flex-wrap: wrap; }
    .on-air {
      background: var(--danger); color: #fff; padding: 2px 8px; border-radius: 4px;
      font-weight: 600; font-size: 0.75rem; animation: pulse 1.8s infinite;
    }
    @keyframes pulse { 0%, 100% { opacity: 1; box-shadow: 0 0 6px var(--danger); } 50% { opacity: 0.7; box-shadow: none; } }

    /* GRID DE PANELES */
    .control-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 16px;
      margin-bottom: 20px;
    }
    .panel {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
      transition: background 0.2s, border-color 0.2s;
    }
    .panel:hover { background: var(--panel-hover); border-color: #3a4150; }
    .panel h2 {
      font-size: 0.9rem; color: var(--accent); margin-bottom: 12px;
      text-transform: uppercase; letter-spacing: 0.8px;
      border-bottom: 1px solid var(--border); padding-bottom: 8px;
    }

    /* CONTROLES & SLIDERS */
    .row, .slider-group { display: flex; flex-direction: column; gap: 10px; margin-bottom: 12px; }
    .slider-group { flex-direction: row; flex-wrap: wrap; gap: 12px; }
    label { display: flex; align-items: center; gap: 6px; font-size: 0.85rem; color: var(--text-dim); }
    .val { font-family: var(--font-mono); color: var(--text); min-width: 52px; text-align: right; font-size: 0.8rem; }
    
    input[type="range"] {
      -webkit-appearance: none; width: 100%; height: 6px; background: #252a35;
      border-radius: 3px; outline: none; cursor: pointer;
    }
    input[type="range"]::-webkit-slider-thumb {
      -webkit-appearance: none; width: 16px; height: 16px; background: var(--accent);
      border-radius: 50%; cursor: pointer; box-shadow: 0 0 6px var(--accent-glow); transition: transform 0.15s;
    }
    input[type="range"]::-webkit-slider-thumb:hover { transform: scale(1.15); }
    input[type="range"]::-moz-range-thumb {
      width: 16px; height: 16px; background: var(--accent); border: none; border-radius: 50%; cursor: pointer;
    }

    /* BOTONES & TRANSPORT */
    .transport { display: flex; gap: 8px; margin-top: 4px; }
    button, .preset {
      background: #252a35; color: var(--text); border: 1px solid var(--border);
      padding: 8px 14px; border-radius: 6px; cursor: pointer; font-size: 0.8rem;
      transition: all 0.2s; font-weight: 500;
    }
    button:hover, .preset:hover { background: var(--accent-glow); border-color: var(--accent); color: var(--accent); }
    #btn-start { background: var(--success); color: #000; border-color: var(--success); }
    #btn-stop { background: var(--danger); color: #fff; border-color: var(--danger); }
    .presets { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 8px; }

    /* METROS VERTICALES */
    .meters { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 12px; }
    .meter { display: flex; flex-direction: column; align-items: center; font-size: 0.75rem; }
    .meter-bar {
      width: 100%; height: 90px; background: #11141a; border-radius: 4px;
      position: relative; overflow: hidden; border: 1px solid var(--border);
    }
    .meter-bar::before {
      content: ''; position: absolute; bottom: 0; left: 0; width: 100%;
      height: calc(var(--level, 0) * 1%);
      background: linear-gradient(to top, var(--success) 0%, var(--warning) 65%, var(--danger) 95%);
      transition: height 0.12s ease-out;
    }
    .meter-val { position: absolute; bottom: 2px; width: 100%; text-align: center; color: #fff; font-size: 0.65rem; text-shadow: 0 1px 2px #000; }

    /* ESPECTRO */
    .spectrum-container { margin-bottom: 12px; }
    .spectrum-bars { display: flex; justify-content: space-between; align-items: flex-end; height: 85px; padding: 4px 0; }
    .bar { width: 12%; background: var(--accent); border-radius: 2px 2px 0 0; height: 15%; transition: height 0.15s ease; opacity: 0.85; }
    .freq-labels { display: flex; justify-content: space-between; font-size: 0.65rem; color: var(--text-dim); margin-top: 4px; }

    /* CHECKBOXES & RADIO */
    .checkbox-label, label:has(input[type="checkbox"]) { display: flex; align-items: center; gap: 6px; font-size: 0.82rem; cursor: pointer; }
    input[type="checkbox"], input[type="radio"] { accent-color: var(--accent); transform: scale(1.1); cursor: pointer; }
    .broadcast-mode { display: flex; flex-wrap: wrap; gap: 14px; align-items: center; margin-top: 10px; }
    .live-indicator { color: var(--danger); font-weight: 600; animation: pulse 1.5s infinite; margin-right: 6px; }

    /* FOOTER */
    .app-footer {
      display: flex; justify-content: space-between; flex-wrap: wrap; gap: 12px;
      padding: 16px 0; border-top: 1px solid var(--border); font-size: 0.8rem;
      color: var(--text-dim); font-family: var(--font-mono);
    }

    /* RESPONSIVE */
    @media (max-width: 768px) {
      .control-grid { grid-template-columns: 1fr; }
      .app-header { flex-direction: column; align-items: flex-start; }
      .meters { grid-template-columns: repeat(2, 1fr); }
    }
  </style>
</head>
<body>
  <div class="broadcast-pro">
    <header class="app-header">
      <h1>BROADCAST PRO™ Audio Processor v3.0</h1>
      <div class="status-bar">
        <span>DSP Engine</span>
        <span>Latency: <span id="latency">--</span>ms</span>
        <span>SR: <span id="sr">--</span> Hz</span>
        <span>CPU: <span id="cpu">--</span>%</span>
        <span class="on-air">ON AIR</span>
      </div>
    </header>

    <main class="control-grid">
      <!-- I/O -->
      <section class="panel">
        <h2>Audio In / Output</h2>
        <label>Input Select: 
          <select id="input-device" style="background:#1e222b;color:#fff;border:1px solid var(--border);padding:4px;border-radius:4px;"><option>Default Input</option></select>
        </label>
        <label>Output Device: 
          <select id="output-device" style="background:#1e222b;color:#fff;border:1px solid var(--border);padding:4px;border-radius:4px;"><option>Default Output</option></select>
        </label>
        <div class="slider-group">
          <label>Input Gain: <input type="range" min="-60" max="24" value="0" step="0.1"><span class="val">0.0 dB</span></label>
          <div class="transport">
            <button id="btn-start">▶ START</button>
            <button id="btn-stop">■ STOP</button>
          </div>
        </div>
      </section>

      <!-- AGC -->
      <section class="panel">
        <h2>AGC / Loudness</h2>
        <div class="slider-group">
          <label>Integrated Target: <span class="val">-9 LUFS</span></label>
          <label>Threshold: <input type="range" value="-18"><span class="val">-18 dB</span></label>
          <label>Attack: <input type="range" value="20"><span class="val">20 ms</span></label>
          <label>Release: <input type="range" value="300"><span class="val">300 ms</span></label>
          <label>Max Gain: <input type="range" value="18"><span class="val">18 dB</span></label>
        </div>
      </section>

      <!-- Voice Detect -->
      <section class="panel">
        <h2>Voice Detect 🎤 <span style="color:#888;font-size:0.8rem;margin-left:6px;">No Signal</span></h2>
        <div class="slider-group">
          <label>Gate Thresh: <input type="range" value="-36"><span class="val">-36 dB</span></label>
          <label>Spectral Ctr: <input type="range" value="1800"><span class="val">1800 Hz</span></label>
          <label>Hold Time: <input type="range" value="800"><span class="val">800 ms</span></label>
        </div>
      </section>

      <!-- Multiband -->
      <section class="panel">
        <h2>Adaptive Multiband Compressor (6 Bands)</h2>
        <div class="slider-group">
          <div style="text-align:center"><label>SUB</label><input type="range" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>BASS</label><input type="range" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>L-MID</label><input type="range" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>MID</label><input type="range" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>H-MID</label><input type="range" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>HIGH</label><input type="range" value="0"><span class="val">0.0dB</span></div>
        </div>
        <div class="presets">
          <span style="font-size:0.8rem;color:var(--text-dim);">Presets:</span>
          <button class="preset">BROADCAST</button>
          <button class="preset">WARM</button>
          <button class="preset">BRIGHT</button>
          <button class="preset">PUNCHY</button>
          <button class="preset">FLAT</button>
        </div>
      </section>

      <!-- Limiter & Clipper -->
      <section class="panel">
        <h2>Look-Ahead Limiter & True Peak Clipper</h2>
        <div class="slider-group">
          <label>Ceiling: <input type="range" value="-0.3"><span class="val">-0.3 dB</span></label>
          <label>Look-Ahead: <input type="range" value="3.0"><span class="val">3.0 ms</span></label>
          <label>Release: <input type="range" value="150"><span class="val">150 ms</span></label>
        </div>
        <div class="slider-group">
          <label>Clip Thresh: <input type="range" value="-0.1"><span class="val">-0.1 dB</span></label>
          <label>Soft Clip: <input type="range" value="60"><span class="val">60%</span></label>
        </div>
      </section>

      <!-- Meters & Output -->
      <section class="panel">
        <h2>Output Meters</h2>
        <div class="meters">
          <div class="meter"><span>L</span><div class="meter-bar" style="--level: 12"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>R</span><div class="meter-bar" style="--level: 10"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>M</span><div class="meter-bar" style="--level: 5"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>S</span><div class="meter-bar" style="--level: 8"><span class="meter-val">-∞</span></div></div>
        </div>
        <div class="slider-group">
          <label>Master Out: <input type="range" value="0"><span class="val">0.0 dB</span></label>
          <label>Width: <input type="range" value="100"><span class="val">100%</span></label>
        </div>
      </section>

      <!-- FM & RDS -->
      <section class="panel">
        <h2>FM Pre-Emphasis & MPX Encoder</h2>
        <div style="margin-bottom:10px;display:flex;gap:10px;flex-wrap:wrap;">
          <label><input type="radio" name="pre" value="50"> 50µs</label>
          <label><input type="radio" name="pre" value="75"> 75µs</label>
          <label><input type="radio" name="pre" value="off" checked> OFF</label>
        </div>
        <div class="slider-group">
          <label>Pre-Gain: <input type="range" value="6.0"><span class="val">6.0 dB</span></label>
          <label>Pilot: <input type="range" value="9.0"><span class="val">9.0%</span></label>
          <label>Separation: <input type="range" value="95"><span class="val">95%</span></label>
          <label>RDS Level: <input type="range" value="5.0"><span class="val">5.0%</span></label>
        </div>
        <label class="checkbox-label"><input type="checkbox"> Enable RDS (57kHz)</label>
      </section>

      <!-- Spectrum & Mode -->
      <section class="panel">
        <h2>Real-Time Spectrum & Broadcast Mode</h2>
        <div class="spectrum-container">
          <div class="spectrum-bars">
            <div class="bar" style="height:15%"></div><div class="bar" style="height:40%"></div>
            <div class="bar" style="height:70%"></div><div class="bar" style="height:55%"></div>
            <div class="bar" style="height:30%"></div><div class="bar" style="height:20%"></div>
          </div>
          <div class="freq-labels">
            <span>20Hz</span><span>100Hz</span><span>1kHz</span><span>5kHz</span><span>10kHz</span><span>20kHz</span>
          </div>
        </div>
        <div class="broadcast-mode">
          <span class="live-indicator">● LIVE</span>
          <label><input type="checkbox" checked> Clipping Protection</label>
          <label><input type="checkbox" checked> Auto Normalization</label>
          <label><input type="checkbox"> Safe Mode</label>
        </div>
      </section>
    </main>

    <footer class="app-footer">
      <span>BROADCAST PRO™ Audio Processor v3.0 | Web Audio API</span>
      <span>DSP Buffer: <span id="buffer">--</span></span>
      <span>© 2026 Professional Audio Systems</span>
    </footer>
  </div>
</body>
</html>
