# index.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="icon" type="image/svg+xml" href="icon.svg">
<link rel="apple-touch-icon" href="icon.svg">
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#052e16">
<meta name="description" content="Blackjack-powered workout game. Every card deals a real exercise — beat the dealer to 21 without busting into planks.">
<title>BlacJack Fitness — Beat the Deck, Build the Body</title>
<style>
  *{margin:0;padding:0;box-sizing:border-box}
  body{font-family:'Segoe UI',system-ui,sans-serif;background:#0a1f12;color:#f8fafc;min-height:100vh;
    background-image:radial-gradient(ellipse at 50% -20%,#14532d 0%,#052e16 55%,#031a0e 100%)}
  header{text-align:center;padding:22px 16px 6px}
  h1{font-size:clamp(1.7rem,4.5vw,2.8rem);letter-spacing:5px;color:#fbbf24;text-shadow:0 2px 14px rgba(251,191,36,.35)}
  h1 .sp{color:#ef4444}
  .tagline{color:#a7f3d0;font-size:.92rem;margin-top:4px}
  .icon-btn{background:#00000055;border:1px solid #ffffff22;color:#fbbf24;border-radius:10px;
    padding:7px 13px;cursor:pointer;font-size:1rem;margin-left:6px}
  .icon-btn:hover{background:#ffffff18}
  .stats-bar{display:flex;gap:8px;justify-content:center;flex-wrap:wrap;margin:12px auto 0;max-width:900px}
  .pill{background:#00000040;border:1px solid #fbbf2444;border-radius:999px;padding:5px 14px;
    font-size:.8rem;color:#fde68a;white-space:nowrap}
  .pill b{color:#fff}
  .layout{max-width:1150px;margin:16px auto;padding:0 12px;display:grid;grid-template-columns:1fr 300px;gap:16px}
  @media(max-width:900px){.layout{grid-template-columns:1fr}}
  .felt{border-radius:26px;border:10px solid #78350f;outline:3px solid #b45309;padding:22px 18px;
    background:radial-gradient(ellipse at center,#166534,#14532d 55%,#0d3a20);
    box-shadow:inset 0 0 70px rgba(0,0,0,.5),0 24px 50px rgba(0,0,0,.55)}
  .seat-label{font-size:.68rem;letter-spacing:3px;text-transform:uppercase;color:#86efac;text-align:center;margin-bottom:8px}
  .hand-row{display:flex;flex-wrap:wrap;gap:12px;justify-content:center;min-height:140px;align-items:center}
  .hand-total{text-align:center;margin-top:8px;font-size:.95rem;color:#d1fae5}
  .hand-total b{font-size:1.35rem;color:#fbbf24;font-variant-numeric:tabular-nums}
  .card{width:88px;height:126px;background:linear-gradient(160deg,#fff,#f1f5f9);border-radius:11px;position:relative;
    box-shadow:0 8px 16px rgba(0,0,0,.45);animation:dealIn .45s cubic-bezier(.2,.9,.3,1.25) backwards;
    display:flex;flex-direction:column;align-items:center;justify-content:center;flex-shrink:0}
  .card.red{color:#dc2626}.card.black{color:#111827}
  .card .rank{font-size:1.9rem;font-weight:800;line-height:1}
  .card .suit-big{font-size:2.1rem;line-height:1.2}
  .card .corner{position:absolute;top:5px;left:7px;font-size:.78rem;font-weight:800;line-height:1.1;text-align:center}
  .card .corner.br{top:auto;left:auto;bottom:5px;right:7px;transform:rotate(180deg)}
  .card .ex{position:absolute;bottom:6px;width:100%;text-align:center;font-size:.5rem;font-weight:800;
    letter-spacing:.4px;text-transform:uppercase;opacity:.65;padding:0 4px}
  .hole{background:repeating-linear-gradient(45deg,#7f1d1d,#7f1d1d 8px,#991b1b 8px,#991b1b 16px);
    border:4px solid #fbbf24;display:flex;align-items:center;justify-content:center}
  .hole::after{content:"♠";font-size:2.6rem;color:#fbbf2455}
  @keyframes dealIn{from{transform:translateY(-46px) rotateY(90deg) scale(.6);opacity:0}}
  .msg{text-align:center;font-size:1.08rem;font-weight:700;min-height:56px;color:#fef3c7;
    margin:14px 0;display:flex;align-items:center;justify-content:center;padding:0 8px}
  .msg.win{color:#4ade80}.msg.lose{color:#fca5a5}.msg.bj{color:#fbbf24;animation:pulse .7s infinite alternate}
  @keyframes pulse{from{transform:scale(1)}to{transform:scale(1.07)}}
  .controls{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;min-height:52px;align-items:center}
  .btn{font-family:inherit;font-weight:800;letter-spacing:1px;cursor:pointer;border:none;border-radius:12px;
    padding:12px 26px;font-size:.95rem;text-transform:uppercase;transition:transform .12s;box-shadow:0 5px 0 rgba(0,0,0,.4)}
  .btn:hover:not(:disabled){transform:translateY(-2px)}
  .btn:active:not(:disabled){transform:translateY(3px);box-shadow:none}
  .btn:disabled{opacity:.3;cursor:not-allowed}
  .gold{background:#fbbf24;color:#451a03}.green{background:#22c55e;color:#052e16}
  .red{background:#ef4444;color:#fff}.blue{background:#38bdf8;color:#082f49}
  .bet-area{display:none;flex-direction:column;align-items:center;gap:12px}
  .bet-area.show{display:flex}
  .chip-row{display:flex;gap:10px;flex-wrap:wrap;justify-content:center}
  .poker-chip{width:58px;height:58px;border-radius:50%;cursor:pointer;font-weight:900;font-size:.85rem;
    color:#fff;display:flex;align-items:center;justify-content:center;border:5px dashed rgba(255,255,255,.85);
    box-shadow:0 4px 10px rgba(0,0,0,.4);transition:transform .12s}
  .poker-chip:hover{transform:scale(1.12)}
  .c10{background:#0284c7}.c25{background:#16a34a}.c50{background:#dc2626}.c100{background:#1f2937}
  .bet-display{font-size:1rem}.bet-display b{color:#fbbf24;font-size:1.4rem}
  .clear-bet{background:none;border:none;color:#94a3b8;cursor:pointer;font-size:.8rem;text-decoration:underline}
  aside{display:flex;flex-direction:column;gap:12px}
  .panel{background:#00000059;border:1px solid #ffffff14;border-radius:16px;padding:14px}
  .panel h3{color:#fbbf24;font-size:.72rem;letter-spacing:2px;text-transform:uppercase;margin-bottom:9px}
  .legend li{list-style:none;font-size:.85rem;padding:3px 0;display:flex;justify-content:space-between;color:#e2e8f0}
  .legend span:first-child{font-weight:700}
  .ach{display:flex;gap:10px;align-items:center;padding:6px 0;font-size:.82rem;color:#64748b}
  .ach.unlocked{color:#fde68a}
  .ach .ico{font-size:1.3rem;filter:grayscale(1);opacity:.4}
  .ach.unlocked .ico{filter:none;opacity:1}
  #historyLog{max-height:190px;overflow-y:auto;font-size:.8rem}
  #historyLog div{padding:5px 6px;border-bottom:1px dashed #ffffff1c;color:#cbd5e1}
  .timer-panel{text-align:center}
  .t-presets{display:flex;gap:6px;justify-content:center;margin-bottom:8px}
  .t-presets button,.t-go button{background:#334155;color:#fff;border:none;border-radius:8px;
    padding:6px 12px;cursor:pointer;font-weight:700;font-size:.78rem}
  .t-presets button.active{background:#fbbf24;color:#451a03}
  #tDisp{font-size:2.3rem;font-weight:800;color:#93c5fd;font-variant-numeric:tabular-nums;margin:4px 0}
  .t-go{display:flex;gap:8px;justify-content:center}
  .overlay{position:fixed;inset:0;background:#000a;z-index:400;display:none;align-items:center;justify-content:center;padding:16px}
  .overlay.open{display:flex}
  .modal{background:linear-gradient(170deg,#14532d,#052e16);border:2px solid #fbbf2455;border-radius:20px;
    max-width:520px;width:100%;padding:26px;max-height:88vh;overflow-y:auto;box-shadow:0 30px 80px #000}
  .modal h2{color:#fbbf24;text-align:center;margin-bottom:14px;font-size:1.3rem}
  .sum-line{display:flex;justify-content:space-between;padding:7px 4px;border-bottom:1px dashed #ffffff22;font-size:.92rem}
  .sum-line b{color:#4ade80}
  .pen{color:#fca5a5!important}
  .modal .btn{width:100%;margin-top:16px}
  .set-row{padding:10px 0;border-bottom:1px dashed #ffffff22}
  .set-row label{display:block;font-size:.85rem;margin-bottom:7px;color:#e2e8f0}
  select{background:#1e293b;color:#fff;border:1px solid #334155;border-radius:8px;padding:8px;font-family:inherit;width:100%}
  .close-x{float:right;background:none;border:none;color:#94a3b8;font-size:1.3rem;cursor:pointer}
  #toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(100px);background:#111827;
    border:1px solid #fbbf24;color:#fbbf24;padding:12px 26px;border-radius:999px;font-weight:700;z-index:600;
    transition:transform .3s;font-size:.9rem}
  #toast.show{transform:translateX(-50%) translateY(0)}
  footer{text-align:center;padding:24px;color:#86efac77;font-size:.8rem;line-height:1.7}
</style>
</head>
<body>

<header>
  <div>
    <h1>BLAC<span class="sp">♠</span>JACK FITNESS</h1>
    <button class="icon-btn" onclick="openModal('settingsModal')">⚙️</button>
    <button class="icon-btn" id="sndBtn" onclick="toggleSound()">🔊</button>
  </div>
  <div class="tagline">Your hand IS your workout • Beat the dealer, earn your reps</div>
</header>

<div class="stats-bar">
  <div class="pill">💰 Chips: <b id="sChips">500</b></div>
  <div class="pill">🔥 Streak: <b id="sStreak">0</b>d</div>
  <div class="pill">🎯 Hands: <b id="sRounds">0</b></div>
  <div class="pill">🏆 Blackjacks: <b id="sBJ">0</b></div>
  <div class="pill">💪 Lifetime Reps: <b id="sReps">0</b></div>
</div>
<div style="text-align:center;margin-top:10px">
  <button id="installBtn" class="btn gold" style="display:none">📲 Install App</button>
</div>

<div class="layout">
  <div class="felt">
    <div class="seat-label">Dealer — The House</div>
    <div class="hand-row" id="dealerCards"></div>
    <div class="hand-total">DEALER: <b id="dTotal">–</b></div>

    <div class="msg" id="msg">Set your bet to begin.</div>

    <div class="hand-row" id="playerCards"></div>
    <div class="hand-total">YOU: <b id="pTotal">–</b></div>

    <div class="bet-area show" id="betArea" style="margin-top:16px">
      <div class="chip-row">
        <div class="poker-chip c10" onclick="addBet(10)">10</div>
        <div class="poker-chip c25" onclick="addBet(25)">25</div>
        <div class="poker-chip c50" onclick="addBet(50)">50</div>
        <div class="poker-chip c100" onclick="addBet(100)">100</div>
      </div>
      <div class="bet-display">Bet: <b id="betAmt">0</b></div>
      <div>
        <button class="btn gold" onclick="deal()" disabled id="dealBtn">Deal Cards</button>
        <button class="clear-bet" onclick="resetBet()">clear</button>
      </div>
      <button class="btn blue" id="loanBtn" style="display:none" onclick="takeLoan()">💪 Do 50 Jumping Jacks → Loan +200 chips</button>
    </div>

    <div class="controls" id="playControls" style="margin-top:16px;display:none">
      <button class="btn green" id="hitBtn" onclick="hit()">Hit Me</button>
      <button class="btn red" id="standBtn" onclick="stand()">Stand</button>
      <button class="btn blue" id="doubleBtn" onclick="doubleDown()">Double Down</button>
    </div>

    <div class="controls" id="nextControls" style="margin-top:16px;display:none">
      <button class="btn gold" onclick="newRound()">Next Hand</button>
    </div>
  </div>

  <aside>
    <div class="panel timer-panel">
      <h3>⏱️ Rest Timer</h3>
      <div class="t-presets">
        <button data-t="30" onclick="setPreset(30,this)">30s</button>
        <button data-t="60" class="active" onclick="setPreset(60,this)">60s</button>
        <button data-t="90" onclick="setPreset(90,this)">90s</button>
        <button data-t="120" onclick="setPreset(120,this)">2m</button>
      </div>
      <div id="tDisp">01:00</div>
      <div class="t-go">
        <button onclick="startTimer()">▶ Start</button>
        <button onclick="pauseTimer()">⏸ Pause</button>
        <button onclick="resetTimer()">↺ Reset</button>
      </div>
    </div>

    <div class="panel">
      <h3>Suit Legend</h3>
      <ul class="legend">
        <li><span style="color:#f87171">♥ Hearts</span><span>Push-Ups</span></li>
        <li><span style="color:#f87171">♦ Diamonds</span><span>Squats</span></li>
        <li><span>♣ Clubs</span><span>Crunches</span></li>
        <li><span>♠ Spades</span><span>Burpees</span></li>
        <li><span>Ace</span><span>11 (mercy: 1)</span></li>
        <li><span>J · Q · K</span><span>10</span></li>
      </ul>
    </div>

    <div class="panel"><h3>🏅 Achievements</h3><div id="achList"></div></div>
    <div class="panel"><h3>📜 History</h3><div id="historyLog"><div style="opacity:.5">No hands yet…</div></div></div>
  </aside>
</div>

<div class="overlay" id="summaryOverlay">
  <div class="modal">
    <h2 id="sumTitle">Hand Complete</h2>
    <div id="sumBody"></div>
    <button class="btn gold" onclick="closeSummary()">Collect & Continue</button>
  </div>
</div>

<div class="overlay" id="settingsModal">
  <div class="modal">
    <button class="close-x" onclick="closeModal('settingsModal')">✕</button>
    <h2>⚙️ Settings</h2>
    <div class="set-row">
      <label>Workout Intensity — sets per card:</label>
      <select id="intensitySel">
        <option value="1">🟢 Athlete — 1 set per card</option>
        <option value="2">🟡 Beast — 2 sets per card</option>
        <option value="3">🔴 Savage — 3 sets per card</option>
      </select>
    </div>
    <div class="set-row">
      <label>Starting Bankroll (on next reset):</label>
      <select id="bankSel"><option>300</option><option selected>500</option><option>1000</option></select>
    </div>
    <div class="set-row">
      <label>Danger Zone</label>
      <button class="btn red" style="width:100%" onclick="wipeAll()">Reset ALL Progress</button>
    </div>
    <button class="btn gold" onclick="applySettings();closeModal('settingsModal')">Save Settings</button>
  </div>
</div>

<div id="toast"></div>

<footer>
  Keyboard: <b>H</b> Hit · <b>S</b> Stand · <b>D</b> Double · <b>N</b> Next Hand<br>
  Play responsibly. Consult a physician before starting any fitness program. © 2025 BlacJack Fitness
</footer>

<script>
const EX={'♥':'Push-Ups','♦':'Squats','♣':'Crunches','♠':'Burpees'};
const ACH_DEFS=[
 {id:'first',ico:'🥊',name:'First Blood',desc:'Play your first hand'},
 {id:'bj',   ico:'🂡',name:'Perfect 21', desc:'Hit a natural blackjack'},
 {id:'bj10', ico:'🎩',name:'Card Shark', desc:'10 career blackjacks'},
 {id:'k',    ico:'💪',name:'Iron Will',  desc:'Stand on 20+'},
 {id:'k1',   ico:'🏃',name:'Marathon',   desc:'1,000 lifetime reps'},
 {id:'k5',   ico:'🔥',name:'Machine',    desc:'5,000 lifetime reps'},
 {id:'rich', ico:'💎',name:'High Roller',desc:'Hold 1,000 chips'},
 {id:'week', ico:'📅',name:'Consistent', desc:'7-day streak'}];
let S=JSON.parse(localStorage.getItem('bjf_v2')||'null')||{
 chips:500,streak:0,lastDay:'',rounds:0,blackjacks:0,reps:0,wins:0,losses:0,
 ach:{},history:[],sound:true,intensity:1,bank:500};
let shoe=[],player=[],dealer=[],phase='bet',bet=0,holeHidden=true,busy=false;

const $=id=>document.getElementById(id);
const sleep=ms=>new Promise(r=>setTimeout(r,ms));
function save(){localStorage.setItem('bjf_v2',JSON.stringify(S));refreshStats();}
function toast(m){const t=$('toast');t.textContent=m;t.classList.add('show');
 clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('show'),2400);}

let AC=null;
function beep(f,d,type,vol){f=f||440;d=d||.12;type=type||'sine';vol=(vol===undefined)?.15:vol;
 if(!S.sound)return;
 try{AC=AC||new(window.AudioContext||window.webkitAudioContext)();
  const o=AC.createOscillator(),g=AC.createGain();
  o.type=type;o.frequency.value=f;o.connect(g);g.connect(AC.destination);
  g.gain.setValueAtTime(vol,AC.currentTime);
  g.gain.exponentialRampToValueAtTime(.001,AC.currentTime+d);
  o.start();o.stop(AC.currentTime+d);}catch(e){}}
function toggleSound(){S.sound=!S.sound;$('sndBtn').textContent=S.sound?'🔊':'🔇';save();beep(600);}
function chord(fs){fs.forEach((f,i)=>setTimeout(()=>beep(f,.25,'triangle'),i*90));}

function buildShoe(){shoe=[];
 for(let d=0;d<2;d++)for(const s of['♥','♦','♣','♠'])for(let r=1;r<=13;r++)shoe.push({r,s});
 for(let i=shoe.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[shoe[i],shoe[j]]=[shoe[j],shoe[i]];}}
function draw(){if(shoe.length<20){buildShoe();toast('🔄 Shoe reshuffled');}return shoe.pop();}
function rname(r){return r===1?'A':r===11?'J':r===12?'Q':r===13?'K':r;}
function cardBJ(c){return c.r===1?11:(c.r>10?10:c.r);}
function handVal(h){let t=h.reduce((s,c)=>s+cardBJ(c),0),aces=h.filter(c=>c.r===1).length;
 while(t>21&&aces>0){t-=10;aces--;}return{total:t,soft:aces>0};}
function repsOf(h){const v=handVal(h);if(v.total<=21)return v.total;
 return h.reduce((s,c)=>s+(c.r===1?1:(c.r>10?10:c.r)),0);}

function cardEl(c){const el=document.createElement('div');
 el.className='card '+((c.s==='♥'||c.s==='♦')?'red':'black');
 el.innerHTML='<div class="corner">'+rname(c.r)+'<br>'+c.s+'</div>'+
  '<div class="rank">'+rname(c.r)+'</div><div class="suit-big">'+c.s+'</div>'+
  '<div class="ex">'+EX[c.s]+' × '+cardBJ(c)+(S.intensity>1?' ×'+S.intensity:'')+'</div>';
 return el;}
function holeEl(){const b=document.createElement('div');b.className='card hole';return b;}
function renderHands(){
 const dc=$('dealerCards');dc.innerHTML='';
 dealer.forEach((c,i)=>{dc.appendChild(i===0&&holeHidden?holeEl():cardEl(c));});
 const pc=$('playerCards');pc.innerHTML='';
 player.forEach(c=>pc.appendChild(cardEl(c)));
 const pv=handVal(player);
 $('pTotal').textContent=player.length?pv.total+(pv.soft&&pv.total!==21?' soft':''):'–';
 $('dTotal').textContent=dealer.length?(holeHidden?cardBJ(dealer[1]):handVal(dealer).total):'–';}
function refreshStats(){
 $('sChips').textContent=S.chips;$('sStreak').textContent=S.streak;
 $('sRounds').textContent=S.rounds;$('sBJ').textContent=S.blackjacks;
 $('sReps').textContent=S.reps.toLocaleString();
 $('dealBtn').disabled=bet<=0||bet>S.chips;
 $('loanBtn').style.display=(S.chips<10&&phase==='bet')?'inline-block':'none';
 renderAch();}
function renderAch(){$('achList').innerHTML=ACH_DEFS.map(a=>{
 const u=S.ach[a.id];
 return '<div class="ach '+(u?'unlocked':'')+'" title="'+a.desc+'"><span class="ico">'+a.ico+
 '</span><span>'+a.name+'<br><small>'+a.desc+'</small></span></div>';}).join('');}
function renderHistory(){if(!S.history.length)return;
 $('historyLog').innerHTML=S.history.map(h=>
 '<div>'+h.date+' '+h.res+'<br><small>You '+h.pv+' vs '+h.dv+' · '+
 (h.delta>0?'+':'')+h.delta+'$ · '+h.reps+' reps</small></div>').join('');}

function addBet(n){if(phase!=='bet')return;
 if(bet+n<=S.chips){bet+=n;beep(880,.06,'square',.08);}
 $('betAmt').textContent='$'+bet;refreshStats();}
function resetBet(){if(phase!=='bet')return;bet=0;$('betAmt').textContent='0';refreshStats();}
function takeLoan(){S.chips+=200;S.reps+=50*S.intensity;
 toast('💸 Loan received — now DO those jacks!');beep(500,.3,'triangle');save();}

function setMsg(html,cls){const m=$('msg');m.innerHTML=html;m.className='msg '+(cls||'');}

async function deal(){
 if(busy||bet<=0||bet>S.chips)return;
 busy=true;phase='playing';player=[];dealer=[];holeHidden=true;
 $('betArea').classList.remove('show');$('playControls').style.display='flex';
 $('nextControls').style.display='none';
 setMsg('Bet placed: <b>$'+bet+'</b>. Dealing…','');
 S.chips-=bet;save();
 player.push(draw());renderHands();beep(700,.08);await sleep(260);
 dealer.push(draw());renderHands();beep(600,.08);await sleep(260);
 player.push(draw());renderHands();beep(700,.08);await sleep(260);
 dealer.push(draw());renderHands();beep(600,.08);await sleep(300);
 if(handVal(player).total===21){await finishRound('bj');busy=false;return;}
 setMsg('Your move — Hit, Stand, or Double?','');
 $('doubleBtn').disabled=!(S.chips>=bet);
 busy=false;}

function hit(){
 if(phase!=='playing'||busy)return;
 player.push(draw());renderHands();beep(750,.07);
 const pv=handVal(player).total;
 if(pv>21)finishRound('bust');
 else if(pv===21)stand();
 else{$('doubleBtn').disabled=true;setMsg('You have '+pv+'. Hit again?','');}}

function doubleDown(){
 if(phase!=='playing'||busy||player.length!==2||S.chips<bet)return;
 S.chips-=bet;bet*=2;save();toast('💰💰 Doubled down!');
 player.push(draw());renderHands();beep(900,.1);
 if(handVal(player).total>21)finishRound('bust');else stand();}

async function stand(){
 if(phase!=='playing'||busy)return;
 busy=true;$('hitBtn').disabled=$('standBtn').disabled=$('doubleBtn').disabled=true;
 holeHidden=false;renderHands();beep(500,.15);await sleep(650);
 while(true){const dv=handVal(dealer);
  if(dv.total<17||(dv.total===17&&dv.soft)){
   setMsg('Dealer draws…','');await sleep(750);
   dealer.push(draw());renderHands();beep(550,.08);await sleep(450);
  }else break;}
 const dv=handVal(dealer).total,pv=handVal(player).total;
 if(dv>21)await finishRound('dealerBust');
 else if(dv<pv)await finishRound('win');
 else if(dv>pv)await finishRound('lose');
 else await finishRound('push');
 busy=false;}

async function finishRound(result){
 phase='done';
 $('hitBtn').disabled=$('standBtn').disabled=$('doubleBtn').disabled=true;
 holeHidden=false;renderHands();
 const pv=handVal(player).total,dv=handVal(dealer).total;
 const myReps=repsOf(player);
 let delta=0,extra=[],title='',cls='';
 if(result==='bj'){delta=Math.floor(bet*2.5);S.blackjacks++;cls='bj';title='🂡 BLACKJACK!';
  extra.push(['🎉 Bonus celebration','21 Jumping Jacks',false]);chord([523,659,784,1047]);}
 else if(result==='win'){delta=bet*2;cls='win';title='✅ You WIN!';chord([523,659,784]);}
 else if(result==='dealerBust'){delta=bet*2;cls='win';title='💥 Dealer BUSTS — You Win!';
  chord([523,659,784]);extra.push(['😤 House meltdown',(dv-21)+' Burpees',false]);}
 else if(result==='push'){delta=bet;cls='';title='🤝 Push — bet returned';beep(400,.2);}
 else if(result==='lose'){cls='lose';title='😔 Dealer wins';
  const gap=Math.min((dv-pv)*10,40);
  if(gap>0)extra.push(['⚠️ Forfeit',gap+' Mountain Climbers',true]);chord([392,330,262]);}
 else{cls='lose';title='💥 BUST!';
  const over=myReps>21?(myReps-21):5;
  extra.push(['🧱 Penalty Plank',(over*15)+' seconds',true]);chord([300,250,200,150]);}

 S.chips+=delta;S.rounds++;
 const bankedReps=(result==='bj'?21:myReps)*S.intensity;
 S.reps+=bankedReps;
 S.history.unshift({res:title,pv,dv,delta,reps:bankedReps,
  date:new Date().toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'})});
 S.history=S.history.slice(0,20);renderHistory();
 checkAchievements(pv);save();

 setMsg(title+' '+(delta>0?'<b>+'+(delta-bet)+'</b>':'±0')+' chips',cls);

 let body='<div class="sum-line"><span>Your hand</span><b>'+pv+'</b></div>'+
  '<div class="sum-line"><span>Dealer hand</span><b>'+dv+'</b></div>'+
  '<div class="sum-line"><span>Chip result</span><b>'+(delta-bet>0?'+':'')+(delta-bet)+'</b></div>'+
  '<div style="margin:12px 0 4px;color:#fbbf24;font-size:.75rem;letter-spacing:2px;">WORKOUT ('+S.intensity+'× SETS)</div>';
 player.forEach(c=>{body+='<div class="sum-line"><span>'+c.s+' '+rname(c.r)+
  '</span><b>'+EX[c.s]+' × '+cardBJ(c)+'</b></div>';});
 body+='<div class="sum-line"><span>Total reps banked</span><b>'+bankedReps+'</b></div>';
 extra.forEach(e=>{body+='<div class="sum-line pen"><span>'+e[0]+'</span><b class="pen">'+e[1]+'</b></div>';});
 body+='<div style="margin-top:14px;text-align:center;font-size:.85rem">💰 Balance: <b style="color:#fbbf24">$'+S.chips+'</b></div>';
 $('sumTitle').textContent=title;$('sumBody').innerHTML=body;
 await sleep(600);
 $('summaryOverlay').classList.add('open');
 $('nextControls').style.display='flex';}

function closeSummary(){$('summaryOverlay').classList.remove('open');}
function newRound(){
 closeSummary();
 phase='bet';bet=0;player=[];dealer=[];holeHidden=true;
 renderHands();$('betAmt').textContent='0';
 $('playControls').style.display='none';$('nextControls').style.display='none';
 $('betArea').classList.add('show');
 $('hitBtn').disabled=$('standBtn').disabled=$('doubleBtn').disabled=false;
 setMsg(S.chips<10?'Bankroll low — take a loan & earn it!':'Set your bet to begin.','');
 refreshStats();}

function unlock(id){
 if(!S.ach[id]){S.ach[id]=true;const a=ACH_DEFS.find(x=>x.id===id);
  if(a)toast('🏅 Achievement unlocked: '+a.name+'!');chord([659,880]);}}
function checkAchievements(pv){
 unlock('first');
 if(S.blackjacks>=1)unlock('bj');
 if(S.blackjacks>=10)unlock('bj10');
 if(pv>=20&&phase==='done')unlock('k');
 if(S.reps>=1000)unlock('k1');
 if(S.reps>=5000)unlock('k5');
 if(S.chips>=1000)unlock('rich');
 if(S.streak>=7)unlock('week');}

let tSec=60,tInt=null,tRun=false;
function fmt(s){return String(Math.floor(s/60)).padStart(2,'0')+':'+String(s%60).padStart(2,'0');}
function setPreset(t,btn){pauseTimer();tSec=t;$('tDisp').textContent=fmt(t);
 document.querySelectorAll('.t-presets button').forEach(b=>b.classList.remove('active'));
 btn.classList.add('active');tRun=false;tInt=null;}
function startTimer(){if(tRun)return;tRun=true;
 tInt=setInterval(()=>{tSec--;$('tDisp').textContent=fmt(Math.max(tSec,0));
  if(tSec<=0){pauseTimer();chord([880,880,880]);
   document.title='⏰ REST OVER!';
   setTimeout(()=>document.title='BlacJack Fitness — Beat the Deck, Build the Body',4000);}},1000);}
function pauseTimer(){tRun=false;if(tInt)clearInterval(tInt);tInt=null;}
function resetTimer(){pauseTimer();
 const active=document.querySelector('.t-presets .active');
 tSec=active?parseInt(active.dataset.t):60;$('tDisp').textContent=fmt(tSec);}

function openModal(id){$(id).classList.add('open');}
function closeModal(id){$(id).classList.remove('open');}
function applySettings(){
 S.intensity=parseInt($('intensitySel').value);
 S.bank=parseInt($('bankSel').value);save();toast('⚙️ Settings saved');}
function wipeAll(){
 if(confirm('Wipe ALL stats, chips and achievements?')){
  localStorage.removeItem('bjf_v2');location.reload();}}
document.querySelectorAll('.overlay').forEach(o=>o.addEventListener('click',
 e=>{if(e.target===o)o.classList.remove('open');}));

(function(){const today=new Date().toDateString(),
 yest=new Date(Date.now()-864e5).toDateString();
 if(S.lastDay!==today){S.streak=(S.lastDay===yest)?S.streak+1:1;S.lastDay=today;save();}})();

document.addEventListener('keydown',e=>{const k=e.key.toLowerCase();
 if(k==='h')hit();
 if(k==='s'&&phase==='playing')stand();
 if(k==='d'&&phase==='playing'&&player.length===2)doubleDown();
 if(k==='n'&&phase==='done')newRound();});

let deferredPrompt=null;
addEventListener('beforeinstallprompt',e=>{
 e.preventDefault();deferredPrompt=e;$('installBtn').style.display='inline-block';});
$('installBtn').onclick=()=>{
 if(deferredPrompt){deferredPrompt.prompt();$('installBtn').style.display='none';}};
if('serviceWorker' in navigator){addEventListener('load',()=>navigator.serviceWorker.register('sw.js'));}

$('intensitySel').value=S.intensity;$('bankSel').value=S.bank;
$('sndBtn').textContent=S.sound?'🔊':'🔇';
buildShoe();renderHands();refreshStats();renderHistory();
</script>
</body>
</html>
