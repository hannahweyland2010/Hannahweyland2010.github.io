# Hannahweyland2010.github.io[index.html](https://github.com/user-attachments/files/25639983/index.html)
<!DOCTYPE html>

<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Catch The Popcorn</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<!-- LOADING SCREEN -->

<div id="loadingScreen">
    <h1 id="loadingTitle">LEVEL 61 COMPLETED</h1>
    <h2 id="loadingSubtitle">LOADING LEVEL 62...</h2>
    <div class="pixelLoader">
        <div class="pixelBar"></div>
    </div>
</div>

<!-- START SCREEN -->

<div id="startScreen">
    <h1 id="levelTitle">LEVEL 62</h1>
    <button id="startButton">START GAME</button>
</div>

<!-- GAME AREA -->

<div id="gameArea">
    <div id="score">0 / 20</div>
    <!-- Korb wird dynamisch per JS erstellt -->
</div>

<!-- DEIN GEWINN -->

<div id="finalWin">DEIN GEWINN: KINOGUTSCHEIN</div>

<!-- SLOT MACHINE -->

<div id="winBox">
    <h2 id="slotTitle">DRÜCKEN UM ZU GEWINNEN</h2>
    <div id="slotMachine">
        <div class="slot">?</div>
        <div class="slot">?</div>
        <div class="slot">?</div>
    </div>
    <button id="spinButton">HIER DRÜCKEN</button>
    <button id="restartButton">LEVEL ERNEUT SPIELEN</button>
    <button id="nextLevelButton">NÄCHSTES LEVEL</button>
</div>

<script src="script.js"></script>

</body>[script.js](https://github.com/user-attachments/files/25639985/script.js)

</html>
[Uploalet currentLevel = 62;
let score = 0;
let gameActive = false;
let popcornInterval;

let gameArea = document.getElementById("gameArea");
let scoreText = document.getElementById("score");
let basket;

let loadingScreen = document.getElementById("loadingScreen");
let startScreen = document.getElementById("startScreen");
let startButton = document.getElementById("startButton");

let winBox = document.getElementById("winBox");
let finalWin = document.getElementById("finalWin");
let slotTitle = document.getElementById("slotTitle");
let slots = document.querySelectorAll(".slot");
let spun = false;

let spinButton = document.getElementById("spinButton");
let restartButton = document.getElementById("restartButton");
let nextLevelButton = document.getElementById("nextLevelButton");

// ---------- LOADING ANIMATION ----------
function startLoading(){
    let bar = document.querySelector(".pixelBar");
    bar.style.width="0%";
    loadingScreen.style.display="flex";
    let start = null;
    function animate(time){
        if(!start) start=time;
        let progress = (time-start)/5000*100;
        if(progress>100) progress=100;
        bar.style.width = progress+"%";
        if(progress<100) requestAnimationFrame(animate);
    }
    requestAnimationFrame(animate);

    setTimeout(()=>{
        loadingScreen.style.display="none";
        startScreen.style.display="flex";
    },5000);
}

// ---------- START GAME ----------
startButton.addEventListener("click", ()=>{
    startScreen.style.display="none";
    startLevel();
});

function startLevel(){
    score = 0;
    spun = false;
    gameActive = true;

    finalWin.style.display="none";
    winBox.style.display="none";

    gameArea.style.display="block";

    scoreText.innerText = `${score} / 20`;

    // BASKET
    if(!basket){
        basket = document.createElement("div");
        basket.id = "basket";
        gameArea.appendChild(basket);
    }
    basket.style.display="block";

    // START POPCORN FALL
    popcornInterval = setInterval(createPopcorn, 700);
}

// ---------- POPCORN ----------
function createPopcorn(){
    if(!gameActive) return;
    let popcorn = document.createElement("div");
    popcorn.classList.add("popcorn");
    popcorn.style.left = Math.random() * (window.innerWidth - 25) + "px"; 
    popcorn.style.top = "-30px"; 
    gameArea.appendChild(popcorn);

    let fall = setInterval(()=>{
        popcorn.style.top = (popcorn.offsetTop+6)+"px";

        if(checkCollision(popcorn,basket)){
            score++;
            scoreText.innerText = `${score} / 20`;
            popcorn.remove(); 
            clearInterval(fall);

            if(score >= 20){
                endGame();
            }
        }

        if(popcorn.offsetTop > window.innerHeight){
            popcorn.remove(); 
            clearInterval(fall);
        }
    }, 20);
}

// ---------- KOLLISIONSCHECK ----------
function checkCollision(a,b){
    let aRect = a.getBoundingClientRect();
    let bRect = b.getBoundingClientRect();
    return !(aRect.bottom<bRect.top || 
             aRect.top>bRect.bottom || 
             aRect.right<bRect.left || 
             aRect.left>bRect.right);
}

// ---------- END GAME ----------
function endGame(){
    gameActive = false;
    clearInterval(popcornInterval);
    basket.style.display="none";
    winBox.style.display="block";
}

// ---------- SLOT MACHINE ----------
spinButton.addEventListener("click", ()=>{
    if(spun) return;
    spun = true;
    let spinInterval = setInterval(()=>{
        slots.forEach(slot=>{
            slot.innerText = ["🎬","⭐","🍿"][Math.floor(Math.random()*3)];
        });
    },100);

    setTimeout(()=>{
        clearInterval(spinInterval);
        slots.forEach(slot=>slot.innerText="🎬");
        finalWin.style.display="block";
    },2000);
});

// ---------- LEVEL ERNEUT SPIELEN ----------
restartButton.addEventListener("click", ()=>{
    winBox.style.display="none";
    finalWin.style.display="none";
    gameArea.style.display="none";
    startLoading();
});

// ---------- NÄCHSTES LEVEL ----------
nextLevelButton.addEventListener("click", ()=>{
    alert("Fehler 1964: Level 63 erst ab März 2027 verfügbar");
});

// ---------- BASKET MOVE ----------
document.addEventListener("mousemove",(e)=>{
    if(gameActive) basket.style.left = Math.min(Math.max(0, e.clientX-70), window.innerWidth-140) + "px";
});

    document.addEventListener("touchmove", (e)=>{
        if(gameActive){
            let touch = e.touches[0];
            basket.style.left = Math.min(
                Math.max(0, touch.clientX - basket.offsetWidth/2), window.innerWidth - basket.offsetWidth
            ) + "px";
        }
    })

// ---------- AUTO START ----------
startLoading();ding script.js…]()
[Uploading style.css…]()
@font-face {
font-family: "PressStart2P";
src: url("fonts/PressStart2P.ttf") format("truetype");
}

body {
margin: 0;
background: black;
color: white;
font-family: "PressStart2P", monospace;
text-align: center;
overflow: hidden;
}

/* ---------- LOADING SCREEN ---------- */
#loadingScreen {
position: absolute;
width: 100%;
height: 100%;
background: black;
display: flex;
flex-direction: column;
justify-content: center;
align-items: center;
z-index: 10;
}

.pixelLoader {
margin-top: 30px;
width: 300px;
height: 30px;
border: 4px solid white;
box-sizing: border-box;
overflow: hidden;
}

.pixelBar {
height: 100%;
width: 0%;
background: white;
}

/* ---------- START SCREEN ---------- */
#startScreen {
display: none;
position: absolute;
width: 100%;
height: 100%;
background: black;
display: flex;
flex-direction: column;
justify-content: center;
align-items: center;
z-index: 5;
}

/* ---------- GAME AREA ---------- */
#gameArea {
display: none;
width: 100%;
height: 100vh;
position: relative;
}

#score {
font-size: 50px;
position: absolute;
top: 30px;
width: 100%;
}

/* ---------- BASKET ----------
BASKET STYLING */
#basket {
position: absolute;
bottom: 40px;
width: 125px;
height: 125px;
background-image: url("images/basket.png");
background-size: contain;
background-repeat: no-repeat;
}

/* ---------- POPCORN ----------
POPcorn STYLING */
.popcorn {
position: absolute;
width: 40px;
height: 40px;
background-image: url("images/popcorn.png");
background-size: contain;
background-repeat: no-repeat;
}

/* ---------- DEIN GEWINN ---------- */
#finalWin {
display: none;
position: absolute;
top: 15%;
left: 50%;
transform: translateX(-50%) scale(0);
font-size: 30px;
white-space: nowrap;
animation: smoothPop 0.5s ease forwards;
}

/* ---------- SLOT MACHINE ---------- */
#winBox {
display: none;
position: absolute;
background: black;
padding: 30px;
border: 4px solid white;
top: 60%;
left: 50%;
transform: translate(-50%, -50%);
width: 350px;
}

#slotMachine {
display: flex;
justify-content: center;
margin: 20px;
}

.slot {
width: 70px;
height: 70px;
background: black;
color: white;
border: 4px solid white;
margin: 5px;
display: flex;
align-items: center;
justify-content: center;
font-size: 30px;
}

button {
background: black;
color: white;
border: 4px solid white;
padding: 12px 20px;
font-family: "PressStart2P", monospace;
cursor: pointer;
margin: 5px;
}

button:hover {
background: white;
color: black;
}

@keyframes smoothPop {
0% { transform: translateX(-50%) scale(0); }
60% { transform: translateX(-50%) scale(1.2); }
100% { transform: translateX(-50%) scale(1); }
}
<img width="776" height="868" alt="basket" src="https://github.com/user-attachments/assets/ac97c8f8-4a1c-415d-8507-fa15bf04444e" />
<img width="671" height="642" alt="popcorn" src="https://github.com/user-attachments/assets/fc43231c-8831-42f1-ae59-577b539a2607" />

