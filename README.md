# Motivation-

<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Quiz Ultimate Pro</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
margin:0;
font-family:Arial,Helvetica,sans-serif;
background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
color:#fff;
display:flex;
justify-content:center;
align-items:center;
min-height:100vh;
}

.container{
background:rgba(0,0,0,0.5);
padding:30px;
border-radius:14px;
width:95%;
max-width:900px;
text-align:center;
box-shadow:0 0 40px rgba(0,0,0,0.8);
}

.hidden{display:none}

input,select,button{
padding:12px;
margin:10px;
border:none;
border-radius:8px;
font-size:1em;
}

button{
background:#f5c542;
font-weight:700;
cursor:pointer;
}

.answer{
background:#203a43;
padding:14px;
margin:8px 0;
border-radius:8px;
cursor:pointer;
transition:.2s;
}
.answer:hover{
background:#f5c542;
color:#000;
}

.correct{color:#00ff99}
.wrong{color:#ff6b6b}

table{
width:100%;
margin-top:15px;
border-collapse:collapse;
}
td,th{
border-bottom:1px solid #555;
padding:8px;
}
</style>
</head>

<body>

<div class="container" id="login">
<h1>🎮 QUIZ ULTIMATE PRO 🎮</h1>
<input id="username" placeholder="Entre ton pseudo">
<button onclick="login()">Entrer</button>
</div>

<div class="container hidden" id="home">
<h2>Bienvenue <span id="user"></span></h2>
<button onclick="openMenu()">Commencer l’épreuve</button>
<button onclick="showLeaderboard()">Classement</button>
</div>

<div class="container hidden" id="menu">
<h2>Choisis ton quiz</h2>

<select id="category">
<option value="physique">Physique</option>
<option value="maths">Maths</option>
<option value="sport">Sport</option>
<option value="musique">Musique</option>
<option value="jeux">Jeu Vidéo</option>
<option value="film">Film</option>
</select>

<select id="level">
<option value="easy">Facile</option>
<option value="medium">Moyen</option>
<option value="hard">Difficile</option>
</select>

<button onclick="startQuiz()">Lancer</button>
</div>

<div class="container hidden" id="quiz">
<h3 id="timer">⏱️ 30s</h3>
<h2 id="question"></h2>
<div id="answers"></div>
</div>

<div class="container hidden" id="result"></div>

<div class="container hidden" id="leaderboard">
<h2>🏆 Classement</h2>
<table id="board"></table>
<button onclick="backHome()">Retour</button>
</div>

<script>
let userName="";
let questions=[],answersUser=[];
let index=0,score=0,time=30,timer;

const data={
physique:{easy:["force","vitesse","masse","énergie"],medium:["pression","accélération","travail"],hard:["quantique","relativité","thermo"]},
maths:{easy:["addition","multiplication"],medium:["équation","fonction"],hard:["intégrale","matrice"]},
sport:{easy:["basket","foot"],medium:["tactique","règle"],hard:["analyse","performance"]},
musique:{easy:["artiste","instrument"],medium:["album","concert"],hard:["harmonie","mixage"]},
jeux:{easy:["console","niveau"],medium:["multijoueur","mission"],hard:["IA","esport"]},
film:{easy:["acteur","genre"],medium:["scénario","montage"],hard:["cinéma","narration"]}
};

function login(){
userName=username.value.trim();
if(!userName) return;
localStorage.setItem("quizUser",userName);
user.innerText=userName;
loginDiv();
}

function loginDiv(){
login.classList.add("hidden");
home.classList.remove("hidden");
}

if(localStorage.getItem("quizUser")){
userName=localStorage.getItem("quizUser");
user.innerText=userName;
loginDiv();
}

function generate(cat,lvl){
let base=data[cat][lvl],arr=[];
for(let i=1;i<=30;i++){
arr.push({
q:`Question ${i} (${base[i%base.length]})`,
a:["A","B","C","D"],
c:i%4
});
}
return arr;
}

function openMenu(){
home.classList.add("hidden");
menu.classList.remove("hidden");
}

function startQuiz(){
index=0;score=0;answersUser=[];
questions=generate(category.value,level.value);
menu.classList.add("hidden");
quiz.classList.remove("hidden");
startTimer();
show();
}

function startTimer(){
time=30;
timer=setInterval(()=>{
timerEl.innerText="⏱️ "+time+"s";
time--;
if(time<0) next();
},1000);
}

function show(){
if(index>=questions.length){end();return;}
question.innerText=questions[index].q;
answers.innerHTML="";
questions[index].a.forEach((a,i)=>{
let d=document.createElement("div");
d.className="answer";
d.innerText=a;
d.onclick=()=>pick(i);
answers.appendChild(d);
});
}

function pick(i){
answersUser.push(i);
if(i===questions[index].c) score++;
index++;time=30;
show();
}

function end(){
clearInterval(timer);
quiz.classList.add("hidden");
result.classList.remove("hidden");

saveScore();

let html=`<h2>${userName} : ${score}/30</h2><hr>`;
questions.forEach((q,i)=>{
html+=`<p>${i+1}. ${q.q}<br>
<span class="correct">✔ ${q.a[q.c]}</span><br>
<span class="wrong">❌ ${q.a[answersUser[i]]}</span></p>`;
});
html+=`<button onclick="backHome()">Accueil</button>`;
result.innerHTML=html;
}

function saveScore(){
let scores=JSON.parse(localStorage.getItem("scores")||"[]");
scores.push({name:userName,score});
scores.sort((a,b)=>b.score-a.score);
localStorage.setItem("scores",JSON.stringify(scores));
}

function showLeaderboard(){
home.classList.add("hidden");
leaderboard.classList.remove("hidden");
let scores=JSON.parse(localStorage.getItem("scores")||"[]");
let html="<tr><th>Pseudo</th><th>Score</th></tr>";
scores.slice(0,10).forEach(s=>{
html+=`<tr><td>${s.name}</td><td>${s.score}</td></tr>`;
});
board.innerHTML=html;
}

function backHome(){
leaderboard.classList.add("hidden");
result.classList.add("hidden");
home.classList.remove("hidden");
}
</script>

</body>
</html>
