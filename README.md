<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control de Bandejas Hospitalarias</title>
<style>
:root {
    --color-background-primary: #ffffff;
    --color-background-secondary: #f8faf9;
    --color-background-tertiary: #f1f3f4;
    --color-border-tertiary: #e0e0e0;
    --color-border-secondary: #d1d1d1;
    --color-text-primary: #202124;
    --color-text-secondary: #5f6368;
    --color-text-tertiary: #70757a;
    --color-text-danger: #d93025;
    --border-radius-md: 8px;
    --border-radius-lg: 12px;
    --font-sans: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}

*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font-sans); background: var(--color-background-secondary); color: var(--color-text-primary);}

.topbar{
  display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px;
  padding:12px 16px;
  background:var(--color-background-primary);
  border-bottom:0.5px solid var(--color-border-tertiary);
}
.app-brand{display:flex;align-items:center;gap:10px}
.app-icon{width:32px;height:32px;border-radius:8px;background:#E1F5EE;display:flex;align-items:center;justify-content:center}
.app-icon svg{width:18px;height:18px}
.app-name{font-size:15px;font-weight:600;color:var(--color-text-primary)}
.app-sub{font-size:11px;color:var(--color-text-tertiary);margin-top:1px}

.topbar-right{display:flex;align-items:center;gap:8px;flex-wrap:wrap}
.turno-pill{display:flex;align-items:center;gap:6px;padding:5px 10px;border-radius:20px;border:0.5px solid var(--color-border-secondary);background:var(--color-background-secondary)}
.turno-pill label{font-size:11px;color:var(--color-text-tertiary)}
.turno-pill select{font-size:12px;font-weight:500;border:none;background:transparent;color:var(--color-text-primary);padding:0;cursor:pointer}
.clock-pill{font-size:11px;color:var(--color-text-tertiary);padding:5px 10px;border-radius:20px;background:var(--color-background-secondary);border:0.5px solid var(--color-border-tertiary)}

.stats-row{
  display:grid;grid-template-columns:repeat(3,1fr);gap:8px;
  padding:10px 16px;
  border-bottom:0.5px solid var(--color-border-tertiary);
  background:var(--color-background-secondary);
}
.stat-card{
  background:var(--color-background-primary);
  border-radius:var(--border-radius-md);
  padding:8px 10px;
  border:0.5px solid var(--color-border-tertiary);
  display:flex;align-items:center;gap:8px;
}
.stat-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0}
.stat-dot.pending{background:#BA7517}
.stat-dot.delivered{background:#185FA5}
.stat-dot.collected{background:#3B6D11}
.stat-info{display:flex;flex-direction:column}
.stat-num{font-size:18px;font-weight:500;color:var(--color-text-primary);line-height:1}
.stat-lbl{font-size:10px;color:var(--color-text-tertiary);margin-top:2px}

.action-row{
  display:flex;gap:8px;padding:10px 16px;
  border-bottom:0.5px solid var(--color-border-tertiary);
  background:var(--color-background-primary);
  align-items:center;
}
.search-input {
  flex: 1;
  padding: 8px 12px;
  border-radius: 20px;
  border: 1px solid var(--color-border-tertiary);
  font-size: 13px;
  outline: none;
}
.search-input:focus { border-color: #185FA5; }

.btn-resumen{
  font-size:12px;padding:8px 14px;border-radius:var(--border-radius-md);
  border:0.5px solid #185FA5;background:#E6F1FB;color:#0C447C;cursor:pointer;font-weight:500;
}
.btn-reset{
  font-size:11px;padding:8px 12px;border-radius:var(--border-radius-md);
  border:0.5px solid var(--color-border-secondary);background:transparent;
  color:var(--color-text-danger);cursor:pointer;
}

.tabs-wrap{
  display:flex;overflow-x:auto;
  border-bottom:0.5px solid var(--color-border-tertiary);
  background:var(--color-background-secondary);
  gap:2px;padding:6px 10px 0;
}
.tab-btn{
  font-size:12px;padding:8px 14px;
  border-radius:8px 8px 0 0;
  border:0.5px solid transparent;
  background:transparent;color:var(--color-text-tertiary);
  cursor:pointer;white-space:nowrap;
  display:flex;align-items:center;gap:6px;
}
.tab-btn.active{
  background:var(--color-background-primary);
  color:var(--color-text-primary);font-weight:600;
  border-color:var(--color-border-tertiary);
  border-bottom-color: var(--color-background-primary);
}
.tab-badge{
  font-size:10px;padding:2px 6px;border-radius:10px;
  background:var(--color-background-tertiary);color:var(--color-text-tertiary);
}

.progress-bar-wrap{height:4px;background:var(--color-background-tertiary)}
.progress-bar{height:4px;background:#1D9E75;transition:width .4s}

.room-list{padding:16px;display:flex;flex-direction:column;gap:6px}
.sec-label{
  font-size:10px;font-weight:600;text-transform:uppercase;
  color:var(--color-text-tertiary);padding:10px 0 4px;
  display:flex;align-items:center;gap:8px;
}
.sec-label::after{content:'';flex:1;height:0.5px;background:var(--color-border-tertiary)}

.room-row{
  display:flex;align-items:center;gap:12px;
  padding:12px;
  border-radius:var(--border-radius-md);
  border:0.5px solid var(--color-border-tertiary);
  background:var(--color-background-primary);
}
.room-row.status-pending{ border-left:4px solid #BA7517; }
.room-row.status-delivered{ border-left:4px solid #185FA5; background:#F0F6FD; }
.room-row.status-collected{ border-left:4px solid #3B6D11; background:#F3F9EA; opacity: 0.8; }

.room-name{font-size:14px;font-weight:600;color:var(--color-text-primary);flex:1}
.row-actions{display:flex;gap:6px;align-items:center}

.btn-entregar{ font-size:11px; padding:6px 12px; border-radius:20px; border:1px solid #BA7517; background:#FAEEDA; color:#633806; cursor:pointer;}
.btn-retirar{ font-size:11px; padding:6px 12px; border-radius:20px; border:1px solid #185FA5; background:#E6F1FB; color:#0C447C; cursor:pointer;}
.btn-done{ font-size:11px; padding:6px 12px; border-radius:20px; border:1px solid #3B6D11; background:#EAF3DE; color:#27500A; cursor:default;}
.btn-undo{ font-size:10px; padding:4px 8px; border-radius:20px; border:0.5px solid var(--color-border-secondary); background:transparent; color:var(--color-text-tertiary); cursor:pointer;}

/* Modal Styles */
.modal-overlay{
  position: fixed; top:0; left:0; right:0; bottom:0;
  display:none; background:rgba(0,0,0,.5);
  padding:20px; z-index: 100;
  align-items:center; justify-content:center;
}
.modal-overlay.open{display:flex}
.modal-box{
  background:var(--color-background-primary);
  border-radius:var(--border-radius-lg);
  width:100%; max-width: 500px; max-height: 80vh; display: flex; flex-direction: column;
}
.modal-head{ padding:16px; border-bottom:0.5px solid var(--color-border-tertiary); display: flex; justify-content: space-between; align-items: center;}
.modal-body{ flex: 1; overflow-y:auto; padding:10px 0;}
.modal-foot{ padding:16px; border-top:0.5px solid var(--color-border-tertiary); font-size: 12px; color: var(--color-text-tertiary);}
.resumen-row { display: flex; justify-content: space-between; padding: 8px 16px; border-bottom: 0.5px solid #f0f0f0; font-size: 13px;}

.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0,0,0,0); border: 0; }
</style>
</head>
<body>

<h2 class="sr-only">Checklist de bandejas de comida</h2>

<div class="topbar">
  <div class="app-brand">
    <div class="app-icon">
      <svg viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect x="2" y="4" width="14" height="2" rx="1" fill="#0F6E56"/>
        <rect x="2" y="8" width="10" height="2" rx="1" fill="#1D9E75"/>
        <circle cx="14" cy="13" r="2.5" fill="#EAF3DE" stroke="#3B6D11" stroke-width="1"/>
      </svg>
    </div>
    <div>
      <div class="app-name">Bandejas · Turno</div>
      <div class="app-sub">Control de nutrición</div>
    </div>
  </div>
  <div class="topbar-right">
    <div class="turno-pill">
      <label>Turno</label>
      <select id="sel-turno">
          <option>Desayuno</option>
          <option selected>Almuerzo</option>
          <option>Merienda</option>
          <option>Cena</option>
      </select>
    </div>
    <div class="clock-pill" id="clock"></div>
  </div>
</div>

<div class="stats-row">
  <div class="stat-card">
    <div class="stat-dot pending"></div>
    <div class="stat-info">
      <span class="stat-num" id="cnt-p">0</span>
      <span class="stat-lbl">Pendientes</span>
    </div>
  </div>
  <div class="stat-card">
    <div class="stat-dot delivered"></div>
    <div class="stat-info">
      <span class="stat-num" id="cnt-d">0</span>
      <span class="stat-lbl">Entregadas</span>
    </div>
  </div>
  <div class="stat-card">
    <div class="stat-dot collected"></div>
    <div class="stat-info">
      <span class="stat-num" id="cnt-c">0</span>
      <span class="stat-lbl">Retiradas</span>
    </div>
  </div>
</div>

<div class="progress-bar-wrap"><div class="progress-bar" id="progress-bar"></div></div>

<div class="action-row">
  <input type="text" id="room-search" class="search-input" placeholder="Buscar habitación..." oninput="renderRooms()">
  <button class="btn-resumen" onclick="openResumen()">Resumen</button>
  <button class="btn-reset" onclick="resetTurno()">Reiniciar</button>
</div>

<div class="tabs-wrap" id="tabs-wrap"></div>

<div class="room-list" id="room-list"></div>

<div class="modal-overlay" id="modal-overlay">
  <div class="modal-box">
    <div class="modal-head">
      <div class="modal-title" style="font-weight:600" id="modal-title">Resumen del turno</div>
      <button onclick="closeResumen()" style="background:none; border:none; cursor:pointer; font-size:20px">&times;</button>
    </div>
    <div class="modal-body" id="modal-body"></div>
    <div class="modal-foot" id="modal-summary"></div>
  </div>
</div>

<script>
const SECTORS={
  "Primero A":["100 A","100 B","101 A","101 B","102A","102B","103 A","103B","104","105","106","107 A","107 B","108 A","108 B","109 A","109 B","110 A","110 B","111 A","111B"],
  "Primero B":["112 A","112 B","114 A","114 B","115 A","115B","116A","116 B","117 A","117 B","118A","118 B","119 A","119 B","119 C","119 D","119 E","119 F","120 A","120B","121 A","121 B","122 A","122 B","123 A","123 B"],
  "Select":["124","125","126","127","128","129","130","131","132","133","134","Suite 1","Suite 2","Suite 3","Suite 4","Suite 5","Suite 6","Suite 7","Suite 8"],
  "Premium":["UTIM 1","UTIM 2","UTIM 3","UTIM 4","200","201","202","203","204","205","206","207","208","209","210","211","212","214","215","216"],
  "Sector C":["217 A","217B","218 A","218 B","219A","219 B","220 A","220 B","221 A","221 B"],
  "UTIP":["UTIP 1","UTIP 2","UTIP 3","UTIP 4","UTIP 5","UTIP 6","UTIP 7","UTIP 8","UTIP 9","UTIP 10"],
  "Pedia":["PED 11","PED 12","PED 13","PED 14","PED 15","PED16","PED17","PED 18","PED19","PED 20","PED 21","PED 22 A","PED 22B","PED 23 A","PED 23B","PED 24"],
  "UTI":["UTI 1A","UTI 1B","UTI 2","UTI 3","UTI 4","UTI 5","UTI 6","UTI 7","UTI 8","UTI 9","UTI 10","UTI 11","UTI 12","UTI 14","UTI 15","UTI 16","UTI 17A","UTI 17B","UTI 18A","UTI 18B","UTI 18C","UTI 18D"],
  "Guardia":["Shock 2 Cama A","Shock 2 Cama B","Shock 2 Cama C","Shock 3 Cama A","Shock 3 Cama B","Shock 3 Cama C","Sala 2 Cama A","Sala 2 Cama B","Sala 2 Cama C","Sala 2 Cama D","Sala 1 Cama A","Sala 1 Cama B","Sala 1 Cama C","Sala 1 Cama D","Box 1","Box 2","Box 3"],
  "Guardia Pediátrica":["Shock Pedia A","Shock Pedia B","Shock Pedia C","Box 1","Box 2","Box 3","Triage","Sala 2D","Sala 1 (1)","Sala 1 (2)","Sala 1 (3)","Sala 1 (4)"]
};

let state={};
let currentSector=Object.keys(SECTORS)[0];

// PERSISTENCIA
function loadState() {
    const saved = localStorage.getItem('checklist_v3_state');
    if (saved) {
        state = JSON.parse(saved);
    } else {
        Object.keys(SECTORS).forEach(sec => {
            state[sec] = {};
            SECTORS[sec].forEach(h => { state[sec][h] = { status: 'pending', ts: null }; });
        });
    }
}

function saveState() {
    localStorage.setItem('checklist_v3_state', JSON.stringify(state));
}

function now(){const d=new Date();return d.getHours().toString().padStart(2,'0')+':'+d.getMinutes().toString().padStart(2,'0');}

function updateClock(){
  document.getElementById('clock').textContent=
    new Date().toLocaleDateString('es-AR',{weekday:'short',day:'numeric',month:'short'})+' · '+now();
}

function sectorCounts(sec){
  const rooms=Object.values(state[sec]);
  const p=rooms.filter(r=>r.status==='pending').length;
  const d=rooms.filter(r=>r.status==='delivered').length;
  const c=rooms.filter(r=>r.status==='collected').length;
  return{p,d,c};
}

function globalCounts(){
  let p=0,d=0,c=0,total=0;
  Object.keys(state).forEach(sec=>{
    Object.values(state[sec]).forEach(r=>{
        total++;
        if(r.status==='pending') p++;
        else if(r.status==='delivered') d++;
        else c++;
    });
  });
  return{p,d,c,total};
}

function renderTabs(){
  const wrap=document.getElementById('tabs-wrap');
  wrap.innerHTML=Object.keys(SECTORS).map(sec=>{
    const{p}=sectorCounts(sec);
    const cls='tab-btn'+(sec===currentSector?' active':'');
    const badge = p > 0 ? `<span class="tab-badge">${p}</span>` : '✅';
    return `<button class="${cls}" onclick="switchSector('${sec}')">${sec} ${badge}</button>`;
  }).join('');
}

function rowHTML(h,r){
  let btns='';
  if(r.status==='pending'){
    btns=`<button class="btn-entregar" onclick="setStatus('${h}','delivered')">Entregar</button>`;
  } else if(r.status==='delivered'){
    btns=`<button class="btn-retirar" onclick="setStatus('${h}','collected')">Retirar</button>
          <button class="btn-undo" onclick="setStatus('${h}','pending')">↩</button>`;
  } else {
    btns=`<span class="btn-done">Retirada</span>
          <button class="btn-undo" onclick="setStatus('${h}','delivered')">↩</button>`;
  }
  return `<div class="room-row status-${r.status}">
    <span class="room-name">${h}</span>
    <div class="row-actions">${btns} <span style="font-size:10px; color:gray">${r.ts||''}</span></div>
  </div>`;
}

function renderRooms(){
  const list=document.getElementById('room-list');
  const search = document.getElementById('room-search').value.toLowerCase();
  const rooms=state[currentSector];
  
  const filtered = Object.keys(rooms).filter(h => h.toLowerCase().includes(search));
  
  const pending = filtered.filter(h => rooms[h].status === 'pending');
  const delivered = filtered.filter(h => rooms[h].status === 'delivered');
  const collected = filtered.filter(h => rooms[h].status === 'collected');

  let html='';
  if(pending.length){ html+='<div class="sec-label">Pendientes</div>'; pending.forEach(h=>html+=rowHTML(h,rooms[h])); }
  if(delivered.length){ html+='<div class="sec-label">Para Retirar</div>'; delivered.forEach(h=>html+=rowHTML(h,rooms[h])); }
  if(collected.length){ html+='<div class="sec-label">Completas</div>'; collected.forEach(h=>html+=rowHTML(h,rooms[h])); }
  
  list.innerHTML=html || '<p style="text-align:center; padding:20px; color:gray">No se encontraron habitaciones</p>';
}

function renderStats(){
  const{p,d,c,total}=globalCounts();
  document.getElementById('cnt-p').textContent=p;
  document.getElementById('cnt-d').textContent=d;
  document.getElementById('cnt-c').textContent=c;
  const pct=total>0?Math.round(((d+c)/total)*100):0;
  document.getElementById('progress-bar').style.width=pct+'%';
}

function setStatus(h,status){
  state[currentSector][h].status=status;
  state[currentSector][h].ts=status==='pending'?null:now();
  saveState();
  renderAll();
}

function switchSector(sec){ currentSector=sec; renderAll(); }

function resetTurno(){
  if(!confirm('¿Reiniciar todos los datos del turno?')) return;
  localStorage.removeItem('checklist_v3_state');
  loadState();
  renderAll();
}

function renderAll(){ renderTabs(); renderRooms(); renderStats(); }

function openResumen(){
  const body = document.getElementById('modal-body');
  let html = '';
  Object.keys(state).forEach(sec => {
      html += `<div style="background:#f0f0f0; padding:5px 16px; font-size:11px; font-weight:bold">${sec}</div>`;
      Object.keys(state[sec]).forEach(h => {
          const r = state[sec][h];
          if(r.status !== 'pending') {
              html += `<div class="resumen-row"><span>${h}</span><span>${r.status === 'delivered' ? 'Entregada' : 'Retirada'} (${r.ts})</span></div>`;
          }
      });
  });
  body.innerHTML = html || '<p style="padding:20px">No hay actividad registrada</p>';
  document.getElementById('modal-overlay').classList.add('open');
}

function closeResumen(){ document.getElementById('modal-overlay').classList.remove('open'); }

// SERVICE WORKER (Optimizado para iOS/Local)
if ('serviceWorker' in navigator && window.location.protocol !== 'file:') {
  window.addEventListener('load', () => {
    const swCode = `
      self.addEventListener('install', e => self.skipWaiting());
      self.addEventListener('fetch', e => e.respondWith(fetch(e.request).catch(() => caches.match(e.request))));
    `;
    const blob = new Blob([swCode], { type: 'text/javascript' });
    navigator.serviceWorker.register(URL.createObjectURL(blob)).catch(err => console.log("SW local omitido"));
  });
}

// INICIALIZACIÓN CON PRUEBA DE ERRORES
try {
    updateClock();
    setInterval(updateClock, 30000);
    loadState();
    renderAll();
} catch (e) {
    alert("Error al cargar habitaciones: " + e.message);
}

// INIT
updateClock();
setInterval(updateClock,30000);
loadState();
renderAll();
</script>
</body>
</html>
