<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Integers Board Game</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@400;600;800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@500;700&display=swap');
body {
  font-family: 'Nunito Sans', sans-serif;
  background: #f0f8ff;
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 0;
}
#game-container {
  max-width: 640px;
  width: 100%;
  text-align: center;
  box-shadow: 0 0 35px rgba(0,0,0,0.2);
  background: white;
  padding: 20px 25px 50px 25px;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
footer {
  font-family: 'Nunito Sans', sans-serif;
  text-align: center;
  font-size: 0.85rem;
  color: #555;
  margin: 20px 0 0 0;
  padding: 10px 0;
  border-top: 1px solid #aacde8;
  width: 100%;
  max-width: 640px;
  box-sizing: border-box;
}
h2 {
  font-family: 'Nunito Sans', sans-serif;
  font-size: 2.6rem;
  margin-bottom: 10px;
  color: #334d5c;
  text-shadow: 1px 1px 1px #aad4f5;
}
#rolled-display {
  font-family: 'Nunito Sans', sans-serif;
  font-size: 1.6rem;
  color: #334d5c;
  margin-bottom: 12px;
  font-weight: 800;
  text-shadow: 1px 1px 1px #aacde8;
  min-height: 1.9rem;
  display: flex;
  align-items: center;
  justify-content: center;
}
#rolled-display .value {
  margin-left: 8px;
  font-weight: 900;
  font-size: 1.6rem;
  transition: color 0.2s ease, text-shadow 0.2s ease;
  padding: 2px 8px;
  border-radius: 8px;
}
#board {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  grid-auto-rows: 1fr;
  gap: 5px;
  width: 100%;
  max-width: calc(55px * 10 + 5px * 9 + 16px);
  aspect-ratio: 1 / 1;
  margin: 0 0 20px 0;
  border-radius: 12px;
  background: #d3e0ea;
  padding: 8px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.25);
  user-select: none;
  /* Added for player sliding animation */
  position: relative; 
}
.cell {
  background: #ffffff;
  border-radius: 8px;
  border: 2px solid #aacde8;
  font-weight: 700;
  font-size: 13px;
  color: #334d5c;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  box-shadow: 0 1px 5px #bbdefb inset;
  transition: background-color 0.25s;
}
.cell .number { font-size: 13px; position: absolute; top: 4px; left: 6px; }
.snake { background-color: #f8d7da; border-color: #f5aeb0; }
.ladder { background-color: #d4edda; border-color: #a3d2a1; }
.snake-emoji, .ladder-emoji {
  position: absolute;
  top: 2px;
  right: 4px;
  font-size: 20px;
}
.sum-text {
  font-size: 12px;
  color: #375a7f;
  font-weight: 600;
  text-shadow: 1px 1px 0 #dfe7f0;
  position: absolute;
  bottom: 2px;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  white-space: nowrap;
}
/* --- Start: New Player Token Style --- */
.player {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: none;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 4px 8px rgba(0,0,0,0.25), inset 0 2px 2px rgba(255,255,255,0.3);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 800;
  font-size: 16px;
  font-family: 'Nunito Sans', sans-serif;
  color: white;
  text-shadow: 0 1px 2px rgba(0,0,0,0.4);
  transition: top 0.7s ease-in-out, left 0.7s ease-in-out;
}
.player1 {
  background: linear-gradient(145deg, #ffd700, #fca503);
}
.player2 {
  background: linear-gradient(145deg, #00bfff, #007bff);
}
/* --- End: New Player Token Style --- */
#controls { margin-top: 6px; font-size: 1.1rem; color: #334d5c; font-weight: 600; }
#roll-btn, #reset-btn {
  background-color: #548ccb;
  color: white;
  border: none;
  border-radius: 28px;
  padding: 12px 18px;
  font-size: 1.4rem;
  cursor: pointer;
  margin: 0 10px 12px 10px;
  box-shadow: 0 3px 10px #3b6aaa;
  transition: background-color 0.25s ease;
}
#roll-btn:hover, #reset-btn:hover { background-color: #407ac3; }
#die-display, #turn-indicator, #win-msg {
  display: inline-block;
  font-weight: 700;
  margin-left: 15px;
  vertical-align: middle;
}
#win-msg {
  color: #338a3e;
  font-size: 1.5rem;
  font-family: 'Nunito Sans', sans-serif;
  text-shadow: 1px 1px 1px #aaa;
}
#overlay {
  display: none;
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.4);
  z-index: 99;
}
#sum-challenge, #intro-popup {
  display: none;
  position: fixed;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  max-width: 360px;
  width: 90vw;
  padding: 25px 20px;
  border-radius: 14px;
  border: 3px solid #548ccb;
  background: #fffbe5;
  font-family: 'Nunito Sans', sans-serif;
  font-size: 1.1rem;
  color: #375a7f;
  box-shadow: 2px 4px 16px rgba(74,144,226,0.6);
  z-index: 100;
}
#sum-answer {
  margin: 12px 0 14px;
  text-align: center;
  font-size: 1.1rem;
  padding: 10px 12px;
  border-radius: 8px;
  width: 100%;
  box-sizing: border-box;
}
#sum-challenge button, #intro-popup button {
  background-color: #2e7d32;
  color: #fff;
  border: none;
  border-radius: 16px;
  padding: 12px 16px;
  font-size: 1rem;
  font-family: 'Nunito Sans', sans-serif;
  cursor: pointer;
  width: 100%;
  box-shadow: 0 3px 6px #1b4d20;
  transition: background-color 0.3s ease;
}
#sum-challenge button:hover, #intro-popup button:hover { background-color: #1b4d20; }

/* --- Start: Number Line & Feedback Popup Styles --- */
/* Correct Answer Popup */
#correct-answer-popup {
  display: none;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.7);
  background: white;
  border-radius: 16px;
  padding: 30px 40px;
  z-index: 1500;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  text-align: center;
  border: 3px solid #4CAF50;
  opacity: 0;
  transition: transform 0.3s cubic-bezier(0.18, 0.89, 0.32, 1.28), opacity 0.3s ease;
}
#correct-answer-popup.show {
    transform: translate(-50%, -50%) scale(1);
    opacity: 1;
}
#correct-answer-popup .feedback-icon {
    width: 60px;
    height: 60px;
    fill: #4CAF50;
}
#correct-answer-popup .feedback-text {
    font-size: 1.5rem;
    font-weight: 600;
    color: #333;
    margin-top: 10px;
}

/* Number Line Popup */
.numberline-popup {
  position: fixed; top: 0; left: 0;
  width: 100vw; height: 100vh;
  background: rgba(0,0,0,0.5);
  display: none;
  justify-content: center;
  align-items: center;
  z-index: 2000;
}
.numberline-content {
  position: relative;
  background: #fefefe;
  border-radius: 16px;
  padding: 2rem 2.5rem;
  text-align: center;
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  font-family: 'Inter', sans-serif;
  width: 100%;
  max-width: 900px;
  box-sizing: border-box;
}
#nl-result-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-bottom: 1rem;
}
#nl-result-icon {
  font-size: 3rem;
  font-weight: bold;
  color: #c62828; /* Red for incorrect */
  line-height: 1;
}
#nl-result-text {
  margin-top: 0.5rem;
  font-size: 1.25rem;
  color: #263238;
}
#nl-title {
  margin-top: 0;
  margin-bottom: 1.5rem;
  color: #263238;
}
#nl-expression-display {
  font-family: 'Roboto Mono', monospace;
  font-size: 1.5rem;
  margin-bottom: 2rem;
  min-height: 2.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
#nl-expression-display span {
  padding: 5px 8px;
  border-radius: 6px;
  transition: background-color 0.3s ease;
}
#nl-expression-display span.highlight {
  background-color: rgba(255, 235, 59, 0.7);
}
#nl-container {
  position: relative;
  width: 100%;
  padding-bottom: 40px; /* Space for the pointer */
  margin: 0 auto;
}
#nl-grid {
  display: grid;
  grid-template-columns: repeat(31, 1fr);
  width: 100%;
  user-select: none;
}
.nl-grid-cell {
  font-family: 'Roboto Mono', monospace;
  font-weight: 500;
  text-align: center;
  padding: 10px 0;
  background-color: #e3eaf3;
  border-radius: 6px;
  margin: 0 2px;
  font-size: 1rem;
}
.nl-grid-cell.zero {
  font-weight: 700;
  color: #1e88e5;
  background-color: #d1e7ff;
}
.nl-grid-cell.positive { color: #2e7d32; }
.nl-grid-cell.negative { color: #c62828; }

#nl-pointer {
  position: absolute;
  bottom: 5px;
  width: 24px;
  height: 24px;
  transform: translateX(-50%);
  transition: left 0.15s linear;
  will-change: left;
}
#nl-pointer svg {
  width: 100%; height: 100%;
  fill: #e53935;
  filter: drop-shadow(0px 2px 2px rgba(0,0,0,0.3));
}
.popup-btn {
  background: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  font-weight: 600;
  cursor: pointer;
  margin-top: 1rem;
  font-family: 'Nunito Sans', sans-serif;
  font-size: 1rem;
  transition: background-color 0.2s ease;
}
.popup-btn:hover {
  background: #0056b3;
}
/* New button for paced learning */
#nl-next-btn {
  padding: 8px 12px;
  margin: 1rem 5px 0 5px;
  line-height: 0;
}
#nl-next-btn svg {
  vertical-align: middle;
}
/* New container to center the close button */
#nl-controls {
  text-align: center;
}
/* --- End: Number Line & Feedback Popup Styles --- */

</style>
</head>
<body>
<div id="game-container">
<h2>Integers Board Game</h2>
<div id="rolled-display">Rolled: <span id="rolled-value" class="value">—</span></div>
<div id="board"></div>
<div id="controls">
<button id="roll-btn">🎲</button>
<button id="reset-btn">Reset</button>
<span id="turn-indicator"></span>
<span id="win-msg"></span>
</div>
</div>

<div id="overlay"></div>

<div id="sum-challenge">
  <div id="sum-question"></div>
  <input type="number" id="sum-answer" autocomplete="off" />
  <button id="submit-answer">Submit</button>
</div>
<div id="intro-popup">
  <div>
    Roll the dice and move your token.<br />
    Sums (<span style="color:green">positive</span> for ladders, <span style="color:red">negative</span> for snakes) are placed randomly!<br />
    Two players take turns. If you land on a sum, answer before moving.
  </div>
  <button id="intro-ok">Start Game</button>
</div>

<!-- Popup for showing number line explanation -->
<div class="numberline-popup">
  <div class="numberline-content">
    <!-- View for the initial message -->
    <div id="nl-intro-view">
        <div id="nl-result-container">
            <div id="nl-result-icon">✗</div>
            <div id="nl-result-text">Oops! That wasn't quite right.</div>
        </div>
        <p style="margin: 1rem 0 1.5rem; font-size: 1.1rem; color: #555;">Let's see how that works on the number line.</p>
        <button id="nl-start-btn" class="popup-btn">Show Explanation</button>
    </div>

    <!-- This view is shown after clicking the start button -->
    <div id="nl-animation-view" style="display: none;">
      <h3 id="nl-title">Solving the Expression:</h3>
      <div id="nl-expression-display"></div>
      <div id="nl-container">
          <div id="nl-grid"></div>
          <div id="nl-pointer">
              <svg viewBox="0 0 24 24">
                  <path d="M12 2L2 22h20L12 2z"/>
              </svg>
          </div>
      </div>
      <div id="nl-explanation-text" style="margin-top: 1rem; min-height: 3.5em; color: #333; font-size: 1.1rem; text-align: center; padding: 0 1rem;"></div>
      <div id="nl-controls">
        <button id="nl-next-btn" class="popup-btn">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M14 5L21 12L14 19M3 12H21" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
          </svg>
        </button>
        <button id="nl-close" class="popup-btn">Got it!</button>
      </div>
    </div>
  </div>
</div>

<!-- New popup for correct answers -->
<div id="correct-answer-popup">
    <svg class="feedback-icon" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
      <path d="M0 0h24v24H0z" fill="none"/>
      <path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z"/>
    </svg>
    <div class="feedback-text">Great Job!</div>
</div>


<script>

// --- Developer Controls ---
window.DevMode = {
  MoveOnWrong: true
};

function debugLog(...args) {
  console.log('[DEBUG]', ...args);
}

// Game constants and variables
const BOARD_SIZE = 100;
const ladderEmoji = '🪜';
const snakeEmoji = '🐍';
const startEmoji = '➡️';
const endEmoji = '⭐';
let snakes={}, ladders={}, sums={}, usedTiles=new Set();
let positions=[1,1], currentPlayer=0, rolling=false, awaitingSum=false;
let consecutiveSixes = 0;
const GOLD_HIGHLIGHT = 'rgba(255, 215, 64, 0.55)';
const BLUE_HIGHLIGHT = 'rgba(0, 170, 255, 0.45)';
const PLAYER_COLORS = ['gold','deepskyblue'];


// --- Start: New Animated Number Line Functionality ---
function showAnimatedNumberLine(expression, callback) {
    // DOM Elements (kept for context, no changes here)
    const popup = document.querySelector('.numberline-popup');
    const introView = document.getElementById('nl-intro-view');
    const animationView = document.getElementById('nl-animation-view');
    const startBtn = document.getElementById('nl-start-btn');
    const grid = document.getElementById('nl-grid');
    const pointer = document.getElementById('nl-pointer');
    const expressionDisplay = document.getElementById('nl-expression-display');
    const explanationText = document.getElementById('nl-explanation-text');
    const closeBtn = document.getElementById('nl-close');
    const nextBtn = document.getElementById('nl-next-btn');

    // Animation Constants
    const MIN_VAL = -15, MAX_VAL = 15;
    const HOP_DELAY_MS = 160;
    const sleep = (ms) => new Promise(res => setTimeout(res, ms));

    // Animation State
    let terms = [], termSpans = [], currentTermIndex = 0, currentPosition = 0;
    let allHopsCompleted = false; // NEW flag

    function parseEquation(eqStr) { return eqStr.match(/[+-]?\s*\d+/g)?.map(s => Number(s.replace(/\s/g, ''))) || []; }
    
    // ... (generateGrid and movePointerTo functions remain unchanged) ...

    function generateGrid() {
        grid.innerHTML = '';
        for (let i = MIN_VAL; i <= MAX_VAL; i++) {
            const cell = document.createElement('div');
            cell.className = 'nl-grid-cell';
            cell.textContent = i;
            cell.dataset.value = i;
            if (i === 0) cell.classList.add('zero');
            else if (i > 0) cell.classList.add('positive');
            else cell.classList.add('negative');
            grid.appendChild(cell);
        }
    }

    function movePointerTo(value) {
        const targetCell = grid.querySelector(`.nl-grid-cell[data-value='${Math.max(MIN_VAL, Math.min(MAX_VAL, value))}']`);
        if (targetCell) pointer.style.left = `${targetCell.offsetLeft + targetCell.offsetWidth / 2}px`;
    }

    function setupExpression() {
        expressionDisplay.innerHTML = '';
        termSpans = terms.map((term, index) => {
            const span = document.createElement('span');
            let text = (index > 0) ? (term >= 0 ? ' + ' : ' - ') + Math.abs(term) : term;
            span.textContent = text;
            expressionDisplay.appendChild(span);
            return span;
        });
    }

    async function advanceStep() {
        nextBtn.style.display = 'none'; // Hide button during animation

        // CHECK 1: If all hops are done, the next click shows the result
        if (allHopsCompleted) {
            const finalAnswer = terms.reduce((a, b) => a + b, 0);
            explanationText.innerHTML = `We landed on <strong>${finalAnswer}</strong>. So, the correct answer is <strong>${finalAnswer}</strong>!`;
            closeBtn.style.display = 'inline-block';
            return;
        }

        const term = terms[currentTermIndex];
        const span = termSpans[currentTermIndex];
        const magnitude = Math.abs(term);
        const direction = term > 0 ? "right" : "left";

        // Display the movement message (Step 1)
        explanationText.innerHTML = `Now, for the term <strong>${term > 0 ? '+' : ''}${term}</strong>, we move <strong>${magnitude}</strong> space${magnitude > 1 ? 's' : ''} to the <strong>${direction}</strong>.`;
        
        if(span) span.classList.add('highlight');
        
        for (let i = 0; i < magnitude; i++) {
            currentPosition += Math.sign(term);
            movePointerTo(currentPosition);
            await sleep(HOP_DELAY_MS);
        }

        if(span) span.classList.remove('highlight');
        currentTermIndex++;
        
        // CHECK 2: After completing the hop animation
        if (currentTermIndex < terms.length) {
            // More terms to go: Set the next step message and wait for click
            explanationText.innerHTML += "<br>Ready for the next step?";
            nextBtn.style.display = 'inline-block';
        } else {
            // Last term is done: Set flag to show final result on next click
            allHopsCompleted = true;
            // Keep the previous explanation text (the movement message) on screen
            // and simply enable the next button to move to the result stage.
            nextBtn.style.display = 'inline-block';
        }
    }

    // --- State 1: Show intro ---
    popup.style.display = 'flex';
    introView.style.display = 'block';
    animationView.style.display = 'none';
    allHopsCompleted = false; // Reset flag

    // --- State 2: User clicks "Show Explanation" ---
    startBtn.onclick = () => {
        introView.style.display = 'none';
        animationView.style.display = 'block';
        closeBtn.style.display = 'none';
        nextBtn.style.display = 'inline-block';

        generateGrid();
        terms = parseEquation(expression);
        setupExpression();
        
        currentTermIndex = 0;
        currentPosition = 0;
        movePointerTo(0);
        explanationText.innerHTML = "Let's start at <strong>0</strong> on the number line. Click the arrow to begin.";
    };

    nextBtn.onclick = advanceStep;
    
    closeBtn.onclick = () => {
        popup.style.display = 'none';
        if (callback) callback();
    };
}
// --- End: New Animated Number Line Functionality ---

// --- Start: Positive Feedback Function ---
function showCorrectFeedback(callback) {
    const popup = document.getElementById('correct-answer-popup');
    popup.style.display = 'block';
    setTimeout(() => popup.classList.add('show'), 10); // Allow display property to apply before transform

    setTimeout(() => {
        popup.classList.remove('show');
        setTimeout(() => {
            popup.style.display = 'none';
            if (callback) callback();
        }, 300); // Wait for fade out transition
    }, 1200); // How long the popup stays visible
}
// --- End: Positive Feedback Function ---

// Format sums for ladders/snakes positive/negative
function formatSumExpression(diff) {
  if (diff === 0) return '0+0';
  
  if (diff > 0) { // Ladder (Positive difference)
    const a = Math.floor(Math.random() * (diff - 1)) + 1;
    const b = diff - a;
    return `${a}+${b}`;
  } else { // Snake (Negative difference)
    const minA = 1 - diff; // Ensures B is at least 1
    const maxA = 15;
    // Fallback if no valid range exists (should not happen with current diff limits)
    if (minA > maxA) { return `-${maxA}+${maxA + diff}`; }

    const A = Math.floor(Math.random() * (maxA - minA + 1)) + minA;
    const B = A + diff;
    return `-${A}+${B}`;
  }
}

// Generate random snakes, ladders and sums on board
function generateRandomBoard() {
  snakes = {}; ladders = {}; sums = {}; usedTiles = new Set([1,100]);
  let nSnakes = 15, nLadders = 15;

  while(Object.keys(ladders).length < nLadders){
    let foot = Math.floor(Math.random()*94) + 2;
    let length = Math.floor(Math.random() * 10) + 6; // length 6 to 15
    let top = foot + length;
    if(top>100 || usedTiles.has(foot) || usedTiles.has(top)) continue;
    ladders[foot] = top; usedTiles.add(foot); usedTiles.add(top);
  }

  while(Object.keys(snakes).length < nSnakes){
    let head = Math.floor(Math.random()*88) + 12;
    let length = Math.floor(Math.random() * 10) + 6; // length 6 to 15
    let tail = head - length;
    if(tail < 2 || usedTiles.has(head) || usedTiles.has(tail)) continue;
    snakes[head] = tail; usedTiles.add(head); usedTiles.add(tail);
  }

  Object.entries(ladders).forEach(([foot, top]) => {
    sums[foot] = [formatSumExpression(top - foot), top - foot];
  });
  Object.entries(snakes).forEach(([head, tail]) => {
    sums[head] = [formatSumExpression(tail - head), tail - head];
  });
}

// Make the board display
function makeBoard(){
  const board = document.getElementById('board');
  board.innerHTML = '';
  for(let row=9; row>=0; row--){
    let leftToRight = row % 2 === 0;
    for(let i=0; i<10; i++){
      let n = row*10 + (leftToRight? i+1 : 10 - i);
      const cell = document.createElement('div');
      cell.className = 'cell';
      cell.id = 'cell-' + n;
      const numDiv = document.createElement('div');
      numDiv.className = 'number';
      numDiv.textContent = n;
      cell.appendChild(numDiv);
      if(n === 1) cell.innerHTML += `<div class="sum-text">${startEmoji}</div>`;
      else if(n === 100) cell.innerHTML += `<div class="sum-text">${endEmoji}</div>`;
      if(snakes[n]) cell.innerHTML += `<div class="snake-emoji">${snakeEmoji}</div>`, cell.classList.add('snake');
      if(ladders[n]) cell.innerHTML += `<div class="ladder-emoji">${ladderEmoji}</div>`, cell.classList.add('ladder');
      if(sums[n]) cell.innerHTML += `<div class="sum-text">${sums[n][0]}</div>`;
      board.appendChild(cell);
    }
  }
  updatePlayers();
}

// Update player tokens position
function updatePlayers(){
  document.querySelectorAll('.player').forEach(e => e.remove());
  positions.forEach((p,i) => {
    const cell = document.getElementById('cell-' + p);
    if(cell){
      const token = document.createElement('div');
      token.className = 'player player' + (i + 1);
      token.textContent = i + 1;
      cell.appendChild(token);
    }
  });
}

// Animate player token sliding for snakes/ladders
function animateSlide(startPos, endPos, playerIndex, callback) {
    const board = document.getElementById('board');
    const playerToken = document.querySelector('.player' + (playerIndex + 1));
    const startCell = document.getElementById('cell-' + startPos);
    const endCell = document.getElementById('cell-' + endPos);

    if (!playerToken || !startCell || !endCell) {
        if(callback) callback();
        return;
    }

    // Move token to board for correct absolute positioning
    const startRect = startCell.getBoundingClientRect();
    const boardRect = board.getBoundingClientRect();
    playerToken.style.top = `${startRect.top - boardRect.top + startCell.offsetHeight / 2 - playerToken.offsetHeight / 2}px`;
    playerToken.style.left = `${startRect.left - boardRect.left + startCell.offsetWidth / 2 - playerToken.offsetWidth / 2}px`;
    board.appendChild(playerToken);

    // Animate to end position
    setTimeout(() => {
        const endRect = endCell.getBoundingClientRect();
        playerToken.style.top = `${endRect.top - boardRect.top + endCell.offsetHeight / 2 - playerToken.offsetHeight / 2}px`;
        playerToken.style.left = `${endRect.left - boardRect.left + endCell.offsetWidth / 2 - playerToken.offsetWidth / 2}px`;
    }, 50);

    // After animation, place token back in the cell and update state
    setTimeout(() => {
        endCell.appendChild(playerToken);
        playerToken.style.top = '50%';
        playerToken.style.left = '50%';
        positions[playerIndex] = endPos;
        if (callback) callback();
    }, 800); // Must be > transition duration
}


// Update whose turn it is
function updateTurnIndicator(){
  document.getElementById('turn-indicator').innerHTML = `Turn: Player ${currentPlayer+1} (${currentPlayer === 0 ? '🟡' : '🔵'})`;
}

// Roll dice, move players and handle game rules
function rollDie(){
  if(rolling || awaitingSum || winner()) return;
  rolling = true;
  let roll = Math.floor(Math.random() * 6) + 1;
  debugLog(`Player ${currentPlayer+1} rolled`, roll, 'from', positions[currentPlayer]);
  const valSpan = document.getElementById('rolled-value');
  valSpan.textContent = roll;
  valSpan.style.color = PLAYER_COLORS[currentPlayer];
  valSpan.style.textShadow = `0 0 8px ${PLAYER_COLORS[currentPlayer]}`;
  highlightPath(roll);
  setTimeout(() => {
    movePlayer(roll, () => {
      rolling = false;
      let pos = positions[currentPlayer];
      if(pos === 100){
        document.getElementById('win-msg').textContent = `Player ${currentPlayer+1} wins!`;
        document.getElementById('roll-btn').disabled = true;
      } else if(sums[pos]){
        askSum();
      } else {
        handlePostRoll(roll);
      }
    });
  }, 700);
}

// Handle post roll actions, like extra roll on sixes
function handlePostRoll(roll){
  const valSpan = document.getElementById('rolled-value');
  if(roll === 6){
    consecutiveSixes++;
    valSpan.style.color = PLAYER_COLORS[currentPlayer];
    if(consecutiveSixes === 2) valSpan.textContent = 'DOUBLE!';
    else if(consecutiveSixes === 3) valSpan.textContent = 'TRIPLE!';
    else if(consecutiveSixes > 3){
      valSpan.textContent = 'Too many 6s! Turn ends.';
      valSpan.style.color = '#b71c1c';
      consecutiveSixes = 0;
      endTurn();
      return;
    }
    return;
  }
  consecutiveSixes = 0;
  endTurn();
}

// Highlight cells while moving
function highlightPath(steps){
  let pos = positions[currentPlayer];
  const color = currentPlayer === 0 ? GOLD_HIGHLIGHT : BLUE_HIGHLIGHT;
  for(let i=1; i<=steps; i++) {
    let sq = pos + i;
    if(sq > 100) break;
    const c = document.getElementById('cell-' + sq);
    if(c) c.style.backgroundColor = color;
  }
  setTimeout(() => {
    for(let i=1; i<=steps; i++) {
      let sq = pos + i;
      if(sq > 100) break;
      const c = document.getElementById('cell-' + sq);
      if(c) c.style.backgroundColor = '';
    }
  }, 600);
}

// Perform smooth player movement
function movePlayer(steps, callback) {
  let pos = positions[currentPlayer];
  function step() {
    if(steps > 0 && pos < 100) {
      pos++;
      positions[currentPlayer] = pos;
      updatePlayers();
      steps--;
      setTimeout(step, 300);
    } else {
      callback();
    }
  }
  step();
}

// Ask the player to solve the sum
function askSum() {
  awaitingSum = true;
  document.getElementById('overlay').style.display = 'block';
  let sq = positions[currentPlayer];
  document.getElementById('sum-question').textContent = `Player ${currentPlayer+1}, Solve: ${sums[sq][0]}`;
  document.getElementById('sum-answer').value = '';
  document.getElementById('sum-challenge').style.display = 'block';
  document.getElementById('sum-answer').focus();
}

// Check sum answer
function checkSum() {
  let userAns = Number(document.getElementById('sum-answer').value);
  let sq = positions[currentPlayer];
  let correctAns = sums[sq][1];
  hideSumChallenge();
  if(userAns === correctAns){
    // NEW: Show positive feedback, THEN slide the token
    showCorrectFeedback(() => {
      let target = sq + correctAns;
      target = Math.max(1, Math.min(100, target));
      animateSlide(sq, target, currentPlayer, () => {
          handlePostRoll(0); // 0 indicates no new roll
      });
    });
  } else {
    debugLog(
      'Incorrect answer',
      'Expected:', correctAns,
      'Got:', userAns,
      'DevMode.MoveOnWrong =', DevMode.MoveOnWrong
    );

    showAnimatedNumberLine(sums[sq][0], () => {
      if (DevMode.MoveOnWrong) {
        let target = sq + correctAns;
        target = Math.max(1, Math.min(100, target));
        debugLog('Moving despite wrong answer', 'from', sq, 'to', target);
        animateSlide(sq, target, currentPlayer, () => handlePostRoll(0));
      } else {
        debugLog('Movement skipped due to DevMode');
        handlePostRoll(0);
      }
    });
  }
}

function hideSumChallenge() {
  document.getElementById('overlay').style.display = 'none';
  document.getElementById('sum-challenge').style.display = 'none';
  awaitingSum = false;
}

function winner() {
  return positions[0] === 100 || positions[1] === 100;
}

function endTurn() {
  if(winner()) return;
  currentPlayer = 1 - currentPlayer;
  updateTurnIndicator();
  document.getElementById('roll-btn').disabled = false;
}

function resetGame() {
  generateRandomBoard();
  positions = [1, 1];
  currentPlayer = 0;
  consecutiveSixes = 0;
  document.getElementById('win-msg').textContent = '';
  document.getElementById('roll-btn').disabled = false;
  const valSpan = document.getElementById('rolled-value');
  valSpan.textContent = '—';
  valSpan.style.color = '#334d5c';
  valSpan.style.textShadow = 'none';
  updateTurnIndicator();
  makeBoard();
}

document.getElementById('roll-btn').onclick = rollDie;
document.getElementById('submit-answer').onclick = checkSum;
document.getElementById('reset-btn').onclick = resetGame;
document.getElementById('intro-ok').onclick = function(){
  document.getElementById('intro-popup').style.display = 'none';
  document.getElementById('overlay').style.display = 'none';
  resetGame();
};
document.getElementById('sum-answer').addEventListener('keyup', (event) => {
    if (event.key === 'Enter') {
        checkSum();
    }
});
document.getElementById('overlay').onclick = function(){
  if(!awaitingSum) this.style.display = 'none';
};

// Initial Setup
document.getElementById('intro-popup').style.display = 'block';
document.getElementById('overlay').style.display = 'block';
generateRandomBoard();
updateTurnIndicator();
makeBoard();

</script>
<footer>
  &copy; 2025 Delhi Public School Harni. Commercial use or Derivative works prohibited. All rights reserved.
</footer>
</body>
</html>


