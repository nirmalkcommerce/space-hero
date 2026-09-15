# space-hero
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1, user-scalable=no">
<title>SPACE HERO: Alien Attack!</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Baloo+2:wght@500;600;700;800&display=swap" rel="stylesheet">
<style>
  :root{
    --bg-deep:#150934;
    --bg-deep2:#0d0625;
    --panel:#241355;
    --panel-edge:#3a1f7a;
    --pink:#FF5DA2;
    --cyan:#4DEEEA;
    --yellow:#FFD23F;
    --green:#6BCB77;
    --white:#FFFFFF;
    --orange:#FF9F45;
    --purple:#9B5DE5;
    --ink:#1B1035;
  }
  *{box-sizing:border-box;}
  html,body{
    margin:0;padding:0;height:100%;
    background:radial-gradient(circle at 50% 20%, #2a1660 0%, var(--bg-deep2) 65%);
    font-family:'Baloo 2', 'Trebuchet MS', sans-serif;
    color:var(--white);
    -webkit-tap-highlight-color:transparent;
    user-select:none;
    overflow:hidden;
  }
  #app{
    display:flex;flex-direction:column;align-items:center;justify-content:center;
    min-height:100%;
    padding:10px;
    gap:8px;
  }
  .stage{
    position:relative;
    width:100%;
    max-width:820px;
    aspect-ratio:8/5;
    border-radius:22px;
    overflow:hidden;
    box-shadow:0 0 0 5px var(--panel-edge), 0 0 0 9px rgba(255,255,255,0.06), 0 18px 40px rgba(0,0,0,0.55);
    background:#000;
  }
  canvas{
    display:block;
    width:100%;
    height:100%;
  }
  .panel{
    position:absolute;inset:0;
    display:flex;flex-direction:column;align-items:center;justify-content:center;
    gap:14px;
    background:linear-gradient(180deg, rgba(21,9,52,0.92), rgba(13,6,37,0.96));
    text-align:center;
    padding:18px;
  }
  .hidden{display:none !important;}
  h1.title{
    font-size:clamp(28px,6vw,48px);
    margin:0;
    color:var(--yellow);
    letter-spacing:1px;
    text-shadow:0 3px 0 #b8790a, 0 6px 12px rgba(0,0,0,0.4);
  }
  .subtitle{
    font-size:clamp(14px,2.6vw,20px);
    color:var(--cyan);
    margin:-8px 0 4px 0;
    font-weight:600;
  }
  .alienwave{
    font-size:34px;
    animation:wave 1.4s ease-in-out infinite;
    display:inline-block;
  }
  @keyframes wave{
    0%,100%{transform:rotate(0deg) translateY(0);}
    25%{transform:rotate(-12deg) translateY(-3px);}
    75%{transform:rotate(10deg) translateY(-3px);}
  }
  .btn{
    font-family:inherit;
    font-weight:700;
    font-size:clamp(15px,2.6vw,19px);
    color:var(--ink);
    background:var(--yellow);
    border:none;
    border-radius:16px;
    padding:13px 30px;
    cursor:pointer;
    box-shadow:0 5px 0 #b8790a;
    transition:transform 0.06s ease;
    min-width:180px;
  }
  .btn:active{transform:translateY(4px);box-shadow:0 1px 0 #b8790a;}
  .btn.secondary{background:var(--cyan);box-shadow:0 5px 0 #1a9a98;}
  .btn.secondary:active{box-shadow:0 1px 0 #1a9a98;}
  .btn.pink{background:var(--pink);box-shadow:0 5px 0 #c22c73;}
  .btn.pink:active{box-shadow:0 1px 0 #c22c73;}
  .btn.green{background:var(--green);box-shadow:0 5px 0 #3f9a4c;}
  .btn.green:active{box-shadow:0 1px 0 #3f9a4c;}
  .btn-row{display:flex;gap:12px;flex-wrap:wrap;justify-content:center;}
  .howto-row{
    display:flex;gap:22px;flex-wrap:wrap;justify-content:center;
    margin:6px 0;
  }
  .howto-card{
    background:var(--panel);
    border:3px solid var(--panel-edge);
    border-radius:16px;
    padding:14px 18px;
    min-width:120px;
  }
  .howto-card .big{font-size:30px;display:block;margin-bottom:4px;}
  .howto-card .lbl{font-size:14px;color:var(--yellow);font-weight:700;}
  .statline{font-size:clamp(15px,2.6vw,19px);color:var(--white);margin:2px 0;}
  .statline b{color:var(--yellow);}
  .stars{font-size:34px;letter-spacing:6px;margin:4px 0;}
  .toggle-row{display:flex;align-items:center;gap:10px;background:var(--panel);border:3px solid var(--panel-edge);border-radius:14px;padding:10px 18px;font-size:16px;}
  .small-note{font-size:12px;color:#b9aee0;max-width:420px;}
  #hud{
    position:absolute;top:0;left:0;right:0;
    display:flex;justify-content:space-between;align-items:flex-start;
    padding:8px 12px;
    pointer-events:none;
    font-weight:700;
    font-size:clamp(12px,2.4vw,17px);
    text-shadow:0 2px 3px rgba(0,0,0,0.6);
  }
  #hud .chip{
    background:rgba(21,9,52,0.55);
    border:2px solid rgba(255,255,255,0.18);
    border-radius:12px;
    padding:5px 10px;
  }
  #comboPop{
    position:absolute;
    left:50%;top:14%;
    transform:translate(-50%,0);
    font-size:clamp(18px,4vw,30px);
    color:var(--pink);
    font-weight:800;
    text-shadow:0 3px 0 #7a1245;
    pointer-events:none;
    opacity:0;
  }
  .controls-wrap{
    display:flex;justify-content:space-between;align-items:center;
    width:100%;max-width:820px;
    gap:10px;
  }
  .ctrl-btn{
    width:78px;height:64px;
    border-radius:18px;
    border:none;
    font-size:26px;
    font-weight:800;
    color:var(--ink);
    background:var(--cyan);
    box-shadow:0 5px 0 #1a9a98;
    touch-action:none;
  }
  .ctrl-btn:active{transform:translateY(4px);box-shadow:0 1px 0 #1a9a98;}
  #btnFire{
    background:var(--pink);
    box-shadow:0 5px 0 #c22c73;
    width:110px;height:72px;
    font-size:16px;
  }
  #btnFire:active{box-shadow:0 1px 0 #c22c73;}
  .dpad{display:flex;gap:10px;}
  #btnPause{
    background:var(--panel);
    border:2px solid var(--panel-edge);
    color:var(--white);
    width:46px;height:46px;
    border-radius:12px;
    font-size:18px;
  }
  .top-strip{display:flex;justify-content:space-between;width:100%;max-width:820px;align-items:center;}
  .logo-mini{font-weight:800;color:var(--yellow);font-size:14px;letter-spacing:0.5px;}
  select{
    font-family:inherit;font-weight:700;border-radius:10px;border:2px solid var(--panel-edge);
    background:var(--ink);color:var(--white);padding:5px 8px;
  }
</style>
</head>
<body>
<div id="app">
  <div class="top-strip">
    <div class="logo-mini">✦ SPACE HERO ✦</div>
    <button id="btnPause" class="hidden">II</button>
  </div>

  <div class="stage" id="stage">
    <canvas id="game" width="800" height="500"></canvas>
    <div id="hud" class="hidden">
      <div class="chip" id="hudScore">SCORE: 000000</div>
      <div class="chip" id="hudWave">WAVE 01</div>
      <div class="chip" id="hudLives">♥ ♥ ♥</div>
    </div>
    <div id="comboPop"></div>

    <!-- MAIN MENU -->
    <div class="panel" id="panelMenu">
      <span class="alienwave">👾</span>
      <h1 class="title">SPACE HERO</h1>
      <div class="subtitle">ALIEN ATTACK!</div>
      <div class="btn-row">
        <button class="btn" id="btnPlay">PLAY</button>
      </div>
      <div class="btn-row">
        <button class="btn secondary" id="btnHowTo">HOW TO PLAY</button>
        <button class="btn secondary" id="btnScores">SCORES</button>
        <button class="btn secondary" id="btnSettings">SETTINGS</button>
      </div>
    </div>

    <!-- HOW TO PLAY -->
    <div class="panel hidden" id="panelHowTo">
      <h1 class="title" style="font-size:clamp(22px,5vw,34px)">HOW TO PLAY</h1>
      <div class="howto-row">
        <div class="howto-card"><span class="big">◀</span><span class="lbl">MOVE LEFT</span></div>
        <div class="howto-card"><span class="big">▶</span><span class="lbl">MOVE RIGHT</span></div>
        <div class="howto-card"><span class="big">★</span><span class="lbl">TAP FIRE TO SHOOT</span></div>
      </div>
      <div class="subtitle">SAVE THE GALAXY!</div>
      <div class="small-note">Dodge the aliens, blast them with your energy blaster, and grab glowing power-ups. Don't let any alien reach the bottom!</div>
      <button class="btn pink" id="btnHowToBack">BACK</button>
    </div>

    <!-- SCORES -->
    <div class="panel hidden" id="panelScores">
      <h1 class="title" style="font-size:clamp(22px,5vw,34px)">SCORES</h1>
      <div class="statline">Best score this session: <b id="scoreBest">0</b></div>
      <div class="statline">Levels completed this session: <b id="scoreLevels">0</b></div>
      <div class="statline">Best combo this session: <b id="scoreCombo">x0</b></div>
      <div class="small-note">Scores reset when you close this game — nothing is saved between visits.</div>
      <button class="btn pink" id="btnScoresBack">BACK</button>
    </div>

    <!-- SETTINGS -->
    <div class="panel hidden" id="panelSettings">
      <h1 class="title" style="font-size:clamp(22px,5vw,34px)">SETTINGS</h1>
      <div class="toggle-row">
        <span>Sound</span>
        <select id="selSound"><option value="on">On</option><option value="off">Off</option></select>
      </div>
      <div class="toggle-row">
        <span>Screen Shake</span>
        <select id="selShake"><option value="on">On</option><option value="off">Off</option></select>
      </div>
      <div class="toggle-row">
        <span>Difficulty</span>
        <select id="selDifficulty">
          <option value="easy">Easy</option>
          <option value="normal" selected>Normal</option>
        </select>
      </div>
      <button class="btn pink" id="btnSettingsBack">BACK</button>
    </div>

    <!-- LEVEL INTRO -->
    <div class="panel hidden" id="panelLevelIntro">
      <h1 class="title" id="liTitle" style="font-size:clamp(26px,6vw,42px)">LEVEL 01</h1>
      <div class="subtitle" id="liEnv">Moon Base</div>
    </div>

    <!-- BOSS WARNING -->
    <div class="panel hidden" id="panelBossWarning">
      <h1 class="title" style="color:var(--pink)">WARNING!</h1>
      <div class="subtitle" style="font-size:clamp(16px,3vw,24px);color:var(--white)">CAPTAIN ZORBO HAS ARRIVED!</div>
      <span class="alienwave" style="font-size:60px">👽</span>
    </div>

    <!-- LEVEL COMPLETE -->
    <div class="panel hidden" id="panelLevelComplete">
      <h1 class="title" style="font-size:clamp(24px,5vw,36px)">LEVEL COMPLETE!</h1>
      <div class="stars" id="lcStars">⭐⭐⭐</div>
      <div class="statline">Aliens defeated: <b id="lcAliens">0</b></div>
      <div class="statline">Score: <b id="lcScore">0</b></div>
      <div class="statline">Best combo: <b id="lcCombo">x0</b></div>
      <button class="btn green" id="btnNextLevel">NEXT LEVEL →</button>
    </div>

    <!-- GAME OVER -->
    <div class="panel hidden" id="panelGameOver">
      <span class="alienwave" style="font-size:50px">🥺</span>
      <h1 class="title" style="color:var(--pink)">TRY AGAIN!</h1>
      <div class="statline">Score: <b id="goScore">0</b></div>
      <div class="btn-row">
        <button class="btn" id="btnRetry">RETRY</button>
        <button class="btn secondary" id="btnGoMenu">MAIN MENU</button>
      </div>
    </div>

    <!-- VICTORY -->
    <div class="panel hidden" id="panelVictory">
      <span class="alienwave" style="font-size:56px">🎉</span>
      <h1 class="title" style="color:var(--green)">GALAXY SAVED!</h1>
      <div class="statline">Final score: <b id="vScore">0</b></div>
      <div class="statline" style="color:var(--cyan)">"I'll be back... for snacks!" — Captain Zorbo</div>
      <div class="btn-row">
        <button class="btn green" id="btnPlayAgain">PLAY AGAIN</button>
        <button class="btn secondary" id="btnVictoryMenu">MAIN MENU</button>
      </div>
    </div>

    <!-- PAUSE -->
    <div class="panel hidden" id="panelPause">
      <h1 class="title" style="font-size:clamp(24px,5vw,36px)">PAUSED</h1>
      <div class="btn-row">
        <button class="btn" id="btnResume">RESUME</button>
        <button class="btn secondary" id="btnPauseMenu">MAIN MENU</button>
      </div>
    </div>
  </div>

  <div class="controls-wrap">
    <button class="ctrl-btn" id="btnLeft">◀</button>
    <button id="btnFire">★ FIRE ★</button>
    <button class="ctrl-btn" id="btnRight">▶</button>
  </div>
</div>

<script>
(function(){
"use strict";

// ---------- CANVAS SETUP ----------
const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');
const W = 800, H = 500;

// ---------- AUDIO ----------
let audioCtx = null;
let soundOn = true;
function ensureAudio(){
  if(!audioCtx){
    try{ audioCtx = new (window.AudioContext || window.webkitAudioContext)(); }catch(e){ audioCtx=null; }
  }
}
function beep(freq, dur, type, gainVal, glideTo){
  if(!soundOn || !audioCtx) return;
  const t0 = audioCtx.currentTime;
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.type = type || 'square';
  osc.frequency.setValueAtTime(freq, t0);
  if(glideTo) osc.frequency.linearRampToValueAtTime(glideTo, t0+dur);
  gain.gain.setValueAtTime(gainVal||0.08, t0);
  gain.gain.exponentialRampToValueAtTime(0.001, t0+dur);
  osc.connect(gain); gain.connect(audioCtx.destination);
  osc.start(t0); osc.stop(t0+dur+0.02);
}
const SFX = {
  pew: ()=>beep(880,0.09,'square',0.06,1400),
  pop: ()=>beep(520,0.14,'triangle',0.09,220),
  ding: ()=>beep(1046,0.16,'sine',0.09,1500),
  wow: ()=>beep(660,0.22,'sawtooth',0.06,990),
  hit: ()=>beep(180,0.22,'square',0.10,90),
  boss: ()=>beep(140,0.5,'sawtooth',0.10,260),
  gameover: ()=>{beep(440,0.18,'triangle',0.08,300); setTimeout(()=>beep(300,0.28,'triangle',0.08,150),160);},
  victory: ()=>{beep(523,0.14,'square',0.08); setTimeout(()=>beep(659,0.14,'square',0.08),140); setTimeout(()=>beep(784,0.14,'square',0.08),280); setTimeout(()=>beep(1046,0.3,'square',0.09),420);},
  level: ()=>{beep(660,0.12,'square',0.08); setTimeout(()=>beep(880,0.16,'square',0.08),120);}
};

// ---------- STATE MACHINE ----------
const STATE = { MENU:'MENU', HOWTO:'HOWTO', SCORES:'SCORES', SETTINGS:'SETTINGS',
  PLAYING:'PLAYING', PAUSED:'PAUSED', LEVEL_INTRO:'LEVEL_INTRO', BOSS_WARNING:'BOSS_WARNING',
  LEVEL_COMPLETE:'LEVEL_COMPLETE', GAME_OVER:'GAME_OVER', VICTORY:'VICTORY' };
let state = STATE.MENU;
let stateBeforePause = STATE.PLAYING;

// ---------- SESSION STATS ----------
let sessionBest = 0;
let sessionLevels = 0;
let sessionBestCombo = 0;
let screenShakeOn = true;
let difficulty = 'normal';

// ---------- PANELS ----------
const panels = {
  menu: document.getElementById('panelMenu'),
  howto: document.getElementById('panelHowTo'),
  scores: document.getElementById('panelScores'),
  settings: document.getElementById('panelSettings'),
  levelIntro: document.getElementById('panelLevelIntro'),
  bossWarning: document.getElementById('panelBossWarning'),
  levelComplete: document.getElementById('panelLevelComplete'),
  gameOver: document.getElementById('panelGameOver'),
  victory: document.getElementById('panelVictory'),
  pause: document.getElementById('panelPause'),
};
function hideAllPanels(){ Object.values(panels).forEach(p=>p.classList.add('hidden')); }
function showPanel(p){ hideAllPanels(); p.classList.remove('hidden'); }

const hud = document.getElementById('hud');
const btnPauseTop = document.getElementById('btnPause');

// ---------- INPUT ----------
const keys = { left:false, right:false, fire:false };
window.addEventListener('keydown', (e)=>{
  if(['ArrowLeft','a','A'].includes(e.key)) keys.left = true;
  if(['ArrowRight','d','D'].includes(e.key)) keys.right = true;
  if(e.key===' '||e.key==='ArrowUp'||e.key==='w'||e.key==='W'){ keys.fire = true; e.preventDefault(); }
  if(e.key==='p'||e.key==='P'||e.key==='Escape'){ togglePause(); }
});
window.addEventListener('keyup', (e)=>{
  if(['ArrowLeft','a','A'].includes(e.key)) keys.left = false;
  if(['ArrowRight','d','D'].includes(e.key)) keys.right = false;
  if(e.key===' '||e.key==='ArrowUp'||e.key==='w'||e.key==='W') keys.fire = false;
});
function bindHold(el, onDown, onUp){
  el.addEventListener('pointerdown', (e)=>{ e.preventDefault(); ensureAudio(); onDown(); });
  el.addEventListener('pointerup', (e)=>{ e.preventDefault(); onUp(); });
  el.addEventListener('pointerleave', (e)=>{ onUp(); });
  el.addEventListener('pointercancel', (e)=>{ onUp(); });
}
bindHold(document.getElementById('btnLeft'), ()=>keys.left=true, ()=>keys.left=false);
bindHold(document.getElementById('btnRight'), ()=>keys.right=true, ()=>keys.right=false);
bindHold(document.getElementById('btnFire'), ()=>keys.fire=true, ()=>keys.fire=false);

// ---------- STARFIELD BACKGROUND ----------
let stars = [];
function initStars(){
  stars = [];
  for(let i=0;i<70;i++){
    stars.push({ x:Math.random()*W, y:Math.random()*H, r:Math.random()*1.8+0.4, s:Math.random()*30+10 });
  }
}
initStars();

// ---------- LEVEL / ENVIRONMENT THEMES ----------
const ENVIRONMENTS = [
  { name:'Moon Base', sky1:'#160c3a', sky2:'#2c1a5e', ground:'#3a2a66' },
  { name:'Candy-Colored Planet', sky1:'#3a0f4d', sky2:'#7a2a72', ground:'#a44a8a' },
  { name:'Space Station', sky1:'#0c1a3a', sky2:'#16305e', ground:'#274a7a' },
  { name:'Crystal Planet', sky1:'#0c2e3a', sky2:'#155a5e', ground:'#1f8a8a' },
  { name:'Alien Headquarters', sky1:'#2a0c1a', sky2:'#5e1630', ground:'#7a2748' },
];

// ---------- PLAYER ----------
const player = {
  x: W/2, y: H-60,
  speed: 340,
  lives: 3,
  invuln: 0,
  fireCooldown: 0,
  fireRate: 0.30,
  rapidTimer: 0,
  doubleTimer: 0,
  shieldTimer: 0,
  magnetTimer: 0,
  blinkPhase: 0,
  hitFlash: 0,
  oopsTimer: 0,
};

function resetPlayer(){
  player.x = W/2; player.y = H-60;
  player.lives = 3; player.invuln = 1.2;
  player.fireCooldown = 0; player.rapidTimer=0; player.doubleTimer=0; player.shieldTimer=0; player.magnetTimer=0;
  player.hitFlash=0; player.oopsTimer=0;
}

// ---------- ENTITIES ----------
let bullets = [];       // player bullets
let enemyBullets = [];  // enemy bullets
let enemies = [];
let particles = [];
let powerups = [];
let floatTexts = [];

let score = 0;
let combo = 0;
let comboTimer = 0;
let comboBestThisLevel = 0;
let aliensDefeatedThisLevel = 0;
let livesLostThisLevel = 0;

let currentLevel = 1;
const MAX_LEVEL = 5;
let waveSpawnQueue = [];
let waveTimer = 0;
let levelEnemiesTotal = 0;
let boss = null;
let bossSummonTimer = 0;

let shakeTime = 0, shakeMag = 0;

// ---------- HELPERS ----------
function rand(a,b){ return a + Math.random()*(b-a); }
function clamp(v,a,b){ return Math.max(a, Math.min(b, v)); }
function dist2(ax,ay,bx,by){ const dx=ax-bx, dy=ay-by; return dx*dx+dy*dy; }

function addShake(mag, time){
  if(!screenShakeOn) return;
  shakeMag = Math.max(shakeMag, mag); shakeTime = Math.max(shakeTime, time);
}

function spawnParticles(x,y,color,count,speed,life){
  for(let i=0;i<count;i++){
    const a = Math.random()*Math.PI*2;
    const sp = rand(speed*0.4, speed);
    particles.push({
      x,y, vx:Math.cos(a)*sp, vy:Math.sin(a)*sp,
      life: life||0.6, maxLife:life||0.6,
      r: rand(2,5), color
    });
  }
}
function spawnFloatText(x,y,text,color){
  floatTexts.push({x,y,text,color,life:0.9,maxLife:0.9});
}

// ---------- ALIEN DEFINITIONS ----------
// type: zippy, blobbo, spikey, ufobot
function makeAlien(type, x, y, speedMult, level){
  const base = {
    type, x, y, hp:1, maxHp:1, value:10, r:20,
    phase: Math.random()*Math.PI*2,
    dirX: Math.random()<0.5 ? -1:1,
    vy: 26*speedMult,
    dead:false, deathTimer:0, deathAnim:0,
    stallTimer: rand(0.5,2.0),
    hitFlash:0,
    shootTimer: rand(1.5,3.5),
  };
  if(type==='zippy'){ base.value=10; base.r=18; base.speedX=60*speedMult; }
  if(type==='blobbo'){
    base.value=25; base.r=24; base.speedX=34*speedMult;
    // Blobbo toughens up from Level 3 onward — takes two hits instead of one.
    base.hp = (level>=3) ? 2 : 1;
  }
  if(type==='spikey'){ base.value=50; base.r=17; base.speedX=110*speedMult; base.vy=40*speedMult; }
  if(type==='ufobot'){ base.value=30; base.r=22; base.speedX=70*speedMult; base.diving=false; base.diveT=0; }
  base.maxHp = base.hp;
  return base;
}

function levelSpeedMult(level){
  const diffMult = difficulty==='easy' ? 0.78 : 1.0;
  return (0.85 + (level-1)*0.13) * diffMult;
}

function buildWaveQueue(level){
  const sm = levelSpeedMult(level);
  const list = [];
  if(level===1){
    for(let i=0;i<5;i++) list.push({type:'zippy', delay:i*1.1});
  } else if(level===2){
    for(let i=0;i<8;i++) list.push({type: i%3===0?'blobbo':'zippy', delay:i*0.9});
  } else if(level===3){
    const seq = ['zippy','blobbo','spikey','zippy','ufobot','blobbo','spikey','zippy','ufobot','spikey'];
    seq.forEach((t,i)=>list.push({type:t, delay:i*0.85}));
  } else if(level===4){
    const seq = ['spikey','ufobot','zippy','spikey','blobbo','ufobot','spikey','zippy','ufobot','spikey','blobbo','zippy'];
    seq.forEach((t,i)=>list.push({type:t, delay:i*0.68}));
  }
  return { list, sm };
}

function startLevel(level){
  currentLevel = level;
  enemies = []; bullets=[]; enemyBullets=[]; particles=[]; powerups=[]; floatTexts=[];
  combo=0; comboTimer=0; comboBestThisLevel=0; aliensDefeatedThisLevel=0; livesLostThisLevel=0;
  boss = null; bossSummonTimer = 0;
  resetPlayer();
  waveTimer = 0;
  hud.classList.remove('hidden');
  btnPauseTop.classList.remove('hidden');
  updateHUD();

  if(level===MAX_LEVEL){
    state = STATE.BOSS_WARNING;
    showPanel(panels.bossWarning);
    if(soundOn) SFX.boss();
    setTimeout(()=>{
      if(state===STATE.BOSS_WARNING){
        hideAllPanels();
        state = STATE.PLAYING;
        spawnBoss();
      }
    }, 1800);
  } else {
    const q = buildWaveQueue(level);
    waveSpawnQueue = q.list;
    levelEnemiesTotal = q.list.length;
    const env = ENVIRONMENTS[(level-1) % ENVIRONMENTS.length];
    document.getElementById('liTitle').textContent = 'LEVEL ' + String(level).padStart(2,'0');
    document.getElementById('liEnv').textContent = env.name;
    state = STATE.LEVEL_INTRO;
    showPanel(panels.levelIntro);
    SFX.level();
    setTimeout(()=>{
      if(state===STATE.LEVEL_INTRO){
        hideAllPanels();
        waveTimer = 0; // don't count the intro banner's time against the first spawn delays
        state = STATE.PLAYING;
      }
    }, 1200);
  }
}

function spawnBoss(){
  boss = {
    x: W/2, y: -80, r:52,   // starts above the screen and flies in (see updateBoss)
    hp: 26, maxHp:26,
    dirX: 1, speed: 90,
    shootTimer: 1.6,
    hitFlash:0,
    dead:false, deathTimer:0,
  };
}

// ---------- WAVE SPAWNING DURING PLAY ----------
function updateWaveSpawns(dt){
  if(currentLevel===MAX_LEVEL) return;
  waveTimer += dt;
  while(waveSpawnQueue.length && waveSpawnQueue[0].delay <= waveTimer){
    const item = waveSpawnQueue.shift();
    const sm = levelSpeedMult(currentLevel);
    const x = rand(60, W-60);
    enemies.push(makeAlien(item.type, x, -30, sm, currentLevel));
  }
}

// ---------- ENEMY BEHAVIOR ----------
function updateEnemy(e, dt){
  e.phase += dt;
  if(e.hitFlash>0) e.hitFlash -= dt;

  if(e.type==='zippy'){
    e.x += Math.sin(e.phase*2.2)*e.speedX*dt*1.4;
    e.y += e.vy*dt;
  } else if(e.type==='blobbo'){
    e.stallTimer -= dt;
    if(e.stallTimer<=0){
      if(Math.random()<0.5){ e.dirX*=-1; }
      e.stallTimer = rand(0.6,1.8);
    }
    e.x += e.dirX*e.speedX*dt;
    e.y += e.vy*dt*0.85;
  } else if(e.type==='spikey'){
    e.stallTimer -= dt;
    if(e.stallTimer<=0){ e.dirX*=-1; e.stallTimer = rand(0.35,0.8); }
    e.x += e.dirX*e.speedX*dt;
    e.y += e.vy*dt;
  } else if(e.type==='ufobot'){
    e.x += Math.sin(e.phase*1.1)*e.speedX*dt*1.2;
    if(!e.diving){
      e.y += 18*dt;
      if(Math.random()<0.003) { e.diving=true; e.diveT=0; }
    } else {
      e.diveT += dt;
      e.y += 90*dt;
      if(e.diveT>0.7){ e.diving=false; }
    }
  }
  e.x = clamp(e.x, e.r, W-e.r);

  // occasional enemy shot (only after level 2, sparse)
  if(currentLevel>=3){
    e.shootTimer -= dt;
    if(e.shootTimer<=0 && e.y>40 && e.y<H-140){
      e.shootTimer = rand(2.5,4.5);
      const ang = Math.atan2(player.y-e.y, player.x-e.x);
      enemyBullets.push({ x:e.x, y:e.y, vx:Math.cos(ang)*130, vy:Math.sin(ang)*130, r:7, color:'#FF9F45' });
    }
  }
}

function drawAlien(e){
  ctx.save();
  ctx.translate(e.x, e.y);
  const flash = e.hitFlash>0;
  ctx.lineWidth = 3;
  ctx.strokeStyle = '#1B1035';

  // tiny hit-pips above any alien that needs more than one shot, so kids can see it took damage
  if(e.maxHp>1){
    const pipGap = 10;
    const startX = -((e.maxHp-1)*pipGap)/2;
    for(let i=0;i<e.maxHp;i++){
      ctx.beginPath();
      ctx.arc(startX+i*pipGap, -e.r-14, 3.2, 0, Math.PI*2);
      ctx.fillStyle = i < e.hp ? '#FFD23F' : 'rgba(255,255,255,0.25)';
      ctx.fill();
    }
  }

  if(e.type==='zippy'){
    ctx.fillStyle = flash ? '#ffffff' : '#6BCB77';
    ctx.beginPath(); ctx.arc(0,0,e.r,0,Math.PI*2); ctx.fill(); ctx.stroke();
    // antenna
    ctx.beginPath(); ctx.moveTo(0,-e.r); ctx.lineTo(0,-e.r-8); ctx.stroke();
    ctx.beginPath(); ctx.arc(0,-e.r-10,3,0,Math.PI*2); ctx.fillStyle='#FFD23F'; ctx.fill();
    // three eyes
    ctx.fillStyle='#fff';
    [[-8,-3],[8,-3],[0,6]].forEach(p=>{ ctx.beginPath(); ctx.arc(p[0],p[1],5,0,Math.PI*2); ctx.fill(); });
    ctx.fillStyle='#1B1035';
    [[-8,-3],[8,-3],[0,6]].forEach(p=>{ ctx.beginPath(); ctx.arc(p[0],p[1],2.2,0,Math.PI*2); ctx.fill(); });
    // smile
    ctx.beginPath(); ctx.arc(0,4,9,0.15*Math.PI,0.85*Math.PI); ctx.strokeStyle='#154d1e'; ctx.stroke();
  } else if(e.type==='blobbo'){
    ctx.fillStyle = flash ? '#ffffff' : '#9B5DE5';
    ctx.beginPath(); ctx.ellipse(0,0,e.r,e.r*0.92,0,0,Math.PI*2); ctx.fill(); ctx.strokeStyle='#1B1035'; ctx.stroke();
    ctx.fillStyle='#fff';
    [[-9,-2],[9,-2]].forEach(p=>{ ctx.beginPath(); ctx.arc(p[0],p[1],8,0,Math.PI*2); ctx.fill(); });
    ctx.fillStyle='#1B1035';
    [[-9,-2],[9,-2]].forEach(p=>{ ctx.beginPath(); ctx.arc(p[0]+1,p[1]+1,3.4,0,Math.PI*2); ctx.fill(); });
    // tiny feet
    ctx.fillStyle='#5a2c9e';
    ctx.beginPath(); ctx.ellipse(-8,e.r*0.85,5,3,0,0,Math.PI*2); ctx.fill();
    ctx.beginPath(); ctx.ellipse(8,e.r*0.85,5,3,0,0,Math.PI*2); ctx.fill();
  } else if(e.type==='spikey'){
    ctx.fillStyle = flash ? '#ffffff' : '#FF9F45';
    // soft spikes
    ctx.beginPath();
    for(let i=0;i<10;i++){
      const a = (i/10)*Math.PI*2;
      const rr = e.r + (i%2===0? 6:0);
      const px = Math.cos(a)*rr, py = Math.sin(a)*rr;
      if(i===0) ctx.moveTo(px,py); else ctx.lineTo(px,py);
    }
    ctx.closePath(); ctx.fill(); ctx.stroke();
    ctx.fillStyle='#fff';
    [[-7,-2],[7,-2]].forEach(p=>{ ctx.beginPath(); ctx.arc(p[0],p[1],5.5,0,Math.PI*2); ctx.fill(); });
    ctx.fillStyle='#1B1035';
    [[-7,-2],[7,-2]].forEach(p=>{ ctx.beginPath(); ctx.arc(p[0],p[1],2.4,0,Math.PI*2); ctx.fill(); });
  } else if(e.type==='ufobot'){
    ctx.fillStyle = flash ? '#ffffff' : '#4DEEEA';
    ctx.beginPath(); ctx.ellipse(0,4,e.r,e.r*0.5,0,0,Math.PI*2); ctx.fill(); ctx.stroke();
    ctx.fillStyle='#dff';
    ctx.beginPath(); ctx.ellipse(0,-4,e.r*0.55,e.r*0.42,0,Math.PI,0); ctx.fill(); ctx.stroke();
    ctx.fillStyle='#2c1a5e';
    ctx.beginPath(); ctx.arc(0,-4,6,0,Math.PI*2); ctx.fill();
    ctx.fillStyle= (Math.floor(e.phase*4)%2===0) ? '#FFD23F':'#FF5DA2';
    [-12,0,12].forEach(dx=>{ ctx.beginPath(); ctx.arc(dx,6,2.4,0,Math.PI*2); ctx.fill(); });
  }
  ctx.restore();
}

function alienDeathParticles(e){
  const colors = { zippy:'#6BCB77', blobbo:'#9B5DE5', spikey:'#FF9F45', ufobot:'#4DEEEA' };
  spawnParticles(e.x,e.y,colors[e.type]||'#FFD23F',16,150,0.6);
}

// ---------- POWER-UPS ----------
const POWER_TYPES = ['rapid','double','shield','starblast','magnet'];
function maybeDropPowerup(x,y){
  if(Math.random()<0.16){
    const type = POWER_TYPES[Math.floor(Math.random()*POWER_TYPES.length)];
    powerups.push({x,y,type, vy:55, r:14, phase:0});
  }
}
const POWER_META = {
  rapid:   { color:'#FFD23F', icon:'⚡', label:'RAPID FIRE' },
  double:  { color:'#4DEEEA', icon:'✦✦', label:'DOUBLE SHOT' },
  shield:  { color:'#6BCB77', icon:'◍', label:'SHIELD' },
  starblast:{color:'#FF5DA2', icon:'★', label:'STAR BLAST' },
  magnet:  { color:'#9B5DE5', icon:'🧲', label:'MAGNET' },
};
function applyPowerup(type){
  if(type==='rapid'){ player.rapidTimer = 8; SFX.ding(); spawnFloatText(player.x,player.y-40,'RAPID FIRE!','#FFD23F'); }
  else if(type==='double'){ player.doubleTimer = 8; SFX.ding(); spawnFloatText(player.x,player.y-40,'DOUBLE SHOT!','#4DEEEA'); }
  else if(type==='shield'){ player.shieldTimer = 7; SFX.ding(); spawnFloatText(player.x,player.y-40,'SHIELD UP!','#6BCB77'); }
  else if(type==='magnet'){ player.magnetTimer = 9; SFX.ding(); spawnFloatText(player.x,player.y-40,'MAGNET!','#9B5DE5'); }
  else if(type==='starblast'){
    SFX.wow();
    spawnFloatText(player.x,player.y-40,'STAR BLAST!','#FF5DA2');
    addShake(10,0.4);
    enemies.forEach(en=>{
      if(!en.dead){ en.dead=true; en.deathAnim=type; score += Math.round(en.value*getComboMult());
        alienDeathParticles(en); aliensDefeatedThisLevel++; }
    });
    enemies = enemies.filter(en=>!en.dead);
    if(boss){ boss.hp -= 4; boss.hitFlash=0.3; spawnParticles(boss.x,boss.y,'#FF5DA2',20,180,0.6); }
  }
}

// ---------- COMBO ----------
function getComboMult(){ return 1 + Math.floor(combo/5)*0.5; }
function registerKill(value){
  combo++;
  comboTimer = 2.6;
  comboBestThisLevel = Math.max(comboBestThisLevel, combo);
  sessionBestCombo = Math.max(sessionBestCombo, combo);
  const gained = Math.round(value*getComboMult());
  score += gained;
  aliensDefeatedThisLevel++;
  spawnFloatText(player.x, player.y-30, '+'+gained, '#FFD23F');
  if(combo>=3 && combo%3===0){
    popCombo('COMBO x'+combo+'!');
    SFX.wow();
  }
}
function resetCombo(){ combo = 0; }

let comboPopEl = document.getElementById('comboPop');
let comboPopTimer = 0;
function popCombo(text){
  comboPopEl.textContent = text;
  comboPopEl.style.opacity = '1';
  comboPopEl.style.transform = 'translate(-50%,0) scale(1.15)';
  comboPopTimer = 0.9;
}

// ---------- PLAYER SHOOTING ----------
function tryPlayerShoot(dt){
  player.fireCooldown -= dt;
  const rate = player.rapidTimer>0 ? player.fireRate*0.42 : player.fireRate;
  if(keys.fire && player.fireCooldown<=0 && state===STATE.PLAYING){
    player.fireCooldown = rate;
    SFX.pew();
    if(player.doubleTimer>0){
      bullets.push({x:player.x-13,y:player.y-28,vy:-480,r:5});
      bullets.push({x:player.x+13,y:player.y-28,vy:-480,r:5});
    } else {
      bullets.push({x:player.x,y:player.y-30,vy:-480,r:5});
    }
  }
}

// ---------- COLLISIONS ----------
function circleHit(ax,ay,ar,bx,by,br){
  return dist2(ax,ay,bx,by) <= (ar+br)*(ar+br);
}

function playerTakeHit(){
  if(player.invuln>0 || player.shieldTimer>0) return;
  player.lives--;
  livesLostThisLevel++;
  player.invuln = 1.6;
  player.hitFlash = 0.5;
  player.oopsTimer = 0.9;
  resetCombo();
  addShake(8,0.35);
  SFX.hit();
  if(player.lives<=0){
    endGame();
  }
}

// Shared collision passes — called exactly once per frame from update(), for every level
// including the boss level, so aliens are never checked against bullets/bottom-of-screen twice.
function checkEnemiesReachedBottom(){
  enemies.forEach(e=>{
    if(!e.dead && e.y > H-70){
      e.dead = true; e.deathAnim='escape';
      playerTakeHit();
    }
  });
}

function resolveBulletsVsEnemies(){
  for(const b of bullets){
    for(const e of enemies){
      if(e.dead) continue;
      if(circleHit(b.x,b.y,b.r,e.x,e.y,e.r)){
        b.hit = true;
        e.hp -= 1;
        e.hitFlash = 0.15;
        if(e.hp<=0){
          e.dead = true;
          e.deathAnim = e.type;
          alienDeathParticles(e);
          registerKill(e.value);
          SFX.pop();
          maybeDropPowerup(e.x,e.y);
        } else {
          // chipped but not defeated (e.g. a tougher Blobbo) — spark, no points yet
          spawnParticles(b.x,b.y,'#e6d4ff',6,90,0.3);
          SFX.hit();
        }
        break;
      }
    }
  }
  bullets = bullets.filter(b=>!b.hit);
  enemies = enemies.filter(e=>!e.dead);
}

// ---------- UPDATE ----------
let lastTime = performance.now();
function update(dt){
  if(state!==STATE.PLAYING) return;

  // player movement
  let mv = 0;
  if(keys.left) mv -= 1;
  if(keys.right) mv += 1;
  player.x += mv*player.speed*dt;
  player.x = clamp(player.x, 30, W-30);
  if(player.invuln>0) player.invuln -= dt;
  if(player.hitFlash>0) player.hitFlash -= dt;
  if(player.oopsTimer>0) player.oopsTimer -= dt;
  if(player.rapidTimer>0) player.rapidTimer -= dt;
  if(player.doubleTimer>0) player.doubleTimer -= dt;
  if(player.shieldTimer>0) player.shieldTimer -= dt;
  if(player.magnetTimer>0) player.magnetTimer -= dt;
  player.blinkPhase += dt;

  tryPlayerShoot(dt);

  // bullets
  bullets.forEach(b=>b.y += b.vy*dt);
  bullets = bullets.filter(b=>b.y>-20);

  enemyBullets.forEach(b=>{ b.x+=b.vx*dt; b.y+=b.vy*dt; });
  enemyBullets = enemyBullets.filter(b=>b.y<H+20 && b.y>-20 && b.x>-20 && b.x<W+20);

  // combo decay
  if(combo>0){
    comboTimer -= dt;
    if(comboTimer<=0) resetCombo();
  }
  if(comboPopTimer>0){
    comboPopTimer -= dt;
    if(comboPopTimer<=0) comboPopEl.style.opacity='0';
  }

  if(currentLevel===MAX_LEVEL){
    updateBoss(dt);
  } else {
    updateWaveSpawns(dt);
    enemies.forEach(e=>{ if(!e.dead) updateEnemy(e, dt); });
  }

  // enemy reaches player zone, and bullet vs enemy — run once, for every level (see functions above)
  checkEnemiesReachedBottom();
  resolveBulletsVsEnemies();

  // bullet vs boss
  if(boss && !boss.dead){
    for(const b of bullets){
      if(circleHit(b.x,b.y,b.r,boss.x,boss.y,boss.r)){
        b.hit = true;
        boss.hp -= 1;
        boss.hitFlash = 0.15;
        spawnParticles(b.x,b.y,'#FF5DA2',6,90,0.35);
        SFX.hit();
        if(boss.hp<=0){
          boss.dead = true; boss.deathTimer = 1.6;
          score += 500;
          addShake(14,0.6);
          SFX.wow();
        }
      }
    }
    bullets = bullets.filter(b=>!b.hit);
  }

  // enemy bullets vs player
  for(const b of enemyBullets){
    if(circleHit(b.x,b.y,b.r,player.x,player.y-10,20)){
      b.hit = true;
      playerTakeHit();
    }
  }
  enemyBullets = enemyBullets.filter(b=>!b.hit);

  // powerups
  powerups.forEach(p=>{
    p.phase += dt;
    p.y += p.vy*dt;
    if(player.magnetTimer>0){
      const dx = player.x-p.x, dy=player.y-p.y;
      const d = Math.sqrt(dx*dx+dy*dy);
      if(d<220){ p.x += (dx/d)*220*dt; p.y += (dy/d)*220*dt; }
    }
  });
  powerups = powerups.filter(p=>{
    if(circleHit(p.x,p.y,p.r,player.x,player.y-10,26)){
      applyPowerup(p.type);
      return false;
    }
    return p.y < H+20;
  });

  // particles
  particles.forEach(pt=>{
    pt.x += pt.vx*dt; pt.y += pt.vy*dt;
    pt.vx *= 0.94; pt.vy *= 0.94;
    pt.life -= dt;
  });
  particles = particles.filter(pt=>pt.life>0);

  floatTexts.forEach(t=>{ t.y -= 34*dt; t.life -= dt; });
  floatTexts = floatTexts.filter(t=>t.life>0);

  if(shakeTime>0) shakeTime -= dt; else shakeMag = 0;

  updateHUD();
  checkLevelEnd();
}

function updateBoss(dt){
  if(!boss) return;
  if(boss.dead){
    boss.deathTimer -= dt;
    boss.y += 40*dt;
    if(boss.deathTimer<=0){
      boss = null;
      state = STATE.VICTORY;
      sessionBest = Math.max(sessionBest, score);
      sessionLevels = Math.max(sessionLevels, MAX_LEVEL);
      document.getElementById('vScore').textContent = score;
      showPanel(panels.victory);
      hud.classList.add('hidden');
      btnPauseTop.classList.add('hidden');
      SFX.victory();
    }
    return;
  }
  if(boss.y < 110){ boss.y += 60*dt; return; }
  boss.x += boss.dirX*boss.speed*dt;
  if(boss.x<90 || boss.x>W-90) boss.dirX*=-1;
  if(boss.hitFlash>0) boss.hitFlash -= dt;

  boss.shootTimer -= dt;
  if(boss.shootTimer<=0){
    boss.shootTimer = rand(1.3,2.1);
    for(let i=-1;i<=1;i++){
      enemyBullets.push({x:boss.x+i*26, y:boss.y+40, vx:i*40, vy:150, r:9, color:'#FF5DA2'});
    }
  }

  bossSummonTimer -= dt;
  if(bossSummonTimer<=0){
    bossSummonTimer = rand(6,9);
    const sm = levelSpeedMult(4);
    enemies.push(makeAlien(Math.random()<0.5?'zippy':'spikey', rand(80,W-80), -20, sm, MAX_LEVEL));
  }
  enemies.forEach(e=>{ if(!e.dead) updateEnemy(e, dt); });
  // Collisions for these summoned aliens (reaching bottom, getting shot) are handled once,
  // in the shared checkEnemiesReachedBottom()/resolveBulletsVsEnemies() calls in update() —
  // not duplicated here.
}

function checkLevelEnd(){
  if(currentLevel===MAX_LEVEL) return; // handled by boss death
  if(waveSpawnQueue.length===0 && enemies.length===0 && state===STATE.PLAYING){
    levelComplete();
  }
}

function levelComplete(){
  state = STATE.LEVEL_COMPLETE;
  sessionBest = Math.max(sessionBest, score);
  sessionLevels = Math.max(sessionLevels, currentLevel);
  let stars = 1;
  // 3 stars requires an unbroken combo covering ~80% of the level's aliens (never less than 3) —
  // scaled to level size so short levels aren't mathematically impossible to 3-star.
  const threshold3 = Math.max(3, Math.ceil(levelEnemiesTotal * 0.8));
  if(livesLostThisLevel===0 && comboBestThisLevel>=threshold3) stars = 3;
  else if(livesLostThisLevel<=1) stars = 2;
  document.getElementById('lcStars').textContent = '⭐'.repeat(stars) + '☆'.repeat(3-stars);
  document.getElementById('lcAliens').textContent = aliensDefeatedThisLevel;
  document.getElementById('lcScore').textContent = score;
  document.getElementById('lcCombo').textContent = 'x'+comboBestThisLevel;
  showPanel(panels.levelComplete);
  SFX.level();
}

function endGame(){
  state = STATE.GAME_OVER;
  sessionBest = Math.max(sessionBest, score);
  document.getElementById('goScore').textContent = score;
  showPanel(panels.gameOver);
  hud.classList.add('hidden');
  btnPauseTop.classList.add('hidden');
  SFX.gameover();
}

// ---------- HUD ----------
function updateHUD(){
  document.getElementById('hudScore').textContent = 'SCORE: ' + String(score).padStart(6,'0');
  document.getElementById('hudWave').textContent = 'WAVE ' + String(currentLevel).padStart(2,'0');
  document.getElementById('hudLives').textContent = '♥ '.repeat(Math.max(player.lives,0)).trim() || '—';
}

// ---------- DRAW ----------
function drawBackground(){
  const env = ENVIRONMENTS[(currentLevel-1) % ENVIRONMENTS.length];
  const g = ctx.createLinearGradient(0,0,0,H);
  g.addColorStop(0, env.sky1);
  g.addColorStop(1, env.sky2);
  ctx.fillStyle = g;
  ctx.fillRect(0,0,W,H);

  stars.forEach(s=>{
    s.y += s.s* (1/60);
    if(s.y>H) s.y = 0;
    ctx.fillStyle = 'rgba(255,255,255,'+rand(0.4,0.9).toFixed(2)+')';
    ctx.beginPath(); ctx.arc(s.x, s.y, s.r, 0, Math.PI*2); ctx.fill();
  });

  ctx.fillStyle = env.ground;
  ctx.fillRect(0,H-34,W,34);
  ctx.fillStyle = 'rgba(255,255,255,0.08)';
  for(let i=0;i<W;i+=40){
    ctx.fillRect(i, H-34, 22, 4);
  }
}

function drawPlayer(){
  if(state!==STATE.PLAYING && state!==STATE.PAUSED) return;
  ctx.save();
  ctx.translate(player.x, player.y);
  const bob = Math.sin(player.blinkPhase*3)*2;
  ctx.translate(0,bob);

  const flashing = player.invuln>0 && Math.floor(player.invuln*10)%2===0;
  if(flashing) ctx.globalAlpha = 0.45;

  // shield bubble
  if(player.shieldTimer>0){
    ctx.save();
    ctx.globalAlpha = 0.35 + Math.sin(player.blinkPhase*6)*0.1;
    ctx.strokeStyle = '#6BCB77'; ctx.lineWidth=4;
    ctx.beginPath(); ctx.arc(0,-6,40,0,Math.PI*2); ctx.stroke();
    ctx.fillStyle='rgba(107,203,119,0.12)'; ctx.fill();
    ctx.restore();
  }

  // backpack
  ctx.fillStyle = '#3a1f7a';
  ctx.fillRect(-24,-18,10,26);

  // body
  ctx.fillStyle = '#4DEEEA';
  ctx.strokeStyle = '#1B1035'; ctx.lineWidth = 3;
  ctx.beginPath();
  ctx.moveTo(-16,20); ctx.lineTo(-14,-6);
  ctx.quadraticCurveTo(0,-14,14,-6); ctx.lineTo(16,20);
  ctx.quadraticCurveTo(0,28,-16,20);
  ctx.closePath(); ctx.fill(); ctx.stroke();

  // emblem
  ctx.fillStyle='#FFD23F';
  ctx.beginPath(); ctx.arc(0,2,5,0,Math.PI*2); ctx.fill();

  // helmet
  ctx.fillStyle = '#FFF';
  ctx.beginPath(); ctx.arc(0,-20,18,0,Math.PI*2); ctx.fill(); ctx.stroke();
  ctx.fillStyle = '#9B5DE5';
  ctx.beginPath(); ctx.ellipse(2,-20,12,13,0,0,Math.PI*2); ctx.fill();
  // eyes / oops
  if(player.oopsTimer>0){
    ctx.strokeStyle='#fff'; ctx.lineWidth=2;
    ctx.beginPath(); ctx.moveTo(-6,-24); ctx.lineTo(-1,-19); ctx.moveTo(-1,-24); ctx.lineTo(-6,-19); ctx.stroke();
    ctx.beginPath(); ctx.moveTo(6,-24); ctx.lineTo(11,-19); ctx.moveTo(11,-24); ctx.lineTo(6,-19); ctx.stroke();
  } else {
    ctx.fillStyle='#fff';
    ctx.beginPath(); ctx.arc(-4,-20,3.4,0,Math.PI*2); ctx.arc(8,-20,3.4,0,Math.PI*2); ctx.fill();
  }
  // helmet light
  ctx.fillStyle = Math.floor(player.blinkPhase*2)%2===0 ? '#FFD23F' : '#FF5DA2';
  ctx.beginPath(); ctx.arc(0,-34,3,0,Math.PI*2); ctx.fill();

  // gun
  ctx.save();
  ctx.translate(18,4);
  ctx.fillStyle = player.doubleTimer>0 ? '#FF5DA2' : '#FFD23F';
  ctx.strokeStyle='#1B1035'; ctx.lineWidth=2.5;
  ctx.beginPath(); ctx.roundRect(-4,-6,22,12,4); ctx.fill(); ctx.stroke();
  ctx.fillStyle='#4DEEEA';
  ctx.beginPath(); ctx.arc(10,0,3.4,0,Math.PI*2); ctx.fill();
  ctx.restore();

  ctx.globalAlpha = 1;
  ctx.restore();
}

function drawBullets(){
  bullets.forEach(b=>{
    ctx.save();
    const grad = ctx.createRadialGradient(b.x,b.y,0,b.x,b.y,10);
    grad.addColorStop(0,'#fff'); grad.addColorStop(1,'#4DEEEA');
    ctx.fillStyle = grad;
    ctx.beginPath(); ctx.ellipse(b.x,b.y,4.5,11,0,0,Math.PI*2); ctx.fill();
    ctx.restore();
  });
  enemyBullets.forEach(b=>{
    ctx.fillStyle = b.color || '#FF9F45';
    ctx.beginPath(); ctx.arc(b.x,b.y,b.r,0,Math.PI*2); ctx.fill();
    ctx.strokeStyle='#1B1035'; ctx.lineWidth=1.5; ctx.stroke();
  });
}

function drawParticles(){
  particles.forEach(p=>{
    ctx.globalAlpha = Math.max(p.life/p.maxLife,0);
    ctx.fillStyle = p.color;
    ctx.beginPath(); ctx.arc(p.x,p.y,p.r,0,Math.PI*2); ctx.fill();
  });
  ctx.globalAlpha = 1;
}

function drawFloatTexts(){
  floatTexts.forEach(t=>{
    ctx.globalAlpha = Math.max(t.life/t.maxLife,0);
    ctx.fillStyle = t.color;
    ctx.font = '700 18px Baloo 2, sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText(t.text, t.x, t.y);
  });
  ctx.globalAlpha = 1;
  ctx.textAlign='left';
}

function drawPowerups(){
  powerups.forEach(p=>{
    const meta = POWER_META[p.type];
    ctx.save();
    ctx.translate(p.x,p.y);
    const s = 1 + Math.sin(p.phase*5)*0.08;
    ctx.scale(s,s);
    ctx.fillStyle = meta.color;
    ctx.strokeStyle = '#1B1035'; ctx.lineWidth=2.5;
    ctx.beginPath(); ctx.arc(0,0,14,0,Math.PI*2); ctx.fill(); ctx.stroke();
    ctx.fillStyle = '#1B1035';
    ctx.font = '700 13px Baloo 2, sans-serif';
    ctx.textAlign='center'; ctx.textBaseline='middle';
    ctx.fillText(meta.icon, 0, 1);
    ctx.restore();
  });
  ctx.textAlign='left'; ctx.textBaseline='alphabetic';
}

function drawEnemies(){
  enemies.forEach(e=> drawAlien(e));
}

function drawBoss(){
  if(!boss) return;
  ctx.save();
  ctx.translate(boss.x, boss.y);
  if(boss.dead) ctx.globalAlpha = clamp(boss.deathTimer/1.6,0,1);
  const flash = boss.hitFlash>0;

  // cape
  ctx.fillStyle = '#7a1245';
  ctx.beginPath(); ctx.moveTo(-30,10); ctx.lineTo(-46,50); ctx.lineTo(-10,30); ctx.closePath(); ctx.fill();
  ctx.beginPath(); ctx.moveTo(30,10); ctx.lineTo(46,50); ctx.lineTo(10,30); ctx.closePath(); ctx.fill();

  // body / armor
  ctx.fillStyle = flash ? '#ffffff' : '#9B5DE5';
  ctx.strokeStyle = '#1B1035'; ctx.lineWidth=4;
  ctx.beginPath(); ctx.ellipse(0,20,40,32,0,0,Math.PI*2); ctx.fill(); ctx.stroke();

  // helmet
  ctx.fillStyle = flash ? '#ffffff' : '#FF9F45';
  ctx.beginPath(); ctx.arc(0,-24,38,0,Math.PI*2); ctx.fill(); ctx.stroke();
  // visor
  ctx.fillStyle='#1B1035';
  ctx.beginPath(); ctx.ellipse(0,-20,24,16,0,0,Math.PI*2); ctx.fill();
  // eyes
  ctx.fillStyle='#4DEEEA';
  ctx.beginPath(); ctx.arc(-10,-20,6,0,Math.PI*2); ctx.arc(10,-20,6,0,Math.PI*2); ctx.fill();
  // eyebrows
  ctx.strokeStyle='#1B1035'; ctx.lineWidth=4;
  ctx.beginPath(); ctx.moveTo(-18,-34); ctx.quadraticCurveTo(-10,-42,-2,-34); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(2,-34); ctx.quadraticCurveTo(10,-42,18,-34); ctx.stroke();
  // dramatic mouth
  ctx.strokeStyle = '#1B1035'; ctx.lineWidth=3;
  ctx.beginPath(); ctx.arc(0,-4,10,0.1*Math.PI,0.9*Math.PI); ctx.stroke();

  ctx.restore();
  ctx.globalAlpha = 1;

  // health bar
  if(!boss.dead){
    const bw = 220, bh=16, bx = W/2-bw/2, by = 18;
    ctx.fillStyle = 'rgba(0,0,0,0.4)';
    ctx.beginPath(); ctx.roundRect(bx-3,by-3,bw+6,bh+6,8); ctx.fill();
    ctx.fillStyle = '#3a1f7a';
    ctx.beginPath(); ctx.roundRect(bx,by,bw,bh,6); ctx.fill();
    const pct = clamp(boss.hp/boss.maxHp,0,1);
    ctx.fillStyle = pct>0.5 ? '#6BCB77' : (pct>0.25 ? '#FFD23F' : '#FF5DA2');
    ctx.beginPath(); ctx.roundRect(bx,by,bw*pct,bh,6); ctx.fill();
    ctx.strokeStyle='#1B1035'; ctx.lineWidth=2;
    ctx.beginPath(); ctx.roundRect(bx,by,bw,bh,6); ctx.stroke();
    ctx.fillStyle='#fff'; ctx.font='700 12px Baloo 2, sans-serif'; ctx.textAlign='center';
    ctx.fillText('CAPTAIN ZORBO', W/2, by+bh+16);
    ctx.textAlign='left';
  }
}

function render(){
  ctx.save();
  ctx.clearRect(0,0,W,H);
  ctx.save();
  if(shakeTime>0){
    ctx.translate(rand(-shakeMag,shakeMag), rand(-shakeMag,shakeMag));
  }
  drawBackground();
  if(state===STATE.PLAYING || state===STATE.PAUSED || state===STATE.BOSS_WARNING || state===STATE.LEVEL_INTRO){
    drawPowerups();
    drawEnemies();
    drawBoss();
    drawBullets();
    drawPlayer();
    drawParticles();
    drawFloatTexts();
  }
  ctx.restore();
  ctx.restore();
}

// ---------- MAIN LOOP ----------
function loop(now){
  const dt = Math.min((now-lastTime)/1000, 0.033);
  lastTime = now;
  update(dt);
  render();
  requestAnimationFrame(loop);
}
requestAnimationFrame((t)=>{ lastTime=t; requestAnimationFrame(loop); });

// ---------- PAUSE ----------
function togglePause(){
  if(state===STATE.PLAYING){
    stateBeforePause = state;
    state = STATE.PAUSED;
    showPanel(panels.pause);
  } else if(state===STATE.PAUSED){
    hideAllPanels();
    state = STATE.PLAYING;
  }
}

// ---------- BUTTON WIRING ----------
function goMenu(){
  state = STATE.MENU;
  hud.classList.add('hidden');
  btnPauseTop.classList.add('hidden');
  showPanel(panels.menu);
}

document.getElementById('btnPlay').addEventListener('click', ()=>{ ensureAudio(); score=0; startLevel(1); });
document.getElementById('btnHowTo').addEventListener('click', ()=> showPanel(panels.howto));
document.getElementById('btnHowToBack').addEventListener('click', ()=> showPanel(panels.menu));
document.getElementById('btnScores').addEventListener('click', ()=>{
  document.getElementById('scoreBest').textContent = sessionBest;
  document.getElementById('scoreLevels').textContent = sessionLevels;
  document.getElementById('scoreCombo').textContent = 'x'+sessionBestCombo;
  showPanel(panels.scores);
});
document.getElementById('btnScoresBack').addEventListener('click', ()=> showPanel(panels.menu));
document.getElementById('btnSettings').addEventListener('click', ()=> showPanel(panels.settings));
document.getElementById('btnSettingsBack').addEventListener('click', ()=> showPanel(panels.menu));
document.getElementById('btnNextLevel').addEventListener('click', ()=>{
  if(currentLevel>=MAX_LEVEL){ goMenu(); return; }
  startLevel(currentLevel+1);
});
document.getElementById('btnRetry').addEventListener('click', ()=>{ score=0; startLevel(1); });
document.getElementById('btnGoMenu').addEventListener('click', goMenu);
document.getElementById('btnPlayAgain').addEventListener('click', ()=>{ score=0; startLevel(1); });
document.getElementById('btnVictoryMenu').addEventListener('click', goMenu);
document.getElementById('btnResume').addEventListener('click', togglePause);
document.getElementById('btnPauseMenu').addEventListener('click', ()=>{ hideAllPanels(); goMenu(); });
btnPauseTop.addEventListener('click', togglePause);

document.getElementById('selSound').addEventListener('change', (e)=>{ soundOn = e.target.value==='on'; if(soundOn) ensureAudio(); });
document.getElementById('selShake').addEventListener('change', (e)=>{ screenShakeOn = e.target.value==='on'; });
document.getElementById('selDifficulty').addEventListener('change', (e)=>{ difficulty = e.target.value; });

// polyfill roundRect for older browsers
if(!CanvasRenderingContext2D.prototype.roundRect){
  CanvasRenderingContext2D.prototype.roundRect = function(x,y,w,h,r){
    if(typeof r === 'number') r = {tl:r,tr:r,br:r,bl:r};
    this.beginPath();
    this.moveTo(x+r.tl,y);
    this.lineTo(x+w-r.tr,y);
    this.arcTo(x+w,y,x+w,y+r.tr,r.tr);
    this.lineTo(x+w,y+h-r.br);
    this.arcTo(x+w,y+h,x+w-r.br,y+h,r.br);
    this.lineTo(x+r.bl,y+h);
    this.arcTo(x,y+h,x,y+h-r.bl,r.bl);
    this.lineTo(x,y+r.tl);
    this.arcTo(x,y,x+r.tl,y,r.tl);
    this.closePath();
    return this;
  };
}

})();
</script>
</body>
</html>
