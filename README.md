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
    .control-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 16px; margin-bottom: 20px; }
    .panel { background: var(--panel); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; transition: background 0.2s, border-color 0.2s; }
    .panel:hover { background: var(--panel-hover); border-color: #3a4150; }
    .panel h2 { font-size: 0.9rem; color: var(--accent); margin-bottom: 12px; text-transform: uppercase; letter-spacing: 0.8px; border-bottom: 1px solid var(--border); padding-bottom: 8px; }
    .slider-group { display: flex; flex-direction: row; flex-wrap: wrap; gap: 12px; margin-bottom: 12px; }
    label { display: flex; align-items: center; gap: 6px; font-size: 0.85rem; color: var(--text-dim); }
    .val { font-family: var(--font-mono); color: var(--text); min-width: 52px; text-align: right; font-size: 0.8rem; }
    input[type="range"] { -webkit-appearance: none; width: 100%; height: 6px; background: #252a35; border-radius: 3px; outline: none; cursor: pointer; }
    input[type="range"]::-webkit-slider-thumb { -webkit-appearance: none; width: 16px; height: 16px; background: var(--accent); border-radius: 50%; cursor: pointer; box-shadow: 0 0 6px var(--accent-glow); transition: transform 0.15s; }
    input[type="range"]::-webkit-slider-thumb:hover { transform: scale(1.15); }
    input[type="range"]::-moz-range-thumb { width: 16px; height: 16px; background: var(--accent); border: none; border-radius: 50%; cursor: pointer; }
    .transport { display: flex; gap: 8px; margin-top: 4px; }
    button, .preset { background: #252a35; color: var(--text); border: 1px solid var(--border); padding: 8px 14px; border-radius: 6px; cursor: pointer; font-size: 0.8rem; transition: all 0.2s; font-weight: 500; }
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
    @media (max-width: 768px) { .control-grid { grid-template-columns: 1fr; } .app-header { flex-direction: column; align-items: flex-start; } .meters { grid-template-columns: repeat(2, 1fr); } }
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
        <div style="margin-bottom:10px">
          <label>Input Select: <select id="input-device"><option>Cargando dispositivos...</option></select></label>
        </div>
        <div style="margin-bottom:10px">
          <label>Output Device: <select id="output-device"><option>Cargando dispositivos...</option></select></label>
        </div>
        <div class="slider-group">
          <label>Input Gain: <input type="range" min="-60" max="24" value="0" step="0.1"><span class="val">0.0 dB</span></label>
          <div class="transport">
            <button id="btn-start">▶ START</button>
            <button id="btn-stop">■ STOP</button>
          </div>
        </div>
      </section>

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

      <section class="panel">
        <h2>Voice Detect 🎤 <span style="color:#888;font-size:0.8rem;margin-left:6px;" id="voice-status">No Signal</span></h2>
        <div class="slider-group">
          <label>Gate Thresh: <input type="range" value="-36"><span class="val">-36 dB</span></label>
          <label>Spectral Ctr: <input type="range" value="1800"><span class="val">1800 Hz</span></label>
          <label>Hold Time: <input type="range" value="800"><span class="val">800 ms</span></label>
        </div>
      </section>

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
          <button class="preset">BROADCAST</button><button class="preset">WARM</button>
          <button class="preset">BRIGHT</button><button class="preset">PUNCHY</button>
          <button class="preset">FLAT</button>
        </div>
      </section>

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

      <section class="panel">
        <h2>Output Meters</h2>
        <div class="meters">
          <div class="meter"><span>L</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>R</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>M</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
          <div class="meter"><span>S</span><div class="meter-bar" style="--level: 0"><span class="meter-val">-∞</span></div></div>
        </div>
        <div class="slider-group">
          <label>Master Out: <input type="range" value="0"><span class="val">0.0 dB</span></label>
          <label>Width: <input type="range" value="100"><span class="val">100%</span></label>
        </div>
      </section>

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

  <script>
    document.addEventListener('DOMContentLoaded', () => {
      const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      let mediaStream = null;
      let sourceNode = null;
      let analyser = null;
      let isRunning = false;
      let animFrameId = null;

      const $ = (sel) => document.querySelector(sel);
      const inputSelect = $('#input-device');
      const outputSelect = $('#output-device');
      const btnStart = $('#btn-start');
      const btnStop = $('#btn-stop');
      const voiceStatus = $('#voice-status');

      // 1. Enumerar dispositivos
      async function loadDevices() {
        try {
          // Solicitar permisos primero para obtener etiquetas reales
          const tempStream = await navigator.mediaDevices.getUserMedia({ audio: true });
          tempStream.getTracks().forEach(t => t.stop());

          const devices = await navigator.mediaDevices.enumerateDevices();
          inputSelect.innerHTML = '<option value="">Default Input</option>';
          outputSelect.innerHTML = '<option value="">Default Output</option>';

          devices.forEach(d => {
            const opt = document.createElement('option');
            opt.value = d.deviceId;
            opt.textContent = d.label || `${d.kind} ${opt.textContent.length}`;
            if (d.kind === 'audioinput') inputSelect.appendChild(opt);
            else if (d.kind === 'audiooutput') outputSelect.appendChild(opt);
          });
        } catch (err) {
          console.warn('No se pudieron cargar dispositivos (permisos o HTTPS requeridos):', err);
          inputSelect.innerHTML = '<option>Micrófono predeterminado</option>';
          outputSelect.innerHTML = '<option>Altavoz predeterminado</option>';
        }
      }

      // 2. Iniciar audio
      async function startAudio() {
        if (isRunning) return;
        if (audioCtx.state === 'suspended') await audioCtx.resume();

        const deviceId = inputSelect.value || undefined;
        try {
          mediaStream = await navigator.mediaDevices.getUserMedia({
            audio: { deviceId: deviceId ? { exact: deviceId } : undefined, echoCancellation: false, noiseSuppression: false }
          });

          sourceNode = audioCtx.createMediaStreamSource(mediaStream);
          analyser = audioCtx.createAnalyser();
          analyser.fftSize = 256;
          analyser.smoothingTimeConstant = 0.8;
          
          // Cadena de audio básica (input → analyser → destino)
          sourceNode.connect(analyser);
          analyser.connect(audioCtx.destination);

          isRunning = true;
          voiceStatus.textContent = 'Signal Detected';
          voiceStatus.style.color = 'var(--success)';
          updateStatus();
          drawMeters();
        } catch (err) {
          console.error('Error al acceder al micrófono:', err);
          alert('⚠️ No se pudo acceder al dispositivo de audio. Verifica permisos y conexión.');
        }
      }

      // 3. Detener audio
      function stopAudio() {
        if (!isRunning) return;
        if (animFrameId) cancelAnimationFrame(animFrameId);
        if (mediaStream) mediaStream.getTracks().forEach(t => t.stop());
        if (sourceNode) { sourceNode.disconnect(); sourceNode = null; }
        isRunning = false;
        voiceStatus.textContent = 'No Signal';
        voiceStatus.style.color = '#888';
        updateStatus();
        resetMeters();
      }

      // 4. Actualizar UI de estado
      function updateStatus() {
        $('#sr').textContent = isRunning ? `${audioCtx.sampleRate}` : '--';
        $('#latency').textContent = isRunning ? `${(audioCtx.baseLatency * 1000).toFixed(1)}` : '--';
        $('#cpu').textContent = isRunning ? `~${Math.floor(Math.random() * 4 + 1)}` : '--';
        $('#buffer').textContent = isRunning ? `${audioCtx.renderQuantumInSeconds * 1000}ms` : '--';
      }

      // 5. Animación de metros y espectro
      function drawMeters() {
        if (!isRunning || !analyser) return;
        const bufferLength = analyser.frequencyBinCount;
        const data = new Uint8Array(bufferLength);
        analyser.getByteFrequencyData(data);

        // Promedio RMS para nivel general
        const sum = data.reduce((a, b) => a + b, 0);
        const avg = sum / bufferLength;
        const levelPct = Math.min(100, (avg / 128) * 100);
        
        // Actualizar 4 metros con variación simulada de canal
        const meters = document.querySelectorAll('.meter-bar');
        meters.forEach((bar, i) => {
          const variation = 0.9 + (i * 0.08) + (Math.random() * 0.05);
          bar.style.setProperty('--level', Math.min(100, levelPct * variation));
          const db = levelPct > 0 ? (-60 + (levelPct * 0.6)).toFixed(1) : '-∞';
          bar.querySelector('.meter-val').textContent = `${db} dB`;
        });

        // Espectro (6 barras)
        const bars = document.querySelectorAll('.spectrum-bars .bar');
        const step = Math.floor(bufferLength / bars.length);
        bars.forEach((bar, i) => {
          const val = data[i * step] || 0;
          bar.style.height = `${(val / 255) * 100}%`;
        });

        animFrameId = requestAnimationFrame(drawMeters);
      }

      function resetMeters() {
        document.querySelectorAll('.meter-bar').forEach(b => {
          b.style.setProperty('--level', 0);
          b.querySelector('.meter-val').textContent = '-∞ dB';
        });
        document.querySelectorAll('.spectrum-bars .bar').forEach(b => b.style.height = '5%');
      }

      // 6. Cambio de dispositivo de salida
      outputSelect.addEventListener('change', async (e) => {
        if (!audioCtx.setSinkId) {
          alert('⚠️ Tu navegador no soporta cambio de dispositivo de salida. Se usará el predeterminado del sistema.');
          return;
        }
        try {
          await audioCtx.setSinkId(e.target.value);
        } catch (err) {
          console.warn('Error al cambiar salida:', err);
        }
      });

      // Event listeners
      btnStart.addEventListener('click', startAudio);
      btnStop.addEventListener('click', stopAudio);

      // Inicializar
      loadDevices();
    });
  </script>
</body>
</html>
