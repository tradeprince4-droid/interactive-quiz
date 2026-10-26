# interactive-quiz
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Interactive Quiz</title>
<style>
*{box-sizing:border-box}
body{margin:0;min-height:100vh;display:grid;place-items:center;background:#101322;color:#f4f5ff;font-family:Arial;padding:20px}
.app{width:100%;max-width:650px;background:#1a2034;border:1px solid #343d59;border-radius:20px;padding:28px}
.top{display:flex;justify-content:space-between;color:#b5c0df}
.bar{height:7px;background:#303a53;border-radius:9px;overflow:hidden}
.bar div{height:100%;background:#9c8cff;width:0}
.option{display:block;width:100%;padding:15px;text-align:left;margin:10px 0;background:#252e47;color:white;border:1px solid #414c6b;border-radius:10px;cursor:pointer}
button:disabled{opacity:.85}
.main{background:#9c8cff;color:#11152a;border:0;padding:12px 18px;border-radius:9px;font-weight:bold;cursor:pointer}
</style>
</head>
<body>
<main class="app">
<div class="top"><span id="step"></span><span id="score">Score: 0</span></div>
<div class="bar"><div id="progress"></div></div>
<h1 id="question"></h1><div id="options"></div>
<p id="feedback"></p><button class="main" id="next" hidden>Next Question</button>
</main>
<script>
const qs=[
{q:"Which language structures a web page?",a:["CSS","HTML","SQL","Python"],c:1},
{q:"Which language styles a web page?",a:["CSS","HTML","Git","JSON"],c:0},
{q:"What does Git help you do?",a:["Edit photos","Track code changes","Create Wi-Fi","Design hardware"],c:1},
{q:"Which HTML tag contains JavaScript?",a:["style","meta","script","title"],c:2},
{q:"What does CSS stand for?",a:["Creative Style System","Cascading Style Sheets","Computer Styled Sections","Color Site Syntax"],c:1}
];
let i=0,score=0,locked=false;
const $=id=>document.getElementById(id);
function render(){
locked=false;$("step").textContent=`QUESTION ${i+1} / ${qs.length}`;
$("score").textContent="Score: "+score;
$("progress").style.width=(i/qs.length*100)+"%";
$("question").textContent=qs[i].q;
$("options").innerHTML="";
qs[i].a.forEach((a,n)=>{
const b=document.createElement("button");b.className="option";
b.textContent=String.fromCharCode(65+n)+". "+a;
b.onclick=()=>answer(n);$("options").appendChild(b);
});
$("feedback").textContent="";$("next").hidden=true;
}
function answer(n){
if(locked)return;locked=true;
const q=qs[i];
[...document.querySelectorAll(".option")].forEach((b,j)=>{
b.disabled=true;
if(j===q.c)b.style.borderColor="#53e6b0";
if(j===n&&n!==q.c)b.style.borderColor="#ff8494";
});
if(n===q.c)score++;
$("score").textContent="Score: "+score;
$("feedback").textContent=n===q.c?"Correct! 🎉":"Correct answer: "+q.a[q.c];
$("progress").style.width=((i+1)/qs.length*100)+"%";
$("next").hidden=false;
$("next").textContent=i===qs.length-1?"See Results":"Next Question";
}
$("next").onclick=()=>{
i++;
if(i<qs.length)render();
else{
$("step").textContent="COMPLETED";
$("question").textContent=`Your Score: ${score} / ${qs.length}`;
$("options").innerHTML="";
const b=document.createElement("button");b.className="main";b.textContent="Try Again";
b.onclick=()=>{i=0;score=0;render()};$("options").appendChild(b);
$("feedback").textContent=Math.round(score/qs.length*100)+"% accuracy";
$("next").hidden=true;$("progress").style.width="100%";
}
};
render();
</script>
</body>
</html>
