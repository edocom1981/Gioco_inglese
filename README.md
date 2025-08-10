# Gioco_inglese
Un gioco per imparare l’inglese
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
<title>English Fun — Mini Games</title>
<style>
  :root { --bg:#0b1020; --card:#121a32; --ink:#f5f7ff; --muted:#a9b3c9; --accent:#4fd1c5; --accent2:#8b5cf6; --good:#22c55e; --bad:#ef4444; }
  *{box-sizing:border-box} html,body{margin:0;height:100%;background:var(--bg);color:var(--ink);font-family: system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, "Apple Color Emoji", "Segoe UI Emoji"; }
  button{background:linear-gradient(180deg,var(--accent),#22a39a);color:#07121a;border:0;border-radius:14px;padding:14px 18px;font-weight:700;font-size:16px;cursor:pointer; box-shadow: 0 6px 18px rgba(79,209,197,.25); transition:transform .06s ease}
  button:active{transform:translateY(1px)}
  .ghost{background:transparent;border:1px solid #24314f;color:var(--ink);box-shadow:none}
  .accent2{background:linear-gradient(180deg,var(--accent2),#5b35cf); box-shadow:0 6px 18px rgba(139,92,246,.25)}
  .wrap{max-width:920px;margin:0 auto;padding:20px; padding-bottom:34vh}
  header{padding:24px 20px 8px; text-align:center}
  h1{margin:0;font-size:28px;letter-spacing:.3px}
  .sub{margin-top:6px;color:var(--muted);font-size:14px}
  .cards{display:grid;gap:14px; grid-template-columns: repeat( auto-fit, minmax(220px, 1fr) ); margin-top:16px}
  .card{background:linear-gradient(180deg, #121a32, #0e172e); border:1px solid #1d2745; border-radius:18px; padding:16px; display:flex; flex-direction:column; gap:10px; min-height:160px}
  .row{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
  .center{display:flex;align-items:center;justify-content:center;text-align:center}
  .big-emoji{font-size:56px; line-height:1.1}
  .pill{display:inline-flex;align-items:center;gap:6px;background:#0f1a34;border:1px solid #203159;border-radius:999px;padding:6px 10px;color:#c8d2ea;font-size:12px}
  .hidden{display:none !important}
  .stack{display:flex;flex-direction:column;gap:12px}
  .choices{display:grid;gap:10px; grid-template-columns: repeat(2, 1fr)}
  .badge{font-size:12px;color:#c6d0ea}
  .score{position:fixed; right:16px; top:12px; background:#0f1a34; border:1px solid #203159;border-radius:14px; padding:8px 12px; font-weight:700}
  .footer{position:fixed; left:0; right:0; bottom:0; padding:12px 16px; background:linear-gradient(180deg, rgba(11,16,32,0), rgba(11,16,32,.85) 35%, rgba(11,16,32,.95)); backdrop-filter: blur(6px); border-top:1px solid #172243}
  .footer .bar{max-width:920px;margin:0 auto;display:flex;gap:8px; flex-wrap:wrap; justify-content:center}
  .key{display:inline-flex;align-items:center; gap:8px; font-weight:700}
  .key .dot{width:10px;height:10px;border-radius:50%}
  .ok{color:var(--good)} .ko{color:var(--bad)}
  .progress{height:8px;background:#102042;border:1px solid #203159;border-radius:999px; overflow:hidden}
  .progress > div{height:100%; background: linear-gradient(90deg,var(--accent2), var(--accent)); width:0%}
  .tiny{font-size:12px;color:#93a0bf}
  .link{color:#96e7de;text-decoration:underline}
  .grid-rooms{display:grid; gap:10px; grid-template-columns: repeat(3, 1fr) }
  .room{aspect-ratio:1/1; display:flex; align-items:center; justify-content:center; background:#0f1a34; border:1px dashed #2a3b64; border-radius:12px; font-size:32px}
  .room.locked{opacity:.55}
  .note{font-size:14px;color:#b9c6e8}
  .balloon-area{position:relative; height:360px; background:#0f1a34; border:1px solid #22345c; border-radius:16px; overflow:hidden}
  .balloon{position:absolute; bottom:-80px; padding:10px 14px; border-radius:999px; background:#1b2b52; border:1px solid #2b4175; color:#dbe7ff; font-weight:700; box-shadow:0 8px 18px rgba(0,0,0,.25)}
  .tap{border:1px dashed #2b4175; padding:4px 8px; border-radius:8px}
  @media (min-width:700px){ .big-emoji{font-size:68px} }
</style>
</head>
<body>
  <div class="score" id="scoreBox">⭐ 0</div>
  <header>
    <h1>English Fun</h1>
    <div class="sub">3 mini-giochi per imparare vocaboli con audio e piccoli indovinelli.</div>
  </header>

  <div class="wrap">
    <!-- HOME -->
    <section id="home">
      <div class="cards">
        <div class="card">
          <div class="row" style="justify-content:space-between">
            <div class="pill">Game 1 · Flashcards + Quiz</div>
            <div class="badge" id="packName">Pack: Starter</div>
          </div>
          <div class="row" style="gap:16px; align-items:center">
            <div class="big-emoji">🍎🐶🚗🏠</div>
            <div class="stack" style="flex:1">
              <div>Ascolta la pronuncia e scegli la parola giusta.</div>
              <div class="progress"><div id="prog1"></div></div>
              <div class="tiny">Suggerimento: tocca 🔊 per riascoltare.</div>
              <div class="row">
                <button onclick="startGame('flash')">Play</button>
                <button class="ghost" onclick="togglePack()">Cambia pack</button>
              </div>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="pill">Game 2 · Treasure Hunt</div>
          <div class="stack">
            <div class="note">Leggi l’indovinello in inglese e trova l’oggetto giusto per la chiave!</div>
            <div class="grid-rooms" id="roomsPreview">
              <div class="room">🍎</div><div class="room locked">🔒</div><div class="room locked">🔒</div>
            </div>
            <div class="row">
              <button class="accent2" onclick="startGame('treasure')">Play</button>
              <button class="ghost" onclick="howTo('treasure')">Come si gioca?</button>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="pill">Game 3 · Word Pop</div>
          <div class="stack">
            <div class="note">Tocca solo i palloncini con la parola bersaglio (tempo limitato).</div>
            <div class="balloon-area" id="balloonPreview"><div class="balloon" style="left:20px; bottom:8px">apple</div></div>
            <div class="row">
              <button onclick="startGame('pop')">Play</button>
              <button class="ghost" onclick="howTo('pop')">Come si gioca?</button>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="pill">Parent · Personalizza</div>
          <div class="stack">
            <div class="note">Aggiungi/edita parole e frasi. I progressi restano nel dispositivo.</div>
            <div class="row">
              <button onclick="openEditor()">Apri editor</button>
              <button class="ghost" onclick="resetProgress()">Azzera progressi</button>
            </div>
          </div>
        </div>

      </div>
    </section>

    <!-- GAME AREA -->
    <section id="game" class="hidden">
      <div class="card" id="gameCard">
        <div class="row" style="justify-content:space-between; align-items:center">
          <button class="ghost" onclick="backHome()">← Indietro</button>
          <div class="pill" id="gameTitle">Game</div>
          <button onclick="speakCurrent()">🔊 Pronuncia</button>
        </div>
        <div id="gameContent" class="stack"></div>
      </div>
    </section>

    <!-- EDITOR -->
    <section id="editor" class="hidden">
      <div class="card">
        <div class="row" style="justify-content:space-between">
          <div class="pill">Editor vocabolario</div>
          <button class="ghost" onclick="backHome()">Chiudi</button>
        </div>
        <div class="stack">
          <div class="tiny">Formato: emoji, parola inglese, traduzione italiana (opzionale)</div>
          <textarea id="editorText" rows="8" style="width:100%;background:#0d1630;color:#e9efff;border:1px solid #24345f;border-radius:12px;padding:12px;font-family: ui-monospace,SFMono-Regular,Menlo,monospace"></textarea>
          <div class="row" style="justify-content:space-between">
            <button onclick="saveEditor()">Salva</button>
            <span class="tiny">Consiglio: poche parole per volta (8-12) aiutano la memorizzazione.</span>
          </div>
        </div>
      </div>
    </section>

  </div>

  <div class="footer">
    <div class="bar">
      <span class="key"><span class="dot" style="background:var(--good)"></span> Corrette</span>
      <span class="key"><span class="dot" style="background:var(--bad)"></span> Errori</span>
      <span class="key"><span class="dot" style="background:var(--accent)"></span> Progress</span>
      <span class="tiny">Tocca 🔊 per ascoltare. Funziona offline. Dati salvati localmente.</span>
    </div>
  </div>

<script>
/* ========= DATA ========= */
const defaultPacks = {
  "Starter":[
    {emoji:"🍎", en:"apple", it:"mela"},
    {emoji:"🐶", en:"dog", it:"cane"},
    {emoji:"🚗", en:"car", it:"auto"},
    {emoji:"🏠", en:"house", it:"casa"},
    {emoji:"🧃", en:"juice", it:"succo"},
    {emoji:"🧸", en:"teddy", it:"orsacchiotto"},
    {emoji:"📚", en:"book", it:"libro"},
    {emoji:"🧦", en:"socks", it:"calzini"},
  ],
  "Food":[
    {emoji:"🍞", en:"bread", it:"pane"},
    {emoji:"🧀", en:"cheese", it:"formaggio"},
    {emoji:"🍌", en:"banana", it:"banana"},
    {emoji:"🍕", en:"pizza", it:"pizza"},
    {emoji:"🍓", en:"strawberry", it:"fragola"},
    {emoji:"🥛", en:"milk", it:"latte"},
    {emoji:"🥚", en:"egg", it:"uovo"},
    {emoji:"🥕", en:"carrot", it:"carota"},
  ]
};

const RIDDLES = [
  { clue:"I have pages but I'm not a tree. What am I?", answer:"book", emoji:"📚" },
  { clue:"You can sleep in me. What am I?", answer:"house", emoji:"🏠" },
  { clue:"I can bark but I'm not a tree. What am I?", answer:"dog", emoji:"🐶" },
  { clue:"I'm sweet and red. What am I?", answer:"apple", emoji:"🍎" },
  { clue:"You drink me with breakfast. What am I?", answer:"milk", emoji:"🥛" },
];

let packs = loadPacks();
let currentPackName = Object.keys(packs)[0] || "Starter";
let currentPack = packs[currentPackName];
let score = Number(localStorage.getItem("ef_score")||0);
let progress = JSON.parse(localStorage.getItem("ef_progress")||"{}"); // {pack: {done:int, total:int}}
updateScore(0); // init
updatePackName();
updatePreviews();

/* ========= UTIL ========= */
function $(id){ return document.getElementById(id); }
function shuffle(a){ for(let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]] } return a; }
function speak(text){
  try{
    const u = new SpeechSynthesisUtterance(text);
    u.lang = 'en-US';
    u.rate = 0.95; u.pitch = 1.0;
    speechSynthesis.cancel(); speechSynthesis.speak(u);
  }catch(e){ /* ignore */ }
}
function vibrate(ms=30){ if (window.navigator.vibrate) navigator.vibrate(ms); }
function updateScore(delta){ score = Math.max(0, score + delta); localStorage.setItem("ef_score", String(score)); $("scoreBox").textContent = `⭐ ${score}`; }
function setProgress(key, done, total){ progress[key]={done,total}; localStorage.setItem("ef_progress", JSON.stringify(progress)); const pct = total? Math.min(100, Math.round((done/total)*100)) : 0; $("prog1").style.width = pct+"%"; }
function updatePackName(){ $("packName").textContent = "Pack: " + currentPackName; }
function updatePreviews(){
  const rp = $("roomsPreview"); rp.innerHTML = "";
  for(let i=0;i<3;i++){
    const div = document.createElement("div");
    div.className = "room" + (i>0? " locked": "");
    div.textContent = ["🍎","🐶","🚗"][i]||"🔒";
    rp.appendChild(div);
  }
  const bp = $("balloonPreview");
  bp.innerHTML = '<div class="balloon" style="left:24px; bottom:8px">apple</div>';
}

/* ========= NAV ========= */
function startGame(kind){
  $("home").classList.add("hidden");
  $("editor").classList.add("hidden");
  $("game").classList.remove("hidden");
  $("gameContent").innerHTML = "";
  $("gameTitle").textContent = kind==="flash" ? "Flashcards + Quiz" : (kind==="treasure" ? "Treasure Hunt" : "Word Pop");
  if (kind==="flash") gameFlash();
  else if (kind==="treasure") gameTreasure();
  else gamePop();
}
function backHome(){
  $("game").classList.add("hidden");
  $("editor").classList.add("hidden");
  $("home").classList.remove("hidden");
}
function howTo(kind){
  alert(kind==="treasure" ? "Leggi l’indovinello (in inglese) e tocca l’oggetto giusto nella griglia. Ogni risposta corretta sblocca una stanza e ti dà una chiave."
                          : "Tocca solo i palloncini con la PAROLA bersaglio. Attenzione al tempo!");
}

/* ========= GAME 1: FLASHCARDS + QUIZ ========= */
function gameFlash(){
  const pack = [...currentPack];
  const total = pack.length;
  let idx = 0, correct=0;

  const cont = $("gameContent");
  const top = document.createElement("div");
  top.className="row"; top.style.justifyContent="space-between";
  top.innerHTML = `<div class="badge">Words: ${total}</div><div class="badge">Correct: <span id="okF">0</span></div>`;
  cont.appendChild(top);

  const card = document.createElement("div");
  card.className = "center stack";
  cont.appendChild(card);

  function step(){
    setProgress(currentPackName, idx, total);
    if(idx>=total){
      card.innerHTML = `<div class="big-emoji">🎉</div><div>Great job! Score +${correct}</div><button onclick="backHome()">Fine</button>`;
      updateScore(correct);
      return;
    }
    const item = pack[idx];
    card.innerHTML = `
      <div class="big-emoji" aria-label="${item.en}">${item.emoji}</div>
      <div class="row" style="gap:8px; justify-content:center"><button onclick="speak('${item.en}')">🔊 Listen</button><span class="tiny">${item.it?("("+item.it+")"):""}</span></div>
      <div class="choices" id="choices"></div>
    `;
    speak(item.en);
    const distractors = shuffle(currentPack.filter(x=>x.en!==item.en)).slice(0,3).map(x=>x.en);
    const options = shuffle([item.en, ...distractors]);
    const box = card.querySelector("#choices");
    options.forEach(opt=>{
      const b = document.createElement("button");
      b.textContent = opt;
      b.onclick = ()=>{
        if(opt===item.en){
          vibrate(15); b.style.background = `linear-gradient(180deg, var(--good), #138a40)`; correct++; $("okF").textContent = correct;
        }else{
          vibrate(60); b.style.background = `linear-gradient(180deg, var(--bad), #a32020)`; 
        }
        setTimeout(()=>{ idx++; step(); }, 420);
      };
      box.appendChild(b);
    });
  }
  step();
}
function speakCurrent(){
  const title = $("gameTitle").textContent;
  if(title.includes("Flashcards")){
    // try to find last spoken? fallback: say "Listen"
    speak("Listen and choose the correct word.");
  }else if(title.includes("Treasure")){
    speak("Solve the riddle and tap the correct object.");
  }else{
    speak("Pop the balloons with the target word.");
  }
}

/* ========= GAME 2: TREASURE HUNT ========= */
function gameTreasure(){
  const cont = $("gameContent");
  let level = 0, keys=0;
  cont.innerHTML = `
    <div class="stack">
      <div class="note" id="clueBox"></div>
      <div class="grid-rooms" id="rooms"></div>
      <div class="row" style="justify-content:space-between">
        <div class="pill">Keys: <span id="keys">0</span> / ${RIDDLES.length}</div>
        <div class="pill">Level <span id="lvl">1</span></div>
      </div>
    </div>
  `;
  const roomsBox = $("rooms");
  function render(){
    const r = RIDDLES[level];
    $("clueBox").textContent = r.clue;
    speak(r.clue);
    roomsBox.innerHTML = "";
    const candidates = shuffle([...new Set([r.answer, ...shuffle(Object.values(packs).flat().map(x=>x.en)).slice(0,5)])]).slice(0,6);
    candidates.forEach(word=>{
      const em = findEmoji(word) || "❓";
      const d = document.createElement("div");
      d.className = "room";
      d.innerHTML = `<div>${em}</div>`;
      d.title = word;
      d.onclick = ()=>{
        if(word===r.answer){
          d.style.borderColor = "#2bd67b"; d.style.background = "#113024";
          keys++; updateScore(2);
          $("keys").textContent = keys;
          level++;
          $("lvl").textContent = Math.min(level+1, RIDDLES.length);
          if(level>=RIDDLES.length){
            $("clueBox").innerHTML = "You found all keys! 🎉";
          }else{
            setTimeout(render, 450);
          }
        }else{
          d.style.borderColor = "#a82828"; d.style.background = "#2b1420"; vibrate(60);
        }
      };
      roomsBox.appendChild(d);
    });
  }
  render();
}

/* ========= GAME 3: WORD POP ========= */
function gamePop(){
  const cont = $("gameContent");
  cont.innerHTML = `
    <div class="stack">
      <div class="row" style="justify-content:space-between; align-items:center">
        <div class="pill">Target: <strong id="targetWord">apple</strong></div>
        <div class="pill">Time: <span id="time">30</span>s</div>
      </div>
      <div class="balloon-area" id="area"></div>
      <div class="row" style="justify-content:space-between">
        <div class="pill">Hits: <span id="hits">0</span></div>
        <div class="pill">Miss: <span id="miss">0</span></div>
      </div>
    </div>
  `;
  const area = $("area");
  const words = shuffle(currentPack.map(x=>x.en));
  let target = words[0] || "apple";
  $("targetWord").textContent = target;
  let t = 30, hits = 0, miss = 0;
  const timer = setInterval(()=>{
    t--; $("time").textContent = t;
    if(t<=0){ clearInterval(timer); end(); }
  }, 1000);

  const spawner = setInterval(()=>{
    spawnBalloon();
    if(Math.random()<0.35) spawnBalloon();
  }, 750);

  function spawnBalloon(){
    const b = document.createElement("div");
    b.className = "balloon";
    const vocab = Math.random()<0.55 ? target : (words[Math.floor(Math.random()*words.length)]||target);
    b.textContent = vocab;
    const left = Math.floor(Math.random()*(area.clientWidth-90));
    b.style.left = left+"px";
    area.appendChild(b);
    let y=-80;
    const speed = 1 + Math.random()*1.6;
    const anim = setInterval(()=>{
      y += (2.2*speed);
      b.style.bottom = y + "px";
      if(y> area.clientHeight + 80){
        clearInterval(anim); area.removeChild(b);
      }
    }, 16);
    b.onclick = ()=>{
      if(vocab===target){
        b.style.background = "#174028"; b.style.borderColor="#2bd67b"; hits++; $("hits").textContent = hits; updateScore(1); vibrate(18);
      }else{
        b.style.background = "#3b1521"; b.style.borderColor="#c03939"; miss++; $("miss").textContent = miss; vibrate(60);
      }
      setTimeout(()=>{ try{ area.removeChild(b);}catch(e){} }, 60);
    };
  }

  function end(){
    clearInterval(spawner);
    area.innerHTML = "";
    const bonus = Math.max(0, hits*2 - miss);
    const wrap = document.createElement("div");
    wrap.className="center stack";
    wrap.innerHTML = `<div class="big-emoji">🎯</div>
      <div>Hits: ${hits} · Miss: ${miss}</div>
      <div>Bonus score: +${bonus}</div>
      <button onclick="backHome()">Fine</button>`;
    updateScore(bonus);
    cont.appendChild(wrap);
  }
}

/* ========= PACKS / EDITOR ========= */
function openEditor(){
  $("home").classList.add("hidden"); $("editor").classList.remove("hidden");
  const rows = [];
  for(const [name, list] of Object.entries(packs)){
    rows.push(`# ${name}`);
    list.forEach(it=>rows.push(`${it.emoji}, ${it.en}, ${it.it||""}`));
    rows.push("");
  }
  $("editorText").value = rows.join("\n");
}
function saveEditor(){
  const txt = $("editorText").value;
  const lines = txt.split(/\r?\n/);
  const newPacks = {};
  let current = "Custom";
  newPacks[current]=[];
  for(const line of lines){
    if(line.trim().startsWith("#")){
      current = line.replace("#","").trim() || "Custom";
      newPacks[current]=[];
      continue;
    }
    const m = line.split(",").map(s=>s&&s.trim()).filter(Boolean);
    if(m.length>=2){
      newPacks[current].push({emoji:m[0], en:m[1].toLowerCase(), it:m[2]||""});
    }
  }
  packs = newPacks;
  localStorage.setItem("ef_packs", JSON.stringify(packs));
  currentPackName = Object.keys(packs)[0];
  currentPack = packs[currentPackName];
  updatePackName(); updatePreviews();
  alert("Salvato! Torno alla Home.");
  backHome();
}
function loadPacks(){
  const raw = localStorage.getItem("ef_packs");
  if(!raw) return JSON.parse(JSON.stringify(defaultPacks));
  try{ return JSON.parse(raw); }catch(e){ return JSON.parse(JSON.stringify(defaultPacks)); }
}
function togglePack(){
  const names = Object.keys(packs);
  let i = names.indexOf(currentPackName);
  i = (i+1) % names.length;
  currentPackName = names[i];
  currentPack = packs[currentPackName];
  updatePackName(); updatePreviews();
}

/* ========= HELPERS ========= */
function findEmoji(word){
  for(const list of Object.values(packs)){
    const f = list.find(x=>x.en===word);
    if(f) return f.emoji;
  }
  const map = {apple:"🍎", dog:"🐶", car:"🚗", house:"🏠", milk:"🥛", book:"📚", juice:"🧃", socks:"🧦", bread:"🍞", cheese:"🧀", banana:"🍌", pizza:"🍕", strawberry:"🍓", egg:"🥚", carrot:"🥕"};
  return map[word];
}
function resetProgress(){
  if(!confirm("Sicuro di azzerare punteggio e progressi?")) return;
  localStorage.removeItem("ef_score"); localStorage.removeItem("ef_progress");
  updateScore(-score); setProgress(currentPackName,0,(currentPack||[]).length);
  alert("Azzera eseguito.");
}
</script>
</body>
</html>
