<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, maximum-scale=1.0">
    <title>오르방 마리오 모바일</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }

        body {
            background: linear-gradient(to bottom, #5C94FC 0%, #4169E1 30%, #87CEEB 100%);
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            width: 100vw;
            height: 100vh;
            position: fixed;
        }

        .game-container {
            width: 100vw;
            height: 100vh;
            position: relative;
            display: flex;
            flex-direction: column;
        }

        .game-header {
            height: 8vh;
            background: rgba(0,0,0,0.7);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 15px;
            color: #FFF;
            font-size: 16px;
            font-weight: bold;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.8);
            z-index: 10;
        }

        .score-info {
            display: flex;
            gap: 20px;
        }

        .lives-info {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        #gameCanvas {
            width: 100vw;
            height: 72vh;
            background: linear-gradient(to bottom, #5C94FC 0%, #87CEEB 40%, #32CD32 100%);
            display: block;
        }

        .controls-container {
            height: 20vh;
            background: rgba(0,0,0,0.8);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
            position: relative;
        }

        .d-pad {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }

        .d-pad-row {
            display: flex;
            gap: 5px;
        }

        .control-btn {
            width: 60px;
            height: 60px;
            background: linear-gradient(145deg, #4a4a4a, #2a2a2a);
            border: 3px solid #666;
            border-radius: 12px;
            color: #FFF;
            font-size: 24px;
            font-weight: bold;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
            transition: all 0.1s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.8);
        }

        .control-btn:active {
            background: linear-gradient(145deg, #2a2a2a, #4a4a4a);
            box-shadow: 0 2px 4px rgba(0,0,0,0.3);
            transform: translateY(2px);
        }

        .action-buttons {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .jump-btn {
            width: 80px;
            height: 80px;
            background: linear-gradient(145deg, #FF6B35, #E55100);
            border: 3px solid #FF8A50;
            border-radius: 50%;
            color: #FFF;
            font-size: 14px;
            font-weight: bold;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
            transition: all 0.1s;
            box-shadow: 0 6px 12px rgba(255, 107, 53, 0.4);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.8);
        }

        .jump-btn:active {
            background: linear-gradient(145deg, #E55100, #FF6B35);
            box-shadow: 0 3px 6px rgba(255, 107, 53, 0.4);
            transform: translateY(3px);
        }

        .run-btn {
            width: 70px;
            height: 70px;
            background: linear-gradient(145deg, #4CAF50, #2E7D32);
            border: 3px solid #66BB6A;
            border-radius: 50%;
            color: #FFF;
            font-size: 12px;
            font-weight: bold;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            user-select: none;
            transition: all 0.1s;
            box-shadow: 0 6px 12px rgba(76, 175, 80, 0.4);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.8);
        }

        .run-btn:active {
            background: linear-gradient(145deg, #2E7D32, #4CAF50);
            box-shadow: 0 3px 6px rgba(76, 175, 80, 0.4);
            transform: translateY(3px);
        }

        .power-indicator {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            color: #FFD700;
            font-size: 14px;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
            background: rgba(0,0,0,0.5);
            padding: 5px 15px;
            border-radius: 20px;
        }

        .game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(255, 255, 255, 0.95);
            border: 4px solid #FF6B35;
            color: #333;
            padding: 30px;
            border-radius: 20px;
            text-align: center;
            font-size: 20px;
            box-shadow: 0 0 40px rgba(0,0,0,0.7);
            display: none;
            z-index: 100;
            max-width: 80vw;
        }

        .start-screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            color: #FFF;
            text-align: center;
        }

        .start-btn {
            background: linear-gradient(45deg, #FF6B35, #F7931E);
            border: none;
            color: white;
            padding: 20px 40px;
            font-size: 20px;
            margin: 20px;
            cursor: pointer;
            border-radius: 25px;
            font-weight: bold;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
            box-shadow: 0 4px 15px rgba(255, 107, 53, 0.4);
            transition: all 0.3s ease;
        }

        .start-btn:active {
            transform: translateY(2px);
            box-shadow: 0 2px 8px rgba(255, 107, 53, 0.4);
        }

        .instructions {
            margin: 20px;
            font-size: 14px;
            line-height: 1.6;
            max-width: 80vw;
        }

        /* 가로 모드 대응 */
        @media screen and (orientation: landscape) {
            .game-header {
                height: 6vh;
                font-size: 14px;
            }
            
            #gameCanvas {
                height: 74vh;
            }
            
            .controls-container {
                height: 20vh;
            }
            
            .control-btn {
                width: 50px;
                height: 50px;
                font-size: 20px;
            }
            
            .jump-btn {
                width: 70px;
                height: 70px;
                font-size: 12px;
            }
            
            .run-btn {
                width: 60px;
                height: 60px;
                font-size: 10px;
            }
        }

        /* 소형 디바이스 대응 */
        @media screen and (max-width: 375px) {
            .control-btn {
                width: 50px;
                height: 50px;
                font-size: 20px;
            }
            
            .jump-btn {
                width: 70px;
                height: 70px;
            }
            
            .run-btn {
                width: 60px;
                height: 60px;
            }
            
            .score-info {
                gap: 15px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="start-screen" id="startScreen">
            <h1 style="font-size: 28px; margin-bottom: 20px;">🎮 오르방 마리오 모바일</h1>
            <div class="instructions">
                <p><strong>조작법:</strong></p>
                <p>← → : 좌우 이동</p>
                <p>점프 버튼: 점프</p>
                <p>달리기/파이어 버튼: 달리기 또는 파이어볼</p>
                <p><br>적을 밟아서 물리치고<br>코인과 파워업을 모으세요!</p>
            </div>
            <button class="start-btn" onclick="startGame()">게임 시작</button>
        </div>

        <div class="game-header">
            <div class="score-info">
                <span>점수: <span id="score">0</span></span>
                <span>코인: <span id="coins">0</span></span>
            </div>
            <div class="lives-info">
                <span>× <span id="lives">3</span></span>
            </div>
        </div>

        <canvas id="gameCanvas"></canvas>

        <div class="controls-container">
            <div class="power-indicator">
                <span id="powerStatus">일반 오르방</span>
            </div>
            
            <div class="d-pad">
                <div class="d-pad-row">
                    <div class="control-btn" id="leftBtn">←</div>
                    <div class="control-btn" id="rightBtn">→</div>
                </div>
            </div>

            <div class="action-buttons">
                <div class="run-btn" id="runBtn">달리기<br/>파이어</div>
                <div class="jump-btn" id="jumpBtn">점프</div>
            </div>
        </div>

        <div class="game-over" id="gameOver">
            <h2>게임 오버!</h2>
            <p>최종 점수: <span id="finalScore">0</span></p>
            <p>수집한 코인: <span id="finalCoins">0</span></p>
            <button class="start-btn" onclick="restartGame()">다시 도전</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        
        // 캔버스 크기 설정
        function resizeCanvas() {
            const container = document.querySelector('.game-container');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight * 0.72;
        }
        
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);
        
        let gameRunning = false;
        let score = 0;
        let lives = 3;
        let coins = 0;
        let camera = { x: 0, y: 0 };

        const player = {
            x: 100,
            y: 300,
            width: 35,
            height: 45,
            velocityX: 0,
            velocityY: 0,
            speed: 3.5,
            runSpeed: 6,
            jumpPower: 14,
            onGround: false,
            direction: 1,
            powerLevel: 0,
            invulnerable: false,
            invulnerableTime: 0,
            running: false,
            animFrame: 0
        };

        const gravity = 0.7;
        const friction = 0.85;

        // 모바일에 맞게 스케일 조정
        const scale = Math.min(canvas.width / 1000, 1);
        
        const platforms = [
            { x: 0, y: canvas.height - 80, width: 2500, height: 80, color: '#8B4513' },
            { x: 300, y: canvas.height - 200, width: 120, height: 20, color: '#228B22' },
            { x: 550, y: canvas.height - 280, width: 150, height: 20, color: '#228B22' },
            { x: 850, y: canvas.height - 220, width: 120, height: 20, color: '#228B22' },
            { x: 1150, y: canvas.height - 300, width: 150, height: 20, color: '#228B22' },
            { x: 1450, y: canvas.height - 180, width: 120, height: 20, color: '#228B22' },
            { x: 1750, y: canvas.height - 260, width: 150, height: 20, color: '#228B22' },
            // 파이프들
            { x: 500, y: canvas.height - 160, width: 50, height: 80, color: '#228B22', type: 'pipe' },
            { x: 1200, y: canvas.height - 160, width: 50, height: 80, color: '#228B22', type: 'pipe' },
            { x: 1800, y: canvas.height - 160, width: 50, height: 80, color: '#228B22', type: 'pipe' }
        ];

        const enemies = [
            { x: 400, y: canvas.height - 120, width: 25, height: 25, velocityX: -1, color: '#8B4513', alive: true, type: 'goomba' },
            { x: 600, y: canvas.height - 320, width: 25, height: 25, velocityX: -1.2, color: '#8B4513', alive: true, type: 'goomba' },
            { x: 900, y: canvas.height - 260, width: 25, height: 25, velocityX: 1.5, color: '#8B4513', alive: true, type: 'goomba' },
            { x: 1300, y: canvas.height - 120, width: 30, height: 30, velocityX: -1.8, color: '#228B22', alive: true, type: 'koopa' },
            { x: 1600, y: canvas.height - 220, width: 25, height: 25, velocityX: -1, color: '#8B4513', alive: true, type: 'goomba' }
        ];

        const coinsData = [
            { x: 350, y: canvas.height - 240, width: 18, height: 18, collected: false, floating: true },
            { x: 600, y: canvas.height - 320, width: 18, height: 18, collected: false, floating: true },
            { x: 900, y: canvas.height - 260, width: 18, height: 18, collected: false, floating: true },
            { x: 1200, y: canvas.height - 340, width: 18, height: 18, collected: false, floating: true },
            { x: 1500, y: canvas.height - 220, width: 18, height: 18, collected: false, floating: true },
            { x: 1800, y: canvas.height - 300, width: 18, height: 18, collected: false, floating: true }
        ];

        const powerUps = [
            { x: 450, y: canvas.height - 200, width: 25, height: 25, type: 'mushroom', collected: false },
            { x: 950, y: canvas.height - 280, width: 25, height: 25, type: 'fireflower', collected: false },
            { x: 1550, y: canvas.height - 200, width: 25, height: 25, type: 'mushroom', collected: false }
        ];

        const fireballs = [];
        const particles = [];

        // 터치 컨트롤
        const controls = {
            left: false,
            right: false,
            jump: false,
            run: false
        };

        // 터치 이벤트 핸들러
        function setupTouchControls() {
            const leftBtn = document.getElementById('leftBtn');
            const rightBtn = document.getElementById('rightBtn');
            const jumpBtn = document.getElementById('jumpBtn');
            const runBtn = document.getElementById('runBtn');

            // 왼쪽 버튼
            leftBtn.addEventListener('touchstart', (e) => {
                e.preventDefault();
                controls.left = true;
                leftBtn.style.background = 'linear-gradient(145deg, #2a2a2a, #4a4a4a)';
            });
            leftBtn.addEventListener('touchend', (e) => {
                e.preventDefault();
                controls.left = false;
                leftBtn.style.background = 'linear-gradient(145deg, #4a4a4a, #2a2a2a)';
            });

            // 오른쪽 버튼
            rightBtn.addEventListener('touchstart', (e) => {
                e.preventDefault();
                controls.right = true;
                rightBtn.style.background = 'linear-gradient(145deg, #2a2a2a, #4a4a4a)';
            });
            rightBtn.addEventListener('touchend', (e) => {
                e.preventDefault();
                controls.right = false;
                rightBtn.style.background = 'linear-gradient(145deg, #4a4a4a, #2a2a2a)';
            });

            // 점프 버튼
            jumpBtn.addEventListener('touchstart', (e) => {
                e.preventDefault();
                controls.jump = true;
            });
            jumpBtn.addEventListener('touchend', (e) => {
                e.preventDefault();
                controls.jump = false;
            });

            // 달리기/파이어 버튼
            runBtn.addEventListener('touchstart', (e) => {
                e.preventDefault();
                controls.run = true;
            });
            runBtn.addEventListener('touchend', (e) => {
                e.preventDefault();
                controls.run = false;
            });

            // 마우스 이벤트도 추가 (데스크톱 테스트용)
            leftBtn.addEventListener('mousedown', () => controls.left = true);
            leftBtn.addEventListener('mouseup', () => controls.left = false);
            rightBtn.addEventListener('mousedown', () => controls.right = true);
            rightBtn.addEventListener('mouseup', () => controls.right = false);
            jumpBtn.addEventListener('mousedown', () => controls.jump = true);
            jumpBtn.addEventListener('mouseup', () => controls.jump = false);
            runBtn.addEventListener('mousedown', () => controls.run = true);
            runBtn.addEventListener('mouseup', () => controls.run = false);
        }

        function drawBackground() {
            const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
            gradient.addColorStop(0, '#5C94FC');
            gradient.addColorStop(0.4, '#87CEEB');
            gradient.addColorStop(1, '#32CD32');
            ctx.fillStyle = gradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // 구름들 (모바일에 맞게 조정)
            const clouds = [
                { x: 200, y: 60 }, { x: 500, y: 40 }, { x: 800, y: 80 }, 
                { x: 1100, y: 50 }, { x: 1400, y: 70 }
            ];
            
            ctx.fillStyle = '#FFF';
            clouds.forEach(cloud => {
                const x = cloud.x - camera.x;
                if (x > -100 && x < canvas.width + 100) {
                    ctx.beginPath();
                    ctx.arc(x, cloud.y, 20, 0, Math.PI * 2);
                    ctx.arc(x + 15, cloud.y, 25, 0, Math.PI * 2);
                    ctx.arc(x + 35, cloud.y, 20, 0, Math.PI * 2);
                    ctx.arc(x + 20, cloud.y - 12, 15, 0, Math.PI * 2);
                    ctx.fill();
                }
            });
        }

        function drawPlayer() {
            ctx.save();
            
            if (player.invulnerable && Math.floor(player.invulnerableTime / 5) % 2) {
                ctx.globalAlpha = 0.5;
            }
            
            const x = player.x - camera.x;
            const y = player.y - camera.y;
            
            drawOrbangCharacter(x, y);
            
            ctx.restore();
        }

        function drawOrbangCharacter(x, y) {
            const frameOffset = Math.floor(player.animFrame / 8) % 2;
            const isMoving = Math.abs(player.velocityX) > 0.1;
            
            let sizeMultiplier = 1;
            if (player.powerLevel >= 1) sizeMultiplier = 1.2;
            
            const w = player.width * sizeMultiplier;
            const h = player.height * sizeMultiplier;
            
            // 몸체 (파란 작업복)
            ctx.fillStyle = player.powerLevel === 2 ? '#FF4500' : '#4169E1';
            ctx.beginPath();
            ctx.roundRect(x + 3, y + 18, w - 6, h - 20, 6);
            ctx.fill();
            
            // 머리 (회색)
            ctx.fillStyle = '#C0C0C0';
            ctx.beginPath();
            ctx.arc(x + w/2, y + 13, 12 * sizeMultiplier, 0, Math.PI * 2);
            ctx.fill();
            
            // 모자 (주황색)
            ctx.fillStyle = player.powerLevel === 2 ? '#FF0000' : '#FF8C42';
            ctx.beginPath();
            ctx.ellipse(x + w/2, y + 6, 14 * sizeMultiplier, 8 * sizeMultiplier, 0, Math.PI, 0);
            ctx.fill();
            
            // 모자 로고
            ctx.fillStyle = '#FFF';
            ctx.font = `${Math.floor(10 * sizeMultiplier)}px Arial`;
            ctx.textAlign = 'center';
            ctx.fillText('O', x + w/2, y + 10);
            
            // 얼굴
            ctx.fillStyle = '#000';
            ctx.beginPath();
            ctx.arc(x + w/2 - 4, y + 12, 1.5, 0, Math.PI * 2);
            ctx.arc(x + w/2 + 4, y + 12, 1.5, 0, Math.PI * 2);
            ctx.fill();
            
            // 콧수염
            ctx.fillStyle = '#8B4513';
            ctx.beginPath();
            ctx.ellipse(x + w/2, y + 17, 6, 2, 0, 0, Math.PI * 2);
            ctx.fill();
            
            // 팔
            ctx.fillStyle = '#C0C0C0';
            const armOffset = isMoving ? frameOffset * 4 - 2 : 0;
            ctx.beginPath();
            ctx.arc(x + 6 + armOffset, y + 25, 4, 0, Math.PI * 2);
            ctx.arc(x + w - 6 - armOffset, y + 25, 4, 0, Math.PI * 2);
            ctx.fill();
            
            // 다리
            if (player.onGround && isMoving) {
                const legOffset = frameOffset * 6 - 3;
                ctx.beginPath();
                ctx.arc(x + w/2 - 6 + legOffset, y + h - 4, 4, 0, Math.PI * 2);
                ctx.arc(x + w/2 + 6 - legOffset, y + h - 4, 4, 0, Math.PI * 2);
                ctx.fill();
            } else {
                ctx.beginPath();
                ctx.arc(x + w/2 - 6, y + h - 4, 4, 0, Math.PI * 2);
                ctx.arc(x + w/2 + 6, y + h - 4, 4, 0, Math.PI * 2);
                ctx.fill();
            }
            
            if (isMoving) player.animFrame++;
        }

        function drawPlatforms() {
            platforms.forEach(platform => {
                const x = platform.x - camera.x;
                const y = platform.y - camera.y;
                
                if (x > -platform.width && x < canvas.width + platform.width) {
                    ctx.fillStyle = 'rgba(0,0,0,0.2)';
                    ctx.fillRect(x + 2, y + 2, platform.width, platform.height);
                    
                    if (platform.type === 'pipe') {
                        ctx.fillStyle = platform.color;
                        ctx.fillRect(x, y, platform.width, platform.height);
                        ctx.fillStyle = '#32CD32';
                        ctx.fillRect(x - 4, y, platform.width + 8, 12);
                        ctx.fillStyle = 'rgba(255,255,255,0.3)';
                        ctx.fillRect(x + 4, y + 4, 8, platform.height - 8);
                    } else {
                        ctx.fillStyle = platform.color;
                        ctx.fillRect(x, y, platform.width, platform.height);
                        ctx.fillStyle = 'rgba(255,255,255,0.3)';
                        ctx.fillRect(x, y, platform.width, 4);
                    }
                }
            });
        }

        function drawEnemies() {
            enemies.forEach(enemy => {
                if (!enemy.alive) return;
                
                const x = enemy.x - camera.x;
                const y = enemy.y - camera.y;
                
                if (x > -enemy.width && x < canvas.width + enemy.width) {
                    ctx.fillStyle = 'rgba(0,0,0,0.3)';
                    ctx.ellipse(x + enemy.width/2, y + enemy.height + 2, 
                               enemy.width/2, 4, 0, 0, Math.PI * 2);
                    ctx.fill();
                    
                    if (enemy.type === 'goomba') {
                        ctx.fillStyle = enemy.color;
                        ctx.beginPath();
                        ctx.roundRect(x, y, enemy.width, enemy.height, 4);
                        ctx.fill();
                        
                        ctx.fillStyle = '#000';
                        ctx.beginPath();
                        ctx.arc(x + enemy.width/2 - 4, y + 8, 1.5, 0, Math.PI * 2);
                        ctx.arc(x + enemy.width/2 + 4, y + 8, 1.5, 0, Math.PI * 2);
                        ctx.fill();
                    } else if (enemy.type === 'koopa') {
                        ctx.fillStyle = enemy.color;
                        ctx.beginPath();
                        ctx.arc(x + enemy.width/2, y + enemy.height/2, enemy.width/2, 0, Math.PI * 2);
                        ctx.fill();
                        
                        ctx.strokeStyle = '#228B22';
                        ctx.lineWidth = 2;
                        ctx.beginPath();
                        ctx.arc(x + enemy.width/2, y + enemy.height/2, enemy.width/2 - 4, 0, Math.PI * 2);
                        ctx.stroke();
                    }
                }
            });
        }

        function drawCoins() {
            coinsData.forEach(coin => {
                if (coin.collected) return;
                
                const time = Date.now() * 0.005;
                const floatOffset = coin.floating ? Math.sin(time + coin.x * 0.01) * 4 : 0;
                const x = coin.x - camera.x;
                const y = coin.y - camera.y + floatOffset;
                
                if (x > -coin.width && x < canvas.width + coin.width) {
                    const rotation = time;
                    
                    ctx.save();
                    ctx.translate(x + coin.width/2, y + coin.height/2);
                    ctx.rotate(rotation);
                    
                    ctx.fillStyle = 'rgba(0,0,0,0.3)';
                    ctx.beginPath();
                    ctx.arc(1, 1, 9, 0, Math.PI * 2);
                    ctx.arc(1, 1, 4, 0, Math.PI * 2);
                    ctx.fill('evenodd');
                    
                    ctx.fillStyle = '#FFD700';
                    ctx.beginPath();
                    ctx.arc(0, 0, 9, 0, Math.PI * 2);
                    ctx.arc(0, 0, 4, 0, Math.PI * 2);
                    ctx.fill('evenodd');
                    
                    ctx.fillStyle = '#FFA500';
                    ctx.font = '10px Arial';
                    ctx.textAlign = 'center';
                    ctx.fillText('C', 0, 3);
                    
                    ctx.restore();
                }
            });
        }

        function drawPowerUps() {
            powerUps.forEach(powerUp => {
                if (powerUp.collected) return;
                
                const x = powerUp.x - camera.x;
                const y = powerUp.y - camera.y;
                
                if (x > -powerUp.width && x < canvas.width + powerUp.width) {
                    if (powerUp.type === 'mushroom') {
                        ctx.fillStyle = '#FF0000';
                        ctx.beginPath();
                        ctx.arc(x + powerUp.width/2, y + 10, 10, Math.PI, 0);
                        ctx.fill();
                        
                        ctx.fillStyle = '#FFFACD';
                        ctx.fillRect(x + 8, y + 10, 8, 12);
                        
                        ctx.fillStyle = '#FFF';
                        ctx.beginPath();
                        ctx.arc(x + 6, y + 6, 2, 0, Math.PI * 2);
                        ctx.arc(x + 18, y + 8, 1.5, 0, Math.PI * 2);
                        ctx.fill();
                    } else if (powerUp.type === 'fireflower') {
                        ctx.fillStyle = '#FF4500';
                        ctx.beginPath();
                        for (let i = 0; i < 8; i++) {
                            const angle = (i * Math.PI * 2) / 8;
                            const px = x + 12 + Math.cos(angle) * 8;
                            const py = y + 12 + Math.sin(angle) * 8;
                            ctx.arc(px, py, 3, 0, Math.PI * 2);
                        }
                        ctx.fill();
                        
                        ctx.fillStyle = '#FFD700';
                        ctx.beginPath();
                        ctx.arc(x + 12, y + 12, 4, 0, Math.PI * 2);
                        ctx.fill();
                    }
                }
            });
        }

        function drawFireballs() {
            for (let i = fireballs.length - 1; i >= 0; i--) {
                const fireball = fireballs[i];
                const x = fireball.x - camera.x;
                const y = fireball.y - camera.y;
                
                if (x > -20 && x < canvas.width + 20) {
                    ctx.fillStyle = 'rgba(255, 165, 0, 0.6)';
                    ctx.beginPath();
                    ctx.arc(x - fireball.velocityX * 2, y, 6, 0, Math.PI * 2);
                    ctx.fill();
                    
                    ctx.fillStyle = '#FF4500';
                    ctx.beginPath();
                    ctx.arc(x, y, 4, 0, Math.PI * 2);
                    ctx.fill();
                    
                    ctx.fillStyle = '#FFD700';
                    ctx.beginPath();
                    ctx.arc(x, y, 2, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                fireball.x += fireball.velocityX;
                fireball.y += fireball.velocityY;
                fireball.velocityY += 0.3;
                fireball.life--;
                
                if (fireball.life <= 0 || fireball.x < camera.x - 100 || fireball.x > camera.x + canvas.width + 100) {
                    fireballs.splice(i, 1);
                }
            }
        }

        function drawParticles() {
            for (let i = particles.length - 1; i >= 0; i--) {
                const particle = particles[i];
                const x = particle.x - camera.x;
                const y = particle.y - camera.y;
                
                if (x > -10 && x < canvas.width + 10) {
                    ctx.globalAlpha = particle.life / particle.maxLife;
                    ctx.fillStyle = particle.color;
                    ctx.beginPath();
                    ctx.arc(x, y, particle.size, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.globalAlpha = 1;
                }
                
                particle.x += particle.velocityX;
                particle.y += particle.velocityY;
                particle.velocityY += 0.2;
                particle.life--;
                
                if (particle.life <= 0) {
                    particles.splice(i, 1);
                }
            }
        }

        function updatePlayer() {
            const currentSpeed = (controls.run && player.running) ? player.runSpeed : player.speed;
            
            if (controls.left) {
                player.velocityX = -currentSpeed;
                player.direction = -1;
            } else if (controls.right) {
                player.velocityX = currentSpeed;
                player.direction = 1;
            } else {
                player.velocityX *= friction;
            }

            player.running = controls.run;

            if (controls.jump && player.onGround) {
                player.velocityY = -player.jumpPower;
                player.onGround = false;
            }

            if (controls.run && player.powerLevel === 2 && Math.random() < 0.08) {
                fireballs.push({
                    x: player.x + (player.direction > 0 ? player.width : 0),
                    y: player.y + player.height/2,
                    velocityX: player.direction * 6,
                    velocityY: -1.5,
                    life: 100
                });
            }

            player.velocityY += gravity;
            player.x += player.velocityX;
            player.y += player.velocityY;

            if (player.invulnerable) {
                player.invulnerableTime--;
                if (player.invulnerableTime <= 0) {
                    player.invulnerable = false;
                }
            }

            // 플랫폼 충돌
            player.onGround = false;
            platforms.forEach(platform => {
                if (player.x < platform.x + platform.width &&
                    player.x + player.width > platform.x &&
                    player.y < platform.y + platform.height &&
                    player.y + player.height > platform.y) {
                    
                    if (player.velocityY > 0 && player.y < platform.y) {
                        player.y = platform.y - player.height;
                        player.velocityY = 0;
                        player.onGround = true;
                    }
                }
            });

            if (player.x < 0) player.x = 0;
            if (player.y > canvas.height + 100) {
                takeDamage();
            }
        }

        function updateEnemies() {
            enemies.forEach(enemy => {
                if (!enemy.alive) return;
                
                enemy.x += enemy.velocityX;
                
                let onPlatform = false;
                platforms.forEach(platform => {
                    if (enemy.x + enemy.width > platform.x &&
                        enemy.x < platform.x + platform.width &&
                        enemy.y + enemy.height >= platform.y &&
                        enemy.y + enemy.height <= platform.y + platform.height + 10) {
                        onPlatform = true;
                    }
                });
                
                if (!onPlatform || enemy.x <= 0 || enemy.x >= 2500) {
                    enemy.velocityX *= -1;
                }
            });
        }

        function checkCollisions() {
            // 적과의 충돌
            enemies.forEach(enemy => {
                if (!enemy.alive || player.invulnerable) return;
                
                if (player.x < enemy.x + enemy.width &&
                    player.x + player.width > enemy.x &&
                    player.y < enemy.y + enemy.height &&
                    player.y + player.height > enemy.y) {
                    
                    if (player.velocityY > 0 && player.y < enemy.y - 5) {
                        enemy.alive = false;
                        player.velocityY = -6;
                        score += enemy.type === 'koopa' ? 200 : 100;
                        
                        for (let i = 0; i < 6; i++) {
                            particles.push({
                                x: enemy.x + enemy.width/2,
                                y: enemy.y + enemy.height/2,
                                velocityX: (Math.random() - 0.5) * 4,
                                velocityY: (Math.random() - 0.5) * 4,
                                size: 2,
                                life: 25,
                                maxLife: 25,
                                color: enemy.color
                            });
                        }
                    } else {
                        takeDamage();
                    }
                }
            });

            // 파이어볼과 적 충돌
            fireballs.forEach((fireball, fireIndex) => {
                enemies.forEach((enemy, enemyIndex) => {
                    if (!enemy.alive) return;
                    
                    if (fireball.x < enemy.x + enemy.width &&
                        fireball.x + 8 > enemy.x &&
                        fireball.y < enemy.y + enemy.height &&
                        fireball.y + 8 > enemy.y) {
                        
                        enemy.alive = false;
                        fireballs.splice(fireIndex, 1);
                        score += 200;
                        
                        for (let i = 0; i < 8; i++) {
                            particles.push({
                                x: enemy.x + enemy.width/2,
                                y: enemy.y + enemy.height/2,
                                velocityX: (Math.random() - 0.5) * 6,
                                velocityY: (Math.random() - 0.5) * 6,
                                size: 3,
                                life: 20,
                                maxLife: 20,
                                color: '#FF4500'
                            });
                        }
                    }
                });
            });

            // 코인과의 충돌
            coinsData.forEach(coin => {
                if (coin.collected) return;
                
                if (player.x < coin.x + coin.width &&
                    player.x + player.width > coin.x &&
                    player.y < coin.y + coin.height &&
                    player.y + player.height > coin.y) {
                    
                    coin.collected = true;
                    coins++;
                    score += 200;
                    
                    for (let i = 0; i < 4; i++) {
                        particles.push({
                            x: coin.x + coin.width/2,
                            y: coin.y + coin.height/2,
                            velocityX: (Math.random() - 0.5) * 3,
                            velocityY: (Math.random() - 0.5) * 3,
                            size: 2,
                            life: 15,
                            maxLife: 15,
                            color: '#FFD700'
                        });
                    }
                }
            });

            // 파워업과의 충돌
            powerUps.forEach(powerUp => {
                if (powerUp.collected) return;
                
                if (player.x < powerUp.x + powerUp.width &&
                    player.x + player.width > powerUp.x &&
                    player.y < powerUp.y + powerUp.height &&
                    player.y + player.height > powerUp.y) {
                    
                    powerUp.collected = true;
                    score += 1000;
                    
                    if (powerUp.type === 'mushroom' && player.powerLevel === 0) {
                        player.powerLevel = 1;
                        updatePowerStatus();
                    } else if (powerUp.type === 'fireflower') {
                        player.powerLevel = 2;
                        updatePowerStatus();
                    }
                    
                    for (let i = 0; i < 10; i++) {
                        particles.push({
                            x: powerUp.x + powerUp.width/2,
                            y: powerUp.y + powerUp.height/2,
                            velocityX: (Math.random() - 0.5) * 4,
                            velocityY: (Math.random() - 0.5) * 4,
                            size: 2,
                            life: 25,
                            maxLife: 25,
                            color: powerUp.type === 'mushroom' ? '#FF0000' : '#FF4500'
                        });
                    }
                }
            });
        }

        function takeDamage() {
            if (player.invulnerable) return;
            
            if (player.powerLevel > 0) {
                player.powerLevel--;
                player.invulnerable = true;
                player.invulnerableTime = 120;
                updatePowerStatus();
            } else {
                lives--;
                if (lives <= 0) {
                    gameOver();
                } else {
                    resetPlayerPosition();
                    player.invulnerable = true;
                    player.invulnerableTime = 180;
                }
            }
            updateUI();
        }

        function updateCamera() {
            camera.x = player.x - canvas.width / 3;
            if (camera.x < 0) camera.x = 0;
            if (camera.x > 2500 - canvas.width) camera.x = 2500 - canvas.width;
        }

        function resetPlayerPosition() {
            player.x = 100;
            player.y = 300;
            player.velocityX = 0;
            player.velocityY = 0;
        }

        function updatePowerStatus() {
            const statusElement = document.getElementById('powerStatus');
            switch(player.powerLevel) {
                case 0:
                    statusElement.textContent = '일반 오르방';
                    break;
                case 1:
                    statusElement.textContent = '슈퍼 오르방';
                    break;
                case 2:
                    statusElement.textContent = '파이어 오르방';
                    break;
            }
        }

        function updateUI() {
            document.getElementById('score').textContent = score;
            document.getElementById('lives').textContent = lives;
            document.getElementById('coins').textContent = coins;
        }

        function gameLoop() {
            if (!gameRunning) return;
            
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            updateCamera();
            drawBackground();
            drawPlatforms();
            drawCoins();
            drawPowerUps();
            updatePlayer();
            updateEnemies();
            drawPlayer();
            drawEnemies();
            drawFireballs();
            drawParticles();
            checkCollisions();
            
            requestAnimationFrame(gameLoop);
        }

        function startGame() {
            gameRunning = true;
            score = 0;
            lives = 3;
            coins = 0;
            
            player.powerLevel = 0;
            player.invulnerable = false;
            
            enemies.forEach(enemy => enemy.alive = true);
            coinsData.forEach(coin => coin.collected = false);
            powerUps.forEach(powerUp => powerUp.collected = false);
            
            fireballs.length = 0;
            particles.length = 0;
            
            resetPlayerPosition();
            updateUI();
            updatePowerStatus();
            
            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('gameOver').style.display = 'none';
            
            gameLoop();
        }

        function gameOver() {
            gameRunning = false;
            document.getElementById('finalScore').textContent = score;
            document.getElementById('finalCoins').textContent = coins;
            document.getElementById('gameOver').style.display = 'block';
        }

        function restartGame() {
            document.getElementById('startScreen').style.display = 'flex';
            startGame();
        }

        // 초기화
        setupTouchControls();
        
        // 화면 회전 감지
        window.addEventListener('orientationchange', () => {
            setTimeout(resizeCanvas, 100);
        });

        // 터치 스크롤 방지
        document.addEventListener('touchmove', (e) => {
            e.preventDefault();
        }, { passive: false });

        // 줌 방지
        document.addEventListener('gesturestart', (e) => {
            e.preventDefault();
        });

        // 초기 화면 그리기
        drawBackground();
        drawPlatforms();
        drawCoins();
        drawPowerUps();
        drawPlayer();
        drawEnemies();
        updatePowerStatus();
    </script>
</body>
</html>