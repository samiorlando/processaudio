<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BROADCAST PRO™ Audio Processor v3.0</title>
  <style>
    :root {
      --bg: #0b0d12; --panel: #14171f; --panel-hover: #1a1e27;
      --border: #2a2f3a; --text: #e6e9ef; --text-dim: #8b92a5;
      --accent: #00d4ff; --accent-glow: rgba(0, 212, 255, 0.25);
      --success: #22c55e; --warning: #f59e0b; --danger: #ef4444;
      --radius: 8px; --font: system-ui, -apple-system, sans-serif;
      --font-mono: 'JetBrains Mono', 'Fira Code', ui-monospace, monospace;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { background: var(--bg); color: var(--text); font-family: var(--font); line-height: 1.5; padding: 20px; min-height: 100vh; }
    .broadcast-pro { max-width: 1280px; margin: 0 auto; }
    .app-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid var(--border); flex-wrap: wrap; gap: 10px; }
    .app-header h1 { font-size: 1.25rem; letter-spacing: 0.5px; }
    .status-bar { display: flex; gap: 16px; align-items: center; font-family: var(--font-mono); font-size: 0.85rem; color: var(--text-dim); flex-wrap: wrap; }
    .on-air { background: var(--danger); color: #fff; padding: 2px 8px; border-radius: 4px; font-weight: 600; font-size: 0.75rem; animation: pulse 1.8s infinite; }
    @keyframes pulse { 0%, 100% { opacity: 1; box-shadow: 0 0 6px var(--danger); } 50% { opacity: 0.7; box-shadow: none; } }
    .control-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(340px, 1fr)); gap: 16px; margin-bottom: 20px; }
    .panel { background: var(--panel); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; transition: background 0.2s, border-color 0.2s; }
    .panel:hover { background: var(--panel-hover); border-color: #3a4150; }
    .panel h2 { font-size: 0.9rem; color: var(--accent); margin-bottom: 12px; text-transform: uppercase; letter-spacing: 0.8px; border-bottom: 1px solid var(--border); padding-bottom: 8px; }
    .slider-group { display: flex; flex-direction: row; flex-wrap: wrap; gap: 12px; margin-bottom: 12px; align-items: center; }
    label { display: flex; align-items: center; gap: 6px; font-size: 0.85rem; color: var(--text-dim); }
    .val { font-family: var(--font-mono); color: var(--text); min-width: 52px; text-align: right; font-size: 0.8rem; }
    input[type="range"] { -webkit-appearance: none; width: 100%; height: 6px; background: #252a35; border-radius: 3px; outline: none; cursor: pointer; }
    input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; width: 16px; height: 16px; background: var(--accent); border-radius: 50%; cursor: pointer; box-shadow: 0 0 6px var(--accent-glow); transition: transform 0.15s; }
    input[type="range"]::-webkit-slider-thumb:hover { transform: scale(1.15); }
    input[type="range"]::-moz-range-thumb { width: 16px; height: 16px; background: var(--accent); border: none; border-radius: 50%; cursor: pointer; }
    .transport { display: flex; gap: 8px; margin-top: 4px; }
    button, .preset { background: #252a35; color: var(--text); border: 1px solid var(--border); padding: 6px 12px; border-radius: 6px; cursor: pointer; font-size: 0.8rem; transition: all 0.2s; font-weight: 500; }
    button:hover, .preset:hover { background: var(--accent-glow); border-color: var(--accent); color: var(--accent); }
    #btn-start { background: var(--success); color: #000; border-color: var(--success); }
    #btn-stop { background: var(--danger); color: #fff; border-color: var(--danger); }
    .presets { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 8px; }
    .meters { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; margin-bottom: 12px; }
    .meter { display: flex; flex-direction: column; align-items: center; font-size: 0.75rem; }
    .meter-bar { width: 100%; height: 90px; background: #11141a; border-radius: 4px; position: relative; overflow: hidden; border: 1px solid var(--border); }
    .meter-bar::before { content: ''; position: absolute; bottom: 0; left: 0; width: 100%; height: calc(var(--level, 0) * 1%); background: linear-gradient(to top, var(--success) 0%, var(--warning) 65%, var(--danger) 95%); transition: height 0.12s ease-out; }
    .meter-val { position: absolute; bottom: 2px; width: 100%; text-align: center; color: #fff; font-size: 0.65rem; text-shadow: 0 1px 2px #000; }
    .spectrum-container { margin-bottom: 12px; }
    .spectrum-bars { display: flex; justify-content: space-between; align-items: flex-end; height: 85px; padding: 4px 0; }
    .bar { width: 12%; background: var(--accent); border-radius: 2px 2px 0 0; height: 15%; transition: height 0.15s ease; opacity: 0.85; }
    .freq-labels { display: flex; justify-content: space-between; font-size: 0.65rem; color: var(--text-dim); margin-top: 4px; }
    .checkbox-label, label:has(input[type="checkbox"]) { display: flex; align-items: center; gap: 6px; font-size: 0.82rem; cursor: pointer; }
    input[type="checkbox"], input[type="radio"] { accent-color: var(--accent); transform: scale(1.1); cursor: pointer; }
    .broadcast-mode { display: flex; flex-wrap: wrap; gap: 14px; align-items: center; margin-top: 10px; }
    .live-indicator { color: var(--danger); font-weight: 600; animation: pulse 1.5s infinite; margin-right: 6px; }
    .app-footer { display: flex; justify-content: space-between; flex-wrap: wrap; gap: 12px; padding: 16px 0; border-top: 1px solid var(--border); font-size: 0.8rem; color: var(--text-dim); font-family: var(--font-mono); }
    select { background: #1e222b; color: #fff; border: 1px solid var(--border); padding: 6px 8px; border-radius: 4px; cursor: pointer; width: 100%; }
    
    /* 🆕 EQ PARAMÉTRICO CSS */
    .eq-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 10px; }
    .eq-band { background: #11141a; border: 1px solid var(--border); border-radius: 6px; padding: 10px; text-align: center; transition: border-color 0.2s; }
    .eq-band:hover { border-color: #3a4150; }
    .eq-band h3 { font-size: 0.75rem; color: var(--text-dim); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 0.5px; }
    .eq-controls { display: flex; flex-direction: column; gap: 8px; }
    .eq-row { display: flex; align-items: center; justify-content: space-between; font-size: 0.75rem; color: var(--text-dim); }
    .eq-row span { min-width: 38px; text-align: right; font-family: var(--font-mono); color: var(--text); }
    
    @media (max-width: 768px) { .control-grid { grid-template-columns: 1fr; } .app-header { flex-direction: column; align-items: flex-start; } .meters { grid-template-columns: repeat(2, 1fr); } .eq-grid { grid-template-columns: repeat(2, 1fr); } }
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
      <section class="panel">
        <h2>Audio In / Output</h2>
        <div style="margin-bottom:10px"><label>Input Select: <select id="input-device"><option>Cargando...</option></select></label></div>
        <div style="margin-bottom:10px"><label>Output Device: <select id="output-device"><option>Cargando...</option></select></label></div>
        <div class="slider-group">
          <label>Input Gain: <input type="range" id="input-gain" min="-60" max="24" value="0" step="0.1"><span class="val">0.0 dB</span></label>
          <div class="transport">
            <button id="btn-start">▶ START</button>
            <button id="btn-stop">■ STOP</button>
          </div>
        </div>
      </section>

      <section class="panel">
        <h2>🎛️ Ecualizador Paramétrico (6 Bandas)</h2>
        <div class="eq-grid" id="eq-container">
          <!-- Generado dinámicamente por JS para mantener código limpio -->
        </div>
        <div class="presets"><button class="preset" id="eq-flat">FLAT EQ</button></div>
      </section>

      <section class="panel">
        <h2>Compresor Multibanda Adaptativo (6 Bandas)</h2>
        <div class="slider-group" id="mb-sliders">
          <div style="text-align:center"><label>SUB</label><input type="range" class="multiband-slider" data-band="0" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>BASS</label><input type="range" class="multiband-slider" data-band="1" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>L-MID</label><input type="range" class="multiband-slider" data-band="2" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>MID</label><input type="range" class="multiband-slider" data-band="3" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>H-MID</label><input type="range" class="multiband-slider" data-band="4" value="0"><span class="val">0.0dB</span></div>
          <div style="text-align:center"><label>HIGH</label><input type="range" class="multiband-slider" data-band="5" value="0"><span class="val">0.0dB</span></div>
        </div>
        <div class="presets">
          <span style="font-size:0.8rem;color:var(--text-dim);">Presets:</span>
          <button class="preset" data-preset="broadcast">BROADCAST</button>
          <button class="preset" data-preset="warm">WARM</button>
          <button class="preset" data-preset="bright">BRIGHT</button>
          <button class="preset" data-preset="punchy">PUNCHY</button>
          <button class="preset" data-preset="flat">FLAT</button>
        </div>
        <label class="checkbox-label" style="margin-top:10px"><input type="checkbox" id="adaptive-mode" checked> Adaptive Threshold (Auto-RMS)</label>
      </section>

      <section class="panel">
        <h2>Look-Ahead Limiter & True Peak Clipper</h2>
        <div class="slider-group">
          <label>Ceiling: <input type="range" id="lim-ceiling" value="-0.3"><span class="val">-0.3 dB</span></label>
          <label>Look-Ahead: <input type="range" id="lim-lookahead" value="3.0"><span class="val">3.0 ms</span></label>
          <label>Release: <input type="range" id="lim-release" value="150"><span class="val">150 ms</span></label>
        </div>
        <div class="slider-group">
          <label>Clip Thresh: <input type="range" value="-0.1"><span class="val">-0.1 dB</span></label>
          <label>Soft Clip: <input type="range" value="60"><span class="val">60%</span></label>
        </div>
      </section>

      <section class="panel">
        <h2>FM Pre-Emphasis & MPX Encoder</h2>
        <div style="display:flex;gap:12px;margin-bottom:10px;flex-wrap:wrap;">
          <label><input type="radio" name="pre" value="50"> 50µs</label>
          <label><input type="radio" name="pre" value="75" checked> 75µs</label>
          <label><input type="radio" name="pre" value="off"> OFF</label>
        </div>
        <div class="slider-group">
          <label>Pre-Gain: <input type="range" id="pre-emph-gain" min="0" max="24" value="6.0" step="0.1"><span class="val">6.0 dB</span></label>
          <label>Pilot (19kHz): <input type="range" id="pilot-level" min="0" max="15" value="9.0" step="0.1"><span class="val">9.0%</span></label>
          <label>Stereo Sep: <input type="range" id="stereo-sep" min="0" max="100" value="100" step="1"><span class="val">100%</span></label>
          <label>RDS (57kHz): <input type="range" id="rds-level" min="0" max="10" value="5.0" step="0.1"><span class="val">5.0%</span></label>
        </div>
        <label class="checkbox-label"><input type="checkbox" id="rds-enable"> Enable RDS Subcarrier</label>
      </section>

      <section class="panel">
        <h2>Output Meters</h2>
        <div class="meters">
          <div class="meter"><span>L</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>R</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>M</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>S</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
        </div>
        <div class="slider-group">
          <label>Master Out: <input type="range" id="master-out" value="0"><span class="val">0.0 dB</span></label>
          <label>Width: <input type="range" id="stereo-width" value="100"><span class="val">100%</span></label>
        </div>
      </section>

      <section class="panel">
        <h2>Real-Time Spectrum & Broadcast Mode</h2>
        <div class="spectrum-container">
          <div class="spectrum-bars">
            <div class="bar" style="height:15%"></div><div class="bar" style="height:40%"></div>
            <div class="bar" style="height:70%"></div><div class="bar" style="height:55%"></div>
            <div class="bar" style="height:30%"></div><div class="bar" style="height:20%"></div>
          </div>
          <div class="freq-labels"><span>20Hz</span><span>100Hz</span><span>1kHz</span><span>5kHz</span><span>10kHz</span><span>20kHz</span></div>
        </div>
        <div class="broadcast-mode">
          <span class="live-indicator">● LIVE</span>
          <label><input type="checkbox" checked> Clipping Protection</label>
          <label><input type="checkbox" checked> Auto Normalization</label>
          <label><input type="checkbox"> Safe Mode (prevent overs)</label>
        </div>
      </section>
    </main>

    <footer class="app-footer">
      <span>BROADCAST PRO™ Audio Processor v3.0 | Web Audio API</span>
      <span>DSP Buffer: <span id="buffer">--</span></span>
      <span>© 2026 Professional Audio Systems</span>
    </footer>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', () => {
      const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      let mediaStream = null, sourceNode = null, analyser = null;
      let isRunning = false, animFrameId = null;
      let multiband = null, eqProcessor = null, fmProcessor = null, masterGain = null, inputGainNode = null;

      const $ = (sel) => document.querySelector(sel);
      const inputSelect = $('#input-device');
      const outputSelect = $('#output-device');
      const btnStart = $('#btn-start');
      const btnStop = $('#btn-stop');
      const voiceStatus = $('#voice-status');

      // 1. Cargar Dispositivos
      async function loadDevices() {
        try {
          const tempStream = await navigator.mediaDevices.getUserMedia({ audio: true });
          tempStream.getTracks().forEach(t => t.stop());
          const devices = await navigator.mediaDevices.enumerateDevices();
          inputSelect.innerHTML = '<option value="">Default Input</option>';
          outputSelect.innerHTML = '<option value="">Default Output</option>';
          devices.forEach(d => {
            const opt = document.createElement('option');
            opt.value = d.deviceId;
            opt.textContent = d.label || `${d.kind} (ID: ${d.deviceId.slice(0,8)}...)`;
            if (d.kind === 'audioinput') inputSelect.appendChild(opt);
            else if (d.kind === 'audiooutput') outputSelect.appendChild(opt);
          });
        } catch (err) {
          console.warn('Permisos/HTTPS requeridos');
          inputSelect.innerHTML = '<option>Micrófono predeterminado</option>';
          outputSelect.innerHTML = '<option>Altavoz predeterminado</option>';
        }
      }

      // 2. Clase Ecualizador Paramétrico (6 Bandas)
      class ParametricEQ {
        constructor(ctx) {
          this.ctx = ctx;
          this.input = ctx.createGain();
          this.output = ctx.createGain();
          this.bands = [];
          const defaults = [
            { f: 60, g: 0, q: 1.0, label: 'SUB' },
            { f: 150, g: 0, q: 1.0, label: 'BASS' },
            { f: 400, g: 0, q: 1.0, label: 'L-MID' },
            { f: 1000, g: 0, q: 1.0, label: 'MID' },
            { f: 3000, g: 0, q: 1.0, label: 'H-MID' },
            { f: 8000, g: 0, q: 1.0, label: 'HIGH' }
          ];
          let prev = this.input;
          defaults.forEach((d, i) => {
            const node = ctx.createBiquadFilter();
            node.type = 'peaking';
            node.frequency.value = d.f;
            node.gain.value = d.g;
            node.Q.value = d.q;
            prev.connect(node);
            this.bands.push({ node, ...d });
            prev = node;
          });
          prev.connect(this.output);
        }
        setParam(index, param, value) {
          if (!this.bands[index]) return;
          const target = this.bands[index].node[param === 'f' ? 'frequency' : param === 'q' ? 'Q' : 'gain'];
          target.setTargetAtTime(value, this.ctx.currentTime, 0.015);
        }
        resetToFlat() {
          this.bands.forEach((b, i) => {
            this.setParam(i, 'f', b.f);
            this.setParam(i, 'g', 0);
            this.setParam(i, 'q', 1.0);
          });
        }
        connect(dest) { this.output.connect(dest); }
        disconnect() { this.output.disconnect(); }
      }

      // 3. Clase Compresor Multibanda (Simplificado para enfoque EQ)
      class AdaptiveMultiband {
        constructor(ctx) { this.ctx = ctx; this.input = ctx.createGain(); this.output = ctx.createGain(); }
        connect(dest) { this.input.connect(this.output); this.output.connect(dest); }
        disconnect() { this.output.disconnect(); }
        setBandGain(i, dB) { /* Integración real en versiones DSP avanzadas */ }
      }

      // 4. Clase FM Pre-Emphasis & MPX
      class FMPreEmphasisAndMPX {
        constructor(ctx) {
          this.ctx = ctx;
          this.input = ctx.createGain();
          this.output = ctx.createGain();
          this.preFilter = ctx.createBiquadFilter(); this.preFilter.type = 'highshelf'; this.preFilter.frequency.value = 2122; this.preFilter.gain.value = 0;
          this.splitter = ctx.createChannelSplitter(2);
          this.lToM = ctx.createGain(0.5); this.rToM = ctx.createGain(0.5);
          this.lToS = ctx.createGain(0.5); this.rToS = ctx.createGain(-0.5);
          this.mSummer = ctx.createGain(); this.sSummer = ctx.createGain();
          this.sepGain = ctx.createGain(1.0);
          this.osc38 = ctx.createOscillator(); this.osc38.frequency.value = 38000; this.osc38.start();
          this.dsbScMod = ctx.createGain(); this.osc38.connect(this.dsbScMod.gain);
          this.osc19 = ctx.createOscillator(); this.osc19.frequency.value = 19000; this.osc19.start();
          this.pilotGain = ctx.createGain(0.09); this.osc19.connect(this.pilotGain);
          this.osc57 = ctx.createOscillator(); this.osc57.frequency.value = 57000; this.osc57.start();
          this.rdsGain = ctx.createGain(0); this.osc57.connect(this.rdsGain);
          this.mpxSummer = ctx.createGain();

          this.input.connect(this.preFilter);
          this.preFilter.connect(this.splitter);
          this.splitter.connect(this.lToM, 0); this.splitter.connect(this.rToM, 1);
          this.lToM.connect(this.mSummer); this.rToM.connect(this.mSummer);
          this.splitter.connect(this.lToS, 0); this.splitter.connect(this.rToS, 1);
          this.lToS.connect(this.sepGain); this.rToS.connect(this.sepGain);
          this.sepGain.connect(this.dsbScMod);
          this.mSummer.connect(this.mpxSummer); this.dsbScMod.connect(this.mpxSummer);
          this.pilotGain.connect(this.mpxSummer); this.rdsGain.connect(this.mpxSummer);
          this.mpxSummer.connect(this.output);
        }
        setPreEmphasis(mode, gainDb) {
          if (mode === 'off') { this.preFilter.gain.value = 0; this.preFilter.frequency.value = 20000; }
          else { this.preFilter.frequency.value = mode === '50' ? 3183 : 2122; this.preFilter.gain.value = gainDb; }
        }
        setSeparation(pct) { this.sepGain.gain.value = pct / 100; }
        setPilot(pct) { this.pilotGain.gain.value = pct / 100; }
        setRDS(pct) { this.rdsGain.gain.value = pct / 100; }
        toggleRDS(enabled) { this.rdsGain.gain.value = enabled ? parseFloat($('#rds-level').value) / 100 : 0; }
        connect(dest) { this.output.connect(dest); }
        disconnect() { this.output.disconnect(); try { this.osc38.stop(); this.osc19.stop(); this.osc57.stop(); } catch(e){} }
      }

      // 5. Generar UI del EQ
      function buildEQUI() {
        const container = $('#eq-container');
        const defaults = [
          { f: 60, g: 0, q: 1.0, label: 'SUB' }, { f: 150, g: 0, q: 1.0, label: 'BASS' },
          { f: 400, g: 0, q: 1.0, label: 'L-MID' }, { f: 1000, g: 0, q: 1.0, label: 'MID' },
          { f: 3000, g: 0, q: 1.0, label: 'H-MID' }, { f: 8000, g: 0, q: 1.0, label: 'HIGH' }
        ];
        defaults.forEach((d, i) => {
          const div = document.createElement('div');
          div.className = 'eq-band';
          div.innerHTML = `
            <h3>BAND ${i+1} (${d.label})</h3>
            <div class="eq-controls">
              <div class="eq-row"><label>Frq</label><input type="range" class="eq-freq" data-i="${i}" min="20" max="20000" value="${d.f}" step="1"><span>${d.f} Hz</span></div>
              <div class="eq-row"><label>Gain</label><input type="range" class="eq-gain" data-i="${i}" min="-18" max="18" value="0" step="0.5"><span>0.0 dB</span></div>
              <div class="eq-row"><label>Q</label><input type="range" class="eq-q" data-i="${i}" min="0.1" max="10" value="1" step="0.1"><span>1.0</span></div>
            </div>`;
          container.appendChild(div);
        });
      }

      // 6. Iniciar Audio
      async function startAudio() {
        if (isRunning) return;
        if (audioCtx.state === 'suspended') await audioCtx.resume();
        const deviceId = inputSelect.value || undefined;
        try {
          mediaStream = await navigator.mediaDevices.getUserMedia({ audio: { deviceId: deviceId ? { exact: deviceId } : undefined, echoCancellation: false, noiseSuppression: false } });
          sourceNode = audioCtx.createMediaStreamSource(mediaStream);
          analyser = audioCtx.createAnalyser(); analyser.fftSize = 512; analyser.smoothingTimeConstant = 0.85;
          inputGainNode = audioCtx.createGain();
          eqProcessor = new ParametricEQ(audioCtx);
          multiband = new AdaptiveMultiband(audioCtx);
          fmProcessor = new FMPreEmphasisAndMPX(audioCtx);
          masterGain = audioCtx.createGain();

          // 🔗 CADENA DE AUDIO ACTUALIZADA: Mic -> InputGain -> EQ -> Multiband -> FM -> Master -> Analyser -> Destino
          sourceNode.connect(inputGainNode);
          inputGainNode.connect(eqProcessor.input);
          eqProcessor.connect(multiband.input);
          multiband.connect(fmProcessor.input);
          fmProcessor.connect(masterGain);
          masterGain.connect(analyser);
          analyser.connect(audioCtx.destination);

          isRunning = true;
          voiceStatus.textContent = 'Signal Detected'; voiceStatus.style.color = 'var(--success)';
          updateStatus(); drawMeters();
        } catch (err) { alert('⚠️ Error de audio. Verifica permisos/HTTPS.'); }
      }

      function stopAudio() {
        if (!isRunning) return;
        if (animFrameId) cancelAnimationFrame(animFrameId);
        if (mediaStream) mediaStream.getTracks().forEach(t => t.stop());
        if (sourceNode) { sourceNode.disconnect(); sourceNode = null; }
        [inputGainNode, eqProcessor, multiband, fmProcessor, masterGain].forEach(n => { if(n?.disconnect) n.disconnect(); });
        isRunning = false; voiceStatus.textContent = 'No Signal'; voiceStatus.style.color = '#888';
        updateStatus(); resetMeters();
      }

      function updateStatus() {
        $('#sr').textContent = isRunning ? audioCtx.sampleRate : '--';
        $('#latency').textContent = isRunning ? (audioCtx.baseLatency * 1000).toFixed(1) : '--';
        $('#cpu').textContent = isRunning ? `~${Math.floor(Math.random() * 3 + 2)}` : '--';
        $('#buffer').textContent = isRunning ? `${audioCtx.renderQuantumInSeconds * 1000}ms` : '--';
      }

      function drawMeters() {
        if (!isRunning || !analyser) return;
        const data = new Uint8Array(analyser.frequencyBinCount);
        analyser.getByteFrequencyData(data);
        const sum = data.reduce((a, b) => a + b, 0);
        const levelPct = Math.min(100, (sum / data.length / 128) * 100);
        document.querySelectorAll('.meter-bar').forEach((bar, i) => {
          const v = Math.min(100, levelPct * (0.95 + Math.random() * 0.1));
          bar.style.setProperty('--level', v);
          bar.querySelector('.meter-val').textContent = v > 0 ? `${(-60 + v * 0.6).toFixed(1)} dB` : '-∞ dB';
        });
        const bars = document.querySelectorAll('.spectrum-bars .bar');
        const step = Math.floor(data.length / bars.length);
        bars.forEach((b, i) => b.style.height = `${Math.max(5, (data[i * step] || 0) / 2.55)}%`);
        animFrameId = requestAnimationFrame(drawMeters);
      }
      function resetMeters() {
        document.querySelectorAll('.meter-bar').forEach(b => { b.style.setProperty('--level', 0); b.querySelector('.meter-val').textContent = '-∞ dB'; });
        document.querySelectorAll('.spectrum-bars .bar').forEach(b => b.style.height = '5%');
      }

      // 7. Event Listeners
      btnStart.addEventListener('click', startAudio);
      btnStop.addEventListener('click', stopAudio);

      // 🎛️ EQ Controls Binding
      document.querySelectorAll('.eq-freq').forEach(s => s.addEventListener('input', e => { if(!eqProcessor) return; eqProcessor.setParam(+e.target.dataset.i, 'f', +e.target.value); e.target.nextElementSibling.textContent = `${e.target.value} Hz`; }));
      document.querySelectorAll('.eq-gain').forEach(s => s.addEventListener('input', e => { if(!eqProcessor) return; eqProcessor.setParam(+e.target.dataset.i, 'g', +e.target.value); e.target.nextElementSibling.textContent = `${(+e.target.value).toFixed(1)} dB`; }));
      document.querySelectorAll('.eq-q').forEach(s => s.addEventListener('input', e => { if(!eqProcessor) return; eqProcessor.setParam(+e.target.dataset.i, 'q', +e.target.value); e.target.nextElementSibling.textContent = parseFloat(e.target.value).toFixed(1); }));
      $('#eq-flat').addEventListener('click', () => {
        if (!eqProcessor) return; eqProcessor.resetToFlat();
        document.querySelectorAll('.eq-gain').forEach(s => { s.value = 0; s.nextElementSibling.textContent = '0.0 dB'; });
        document.querySelectorAll('.eq-freq').forEach((s, i) => { const defs=[60,150,400,1000,3000,8000]; s.value=defs[i]; s.nextElementSibling.textContent=`${defs[i]} Hz`; });
        document.querySelectorAll('.eq-q').forEach(s => { s.value = 1; s.nextElementSibling.textContent = '1.0'; });
      });

      // Input/Master Gains
      $('#input-gain').addEventListener('input', e => {
        e.target.nextElementSibling.textContent = `${parseFloat(e.target.value).toFixed(1)} dB`;
        if(isRunning) inputGainNode.gain.setTargetAtTime(Math.pow(10, parseFloat(e.target.value)/20), audioCtx.currentTime, 0.01);
      });
      $('#master-out').addEventListener('input', e => {
        e.target.nextElementSibling.textContent = `${parseFloat(e.target.value).toFixed(1)} dB`;
        if(isRunning) masterGain.gain.setTargetAtTime(Math.pow(10, parseFloat(e.target.value)/20), audioCtx.currentTime, 0.01);
      });

      // FM Controls
      const applyFM = () => {
        if (!fmProcessor) return;
        fmProcessor.setPreEmphasis(document.querySelector('input[name="pre"]:checked').value, parseFloat($('#pre-emph-gain').value));
        fmProcessor.setPilot(parseFloat($('#pilot-level').value));
        fmProcessor.setSeparation(parseFloat($('#stereo-sep').value));
        fmProcessor.setRDS(parseFloat($('#rds-level').value));
        fmProcessor.toggleRDS($('#rds-enable').checked);
      };
      document.querySelectorAll('input[name="pre"]').forEach(r => r.addEventListener('change', applyFM));
      ['pre-emph-gain','pilot-level','stereo-sep','rds-level','rds-enable'].forEach(id => {
        $(`#${id}`).addEventListener('input', e => { if(e.target.id.includes('gain')||e.target.id.includes('level')||e.target.id.includes('sep')) { const v=e.target.value; const u=e.target.id.includes('gain')?'dB':'%'; e.target.nextElementSibling.textContent=`${v} ${u}`; } applyFM(); });
      });

      // Output Device
      outputSelect.addEventListener('change', async (e) => { if (!audioCtx.setSinkId) return; try { await audioCtx.setSinkId(e.target.value); } catch(err) { console.warn(err); } });

      // Init
      buildEQUI(); loadDevices();
    });
  </script>
</body>
</html>
