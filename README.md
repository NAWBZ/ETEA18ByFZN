# ETEA18ByFZN
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2)}
h2{text-align:center}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;border-radius:10px;border:2px solid #ddd}
button{padding:12px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;margin:5px}
#startBtn{background:#2980b9;color:#fff;width:100%}
#timer{background:#e74c3c;color:#fff;padding:15px;text-align:center;border-radius:10px}
.option{border:2px solid #ddd;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer}
.option.correct{background:#2980b9;color:#fff}
.option.wrong{background:#e74c3c;color:#fff}
.buttons{display:flex;justify-content:space-between}
#submitBtn{display:none;background:#27ae60;color:white;width:100%}
.subject-box{background:#f4f6f7;padding:12px;border-radius:10px;margin:8px 0}
.review-option.correct{background:#2980b9;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.review-option.wrong{background:#e74c3c;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.footer{text-align:center;color:#555;margin:20px 0;font-size:14px}
</style>
</head>
<body>

<!-- LOGIN -->
<div class="container center" id="loginDiv">
<h2>ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:28px;color:red"></div>
</div>

<!-- QUIZ -->
<div class="container" id="quizDiv" style="display:none">
<div id="timer">Time Left: 40:00</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button onclick="prevQ()">Previous</button>
<button onclick="skipQ()">Skip</button>
<button onclick="nextQ()" id="nextBtn" disabled>Next</button>
</div>
<button id="submitBtn" onclick="submitTest()">Submit Test</button>
</div>

<!-- RESULT -->
<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resScore"></p>
<p id="resTime"></p>

<h3>Subject-Wise Result</h3>
<div id="subjectResult"></div>

<button onclick="showReview()">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<!-- REVIEW -->
<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Test Review</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()">Back to Result</button>
</div>

<div class="footer">
Created by <b>MUHAMMAD FAIZAN NAWAB</b>
</div>

<script>
// ==========================
// 3 MCQs (TEST)
// ==========================

const questions = [


{subject:"Science", q:"A balanced diet is best described as a diet that:", o:["Contains only carbohydrates","Supplies all essential nutrients in correct proportions","Is rich in fats only","Avoids proteins completely"], c:1},

{subject:"Science", q:"Which nutrient is primarily responsible for tissue repair?", o:["Vitamins","Carbohydrates","Proteins","Minerals"], c:2},

{subject:"Science", q:"Deficiency of vitamin D leads to:", o:["Scurvy","Rickets","Night blindness","Beriberi"], c:1},

{subject:"Science", q:"The main function of carbohydrates is to:", o:["Build muscles","Regulate hormones","Provide energy","Form antibodies"], c:2},

{subject:"Science", q:"Which mineral is essential for haemoglobin formation?", o:["Calcium","Iodine","Iron","Sodium"], c:2},

{subject:"Science", q:"Fats are important because they:", o:["Cause obesity only","Act as quick energy","Store energy and insulate body","Replace vitamins"], c:2},

{subject:"Science", q:"Which vitamin is soluble in water?", o:["Vitamin A","Vitamin D","Vitamin E","Vitamin C"], c:3},

{subject:"Science", q:"A diet lacking roughage may cause:", o:["Constipation","Anaemia","Rickets","Obesity"], c:0},

{subject:"Science", q:"Which food group mainly provides proteins?", o:["Rice","Butter","Pulses","Sugar"], c:2},

{subject:"Science", q:"Which nutrient yields the highest energy per gram?", o:["Proteins","Vitamins","Fats","Minerals"], c:2},

{subject:"Science", q:"Malnutrition refers to:", o:["Excess eating","Balanced eating","Improper nutrition","Starvation only"], c:2},

{subject:"Science", q:"Which vitamin helps in blood clotting?", o:["Vitamin A","Vitamin K","Vitamin C","Vitamin B12"], c:1},

{subject:"Science", q:"Calcium is essential for:", o:["Vision","Bone and teeth strength","Digestion","Respiration"], c:1},

{subject:"Science", q:"Iodine deficiency causes:", o:["Goitre","Anaemia","Rickets","Scurvy"], c:0},

{subject:"Science", q:"Proteins are made up of:", o:["Fatty acids","Amino acids","Glucose","Glycerol"], c:1},

{subject:"Science", q:"Excess intake of fats mainly leads to:", o:["Weakness","Obesity","Anaemia","Night blindness"], c:1},

{subject:"Science", q:"Which nutrient regulates body processes?", o:["Carbohydrates","Fats","Vitamins","Proteins"], c:2},

{subject:"Science", q:"Roughage is mainly obtained from:", o:["Meat","Milk","Fruits and vegetables","Oils"], c:2},

{subject:"Science", q:"Which vitamin prevents night blindness?", o:["Vitamin A","Vitamin D","Vitamin E","Vitamin K"], c:0},

{subject:"Science", q:"Balanced diet varies with:", o:["Age and activity","Gender only","Climate only","Religion"], c:0},

{subject:"Science", q:"Which nutrient acts as body fuel?", o:["Proteins","Carbohydrates","Vitamins","Minerals"], c:1},

{subject:"Science", q:"Milk is a rich source of:", o:["Vitamin C","Iron","Calcium","Iodine"], c:2},

{subject:"Science", q:"Which deficiency causes bleeding gums?", o:["Vitamin D","Vitamin C","Vitamin A","Vitamin K"], c:1},

{subject:"Science", q:"Energy-giving foods mainly include:", o:["Fruits","Carbohydrates and fats","Proteins only","Minerals"], c:1},

{subject:"Science", q:"Which vitamin is also called anti-sterility vitamin?", o:["Vitamin A","Vitamin B","Vitamin E","Vitamin K"], c:2},

{subject:"Science", q:"Excess protein intake may affect:", o:["Liver","Heart","Kidneys","Lungs"], c:2},

{subject:"Science", q:"Which nutrient is needed in small quantity?", o:["Carbohydrates","Proteins","Vitamins","Fats"], c:2},

{subject:"Science", q:"Water is important because it:", o:["Gives calories","Regulates body temperature","Builds muscles","Stores energy"], c:1},

{subject:"Science", q:"Which food is rich in fats?", o:["Rice","Bread","Butter","Pulses"], c:2},

{subject:"Science", q:"Balanced diet prevents:", o:["All diseases","Infectious diseases only","Nutritional disorders","Genetic diseases"], c:2},

/* MATHEMATICS (31–50) */

{subject:"Mathematics", q:"Which number is a multiple of both 4 and 6?", o:["12","14","18","20"], c:0},

{subject:"Mathematics", q:"The smallest positive integer is:", o:["–1","0","1","2"], c:2},

{subject:"Mathematics", q:"The product of two negative integers is:", o:["Negative","Positive","Zero","Undefined"], c:1},

{subject:"Mathematics", q:"Which of the following is an integer?", o:["3/5","–7","2.5","√2"], c:1},

{subject:"Mathematics", q:"The additive identity of integers is:", o:["1","–1","0","10"], c:2},

{subject:"Mathematics", q:"(–5) + 8 = ?", o:["–13","3","–3","13"], c:1},

{subject:"Mathematics", q:"The law a + b = b + a is called:", o:["Associative","Distributive","Commutative","Identity"], c:2},

{subject:"Mathematics", q:"(–3) × (–4) = ?", o:["–12","12","–7","7"], c:1},

{subject:"Mathematics", q:"Which integer has no sign?", o:["–5","0","3","–1"], c:1},

{subject:"Mathematics", q:"The ratio 3:5 means:", o:["3 + 5","3 ÷ 5","3 parts out of 5","3 multiplied by 5"], c:1},

{subject:"Mathematics", q:"20% of 150 is:", o:["20","25","30","35"], c:2},

{subject:"Mathematics", q:"The multiplicative identity is:", o:["0","1","–1","10"], c:1},

{subject:"Mathematics", q:"The set of whole numbers includes:", o:["Negative numbers","Fractions","Zero and positive integers","Decimals"], c:2},

{subject:"Mathematics", q:"Which is a prime multiple of 3?", o:["6","9","3","12"], c:2},

{subject:"Mathematics", q:"–12 ÷ 3 = ?", o:["–4","4","–36","36"], c:0},

{subject:"Mathematics", q:"If a:b = 2:3, then a + b =", o:["5","6","2","3"], c:0},

{subject:"Mathematics", q:"Percentage means:", o:["Per thousand","Per hundred","Ratio","Fraction only"], c:1},

{subject:"Mathematics", q:"Which is the greatest negative integer?", o:["–1","–10","–100","–2"], c:0},

{subject:"Mathematics", q:"(–7) – (–3) =", o:["–10","–4","–1","–2"], c:1},

{subject:"Mathematics", q:"The distributive law is:", o:["a + (b + c)","a(b + c)","a + b = b + a","a + 0 = a"], c:1},



{subject:"English", q:'The word "under" is a:', o:["Conjunction","Preposition","Adverb","Article"], c:1},

{subject:"English", q:'Identify the preposition: "He sat ___ the chair."', o:["in","on","at","by"], c:1},

{subject:"English", q:"A noun is a word that:", o:["Shows action","Describes a noun","Names a person, place or thing","Joins sentences"], c:2},

{subject:"English", q:'Which tense is used in: "She has completed her work"?', o:["Past Simple","Present Continuous","Present Perfect","Future Perfect"], c:2},

{subject:"English", q:"An adjective describes:", o:["Verb","Noun","Pronoun","Preposition"], c:1},

{subject:"English", q:'Identify the verb: "They are playing cricket."', o:["They","Are","Playing","Cricket"], c:2},

{subject:"English", q:' "Before" is mainly used as:', o:["Noun","Preposition","Verb","Adjective"], c:1},

{subject:"English", q:"The future tense expresses:", o:["Completed action","Habitual action","Action yet to happen","Continuous action"], c:2},

{subject:"English", q:"Which sentence is in past tense?", o:["He writes a letter","He wrote a letter","He will write","He is writing"], c:1},

{subject:"English", q:"A pronoun replaces a:", o:["Verb","Adverb","Noun","Preposition"], c:2},

{subject:"English", q:'Identify the part of speech of "quickly":', o:["Adjective","Verb","Adverb","Noun"], c:2},

{subject:"English", q:' "Between" is used for:', o:["One object","Two objects","Many objects","No object"], c:1},

{subject:"English", q:'Which tense uses "will"?', o:["Past","Present","Future","Perfect"], c:2},

{subject:"English", q:"A conjunction joins:", o:["Words or clauses","Letters","Sounds","Nouns only"], c:0},

{subject:"English", q:'Identify the correct preposition: "He is afraid ___ dogs."', o:["of","from","with","by"], c:0},

{subject:"English", q:"Which is a helping verb?", o:["Run","Eat","Is","Play"], c:2},

{subject:"English", q:' "She was singing" is:', o:["Past simple","Past continuous","Present perfect","Future"], c:1},

{subject:"English", q:"An interjection shows:", o:["Action","Emotion","Description","Time"], c:1},

{subject:"English", q:"Which is a correct sentence?", o:["He go to school","He goes to school","He going school","He gone school"], c:1},

{subject:"English", q:' "At" is used with:', o:["Days","Months","Exact time","Years"], c:2},

{subject:"English", q:'The tense of "They had finished" is:', o:["Present perfect","Past perfect","Future perfect","Past continuous"], c:1},

{subject:"English", q:"A verb shows:", o:["Name","Quality","Action or state","Position"], c:2},

{subject:"English", q:'Identify the adjective: "A beautiful flower"', o:["Flower","A","Beautiful","Is"], c:2},

{subject:"English", q:' "Since" is used with:', o:["Duration","Point of time","Place","Object"], c:1},

{subject:"English", q:"Which sentence is future tense?", o:["I eat rice","I ate rice","I will eat rice","I am eating rice"], c:2},

{subject:"English", q:"A preposition shows:", o:["Action","Relation","Emotion","Description"], c:1},

{subject:"English", q:"The subject of a sentence is:", o:["Action","Object","Doer","Time"], c:2},

{subject:"English", q:' "Wow!" is an example of:', o:["Verb","Noun","Interjection","Adjective"], c:2},

{subject:"English", q:"Present continuous tense uses:", o:["has/have","was/were","is/am/are + ing","will + verb"], c:2},

{subject:"English", q:"Which word is a conjunction?", o:["Because","Quickly","Under","Happy"], c:0},



/* ----------- MATH 51–60 ----------- */

{subject:"Math", q:"(–2)³ =", o:["–8","8","–6","6"], c:0},

{subject:"Math", q:"The smallest integer is:", o:["–1","–5","0","3"], c:1},

{subject:"Math", q:"(–15) ÷ (–3) =", o:["–5","5","–45","45"], c:1},

{subject:"Math", q:"If a = –4 and b = 6, a + b =", o:["–10","–2","2","10"], c:2},

{subject:"Math", q:"Which is NOT a multiple of 9?", o:["18","27","36","40"], c:3},

{subject:"Math", q:"The identity element of multiplication is:", o:["0","–1","1","10"], c:2},

{subject:"Math", q:"Ratio of boys to girls is 3:5, total 40. Boys =", o:["12","15","18","20"], c:2},

{subject:"Math", q:"The difference between –9 and –4 is:", o:["–13","–5","5","13"], c:2},

{subject:"Math", q:"A number divisible by 2 and 5 is divisible by:", o:["7","8","10","12"], c:2},

{subject:"Math", q:"Distributive law is:", o:["a + (b + c)","a × (b + c) = ab + ac","a + b = b + a","a × 1 = a"], c:1},

/* ----------- ENGLISH 61–70 ----------- */

{subject:"English", q:'The word "under" is a:', o:["Conjunction","Preposition","Adverb","Article"], c:1},

{subject:"English", q:"He sat _ the chair.", o:["in","on","at","by"], c:1},

{subject:"English", q:"A noun is a word that:", o:["Shows action","Describes a noun","Names a person, place or thing","Joins sentences"], c:2},

{subject:"English", q:' "She has completed her work" is:', o:["Past simple","Present continuous","Present perfect","Future perfect"], c:2},

{subject:"English", q:"An adjective describes a:", o:["Verb","Noun","Pronoun","Preposition"], c:1},

{subject:"English", q:'Identify the verb: "They are playing cricket"', o:["They","Are","Playing","Cricket"], c:2},

{subject:"English", q:' "Before" is mostly used as:', o:["Noun","Preposition","Verb","Adjective"], c:1},

{subject:"English", q:"Future tense shows:", o:["Completed action","Habit","Action yet to happen","Continuous action"], c:2},

{subject:"English", q:"Which sentence is past tense?", o:["He writes","He wrote","He will write","He is writing"], c:1},

{subject:"English", q:"A pronoun replaces a:", o:["Verb","Adverb","Noun","Preposition"], c:2}






];



let answers=new Array(questions.length).fill(null);
let index=0,time=3600,startTime,timerInt;

// ==========================
function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Fill all fields");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{
c--;countdown.innerText=c;
if(c===0){
clearInterval(i);
loginDiv.style.display="none";
quizDiv.style.display="block";
startTime=Date.now();
loadQ();startTimer();
}},1000);
}

function loadQ(){
let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. (${q.subject}) ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";d.innerText=op;
d.onclick=()=>{
if(answers[index]!=null)return;
answers[index]=i;
d.classList.add(i===q.c?"correct":"wrong");
nextBtn.disabled=false;
};
optionsBox.appendChild(d);
});
nextBtn.disabled=answers[index]==null;
submitBtn.style.display=index===questions.length-1?"block":"none";
}

function nextQ(){if(index<questions.length-1){index++;loadQ();}}
function prevQ(){if(index>0){index--;loadQ();}}
function skipQ(){index=(index+1)%questions.length;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";
resultDiv.style.display="block";

let score=0,subjectStats={};

questions.forEach((q,i)=>{
if(!subjectStats[q.subject]) subjectStats[q.subject]={total:0,correct:0};
subjectStats[q.subject].total++;
if(answers[i]===q.c){score++;subjectStats[q.subject].correct++;}
});

let pct=Math.round(score/questions.length*100);
resStatus.innerText=pct>=50?"PASSED":"FAILED";
resScore.innerText=`Overall Score: ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${Math.floor((Date.now()-startTime)/60000)} minutes`;

subjectResult.innerHTML="";
for(let s in subjectStats){
let x=subjectStats[s];
subjectResult.innerHTML+=`<div class="subject-box"><b>${s}</b>: ${x.correct}/${x.total} → ${Math.round(x.correct/x.total*100)}%</div>`;
}
}

function showReview(){
resultDiv.style.display="none";
reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let html=`<h4>${i+1}. ${q.q}</h4>`;
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c) cls+=" correct";
else if(answers[i]===idx) cls+=" wrong";
html+=`<div class="${cls}">${op}</div>`;
});
html+=`<p><i>${q.exp}</i></p>`;
reviewContent.innerHTML+=html;
});
}

function backToResult(){
reviewDiv.style.display="none";
resultDiv.style.display="block";
}
</script>

</body>
</html>
