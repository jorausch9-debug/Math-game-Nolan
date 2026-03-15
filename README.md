# Math-game-Nolan
Math cars 1-5
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🚚 VROOM COUNT! 🚒</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@700&amp;display=swap');

        :root {
            --grass: #7CFF6E;
        }

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            padding: 0;
            height: 100vh;
            overflow: hidden;
            background: var(--grass);
            font-family: 'Fredoka', sans-serif;
            touch-action: none;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        #game {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
        }

        #header {
            text-align: center;
            padding: 15px 20px 10px;
            background: rgba(255,255,255,0.3);
            z-index: 10;
        }

        #goal {
            font-size: 110px;
            line-height: 1;
            font-weight: 700;
            color: #FF8C00;
            text-shadow: 
                6px 6px 0 #222,
                -3px -3px 0 #222;
            margin: 0;
            transition: color 0.1s;
        }

        #road-container {
            position: relative;
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }

        #parking-lot {
            position: relative;
            width: 94vw;
            max-width: 920px;
            height: 260px;
            background: #B8B8B8;
            border: 18px solid #333;
            border-radius: 30px;
            box-shadow: 
                0 15px 0 #555,
                inset 0 20px 0 #999,
                inset 0 -20px 0 #777;
            overflow: hidden;
        }

        /* Road stripes */
        #parking-lot::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 0;
            width: 100%;
            height: 12px;
            background: repeating-linear-gradient(
                90deg,
                #FFF 0px,
                #FFF 35px,
                transparent 35px,
                transparent 70px
            );
            transform: translateY(-50%);
            opacity: 0.6;
        }

        .slot {
            position: absolute;
            top: 55px;
            height: 150px;
            border: 12px dashed #00BFFF;
            border-radius: 20px;
            background: rgba(255,255,255,0.15);
            box-shadow: inset 0 0 15px rgba(0,0,0,0.3);
            transition: transform 0.3s;
        }

        .vehicle {
            position: absolute;
            top: 65px;
            width: 170px;
            height: 120px;
            filter: drop-shadow(8px 12px 6px rgba(0,0,0,0.4));
            z-index: 5;
            cursor: pointer;
        }

        .count-pop {
            position: absolute;
            left: 50%;
            top: 35%;
            transform: translateX(-50%);
            font-size: 160px;
            font-weight: 700;
            color: #FF8C00;
            text-shadow: 8px 8px 0 #222;
            pointer-events: none;
            z-index: 20;
            animation: popCount 1.4s forwards;
        }

        @keyframes popCount {
            0%   { transform: translateX(-50%) scale(0.1) rotate(-20deg); opacity: 0; }
            25%  { transform: translateX(-50%) scale(1.35) rotate(15deg); }
            50%  { transform: translateX(-50%) scale(0.95) rotate(-8deg); }
            100% { transform: translateX(-50%) scale(1.1) translateY(-120px); opacity: 0; }
        }

        .confetti {
            position: absolute;
            z-index: 30;
            width: 14px;
            height: 14px;
            border-radius: 3px;
            animation: confettiFall 3.2s linear forwards, confettiTwirl 1.8s linear infinite;
            box-shadow: 0 2px 4px rgba(0,0,0,0.3);
        }

        @keyframes confettiFall {
            to {
                transform: translateY(140vh) rotate(800deg);
            }
        }

        @keyframes confettiTwirl {
            0%   { transform: rotate(0deg); }
            100% { transform: rotate(1080deg); }
        }

        #celebration {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: none;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            z-index: 40;
            pointer-events: none;
        }

        #win-text {
            font-size: 95px;
            color: #FF0;
            text-shadow: 
                0 0 20px #F0F,
                0 0 40px #0FF,
                8px 8px 0 #222;
            margin: 0 0 20px;
            animation: winBounce 0.8s infinite alternate;
        }

        #smiley {
            font-size: 180px;
            filter: drop-shadow(0 10px 20px #000);
            animation: winBounce 0.8s infinite alternate;
        }

        @keyframes winBounce {
            from { transform: scale(0.9) rotate(-8deg); }
            to   { transform: scale(1.08) rotate(8deg); }
        }

        .decor {
            position: absolute;
            z-index: 2;
        }

        .cone {
            width: 42px;
            height: 62px;
        }
        
        .cone-base {
            width: 42px;
            height: 12px;
            background: #222;
            margin: 0 auto;
            border-radius: 3px;
        }
        
        .cone-triangle {
            width: 0;
            height: 0;
            border-left: 21px solid transparent;
            border-right: 21px solid transparent;
            border-bottom: 55px solid #FF6600;
            margin: 0 auto;
        }
    </style>
</head>
<body>
    <div id="game">
        <!-- Header -->
        <div id="header">
            <div id="goal">1</div>
        </div>

        <!-- Road area -->
        <div id="road-container">
            <div id="parking-lot">
                <!-- Slots will be added by JS -->
                <!-- Decorations -->
                <div class="decor" style="bottom:15px; left:5%; transform:rotate(-15deg);">
                    <div class="cone">
                        <div class="cone-triangle"></div>
                        <div class="cone-base"></div>
                    </div>
                </div>
                <div class="decor" style="bottom:15px; right:8%; transform:rotate(18deg);">
                    <div class="cone">
                        <div class="cone-triangle"></div>
                        <div class="cone-base"></div>
                    </div>
                </div>
                <!-- Stop sign -->
                <div class="decor" style="top:12px; right:6%; font-size:42px; filter:drop-shadow(3px 3px 3px #000);">🛑</div>
            </div>
        </div>

        <!-- Celebration overlay -->
        <div id="celebration">
            <div id="win-text">YOU DID IT!</div>
            <div id="smiley">😄</div>
        </div>
    </div>

    <script>
        // ============== SETUP ==============
        let audioCtx;
        let target = 1;
        let current = 0;
        let parkedVehicles = [];
        let celebrating = false;
        let slots = [];
        let vehicleColors = ['#FF2D00', '#FFCC00', '#00D400', '#00AAFF', '#FF00CC'];
        let lastVehicleType = -1;

        const game = document.getElementById('game');
        const goalEl = document.getElementById('goal');
        const parkingLot = document.getElementById('parking-lot');
        const celebrationEl = document.getElementById('celebration');
        const winText = document.getElementById('win-text');

        // ============== AUDIO ==============
        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        function playRumble(type) {
            if (!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            const filter = audioCtx.createBiquadFilter();
            
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(68 + type * 4, audioCtx.currentTime);
            filter.type = 'lowpass';
            filter.frequency.setValueAtTime(280, audioCtx.currentTime);
            gain.gain.value = 0.18;
            
            osc.connect(filter);
            filter.connect(gain);
            gain.connect(audioCtx.destination);
            
            osc.start();
            setTimeout(() => {
                gain.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.6);
                setTimeout(() => osc.stop(), 800);
            }, 400);
        }

        function playHonk(type) {
            if (!audioCtx) return;
            const baseFreq = 380 + type * 70;
            
            // First beep
            const osc1 = audioCtx.createOscillator();
            const gain1 = audioCtx.createGain();
            osc1.type = 'square';
            osc1.frequency.value = baseFreq;
            gain1.gain.value = 0.35;
            osc1.connect(gain1);
            gain1.connect(audioCtx.destination);
            osc1.start();
            setTimeout(() => { gain1.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.08); }, 80);
            setTimeout(() => osc1.stop(), 200);

            // Second beep
            setTimeout(() => {
                const osc2 = audioCtx.createOscillator();
                const gain2 = audioCtx.createGain();
                osc2.type = 'square';
                osc2.frequency.value = baseFreq + 120;
                gain2.gain.value = 0.35;
                osc2.connect(gain2);
                gain2.connect(audioCtx.destination);
                osc2.start();
                setTimeout(() => { gain2.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.08); }, 90);
                setTimeout(() => osc2.stop(), 220);
            }, 180);
        }

        function playSiren() {
            if (!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'sine';
            osc.frequency.setValueAtTime(520, audioCtx.currentTime);
            gain.gain.value = 0.4;
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();

            let time = 0;
            const interval = setInterval(() => {
                if (!audioCtx) return;
                time += 0.08;
                const freq = 520 + Math.sin(time * 12) * 180;
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
            }, 80);

            setTimeout(() => {
                clearInterval(interval);
                gain.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.6);
                setTimeout(() => osc.stop(), 800);
            }, 2100);
        }

        // ============== VEHICLE CREATION ==============
        function createVehicleSVG(type) {
            const ns = "http://www.w3.org/2000/svg";
            const svg = document.createElementNS(ns, "svg");
            svg.setAttribute("width", "170");
            svg.setAttribute("height", "118");
            svg.setAttribute("viewBox", "0 0 170 118");
            svg.style.width = "170px";
            svg.style.height = "118px";

            const color = vehicleColors[type];

            // BODY
            const body = document.createElementNS(ns, "rect");
            body.setAttribute("x", "18");
            body.setAttribute("y", "42");
            body.setAttribute("width", "112");
            body.setAttribute("height", "38");
            body.setAttribute("rx", "9");
            body.setAttribute("fill", color);
            body.setAttribute("stroke", "#222");
            body.setAttribute("stroke-width", "7");
            svg.appendChild(body);

            // CAB
            const cab = document.createElementNS(ns, "rect");
            cab.setAttribute("x", "12");
            cab.setAttribute("y", "28");
            cab.setAttribute("width", "38");
            cab.setAttribute("height", "26");
            cab.setAttribute("rx", "6");
            cab.setAttribute("fill", color);
            cab.setAttribute("stroke", "#222");
            cab.setAttribute("stroke-width", "6");
            svg.appendChild(cab);

            // WINDOW
            const window = document.createElementNS(ns, "rect");
            window.setAttribute("x", "19");
            window.setAttribute("y", "32");
            window.setAttribute("width", "17");
            window.setAttribute("height", "13");
            window.setAttribute("fill", "#A0F0FF");
            window.setAttribute("stroke", "#222");
            window.setAttribute("stroke-width", "3");
            svg.appendChild(window);

            // WHEELS
            const wheel1 = document.createElementNS(ns, "circle");
            wheel1.setAttribute("cx", "42");
            wheel1.setAttribute("cy", "83");
            wheel1.setAttribute("r", "17");
            wheel1.setAttribute("fill", "#111");
            wheel1.setAttribute("stroke", "#333");
            wheel1.setAttribute("stroke-width", "7");
            svg.appendChild(wheel1);

            const wheel2 = document.createElementNS(ns, "circle");
            wheel2.setAttribute("cx", "105");
            wheel2.setAttribute("cy", "83");
            wheel2.setAttribute("r", "17");
            wheel2.setAttribute("fill", "#111");
            wheel2.setAttribute("stroke", "#333");
            wheel2.setAttribute("stroke-width", "7");
            svg.appendChild(wheel2);

            // HUBS
            const hub1 = document.createElementNS(ns, "circle");
            hub1.setAttribute("cx", "42");
            hub1.setAttribute("cy", "83");
            hub1.setAttribute("r", "6");
            hub1.setAttribute("fill", "#EEE");
            svg.appendChild(hub1);
            const hub2 = document.createElementNS(ns, "circle");
            hub2.setAttribute("cx", "105");
            hub2.setAttribute("cy", "83");
            hub2.setAttribute("r", "6");
            hub2.setAttribute("fill", "#EEE");
            svg.appendChild(hub2);

            // HEADLIGHT
            const headlight = document.createElementNS(ns, "circle");
            headlight.setAttribute("cx", "13");
            headlight.setAttribute("cy", "49");
            headlight.setAttribute("r", "9");
            headlight.setAttribute("fill", "#FFEE80");
            headlight.setAttribute("stroke", "#222");
            headlight.setAttribute("stroke-width", "3");
            svg.appendChild(headlight);

            // TYPE SPECIFIC
            if (type === 0) { // FIRE TRUCK
                // Ladder
                const ladder1 = document.createElementNS(ns, "rect");
                ladder1.setAttribute("x", "73");
                ladder1.setAttribute("y", "29");
                ladder1.setAttribute("width", "6");
                ladder1.setAttribute("height", "22");
                ladder1.setAttribute("fill", "#FFF");
                ladder1.setAttribute("stroke", "#222");
                ladder1.setAttribute("stroke-width", "2");
                svg.appendChild(ladder1);
                const ladder2 = document.createElementNS(ns, "rect");
                ladder2.setAttribute("x", "88");
                ladder2.setAttribute("y", "29");
                ladder2.setAttribute("width", "6");
                ladder2.setAttribute("height", "22");
                ladder2.setAttribute("fill", "#FFF");
                ladder2.setAttribute("stroke", "#222");
                ladder2.setAttribute("stroke-width", "2");
                svg.appendChild(ladder2);
            } 
            else if (type === 1) { // DUMP TRUCK
                const dump = document.createElementNS(ns, "rect");
                dump.setAttribute("x", "105");
                dump.setAttribute("y", "32");
                dump.setAttribute("width", "48");
                dump.setAttribute("height", "32");
                dump.setAttribute("fill", color);
                dump.setAttribute("stroke", "#222");
                dump.setAttribute("stroke-width", "6");
                dump.setAttribute("rx", "4");
                svg.appendChild(dump);
            } 
            else if (type === 2) { // CEMENT MIXER
                const drum = document.createElementNS(ns, "ellipse");
                drum.setAttribute("cx", "122");
                drum.setAttribute("cy", "53");
                drum.setAttribute("rx", "29");
                drum.setAttribute("ry", "21");
                drum.setAttribute("fill", "#DDD");
                drum.setAttribute("stroke", "#222");
                drum.setAttribute("stroke-width", "7");
                svg.appendChild(drum);
                // Mixer lines
                const line = document.createElementNS(ns, "rect");
                line.setAttribute("x", "105");
                line.setAttribute("y", "47");
                line.setAttribute("width", "32");
                line.setAttribute("height", "5");
                line.setAttribute("fill", "#222");
                svg.appendChild(line);
            } 
            else if (type === 3) { // MONSTER TRUCK
                wheel1.setAttribute("r", "20");
                wheel2.setAttribute("r", "20");
                wheel1.setAttribute("cy", "85");
                wheel2.setAttribute("cy", "85");
            } 
            else if (type === 4) { // TOW TRUCK
                const hookPath = document.createElementNS(ns, "path");
                hookPath.setAttribute("d", "M 135 65 Q 155 58 162 75");
                hookPath.setAttribute("fill", "none");
                hookPath.setAttribute("stroke", "#222");
                hookPath.setAttribute("stroke-width", "9");
                hookPath.setAttribute("stroke-linecap", "round");
                svg.appendChild(hookPath);
            }

            return svg;
        }

        // ============== SLOT CREATION & POSITIONING ==============
        function createSlots() {
            slots = [];
            for (let i = 0; i < 5; i++) {
                const slot = document.createElement('div');
                slot.className = 'slot';
                parkingLot.appendChild(slot);
                slots.push(slot);
            }
        }

        function positionSlots() {
            const pw = parkingLot.clientWidth;
            const margin = pw * 0.04;
            const available = pw - margin * 2;
            const gap = 22;
            const slotW = Math.max(130, (available - gap * 4) / 5);

            for (let i = 0; i < 5; i++) {
                const left = margin + i * (slotW + gap);
                slots[i].style.left = `${left}px`;
                slots[i].style.width = `${slotW}px`;
                slots[i].style.height = '150px';
                slots[i].style.top = '52px';
            }
        }

        // ============== ADD VEHICLE ==============
        function addVehicle() {
            if (current >= target || celebrating) return;

            const type = (Math.floor(Math.random() * 5) + lastVehicleType + 1) % 5;
            lastVehicleType = type;

            const vehicle = document.createElement('div');
            vehicle.className = 'vehicle';
            const svg = createVehicleSVG(type);
            vehicle.appendChild(svg);

            parkingLot.appendChild(vehicle);
            parkedVehicles.push(vehicle);

            // Start position
            const slotIndex = current;
            const slot = slots[slotIndex];
            const targetX = slot.offsetLeft + (slot.offsetWidth / 2) - 85;

            vehicle.style.left = '-240px';
            vehicle.style.transform = 'scale(0.65) rotate(-18deg)';
            vehicle.style.transition = 'none';

            // Trigger animation
            requestAnimationFrame(() => {
                vehicle.style.transition = 'left 1150ms cubic-bezier(0.34, 1.56, 0.64, 1), transform 1150ms cubic-bezier(0.34, 1.56, 0.64, 1)';
                vehicle.style.left = `${targetX}px`;
                vehicle.style.transform = 'scale(1) rotate(0deg)';
            });

            // Sounds
            initAudio();
            playRumble(type);
            setTimeout(() => playHonk(type), 620);

            // Pop count
            showCountPop();

            current++;

            // Check win
            if (current === target) {
                setTimeout(() => {
                    celebrate();
                }, 900);
            }
        }

        function showCountPop() {
            const pop = document.createElement('div');
            pop.className = 'count-pop';
            pop.textContent = current;
            parkingLot.appendChild(pop);

            setTimeout(() => {
                pop.remove();
            }, 1600);
        }

        // ============== CELEBRATION ==============
        function celebrate() {
            celebrating = true;
            celebrationEl.style.display = 'flex';

            // Rainbow goal
            let hue = 0;
            const rainbowInterval = setInterval(() => {
                hue = (hue + 18) % 360;
                goalEl.style.color = `hsl(${hue}, 100%, 55%)`;
            }, 70);

            // Confetti
            launchConfetti();

            // Siren + group honks
            playSiren();
            setTimeout(() => playHonk(0), 300);
            setTimeout(() => playHonk(2), 550);
            setTimeout(() => playHonk(4), 820);

            // Stop rainbow after 5 seconds
            setTimeout(() => {
                clearInterval(rainbowInterval);
                goalEl.style.color = '#FF8C00';
                endCelebration();
            }, 5200);
        }

        function launchConfetti() {
            const colors = ['#FF0088', '#00FFAA', '#FFCC00', '#00AAFF', '#FF6600', '#AA00FF'];
            for (let i = 0; i < 42; i++) {
                const c = document.createElement('div');
                c.className = 'confetti';
                c.style.left = `${Math.random() * 100}vw`;
                c.style.background = colors[Math.floor(Math.random() * colors.length)];
                c.style.width = `${8 + Math.random() * 14}px`;
                c.style.height = `${8 + Math.random() * 14}px`;
                c.style.opacity = 0.9;
                c.style.animationDuration = `${2.4 + Math.random() * 2.1}s`;
                document.body.appendChild(c);

                setTimeout(() => c.remove(), 5200);
            }
        }

        function endCelebration() {
            celebrationEl.style.display = 'none';
            
            // Drive all vehicles off
            parkedVehicles.forEach((v, i) => {
                v.style.transition = 'left 900ms cubic-bezier(0.4, 0, 1, 1), transform 900ms';
                v.style.left = `${parseFloat(v.style.left) + 420 + i * 30}px`;
                v.style.transform = 'scale(0.9) rotate(12deg)';
            });

            setTimeout(() => {
                // Clean up
                parkedVehicles.forEach(v => {
                    if (v.parentNode) v.parentNode.removeChild(v);
                });
                parkedVehicles = [];
                current = 0;

                // Next round
                target = target === 5 ? 1 : target + 1;
                goalEl.textContent = target;
                celebrating = false;

                // Slight stagger on new slots
                slots.forEach((s, i) => {
                    s.style.top = `${50 + Math.sin(i) * 9}px`;
                });
            }, 1100);
        }

        // ============== TAP HANDLER ==============
        function handleTap(e) {
            // Prevent zoom / scroll
            e.preventDefault();

            if (celebrating) return;

            initAudio(); // first tap unlocks audio

            // Add vehicle
            addVehicle();
        }

        // ============== INIT ==============
        function initGame() {
            createSlots();
            positionSlots();

            // Goal
            goalEl.textContent = target;

            // Resize handler
            window.addEventListener('resize', () => {
                positionSlots();
            });

            // Tap anywhere
            document.addEventListener('pointerdown', handleTap);

            // Keyboard support for testing
            document.addEventListener('keydown', (e) => {
                if (e.key === ' ' || e.key === 'Enter') {
                    handleTap(e);
                }
            });

            // Welcome tap prompt effect (subtle)
            setTimeout(() => {
                const prompt = document.createElement('div');
                prompt.style.position = 'absolute';
                prompt.style.bottom = '80px';
                prompt.style.left = '50%';
                prompt.style.transform = 'translateX(-50%)';
                prompt.style.fontSize = '22px';
                prompt.style.color = '#FFF';
                prompt.style.textShadow = '3px 3px 0 #222';
                prompt.style.opacity = '0.6';
                prompt.style.pointerEvents = 'none';
                prompt.textContent = 'TAP ANYWHERE!';
                document.body.appendChild(prompt);
                
                setTimeout(() => {
                    prompt.style.transition = 'opacity 1.8s';
                    prompt.style.opacity = '0';
                    setTimeout(() => prompt.remove(), 2000);
                }, 2200);
            }, 800);
        }

        // Start the game
        window.onload = initGame;
    </script>
</body>
</html>
git branch -M main
git push -u origin main