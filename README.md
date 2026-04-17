
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dossier: Bitu_Vortex</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #030303;
            --surface: #0a0a0c;
            --accent: #3b82f6;
            --accent-glow: rgba(59, 130, 246, 0.4);
            --text: #ffffff;
            --text-dim: #64748b;
            --warning: #facc15;
            --error: #ef4444;
            --success: #22c55e;
            --border: #1a1a1e;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg);
            background: linear-gradient(45deg, #030303 0%, #080810 100%);
            color: var(--text);
            line-height: 1.5;
            overflow-x: hidden;
        }

        .container { max-width: 850px; margin: 60px auto; padding: 0 30px; animation: fadeIn 1s ease-out; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- Top Status Bar --- */
        .status-bar {
            display: flex;
            align-items: center;
            width: 100%;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.75rem;
            color: var(--accent);
            margin-bottom: 20px;
        }

        .status-left { display: flex; align-items: center; gap: 10px; }

        .pulse {
            width: 8px; height: 8px; background: var(--accent); border-radius: 50%;
            box-shadow: 0 0 10px var(--accent); animation: pulse-animation 2s infinite;
        }
        @keyframes pulse-animation {
            0% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.7); }
            70% { transform: scale(1); box-shadow: 0 0 0 10px rgba(59, 130, 246, 0); }
            100% { transform: scale(0.95); box-shadow: 0 0 0 0 rgba(59, 130, 246, 0); }
        }

        /* --- Header --- */
        header { border-left: 3px solid var(--accent); padding-left: 30px; margin-bottom: 30px; }
        h1 {
            font-size: 3.5rem; font-weight: 800; letter-spacing: -2px;
            overflow: hidden; white-space: nowrap; border-right: 3px solid var(--accent);
            width: 0; animation: typing 2s steps(20, end) forwards, blink 0.8s infinite;
        }
        @keyframes typing { from { width: 0 } to { width: 100% } }
        @keyframes blink { from { border-color: transparent } to { border-color: var(--accent) } }
        .title-sub { color: var(--text-dim); font-family: 'JetBrains Mono', monospace; margin-top: 10px; }

        /* --- Activity Log --- */
        .activity-log {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.7rem;
            color: var(--success);
            background: rgba(34, 197, 94, 0.03);
            border: 1px solid rgba(34, 197, 94, 0.1);
            padding: 10px 15px;
            border-radius: 4px;
            margin-bottom: 30px;
            display: flex;
            gap: 10px;
        }
        .log-cursor { animation: blink 0.8s infinite; border-left: 6px solid var(--success); margin-left: 5px; }

        /* --- Duty Monitor --- */
        .duty-monitor {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 4px;
            padding: 15px;
            margin-bottom: 40px;
        }
        .duty-header {
            display: flex; justify-content: space-between;
            font-family: 'JetBrains Mono', monospace; font-size: 0.65rem;
            color: var(--text-dim); margin-bottom: 12px;
            text-transform: uppercase; letter-spacing: 1px;
        }
        .duty-grid { display: grid; grid-template-columns: repeat(24, 1fr); gap: 4px; height: 12px; }
        .duty-cell { background: #1a1a1e; border-radius: 1px; transition: 0.3s; }
        .duty-cell.active-high { background: #3b82f6; box-shadow: 0 0 8px var(--accent-glow); }
        .duty-cell.variable { background: rgba(59, 130, 246, 0.3); animation: flicker-cell 3s infinite; }
        @keyframes flicker-cell { 0%, 100% { opacity: 1; } 50% { opacity: 0.4; } }

        /* --- Role Entries --- */
        .role-entry {
            display: flex; justify-content: space-between; align-items: center;
            padding: 20px; background: rgba(255,255,255,0.01);
            border: 1px solid var(--border); border-radius: 4px; margin-bottom: 10px;
            position: relative;
            overflow: hidden;
            transition: border 0.3s ease, transform 0.2s ease, box-shadow 0.3s ease;
        }

        .role-entry:hover {
            border-color: var(--accent);
            box-shadow: 0 0 15px var(--accent-glow);
            transform: translateX(5px);
            background: rgba(59, 130, 246, 0.03);
        }

        .role-entry::after {
            content: "";
            position: absolute;
            top: 0;
            left: -150%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(59, 130, 246, 0.15), transparent);
            transition: 0s;
        }

        .role-entry:hover::after {
            left: 150%;
            transition: 0.8s ease-in-out;
        }

        /* --- Communication Section --- */
        .no-hello-section {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 30px;
            margin-bottom: 50px;
        }
        .chat-comparison { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 20px; }
        .chat-box { padding: 15px; border-radius: 6px; font-size: 0.85rem; font-family: 'JetBrains Mono', monospace; }
        .bad { background: rgba(239, 68, 68, 0.05); border: 1px solid rgba(239, 68, 68, 0.2); }
        .good { background: rgba(34, 197, 94, 0.05); border: 1px solid rgba(34, 197, 94, 0.2); }
        .chat-label { font-weight: bold; margin-bottom: 10px; display: block; }

        /* --- Sections --- */
        .section-header {
            font-family: 'JetBrains Mono', monospace; font-size: 0.7rem;
            color: var(--text-dim); margin: 40px 0 20px 0;
            display: flex; align-items: center; gap: 15px;
        }
        .section-header::after { content: ""; height: 1px; flex-grow: 1; background: var(--border); }

        /* --- Footer --- */
        .verification-bar {
            margin-top: 40px; padding: 10px 0;
            border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);
            display: flex; justify-content: space-between; align-items: center;
        }

        .auth-status {
            font-size: 0.7rem; color: var(--accent); font-family: 'JetBrains Mono'; font-weight: 700;
        }

        .digital-sig {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.9rem;
            font-style: italic;
            color: var(--text);
            border-right: 2px solid var(--accent);
            white-space: nowrap;
            overflow: hidden;
            width: 0;
            animation: sig-typing 1.5s steps(11, end) forwards 1s, sig-blink 0.8s infinite;
        }

        @keyframes sig-typing { from { width: 0 } to { width: 105px } }
        @keyframes sig-blink { 50% { border-color: transparent } }

        footer { margin-top: 40px; padding-bottom: 30px; text-align: center; }

        @media (max-width: 600px) { .chat-comparison { grid-template-columns: 1fr; } h1 { font-size: 2.2rem; } }
    </style>
</head>
<body>

<div class="container">
    <div class="status-bar">
        <div class="status-left">
            <div class="pulse"></div>
            <span>NODE_7_ACTIVE // INDIA_REGION</span>
        </div>
    </div>

    <header>
        <h1>Bitu_Vortex</h1>
        <p class="title-sub">Colonel/deputy Of APD • Server 1 </p>
    </header>

    <div class="activity-log">
        <span id="log-timestamp">TIMESTAMP_SYNCING</span>
        <span style="opacity: 0.5;">></span>
        <span id="log-text">ADMIN_STATUS: Active duty cycle.</span>
        <span class="log-cursor"></span>
    </div>

    <div class="duty-monitor">
        <div class="duty-header">
            <span>Operational_Duty_Cycle</span>
            <span style="color: var(--success);">Protocol: ACTIVE</span>
        </div>
        <div class="duty-grid">
            <div class="duty-cell"></div><div class="duty-cell"></div><div class="duty-cell"></div><div class="duty-cell"></div><div class="duty-cell"></div><div class="duty-cell"></div><div class="duty-cell"></div>
            <div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div>
            <div class="duty-cell variable"></div><div class="duty-cell variable"></div><div class="duty-cell variable"></div>
            <div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div><div class="duty-cell active-high"></div>
            <div class="duty-cell"></div>
        </div>
    </div>

    <div class="no-hello-section">
        <h2 style="font-family: 'JetBrains Mono'; font-size: 0.9rem; color: var(--warning); margin-bottom: 10px;">COMMUNICATION_PROTOCOL</h2>
        <div class="chat-comparison">
            <div class="chat-box bad">
                <span class="chat-label" style="color: var(--error);">❌ LAGGED</span>
                "Hi/Hello." <br> [Waiting for Response]
            </div>
            <div class="chat-box good">
                <span class="chat-label" style="color: var(--success);">✅ SYNCED</span>
                "For works..." <br> [Immediate Status: Resolved]
            </div>
        </div>
    </div>

    <div class="section-header">ACTIVE_CLEARANCE</div>
    <div class="role-entry"><div><h3>Colonel/deputy Of APD</h3><span style="color: var(--accent); font-family: 'JetBrains Mono'; font-size: 0.75rem;">Server 1</span></div></div>
    <div class="role-entry"><div><h3>...</h3><span style="color: var(--accent); font-family: 'JetBrains Mono'; font-size: 0.75rem;">...</span></div></div>
    <div class="role-entry"><div><h3>...</h3><span style="color: var(--accent); font-family: 'JetBrains Mono'; font-size: 0.75rem;">...</span></div></div>

    <div class="section-header">HISTORICAL_RECORDS</div>
    <div class="role-entry" style="opacity: 0.7;">
        <div><h3>Curator of Police Department</h3><span>Server 1 Archive</span></div>
    </div>
    <div class="role-entry" style="opacity: 0.7;">
        <div><h3>Administrator</h3><span>Server 1 Archive</span></div>
    </div>
    <div class="role-entry" style="opacity: 0.7;">
        <div><h3>Colonel/deputy Of APD x1 (1 Times)</h3><span>Server 1 Archive</span></div>
    </div>
    <div class="role-entry" style="opacity: 0.7;">
        <div><h3>Lieutenant Colonel Of APD x2 (2 Times)</h3><span>Server 1 Archive</span></div>
    </div>
    <div class="role-entry" style="opacity: 0.7;">
        <div><h3>Lieutenant Colonel YPD x1 (1 Times)</h3><span>Server 1 Archive</span></div>
    </div>
    <div class="role-entry" style="opacity: 0.7;">
        <div><h3>Major Of YPD x1 (1 Times)</h3><span>Server 1 Archive</span></div>
    </div>

    <div class="verification-bar">
        <div class="auth-status">IDENTITY // AUTHENTICATING...</div>
        <div class="digital-sig">Bitu_Vortex</div>
    </div>

    <footer>
        <p style="color: var(--text-dim); font-size: 0.75rem; opacity: 0.6;">&copy; 2026 Bitu_Vortex. All Rights Reserved.</p>
        <div style="margin-top: 20px; border-top: 1px solid var(--border); padding-top: 25px;">
            <p style="font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; color: var(--accent); letter-spacing: 1.5px; font-weight: 700; line-height: 1.6; text-transform: uppercase;">
                Designed By Bitu_Vortex
            </p>
            <p style="font-size: 0.55rem; margin-top: 20px; color: #1a1a1e; font-family: 'JetBrains Mono'; letter-spacing: 3px;">
                SECURE_NODE_7 // Current region: India // ENCRYPTION: AES-256
            </p>
        </div>
    </footer>
</div>

<script>
    const logMessages = [
        "ADMIN_STATUS: Active duty cycle.",
        "Current region: Ireland"
    ];
    let msgIndex = 0;

    function updateLog() {
        msgIndex = (msgIndex + 1) % logMessages.length;
        document.getElementById('log-text').textContent = logMessages[msgIndex];

        // --- Custom Time Logic (+3 Hours from local) ---
        const now = new Date();
        now.setHours(now.getHours() + 3);

        const ts = now.toISOString().replace('T', ' ').split('.')[0];
        document.getElementById('log-timestamp').textContent = ts;
    }

    setInterval(updateLog, 4000);
    updateLog();

    setTimeout(() => {
        document.querySelector('.auth-status').textContent = 'IDENTITY // AUTHENTIC';
    }, 2500);

</script>

</body>
</html>
