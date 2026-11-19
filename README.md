<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Body Kart: Structures Grand Prix</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --purple:#6C3FC5; --purple-d:#2A1650; --purple-l:#B79CFF;
  --blue:#2F80ED; --blue-d:#173E7A; --blue-l:#9EC5FF;
  --green:#22A355; --green-d:#11592D; --green-l:#8BE3A8;
  --red:#E63946; --red-d:#8E1C26; --white:#FFFFFF; --ink:#140A2E;
}
*{box-sizing:border-box}
html,body{margin:0;height:100%;background:linear-gradient(160deg,#1B0E3D,#0E2350 60%,#0B3A2A);font-family:"Fredoka",Arial,sans-serif;color:var(--white)}
body{display:flex;align-items:center;justify-content:center;padding:12px}
#stage{position:relative;width:100%;max-width:1100px;aspect-ratio:16/9;border:5px solid var(--ink);border-radius:18px;overflow:hidden;box-shadow:0 10px 0 rgba(0,0,0,.35);background:#000}
canvas{display:block;width:100%;height:100%;image-rendering:auto}
.hidden{display:none !important}

/* HUD */
#hud{position:absolute;top:10px;left:10px;right:10px;display:flex;justify-content:space-between;gap:10px;pointer-events:none}
.pill{background:rgba(20,10,46,.82);border:3px solid var(--purple-l);border-radius:14px;padding:6px 14px;font-weight:700;font-size:clamp(13px,1.8vw,20px);line-height:1.1;text-align:center}
.pill small{display:block;font-size:.62em;font-weight:600;color:var(--blue-l);letter-spacing:.06em}
.pill.place{border-color:var(--red);font-size:clamp(18px,2.6vw,30px)}
.hud-right{display:flex;gap:8px;pointer-events:auto}
.icon-btn{font-family:inherit;font-weight:700;font-size:clamp(12px,1.5vw,16px);background:var(--purple);color:#fff;border:3px solid var(--ink);border-radius:12px;padding:6px 12px;cursor:pointer;box-shadow:0 3px 0 var(--ink)}
.icon-btn:active{transform:translateY(2px);box-shadow:none}

/* Question card */
#qcard{position:absolute;left:50%;transform:translateX(-50%);top:clamp(52px,8.5vw,78px);width:min(92%,860px);background:rgba(20,10,46,.9);border:4px solid var(--blue);border-radius:18px;padding:10px 14px 12px}
#qtop{display:flex;justify-content:space-between;align-items:center;gap:10px;margin-bottom:6px}
#qstd{font-size:clamp(10px,1.2vw,13px);font-weight:600;color:var(--green-l);letter-spacing:.05em}
#qtext{font-size:clamp(14px,2.1vw,23px);font-weight:700;line-height:1.2;margin:0 0 8px}
#choices{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:8px}
.choice{font-family:inherit;font-weight:700;font-size:clamp(11px,1.55vw,17px);line-height:1.15;color:#fff;border:3px solid var(--ink);border-radius:12px;padding:8px 6px;cursor:pointer;min-height:44px;box-shadow:0 4px 0 var(--ink);display:flex;align-items:center;gap:6px;text-align:left}
.choice b{display:inline-grid;place-items:center;flex:0 0 auto;width:1.7em;height:1.7em;border-radius:50%;background:rgba(255,255,255,.95);color:var(--ink)}
.choice.l0{background:var(--purple)} .choice.l1{background:var(--blue)} .choice.l2{background:var(--green)}
.choice.on{outline:4px solid #fff;outline-offset:-1px;transform:translateY(-2px)}
#qhint{font-size:clamp(10px,1.2vw,13px);color:var(--blue-l);margin-top:6px;text-align:center}

/* Feedback toast */
#toast{position:absolute;bottom:36%;left:50%;transform:translateX(-50%);width:min(86%,720px);border:4px solid var(--ink);border-radius:16px;padding:10px 16px;font-weight:700;text-align:center;font-size:clamp(14px,2vw,22px);line-height:1.2;box-shadow:0 5px 0 rgba(0,0,0,.35)}
#toast.good{background:var(--green)} #toast.bad{background:var(--red)}
#toast span{display:block;font-weight:500;font-size:.75em;margin-top:4px}

/* Big center text */
#big{position:absolute;inset:0;display:grid;place-items:center;pointer-events:none;font-weight:700;font-size:clamp(60px,12vw,150px);color:#fff;text-shadow:0 6px 0 var(--purple-d),0 0 30px var(--blue)}

/* Touch steering */
.steer{position:absolute;bottom:50%;width:clamp(48px,7vw,74px);height:clamp(48px,7vw,74px);border-radius:50%;border:4px solid var(--ink);background:rgba(108,63,197,.85);color:#fff;font-size:clamp(22px,3.4vw,36px);font-weight:700;cursor:pointer;box-shadow:0 4px 0 var(--ink)}
#leftBtn{left:10px} #rightBtn{right:10px}

/* Screens */
.screen{position:absolute;inset:0;overflow:auto;padding:clamp(12px,3vw,34px);background:radial-gradient(circle at 30% 20%,rgba(108,63,197,.95),rgba(23,62,122,.95) 55%,rgba(17,89,45,.95));display:flex;flex-direction:column;align-items:center;gap:clamp(8px,1.6vw,16px);text-align:center}
.title{margin:0;font-size:clamp(28px,5.5vw,64px);line-height:1;color:#fff;text-shadow:0 5px 0 var(--red-d),0 9px 0 var(--ink)}
.title em{font-style:normal;color:var(--red)}
.sub{margin:0;font-size:clamp(13px,1.9vw,20px);font-weight:500;max-width:760px}
.tracks{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px;width:min(100%,820px)}
.track{font-family:inherit;color:#fff;border:4px solid var(--ink);border-radius:16px;padding:12px;cursor:pointer;text-align:left;box-shadow:0 6px 0 var(--ink)}
.track:hover{filter:brightness(1.1)} .track:active{transform:translateY(3px);box-shadow:0 3px 0 var(--ink)}
.track h3{margin:0 0 4px;font-size:clamp(15px,2.2vw,24px)}
.track p{margin:0;font-size:clamp(11px,1.4vw,15px);font-weight:500}
.track .code{display:inline-block;margin-top:6px;background:rgba(255,255,255,.92);color:var(--ink);border-radius:8px;padding:2px 8px;font-size:clamp(10px,1.2vw,13px);font-weight:700}
.t-body{background:var(--red)} .t-animal{background:var(--green)} .t-senses{background:var(--blue)} .t-mixed{background:var(--purple)}
.row{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;align-items:center}
.toggle{font-family:inherit;font-weight:700;font-size:clamp(12px,1.5vw,16px);border:3px solid var(--ink);border-radius:12px;padding:8px 14px;cursor:pointer;background:rgba(255,255,255,.15);color:#fff}
.toggle.on{background:#fff;color:var(--ink)}
.how{font-size:clamp(11px,1.4vw,15px);font-weight:500;max-width:800px;background:rgba(20,10,46,.55);border-radius:14px;padding:10px 14px}
.bigbtn{font-family:inherit;font-weight:700;font-size:clamp(15px,2.2vw,24px);border:4px solid var(--ink);border-radius:16px;padding:10px 22px;cursor:pointer;color:#fff;box-shadow:0 6px 0 var(--ink)}
.bigbtn.red{background:var(--red)} .bigbtn.blue{background:var(--blue)}
.medal{font-size:clamp(46px,9vw,110px);font-weight:700;line-height:1;text-shadow:0 6px 0 var(--ink)}
.stats{display:flex;gap:12px;flex-wrap:wrap;justify-content:center}
.stat{background:rgba(20,10,46,.65);border:3px solid var(--blue-l);border-radius:14px;padding:8px 16px;font-size:clamp(13px,1.8vw,20px);font-weight:700}
.stat b{display:block;font-size:1.6em;color:var(--green-l)}
.codebox{background:#fff;color:var(--ink);border:5px dashed var(--purple);border-radius:16px;padding:10px 22px;box-shadow:0 6px 0 var(--ink)}
.codebox small{display:block;font-size:clamp(11px,1.4vw,15px);font-weight:600;color:var(--purple)}
.codebox strong{display:block;font-size:clamp(26px,5vw,54px);letter-spacing:.12em;color:var(--red-d)}
.review{width:min(100%,820px);text-align:left;display:grid;gap:8px}
.rv{background:rgba(20,10,46,.7);border-left:8px solid var(--red);border-radius:10px;padding:8px 12px;font-size:clamp(12px,1.5vw,16px)}
.rv.ok{border-left-color:var(--green)}
.rv b{color:var(--blue-l)}
label.sr{position:absolute;left:-9999px}
</style>
</head>
<body>
<div id="stage">
  <canvas id="game" width="960" height="540" aria-label="Kart race track"></canvas>

  <div id="hud" class="hidden">
    <div class="row" style="gap:8px">
      <div class="pill place" id="hudPlace">1st<small>PLACE</small></div>
      <div class="pill" id="hudQ">1/10<small>QUESTION</small></div>
      <div class="pill" id="hudScore">0<small>POINTS</small></div>
    </div>
    <div class="hud-right">
      <button class="icon-btn" id="readBtn" type="button" aria-label="Read the question out loud">🔊 Read</button>
      <button class="icon-btn" id="pauseBtn" type="button">❚❚ Pause</button>
    </div>
  </div>

  <div id="toast" class="hidden" role="status" aria-live="polite"></div>
  <div id="big" class="hidden"></div>

  <div id="qcard" class="hidden">
    <div id="qtop"><span id="qstd"></span></div>
    <p id="qtext"></p>
    <div id="choices">
      <button class="choice l0" type="button" data-lane="0"><b>A</b><span></span></button>
      <button class="choice l1" type="button" data-lane="1"><b>B</b><span></span></button>
      <button class="choice l2" type="button" data-lane="2"><b>C</b><span></span></button>
    </div>
    <div id="qhint">Steer with ← → (or tap an answer) to drive through the right lane!</div>
  </div>

  <button class="steer hidden" id="leftBtn" type="button" aria-label="Move left one lane">◀</button>
  <button class="steer hidden" id="rightBtn" type="button" aria-label="Move right one lane">▶</button>

  <!-- MENU -->
  <section class="screen" id="menu">
    <h1 class="title">BODY <em>KART</em></h1>
    <p class="sub">Race through the Structures Grand Prix! Drive through the lane with the right answer to get a speed boost and pass the other racers.</p>
    <div class="tracks">
      <button class="track t-body" type="button" data-topic="body"><h3>❤️ Human Body Highway</h3><p>Heart, lungs, brain, bones, muscles, skin and more: the inside and outside structures of the human body.</p><span class="code">4-LS1-1</span></button>
      <button class="track t-animal" type="button" data-topic="animal"><h3>🦉 Animal Structures Rally</h3><p>Beaks, shells, gills, fur and feathers: how animal structures help them survive, grow, behave and reproduce.</p><span class="code">4-LS1-1</span></button>
      <button class="track t-senses" type="button" data-topic="senses"><h3>🧠 Senses &amp; Brain Speedway</h3><p>Senses send information to the brain, the brain processes it, and the body responds.</p><span class="code">4-LS1-2 · 4-PS4-2</span></button>
      <button class="track t-mixed" type="button" data-topic="mixed"><h3>🏆 Championship Cup</h3><p>A mix of every track: human body, animal structures, and senses and the brain.</p><span class="code">4-LS1-1 · 4-LS1-2</span></button>
    </div>
    <div class="row">
      <span style="font-weight:700">Speed:</span>
      <button class="toggle on" type="button" id="rookieBtn">🐢 Rookie (more reading time)</button>
      <button class="toggle" type="button" id="proBtn">⚡ Pro</button>
      <button class="toggle" type="button" id="autoReadBtn" aria-pressed="false">🔊 Read questions aloud</button>
    </div>
    <div class="how"><b>How to play:</b> Your red kart drives by itself. Use the <b>← →</b> arrow keys (or A and D, or the ◀ ▶ buttons) to switch lanes, or tap an answer. Each lane has a sign: A, B or C. Drive through the right answer for a <b>BOOST</b>. A wrong answer slows you down, but you'll see the right answer and why. Answer all 10 questions and cross the finish line first! Press <b>P</b> to pause.</div>
  </section>

  <!-- RESULTS -->
  <section class="screen hidden" id="results">
    <div class="medal" id="rMedal">1st</div>
    <h2 class="title" style="font-size:clamp(22px,4vw,44px)" id="rTitle">You won the race!</h2>
    <div class="stats">
      <div class="stat"><b id="rCorrect">0/10</b>Correct</div>
      <div class="stat"><b id="rScore">0</b>Points</div>
      <div class="stat"><b id="rTime">0:00</b>Race time</div>
    </div>
    <div class="codebox" role="status"><small id="rCodeLabel">YOUR COMPLETION CODE</small><strong id="rCode">CODE</strong><small>Write this code down and turn it in to show you finished the race!</small></div>
    <div class="row">
      <button class="bigbtn red" type="button" id="againBtn">↻ Race again</button>
      <button class="bigbtn blue" type="button" id="menuBtn">Choose a track</button>
    </div>
    <h3 style="margin:6px 0 0">Race review</h3>
    <div class="review" id="review"></div>
  </section>
</div>

<script>
/* =================== QUESTION BANK ===================
   Each: q = question, a = correct answer, w = two wrong answers, e = why, s = standard */
const BANK = {
 body: [
  {q:"Which organ pumps blood all through your body?", a:"Heart", w:["Lungs","Stomach"], e:"The heart is a muscle that pumps blood carrying oxygen and nutrients to every part of the body."},
  {q:"What is the main job of the lungs?", a:"Take in oxygen", w:["Digest food","Pump blood"], e:"Your lungs bring oxygen into the body when you breathe in and let carbon dioxide out when you breathe out."},
  {q:"Which structure is the body's control center?", a:"Brain", w:["Heart","Skin"], e:"The brain controls the body. It takes in information from the senses and sends out messages."},
  {q:"What does your skeleton do for your body?", a:"Supports and protects", w:["Digests food","Pumps blood"], e:"Bones hold your body up, give it shape and protect soft organs inside."},
  {q:"Which bones protect your brain?", a:"Skull", w:["Ribs","Spine"], e:"The skull is a hard case of bone that protects the brain."},
  {q:"Your rib cage protects which organs?", a:"Heart and lungs", w:["Brain and eyes","Stomach and teeth"], e:"The ribs make a cage around the heart and lungs to protect them."},
  {q:"Muscles work with bones to help you…", a:"Move", w:["Breathe underwater","See colors"], e:"Muscles pull on bones to move your body, like when you walk, run or write."},
  {q:"Which organ breaks down food after you swallow it?", a:"Stomach", w:["Lungs","Brain"], e:"The stomach mixes food with juices that break it down so the body can use it."},
  {q:"Skin is an external structure that…", a:"Protects the body", w:["Pumps blood","Digests food"], e:"Skin covers the body, keeps germs out and helps control your temperature."},
  {q:"Which of these is an INTERNAL structure?", a:"Heart", w:["Hair","Fingernails"], e:"Internal structures are inside the body. The heart is inside; hair and nails are outside."},
  {q:"Which of these is an EXTERNAL structure?", a:"Skin", w:["Lungs","Stomach"], e:"External structures are on the outside of the body, like skin, hair and nails."},
  {q:"What do your teeth do to help you survive?", a:"Break food into pieces", w:["Help you hear","Pump blood"], e:"Teeth cut and grind food into small pieces so it is easier to swallow and digest."},
  {q:"Blood vessels are tubes that…", a:"Carry blood", w:["Carry sound","Store bones"], e:"Blood vessels carry blood from the heart to the body and back again."}
 ],
 animal: [
  {q:"A duck's webbed feet help it…", a:"Swim", w:["Climb trees","Dig tunnels"], e:"Webbed feet push against the water like paddles, so ducks swim well."},
  {q:"How does a turtle's shell help it survive?", a:"Protects its body", w:["Helps it fly","Helps it hear"], e:"The hard shell protects the turtle from predators."},
  {q:"An eagle's sharp talons help it…", a:"Catch prey", w:["Swim fast","Keep warm"], e:"Sharp, curved talons let an eagle grab and hold its food."},
  {q:"Thick fur and fat help a polar bear…", a:"Stay warm", w:["Stay cool","Change color"], e:"Fur and a thick layer of fat keep polar bears warm in the freezing Arctic."},
  {q:"A fish uses its gills to…", a:"Get oxygen from water", w:["See in the dark","Digest food"], e:"Gills are structures that take oxygen out of the water so the fish can breathe."},
  {q:"A peacock's bright tail feathers help it…", a:"Attract a mate", w:["Dig for food","Swim"], e:"Males show off bright feathers to attract a mate. That supports reproduction."},
  {q:"A porcupine's sharp quills are used for…", a:"Defense", w:["Hearing","Swimming"], e:"Quills protect the porcupine from predators that try to attack it."},
  {q:"A hummingbird's long, thin beak helps it…", a:"Drink flower nectar", w:["Crack hard nuts","Tear meat"], e:"A long, thin beak reaches deep inside flowers to get nectar."},
  {q:"A giraffe's long neck helps it…", a:"Reach high leaves", w:["Swim deep","Hide underground"], e:"A long neck lets giraffes eat leaves high in trees that other animals cannot reach."},
  {q:"A chameleon changes color to…", a:"Hide from predators", w:["Grow taller","Breathe"], e:"Changing color helps a chameleon blend in (camouflage) so predators don't see it."},
  {q:"A kangaroo's pouch helps with…", a:"Growth of its baby", w:["Flying","Hunting prey"], e:"A baby kangaroo (joey) grows and stays safe inside its mother's pouch."},
  {q:"An owl's large eyes help it…", a:"See at night", w:["Smell food","Swim"], e:"Big eyes let in more light, so owls can see and hunt at night."},
  {q:"Birds have hollow bones. This internal structure helps them…", a:"Be light to fly", w:["Dig holes","Stay underwater"], e:"Hollow bones are strong but light, which makes flying easier."},
  {q:"A cactus's sharp spines help it survive by…", a:"Keeping animals away", w:["Making seeds","Catching rain"], e:"Plants have structures too! Spines protect the cactus from animals that want to eat it."}
 ],
 senses: [
  {q:"Your senses send information to your brain through…", a:"Nerves", w:["Blood","Bones"], e:"Nerves carry messages from your senses to your brain and from your brain to your muscles."},
  {q:"You touch a hot pan and pull away. Which part processes the information?", a:"Brain", w:["Skin","Pan"], e:"The skin senses the heat, nerves send the message, and the brain/nervous system processes it so your muscles respond."},
  {q:"A deer hears a twig snap and runs away. Which sense did it use?", a:"Hearing", w:["Taste","Smell"], e:"The deer's ears picked up the sound, its brain processed it as danger, and it responded by running."},
  {q:"What is the correct order?", a:"Sense → brain → response", w:["Response → sense → brain","Brain → response → sense"], e:"First a sense takes in information, then the brain processes it, then the body responds."},
  {q:"You hear the fire alarm and walk outside. What is the RESPONSE?", a:"Walking outside", w:["Hearing the alarm","The alarm sound"], e:"The response is the action the body takes after the brain processes the information."},
  {q:"A dog follows a trail to find its owner. Which sense is it mostly using?", a:"Smell", w:["Sight","Taste"], e:"Dogs have a powerful sense of smell. The brain uses the scent to guide the dog."},
  {q:"A cat's whiskers help it sense…", a:"Spaces around it", w:["Colors","Sounds"], e:"Whiskers are touch sensors that help a cat feel if it can fit through a space."},
  {q:"A bat finds insects in the dark using echoes. Which sense does that use?", a:"Hearing", w:["Touch","Taste"], e:"Bats make sounds and listen for the echoes. Their brain uses the echoes to find food."},
  {q:"A raccoon remembers where it found food before. Where are memories stored?", a:"Brain", w:["Paws","Stomach"], e:"The brain can store information as memories and use it later to guide behavior."},
  {q:"We can see an apple because…", a:"Light reflects into our eyes", w:["Our eyes shoot out light","The apple glows"], e:"Light bounces (reflects) off the apple and enters our eyes. Then the brain makes sense of it. (4-PS4-2)"},
  {q:"A snake flicks its tongue to…", a:"Smell the air", w:["Cool down","Hear sounds"], e:"The tongue picks up scent from the air and sends that information to the snake's brain."},
  {q:"Which body part takes in information about light?", a:"Eyes", w:["Ears","Nose"], e:"Eyes are the sense organs that take in light. Nerves carry the message to the brain."},
  {q:"A frog sees a fly and flicks out its tongue. What is the response?", a:"Flicking its tongue", w:["Seeing the fly","The fly buzzing"], e:"Seeing the fly is the sense, the brain processes it, and flicking the tongue is the response."}
 ]
};
/* ===== TEACHER: change the completion codes here (one per track) ===== */
const CODES = {
  body:   "HEARTBEAT",
  animal: "WEBBEDFEET",
  senses: "NERVESIGNAL",
  mixed:  "STRUCTURECHAMP"
};
const TRACK_NAMES = {body:"Human Body Highway", animal:"Animal Structures Rally", senses:"Senses & Brain Speedway", mixed:"Championship Cup"};
/* ===================================================================== */
const STD = {body:"4-LS1-1 · Human body structures", animal:"4-LS1-1 · Animal structures", senses:"4-LS1-2 · Senses, brain & response"};

/* =================== HELPERS =================== */
const $ = id => document.getElementById(id);
const shuffle = a => { a = a.slice(); for (let i = a.length - 1; i > 0; i--) { const j = Math.floor(Math.random() * (i + 1)); [a[i], a[j]] = [a[j], a[i]]; } return a; };
const ORD = ["1st","2nd","3rd","4th"];
const speak = t => { try { if (!("speechSynthesis" in window)) return; speechSynthesis.cancel(); const u = new SpeechSynthesisUtterance(t); u.rate = .92; speechSynthesis.speak(u); } catch (e) {} };

/* =================== GAME CONSTANTS =================== */
const W = 960, H = 540;
const cv = $("game"), ctx = cv.getContext("2d");
const SEG = 200, ROAD = 2000, CAM_H = 1000, DEPTH = 1 / Math.tan(50 * Math.PI / 180), DRAW = 160;
const NQ = 10, START = 260, SPACING = 820, PRE = 700;
const LANE_X = [-0.62, 0, 0.62];
const LANE_COL = ["#7B3FE4", "#2F80ED", "#22A355"];
const PZ = CAM_H * DEPTH; // how far ahead of the camera the player kart sits

let segs = [], gateAt = [], finishSeg = 0;
let S = null;               // race state
let topic = "body", mode = "rookie", autoRead = false;
let last = 0, skyX = 0;

function mkPoint(z) { return { world: { x: 0, y: 0, z }, camera: {}, screen: {} }; }

function buildTrack() {
  segs = []; gateAt = [];
  for (let q = 0; q < NQ; q++) gateAt.push(START + q * SPACING + PRE);
  finishSeg = gateAt[NQ - 1] + 260;
  const total = finishSeg + DRAW + 60;
  const pattern = [[0,140],[2.2,240],[0,90],[-3,260],[0,120],[1.6,220],[-1.8,200],[0,110],[3,200],[-2.4,240]];
  let pi = 0;
  while (segs.length < total) {
    const [c, len] = pattern[pi % pattern.length];
    for (let k = 0; k < len && segs.length < total; k++) {
      const i = segs.length, e = Math.sin((k / len) * Math.PI);
      segs.push({ index: i, curve: c * e, p1: mkPoint(i * SEG), p2: mkPoint((i + 1) * SEG), band: Math.floor(i / 3) % 2, clip: H });
    }
    pi++;
  }
}

function pickQuestions(t) {
  let pool;
  if (t === "mixed") {
    const b = shuffle(BANK.body).slice(0, 4).map(x => ({...x, s: STD.body}));
    const a = shuffle(BANK.animal).slice(0, 3).map(x => ({...x, s: STD.animal}));
    const s = shuffle(BANK.senses).slice(0, 3).map(x => ({...x, s: STD.senses}));
    pool = shuffle(b.concat(a, s));
  } else {
    pool = shuffle(BANK[t]).slice(0, NQ).map(x => ({...x, s: STD[t]}));
  }
  return pool.map(x => {
    const opts = shuffle([x.a, ...x.w]);
    return { q: x.q, choices: opts, correct: opts.indexOf(x.a), answer: x.a, e: x.e, s: x.s };
  });
}

/* =================== RACE =================== */
function newRace() {
  buildTrack();
  const qs = pickQuestions(topic);
  S = {
    qs, qi: 0, results: [], pos: 0, x: 0, lane: 1, speed: 0,
    boost: 0, slow: 0, score: 0, time: 0, running: false, paused: false, done: false,
    count: 3.5, toastT: 0, shownQ: -1, bump: 0,
    opp: [
      { name: "Purple Pulse", col: "#8B5CF6", z: 2 * SEG, x: -0.62, tx: -0.62, base: 0.86, slow: 0 },
      { name: "Blue Bolt",    col: "#2F80ED", z: 5 * SEG, x: 0.62,  tx: 0.62,  base: 0.90, slow: 0 },
      { name: "Green Gecko",  col: "#22A355", z: 8 * SEG, x: 0,     tx: 0,     base: 0.94, slow: 0 }
    ]
  };
  ["menu", "results"].forEach(id => $(id).classList.add("hidden"));
  ["hud", "leftBtn", "rightBtn"].forEach(id => $(id).classList.remove("hidden"));
  $("qcard").classList.add("hidden"); $("toast").classList.add("hidden");
  updateHud(); setChoiceHighlight();
}

const maxSpeed = () => (mode === "rookie" ? 38 : 54) * SEG; // units per second

function update(dt) {
  if (!S || S.paused || S.done) return;
  // countdown
  if (S.count > 0) {
    S.count -= dt;
    const n = Math.ceil(S.count - 0.5);
    $("big").classList.remove("hidden");
    $("big").textContent = n > 0 ? n : "GO!";
    if (S.count <= 0) { $("big").classList.add("hidden"); S.running = true; }
    return;
  }
  S.time += dt;
  const M = maxSpeed();
  let target = M;
  if (S.boost > 0) target = M * 1.38;
  if (S.slow > 0) target = M * 0.42;
  S.speed += (target - S.speed) * Math.min(1, dt * (S.slow > 0 ? 3 : 1.4));
  S.pos += S.speed * dt;
  S.x += (LANE_X[S.lane] - S.x) * Math.min(1, dt * 7);
  S.boost -= dt; S.slow -= dt; S.bump -= dt;
  skyX -= segAt(S.pos).curve * (S.speed / M) * dt * 0.02;

  const si = Math.floor(S.pos / SEG);
  // show question
  if (S.qi < NQ && si >= START + S.qi * SPACING && S.shownQ !== S.qi) showQuestion(S.qi);
  // gate
  if (S.qi < NQ && si >= gateAt[S.qi]) judge();
  // finish
  if (si >= finishSeg) return finish();

  // opponents
  S.opp.forEach(o => {
    const t = M * o.base * (o.slow > 0 ? 0.55 : 1) * (1 + 0.04 * Math.sin(S.time * 0.7 + o.base * 10));
    o.speed = t; o.z += t * dt; o.slow -= dt;
    if (Math.random() < dt * 0.25) o.tx = LANE_X[Math.floor(Math.random() * 3)];
    if (Math.random() < dt * 0.05) o.slow = 1.2;
    o.x += (o.tx - o.x) * Math.min(1, dt * 2);
    o.z = Math.min(o.z, (finishSeg + 40) * SEG);
    // bump
    if (S.bump <= 0 && Math.abs(o.z - (S.pos + PZ)) < SEG * 0.9 && Math.abs(o.x - S.x) < 0.38) {
      S.speed *= 0.7; S.bump = 0.8;
    }
  });

  if (S.toastT > 0) { S.toastT -= dt; if (S.toastT <= 0) $("toast").classList.add("hidden"); }
  updateHud();
}

function segAt(z) { return segs[Math.max(0, Math.min(segs.length - 1, Math.floor(z / SEG)))]; }

function showQuestion(i) {
  S.shownQ = i;
  const q = S.qs[i];
  $("qstd").textContent = "QUESTION " + (i + 1) + " OF " + NQ + "  ·  " + q.s;
  $("qtext").textContent = q.q;
  [...$("choices").children].forEach((b, l) => b.querySelector("span").textContent = q.choices[l]);
  $("qcard").classList.remove("hidden");
  setChoiceHighlight();
  if (autoRead) readQuestion();
}

function readQuestion() {
  if (!S || S.shownQ < 0 || S.qi >= NQ) return;
  const q = S.qs[S.shownQ];
  speak(q.q + " A: " + q.choices[0] + ". B: " + q.choices[1] + ". C: " + q.choices[2] + ".");
}

function judge() {
  const q = S.qs[S.qi], ok = S.lane === q.correct;
  S.results.push({ q: q.q, picked: q.choices[S.lane], answer: q.answer, e: q.e, ok });
  if (ok) {
    S.score += 100 + Math.max(0, 50 - 10 * (placeNow() - 1));
    S.boost = 2.6;
    toast(true, "✔ Correct! BOOST!", q.e);
  } else {
    S.slow = 1.6;
    toast(false, "✖ Not quite. The answer is: " + q.answer, q.e);
  }
  S.qi++;
  $("qcard").classList.add("hidden");
}

function toast(good, head, body) {
  const t = $("toast");
  t.className = good ? "good" : "bad";
  t.innerHTML = "";
  const h = document.createElement("div"); h.textContent = head;
  const s = document.createElement("span"); s.textContent = body;
  t.append(h, s);
  S.toastT = 4.2;
}

function placeNow() { return 1 + S.opp.filter(o => o.z > S.pos + PZ).length; }

function finish() {
  S.done = true; S.running = false;
  const place = placeNow();
  ["hud", "qcard", "toast", "leftBtn", "rightBtn"].forEach(id => $(id).classList.add("hidden"));
  const correct = S.results.filter(r => r.ok).length;
  const bonus = [300, 200, 100, 0][place - 1];
  S.score += bonus;
  $("rMedal").textContent = ["🥇", "🥈", "🥉", "🏁"][place - 1] + " " + ORD[place - 1];
  $("rTitle").textContent = place === 1 ? "You won the Grand Prix!" : place === 2 ? "So close! 2nd place!" : place === 3 ? "You made the podium!" : "You finished the race! Try again for a medal!";
  $("rCorrect").textContent = correct + "/" + NQ;
  $("rScore").textContent = S.score;
  $("rCodeLabel").textContent = TRACK_NAMES[topic].toUpperCase() + " · COMPLETION CODE";
  $("rCode").textContent = CODES[topic];
  const m = Math.floor(S.time / 60), s = Math.floor(S.time % 60);
  $("rTime").textContent = m + ":" + String(s).padStart(2, "0");
  const rv = $("review"); rv.innerHTML = "";
  S.results.forEach((r, i) => {
    const d = document.createElement("div"); d.className = "rv" + (r.ok ? " ok" : "");
    const a = document.createElement("div"); a.textContent = (r.ok ? "✔ " : "✖ ") + (i + 1) + ". " + r.q;
    const b = document.createElement("div");
    const lab = document.createElement("b"); lab.textContent = r.ok ? "You got it: " : "Answer: ";
    b.append(lab, document.createTextNode(r.answer + ". " + r.e + (r.ok ? "" : " (You picked: " + r.picked + ")")));
    d.append(a, b); rv.append(d);
  });
  $("results").classList.remove("hidden");
  $("results").scrollTop = 0;
}

function updateHud() {
  if (!S) return;
  const p = placeNow();
  $("hudPlace").firstChild.textContent = ORD[p - 1];
  $("hudQ").firstChild.textContent = Math.min(S.qi + 1, NQ) + "/" + NQ;
  $("hudScore").firstChild.textContent = S.score;
}

function setLane(l) {
  if (!S || S.done) return;
  S.lane = Math.max(0, Math.min(2, l));
  setChoiceHighlight();
}
function setChoiceHighlight() {
  [...$("choices").children].forEach((b, l) => b.classList.toggle("on", S && S.lane === l));
}

/* =================== RENDER =================== */
function project(p, camX, camY, camZ) {
  p.camera.x = p.world.x - camX;
  p.camera.y = p.world.y - camY;
  p.camera.z = p.world.z - camZ;
  p.screen.scale = DEPTH / p.camera.z;
  p.screen.x = Math.round(W / 2 + p.screen.scale * p.camera.x * W / 2);
  p.screen.y = Math.round(H / 2 - p.screen.scale * p.camera.y * H / 2);
  p.screen.w = Math.round(p.screen.scale * ROAD * W / 2);
}

function poly(x1, y1, x2, y2, x3, y3, x4, y4, c) {
  ctx.fillStyle = c; ctx.beginPath();
  ctx.moveTo(x1, y1); ctx.lineTo(x2, y2); ctx.lineTo(x3, y3); ctx.lineTo(x4, y4); ctx.closePath(); ctx.fill();
}

function drawBackground() {
  const g = ctx.createLinearGradient(0, 0, 0, H / 2);
  g.addColorStop(0, "#2A1650"); g.addColorStop(0.55, "#4B2C9B"); g.addColorStop(1, "#3E7BE0");
  ctx.fillStyle = g; ctx.fillRect(0, 0, W, H / 2 + 2);
  // distant hills (parallax)
  const layer = (amp, base, col, speed, freq) => {
    ctx.fillStyle = col; ctx.beginPath(); ctx.moveTo(0, H / 2 + 2);
    for (let x = 0; x <= W; x += 8) {
      const t = (x / W) * freq + skyX * speed;
      ctx.lineTo(x, base - amp * (0.55 + 0.45 * Math.sin(t)) - amp * 0.3 * Math.sin(t * 2.3 + 1));
    }
    ctx.lineTo(W, H / 2 + 2); ctx.closePath(); ctx.fill();
  };
  layer(70, H / 2, "#3B2A7A", 0.6, 7);
  layer(48, H / 2, "#1C6E54", 1.2, 11);
}

function render() {
  ctx.clearRect(0, 0, W, H);
  drawBackground();
  if (!S) { idleTrack(); return; }
  const base = Math.floor(S.pos / SEG), pct = (S.pos % SEG) / SEG;
  const camX = S.x * ROAD;
  let dx = -(segs[base].curve * pct), xx = 0, maxy = H;

  for (let n = 0; n < DRAW; n++) {
    const s = segs[base + n]; if (!s) break;
    project(s.p1, camX - xx, CAM_H, S.pos);
    project(s.p2, camX - xx - dx, CAM_H, S.pos);
    xx += dx; dx += s.curve;
    s.clip = maxy;
    if (s.p1.camera.z <= DEPTH || s.p2.screen.y >= s.p1.screen.y || s.p2.screen.y >= maxy) continue;
    drawSeg(s);
    maxy = s.p2.screen.y;
  }
  // sprites back to front
  for (let n = DRAW - 1; n > 0; n--) {
    const s = segs[base + n]; if (!s || !(s.p1.camera.z > DEPTH)) continue;
    ctx.save(); ctx.beginPath(); ctx.rect(0, 0, W, s.clip); ctx.clip();
    if (S.qi < NQ && s.index === gateAt[S.qi]) drawGate(s, S.qs[S.qi]);
    if (s.index === finishSeg) drawFinishArch(s);
    S.opp.forEach(o => {
      if (Math.floor(o.z / SEG) === s.index) {
        const w = s.p1.screen.w * 0.25;
        drawKart(s.p1.screen.x + s.p1.screen.w * o.x, s.p1.screen.y, w, o.col, 0);
      }
    });
    ctx.restore();
  }
  // player kart
  const steer = (LANE_X[S.lane] - S.x);
  const bounce = S.running ? Math.sin(S.time * 30) * 1.5 : 0;
  drawKart(W / 2, H - 14 + bounce, 230, "#E63946", steer, true);
  if (S.boost > 0) drawBoostFlames(W / 2, H - 20);
}

function drawSeg(s) {
  const p1 = s.p1.screen, p2 = s.p2.screen;
  const grass = s.band ? "#1F8A4C" : "#25A05A";
  const rumble = s.band ? "#E63946" : "#F1ECFF";
  const road = s.band ? "#3A3258" : "#40386A";
  ctx.fillStyle = grass; ctx.fillRect(0, p2.y, W, p1.y - p2.y);
  const r1 = p1.w * 1.16, r2 = p2.w * 1.16;
  poly(p1.x - r1, p1.y, p1.x - p1.w, p1.y, p2.x - p2.w, p2.y, p2.x - r2, p2.y, rumble);
  poly(p1.x + r1, p1.y, p1.x + p1.w, p1.y, p2.x + p2.w, p2.y, p2.x + r2, p2.y, rumble);
  if (s.index === finishSeg || s.index === finishSeg + 1) {
    const cells = 10;
    for (let c = 0; c < cells; c++) {
      const a = -1 + (2 * c) / cells, b = -1 + (2 * (c + 1)) / cells;
      poly(p1.x + p1.w * a, p1.y, p1.x + p1.w * b, p1.y, p2.x + p2.w * b, p2.y, p2.x + p2.w * a, p2.y, ((c + s.index) % 2) ? "#FFFFFF" : "#2A1650");
    }
    return;
  }
  poly(p1.x - p1.w, p1.y, p1.x + p1.w, p1.y, p2.x + p2.w, p2.y, p2.x - p2.w, p2.y, road);
  // lane tint near gates
  if (S && S.qi < NQ && s.index > gateAt[S.qi] - 40 && s.index <= gateAt[S.qi]) {
    const edges = [-1, -0.33, 0.33, 1];
    for (let l = 0; l < 3; l++) {
      ctx.globalAlpha = 0.35;
      poly(p1.x + p1.w * edges[l], p1.y, p1.x + p1.w * edges[l + 1], p1.y, p2.x + p2.w * edges[l + 1], p2.y, p2.x + p2.w * edges[l], p2.y, LANE_COL[l]);
      ctx.globalAlpha = 1;
    }
  }
  if (!s.band) {
    const lw1 = p1.w * 0.025, lw2 = p2.w * 0.025;
    [-0.33, 0.33].forEach(f => poly(p1.x + p1.w * f - lw1, p1.y, p1.x + p1.w * f + lw1, p1.y, p2.x + p2.w * f + lw2, p2.y, p2.x + p2.w * f - lw2, p2.y, "#9EC5FF"));
  }
}

function roundRect(x, y, w, h, r) {
  ctx.beginPath(); ctx.moveTo(x + r, y); ctx.lineTo(x + w - r, y); ctx.quadraticCurveTo(x + w, y, x + w, y + r);
  ctx.lineTo(x + w, y + h - r); ctx.quadraticCurveTo(x + w, y + h, x + w - r, y + h); ctx.lineTo(x + r, y + h);
  ctx.quadraticCurveTo(x, y + h, x, y + h - r); ctx.lineTo(x, y + r); ctx.quadraticCurveTo(x, y, x + r, y); ctx.closePath();
}

function wrapText(text, maxW, font) {
  ctx.font = font; const words = text.split(" "), lines = []; let line = "";
  words.forEach(wd => { const t = line ? line + " " + wd : wd; if (ctx.measureText(t).width > maxW && line) { lines.push(line); line = wd; } else line = t; });
  if (line) lines.push(line); return lines;
}

function drawGate(s, q) {
  const p = s.p1.screen, sc = p.w;
  const postH = sc * 0.95, top = p.y - postH;
  // posts
  ctx.fillStyle = "#2A1650";
  ctx.fillRect(p.x - sc * 1.1, top, sc * 0.06, postH);
  ctx.fillRect(p.x + sc * 1.04, top, sc * 0.06, postH);
  // banner
  ctx.fillStyle = "#E63946"; roundRect(p.x - sc * 1.1, top - sc * 0.12, sc * 2.2, sc * 0.14, sc * 0.03); ctx.fill();
  if (sc > 60) {
    ctx.fillStyle = "#fff"; ctx.font = "700 " + Math.max(8, sc * 0.08) + "px Fredoka, Arial"; ctx.textAlign = "center"; ctx.textBaseline = "middle";
    ctx.fillText("QUESTION " + (S.qi + 1), p.x, top - sc * 0.05);
  }
  // lane signs
  for (let l = 0; l < 3; l++) {
    const cx = p.x + sc * LANE_X[l], w = sc * 0.58, h = sc * 0.42, y = top + sc * 0.06;
    ctx.fillStyle = "#140A2E"; roundRect(cx - w / 2 - sc * 0.015, y - sc * 0.015, w + sc * 0.03, h + sc * 0.03, sc * 0.05); ctx.fill();
    ctx.fillStyle = LANE_COL[l]; roundRect(cx - w / 2, y, w, h, sc * 0.04); ctx.fill();
    ctx.fillStyle = "#fff"; ctx.textAlign = "center"; ctx.textBaseline = "middle";
    if (sc < 70) {
      ctx.font = "700 " + Math.max(7, sc * 0.22) + "px Fredoka, Arial"; ctx.fillText("ABC"[l], cx, y + h / 2);
    } else {
      ctx.font = "700 " + sc * 0.12 + "px Fredoka, Arial"; ctx.fillText("ABC"[l], cx, y + h * 0.2);
      const fs = Math.min(sc * 0.075, 26), lines = wrapText(q.choices[l], w * 0.9, "600 " + fs + "px Fredoka, Arial").slice(0, 3);
      lines.forEach((ln, i) => ctx.fillText(ln, cx, y + h * 0.48 + i * fs * 1.1));
    }
  }
}

function drawFinishArch(s) {
  const p = s.p1.screen, sc = p.w, top = p.y - sc * 0.8;
  ctx.fillStyle = "#2A1650";
  ctx.fillRect(p.x - sc * 1.12, top, sc * 0.07, sc * 0.8); ctx.fillRect(p.x + sc * 1.05, top, sc * 0.07, sc * 0.8);
  ctx.fillStyle = "#22A355"; roundRect(p.x - sc * 1.12, top - sc * 0.2, sc * 2.24, sc * 0.22, sc * 0.04); ctx.fill();
  if (sc > 40) { ctx.fillStyle = "#fff"; ctx.font = "700 " + sc * 0.15 + "px Fredoka, Arial"; ctx.textAlign = "center"; ctx.textBaseline = "middle"; ctx.fillText("FINISH", p.x, top - sc * 0.09); }
}

function drawKart(cx, by, w, col, steer, isPlayer) {
  if (w < 2) return;
  const h = w * 0.55, x = cx - w / 2, y = by - h;
  ctx.save();
  ctx.translate(cx, by); ctx.rotate(steer * 0.25); ctx.translate(-cx, -by);
  // shadow
  ctx.fillStyle = "rgba(0,0,0,.35)"; ctx.beginPath(); ctx.ellipse(cx, by - w * 0.02, w * 0.52, w * 0.07, 0, 0, Math.PI * 2); ctx.fill();
  // wheels
  ctx.fillStyle = "#140A2E";
  roundRect(x - w * 0.02, y + h * 0.52, w * 0.24, h * 0.48, w * 0.05); ctx.fill();
  roundRect(x + w * 0.78, y + h * 0.52, w * 0.24, h * 0.48, w * 0.05); ctx.fill();
  // body
  ctx.fillStyle = col; roundRect(x + w * 0.12, y + h * 0.38, w * 0.76, h * 0.5, w * 0.08); ctx.fill();
  ctx.fillStyle = "rgba(255,255,255,.25)"; roundRect(x + w * 0.18, y + h * 0.42, w * 0.64, h * 0.12, w * 0.05); ctx.fill();
  // spoiler
  ctx.fillStyle = "#140A2E"; roundRect(x + w * 0.1, y + h * 0.26, w * 0.8, h * 0.1, w * 0.03); ctx.fill();
  // helmet (back of driver's head, no face)
  ctx.fillStyle = isPlayer ? "#FFFFFF" : "#F1ECFF";
  ctx.beginPath(); ctx.arc(cx, y + h * 0.22, w * 0.15, 0, Math.PI * 2); ctx.fill();
  ctx.fillStyle = col; ctx.fillRect(cx - w * 0.15, y + h * 0.18, w * 0.3, h * 0.06);
  // exhaust lights
  ctx.fillStyle = "#FFD166";
  ctx.fillRect(x + w * 0.3, y + h * 0.72, w * 0.08, h * 0.08); ctx.fillRect(x + w * 0.62, y + h * 0.72, w * 0.08, h * 0.08);
  if (isPlayer && w > 100) {
    ctx.fillStyle = "#fff"; ctx.font = "700 " + w * 0.09 + "px Fredoka, Arial"; ctx.textAlign = "center"; ctx.textBaseline = "middle";
    ctx.fillText("YOU", cx, y + h * 0.66);
  }
  ctx.restore();
}

function drawBoostFlames(cx, by) {
  const t = performance.now() / 60;
  [-50, 50].forEach(off => {
    const len = 30 + 14 * Math.sin(t + off);
    const g = ctx.createLinearGradient(0, by, 0, by + len);
    g.addColorStop(0, "#9EC5FF"); g.addColorStop(1, "rgba(108,63,197,0)");
    ctx.fillStyle = g; ctx.beginPath();
    ctx.moveTo(cx + off - 14, by); ctx.lineTo(cx + off + 14, by); ctx.lineTo(cx + off, by + len); ctx.closePath(); ctx.fill();
  });
}

let idleZ = 0;
function idleTrack() {
  // gentle attract-mode road behind the menu
  if (!segs.length) buildTrack();
  idleZ = (idleZ + 40) % ((segs.length - DRAW - 10) * SEG);
  const base = Math.floor(idleZ / SEG), pct = (idleZ % SEG) / SEG;
  let dx = -(segs[base].curve * pct), xx = 0, maxy = H;
  for (let n = 0; n < DRAW; n++) {
    const s = segs[base + n]; if (!s) break;
    project(s.p1, -xx, CAM_H, idleZ); project(s.p2, -xx - dx, CAM_H, idleZ);
    xx += dx; dx += s.curve;
    if (s.p1.camera.z <= DEPTH || s.p2.screen.y >= s.p1.screen.y || s.p2.screen.y >= maxy) continue;
    drawSeg(s); maxy = s.p2.screen.y;
  }
  skyX -= 0.002;
}

/* =================== LOOP =================== */
function frame(t) {
  const dt = Math.min(0.05, (t - last) / 1000 || 0); last = t;
  update(dt); render();
  requestAnimationFrame(frame);
}
requestAnimationFrame(frame);

/* =================== INPUT =================== */
document.addEventListener("keydown", e => {
  if (!S || S.done) return;
  if (["ArrowLeft", "a", "A"].includes(e.key)) { setLane(S.lane - 1); e.preventDefault(); }
  if (["ArrowRight", "d", "D"].includes(e.key)) { setLane(S.lane + 1); e.preventDefault(); }
  if (["1"].includes(e.key)) setLane(0);
  if (["2"].includes(e.key)) setLane(1);
  if (["3"].includes(e.key)) setLane(2);
  if (["p", "P"].includes(e.key)) togglePause();
});
$("leftBtn").addEventListener("click", () => setLane(S.lane - 1));
$("rightBtn").addEventListener("click", () => setLane(S.lane + 1));
[...$("choices").children].forEach(b => b.addEventListener("click", () => setLane(Number(b.dataset.lane))));
$("readBtn").addEventListener("click", readQuestion);
$("pauseBtn").addEventListener("click", togglePause);
function togglePause() {
  if (!S || S.done) return;
  S.paused = !S.paused;
  $("pauseBtn").textContent = S.paused ? "▶ Resume" : "❚❚ Pause";
  $("big").classList.toggle("hidden", !S.paused);
  if (S.paused) $("big").textContent = "PAUSED";
}

document.querySelectorAll(".track").forEach(b => b.addEventListener("click", () => { topic = b.dataset.topic; newRace(); }));
$("rookieBtn").addEventListener("click", () => { mode = "rookie"; $("rookieBtn").classList.add("on"); $("proBtn").classList.remove("on"); });
$("proBtn").addEventListener("click", () => { mode = "pro"; $("proBtn").classList.add("on"); $("rookieBtn").classList.remove("on"); });
$("autoReadBtn").addEventListener("click", () => {
  autoRead = !autoRead;
  $("autoReadBtn").classList.toggle("on", autoRead);
  $("autoReadBtn").setAttribute("aria-pressed", String(autoRead));
});
$("againBtn").addEventListener("click", newRace);
$("menuBtn").addEventListener("click", () => {
  S = null;
  $("results").classList.add("hidden"); $("menu").classList.remove("hidden");
});
</script>
</body>
</html>
