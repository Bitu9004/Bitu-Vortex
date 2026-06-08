
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BITU_VORTEX // COMMAND NETWORK</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --bg-dark: #030307;
            --bg-card: rgba(10, 12, 22, 0.7);
            --border-glow-blue: rgba(0, 240, 255, 0.25);
            --border-glow-red: rgba(255, 0, 85, 0.35);
            --neon-blue: #00f0ff;
            --neon-red: #ff0055;
            --text-main: #a3b8cc;
            --text-bright: #ffffff;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
        }

        body, html {
            width: 100%;
            height: 100%;
            background-color: var(--bg-dark);
            font-family: 'Share Tech Mono', monospace;
            color: var(--text-main);
            overflow: hidden;
        }

        #particleCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            pointer-events: none;
        }

        .scanlines {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.3) 50%);
            background-size: 100% 4px;
            z-index: 999;
            pointer-events: none;
        }

        .scanlines::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, rgba(255,255,255,0), rgba(0, 240, 255, 0.04) 10%, rgba(255,255,255,0) 20%);
            animation: crtRefresh 6s linear infinite;
        }

        #landingScreen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 10;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.8s cubic-bezier(0.4, 0, 0.2, 1), transform 0.8s ease;
        }

        .ring-container {
            position: relative;
            width: 320px;
            height: 320px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 40px;
        }

        .energy-ring {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            border: 2px dashed transparent;
        }
        .energy-ring.outer {
            border-top-color: var(--neon-blue);
            border-bottom-color: var(--neon-blue);
            filter: drop-shadow(0 0 15px var(--neon-blue));
            animation: rotateClockwise 15s linear infinite;
        }
        .energy-ring.inner {
            width: 85%;
            height: 85%;
            border-left-color: var(--neon-red);
            border-right-color: var(--neon-red);
            filter: drop-shadow(0 0 10px var(--neon-red));
            animation: rotateCounter 8s linear infinite;
        }

        .huge-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.8rem;
            font-weight: 900;
            color: var(--text-bright);
            text-align: center;
            letter-spacing: 4px;
            z-index: 2;
            text-shadow: 0 0 20px rgba(0, 240, 255, 0.6);
            position: relative;
        }

        .glitch-hover:hover {
            animation: textGlitch 0.3s linear infinite;
            color: var(--neon-blue);
            text-shadow: 2px -2px var(--neon-red), -2px 2px #fff;
        }

        .btn-box {
            display: flex;
            flex-direction: column;
            gap: 15px;
            z-index: 2;
            align-items: center;
        }

        .enter-btn {
            font-family: 'Orbitron', sans-serif;
            background: rgba(2, 5, 12, 0.8);
            color: var(--neon-blue);
            border: 1px solid var(--neon-blue);
            padding: 15px 40px;
            font-size: 1.1rem;
            font-weight: 700;
            letter-spacing: 3px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            border-radius: 4px;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.15);
            transition: all 0.3s ease;
            touch-action: manipulation;
        }

        .enter-btn:hover {
            color: var(--text-bright);
            background: var(--neon-blue);
            box-shadow: 0 0 35px var(--neon-blue), inset 0 0 15px rgba(255,255,255,0.5);
            text-shadow: 0 0 5px #000;
        }

        #enterSystemBtn {
            display: none;
            border-color: var(--neon-red);
            color: var(--neon-red);
            box-shadow: 0 0 15px rgba(255, 0, 85, 0.15);
        }

        #enterSystemBtn:hover {
            background: var(--neon-red);
            box-shadow: 0 0 35px var(--neon-red), inset 0 0 15px rgba(255,255,255,0.5);
        }

        #scannerBar {
            position: fixed;
            top: -10px;
            left: 0;
            width: 100%;
            height: 8px;
            background: linear-gradient(to bottom, transparent, var(--neon-blue), transparent);
            box-shadow: 0 0 25px var(--neon-blue);
            z-index: 1000;
            display: none;
        }

        #dashboardScreen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 5;
            opacity: 0;
            visibility: hidden;
            overflow-y: auto;
            padding: 30px 20px;
            display: flex;
            justify-content: center;
            transition: opacity 1s ease;
        }

        .dashboard-container {
            width: 100%;
            max-width: 800px;
            display: flex;
            flex-direction: column;
            gap: 25px;
            padding-bottom: 60px;
        }

        .profile-card {
            background: var(--bg-card);
            border: 1px solid var(--border-glow-blue);
            backdrop-filter: blur(14px);
            -webkit-backdrop-filter: blur(14px);
            border-radius: 8px;
            padding: 25px;
            position: relative;
            overflow: hidden;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
            cursor: pointer;
            touch-action: manipulation;
        }

        .profile-card::after {
            content: '';
            position: absolute;
            top: 5px;
            right: 5px;
            width: 15px;
            height: 15px;
            border-top: 2px solid var(--neon-red);
            border-right: 2px solid var(--neon-red);
            animation: pulseGlow 2s infinite alternate;
        }

        .profile-card::before {
            content: '';
            position: absolute;
            bottom: 5px;
            left: 5px;
            width: 15px;
            height: 15px;
            border-bottom: 2px solid var(--neon-blue);
            border-left: 2px solid var(--neon-blue);
            animation: pulseGlow 2s infinite alternate-reverse;
        }

        .node-status {
            font-size: 0.75rem;
            color: var(--neon-blue);
            letter-spacing: 2px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .status-dot {
            width: 6px;
            height: 6px;
            background-color: #10b981;
            border-radius: 50%;
            box-shadow: 0 0 8px #10b981;
            animation: pulseGlow 1s infinite alternate;
        }

        .profile-name {
            font-family: 'Orbitron', sans-serif;
            font-size: 2.2rem;
            color: var(--text-bright);
            font-weight: 900;
            letter-spacing: 2px;
            margin-bottom: 8px;
        }

        .profile-meta {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            font-size: 0.9rem;
            color: #64748b;
        }

        .profile-meta span {
            color: var(--neon-red);
        }

        .console-output {
            background: rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.03);
            padding: 12px 15px;
            border-radius: 4px;
            font-size: 0.85rem;
            color: #10b981;
            margin-top: 18px;
            min-height: 42px;
            display: flex;
            align-items: center;
        }

        .section-title {
            font-family: 'Orbitron', sans-serif;
            font-size: 0.9rem;
            letter-spacing: 3px;
            color: var(--text-bright);
            margin-bottom: 15px;
            text-transform: uppercase;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255,255,255,0.08);
            padding-bottom: 6px;
        }

        .section-title span {
            font-size: 0.75rem;
            color: var(--neon-red);
            animation: pulseGlow 1.5s infinite alternate;
        }

        .grid-layout {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .info-card {
            background: var(--bg-card);
            border: 1px solid rgba(255, 255, 255, 0.04);
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            padding: 18px;
            border-radius: 6px;
            position: relative;
            transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
            transform: translateY(30px);
            opacity: 0;
            cursor: pointer;
            touch-action: manipulation;
        }

        .info-card.visible {
            transform: translateY(0);
            opacity: 1;
        }

        .info-card:hover {
            border-color: var(--border-glow-red);
            box-shadow: 0 0 25px rgba(255, 0, 85, 0.18), inset 0 0 10px rgba(255, 0, 85, 0.05);
            transform: scale(1.015) translateY(-3px);
        }

        .card-main-text {
            font-size: 1.15rem;
            color: var(--text-bright);
            margin-bottom: 4px;
            font-weight: bold;
            letter-spacing: 1px;
        }

        .card-sub-text {
            font-size: 0.85rem;
            color: var(--neon-blue);
        }

        .progress-container {
            margin-top: 14px;
            width: 100%;
            height: 4px;
            background: rgba(255,255,255,0.05);
            border-radius: 2px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, var(--neon-blue), var(--neon-red));
            box-shadow: 0 0 8px var(--neon-blue);
            transition: width 2.2s cubic-bezier(0.1, 0.8, 0.2, 1);
        }

        @keyframes rotateClockwise { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        @keyframes rotateCounter { from { transform: rotate(360deg); } to { transform: rotate(0deg); } }
        @keyframes pulseGlow { 0% { opacity: 0.3; filter: brightness(0.7); } 100% { opacity: 1; filter: brightness(1.3); } }
        @keyframes crtRefresh { 0% { transform: translateY(-100%); } 100% { transform: translateY(100%); } }
        @keyframes scanTransition { 0% { top: -10px; } 40% { top: 100%; opacity: 1; } 100% { top: 100%; opacity: 0; } }
        @keyframes textGlitch { 0% { transform: translate(0) skew(0deg); } 20% { transform: translate(-2px, 2px) skew(-4deg); } 40% { transform: translate(-2px, -2px) skew(4deg); } 60% { transform: translate(2px, 2px) skew(-2deg); } 80% { transform: translate(2px, -2px) skew(2deg); } 100% { transform: translate(0) skew(0deg); } }

        #dashboardScreen::-webkit-scrollbar { width: 6px; }
        #dashboardScreen::-webkit-scrollbar-track { background: var(--bg-dark); }
        #dashboardScreen::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 3px; }
        #dashboardScreen::-webkit-scrollbar-thumb:hover { background: var(--neon-blue); }

        @media (max-width: 480px) {
            .huge-title { font-size: 2.1rem; }
            .ring-container { width: 260px; height: 260px; }
            .profile-name { font-size: 1.6rem; }
        }
    </style>
</head>
<body>

    <div class="scanlines"></div>
    <canvas id="particleCanvas"></canvas>
    <div id="scannerBar"></div>

    <div id="landingScreen">
        <div class="ring-container">
            <div class="energy-ring outer"></div>
            <div class="energy-ring inner"></div>
            <h1 class="huge-title glitch-hover">BITU_VORTEX</h1>
        </div>
        <div class="btn-box">
            <button class="enter-btn" id="initSystemBtn" onclick="preInitializeVoice()">INITIALIZE PROTOCOL</button>
            <button class="enter-btn" id="enterSystemBtn" onclick="triggerSystemCore()">ENTER SYSTEM</button>
        </div>
    </div>

    <div id="dashboardScreen">
        <div class="dashboard-container">
            
            <div class="profile-card" onclick="speakText('Bitu Vortex, Administrator 01, Server 1')">
                <div class="node-status"><div class="status-dot"></div>// SYSTEM_SECURE // NODE_07_CORE</div>
                <h2 class="profile-name glitch-hover">BITU_VORTEX</h2>
                <div class="profile-meta">
                    <div>RANK: <span>ADMINISTRATOR 01</span></div>
                    <div>CLEARANCE: <span>LEVEL VII</span></div>
                    <div>NODE: <span>SERVER 1</span></div>
                </div>
                <div class="console-output" id="typewriterText"></div>
            </div>

            <div>
                <h3 class="section-title">CURRENT ASSIGNMENTS <span>[ONLINE]</span></h3>
                <div class="grid-layout">
                    <div class="info-card scroll-reveal" onclick="speakText('Administrator 01')">
                        <div class="card-main-text">Administrator 01</div>
                        <div class="card-sub-text">Core Matrix Operations // Server 1</div>
                        <div class="progress-container"><div class="progress-bar" data-width="95%"></div></div>
                    </div>
                    <div class="info-card scroll-reveal" onclick="speakText('Curator Of Government House')">
                        <div class="card-main-text">Curator Of Government House</div>
                        <div class="card-sub-text">Executive Records Oversight // Server 1</div>
                        <div class="progress-container"><div class="progress-bar" data-width="84%"></div></div>
                    </div>
                </div>
            </div>

            <div>
                <h3 class="section-title">HISTORICAL RECORDS ARCHIVE <span>[ENCRYPTED]</span></h3>
                <div class="grid-layout">
                    <div class="info-card scroll-reveal" onclick="speakText('Curator Of Police Department')">
                        <div class="card-main-text">Curator Of Police Department</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                    <div class="info-card scroll-reveal" onclick="speakText('Colonel Of APD times three')">
                        <div class="card-main-text">Colonel Of APD ×3</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                    <div class="info-card scroll-reveal" onclick="speakText('Lieutenant Colonel Of APD times two')">
                        <div class="card-main-text">Lieutenant Colonel Of APD ×2</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                    <div class="info-card scroll-reveal" onclick="speakText('Lieutenant Colonel Of YPD times one')">
                        <div class="card-main-text">Lieutenant Colonel Of YPD ×1</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <script>
        // Interactive Space Particle Dynamics
        const canvas = document.getElementById('particleCanvas');
        const ctx = canvas.getContext('2d');
        let particlesArray = [];
        let mouse = { x: null, y: null, radius: 80 };

        window.addEventListener('mousemove', (e) => { mouse.x = e.x; mouse.y = e.y; });
        window.addEventListener('mouseout', () => { mouse.x = null; mouse.y = null; });
        window.addEventListener('touchmove', (e) => {
            if(e.touches.length > 0) {
                mouse.x = e.touches[0].clientX;
                mouse.y = e.touches[0].clientY;
            }
        });
        window.addEventListener('touchend', () => { mouse.x = null; mouse.y = null; });

        function resizeCanvas() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        class Particle {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.size = Math.random() * 2 + 0.5;
                this.color = Math.random() > 0.4 ? 'rgba(0, 240, 255, 0.4)' : 'rgba(255, 0, 85, 0.35)';
            }
            update() {
                if (mouse.x != null && mouse.y != null) {
                    let dx = mouse.x - this.x;
                    let dy = mouse.y - this.y;
                    let distance = Math.hypot(dx, dy);
                    if (distance < mouse.radius) {
                        let force = (mouse.radius - distance) / mouse.radius;
                        this.x -= (dx / distance) * force * 3;
                        this.y -= (dy / distance) * force * 3;
                    }
                }
                this.x += (Math.random() - 0.5) * 0.3;
                this.y += (Math.random() - 0.5) * 0.3;
            }
            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fillStyle = this.color;
                ctx.fill();
            }
        }

        function initParticles() {
            const count = Math.min(Math.floor(canvas.width / 10), 140);
            for (let i = 0; i < count; i++) particlesArray.push(new Particle());
        }

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < particlesArray.length; i++) {
                particlesArray[i].update();
                particlesArray[i].draw();
            }
            requestAnimationFrame(animateParticles);
        }
        initParticles();
        animateParticles();

        // Voice Engine System
        function speakText(phrase) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel(); 
                const utterance = new SpeechSynthesisUtterance(phrase);
                const voices = window.speechSynthesis.getVoices();
                
                // Optimized voice selectors prioritizing clean mobile speech engines
                const systemVoice = voices.find(voice => 
                    voice.name.includes('Google UK English Female') || 
                    voice.name.includes('Samantha') || 
                    voice.name.includes('Zira') || 
                    voice.lang.startsWith('en')
                );
                
                if (systemVoice) utterance.voice = systemVoice;
                utterance.rate = 1.0;
                utterance.pitch = 0.95;
                window.speechSynthesis.speak(utterance);
            }
        }

        // Landing Screen Step 1: Pre-initialize Audio Context and Say Welcome
        function preInitializeVoice() {
            document.getElementById('initSystemBtn').style.display = 'none';
            const enterBtn = document.getElementById('enterSystemBtn');
            enterBtn.style.display = 'block';
            
            speakText("Welcome. Please click on Enter System to proceed.");
        }

        // Landing Screen Step 2: Transition to Core Network
        function triggerSystemCore() {
            const landing = document.getElementById('landingScreen');
            const scanner = document.getElementById('scannerBar');
            
            scanner.style.display = 'block';
            scanner.style.animation = 'scanTransition 0.8s cubic-bezier(0.2, 0.8, 0.2, 1) forwards';
            
            landing.style.opacity = '0';
            landing.style.transform = 'scale(0.95)';

            setTimeout(() => {
                landing.style.display = 'none';
                launchSecureWorkspace();
            }, 750);
        }

        function launchSecureWorkspace() {
            const dash = document.getElementById('dashboardScreen');
            dash.style.visibility = 'visible';
            dash.style.opacity = '1';

            const cards = document.querySelectorAll('.scroll-reveal');
            cards.forEach((card, index) => {
                setTimeout(() => { card.classList.add('visible'); }, index * 120);
            });

            const progressBars = document.querySelectorAll('.progress-bar');
            setTimeout(() => {
                progressBars.forEach(bar => { bar.style.width = bar.getAttribute('data-width'); });
            }, 500);

            const welcomeMessage = "Access granted. Secure proxy terminal routing established successfully. Select any matrix element to stream telemetry audio.";
            setTimeout(() => {
                runTerminalTeletype(welcomeMessage);
                speakText(welcomeMessage);
            }, 300);
        }

        function runTerminalTeletype(text) {
            const target = document.getElementById('typewriterText');
            let index = 0;
            target.innerHTML = '';
            
            function process() {
                if (index < text.length) {
                    target.innerHTML += text.charAt(index);
                    index++;
                    setTimeout(process, 20);
                } else {
                    target.innerHTML += '<span style="animation: pulseGlow 0.8s infinite alternate">█</span>';
                }
            }
            process();
        }

        if ('speechSynthesis' in window) {
            window.speechSynthesis.getVoices();
            window.speechSynthesis.onvoiceschanged = () => { window.speechSynthesis.getVoices(); };
        }
    </script>
</body>
</html>
