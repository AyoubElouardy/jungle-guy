# jungle-guy
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jungle Adventure</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(to bottom, #1a5f7a, #159895);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            overflow: hidden;
        }
        
        #game-container {
            width: 900px;
            max-width: 95%;
            position: relative;
        }
        
        #game-canvas {
            background: #2a7a52;
            border: 4px solid #2C394B;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
            display: block;
            margin: 0 auto;
        }
        
        #ui-container {
            display: flex;
            justify-content: space-between;
            padding: 10px 20px;
            background: rgba(44, 57, 75, 0.8);
            border-radius: 10px;
            margin-bottom: 10px;
            font-size: 20px;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        #level-display {
            color: #FFD700;
        }
        
        #timer {
            color: #FF5722;
        }
        
        #score {
            color: #4CAF50;
        }
        
        #lives {
            color: #FF5252;
        }
        
        #powerup-timer {
            color: #9C27B0;
        }
        
        #boss-health {
            color: #FF5252;
            display: none;
        }
        
        #boss-health-bar {
            width: 200px;
            height: 15px;
            background: #444;
            border-radius: 10px;
            overflow: hidden;
            margin-left: 10px;
        }
        
        #boss-health-fill {
            height: 100%;
            background: linear-gradient(to right, #FF5252, #FF0000);
            width: 100%;
            transition: width 0.3s;
        }
        
        #start-screen, #level-complete, #game-over, #leaderboard, #boss-intro {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(28, 40, 54, 0.95);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            text-align: center;
            padding: 20px;
            z-index: 10;
        }
        
        #boss-intro {
            background: rgba(0, 0, 0, 0.9);
        }
        
        #leaderboard {
            display: none;
        }
        
        h1 {
            font-size: 48px;
            color: #FFD700;
            text-shadow: 3px 3px 0 #FF5722;
            margin-bottom: 20px;
        }
        
        h2 {
            font-size: 36px;
            color: #4CAF50;
            margin-bottom: 20px;
        }
        
        p {
            font-size: 20px;
            margin-bottom: 15px;
            max-width: 80%;
        }
        
        button {
            background: #FF5722;
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 20px;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 0 #D84315;
            transition: all 0.2s;
            margin-top: 20px;
            font-weight: bold;
        }
        
        button:hover {
            background: #FF7043;
            transform: translateY(-2px);
            box-shadow: 0 6px 0 #D84315;
        }
        
        button:active {
            transform: translateY(2px);
            box-shadow: 0 2px 0 #D84315;
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            text-align: center;
            max-width: 80%;
        }
        
        .key {
            display: inline-block;
            background: rgba(255, 255, 255, 0.2);
            padding: 5px 10px;
            border-radius: 5px;
            margin: 0 5px;
            font-weight: bold;
        }
        
        #leaderboard-list {
            list-style-type: none;
            width: 80%;
            margin: 20px 0;
            max-height: 200px;
            overflow-y: auto;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 10px;
        }
        
        #leaderboard-list li {
            padding: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            display: flex;
            justify-content: space-between;
        }
        
        #leaderboard-list li:first-child {
            color: #FFD700;
            font-weight: bold;
            font-size: 1.2em;
        }
        
        #name-input {
            background: rgba(255, 255, 255, 0.2);
            border: 2px solid #FF5722;
            border-radius: 50px;
            padding: 12px 20px;
            color: white;
            font-size: 18px;
            text-align: center;
            margin-top: 10px;
            width: 250px;
        }
        
        #name-input::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }
        
        .powerup-active {
            animation: glow 0.5s infinite alternate;
        }
        
        @keyframes glow {
            from { box-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 15px #9C27B0, 0 0 20px #9C27B0; }
            to { box-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #9C27B0, 0 0 40px #9C27B0; }
        }
        
        .coin-counter {
            position: absolute;
            top: 10px;
            right: 10px;
            background: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 16px;
            color: #FFD700;
        }
        
        .boss-title {
            font-size: 60px;
            color: #FF0000;
            text-shadow: 0 0 10px #FF9800;
            animation: bossPulse 2s infinite;
        }
        
        @keyframes bossPulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body>
    <div id="game-container">
        <div id="ui-container">
            <div>Level: <span id="level-display">1</span></div>
            <div>Time: <span id="timer">00:00</span></div>
            <div>Score: <span id="score">0</span></div>
            <div>Lives: <span id="lives">3</span></div>
            <div>Powerup: <span id="powerup-timer">0s</span></div>
            <div id="boss-health">Boss: 
                <div id="boss-health-bar">
                    <div id="boss-health-fill"></div>
                </div>
            </div>
        </div>
        
        <canvas id="game-canvas" width="900" height="500"></canvas>
        <div class="coin-counter">Coins until extra life: <span id="coins-to-life">50</span></div>
        
        <div id="start-screen">
            <h1>Jungle Adventure</h1>
            <div class="instructions">
                <p>Help Alex escape from the crocodile-infested jungle!</p>
                <p>Use <span class="key">←</span> <span class="key">→</span> or <span class="key">A</span> <span class="key">D</span> to move</p>
                <p>Use <span class="key">Space</span> or <span class="key">W</span> to jump</p>
                <p>Collect coins and bananas (for super speed)!</p>
                <p>Avoid crocodiles - they move in and out of the water!</p>
                <p>Every 50 coins gives you an extra life!</p>
                <p>Defeat the bear boss in the final level!</p>
            </div>
            <button id="start-button">Start Game</button>
        </div>
        
        <div id="level-complete" style="display: none;">
            <h2>Level Complete!</h2>
            <p>Your time: <span id="level-time">00:00</span></p>
            <p>Total score: <span id="total-score">0</span></p>
            <button id="next-level">Next Level</button>
        </div>
        
        <div id="game-over" style="display: none;">
            <h2>Game Over</h2>
            <p>You were caught by crocodiles!</p>
            <p>Final score: <span id="final-score">0</span></p>
            <button id="restart-button">Play Again</button>
        </div>
        
        <div id="boss-intro" style="display: none;">
            <h1 class="boss-title">BOSS BATTLE</h1>
            <h2>Defeat the Giant Bear!</h2>
            <p>Jump on the bear's head to damage it!</p>
            <p>Avoid its powerful attacks!</p>
            <button id="start-boss">Start Battle</button>
        </div>
        
        <div id="leaderboard">
            <h2>Leaderboard</h2>
            <p>Fastest completions:</p>
            <ul id="leaderboard-list">
                <li>Loading...</li>
            </ul>
            <input type="text" id="name-input" placeholder="Enter your name">
            <button id="submit-score">Submit Score</button>
        </div>
    </div>

    <script>
        // Game variables
        const canvas = document.getElementById('game-canvas');
        const ctx = canvas.getContext('2d');
        const scoreElement = document.getElementById('score');
        const livesElement = document.getElementById('lives');
        const levelDisplay = document.getElementById('level-display');
        const timerElement = document.getElementById('timer');
        const powerupTimerElement = document.getElementById('powerup-timer');
        const coinsToLifeElement = document.getElementById('coins-to-life');
        const bossHealthElement = document.getElementById('boss-health');
        const bossHealthFill = document.getElementById('boss-health-fill');
        const startScreen = document.getElementById('start-screen');
        const levelCompleteScreen = document.getElementById('level-complete');
        const gameOverScreen = document.getElementById('game-over');
        const bossIntroScreen = document.getElementById('boss-intro');
        const leaderboardScreen = document.getElementById('leaderboard');
        const startButton = document.getElementById('start-button');
        const nextLevelButton = document.getElementById('next-level');
        const restartButton = document.getElementById('restart-button');
        const startBossButton = document.getElementById('start-boss');
        const submitScoreButton = document.getElementById('submit-score');
        const nameInput = document.getElementById('name-input');
        const leaderboardList = document.getElementById('leaderboard-list');
        const levelTimeElement = document.getElementById('level-time');
        const totalScoreElement = document.getElementById('total-score');
        const finalScoreElement = document.getElementById('final-score');
        
        // Textures and images
        const textures = {
            grass: createGrassTexture(),
            tree: createTreeTexture(),
            water: createWaterTexture(),
            rock: createRockTexture(),
            jungleBg: createJungleBackground()
        };
        
        // Game state
        let gameRunning = false;
        let currentLevel = 1;
        let score = 0;
        let lives = 3;
        let levelTime = 0;
        let totalTime = 0;
        let timerInterval;
        let playerName = "Player";
        let coinsCollected = 0;
        let powerupActive = false;
        let powerupTimeLeft = 0;
        let powerupEffect = null;
        
        // Player properties
        const player = {
            x: 50,
            y: 400,
            width: 35,
            height: 55,
            speed: 5,
            jumpForce: 14,
            velocityY: 0,
            jumping: false,
            direction: 1
        };
        
        // Boss properties
        const boss = {
            x: 700,
            y: 380,
            width: 120,
            height: 100,
            speed: 2,
            direction: -1,
            health: 100,
            maxHealth: 100,
            attackCooldown: 0,
            jumpCooldown: 0,
            isAttacking: false
        };
        
        // Game objects
        let platforms = [];
        let coins = [];
        let bananas = [];
        let crocodiles = [];
        let trees = [];
        let rocks = [];
        let levelGoal = { x: 0, y: 0, width: 40, height: 40 };
        let waterAreas = [];
        
        // Key states
        const keys = {};
        
        // Leaderboard data
        let leaderboard = [];
        
        // Create texture functions
        function createGrassTexture() {
            const canvas = document.createElement('canvas');
            canvas.width = 50;
            canvas.height = 50;
            const ctx = canvas.getContext('2d');
            
            // Base green
            ctx.fillStyle = '#7CB342';
            ctx.fillRect(0, 0, 50, 50);
            
            // Grass blades
            ctx.strokeStyle = '#5D8C2B';
            ctx.lineWidth = 2;
            for (let i = 0; i < 20; i++) {
                const x = Math.random() * 50;
                const height = 5 + Math.random() * 10;
                ctx.beginPath();
                ctx.moveTo(x, 50);
                ctx.lineTo(x, 50 - height);
                ctx.stroke();
            }
            
            return canvas;
        }
        
        function createTreeTexture() {
            const canvas = document.createElement('canvas');
            canvas.width = 80;
            canvas.height = 120;
            const ctx = canvas.getContext('2d');
            
            // Trunk
            ctx.fillStyle = '#8B4513';
            ctx.fillRect(35, 40, 10, 80);
            
            // Leaves
            ctx.fillStyle = '#2E8B57';
            ctx.beginPath();
            ctx.arc(40, 30, 25, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.beginPath();
            ctx.arc(30, 20, 20, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.beginPath();
            ctx.arc(50, 20, 20, 0, Math.PI * 2);
            ctx.fill();
            
            return canvas;
        }
        
        function createWaterTexture() {
            const canvas = document.createElement('canvas');
            canvas.width = 50;
            canvas.height = 50;
            const ctx = canvas.getContext('2d');
            
            // Water base
            ctx.fillStyle = '#1E90FF';
            ctx.fillRect(0, 0, 50, 50);
            
            // Wave lines
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.4)';
            ctx.lineWidth = 2;
            for (let i = 0; i < 5; i++) {
                const y = 10 + i * 10;
                ctx.beginPath();
                ctx.moveTo(0, y);
                for (let x = 0; x < 50; x += 5) {
                    ctx.lineTo(x, y + Math.sin(x/10) * 2);
                }
                ctx.stroke();
            }
            
            return canvas;
        }
        
        function createRockTexture() {
            const canvas = document.createElement('canvas');
            canvas.width = 50;
            canvas.height = 50;
            const ctx = canvas.getContext('2d');
            
            // Rock base
            ctx.fillStyle = '#708090';
            ctx.beginPath();
            ctx.arc(25, 25, 20, 0, Math.PI * 2);
            ctx.fill();
            
            // Rock details
            ctx.strokeStyle = '#5A6672';
            ctx.lineWidth = 1;
            for (let i = 0; i < 10; i++) {
                const angle = Math.random() * Math.PI * 2;
                const length = 5 + Math.random() * 10;
                ctx.beginPath();
                ctx.moveTo(25, 25);
                ctx.lineTo(
                    25 + Math.cos(angle) * length,
                    25 + Math.sin(angle) * length
                );
                ctx.stroke();
            }
            
            return canvas;
        }
        
        function createJungleBackground() {
            const canvas = document.createElement('canvas');
            canvas.width = 900;
            canvas.height = 500;
            const ctx = canvas.getContext('2d');
            
            // Sky gradient
            const skyGradient = ctx.createLinearGradient(0, 0, 0, 300);
            skyGradient.addColorStop(0, '#87CEEB');
            skyGradient.addColorStop(1, '#98FB98');
            ctx.fillStyle = skyGradient;
            ctx.fillRect(0, 0, 900, 300);
            
            // Distant mountains
            ctx.fillStyle = '#2F4F4F';
            ctx.beginPath();
            ctx.moveTo(0, 300);
            for (let i = 0; i < 20; i++) {
                const x = i * 50;
                const height = 50 + Math.random() * 70;
                ctx.lineTo(x, 300 - height);
            }
            ctx.lineTo(900, 300);
            ctx.fill();
            
            // Sun
            ctx.fillStyle = '#FFD700';
            ctx.beginPath();
            ctx.arc(800, 80, 40, 0, Math.PI * 2);
            ctx.fill();
            
            // Jungle trees in background
            ctx.fillStyle = '#006400';
            for (let i = 0; i < 30; i++) {
                const x = Math.random() * 900;
                const height = 80 + Math.random() * 100;
                const width = 30 + Math.random() * 40;
                ctx.beginPath();
                ctx.moveTo(x, 300);
                ctx.lineTo(x - width/2, 300 - height);
                ctx.lineTo(x + width/2, 300 - height);
                ctx.closePath();
                ctx.fill();
            }
            
            return canvas;
        }
        
        // Event listeners
        window.addEventListener('keydown', (e) => {
            keys[e.key] = true;
            // Also map WASD to arrow keys
            if (e.key === 'a' || e.key === 'A') keys['ArrowLeft'] = true;
            if (e.key === 'd' || e.key === 'D') keys['ArrowRight'] = true;
            if (e.key === 'w' || e.key === 'W') keys[' '] = true;
        });
        
        window.addEventListener('keyup', (e) => {
            keys[e.key] = false;
            // Also map WASD to arrow keys
            if (e.key === 'a' || e.key === 'A') keys['ArrowLeft'] = false;
            if (e.key === 'd' || e.key === 'D') keys['ArrowRight'] = false;
            if (e.key === 'w' || e.key === 'W') keys[' '] = false;
        });
        
        startButton.addEventListener('click', startGame);
        nextLevelButton.addEventListener('click', loadNextLevel);
        restartButton.addEventListener('click', restartGame);
        startBossButton.addEventListener('click', startBossBattle);
        submitScoreButton.addEventListener('click', submitScore);
        
        // Initialize leaderboard from localStorage
        function loadLeaderboard() {
            const savedLeaderboard = localStorage.getItem('jungleLeaderboard');
            if (savedLeaderboard) {
                leaderboard = JSON.parse(savedLeaderboard);
            } else {
                leaderboard = [
                    { name: "Jungle Master", time: 120, score: 3000 },
                    { name: "Croc Hunter", time: 180, score: 2500 },
                    { name: "Adventurer", time: 240, score: 2000 },
                    { name: "Explorer", time: 300, score: 1500 },
                    { name: "Beginner", time: 360, score: 1000 }
                ];
            }
            updateLeaderboardDisplay();
        }
        
        function updateLeaderboardDisplay() {
            leaderboardList.innerHTML = '';
            leaderboard.sort((a, b) => a.time - b.time).forEach(entry => {
                const li = document.createElement('li');
                const minutes = Math.floor(entry.time / 60);
                const seconds = entry.time % 60;
                li.innerHTML = `${entry.name} - ${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')} - ${entry.score} pts`;
                leaderboardList.appendChild(li);
            });
        }
        
        function startGame() {
            loadLeaderboard();
            startScreen.style.display = 'none';
            currentLevel = 1;
            score = 0;
            lives = 3;
            coinsCollected = 0;
            totalTime = 0;
            updateCoinsToLife();
            loadLevel(currentLevel);
        }
        
        function loadLevel(level) {
            if (level === 5) {
                // Boss level
                bossIntroScreen.style.display = 'flex';
                return;
            }
            
            const config = levelConfigs[level - 1];
            platforms = JSON.parse(JSON.stringify(config.platforms));
            coins = JSON.parse(JSON.stringify(config.coins));
            bananas = JSON.parse(JSON.stringify(config.bananas));
            crocodiles = JSON.parse(JSON.stringify(config.crocodiles));
            trees = JSON.parse(JSON.stringify(config.trees || []));
            rocks = JSON.parse(JSON.stringify(config.rocks || []));
            levelGoal = JSON.parse(JSON.stringify(config.goal));
            waterAreas = JSON.parse(JSON.stringify(config.waterAreas));
            
            // Set initial positions for crocodiles
            crocodiles.forEach(croc => {
                croc.initialX = croc.x;
            });
            
            // Reset player position
            player.x = 50;
            player.y = 400;
            player.velocityY = 0;
            player.jumping = false;
            player.speed = 5;
            
            // Reset powerup
            powerupActive = false;
            powerupTimeLeft = 0;
            powerupEffect = null;
            powerupTimerElement.textContent = "0s";
            document.getElementById('game-container').classList.remove('powerup-active');
            
            // Hide boss health
            bossHealthElement.style.display = 'none';
            
            // Update UI
            levelDisplay.textContent = level;
            updateScore();
            updateLives();
            
            // Start timer
            levelTime = 0;
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                levelTime++;
                totalTime++;
                updateTimer();
                
                // Update powerup timer
                if (powerupActive) {
                    powerupTimeLeft--;
                    powerupTimerElement.textContent = `${powerupTimeLeft}s`;
                    
                    if (powerupTimeLeft <= 0) {
                        endPowerup();
                    }
                }
                
                // Update crocodile water behavior
                updateCrocodiles();
            }, 1000);
            
            gameRunning = true;
            gameLoop();
        }
        
        function startBossBattle() {
            bossIntroScreen.style.display = 'none';
            
            // Setup boss level
            platforms = [
                { x: 0, y: 460, width: 900, height: 40 },
                { x: 200, y: 350, width: 100, height: 20 },
                { x: 400, y: 350, width: 100, height: 20 },
                { x: 600, y: 350, width: 100, height: 20 }
            ];
            
            coins = [];
            bananas = [];
            crocodiles = [];
            trees = [];
            rocks = [];
            waterAreas = [];
            
            // Reset boss
            boss.health = boss.maxHealth;
            boss.x = 700;
            boss.y = 380;
            boss.direction = -1;
            boss.attackCooldown = 0;
            boss.jumpCooldown = 0;
            boss.isAttacking = false;
            
            // Reset player position
            player.x = 100;
            player.y = 400;
            player.velocityY = 0;
            player.jumping = false;
            player.speed = 5;
            
            // Show boss health
            bossHealthElement.style.display = 'flex';
            updateBossHealth();
            
            // Update UI
            levelDisplay.textContent = 5;
            
            // Start timer
            levelTime = 0;
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                levelTime++;
                totalTime++;
                updateTimer();
                
                // Update boss behavior
                updateBoss();
            }, 1000);
            
            gameRunning = true;
            gameLoop();
        }
        
        function updateBoss() {
            // Move boss
            boss.x += boss.speed * boss.direction;
            
            // Change direction at edges
            if (boss.x <= 200 || boss.x + boss.width >= 850) {
                boss.direction *= -1;
            }
            
            // Attack cooldown
            if (boss.attackCooldown > 0) {
                boss.attackCooldown--;
            }
            
            // Jump cooldown
            if (boss.jumpCooldown > 0) {
                boss.jumpCooldown--;
            }
            
            // Random attacks
            if (boss.attackCooldown <= 0 && Math.random() < 0.02) {
                boss.isAttacking = true;
                boss.attackCooldown = 60; // 1 second cooldown
                
                // Charge attack
                if (Math.random() < 0.5) {
                    boss.speed = 6;
                    setTimeout(() => {
                        boss.speed = 2;
                        boss.isAttacking = false;
                    }, 1000);
                }
            }
            
            // Random jumps
            if (boss.jumpCooldown <= 0 && Math.random() < 0.01) {
                boss.y = 300;
                setTimeout(() => {
                    boss.y = 380;
                    boss.jumpCooldown = 120; // 2 second cooldown
                }, 500);
            }
        }
        
        function updateCrocodiles() {
            for (const croc of crocodiles) {
                // Randomly make crocodiles go in and out of water
                if (croc.inWater) {
                    croc.waterTimer--;
                    if (croc.waterTimer <= 0) {
                        croc.inWater = false;
                        croc.y = 410;
                    }
                } else {
                    // 5% chance each second to go in water
                    if (Math.random() < 0.05) {
                        croc.inWater = true;
                        croc.waterTimer = 2 + Math.floor(Math.random() * 3); // 2-4 seconds in water
                        croc.y = 480; // Hide in water
                    }
                }
            }
        }
        
        function loadNextLevel() {
            currentLevel++;
            if (currentLevel <= 5) {
                levelCompleteScreen.style.display = 'none';
                loadLevel(currentLevel);
            } else {
                gameComplete();
            }
        }
        
        function gameComplete() {
            clearInterval(timerInterval);
            gameRunning = false;
            leaderboardScreen.style.display = 'flex';
            finalScoreElement.textContent = score;
        }
        
        function restartGame() {
            gameOverScreen.style.display = 'none';
            startGame();
        }
        
        function submitScore() {
            const name = nameInput.value.trim() || playerName;
            leaderboard.push({ name, time: totalTime, score });
            localStorage.setItem('jungleLeaderboard', JSON.stringify(leaderboard));
            updateLeaderboardDisplay();
            leaderboardScreen.style.display = 'none';
            startScreen.style.display = 'flex';
            startButton.textContent = 'Play Again';
        }
        
        function gameLoop() {
            if (!gameRunning) return;
            
            update();
            render();
            requestAnimationFrame(gameLoop);
        }
        
        function update() {
            // Apply gravity
            player.velocityY += 0.5;
            
            // Move player (with powerup speed if active)
            const currentSpeed = powerupEffect === 'speed' ? player.speed * 1.8 : player.speed;
            
            if (keys['ArrowLeft'] || keys['a'] || keys['A']) {
                player.x -= currentSpeed;
                player.direction = -1;
            }
            if (keys['ArrowRight'] || keys['d'] || keys['D']) {
                player.x += currentSpeed;
                player.direction = 1;
            }
            
            // Jump
            if ((keys[' '] || keys['w'] || keys['W']) && !player.jumping) {
                player.velocityY = -player.jumpForce;
                player.jumping = true;
            }
            
            // Apply velocity
            player.y += player.velocityY;
            
            // Check platform collisions
            player.jumping = true;
            for (const platform of platforms) {
                if (
                    player.x < platform.x + platform.width &&
                    player.x + player.width > platform.x &&
                    player.y + player.height > platform.y &&
                    player.y + player.height <= platform.y + platform.height + player.velocityY
                ) {
                    player.y = platform.y - player.height;
                    player.velocityY = 0;
                    player.jumping = false;
                }
            }
            
            // Screen boundaries
            if (player.x < 0) player.x = 0;
            if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;
            
            // Check if player fell off the screen
            if (player.y > canvas.height) {
                loseLife();
                resetPlayer();
            }
            
            // Update crocodiles (for non-boss levels)
            if (currentLevel < 5) {
                for (const croc of crocodiles) {
                    if (!croc.inWater) {
                        croc.x += croc.speed * croc.direction;
                        
                        // Change direction if hitting movement boundaries
                        if (croc.direction > 0 && croc.x > croc.initialX + croc.moveDistance) {
                            croc.direction *= -1;
                        } else if (croc.direction < 0 && croc.x < croc.initialX) {
                            croc.direction *= -1;
                        }
                        
                        // Check collision with player
                        if (
                            player.x < croc.x + croc.width &&
                            player.x + player.width > croc.x &&
                            player.y < croc.y + croc.height &&
                            player.y + player.height > croc.y
                        ) {
                            // If player is above crocodile, defeat it
                            if (player.velocityY > 0 && player.y + player.height < croc.y + croc.height / 2) {
                                croc.x = -200; // Remove crocodile
                                player.velocityY = -10; // Bounce
                                score += 100;
                                updateScore();
                            } else {
                                loseLife();
                                resetPlayer();
                            }
                        }
                    }
                }
            }
            
            // Boss battle logic
            if (currentLevel === 5) {
                // Check collision with boss
                if (
                    player.x < boss.x + boss.width &&
                    player.x + player.width > boss.x &&
                    player.y < boss.y + boss.height &&
                    player.y + player.height > boss.y
                ) {
                    // If player is above boss, damage it
                    if (player.velocityY > 0 && player.y + player.height < boss.y + boss.height / 2) {
                        boss.health -= 10;
                        updateBossHealth();
                        player.velocityY = -12; // Bounce higher
                        score += 50;
                        updateScore();
                        
                        // Check if boss is defeated
                        if (boss.health <= 0) {
                            score += 500;
                            updateScore();
                            levelComplete();
                        }
                    } else {
                        // Player is hit by boss
                        loseLife();
                        resetPlayer();
                    }
                }
            }
            
            // Check coin collisions
            for (const coin of coins) {
                if (
                    !coin.collected &&
                    player.x < coin.x + coin.width &&
                    player.x + player.width > coin.x &&
                    player.y < coin.y + coin.height &&
                    player.y + player.height > coin.y
                ) {
                    coin.collected = true;
                    score += 50;
                    coinsCollected++;
                    updateScore();
                    updateCoinsToLife();
                    
                    // Check for extra life
                    if (coinsCollected % 50 === 0) {
                        lives++;
                        updateLives();
                    }
                }
            }
            
            // Check banana collisions
            for (const banana of bananas) {
                if (
                    !banana.collected &&
                    player.x < banana.x + banana.width &&
                    player.x + player.width > banana.x &&
                    player.y < banana.y + banana.height &&
                    player.y + player.height > banana.y
                ) {
                    banana.collected = true;
                    activatePowerup('speed');
                    score += 30;
                    updateScore();
                }
            }
            
            // Check if player reached the goal (for non-boss levels)
            if (currentLevel < 5 &&
                player.x < levelGoal.x + levelGoal.width &&
                player.x + player.width > levelGoal.x &&
                player.y < levelGoal.y + levelGoal.height &&
                player.y + player.height > levelGoal.y
            ) {
                levelComplete();
            }
        }
        
        function updateBossHealth() {
            const healthPercent = (boss.health / boss.maxHealth) * 100;
            bossHealthFill.style.width = `${healthPercent}%`;
        }
        
        function activatePowerup(type) {
            powerupActive = true;
            powerupTimeLeft = 5; // 5 seconds
            powerupEffect = type;
            powerupTimerElement.textContent = `${powerupTimeLeft}s`;
            document.getElementById('game-container').classList.add('powerup-active');
        }
        
        function endPowerup() {
            powerupActive = false;
            powerupEffect = null;
            powerupTimerElement.textContent = "0s";
            document.getElementById('game-container').classList.remove('powerup-active');
        }
        
        function updateCoinsToLife() {
            const coinsNeeded = 50 - (coinsCollected % 50);
            coinsToLifeElement.textContent = coinsNeeded;
        }
        
        function render() {
            // Clear canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Draw background
            ctx.drawImage(textures.jungleBg, 0, 0);
            
            // Draw water areas
            waterAreas.forEach(water => {
                // Draw water texture with pattern
                const waterPattern = ctx.createPattern(textures.water, 'repeat');
                ctx.fillStyle = waterPattern;
                ctx.fillRect(water.x, water.y, water.width, water.height);
            });
            
            // Draw platforms with grass texture
            platforms.forEach(platform => {
                const grassPattern = ctx.createPattern(textures.grass, 'repeat');
                ctx.fillStyle = grassPattern;
                ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
            });
            
            // Draw trees
            trees.forEach(tree => {
                ctx.drawImage(textures.tree, tree.x, tree.y - 80, 80, 120);
            });
            
            // Draw rocks
            rocks.forEach(rock => {
                ctx.drawImage(textures.rock, rock.x, rock.y, 40, 40);
            });
            
            // Draw coins
            coins.forEach(coin => {
                if (!coin.collected) {
                    ctx.fillStyle = '#FFD700';
                    ctx.beginPath();
                    ctx.arc(coin.x + coin.width/2, coin.y + coin.height/2, coin.width/2, 0, Math.PI * 2);
                    ctx.fill();
                    
                    ctx.fillStyle = '#FFC400';
                    ctx.beginPath();
                    ctx.arc(coin.x + coin.width/2 - 3, coin.y + coin.height/2 - 3, 3, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Shine effect
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.7)';
                    ctx.beginPath();
                    ctx.arc(coin.x + coin.width/2 - 2, coin.y + coin.height/2 - 2, coin.width/4, 0, Math.PI * 2);
                    ctx.fill();
                }
            });
            
            // Draw bananas
            bananas.forEach(banana => {
                if (!banana.collected) {
                    ctx.fillStyle = '#FFEB3B';
                    ctx.beginPath();
                    ctx.moveTo(banana.x, banana.y + banana.height);
                    ctx.bezierCurveTo(
                        banana.x, banana.y,
                        banana.x + banana.width, banana.y,
                        banana.x + banana.width, banana.y + banana.height/2
                    );
                    ctx.bezierCurveTo(
                        banana.x + banana.width, banana.y + banana.height,
                        banana.x, banana.y + banana.height,
                        banana.x, banana.y + banana.height
                    );
                    ctx.fill();
                    
                    ctx.fillStyle = '#8BC34A';
                    ctx.fillRect(banana.x + banana.width - 5, banana.y, 5, 8);
                }
            });
            
            // Draw crocodiles (only if not in water)
            crocodiles.forEach(croc => {
                if (!croc.inWater) {
                    ctx.fillStyle = '#4CAF50';
                    ctx.fillRect(croc.x, croc.y, croc.width, croc.height);
                    
                    // Crocodile eyes
                    ctx.fillStyle = 'white';
                    ctx.fillRect(croc.x + 10, croc.y + 5, 8, 8);
                    ctx.fillRect(croc.x + 40, croc.y + 5, 8, 8);
                    
                    ctx.fillStyle = 'black';
                    ctx.fillRect(croc.x + 12, croc.y + 7, 4, 4);
                    ctx.fillRect(croc.x + 42, croc.y + 7, 4, 4);
                    
                    // Crocodile teeth
                    ctx.fillStyle = 'white';
                    for (let i = 0; i < 4; i++) {
                        ctx.fillRect(croc.x + 15 + i*10, croc.y, 5, 5);
                    }
                }
            });
            
            // Draw boss bear
            if (currentLevel === 5) {
                // Draw bear body
                ctx.fillStyle = '#8B4513';
                ctx.fillRect(boss.x, boss.y, boss.width, boss.height);
                
                // Draw bear head
                ctx.fillStyle = '#8B4513';
                ctx.fillRect(boss.x + 30, boss.y - 40, 60, 50);
                
                // Draw bear ears
                ctx.fillStyle = '#6B3410';
                ctx.beginPath();
                ctx.arc(boss.x + 40, boss.y - 35, 10, 0, Math.PI * 2);
                ctx.fill();
                ctx.beginPath();
                ctx.arc(boss.x + 80, boss.y - 35, 10, 0, Math.PI * 2);
                ctx.fill();
                
                // Draw bear eyes
                ctx.fillStyle = 'black';
                ctx.fillRect(boss.x + 45, boss.y - 25, 5, 5);
                ctx.fillRect(boss.x + 70, boss.y - 25, 5, 5);
                
                // Draw bear nose
                ctx.fillStyle = 'black';
                ctx.beginPath();
                ctx.arc(boss.x + 57.5, boss.y - 10, 8, 0, Math.PI * 2);
                ctx.fill();
                
                // Draw bear mouth
                ctx.strokeStyle = 'black';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(boss.x + 57.5, boss.y - 5, 5, 0, Math.PI);
                ctx.stroke();
                
                // Draw bear arms
                ctx.fillStyle = '#6B3410';
                ctx.fillRect(boss.x - 10, boss.y + 20, 20, 15);
                ctx.fillRect(boss.x + boss.width - 10, boss.y + 20, 20, 15);
                
                // Draw bear legs
                ctx.fillRect(boss.x + 20, boss.y + boss.height - 10, 25, 20);
                ctx.fillRect(boss.x + boss.width - 45, boss.y + boss.height - 10, 25, 20);
                
                // Draw angry eyebrows if attacking
                if (boss.isAttacking) {
                    ctx.strokeStyle = 'black';
                    ctx.lineWidth = 3;
                    ctx.beginPath();
                    ctx.moveTo(boss.x + 42, boss.y - 30);
                    ctx.lineTo(boss.x + 50, boss.y - 35);
                    ctx.stroke();
                    ctx.beginPath();
                    ctx.moveTo(boss.x + 73, boss.y - 30);
                    ctx.lineTo(boss.x + 65, boss.y - 35);
                    ctx.stroke();
                }
            }
            
            // Draw goal (for non-boss levels)
            if (currentLevel < 5) {
                ctx.fillStyle = '#FF5722';
                ctx.fillRect(levelGoal.x, levelGoal.y, levelGoal.width, levelGoal.height);
                ctx.fillStyle = '#FFD700';
                ctx.beginPath();
                ctx.moveTo(levelGoal.x + levelGoal.width/2, levelGoal.y);
                ctx.lineTo(levelGoal.x + levelGoal.width - 5, levelGoal.y + levelGoal.height);
                ctx.lineTo(levelGoal.x + 5, levelGoal.y + levelGoal.height);
                ctx.closePath();
                ctx.fill();
            }
            
            // Draw player
            ctx.fillStyle = powerupActive ? '#9C27B0' : '#3564FC'; // Purple during powerup, blue normally
            ctx.fillRect(player.x, player.y + 15, player.width, player.height - 15);
            
            ctx.fillStyle = '#F4D47C'; // Skin
            ctx.fillRect(player.x + 5, player.y, player.width - 10, 15);
            
            ctx.fillStyle = '#663300'; // Hair
            ctx.fillRect(player.x, player.y, player.width, 5);
            
            ctx.fillStyle = powerupActive ? '#7B1FA2' : '#294ECC'; // Darker purple/blue for pants
            ctx.fillRect(player.x, player.y + player.height - 10, player.width, 10);
            
            // Player eyes
            ctx.fillStyle = 'white';
            if (player.direction > 0) {
                ctx.fillRect(player.x + 25, player.y + 5, 5, 5);
            } else {
                ctx.fillRect(player.x + 5, player.y + 5, 5, 5);
            }
        }
        
        function updateScore() {
            scoreElement.textContent = score;
        }
        
        function updateLives() {
            livesElement.textContent = lives;
        }
        
        function updateTimer() {
            const minutes = Math.floor(levelTime / 60);
            const seconds = levelTime % 60;
            timerElement.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }
        
        function loseLife() {
            lives--;
            updateLives();
            
            if (lives <= 0) {
                gameOver();
            }
        }
        
        function resetPlayer() {
            player.x = currentLevel === 5 ? 100 : 50;
            player.y = 400;
            player.velocityY = 0;
            endPowerup();
        }
        
        function levelComplete() {
            clearInterval(timerInterval);
            gameRunning = false;
            
            // Calculate level bonus (faster completion = more points)
            const timeBonus = Math.max(0, 300 - levelTime) * 2;
            score += timeBonus;
            
            const minutes = Math.floor(levelTime / 60);
            const seconds = levelTime % 60;
            
            levelTimeElement.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            totalScoreElement.textContent = score;
            
            levelCompleteScreen.style.display = 'flex';
        }
        
        function gameOver() {
            clearInterval(timerInterval);
            gameRunning = false;
            gameOverScreen.style.display = 'flex';
            finalScoreElement.textContent = score;
        }
        
        // Level configurations
        const levelConfigs = [
            // Level 1 - Easy
            {
                platforms: [
                    { x: 0, y: 460, width: 900, height: 40 },
                    { x: 200, y: 350, width: 150, height: 20 },
                    { x: 400, y: 250, width: 150, height: 20 },
                    { x: 600, y: 350, width: 150, height: 20 },
                    { x: 750, y: 250, width: 100, height: 20 }
                ],
                coins: [
                    { x: 230, y: 320, width: 20, height: 20, collected: false },
                    { x: 430, y: 220, width: 20, height: 20, collected: false },
                    { x: 630, y: 320, width: 20, height: 20, collected: false },
                    { x: 780, y: 220, width: 20, height: 20, collected: false },
                    { x: 500, y: 400, width: 20, height: 20, collected: false },
                    { x: 300, y: 400, width: 20, height: 20, collected: false },
                    { x: 550, y: 150, width: 20, height: 20, collected: false }
                ],
                bananas: [
                    { x: 350, y: 310, width: 25, height: 35, collected: false },
                    { x: 550, y: 210, width: 25, height: 35, collected: false }
                ],
                crocodiles: [
                    { x: 300, y: 410, width: 60, height: 30, speed: 1.5, direction: 1, moveDistance: 200, inWater: false, waterTimer: 0 }
                ],
                trees: [
                    { x: 100, y: 460 }, { x: 500, y: 460 }, { x: 800, y: 460 }
                ],
                rocks: [
                    { x: 50, y: 420 }, { x: 850, y: 420 }
                ],
                waterAreas: [
                    { x: 250, y: 460, width: 300, height: 40 }
                ],
                goal: { x: 830, y: 210, width: 40, height: 40 }
            },
            
            // Level 2 - Medium
            {
                platforms: [
                    { x: 0, y: 460, width: 900, height: 40 },
                    { x: 100, y: 350, width: 120, height: 20 },
                    { x: 300, y: 350, width: 120, height: 20 },
                    { x: 500, y: 250, width: 120, height: 20 },
                    { x: 650, y: 350, width: 120, height: 20 },
                    { x: 200, y: 150, width: 100, height: 20 },
                    { x: 700, y: 150, width: 100, height: 20 },
                    { x: 450, y: 350, width: 100, height: 20 }
                ],
                coins: [
                    { x: 130, y: 320, width: 20, height: 20, collected: false },
                    { x: 330, y: 320, width: 20, height: 20, collected: false },
                    { x: 530, y: 220, width: 20, height: 20, collected: false },
                    { x: 680, y: 320, width: 20, height: 20, collected: false },
                    { x: 230, y: 120, width: 20, height: 20, collected: false },
                    { x: 730, y: 120, width: 20, height: 20, collected: false },
                    { x: 450, y: 400, width: 20, height: 20, collected: false },
                    { x: 480, y: 320, width: 20, height: 20, collected: false }
                ],
                bananas: [
                    { x: 550, y: 210, width: 25, height: 35, collected: false },
                    { x: 250, y: 320, width: 25, height: 35, collected: false },
                    { x: 750, y: 120, width: 25, height: 35, collected: false }
                ],
                crocodiles: [
                    { x: 200, y: 410, width: 60, height: 30, speed: 2, direction: 1, moveDistance: 150, inWater: false, waterTimer: 0 },
                    { x: 550, y: 410, width: 60, height: 30, speed: 2, direction: -1, moveDistance: 150, inWater: false, waterTimer: 0 }
                ],
                trees: [
                    { x: 50, y: 460 }, { x: 300, y: 460 }, { x: 600, y: 460 }, { x: 850, y: 460 }
                ],
                rocks: [
                    { x: 150, y: 420 }, { x: 400, y: 420 }, { x: 750, y: 420 }
                ],
                waterAreas: [
                    { x: 150, y: 460, width: 200, height: 40 },
                    { x: 500, y: 460, width: 200, height: 40 }
                ],
                goal: { x: 830, y: 110, width: 40, height: 40 }
            },
            
            // Level 3 - Hard
            {
                platforms: [
                    { x: 0, y: 460, width: 900, height: 40 },
                    { x: 100, y: 380, width: 100, height: 20 },
                    { x: 250, y: 300, width: 100, height: 20 },
                    { x: 400, y: 220, width: 100, height: 20 },
                    { x: 550, y: 300, width: 100, height: 20 },
                    { x: 700, y: 380, width: 100, height: 20 },
                    { x: 300, y: 150, width: 80, height: 20 },
                    { x: 500, y: 150, width: 80, height: 20 },
                    { x: 150, y: 250, width: 80, height: 20 },
                    { x: 650, y: 250, width: 80, height: 20 },
                    { x: 400, y: 380, width: 80, height: 20 }
                ],
                coins: [
                    { x: 130, y: 350, width: 20, height: 20, collected: false },
                    { x: 280, y: 270, width: 20, height: 20, collected: false },
                    { x: 430, y: 190, width: 20, height: 20, collected: false },
                    { x: 580, y: 270, width: 20, height: 20, collected: false },
                    { x: 730, y: 350, width: 20, height: 20, collected: false },
                    { x: 330, y: 120, width: 20, height: 20, collected: false },
                    { x: 530, y: 120, width: 20, height: 20, collected: false },
                    { x: 180, y: 220, width: 20, height: 20, collected: false },
                    { x: 680, y: 220, width: 20, height: 20, collected: false },
                    { x: 450, y: 400, width: 20, height: 20, collected: false },
                    { x: 430, y: 350, width: 20, height: 20, collected: false }
                ],
                bananas: [
                    { x: 430, y: 180, width: 25, height: 35, collected: false },
                    { x: 580, y: 260, width: 25, height: 35, collected: false },
                    { x: 280, y: 260, width: 25, height: 35, collected: false },
                    { x: 730, y: 340, width: 25, height: 35, collected: false }
                ],
                crocodiles: [
                    { x: 150, y: 410, width: 60, height: 30, speed: 2.5, direction: 1, moveDistance: 100, inWater: false, waterTimer: 0 },
                    { x: 400, y: 410, width: 60, height: 30, speed: 2, direction: -1, moveDistance: 200, inWater: false, waterTimer: 0 },
                    { x: 650, y: 410, width: 60, height: 30, speed: 2.5, direction: 1, moveDistance: 100, inWater: false, waterTimer: 0 }
                ],
                trees: [
                    { x: 50, y: 460 }, { x: 200, y: 460 }, { x: 400, y: 460 }, { x: 650, y: 460 }, { x: 850, y: 460 }
                ],
                rocks: [
                    { x: 100, y: 420 }, { x: 300, y: 420 }, { x: 500, y: 420 }, { x: 800, y: 420 }
                ],
                waterAreas: [
                    { x: 100, y: 460, width: 150, height: 40 },
                    { x: 375, y: 460, width: 150, height: 40 },
                    { x: 650, y: 460, width: 150, height: 40 }
                ],
                goal: { x: 830, y: 330, width: 40, height: 40 }
            },
            
            // Level 4 - Very Hard
            {
                platforms: [
                    { x: 0, y: 460, width: 900, height: 40 },
                    { x: 100, y: 350, width: 80, height: 20 },
                    { x: 250, y: 250, width: 80, height: 20 },
                    { x: 400, y: 350, width: 80, height: 20 },
                    { x: 550, y: 250, width: 80, height: 20 },
                    { x: 700, y: 350, width: 80, height: 20 },
                    { x: 200, y: 150, width: 60, height: 20 },
                    { x: 400, y: 150, width: 60, height: 20 },
                    { x: 600, y: 150, width: 60, height: 20 },
                    { x: 350, y: 300, width: 60, height: 20 },
                    { x: 550, y: 350, width: 60, height: 20 },
                    { x: 750, y: 250, width: 60, height: 20 },
                    { x: 300, y: 350, width: 60, height: 20 }
                ],
                coins: [
                    { x: 120, y: 320, width: 20, height: 20, collected: false },
                    { x: 270, y: 220, width: 20, height: 20, collected: false },
                    { x: 420, y: 320, width: 20, height: 20, collected: false },
                    { x: 570, y: 220, width: 20, height: 20, collected: false },
                    { x: 720, y: 320, width: 20, height: 20, collected: false },
                    { x: 220, y: 120, width: 20, height: 20, collected: false },
                    { x: 420, y: 120, width: 20, height: 20, collected: false },
                    { x: 620, y: 120, width: 20, height: 20, collected: false },
                    { x: 370, y: 270, width: 20, height: 20, collected: false },
                    { x: 570, y: 320, width: 20, height: 20, collected: false },
                    { x: 770, y: 220, width: 20, height: 20, collected: false },
                    { x: 500, y: 400, width: 20, height: 20, collected: false },
                    { x: 330, y: 320, width: 20, height: 20, collected: false }
                ],
                bananas: [
                    { x: 270, y: 210, width: 25, height: 35, collected: false },
                    { x: 570, y: 210, width: 25, height: 35, collected: false },
                    { x: 370, y: 260, width: 25, height: 35, collected: false },
                    { x: 720, y: 310, width: 25, height: 35, collected: false },
                    { x: 120, y: 310, width: 25, height: 35, collected: false }
                ],
                crocodiles: [
                    { x: 100, y: 410, width: 60, height: 30, speed: 3, direction: 1, moveDistance: 150, inWater: false, waterTimer: 0 },
                    { x: 350, y: 410, width: 60, height: 30, speed: 2.5, direction: -1, moveDistance: 200, inWater: false, waterTimer: 0 },
                    { x: 600, y: 410, width: 60, height: 30, speed: 3, direction: 1, moveDistance: 150, inWater: false, waterTimer: 0 },
                    { x: 250, y: 210, width: 60, height: 30, speed: 2, direction: 1, moveDistance: 80, inWater: false, waterTimer: 0 },
                    { x: 550, y: 210, width: 60, height: 30, speed: 2, direction: -1, moveDistance: 80, inWater: false, waterTimer: 0 }
                ],
                trees: [
                    { x: 30, y: 460 }, { x: 150, y: 460 }, { x: 300, y: 460 }, 
                    { x: 450, y: 460 }, { x: 600, y: 460 }, { x: 750, y: 460 }, { x: 870, y: 460 }
                ],
                rocks: [
                    { x: 80, y: 420 }, { x: 230, y: 420 }, { x: 400, y: 420 }, 
                    { x: 550, y: 420 }, { x: 700, y: 420 }, { x: 820, y: 420 }
                ],
                waterAreas: [
                    { x: 50, y: 460, width: 200, height: 40 },
                    { x: 325, y: 460, width: 200, height: 40 },
                    { x: 600, y: 460, width: 200, height: 40 }
                ],
                goal: { x: 50, y: 410, width: 40, height: 40 }
            }
        ];
        
        // Initialize the game
        loadLeaderboard();
    </script>
</body>
</html>
