
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>BITU_VORTEX // COMMAND NETWORK</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --bg-dark: #030307;
            --bg-card: rgba(10, 12, 22, 0.75);
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
            pointer-events: none;
        }

        #landingScreen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 20;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.8s cubic-bezier(0.4, 0, 0.2, 1), visibility 0.8s;
            background: radial-gradient(circle at center, rgba(3,3,7,0.95) 0%, #030307 100%);
            backdrop-filter: blur(2px);
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
            background: rgba(2, 5, 12, 0.9);
            color: var(--neon-blue);
            border: 1.5px solid var(--neon-blue);
            padding: 15px 40px;
            font-size: 1.2rem;
            font-weight: 700;
            letter-spacing: 3px;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            border-radius: 4px;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.2);
            transition: all 0.3s ease;
            touch-action: manipulation;
            backdrop-filter: blur(5px);
        }

        .enter-btn:hover, .enter-btn:active {
            color: var(--text-bright);
            background: var(--neon-blue);
            box-shadow: 0 0 35px var(--neon-blue), inset 0 0 15px rgba(255,255,255,0.5);
            text-shadow: 0 0 5px #000;
            border-color: #fff;
            transform: scale(1.02);
        }

        #scannerBar {
            position: fixed;
            top: -10px;
            left: 0;
            width: 100%;
            height: 6px;
            background: linear-gradient(to bottom, transparent, var(--neon-blue), transparent);
            box-shadow: 0 0 25px var(--neon-blue);
            z-index: 1001;
            display: none;
            pointer-events: none;
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
            transition: opacity 1s ease, visibility 0.5s;
            background: radial-gradient(circle at 20% 30%, rgba(0,0,0,0.9), var(--bg-dark));
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
            transition: all 0.2s;
        }

        .profile-card:active {
            transform: scale(0.99);
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
            background: rgba(0,0,0,0.6);
            border: 1px solid rgba(0, 240, 255, 0.2);
            padding: 12px 15px;
            border-radius: 4px;
            font-size: 0.85rem;
            color: #10b981;
            margin-top: 18px;
            min-height: 50px;
            display: flex;
            align-items: center;
            font-family: 'Share Tech Mono', monospace;
            letter-spacing: 0.5px;
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

        .info-card:active {
            transform: scale(0.98);
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
            .huge-title { font-size: 2rem; }
            .ring-container { width: 240px; height: 240px; }
            .profile-name { font-size: 1.6rem; }
            .enter-btn { padding: 12px 28px; font-size: 1rem; }
        }
    </style>
</head>
<body>

    <div class="scanlines"></div>
    <canvas id="particleCanvas"></canvas>
    <div id="scannerBar"></div>

    <!-- LANDING SECTION -->
    <div id="landingScreen">
        <div class="ring-container">
            <div class="energy-ring outer"></div>
            <div class="energy-ring inner"></div>
            <h1 class="huge-title glitch-hover">BITU_VORTEX</h1>
        </div>
        <div class="btn-box">
            <button class="enter-btn" id="enterSystemBtn">ENTER SYSTEM</button>
        </div>
    </div>

    <!-- DASHBOARD SECTION -->
    <div id="dashboardScreen">
        <div class="dashboard-container">
            
            <div class="profile-card" data-speak="Bitu Vortex, Administrator 01, Server 1">
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
                    <div class="info-card scroll-reveal" data-speak="Administrator 01, Core Matrix Operations, Server 1">
                        <div class="card-main-text">Administrator 01</div>
                        <div class="card-sub-text">Core Matrix Operations // Server 1</div>
                        <div class="progress-container"><div class="progress-bar" data-width="95%"></div></div>
                    </div>
                    <div class="info-card scroll-reveal" data-speak="Curator Of Government House, Executive Records Oversight, Server 1">
                        <div class="card-main-text">Curator Of Government House</div>
                        <div class="card-sub-text">Executive Records Oversight // Server 1</div>
                        <div class="progress-container"><div class="progress-bar" data-width="84%"></div></div>
                    </div>
                </div>
            </div>

            <div>
                <h3 class="section-title">HISTORICAL RECORDS ARCHIVE <span>[ENCRYPTED]</span></h3>
                <div class="grid-layout">
                    <div class="info-card scroll-reveal" data-speak="Curator Of Police Department, Historical Archives, Server 1 Archive">
                        <div class="card-main-text">Curator Of Police Department</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                    <div class="info-card scroll-reveal" data-speak="Colonel Of APD, three times, Historical Archives">
                        <div class="card-main-text">Colonel Of APD ×3</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                    <div class="info-card scroll-reveal" data-speak="Lieutenant Colonel Of APD, two times, Historical Archives">
                        <div class="card-main-text">Lieutenant Colonel Of APD ×2</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                    <div class="info-card scroll-reveal" data-speak="Lieutenant Colonel Of YPD, one time, Historical Archives">
                        <div class="card-main-text">Lieutenant Colonel Of YPD ×1</div>
                        <div class="card-sub-text">Historical Archives // Server 1 Archive</div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <script>
        (function() {
            // ------------------------------
            // 1. Particle System
            // ------------------------------
            const canvas = document.getElementById('particleCanvas');
            const ctx = canvas.getContext('2d');
            let particlesArray = [];
            let mouse = { x: null, y: null, radius: 85 };

            function resizeCanvas() { 
                canvas.width = window.innerWidth; 
                canvas.height = window.innerHeight; 
            }
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();

            window.addEventListener('mousemove', (e) => { mouse.x = e.x; mouse.y = e.y; });
            window.addEventListener('mouseleave', () => { mouse.x = null; mouse.y = null; });
            window.addEventListener('touchmove', (e) => {
                if(e.touches.length) { mouse.x = e.touches[0].clientX; mouse.y = e.touches[0].clientY; }
            });
            window.addEventListener('touchend', () => { mouse.x = null; mouse.y = null; });

            class Particle {
                constructor() {
                    this.x = Math.random() * canvas.width;
                    this.y = Math.random() * canvas.height;
                    this.size = Math.random() * 2.5 + 0.5;
                    this.color = Math.random() > 0.5 ? 'rgba(0, 240, 255, 0.5)' : 'rgba(255, 0, 85, 0.4)';
                    this.vx = (Math.random() - 0.5) * 0.25;
                    this.vy = (Math.random() - 0.5) * 0.25;
                }
                update() {
                    if (mouse.x != null && mouse.y != null) {
                        let dx = mouse.x - this.x;
                        let dy = mouse.y - this.y;
                        let distance = Math.hypot(dx, dy);
                        if (distance < mouse.radius) {
                            let force = (mouse.radius - distance) / mouse.radius;
                            this.x -= (dx / (distance + 0.001)) * force * 2.2;
                            this.y -= (dy / (distance + 0.001)) * force * 2.2;
                        }
                    }
                    this.x += this.vx;
                    this.y += this.vy;
                    if(this.x < 0) this.x = canvas.width;
                    if(this.x > canvas.width) this.x = 0;
                    if(this.y < 0) this.y = canvas.height;
                    if(this.y > canvas.height) this.y = 0;
                }
                draw() {
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                    ctx.fillStyle = this.color;
                    ctx.fill();
                }
            }

            function initParticles() {
                particlesArray = [];
                const count = Math.min(Math.floor(canvas.width / 8), 180);
                for (let i = 0; i < count; i++) particlesArray.push(new Particle());
            }
            initParticles();

            function animateParticles() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                for (let p of particlesArray) {
                    p.update();
                    p.draw();
                }
                requestAnimationFrame(animateParticles);
            }
            animateParticles();
            window.addEventListener('resize', () => { initParticles(); resizeCanvas(); });

            // ------------------------------
            // 2. Speech Engine
            // ------------------------------
            let speechSynth = window.speechSynthesis;

            function speakText(phrase) {
                if (!phrase || !speechSynth) return;
                try {
                    speechSynth.cancel();
                    const utterance = new SpeechSynthesisUtterance(phrase);
                    utterance.rate = 0.95;
                    utterance.pitch = 0.98;
                    utterance.volume = 1;
                    
                    const setVoiceAndSpeak = () => {
                        const voices = speechSynth.getVoices();
                        const preferred = voices.find(v => v.lang === 'en-US' && (v.name.includes('Google') || v.name.includes('Samantha') || v.name.includes('Zira'))) ||
                                         voices.find(v => v.lang.startsWith('en'));
                        if (preferred) utterance.voice = preferred;
                        speechSynth.speak(utterance);
                    };
                    
                    if (speechSynth.getVoices().length === 0) {
                        speechSynth.addEventListener('voiceschanged', setVoiceAndSpeak, { once: true });
                    } else {
                        setVoiceAndSpeak();
                    }
                } catch(e) { console.warn("Speech error", e); }
            }

            function attachSpeechToElements() {
                const speakables = document.querySelectorAll('[data-speak]');
                speakables.forEach(el => {
                    el.removeEventListener('click', handleSpeakClick);
                    el.addEventListener('click', handleSpeakClick);
                });
            }

            function handleSpeakClick(e) {
                e.stopPropagation();
                const text = this.getAttribute('data-speak');
                if (text) speakText(text);
            }

            // ------------------------------
            // 3. Transition + Typewriter
            // ------------------------------
            const landing = document.getElementById('landingScreen');
            const dashboard = document.getElementById('dashboardScreen');
            const scannerBar = document.getElementById('scannerBar');
            const enterBtn = document.getElementById('enterSystemBtn');

            const typewriterDiv = document.getElementById('typewriterText');
            const bootMessages = [
                "> CONNECTING TO SECURE NODE...",
                "> BITU_VORTEX CORE ONLINE.",
                "> ACCESS GRANTED. WELCOME ADMIN."
            ];
            let msgIndex = 0;
            let charIndex = 0;
            let currentMessage = "";

            function typeNextMessage() {
                if (!typewriterDiv) return;
                if (msgIndex >= bootMessages.length) {
                    typewriterDiv.innerHTML = currentMessage + '<span style="opacity:0.7; animation: pulseGlow 1s infinite;">_</span>';
                    return;
                }
                if (charIndex === 0) {
                    currentMessage = bootMessages[msgIndex];
                    typewriterDiv.innerHTML = currentMessage.charAt(0) + '<span style="opacity:1;">|</span>';
                    charIndex = 1;
                    setTimeout(typeNextMessage, 70);
                } else if (charIndex <= currentMessage.length) {
                    let partial = currentMessage.substring(0, charIndex);
                    typewriterDiv.innerHTML = partial + '<span style="opacity:1;">|</span>';
                    charIndex++;
                    setTimeout(typeNextMessage, 45);
                } else {
                    typewriterDiv.innerHTML = currentMessage;
                    msgIndex++;
                    charIndex = 0;
                    if (msgIndex < bootMessages.length) {
                        setTimeout(() => {
                            typewriterDiv.innerHTML = currentMessage + '<br>> ';
                            setTimeout(typeNextMessage, 180);
                        }, 350);
                    } else {
                        setTimeout(() => {
                            typewriterDiv.innerHTML = currentMessage + ' <span style="opacity:0.7; animation: pulseGlow 1s infinite;">_</span>';
                        }, 200);
                    }
                }
            }

            function startTypewriter() {
                msgIndex = 0;
                charIndex = 0;
                if(typewriterDiv) typewriterDiv.innerHTML = "";
                typeNextMessage();
            }

            // MAIN FUNCTION - This is what the button calls
            window.enterSystem = function() {
                // Prevent double triggering
                if (!landing || dashboard.style.visibility === 'visible') return;
                
                // Scanner bar animation
                scannerBar.style.display = 'block';
                scannerBar.style.animation = 'none';
                scannerBar.offsetHeight; // Force reflow
                scannerBar.style.animation = 'scanTransition 1.2s forwards';
                
                // Start typewriter effect
                startTypewriter();
                
                // Fade out landing screen
                landing.style.opacity = '0';
                landing.style.visibility = 'hidden';
                setTimeout(() => {
                    landing.style.display = 'none';
                }, 800);
                
                // Show dashboard
                dashboard.style.visibility = 'visible';
                dashboard.style.opacity = '0';
                setTimeout(() => {
                    dashboard.style.opacity = '1';
                    triggerProgressAndReveal();
                }, 120);
                
                setTimeout(() => {
                    scannerBar.style.display = 'none';
                }, 1400);
                
                // Welcome voice
                setTimeout(() => {
                    speakText("System online. Bitu Vortex command network active.");
                }, 900);
            };
            
            function triggerProgressAndReveal() {
                // Animate progress bars
                document.querySelectorAll('.progress-bar').forEach(bar => {
                    const widthVal = bar.getAttribute('data-width');
                    if (widthVal) {
                        setTimeout(() => { bar.style.width = widthVal; }, 100);
                    } else {
                        bar.style.width = '75%';
                    }
                });
                
                // Reveal cards with intersection observer
                const cards = document.querySelectorAll('.info-card');
                const observer = new IntersectionObserver((entries) => {
                    entries.forEach(entry => {
                        if (entry.isIntersecting) {
                            entry.target.classList.add('visible');
                            observer.unobserve(entry.target);
                        }
                    });
                }, { threshold: 0.1 });
                cards.forEach(card => observer.observe(card));
                
                setTimeout(() => {
                    cards.forEach(card => {
                        if (card.getBoundingClientRect().top < window.innerHeight - 100) 
                            card.classList.add('visible');
                    });
                }, 200);
            }
            
            // FIXED: Direct button click handler - SIMPLE AND RELIABLE
            if (enterBtn) {
                // Remove any existing listeners by cloning
                const newBtn = enterBtn.cloneNode(true);
                enterBtn.parentNode.replaceChild(newBtn, enterBtn);
                const finalButton = document.getElementById('enterSystemBtn');
                
                if (finalButton) {
                    // Add click handler
                    finalButton.addEventListener('click', function(e) {
                        e.preventDefault();
                        e.stopPropagation();
                        window.enterSystem();
                    });
                    
                    // Add touch handler for mobile
                    finalButton.addEventListener('touchstart', function(e) {
                        e.preventDefault();
                        window.enterSystem();
                    }, { passive: false });
                }
            }
            
            // Bind speech to all cards
            function bindAllCardSpeeches() {
                attachSpeechToElements();
                document.querySelectorAll('.profile-card, .info-card').forEach(el => {
                    if (!el.getAttribute('data-speak') && el.querySelector('.card-main-text')) {
                        if (el.classList.contains('profile-card')) {
                            el.setAttribute('data-speak', 'Bitu Vortex, Administrator 01, Server 1');
                        } else {
                            const mainTxt = el.querySelector('.card-main-text')?.innerText;
                            if (mainTxt) el.setAttribute('data-speak', mainTxt);
                        }
                    }
                });
                attachSpeechToElements();
            }
            
            // Initialize on load
            window.addEventListener('load', () => {
                bindAllCardSpeeches();
                
                // Reset progress bars
                document.querySelectorAll('.progress-bar').forEach(bar => bar.style.width = '0%');
                
                // Ensure landing screen is visible
                landing.style.display = 'flex';
                landing.style.opacity = '1';
                landing.style.visibility = 'visible';
                dashboard.style.opacity = '0';
                dashboard.style.visibility = 'hidden';
                
                // Make sure button is enabled
                const btn = document.getElementById('enterSystemBtn');
                if(btn) {
                    btn.disabled = false;
                    btn.style.pointerEvents = 'auto';
                }
                
                // Preload speech voices
                if(speechSynth) speechSynth.getVoices();
            });
            
            setTimeout(bindAllCardSpeeches, 500);
        })();
    </script>
</body>
</html>
