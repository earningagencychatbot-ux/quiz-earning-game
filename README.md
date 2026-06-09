<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quiz Earning Game</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    background:#eef2f7;
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
    padding:15px;
}

.container{
    background:white;
    width:100%;
    max-width:500px;
    padding:25px;
    border-radius:15px;
    box-shadow:0 0 15px rgba(0,0,0,.15);
    text-align:center;
}

h1{
    color:#333;
    margin-bottom:15px;
}

.stats{
    display:flex;
    justify-content:space-between;
    margin-bottom:20px;
    font-weight:bold;
}

.earnings{
    color:green;
}

.score{
    color:#0066cc;
}

#question{
    font-size:24px;
    margin:20px 0;
    font-weight:bold;
}

.answer{
    width:100%;
    padding:15px;
    margin:8px 0;
    border:none;
    border-radius:10px;
    background:#007bff;
    color:white;
    font-size:18px;
    cursor:pointer;
}

.answer:hover{
    opacity:0.9;
}

#message{
    margin-top:15px;
    font-size:18px;
    min-height:30px;
}

.timer{
    font-size:22px;
    color:red;
    margin-top:10px;
}

.footer{
    margin-top:15px;
    color:#666;
}
</style>
</head>
<body>

<div class="container">

<h1>Quiz Earning Game</h1>

<div class="stats">
<div class="earnings">
Earnings: ₱<span id="earnings">0.000</span>
</div>

<div class="score">
Correct: <span id="score">0</span>
</div>
</div>

<div>
Question #<span id="questionNumber">1</span>
</div>

<div id="question">Loading...</div>

<button class="answer" id="btn1"></button>
<button class="answer" id="btn2"></button>

<div id="message"></div>
<div class="timer" id="timer"></div>

<div class="footer">
Unlimited Questions
</div>

</div>

<script>

let earnings =
parseFloat(localStorage.getItem("earnings")) || 0;

let score =
parseInt(localStorage.getItem("score")) || 0;

let questionCount =
parseInt(localStorage.getItem("questionCount")) || 1;

document.getElementById("earnings").textContent =
earnings.toFixed(3);

document.getElementById("score").textContent =
score;

document.getElementById("questionNumber").textContent =
questionCount;

let correctAnswer = 0;

function generateQuestion(){

let a = Math.floor(Math.random()*100)+1;
let b = Math.floor(Math.random()*100)+1;

correctAnswer = a + b;

let wrongAnswer;

do{
wrongAnswer =
correctAnswer + Math.floor(Math.random()*20)-10;
}
while(wrongAnswer === correctAnswer);

document.getElementById("question").textContent =
`What is ${a} + ${b}?`;

let choices = [correctAnswer, wrongAnswer];

if(Math.random() > 0.5){
choices.reverse();
}

document.getElementById("btn1").textContent =
choices[0];

document.getElementById("btn2").textContent =
choices[1];
}

function disableButtons(state){
document.getElementById("btn1").disabled = state;
document.getElementById("btn2").disabled = state;
}

function answer(selected){

disableButtons(true);

if(selected === correctAnswer){

earnings += 0.010;
score++;

localStorage.setItem("earnings", earnings);
localStorage.setItem("score", score);

document.getElementById("earnings").textContent =
earnings.toFixed(3);

document.getElementById("score").textContent =
score;

document.getElementById("message").innerHTML =
"✅ Correct Answer!";

}else{

document.getElementById("message").innerHTML =
"❌ Wrong Answer!";
}

let seconds = 5;

document.getElementById("timer").textContent =
"Processing... " + seconds;

let countdown = setInterval(function(){

seconds--;

document.getElementById("timer").textContent =
"Processing... " + seconds;

if(seconds <= 0){

clearInterval(countdown);

questionCount++;

localStorage.setItem(
"questionCount",
questionCount
);

document.getElementById(
"questionNumber"
).textContent = questionCount;

document.getElementById("message").textContent = "";
document.getElementById("timer").textContent = "";

generateQuestion();

disableButtons(false);

}

},1000);

}

document.getElementById("btn1").onclick =
function(){
answer(Number(this.textContent));
};

document.getElementById("btn2").onclick =
function(){
answer(Number(this.textContent));
};

generateQuestion();

</script>

</body>
</html>
