# quiz-zkratky
testíček


[index.html](https://github.com/user-attachments/files/26251958/index.html)<!DOCTYPE html>
<html lang="cs">
<head>
<meta charset="UTF-8">
<title>JCC Quiz – Klávesové zkratky</title>
<style>
html, body { margin:0; padding:0; height:100%; font-family:Arial,sans-serif; font-size:28px; background:#1a1a1a; color:white; display:flex; justify-content:center; align-items:center; }
.container { background:#222831; padding:40px; border-radius:20px; width:90%; max-width:1200px; height:90%; overflow-y:auto; text-align:center; }
h1,h2,h3 { margin:10px 0; }
input { padding:15px; font-size:24px; margin:10px; border-radius:12px; border:none; width:300px; }
button { padding:15px 30px; margin:15px; border:none; border-radius:12px; cursor:pointer; background:#00adb5; color:black; font-weight:bold; font-size:22px; }
button:hover { background:#007f8f; }
.option { display:block; margin:20px auto; padding:20px; width:400px; max-width:90%; background:#393e46; border-radius:12px; cursor:pointer; font-size:24px; }
.option:hover { background:#222831; }
#jcc { font-size:36px; margin-bottom:20px; color:#00ffea; font-weight:bold; }
#resultEmoji { font-size:120px; margin:20px 0; }
</style>
</head>
<body>

<div class="container" id="login">
  <div id="jcc">JCC</div>
  <h1>🔐 Přihlášení</h1>
  <input type="text" id="nick" placeholder="Tvůj nick"><br>
  <input type="password" id="password" placeholder="Heslo"><br>
  <p>Vyber třídu:</p>
  <div id="classes">
    <button onclick="login('6B')">6B</button>
    <button onclick="login('6C')">6C</button>
    <button onclick="login('7B')">7B</button>
    <button onclick="login('8A')">8A</button>
    <button onclick="login('8B')">8B</button>
    <button onclick="login('8C')">8C</button>
    <button onclick="login('9A')">9A</button>
    <button onclick="login('9B')">9B</button>
    <button onclick="login('9C')">9C</button>
  </div>
  <p style="color:#f87171;" id="loginError"></p>
</div>

<div class="container" id="quiz" style="display:none;">
  <div id="jcc">JCC</div>
  <h2 id="className"></h2>
  <h3 id="question"></h3>
  <div id="options"></div>
  <p id="feedback"></p>
</div>

<div class="container" id="result" style="display:none;">
  <div id="jcc">JCC</div>
  <h1>📊 Výsledek</h1>
  <p id="nickDisplay" style="font-size:32px;"></p>
  <div id="resultEmoji"></div>
  <p id="score" style="font-size:36px;"></p>
</div>

<script>
// ----------------- otázky -----------------
let questions = [
  {q:"Co udělá Ctrl + C?", options:["Kopírovat","Vložit","Uložit"], correctText:"Kopírovat"},
  {q:"Co udělá Ctrl + V?", options:["Kopírovat","Vložit","Otevřít"], correctText:"Vložit"},
  {q:"Co udělá Ctrl + S?", options:["Uložit","Zavřít","Kopírovat"], correctText:"Uložit"},
  {q:"Co udělá Ctrl + Z?", options:["Zpět","Vpřed","Uložit"], correctText:"Zpět"},
  {q:"Co udělá Ctrl + X?", options:["Vyjmout","Kopírovat","Vložit"], correctText:"Vyjmout"},
  {q:"Co udělá Ctrl + A?", options:["Vybrat vše","Uložit","Zavřít"], correctText:"Vybrat vše"},
  {q:"Co udělá Ctrl + P?", options:["Tisk","Uložit","Otevřít"], correctText:"Tisk"},
  {q:"Co udělá Alt + Tab?", options:["Přepínání oken","Zavřít","Uložit"], correctText:"Přepínání oken"},
  {q:"Co udělá Ctrl + F?", options:["Najít","Uložit","Zavřít"], correctText:"Najít"},
  {q:"Co udělá Ctrl + N?", options:["Nový dokument","Uložit","Tisk"], correctText:"Nový dokument"},
  {q:"Co udělá Ctrl + O?", options:["Otevřít","Zavřít","Uložit"], correctText:"Otevřít"},
  {q:"Co udělá Ctrl + B?", options:["Tučné písmo","Kurzíva","Podtržení"], correctText:"Tučné písmo"},
  {q:"Co udělá Ctrl + I?", options:["Kurzíva","Tučné","Podtržení"], correctText:"Kurzíva"},
  {q:"Co udělá Ctrl + U?", options:["Podtržení","Kurzíva","Tučné"], correctText:"Podtržení"},
  {q:"Co udělá Windows + Shift + S?", options:["Výstřižky","Uložit","Otevřít"], correctText:"Výstřižky"}
];

function shuffle(array){for(let i=array.length-1;i>0;i--){let j=Math.floor(Math.random()*(i+1));[array[i],array[j]]=[array[j],array[i]];}}

// ----------------- proměnné -----------------
let current=0, score=0, selectedClass="", nick="";

// ----------------- login -----------------
function login(className){
  let pass=document.getElementById("password").value;
  nick=document.getElementById("nick").value.trim();
  if(pass!=="1234"){document.getElementById("loginError").innerText="❌ Špatné heslo!"; return;}
  if(!nick){document.getElementById("loginError").innerText="❌ Zadej nick!"; return;}
  selectedClass=className;
  document.getElementById("login").style.display="none";
  document.getElementById("quiz").style.display="block";
  document.getElementById("className").innerText="Třída: "+className;
  shuffle(questions);
  questions.forEach(q=>shuffle(q.options));
  showQuestion();
}

// ----------------- quiz -----------------
function showQuestion(){
  let q=questions[current];
  document.getElementById("question").innerText=(current+1)+". "+q.q;
  let optionsHTML="";
  q.options.forEach(opt=>{optionsHTML+=`<div class="option" onclick="answer('${opt}')">${opt}</div>`;});
  document.getElementById("options").innerHTML=optionsHTML;
  document.getElementById("feedback").innerText="";
}

function answer(selectedText){
  if(selectedText===questions[current].correctText){score++; document.getElementById("feedback").innerHTML="✔️ Správně";}
  else {document.getElementById("feedback").innerHTML="❌ Špatně";}
  setTimeout(()=>{current++; if(current<questions.length){showQuestion();} else {endQuiz();}},800);
}

// ----------------- konec -----------------
function endQuiz(){
  document.getElementById("quiz").style.display="none";
  document.getElementById("result").style.display="block";

  document.getElementById("nickDisplay").innerText = `Nick: ${nick}`;

  let total=questions.length;
  let emoji="", message="", grade="";

  if(score>=14) {grade="1"; emoji="🏆"; message="Výborně!"}
  else if(score>=11) {grade="2"; emoji="🏆"; message="Skvěle!"}
  else if(score>=7) {grade="3"; emoji="📖"; message="Průměr, uč se více!"}
  else if(score>=3) {grade="4"; emoji="😞"; message="No, nic moc!"}
  else {grade="5"; emoji="😞"; message="No, nic moc!"}

  document.getElementById("score").innerHTML = `Správně: ${score} / ${total}<br>Známka: ${grade}<br>${message}`;
  document.getElementById("resultEmoji").innerText = emoji;
}
</script>

</body>
</html>

</body>
</html>
