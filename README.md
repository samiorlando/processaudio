<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Optimod-PC 1600 Clone - Audio Processor (FIXED)</title>
<style>
  :root {
    --bg-dark: #08080c;
    --bg-panel: #101016;
    --bg-section: #181820;
    --border-color: #252530;
    --text-primary: #e4e4ea;
    --text-secondary: #9999a5;
    --text-dim: #666675;
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
  body { background: var(--bg-dark); color: var(--text-primary); font-family: 'Segoe UI', system-ui, sans-serif; overflow-x: hidden; }
  .header { background: linear-gradient(180deg, #15151f, #0a0a10); border-bottom: 2px solid var(--accent-green-dim); padding: 10px 20px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 100; }
  .logo-main { font-size: 22px; font-weight: 800; letter-spacing: 3px; background: linear-gradient(90deg, #00ff88, #00ccff); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
  .logo-sub { font-size: 9px; color: var(--text-dim); letter-spacing: 1px; }
  .power-btn { width: 46px; height: 46px; border-radius: 50%; border: 2px solid #333; background: radial-gradient(circle, #1a1a1a, #0d0d10); cursor: pointer; display: flex; align-items: center; justify-content: center; transition: 0.3s; }
  .power-btn:hover { border-color: #555; }
  .power-btn.on { border-color: var(--accent-green); box-shadow: 0 0 15px rgba(0,255,136,0.3); }
  .power-icon { width: 18px; height: 18px; border: 2px solid #555; border-radius: 50%; position: relative; }
  .power-icon::after { content: ''; position: absolute; top: -4px; left: 50%; transform: translateX(-50%); width: 2px; height: 8px; background: #555; }
  .power-btn.on .power-icon { border-color: var(--accent-green); box-shadow: 0 0 4px var(--accent-green); }
  .power-btn.on .power-icon::after { background: var(--accent-green); box-shadow: 0 0 4px var(--accent-green); }
  .main-container { padding: 12px; display: flex; flex-direction: column; gap: 10px; max-width: 1400px; margin: 0 auto; }
  .panel { background: var(--bg-panel); border: 1px solid var(--border-color); border-radius: 6px; overflow: hidden; transition: opacity 0.3s; }
  .panel.disabled { opacity: 0.25; pointer-events: none; }
  .panel-header { background: linear-gradient(180deg, #1c1c28, #14141c); padding: 8px 14px; display: flex; align-items: center; justify-content: space-between; border-bottom: 1px solid var(--border-color); }
  .panel-title { font-size: 10px; font-weight: 600; letter-spacing: 1.5px; text-transform: uppercase; color: var(--accent-green-dim); }
  .panel-body { padding: 14px; }
  .row { display: flex; gap: 12px; flex-wrap: wrap; }
  .col { flex: 1; display: flex; flex-direction: column; gap: 10px; }
  select, .btn { background: #0d0d14; border: 1px solid #2a2a35; color: var(--text-primary); padding: 5px 10px; font-size: 11px; border-radius: 4px; cursor: pointer; }
  .btn { font-size: 9px; letter-spacing: 1px; text-transform: uppercase; transition: 0.2s; }
  .btn:hover, .btn.active { border-color: var(--accent-green); color: var(--accent-green); box-shadow: 0 0 6px rgba(0,255,136,0.15); }
  .meter-bar { width: 18px; height: 160px; background: #080810; border-radius: 3px; position: relative; overflow: hidden; border: 1px solid #1a1a25; margin: 0 auto; }
  .meter-fill { position: absolute; bottom: 0; left: 0; width: 100%; height: 0%; transition: height 0.06s linear; }
  .meter-label { font-size: 8px; color: var(--text-secondary); text-align: center; margin-bottom: 4px; letter-spacing: 1px; }
  .meter-scale { position: absolute; right: -12px; top: 0; height: 100%; display: flex; flex-direction: column; justify-content: space-between; padding: 4px 0; pointer-events: none; }
  .meter-scale span { font-size: 7px; color: var(--text-dim); font-family: monospace; }
  .analyzer-canvas { width: 100%; height: 70px; background: #06060a; border-radius: 4px; border: 1px solid #151520; }
  .knob-container { display: flex; flex-direction: column; align-items: center; gap: 4px; cursor: ns-resize; user-select: none; }
  .knob-wrapper { width: 50px; height: 50px; position: relative; }
  .knob-svg { width: 100%; height: 100%; transform: rotate(-90deg); }
  .knob-bg { fill: none; stroke: #1a1a25; stroke-width: 3; }
  .knob-fill { fill: none; stroke: var(--accent-green); stroke-width: 3; stroke-linecap: round; transition: stroke-dashoffset 0.1s; filter: drop-shadow(0 0 3px rgba(0,255,136,0.4)); }
  .knob-fill.amber { stroke: var(--accent-amber); }
  .knob-fill.cyan { stroke: var(--accent-cyan); }
  .knob-fill.red { stroke: var(--accent-red); }
  .knob-inner { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 36px; height: 36px; border-radius: 50%; background: radial-gradient(circle at 35% 35%, #2a2a38, #15151f); border: 1px solid #2a2a38; }
  .knob-indicator { position: absolute; top: 3px; left: 50%; transform: translateX(-50%); width: 2px; height: 7px; background: var(--accent-green); border-radius: 1px; box-shadow: 0 0 3px var(--accent-green); }
  .knob-label { font-size: 8px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.5px; }
  .knob-value { font-size: 9px; color: var(--accent-green); font-family: monospace; text-align: center; min-width: 45px; }
  input[type="range"] { -webkit-appearance: none; background: transparent; cursor: pointer; }
  input[type="range"]::-webkit-slider-runnable-track { height: 3px; background: #1a1a25; border-radius: 2px; }
  input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; width: 12px; height: 12px; border-radius: 50%; background: #333; border: 2px solid var(--accent-green); margin-top: -4px; }
  .slider-horizontal { width: 100%; height: 3px; }
  .slider-vertical { writing-mode: vertical-lr; direction: rtl; height: 90px; width: 14px; }
  .mb-band { background: var(--bg-section); border: 1px solid var(--border-color); border-radius: 5px; padding: 8px; }
  .mb-band-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; padding-bottom: 4px; border-bottom: 1px solid #222; }
  .mb-band-name { font-size: 9px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; }
  .mb-band-name.sub { color: #aa44ff; } .mb-band-name.bass { color: var(--accent-cyan); } .mb-band-name.mid { color: var(--accent-green); }
  .mb-band-name.presence { color: var(--accent-amber); } .mb-band-name.air { color: #ff88aa; } .mb-band-name.limiter { color: var(--accent-red); }
  .mb-params { display: grid; grid-template-columns: 1fr 1fr; gap: 5px; }
  .mb-param label { font-size: 7px; color: var(--text-dim); text-transform: uppercase; display: block; margin-bottom: 2px; }
  .mb-param input { width: 100%; height: 2px; }
  .mb-param span { font-size: 8px; color: var(--accent-green); font-family: monospace; text-align: right; display: block; margin-top: 1px; }
  .geq-band { display: flex; flex-direction: column; align-items: center; gap: 3px; flex: 1; }
  .geq-freq { font-size: 7px; color: var(--text-dim); font-family: monospace; }
  .loudness-box { background: #080810; border: 1px solid #1a1a25; border-radius: 5px; padding: 8px 12px; text-align: center; min-width: 100px; }
  .loudness-val { font-family: monospace; font-size: 22px; font-weight: 700; color: var(--accent-green); text-shadow: 0 0 8px rgba(0,255,136,0.3); }
  .loudness-unit { font-size: 9px; color: var(--text-secondary); }
  .toggle { position: relative; width: 32px; height: 16px; background: #1a1a25; border-radius: 8px; cursor: pointer; border: 1px solid #2a2a35; transition: 0.3s; }
  .toggle.on { background: rgba(0,255,136,0.15); border-color: var(--accent-green); }
  .toggle::after { content: ''; position: absolute; top: 2px; left: 2px; width: 10px; height: 10px; border-radius: 50%; background: #444; transition: 0.3s; }
  .toggle.on::after { left: 18px; background: var(--accent-green); box-shadow: 0 0 4px var(--accent-green); }
  .status-dot { width: 7px; height: 7px; border-radius: 50%; background: #333; display: inline-block; margin-right: 4px; transition: 0.3s; }
  .status-dot.on { background: var(--accent-green); box-shadow: 0 0 6px var(--accent-green); animation: pulse 2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.4} }
  .output-status { font-size: 9px; color: var(--text-dim); margin-top: 4px; text-align: center; }
  .output-status.active { color: var(--accent-green); }
  @media(max-width:800px) { .row { flex-direction: column; } .mb-params { grid-template-columns: 1fr; } }
</style>
</head>
<body>
<div class="header">
  <div>
    <div style="font-size:9px;color:var(--text-dim);letter-spacing:2px;">BROADCAST AUDIO PROCESSOR</div>
    <div class="logo-main">OPTIMOD-PC 1600</div>
  </div>
  <div style="display:flex;align-items:center;gap:15px;">
    <div style="text-align:center;">
      <div class="power-btn" id="powerBtn" onclick="togglePower()"><div class="power-icon"></div></div>
      <div style="font-size:8px;color:var(--text-dim);margin-top:3px;">POWER</div>
    </div>
    <div class="status-dot" id="statusDot"></div>
    <span style="font-size:10px;color:var(--text-dim);">DSP ACTIVE</span>
    <div style="background:#080810;padding:3px 6px;border-radius:3px;font-size:10px;font-family:monospace;border:1px solid #1a1a25;" id="srDisplay">-- kHz</div>
  </div>
</div>

<div class="main-container" id="mainContainer">
  <!-- INPUT -->
  <div class="panel disabled" id="inputPanel">
    <div class="panel-header"><span class="panel-title">🔊 INPUT</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <select id="inputSelect" onchange="changeInput()"><option>Select Input...</option></select>
        <button class="btn" onclick="refreshDevices()">Scan</button>
      </div>
    </div>
    <div class="panel-body">
      <div class="row">
        <div class="knob-container" id="gainKnob" data-param="inputGain" data-min="0" data-max="24" data-value="10" data-unit="dB">
          <div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 50 50"><circle class="knob-bg" cx="25" cy="25" r="20" stroke-dasharray="100"/><circle class="knob-fill" cx="25" cy="25" r="20" stroke-dasharray="100"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div>
          <span class="knob-label">Gain</span><span class="knob-value">10.0 dB</span>
        </div>
        <div style="flex:1;display:flex;gap:10px;">
          <div style="display:flex;gap:6px;">
            <div><div class="meter-label">L</div><div class="meter-bar"><div class="meter-fill" id="inMeterL"></div><div class="meter-scale"><span>0</span><span>-12</span><span>-24</span><span>-48</span></div></div></div>
            <div><div class="meter-label">R</div><div class="meter-bar"><div class="meter-fill" id="inMeterR"></div><div class="meter-scale"><span>0</span><span>-12</span><span>-24</span><span>-48</span></div></div></div>
          </div>
          <canvas id="inCanvas" class="analyzer-canvas" style="flex:1;"></canvas>
        </div>
      </div>
    </div>
  </div>

  <!-- EQ -->
  <div class="panel disabled" id="eqPanel">
    <div class="panel-header"><span class="panel-title">🎚️ PARAMETRIC EQ</span>
      <div style="display:flex;gap:5px;">
        <button class="btn" onclick="setPreset('flat')">Flat</button>
        <button class="btn" onclick="setPreset('bass')">Bass Boost</button>
        <button class="btn" onclick="setPreset('voice')">Voice</button>
        <button class="btn" onclick="setPreset('bright')">Bright</button>
      </div>
    </div>
    <div class="panel-body" id="eqContainer" style="display:flex;justify-content:space-around;flex-wrap:wrap;gap:8px;"></div>
  </div>

  <!-- MB COMP -->
  <div class="panel disabled" id="mbPanel">
    <div class="panel-header"><span class="panel-title">🎛️ MULTIBAND COMPRESSOR</span><div class="toggle on" onclick="toggleSwitch(this)"></div></div>
    <div class="panel-body">
      <div class="row" id="mbContainer"></div>
    </div>
  </div>

  <!-- GRAPHIC EQ -->
  <div class="panel disabled" id="geqPanel">
    <div class="panel-header"><span class="panel-title">📊 GRAPHIC EQ</span><button class="btn" onclick="resetGeq()">Reset</button></div>
    <div class="panel-body"><div id="geqContainer" style="display:flex;justify-content:space-between;gap:4px;align-items:flex-end;"></div></div>
  </div>

  <!-- OUTPUT -->
  <div class="panel disabled" id="outPanel">
    <div class="panel-header"><span class="panel-title">📤 OUTPUT & ROUTING</span>
      <div style="display:flex;gap:8px;align-items:center;">
        <select id="outputSelect" onchange="changeOutput()"><option>Default Output</option></select>
        <button class="btn" onclick="refreshDevices()">Scan</button>
      </div>
    </div>
    <div class="panel-body">
      <div class="row" style="align-items:center;">
        <div class="knob-container" id="volKnob" data-param="outVol" data-min="-30" data-max="6" data-value="0" data-unit="dB">
          <div class="knob-wrapper"><svg class="knob-svg" viewBox="0 0 50 50"><circle class="knob-bg" cx="25" cy="25" r="20" stroke-dasharray="100"/><circle class="knob-fill red" cx="25" cy="25" r="20" stroke-dasharray="100"/></svg><div class="knob-inner"><div class="knob-indicator"></div></div></div>
          <span class="knob-label">Master</span><span class="knob-value">0.0 dB</span>
        </div>
        <div style="display:flex;gap:8px;align-items:flex-end;">
          <div><div class="meter-label">OUT L</div><div class="meter-bar"><div class="meter-fill" id="outMeterL"></div><div class="meter-scale"><span>0</span><span>-12</span><span>-24</span><span>-48</span></div></div></div>
          <div><div class="meter-label">OUT R</div><div class="meter-bar"><div class="meter-fill" id="outMeterR"></div><div class="meter-scale"><span>0</span><span>-12</span><span>-24</span><span>-48</span></div></div></div>
        </div>
        <div style="flex:1;">
          <div class="loudness-box"><div class="loudness-val" id="lufsDisplay">-- <span class="loudness-unit">LUFS</span></div><div style="font-size:8px;color:var(--text-dim);">INTEGRATED LOUDNESS</div></div>
        </div>
        <div style="flex:1;">
          <canvas id="outCanvas" class="analyzer-canvas"></canvas>
        </div>
      </div>
      <div class="output-status" id="outputStatus">⏳ Waiting for engine...</div>
    </div>
  </div>
</div>

<script>
// ==================== CORE AUDIO ENGINE ====================
let ctx = null;
let inputNode = null;
let stream = null;
let isActive = false;

// Processing nodes
let gainNode, analyserIn, analyserOut, analyserMaster;
let eqNodes = [];
let mbComps = [];
let geqNodes = [];
let outGain, limiter;
let outAudioElement = null; // Puente para setSinkId
let outStreamDest = null;

// ==================== POWER & INIT ====================
async function togglePower() {
  const btn = document.getElementById('powerBtn');
  if (!isActive) {
    await initAudio();
    btn.classList.add('on');
    isActive = true;
    document.querySelectorAll('.panel').forEach(p => p.classList.remove('disabled'));
    document.getElementById('statusDot').classList.add('on');
    document.getElementById('outputStatus').textContent = '🟢 Engine Active';
    document.getElementById('outputStatus').classList.add('active');
    renderUI();
    startMeters();
  } else {
    stopAudio();
    btn.classList.remove('on');
    isActive = false;
    document.querySelectorAll('.panel').forEach(p => p.classList.add('disabled'));
    document.getElementById('statusDot').classList.remove('on');
    document.getElementById('outputStatus').textContent = '🔴 Engine Off';
    document.getElementById('outputStatus').classList.remove('active');
  }
}

async function initAudio() {
  ctx = new (window.AudioContext || window.webkitAudioContext)();
  document.getElementById('srDisplay').textContent = (ctx.sampleRate/1000).toFixed(1) + ' kHz';
  
  // 1. Puente de salida para setSinkId
  outAudioElement = new Audio();
  outAudioElement.autoplay = true;
  outAudioElement.muted = false;
  outStreamDest = ctx.createMediaStreamDestination();
  outAudioElement.srcObject = outStreamDest.stream;
  document.body.appendChild(outAudioElement);
  
  // 2. Nodos base
  gainNode = ctx.createGain();
  analyserIn = ctx.createAnalyser(); analyserIn.fftSize = 2048; analyserIn.smoothingTimeConstant = 0.8;
  analyserOut = ctx.createAnalyser(); analyserOut.fftSize = 2048; analyserOut.smoothingTimeConstant = 0.8;
  analyserMaster = ctx.createAnalyser(); analyserMaster.fftSize = 4096; analyserMaster.smoothingTimeConstant = 0.9;
  outGain = ctx.createGain();
  limiter = ctx.createDynamicsCompressor();
  limiter.threshold.value = -0.5; limiter.ratio.value = 20; limiter.attack.value = 0.001; limiter.release.value = 0.1;
  
  // 3. Crear EQ, MB, GEQ
  createEQ();
  createMBComp();
  createGEQ();
  
  // 4. Enrutar cadena
  buildChain();
  
  // 5. Dispositivos
  refreshDevices();
  if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
    changeInput();
  }
}

function stopAudio() {
  if (stream) { stream.getTracks().forEach(t => t.stop()); stream = null; }
  if (inputNode) { inputNode.disconnect(); inputNode = null; }
  if (ctx) { ctx.close(); ctx = null; }
  if (outAudioElement) { outAudioElement.pause(); outAudioElement.srcObject = null; }
}

function buildChain() {
  if (!ctx) return;
  // Desconectar todo
  try { [gainNode, ...eqNodes, ...mbComps.map(c=>c.comp), ...geqNodes, limiter, outGain].forEach(n=>n.disconnect()); } catch(e){}
  
  // Cadena: Source -> Gain -> AnalyserIn -> EQ -> MB -> GEQ -> Limiter -> OutGain -> AnalyserOut -> StreamDest -> HTMLAudioElement
  if (inputNode) {
    inputNode.connect(gainNode);
    gainNode.connect(analyserIn);
    
    let prev = analyserIn;
    eqNodes.forEach(n => { prev.connect(n); prev = n; });
    
    // Multiband en serie simple para demo estable
    mbComps.forEach(b => { prev.connect(b.comp); prev = b.comp; });
    
    let prevGeq = prev;
    geqNodes.forEach(n => { prevGeq.connect(n); prevGeq = n; });
    
    prevGeq.connect(limiter);
    limiter.connect(outGain);
    outGain.connect(analyserOut);
    outGain.connect(analyserMaster);
    analyserOut.connect(outStreamDest); // ¡AQUÍ VA AL PUENTE DE SALIDA!
  }
}

// ==================== INPUT / OUTPUT DEVICES ====================
async function refreshDevices() {
  try {
    const devices = await navigator.mediaDevices.enumerateDevices();
    const inSel = document.getElementById('inputSelect');
    const outSel = document.getElementById('outputSelect');
    inSel.innerHTML = '<option>Select Input...</option>';
    outSel.innerHTML = '<option value="">Default Output (System)</option>';
    
    devices.filter(d => d.kind === 'audioinput').forEach((d,i) => {
      inSel.innerHTML += `<option value="${d.deviceId}">${d.label || 'Input '+i}</option>`;
    });
    devices.filter(d => d.kind === 'audiooutput').forEach((d,i) => {
      outSel.innerHTML += `<option value="${d.deviceId}">${d.label || 'Output '+i}</option>`;
    });
  } catch(e) { console.warn('Device enum failed', e); }
}

async function changeInput() {
  if (!ctx) return;
  const id = document.getElementById('inputSelect').value;
  if (stream) { stream.getTracks().forEach(t=>t.stop()); inputNode.disconnect(); }
  try {
    stream = await navigator.mediaDevices.getUserMedia({ audio: id ? {deviceId:{exact:id}} : true });
    inputNode = ctx.createMediaStreamSource(stream);
    buildChain();
  } catch(e) { alert('Error al acceder al micrófono. Verifica permisos.'); }
}

async function changeOutput() {
  const id = document.getElementById('outputSelect').value;
  if (!outAudioElement || !outAudioElement.setSinkId) {
    document.getElementById('outputStatus').textContent = '⚠️ setSinkId no soportado en este navegador. Usa Default.';
    return;
  }
  try {
    if (id) {
      await outAudioElement.setSinkId(id);
      const name = outAudioElement.options[outAudioElement.selectedIndex].text;
      document.getElementById('outputStatus').textContent = `🔊 Output: ${name}`;
      document.getElementById('outputStatus').classList.add('active');
    } else {
      await outAudioElement.setSinkId('');
      document.getElementById('outputStatus').textContent = '🔊 Output: System Default';
    }
  } catch(e) { console.error(e); document.getElementById('outputStatus').textContent = '❌ Error al cambiar salida.'; }
}

// ==================== PROCESSING CREATION ====================
function createEQ() {
  const defs = [
    { f:80, g:0, q:0.7, t:'lowshelf' }, { f:250, g:0, q:1, t:'peaking' },
    { f:1000, g:0, q:1, t:'peaking' }, { f:4000, g:0, q:1, t:'peaking' }, { f:12000, g:0, q:0.7, t:'highshelf' }
  ];
  eqNodes = defs.map(d => { const n = ctx.createBiquadFilter(); n.frequency.value=d.f; n.gain.value=d.g; n.Q.value=d.q; n.type=d.t; return n; });
}

function createMBComp() {
  mbComps = [
    { name:'SUB', c:'sub', t:-24, r:4, a:0.015, rel:0.2 },
    { name:'BASS', c:'bass', t:-22, r:3.5, a:0.012, rel:0.18 },
    { name:'MID', c:'mid', t:-20, r:3, a:0.008, rel:0.15 },
    { name:'PRES', c:'presence', t:-22, r:3.5, a:0.005, rel:0.12 },
    { name:'AIR', c:'air', t:-24, r:2.5, a:0.003, rel:0.1 }
  ].map(b => {
    const c = ctx.createDynamicsCompressor();
    c.threshold.value=b.t; c.ratio.value=b.r; c.attack.value=b.a; c.release.value=b.rel; c.knee.value=6;
    return { ...b, comp: c };
  });
}

function createGEQ() {
  const freqs = [31,62,125,250,500,1000,2000,4000,6300,8000,10000,12500,14000,16000,18000];
  geqNodes = freqs.map(f => { const n = ctx.createBiquadFilter(); n.type='peaking'; n.frequency.value=f; n.Q.value=1.4; n.gain.value=0; return n; });
}

// ==================== UI RENDERING ====================
function renderUI() {
  // EQ
  const eqBox = document.getElementById('eqContainer');
  eqBox.innerHTML = '';
  eqNodes.forEach((n,i) => {
    const wrap = document.createElement('div');
    wrap.style.cssText = 'display:flex;flex-direction:column;align-items:center;gap:4px;min-width:60px;';
    wrap.innerHTML = `
      <div style="font-size:9px;color:var(--accent-green);">BAND ${i+1}</div>
      <div style="font-size:10px;color:var(--text-dim);font-family:monospace;">${n.frequency.value}Hz</div>
      <input type="range" class="slider-horizontal" min="-12" max="12" step="0.5" value="${n.gain}" oninput="eqNodes[${i}].gain.value=this.value;this.nextElementSibling.textContent=this.value+'dB'">
      <div style="font-size:9px;color:var(--accent-green);font-family:monospace;">${n.gain}dB</div>
      <input type="range" class="slider-horizontal" min="${n.type==='lowshelf'?20:n.type==='highshelf'?2000:50}" max="${n.type==='lowshelf'?500:n.type==='highshelf'?16000:16000}" value="${n.frequency.value}" oninput="eqNodes[${i}].frequency.value=this.value;this.previousElementSibling.textContent=this.value+'Hz'">
    `;
    eqBox.appendChild(wrap);
  });
  
  // MB
  const mbBox = document.getElementById('mbContainer');
  mbBox.innerHTML = '';
  mbComps.forEach((b,i) => {
    const div = document.createElement('div'); div.className='mb-band'; div.style.flex='1';
    div.innerHTML = `
      <div class="mb-band-header"><span class="mb-band-name ${b.c}">${b.name}</span></div>
      <div class="mb-params">
        <div class="mb-param"><label>Thresh</label><input type="range" min="-60" max="0" value="${b.t}" oninput="mbComps[${i}].comp.threshold.value=this.value"><span>${b.t}dB</span></div>
        <div class="mb-param"><label>Ratio</label><input type="range" min="1" max="20" value="${b.r}" oninput="mbComps[${i}].comp.ratio.value=this.value"><span>${b.r}:1</span></div>
        <div class="mb-param"><label>Attack</label><input type="range" min="0.1" max="100" value="${b.a*1000}" oninput="mbComps[${i}].comp.attack.value=this.value/1000"><span>${(b.a*1000).toFixed(0)}ms</span></div>
        <div class="mb-param"><label>Release</label><input type="range" min="10" max="1000" value="${b.rel*1000}" oninput="mbComps[${i}].comp.release.value=this.value/1000"><span>${(b.rel*1000).toFixed(0)}ms</span></div>
      </div>
    `;
    mbBox.appendChild(div);
  });
  
  // GEQ
  const geqBox = document.getElementById('geqContainer');
  geqBox.innerHTML = '';
  const freqs = [31,62,125,250,500,1,2,4,6.3,8,10,12.5,14,16,18];
  geqNodes.forEach((n,i) => {
    const div = document.createElement('div'); div.className='geq-band';
    div.innerHTML = `
      <input type="range" class="slider-vertical" min="-12" max="12" step="0.5" value="0" oninput="geqNodes[${i}].gain.value=this.value">
      <span style="font-size:8px;color:var(--text-dim);font-family:monospace;">${freqs[i]}${freqs[i]>=1?'k':''}</span>
    `;
    geqBox.appendChild(div);
  });
}

function setPreset(type) {
  const vals = {
    flat:[0,0,0,0,0], bass:[6,2,0,1,2], voice:[-3,-1,4,5,3], bright:[0,0,1,3,6]
  };
  if(!vals[type]) return;
  eqNodes.forEach((n,i) => { n.gain.value = vals[type][i]; });
  renderUI();
}
function resetGeq() { geqNodes.forEach(n => n.gain.value = 0); renderUI(); }
function toggleSwitch(el) { el.classList.toggle('on'); }

// ==================== KNOBS INTERACTION ====================
document.addEventListener('mousedown', e => {
  const knob = e.target.closest('.knob-container');
  if(!knob || !isActive) return;
  e.preventDefault();
  const startY = e.clientY;
  const startVal = parseFloat(knob.dataset.value);
  const min = parseFloat(knob.dataset.min);
  const max = parseFloat(knob.dataset.max);
  
  const onMove = ev => {
    const delta = (startY - ev.clientY) * 0.8;
    let val = startVal + (delta/100)*(max-min);
    val = Math.max(min, Math.min(max, val));
    knob.dataset.value = val;
    updateKnob(knob, val);
    applyKnob(knob, val);
  };
  const onUp = () => { document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
});

function updateKnob(k, v) {
  const min=parseFloat(k.dataset.min), max=parseFloat(k.dataset.max);
  const norm = (v-min)/(max-min);
  const offset = 100 - (norm * 75 + 12.5);
  k.querySelector('.knob-fill').style.strokeDashoffset = offset;
  const ind = k.querySelector('.knob-indicator');
  const angle = -90 + (norm*270 - 135);
  const rad = angle * Math.PI / 180;
  ind.style.left = (50 + 38*Math.cos(rad)) + '%';
  ind.style.top = (50 + 38*Math.sin(rad)) + '%';
  
  const unit = k.dataset.unit;
  let txt = v.toFixed(1);
  if(unit==='Hz') txt = v.toFixed(0)+' Hz';
  else if(unit==='dB') txt += ' dB';
  k.querySelector('.knob-value').textContent = txt;
}

function applyKnob(k, v) {
  if(k.dataset.param==='inputGain') gainNode.gain.value = Math.pow(10, v/20);
  if(k.dataset.param==='outVol') outGain.gain.value = Math.pow(10, v/20);
}

// ==================== METERS & ANALYZERS ====================
function startMeters() {
  const inC = document.getElementById('inCanvas');
  const outC = document.getElementById('outCanvas');
  if(inC) { inC.width = inC.offsetWidth*2; inC.height = inC.offsetHeight*2; }
  if(outC) { outC.width = outC.offsetWidth*2; outC.height = outC.offsetHeight*2; }
  
  const draw = () => {
    if(!isActive) return;
    requestAnimationFrame(draw);
    
    // Input Meters
    const dIn = new Uint8Array(analyserIn.frequencyBinCount);
    analyserIn.getByteFrequencyData(dIn);
    updateMeter('inMeterL', calcLvl(dIn)-2);
    updateMeter('inMeterR', calcLvl(dIn)-2);
    drawWave(inC, new Uint8Array(analyserIn.fftSize), analyserIn.getByteTimeDomainData, '#00ff88');
    
    // Output Meters
    const dOut = new Uint8Array(analyserOut.frequencyBinCount);
    analyserOut.getByteFrequencyData(dOut);
    updateMeter('outMeterL', calcLvl(dOut));
    updateMeter('outMeterR', calcLvl(dOut));
    drawWave(outC, new Uint8Array(analyserOut.fftSize), analyserOut.getByteTimeDomainData.bind(analyserOut), '#00ccff');
    
    // Loudness
    const rms = calcRMS(dOut);
    const lufs = -60 + (rms/255)*60;
    document.getElementById('lufsDisplay').innerHTML = lufs.toFixed(1) + ' <span class="loudness-unit">LUFS</span>';
  };
  draw();
}

function calcLvl(d) { let s=0; for(let i=0;i<d.length;i++) s+=d[i]; return s/d.length; }
function calcRMS(d) { let s=0; for(let i=0;i<d.length;i++) s+=d[i]*d[i]; return Math.sqrt(s/d.length); }

function updateMeter(id, val) {
  const el = document.getElementById(id);
  if(!el) return;
  const pct = Math.min(100, (val/255)*100);
  el.style.height = pct + '%';
  if(pct>85) { el.style.background='var(--meter-red)'; el.style.boxShadow='0 0 6px rgba(255,51,51,0.6)'; }
  else if(pct>65) { el.style.background='var(--meter-yellow)'; el.style.boxShadow='0 0 4px rgba(255,204,0,0.5)'; }
  else { el.style.background='var(--meter-green)'; el.style.boxShadow='0 0 4px rgba(0,255,102,0.4)'; }
}

function drawWave(canvas, buffer, getter, color) {
  if(!canvas) return;
  const ctx2d = canvas.getContext('2d');
  getter(buffer);
  ctx2d.clearRect(0,0,canvas.width,canvas.height);
  
  // Grid
  ctx2d.strokeStyle='rgba(255,255,255,0.04)'; ctx2d.lineWidth=1;
  for(let i=0;i<8;i++) { ctx2d.beginPath(); ctx2d.moveTo(0,i*canvas.height/8); ctx2d.lineTo(canvas.width,i*canvas.height/8); ctx2d.stroke(); }
  
  // Wave
  ctx2d.lineWidth=2; ctx2d.strokeStyle=color; ctx2d.shadowColor=color; ctx2d.shadowBlur=4;
  ctx2d.beginPath();
  const w = canvas.width, h = canvas.height;
  const slice = w/buffer.length;
  for(let i=0;i<buffer.length;i++) {
    const v = buffer[i]/128.0;
    const y = (v*h)/2;
    i===0 ? ctx2d.moveTo(0,y) : ctx2d.lineTo(i*slice, y);
  }
  ctx2d.stroke(); ctx2d.shadowBlur=0;
}

// Init Knobs
document.querySelectorAll('.knob-container').forEach(k => updateKnob(k, parseFloat(k.dataset.value)));
</script>
</body>
</html>
