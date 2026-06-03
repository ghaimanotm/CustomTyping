<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Typing Practice</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: system-ui, sans-serif; background: #1a1a2e; color: #e0e0e0; min-height: 100vh; }
.app { max-width: 920px; margin: 0 auto; padding: 20px; }
h1 { font-size: 22px; font-weight: 500; color: #a78bfa; text-align: center; margin-bottom: 4px; }
.subtitle { text-align: center; font-size: 13px; color: #666; margin-bottom: 20px; }

.tabs { display: flex; gap: 4px; margin-bottom: 18px; background: #12122a; border-radius: 10px; padding: 4px; }
.tab { flex: 1; padding: 8px; border: none; border-radius: 8px; background: transparent; color: #888; cursor: pointer; font-size: 14px; transition: all 0.2s; }
.tab.active { background: #7c3aed; color: #fff; font-weight: 500; }

.paste-area { background: #12122a; border-radius: 12px; padding: 16px; margin-bottom: 16px; }
.paste-area label { font-size: 13px; color: #888; display: block; margin-bottom: 8px; }
.paste-area textarea { width: 100%; height: 130px; background: #0d0d1f; border: 1px solid #2d2d50; border-radius: 8px; color: #e0e0e0; font-family: 'Courier New', monospace; font-size: 13px; padding: 10px; resize: vertical; outline: none; }
.paste-area textarea:focus { border-color: #7c3aed; }
.start-btn { margin-top: 10px; width: 100%; padding: 10px; background: #7c3aed; border: none; border-radius: 8px; color: #fff; font-size: 15px; font-weight: 500; cursor: pointer; }
.start-btn:hover { background: #6d28d9; }
.presets { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 12px; }
.preset-btn { padding: 5px 12px; background: #1e1e3a; border: 1px solid #2d2d50; border-radius: 20px; color: #aaa; font-size: 12px; cursor: pointer; }
.preset-btn:hover { border-color: #7c3aed; color: #a78bfa; }

.stats { display: flex; gap: 10px; margin-bottom: 14px; }
.stat { flex: 1; background: #12122a; border-radius: 10px; padding: 10px; text-align: center; }
.stat-val { font-size: 20px; font-weight: 500; color: #a78bfa; }
.stat-lbl { font-size: 11px; color: #555; margin-top: 2px; }
.progress-bar { height: 4px; background: #1e1e3a; border-radius: 2px; margin-bottom: 14px; overflow: hidden; }
.progress-fill { height: 100%; background: #7c3aed; border-radius: 2px; transition: width 0.3s; }

.next-strip { display: flex; align-items: center; gap: 12px; background: #12122a; border-radius: 10px; padding: 10px 16px; margin-bottom: 14px; flex-wrap: wrap; }
.next-key-badge { background: #0d0d1f; border: 1px solid #3d3d60; border-radius: 8px; padding: 6px 14px; font-family: 'Courier New', monospace; font-size: 20px; font-weight: 600; color: #a78bfa; min-width: 42px; text-align: center; }
.next-meta { flex: 1; min-width: 140px; }
.next-meta .fn { font-size: 13px; color: #e0e0e0; margin-bottom: 2px; }
.next-meta .fh { font-size: 11px; color: #666; }
.hand-badge { padding: 5px 12px; border-radius: 16px; font-size: 12px; background: #1e1e3a; color: #94a3b8; }

.legend { display: flex; gap: 14px; justify-content: center; margin-bottom: 10px; flex-wrap: wrap; }
.legend-item { display: flex; align-items: center; gap: 5px; font-size: 11px; color: #888; }
.legend-swatch { width: 8px; height: 18px; border-radius: 4px 4px 2px 2px; border: 1px solid rgba(255,255,255,0.15); }

/* Keyboard */
.keyboard-wrap {
  background: #12122a; border-radius: 12px;
  padding: 16px 12px 80px; /* extra bottom padding for fingers */
  margin-bottom: 14px; overflow-x: auto; position: relative;
}
.keyboard { display: flex; flex-direction: column; gap: 5px; min-width: 570px; }
.kb-row { display: flex; gap: 4px; justify-content: center; }

.key {
  display: flex; align-items: center; justify-content: center;
  height: 42px; min-width: 40px; border-radius: 7px;
  background: #1e1e3a; border: 1px solid #2d2d50;
  font-size: 12px; color: #aaa;
  position: relative; user-select: none; flex-shrink: 0;
  transition: background 0.15s, border-color 0.15s;
}
.key .main { font-size: 13px; }
.key .sh { font-size: 9px; color: #555; position: absolute; top: 4px; left: 50%; transform: translateX(-50%); }
.key.w1 { min-width: 58px; }
.key.w2 { min-width: 74px; }
.key.w3 { min-width: 92px; }
.key.space { min-width: 250px; }

/* Finger color zones on keys */
.lp { background:#221e2a; border-color:#3d2d4433; }
.lr { background:#1e1e2e; border-color:#3d2d5433; }
.lm { background:#1a2420; border-color:#2d4433; }
.li { background:#1a2030; border-color:#2d3d5433; }
.tb { background:#1e1e2a; border-color:#2d2d4433; }
.ri { background:#1a2030; border-color:#2d3d5433; }
.rm { background:#1a2420; border-color:#2d4433; }
.rr { background:#1e1e2e; border-color:#3d2d5433; }
.rp { background:#221e2a; border-color:#3d2d4433; }

.key.next-key { border-width: 2px; z-index: 3; }
.next-key.lp,.next-key.rp { background:#3d1515; border-color:#ef4444; color:#fca5a5; }
.next-key.lr,.next-key.rr { background:#2d155b; border-color:#a855f7; color:#d8b4fe; }
.next-key.lm,.next-key.rm { background:#153d20; border-color:#22c55e; color:#86efac; }
.next-key.li,.next-key.ri { background:#152040; border-color:#3b82f6; color:#93c5fd; }
.next-key.tb             { background:#2a2a3a; border-color:#94a3b8; color:#cbd5e1; }

@keyframes pressOK {
  0%   { transform: scale(1); }
  25%  { transform: scale(0.84); background: #166534; border-color: #22c55e; box-shadow: 0 0 14px #22c55e88; }
  65%  { transform: scale(1.06); }
  100% { transform: scale(1); }
}
@keyframes pressBAD {
  0%   { transform: translateX(0) scale(1); }
  15%  { transform: translateX(-5px) scale(0.90); background: #7f1d1d; border-color: #ef4444; box-shadow: 0 0 14px #ef444488; }
  30%  { transform: translateX(5px); }
  45%  { transform: translateX(-4px); }
  60%  { transform: translateX(4px); }
  75%  { transform: translateX(-2px); }
  100% { transform: translateX(0) scale(1); }
}
.key.anim-ok  { animation: pressOK  0.28s ease forwards; }
.key.anim-bad { animation: pressBAD 0.38s ease forwards; }

/* SVG hand fingers */
#hands-svg { overflow: visible !important; }
.svg-finger path.fbody { transition: fill 0.12s ease, stroke 0.12s ease, stroke-width 0.12s ease; }
.svg-finger { transition: transform 0.13s cubic-bezier(0.34,1.2,0.64,1); }
.svg-finger.next-glow .fbody { stroke: rgba(167,139,250,0.75) !important; fill: rgba(124,58,237,0.12) !important; }

/* Practice text */
.practice-box { background: #12122a; border-radius: 12px; padding: 18px; margin-bottom: 14px; }
.text-display { font-family: 'Courier New', monospace; font-size: 17px; line-height: 1.9; letter-spacing: 0.4px; user-select: none; word-break: break-all; min-height: 72px; }
.ch.correct { color: #34d399; }
.ch.wrong   { color: #f87171; background: rgba(248,113,113,0.15); border-radius: 2px; }
.ch.cursor  { border-left: 2px solid #a78bfa; animation: blink 1s step-end infinite; }
.ch.pending { color: #383858; }
@keyframes blink { 0%,100%{border-color:#a78bfa} 50%{border-color:transparent} }
.typing-input { width:100%; margin-top:12px; padding:10px 14px; background:#0d0d1f; border:1px solid #2d2d50; border-radius:8px; color:#e0e0e0; font-family:'Courier New',monospace; font-size:15px; outline:none; }
.typing-input:focus { border-color:#7c3aed; }

.result-box { background:#12122a; border-radius:12px; padding:24px; text-align:center; margin-bottom:14px; display:none; }
.result-box h2 { color:#a78bfa; margin-bottom:16px; font-size:20px; }
.result-stats { display:flex; justify-content:center; gap:24px; margin-bottom:20px; }
.result-stat .v { font-size:28px; font-weight:500; color:#34d399; }
.result-stat .l { font-size:12px; color:#666; }
.rbtn { padding:10px 26px; background:#7c3aed; border:none; border-radius:8px; color:#fff; font-size:14px; cursor:pointer; }
</style>
</head>
<body>
<div class="app">
  <h1>⌨ Typing Practice</h1>
  <p class="subtitle">Paste any text or code — real finger animations guide every keystroke</p>

  <div class="tabs">
    <button class="tab active" onclick="showTab('practice')">Practice</button>
    <button class="tab" onclick="showTab('setup')">Custom Text</button>
  </div>

  <div id="tab-setup" style="display:none">
    <div class="paste-area">
      <label>Paste your text or code here:</label>
      <div class="presets">
        <button class="preset-btn" onclick="loadPreset('home')">Home row drill</button>
        <button class="preset-btn" onclick="loadPreset('common')">Common words</button>
        <button class="preset-btn" onclick="loadPreset('code')">Python snippet</button>
        <button class="preset-btn" onclick="loadPreset('js')">JavaScript</button>
        <button class="preset-btn" onclick="loadPreset('prose')">Sentences</button>
      </div>
      <textarea id="custom-text" placeholder="Paste anything here — prose, code, commands..."></textarea>
      <button class="start-btn" onclick="startCustom()">Start Practice →</button>
    </div>
  </div>

  <div id="tab-practice">
    <div class="stats">
      <div class="stat"><div class="stat-val" id="wpm">0</div><div class="stat-lbl">WPM</div></div>
      <div class="stat"><div class="stat-val" id="acc">100%</div><div class="stat-lbl">Accuracy</div></div>
      <div class="stat"><div class="stat-val" id="timer">0s</div><div class="stat-lbl">Time</div></div>
      <div class="stat"><div class="stat-val" id="errs">0</div><div class="stat-lbl">Errors</div></div>
    </div>
    <div class="progress-bar"><div class="progress-fill" id="prog" style="width:0%"></div></div>

    <div class="next-strip">
      <div class="next-key-badge" id="nk">–</div>
      <div class="next-meta">
        <div class="fn" id="nfn">–</div>
        <div class="fh" id="nfh">–</div>
      </div>
      <div class="hand-badge" id="nhb">–</div>
    </div>

    <div class="legend">
      <div class="legend-item"><div class="legend-swatch" style="background:linear-gradient(180deg,#1e55d8,#0f2f8a)"></div>Index</div>
      <div class="legend-item"><div class="legend-swatch" style="background:linear-gradient(180deg,#177a3c,#0d4d23)"></div>Middle</div>
      <div class="legend-item"><div class="legend-swatch" style="background:linear-gradient(180deg,#8b22ce,#5b0f9a)"></div>Ring</div>
      <div class="legend-item"><div class="legend-swatch" style="background:linear-gradient(180deg,#c81c1c,#7f1010)"></div>Pinky</div>
      <div class="legend-item"><div class="legend-swatch" style="background:linear-gradient(180deg,#16a34a,#0d4d20);border-color:#4ade8044"></div>Correct ✓</div>
      <div class="legend-item"><div class="legend-swatch" style="background:linear-gradient(180deg,#dc2626,#7f1d1d);border-color:#f8717144"></div>Wrong ✗</div>
    </div>

    <!-- Keyboard + SVG hand overlay -->
    <div class="keyboard-wrap" id="kb-wrap">
      <div class="keyboard" id="keyboard"></div>
      <!-- SVG hands injected by JS -->
    </div>

    <div class="practice-box">
      <div class="text-display" id="text-display"></div>
      <input type="text" class="typing-input" id="typing-input"
        placeholder="Click here and start typing..."
        autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false">
    </div>

    <div class="result-box" id="result-box">
      <h2>🎉 Done!</h2>
      <div class="result-stats">
        <div class="result-stat"><div class="v" id="r-wpm">0</div><div class="l">WPM</div></div>
        <div class="result-stat"><div class="v" id="r-acc">0%</div><div class="l">Accuracy</div></div>
        <div class="result-stat"><div class="v" id="r-time">0s</div><div class="l">Time</div></div>
        <div class="result-stat"><div class="v" id="r-errs">0</div><div class="l">Errors</div></div>
      </div>
      <button class="rbtn" onclick="retry()">Try Again</button>
      <button class="rbtn" style="margin-left:10px;background:#2d2d50" onclick="showTab('setup')">New Text</button>
    </div>
  </div>
</div>

<script>
/* =====================================================================
   FINGER + KEY MAPS
===================================================================== */
const FM = {
  '`':{fi:0,hand:'left', name:'Left pinky',  hint:'Stretch left pinky to ` key'},
  '1':{fi:0,hand:'left', name:'Left pinky',  hint:'Left pinky on 1'},
  '2':{fi:1,hand:'left', name:'Left ring',   hint:'Left ring on 2'},
  '3':{fi:2,hand:'left', name:'Left middle', hint:'Left middle on 3'},
  '4':{fi:3,hand:'left', name:'Left index',  hint:'Left index on 4'},
  '5':{fi:3,hand:'left', name:'Left index',  hint:'Left index stretches to 5'},
  '6':{fi:6,hand:'right',name:'Right index', hint:'Right index stretches to 6'},
  '7':{fi:6,hand:'right',name:'Right index', hint:'Right index on 7'},
  '8':{fi:7,hand:'right',name:'Right middle',hint:'Right middle on 8'},
  '9':{fi:8,hand:'right',name:'Right ring',  hint:'Right ring on 9'},
  '0':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on 0'},
  '-':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on -'},
  '=':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on ='},
  'q':{fi:0,hand:'left', name:'Left pinky',  hint:'Left pinky on Q'},
  'w':{fi:1,hand:'left', name:'Left ring',   hint:'Left ring on W'},
  'e':{fi:2,hand:'left', name:'Left middle', hint:'Left middle on E'},
  'r':{fi:3,hand:'left', name:'Left index',  hint:'Left index on R'},
  't':{fi:3,hand:'left', name:'Left index',  hint:'Left index stretches to T'},
  'y':{fi:6,hand:'right',name:'Right index', hint:'Right index stretches to Y'},
  'u':{fi:6,hand:'right',name:'Right index', hint:'Right index on U'},
  'i':{fi:7,hand:'right',name:'Right middle',hint:'Right middle on I'},
  'o':{fi:8,hand:'right',name:'Right ring',  hint:'Right ring on O'},
  'p':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on P'},
  '[':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on ['},
  ']':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on ]'},
  '\\':{fi:9,hand:'right',name:'Right pinky',hint:'Right pinky on \\'},
  'a':{fi:0,hand:'left', name:'Left pinky',  hint:'Left pinky — home key A'},
  's':{fi:1,hand:'left', name:'Left ring',   hint:'Left ring — home key S'},
  'd':{fi:2,hand:'left', name:'Left middle', hint:'Left middle — home key D'},
  'f':{fi:3,hand:'left', name:'Left index',  hint:'Left index — home key F (feel the bump!)'},
  'g':{fi:3,hand:'left', name:'Left index',  hint:'Left index stretches to G'},
  'h':{fi:6,hand:'right',name:'Right index', hint:'Right index stretches to H'},
  'j':{fi:6,hand:'right',name:'Right index', hint:'Right index — home key J (feel the bump!)'},
  'k':{fi:7,hand:'right',name:'Right middle',hint:'Right middle — home key K'},
  'l':{fi:8,hand:'right',name:'Right ring',  hint:'Right ring — home key L'},
  ';':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky — home key ;'},
  "'":{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on \''},
  'z':{fi:0,hand:'left', name:'Left pinky',  hint:'Left pinky on Z'},
  'x':{fi:1,hand:'left', name:'Left ring',   hint:'Left ring on X'},
  'c':{fi:2,hand:'left', name:'Left middle', hint:'Left middle on C'},
  'v':{fi:3,hand:'left', name:'Left index',  hint:'Left index on V'},
  'b':{fi:3,hand:'left', name:'Left index',  hint:'Left index stretches to B'},
  'n':{fi:6,hand:'right',name:'Right index', hint:'Right index stretches to N'},
  'm':{fi:6,hand:'right',name:'Right index', hint:'Right index on M'},
  ',':{fi:7,hand:'right',name:'Right middle',hint:'Right middle on ,'},
  '.':{fi:8,hand:'right',name:'Right ring',  hint:'Right ring on .'},
  '/':{fi:9,hand:'right',name:'Right pinky', hint:'Right pinky on /'},
  ' ' :{fi:5,hand:'both', name:'Either thumb',hint:'Spacebar — use either thumb'},
  '\n':{fi:9,hand:'right',name:'Right pinky', hint:'Enter — right pinky'},
  '\t':{fi:0,hand:'left', name:'Left pinky',  hint:'Tab — left pinky'},
};

const SHIFT_TO_BASE = {
  '~':'`','!':'1','@':'2','#':'3','$':'4','%':'5','^':'6','&':'7','*':'8','(':'9',')':'0','_':'-','+':'=',
  'Q':'q','W':'w','E':'e','R':'r','T':'t','Y':'y','U':'u','I':'i','O':'o','P':'p','{':'[','}':']','|':'\\',
  'A':'a','S':'s','D':'d','F':'f','G':'g','H':'h','J':'j','K':'k','L':'l',':':';','"':"'",
  'Z':'z','X':'x','C':'c','V':'v','B':'b','N':'n','M':'m','<':',','>':'.','?':'/',
};

const FI_CLS = {0:'lp',1:'lr',2:'lm',3:'li',5:'tb',6:'ri',7:'rm',8:'rr',9:'rp'};

const KB_ROWS = [
  [['`~','k-grave',null,0],['1!','k-1',null,0],['2@','k-2',null,1],['3#','k-3',null,2],['4$','k-4',null,3],['5%','k-5',null,3],['6^','k-6',null,6],['7&','k-7',null,6],['8*','k-8',null,7],['9(','k-9',null,8],['0)','k-0',null,9],['-_','k-minus',null,9],['=+','k-eq',null,9],['⌫','k-bs','w2',9]],
  [['Tab','k-tab','w1',0],['Q','k-q',null,0],['W','k-w',null,1],['E','k-e',null,2],['R','k-r',null,3],['T','k-t',null,3],['Y','k-y',null,6],['U','k-u',null,6],['I','k-i',null,7],['O','k-o',null,8],['P','k-p',null,9],['[{','k-lb',null,9],[']|','k-rb',null,9],['\\|','k-bk','w1',9]],
  [['Caps','k-caps','w2',0],['A','k-a',null,0],['S','k-s',null,1],['D','k-d',null,2],['F','k-f',null,3],['G','k-g',null,3],['H','k-h',null,6],['J','k-j',null,6],['K','k-k',null,7],['L','k-l',null,8],[';:','k-sc',null,9],["'\"","k-qt",null,9],['↵','k-enter','w3',9]],
  [['⇧','k-lshift','w3',0],['Z','k-z',null,0],['X','k-x',null,1],['C','k-c',null,2],['V','k-v',null,3],['B','k-b',null,3],['N','k-n',null,6],['M','k-m',null,6],[',<','k-cm',null,7],['.>','k-dt',null,8],['/?','k-sl',null,9],['⇧','k-rshift','w3',9]],
  [['Ctrl','k-lctrl','w1',0],['Alt','k-lalt','w1',0],['Space','k-space','space',5],['Alt','k-ralt','w1',9],['Ctrl','k-rctrl','w1',9]]
];

const CHAR_TO_KEY = {
  '`':'k-grave','1':'k-1','2':'k-2','3':'k-3','4':'k-4','5':'k-5','6':'k-6','7':'k-7','8':'k-8','9':'k-9','0':'k-0','-':'k-minus','=':'k-eq',
  'q':'k-q','w':'k-w','e':'k-e','r':'k-r','t':'k-t','y':'k-y','u':'k-u','i':'k-i','o':'k-o','p':'k-p','[':'k-lb',']':'k-rb','\\':'k-bk',
  'a':'k-a','s':'k-s','d':'k-d','f':'k-f','g':'k-g','h':'k-h','j':'k-j','k':'k-k','l':'k-l',';':'k-sc',"'":'k-qt',
  'z':'k-z','x':'k-x','c':'k-c','v':'k-v','b':'k-b','n':'k-n','m':'k-m',',':'k-cm','.':'k-dt','/':'k-sl',
  ' ':'k-space','\n':'k-enter','\t':'k-tab'
};

const PRESETS = {
  home: 'asdf jkl; asdf jkl; asdfjkl; fjdk slaf jkds aldf jkas fdlj salk jfda lksj dfak ljsd fakl jdsa',
  common: 'the quick brown fox jumps over the lazy dog. how are you doing today? i hope you have a wonderful day.',
  code: 'def greet(name):\n    print(f"Hello, {name}!")\n\nfor i in range(10):\n    greet(f"user_{i}")',
  js: 'const getData = async (url) => {\n  const res = await fetch(url);\n  return res.json();\n};\n\nconst data = await getData("/api/data");',
  prose: 'Practice makes perfect. The more you type, the faster you will become. Keep your fingers on the home row keys and reach for other keys without moving your wrists too much.'
};

/* Finger color fills and strokes (idle) */
const FINGER_STYLE = {
  lp: { fill:'rgba(160,30,30,0.13)',  stroke:'rgba(220,80,80,0.55)',  glow:'#ef4444' },
  lr: { fill:'rgba(100,20,160,0.13)', stroke:'rgba(168,85,247,0.55)', glow:'#a855f7' },
  lm: { fill:'rgba(15,100,50,0.13)',  stroke:'rgba(34,197,94,0.55)',  glow:'#22c55e' },
  li: { fill:'rgba(20,60,180,0.13)',  stroke:'rgba(59,130,246,0.55)', glow:'#3b82f6' },
  tb: { fill:'rgba(60,70,90,0.13)',   stroke:'rgba(148,163,184,0.45)',glow:'#94a3b8' },
  ri: { fill:'rgba(20,60,180,0.13)',  stroke:'rgba(59,130,246,0.55)', glow:'#3b82f6' },
  rm: { fill:'rgba(15,100,50,0.13)',  stroke:'rgba(34,197,94,0.55)',  glow:'#22c55e' },
  rr: { fill:'rgba(100,20,160,0.13)', stroke:'rgba(168,85,247,0.55)', glow:'#a855f7' },
  rp: { fill:'rgba(160,30,30,0.13)',  stroke:'rgba(220,80,80,0.55)',  glow:'#ef4444' },
};

/* =====================================================================
   STATE
===================================================================== */
let currentText = PRESETS.home;
let typed = '', startTime = null, timerInt = null, errors = 0, done = false;
let activeNextKey = null, activeShiftKey = null, activeNextFinger = null;
let svgFingers = {}; // fi → { outerG, innerG, bodyPath, nailEl }

/* =====================================================================
   BUILD KEYBOARD
===================================================================== */
function buildKeyboard() {
  const kb = document.getElementById('keyboard');
  kb.innerHTML = '';
  KB_ROWS.forEach(row => {
    const rowDiv = document.createElement('div');
    rowDiv.className = 'kb-row';
    row.forEach(([label, id, wcls, fi]) => {
      const el = document.createElement('div');
      el.className = ['key', wcls||'', FI_CLS[fi]].filter(Boolean).join(' ');
      el.id = id;
      const special = ['Tab','Caps','⌫','↵','⇧','Ctrl','Alt','Space'].includes(label);
      el.innerHTML = (!special && label.length===2)
        ? `<span class="sh">${label[1]}</span><span class="main">${label[0]}</span>`
        : `<span class="main">${label}</span>`;
      rowDiv.appendChild(el);
    });
    kb.appendChild(rowDiv);
  });
}

/* =====================================================================
   BUILD SVG HANDS
===================================================================== */
function buildHandsSVG() {
  document.getElementById('hands-svg')?.remove();
  svgFingers = {};

  const wrap = document.getElementById('kb-wrap');
  const wRect = wrap.getBoundingClientRect();
  const NS = 'http://www.w3.org/2000/svg';

  const svg = document.createElementNS(NS, 'svg');
  svg.id = 'hands-svg';
  svg.setAttribute('xmlns', NS);
  Object.assign(svg.style, {
    position:'absolute', top:'0', left:'0',
    width:'100%', height:'100%',
    pointerEvents:'none', zIndex:'10', overflow:'visible'
  });

  /* ---- helper: build one finger ---- */
  function makeFinger(fi, keyId, angleDeg, isThumb, cc, spaceRatio) {
    const keyEl = document.getElementById(keyId);
    if (!keyEl) return;
    const kr = keyEl.getBoundingClientRect();

    /* key center in wrap-relative coords */
    let kx = kr.left - wRect.left + kr.width  * (spaceRatio !== undefined ? (0.5 + spaceRatio) : 0.5);
    const ky = kr.top  - wRect.top  + kr.height * 0.5;

    const len = isThumb ? 52 : 82;
    const hw  = isThumb ? 11 : 8;   /* half-width at widest */

    /* --- SVG path: tip at local (0,0), body goes DOWN (+y) --- */
    function fingerPath(len, hw) {
      /* sides taper: wider in middle, narrower at tip and base */
      const wMid = hw + 2;
      const t = len;
      return [
        `M 0,0`,
        /* right side */
        `C ${hw},0 ${wMid},${t*0.25} ${wMid},${t*0.50}`,
        `C ${wMid},${t*0.72} ${hw+1},${t*0.85} ${hw-1},${t*0.94}`,
        `Q ${hw-3},${t} 0,${t}`,
        /* left side (mirror) */
        `Q ${-(hw-3)},${t} ${-(hw-1)},${t*0.94}`,
        `C ${-(hw+1)},${t*0.85} ${-wMid},${t*0.72} ${-wMid},${t*0.50}`,
        `C ${-wMid},${t*0.25} ${-hw},0 0,0 Z`
      ].join(' ');
    }

    /* outer group: positions finger, applies viewing angle */
    const outer = document.createElementNS(NS, 'g');
    outer.setAttribute('transform', `translate(${kx},${ky}) rotate(${angleDeg})`);

    /* inner group: what gets press-animated */
    const inner = document.createElementNS(NS, 'g');
    inner.id = `svgf-${fi}`;
    inner.setAttribute('class', `svg-finger sf-${cc}`);

    const style = FINGER_STYLE[cc];

    /* main body */
    const body = document.createElementNS(NS, 'path');
    body.setAttribute('class', 'fbody');
    body.setAttribute('d', fingerPath(len, hw));
    body.setAttribute('fill',         style.fill);
    body.setAttribute('stroke',       style.stroke);
    body.setAttribute('stroke-width', '1.8');
    body.setAttribute('stroke-linejoin','round');

    /* fingernail ellipse near tip */
    const nail = document.createElementNS(NS, 'ellipse');
    nail.setAttribute('cx', '0');
    nail.setAttribute('cy', `${len * 0.13}`);
    nail.setAttribute('rx', `${hw - 2}`);
    nail.setAttribute('ry', `${len * 0.10}`);
    nail.setAttribute('fill',   'rgba(220,235,255,0.20)');
    nail.setAttribute('stroke', 'rgba(180,210,240,0.38)');
    nail.setAttribute('stroke-width', '1');

    /* highlight sheen on left side of finger */
    const sheen = document.createElementNS(NS, 'path');
    sheen.setAttribute('d', `M ${-hw+2},${len*0.08} C ${-hw+1},${len*0.28} ${-hw},${len*0.50} ${-hw+2},${len*0.68}`);
    sheen.setAttribute('stroke', 'rgba(255,255,255,0.14)');
    sheen.setAttribute('stroke-width', '2');
    sheen.setAttribute('fill', 'none');
    sheen.setAttribute('stroke-linecap', 'round');

    /* knuckle crease lines (not on thumb) */
    if (!isThumb) {
      [[0.38, 0.33], [0.60, 0.56]].forEach(([y1, y2]) => {
        const crease = document.createElementNS(NS, 'path');
        crease.setAttribute('d', `M ${-(hw+1)},${len*y1} Q 0,${len*y2} ${hw+1},${len*y1}`);
        crease.setAttribute('stroke', 'rgba(140,170,205,0.28)');
        crease.setAttribute('stroke-width', '1.2');
        crease.setAttribute('fill', 'none');
        crease.setAttribute('stroke-linecap','round');
        inner.appendChild(crease);
      });
    }

    inner.appendChild(body);
    inner.appendChild(nail);
    inner.appendChild(sheen);
    outer.appendChild(inner);
    svg.appendChild(outer);

    svgFingers[fi] = { inner, body, nail, cc, kx, ky };
  }

  /* ---- define all 10 fingers ---- */
  /* fi, keyId, angle (° from straight-down), thumb, colorClass, spacebarOffset */
  makeFinger(0,   'k-a',     -20, false, 'lp');
  makeFinger(1,   'k-s',     -11, false, 'lr');
  makeFinger(2,   'k-d',      -3, false, 'lm');
  makeFinger(3,   'k-f',       6, false, 'li');
  makeFinger('lt','k-space',  60, true,  'tb', -0.22); /* left thumb */
  makeFinger(6,   'k-j',      -6, false, 'ri');
  makeFinger(7,   'k-k',       3, false, 'rm');
  makeFinger(8,   'k-l',      11, false, 'rr');
  makeFinger(9,   'k-sc',     20, false, 'rp');
  makeFinger('rt','k-space', -60, true,  'tb',  0.22); /* right thumb */

  wrap.appendChild(svg);
}

/* =====================================================================
   PRESS FINGER
===================================================================== */
function pressFingerSVG(fi, isCorrect) {
  const sf = svgFingers[fi];
  if (!sf) return;
  const { inner, body } = sf;

  /* squish = spreads wider and shortens, like a real fingertip pressing */
  inner.style.transform = 'scaleX(1.22) scaleY(0.85)';
  body.setAttribute('fill',   isCorrect ? 'rgba(34,197,94,0.40)'  : 'rgba(239,68,68,0.38)');
  body.setAttribute('stroke', isCorrect ? '#4ade80' : '#f87171');
  body.setAttribute('stroke-width', '2.2');

  const delay = isCorrect ? 200 : 340;
  setTimeout(() => {
    inner.style.transform = '';
    const style = FINGER_STYLE[sf.cc];
    body.setAttribute('fill',         style.fill);
    body.setAttribute('stroke',       style.stroke);
    body.setAttribute('stroke-width', '1.8');
  }, delay);
}

/* =====================================================================
   HIGHLIGHT NEXT FINGER
===================================================================== */
function setNextFingerGlow(fi, on) {
  const sf = svgFingers[fi];
  if (!sf) return;
  const style = FINGER_STYLE[sf.cc];
  if (on) {
    sf.body.setAttribute('stroke', 'rgba(167,139,250,0.85)');
    sf.body.setAttribute('fill',   'rgba(124,58,237,0.18)');
    sf.body.setAttribute('stroke-width', '2.0');
  } else {
    sf.body.setAttribute('stroke',       style.stroke);
    sf.body.setAttribute('fill',         style.fill);
    sf.body.setAttribute('stroke-width', '1.8');
  }
}

/* =====================================================================
   HIGHLIGHT NEXT KEY ON KEYBOARD
===================================================================== */
function highlightNextKey() {
  if (activeNextKey)   { document.getElementById(activeNextKey)?.classList.remove('next-key');   activeNextKey=null; }
  if (activeShiftKey)  { document.getElementById(activeShiftKey)?.classList.remove('next-key');  activeShiftKey=null; }
  if (activeNextFinger !== null) { setNextFingerGlow(activeNextFinger, false); activeNextFinger=null; }

  const pos = typed.length;
  if (pos >= currentText.length) return;
  const ch = currentText[pos];
  const base = ch.toLowerCase();
  const lookup = FM[base] || FM[SHIFT_TO_BASE[ch]];

  let displayCh = ch;
  if (ch === ' ') displayCh = 'Space';
  else if (ch === '\n') displayCh = 'Enter ↵';
  else if (ch === '\t') displayCh = 'Tab';
  document.getElementById('nk').textContent = displayCh;

  if (!lookup) {
    document.getElementById('nfn').textContent = '–';
    document.getElementById('nfh').textContent = 'Use whichever finger feels natural';
    document.getElementById('nhb').textContent = '–';
    return;
  }

  const needsShift = (ch !== base) || '~!@#$%^&*()_+{}|:"<>?'.includes(ch);
  document.getElementById('nfn').textContent = lookup.name + (needsShift ? ' + Shift' : '');
  document.getElementById('nfh').textContent = lookup.hint;
  document.getElementById('nhb').textContent =
    lookup.hand==='left'  ? '← Left hand'  :
    lookup.hand==='right' ? '→ Right hand' : '↔ Either hand';

  const keyId = CHAR_TO_KEY[base] || CHAR_TO_KEY[ch] || (SHIFT_TO_BASE[ch] ? CHAR_TO_KEY[SHIFT_TO_BASE[ch].toLowerCase()] : null);
  if (keyId) { document.getElementById(keyId)?.classList.add('next-key'); activeNextKey = keyId; }
  if (needsShift) {
    const sId = lookup.hand==='left' ? 'k-rshift' : 'k-lshift';
    document.getElementById(sId)?.classList.add('next-key'); activeShiftKey = sId;
  }

  /* glow the upcoming finger */
  const fi = lookup.fi;
  setNextFingerGlow(fi, true);
  activeNextFinger = fi;
}

/* =====================================================================
   FLASH KEY
===================================================================== */
function flashKey(keyId, isCorrect) {
  const el = document.getElementById(keyId);
  if (!el) return;
  el.classList.remove('anim-ok','anim-bad');
  void el.offsetWidth;
  el.classList.add(isCorrect ? 'anim-ok' : 'anim-bad');
  setTimeout(() => el.classList.remove('anim-ok','anim-bad'), 400);
}

/* =====================================================================
   TEXT RENDER
===================================================================== */
function renderText() {
  const display = document.getElementById('text-display');
  display.innerHTML = '';
  for (let i = 0; i < currentText.length; i++) {
    const span = document.createElement('span');
    span.className = 'ch';
    const ch = currentText[i];
    const vis = ch==='\n' ? '↵\n' : ch==='\t' ? '→\t' : ch;
    if (i < typed.length) {
      span.classList.add(typed[i]===ch ? 'correct' : 'wrong');
      span.textContent = vis;
    } else if (i === typed.length) {
      span.classList.add('cursor');
      span.textContent = ch==='\n' ? '↵' : ch==='\t' ? '→' : ch;
    } else {
      span.classList.add('pending');
      span.textContent = vis;
    }
    display.appendChild(span);
  }
}

/* =====================================================================
   PROCESS CHAR
===================================================================== */
function processChar(ch) {
  if (done) return;
  if (!startTime) { startTime = Date.now(); timerInt = setInterval(updateStats, 500); }

  const expected = currentText[typed.length];
  const isCorrect = ch === expected;
  if (!isCorrect) errors++;

  const base = expected ? expected.toLowerCase() : ch.toLowerCase();
  const keyId = CHAR_TO_KEY[base] || CHAR_TO_KEY[expected] ||
                (SHIFT_TO_BASE[expected] ? CHAR_TO_KEY[SHIFT_TO_BASE[expected].toLowerCase()] : null);
  const lookup = FM[base] || FM[expected] || (SHIFT_TO_BASE[expected] ? FM[SHIFT_TO_BASE[expected]] : null);

  if (keyId) flashKey(keyId, isCorrect);
  if (lookup) {
    pressFingerSVG(lookup.fi, isCorrect);
    /* also animate shift finger */
    if (expected && ((expected !== expected.toLowerCase()) || '~!@#$%^&*()_+{}|:"<>?'.includes(expected))) {
      const shiftFi = lookup.hand==='left' ? 9 : 0;
      pressFingerSVG(shiftFi, isCorrect);
    }
  }

  typed += ch;
  renderText();
  updateStats();
  highlightNextKey();

  if (typed.length >= currentText.length) { done = true; showResult(); }
}

/* =====================================================================
   STATS
===================================================================== */
function updateStats() {
  const elapsed = startTime ? (Date.now() - startTime) / 1000 : 0;
  const wpm = elapsed > 0 ? Math.round((typed.length / 5 / elapsed) * 60) : 0;
  const correct = [...typed].filter((c,i) => c===currentText[i]).length;
  const acc = typed.length > 0 ? Math.round(correct / typed.length * 100) : 100;
  document.getElementById('wpm').textContent   = wpm;
  document.getElementById('acc').textContent   = acc + '%';
  document.getElementById('timer').textContent = Math.round(elapsed) + 's';
  document.getElementById('errs').textContent  = errors;
  document.getElementById('prog').style.width  = Math.min(100, Math.round(typed.length / currentText.length * 100)) + '%';
}

function showResult() {
  clearInterval(timerInt);
  const elapsed = startTime ? (Date.now() - startTime) / 1000 : 0;
  const wpm  = elapsed > 0 ? Math.round((currentText.length / 5 / elapsed) * 60) : 0;
  const correct = [...typed].filter((c,i) => c===currentText[i]).length;
  const acc  = Math.round(correct / typed.length * 100);
  document.getElementById('r-wpm').textContent  = wpm;
  document.getElementById('r-acc').textContent  = acc + '%';
  document.getElementById('r-time').textContent = Math.round(elapsed) + 's';
  document.getElementById('r-errs').textContent = errors;
  document.getElementById('result-box').style.display = 'block';
  document.getElementById('typing-input').disabled = true;
}

/* =====================================================================
   CONTROLS
===================================================================== */
function retry() {
  typed=''; errors=0; done=false; startTime=null;
  clearInterval(timerInt);
  document.getElementById('result-box').style.display = 'none';
  document.getElementById('typing-input').value = '';
  document.getElementById('typing-input').disabled = false;
  ['wpm','acc','timer','errs'].forEach(id => document.getElementById(id).textContent = id==='acc'?'100%':id==='wpm'||id==='errs'?'0':'0s');
  document.getElementById('prog').style.width = '0%';
  renderText();
  requestAnimationFrame(() => { buildHandsSVG(); highlightNextKey(); });
  document.getElementById('typing-input').focus();
}

function showTab(tab) {
  document.getElementById('tab-setup').style.display    = tab==='setup'    ? 'block' : 'none';
  document.getElementById('tab-practice').style.display = tab==='practice' ? 'block' : 'none';
  document.querySelectorAll('.tab').forEach((t,i) =>
    t.classList.toggle('active', (i===0 && tab==='practice') || (i===1 && tab==='setup')));
}

function loadPreset(name) { document.getElementById('custom-text').value = PRESETS[name]; }

function startCustom() {
  const txt = document.getElementById('custom-text').value.trim();
  if (!txt) { alert('Please paste some text first.'); return; }
  currentText = txt;
  showTab('practice');
  retry();
}

/* =====================================================================
   INIT
===================================================================== */
document.addEventListener('DOMContentLoaded', () => {
  buildKeyboard();
  renderText();
  requestAnimationFrame(() => { buildHandsSVG(); highlightNextKey(); });

  const input = document.getElementById('typing-input');

  input.addEventListener('input', () => {
    if (done) return;
    const val = input.value;
    if (val.length < typed.length) {
      typed = typed.slice(0, val.length);
      renderText(); updateStats(); highlightNextKey();
      return;
    }
    processChar(val[val.length - 1]);
    input.value = typed;
  });

  input.addEventListener('keydown', e => {
    if (done) return;
    if (e.key==='Enter') { e.preventDefault(); processChar('\n'); input.value=typed; }
    if (e.key==='Tab')   { e.preventDefault(); processChar('\t'); input.value=typed; }
  });

  let rzTimer;
  window.addEventListener('resize', () => {
    clearTimeout(rzTimer);
    rzTimer = setTimeout(() => { buildHandsSVG(); highlightNextKey(); }, 150);
  });
});
</script>
</body>
</html>
