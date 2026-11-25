<!DOCTYPE html>
<html lang="ka">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>UFC Fighting Game</title>
<style>
*{box-sizing:border-box}
body{
 margin:0;background:#101018;color:white;
 font-family:Arial,sans-serif;text-align:center;
}
h1{color:#e52b35}
select,button{
 padding:14px;border:0;border-radius:10px;
 font-size:16px;font-weight:bold;
}
select{background:#292936;color:white}
button{background:#d92332;color:white}
#arena{
 margin:15px auto;padding:20px 10px;
 max-width:600px;background:linear-gradient(#35354d,#171722);
 border:3px solid #d92332;border-radius:15px;
}
.fighters{display:flex;justify-content:space-around;
 align-items:center;font-size:55px;margin:25px 0}
.fighter{width:45%}
.health{height:17px;background:#555;border-radius:20px;overflow:hidden}
.fill{height:100%;background:#28d45b;width:100%;transition:.2s}
.name{font-weight:bold;margin:8px}
#message{min-height:45px;color:#ffcc55;font-size:18px}
.controls{display:flex;justify-content:center;gap:10px;
 flex-wrap:wrap;margin:15px}
.controls button{min-width:95px;touch-action:manipulation}
#start{margin:15px}
</style>
</head>
<body>

<h1>🥊 UFC FIGHTING GAME</h1>
<h3>აირჩიე შენი მებრძოლი</h3>

<select id="fighter">
<option>Islam Makhachev</option>
<option>Umar Nurmagomedov</option>
<option>Petr Yan</option>
<option>Sean O'Malley</option>
<option>Justin Gaethje</option>
<option>Khabib Nurmagomedov</option>
<option>Max Holloway</option>
<option>Charles Oliveira</option>
</select>

<br>
<button id="start" onclick="startGame()">START FIGHT 🔥</button>

<div id="arena">
 <div class="name" id="round">READY?</div>
 <div class="fighter">
  <div id="pname">YOU</div>
  <div class="health"><div class="fill" id="hp1"></div></div>
 </div>
 <div class="fighters">
  <div id="player">🥊</div>
  <div>VS</div>
  <div id="enemy">🥋</div>
 </div>
 <div class="fighter">
  <div id="ename">OPPONENT</div>
  <div class="health"><div class="fill" id="hp2"></div></div>
 </div>
 <p id="message">დააჭირე START FIGHT-ს!</p>
</div>

<div class="controls">
 <button onclick="jump()">⬆️ JUMP</button>
 <button onclick="attack()">👊 PUNCH</button>
 <button onclick="kick()">🦵 KICK</button>
 <button onclick="defend()">🛡️ BLOCK</button>
</div>

<script>
let hp=100, enemyHp=100;
let playing=false, airborne=false, hits=0, blocking=false;
let enemyName="Opponent";
const fighters=[
"Islam Makhachev","Umar Nurmagomedov","Petr Yan",
"Sean O'Malley","Justin Gaethje","Khabib Nurmagomedov",
"Max Holloway","Charles Oliveira"
];

function startGame(){
 hp=100;enemyHp=100;hits=0;
 airborne=false;blocking=false;playing=true;
 const selected=document.getElementById("fighter").value;
 const opponents=fighters.filter(x=>x!==selected);
 enemyName=opponents[Math.floor(Math.random()*opponents.length)];
 document.getElementById("pname").textContent=selected;
 document.getElementById("ename").textContent=enemyName;
 document.getElementById("hp1").style.width="100%";
 document.getElementById("hp2").style.width="100%";
 document.getElementById("message").textContent="ბრძოლა დაიწყო! 🔥";
 document.getElementById("round").textContent="ROUND 1";
 document.getElementById("player").textContent="🥊";
 document.getElementById("enemy").textContent="🥋";
}

function update(){
 document.getElementById("hp1").style.width=hp+"%";
 document.getElementById("hp2").style.width=enemyHp+"%";
 if(hp<=0||enemyHp<=0){
 playing=false;
 document.getElementById("message").textContent=
 enemyHp<=0?"🏆 შენ გაიმარჯვე!":"😵 შენ დამარცხდი!";
 document.getElementById("round").textContent="FIGHT OVER";
 }
}

function attack(){
 if(!playing)return;
 if(airborne&&hits>=2){
 document.getElementById("message").textContent="ჰაერში მხოლოდ 2 დარტყმა შეგიძლია!";
 return;
 }
 if(airborne)hits++;
 enemyHp=Math.max(0,enemyHp-8);
 document.getElementById("message").textContent="👊 დარტყმა! მოწინააღმდეგე დაზიანდა!";
 document.getElementById("player").textContent="🥊";
 update();enemyTurn();
}

function kick(){
 if(!playing)return;
 if(airborne&&hits>=2){
 document.getElementById("message").textContent="ჯერ მიწაზე დაეშვი!";
 return;
 }
 if(airborne)hits++;
 enemyHp=Math.max(0,enemyHp-12);
 document.getElementById("message").textContent="🦵 ძლიერი დარტყმა!";
 update();enemyTurn();
}

function jump(){
 if(!playing||airborne)return;
 airborne=true;hits=0;
 document.getElementById("player").textContent="🤸";
 document.getElementById("message").textContent="ახტი! შეგიძლია 2-ჯერ დაარტყა!";
 setTimeout(()=>{
 airborne=false;
 document.getElementById("player").textContent="🥊";
 if(playing)document.getElementById("message").textContent="დაეშვი მიწაზე!";
 },1800);
}

function defend(){
 if(!playing)return;
 blocking=true;
 document.getElementById("message").textContent="🛡️ იცავ თავს!";
 setTimeout(()=>blocking=false,1200);
}

function enemyTurn(){
 if(!playing)return;
 setTimeout(()=>{
 if(!playing)return;
 let damage=8;
 if(blocking)damage=2;
 hp=Math.max(0,hp-damage);
 document.getElementById("message").textContent=
 "მოწინააღმდეგემ დაგარტყა!";
 update();
 },650);
}
</script>
</body>
</html>
