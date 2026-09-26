
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BIO_CONTAINMENT_MATRIX_v9 - Bunker Anti-Vírus BSL-4</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }
        body {
            background-color: #030a06;
            color: #10b981;
            font-family: 'Consolas', 'Courier New', monospace;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 10px;
            transition: background 0.5s ease;
        }
        .container {
            width: 100%;
            max-width: 1150px;
            background: #06150c;
            border: 2px solid #10b981;
            box-shadow: 0 0 35px rgba(16, 185, 129, 0.25);
            border-radius: 10px;
            overflow: hidden;
            transition: border-color 0.5s ease, box-shadow 0.5s ease;
        }
        .header {
            background: #020d06;
            padding: 14px;
            text-align: center;
            border-bottom: 2px solid #10b981;
            font-weight: bold;
            font-size: 1.15rem;
            letter-spacing: 3px;
            color: #10b981;
            text-shadow: 0 0 12px rgba(16, 185, 129, 0.8);
            transition: color 0.5s ease, border-color 0.5s ease;
        }
        .hud-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
            gap: 8px;
            padding: 10px;
            background: #041209;
            border-bottom: 1px solid #0f2e1a;
        }
        .hud-card {
            background: #020804;
            border: 1px solid #10b98144;
            padding: 8px;
            border-radius: 4px;
        }
        .hud-title {
            font-size: 0.65rem;
            color: #6ee7b7;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .hud-value {
            font-size: 0.88rem;
            font-weight: bold;
            color: #ffffff;
            margin-top: 3px;
            word-break: break-word;
        }
        .hud-value.alert { color: #ff0055; text-shadow: 0 0 8px #ff0055; }
        .hud-value.active { color: #10b981; text-shadow: 0 0 8px #10b981; }
        .hud-value.theme { color: #10b981; text-shadow: 0 0 8px #10b981; }

        canvas {
            display: block;
            width: 100%;
            height: 460px;
            background-color: #010503;
            cursor: crosshair;
        }

        .virus-selector {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            padding: 10px;
            background: #030d07;
            justify-content: center;
            border-bottom: 1px solid #0f2e1a;
        }

        .btn-virus {
            background: #061b0f;
            color: #a7f3d0;
            border: 1px solid #059669;
            padding: 6px 10px;
            font-family: inherit;
            font-size: 0.68rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 3px;
            transition: all 0.3s ease;
        }

        .btn-virus.selected {
            color: #ffffff;
            box-shadow: 0 0 12px currentColor;
        }

        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            padding: 12px;
            background: #030d07;
            justify-content: center;
            align-items: center;
            border-top: 1px solid #0f2e1a;
        }

        button.btn-bio {
            background: #041209;
            color: #10b981;
            border: 1px solid #10b981;
            padding: 8px 12px;
            font-family: inherit;
            font-size: 0.75rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.2s ease;
            text-shadow: 0 0 4px #10b981;
        }

        button.btn-bio:hover {
            background: #10b981;
            color: #000;
            box-shadow: 0 0 15px #10b981;
        }

        button.btn-hazard {
            color: #ff0055;
            border-color: #ff0055;
            text-shadow: 0 0 4px #ff0055;
        }
        button.btn-hazard:hover {
            background: #ff0055;
            color: #000;
            box-shadow: 0 0 15px #ff0055;
        }

        .terminal {
            background: #010402;
            padding: 8px 12px;
            font-family: 'Courier New', Courier, monospace;
            font-size: 0.74rem;
            color: #10b981aa;
            height: 65px;
            overflow-y: auto;
            border-top: 1px solid #0f2e1a;
        }
    </style>
</head>
<body>

<div class="container" id="main-container">
    <div class="header" id="main-header">
        ☣️ BIO_CONTAINMENT_MATRIX v9 // ANTI-VIRUS BSL-4 BUNKER SIMULATOR
    </div>

    <!-- SELETOR DOS VÍRUS (DO MAIS LETAL AO MENOS LETAL) -->
    <div class="virus-selector" id="virus-selector">
        <button class="btn-virus selected" onclick="selectVirus('rabies')" style="border-color:#ff0055;">1. RAIVA (99.9%)</button>
        <button class="btn-virus" onclick="selectVirus('ebola')" style="border-color:#ff2200;">2. EBOLA (90%)</button>
        <button class="btn-virus" onclick="selectVirus('marburg')" style="border-color:#ff6600;">3. MARBURG (88%)</button>
        <button class="btn-virus" onclick="selectVirus('nipah')" style="border-color:#d946ef;">4. NIPAH (75%)</button>
        <button class="btn-virus" onclick="selectVirus('hantavirus')" style="border-color:#a855f7;">5. HANTAVÍRUS (38%)</button>
        <button class="btn-virus" onclick="selectVirus('smallpox')" style="border-color:#f59e0b;">6. VARIÓLA (30%)</button>
        <button class="btn-virus" onclick="selectVirus('sarscov2')" style="border-color:#eab308;">7. SARS-CoV-2 (2%)</button>
        <button class="btn-virus" onclick="selectVirus('influenza')" style="border-color:#06b6d4;">8. INFLUENZA A (0.1%)</button>
        <button class="btn-virus" onclick="selectVirus('rhinovirus')" style="border-color:#10b981;">9. RINOVÍRUS (0.001%)</button>
    </div>

    <!-- HUD PAINEL BIOLÓGICO -->
    <div class="hud-grid">
        <div class="hud-card">
            <div class="hud-title">PATÓGENO SELECIONADO</div>
            <div class="hud-value theme" id="hud-name">Vírus da Raiva</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">ORIGEM / PRIMEIRO REGISTRO</div>
            <div class="hud-value" id="hud-origin">Global (Histórico Antigo)</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">TAXA DE LETALIDADE</div>
            <div class="hud-value alert" id="hud-fatality">99.9%</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">MORFOLOGIA E FORMATO</div>
            <div class="hud-value" id="hud-shape">Bala (Rhabdoviridae)</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">STATUS DO AIRLOCK BSL-4</div>
            <div class="hud-value active" id="hud-airlock">DESATILADO / ABERTO</div>
        </div>
        <div class="hud-card">
            <div class="hud-title">CARGA VIRAL INTERNA</div>
            <div class="hud-value active" id="hud-load">0 cópias/mL</div>
        </div>
    </div>

    <!-- TELA DA SIMULAÇÃO (CANVAS 2D) -->
    <canvas id="simCanvas" width="1100" height="460"></canvas>

    <!-- CONTROLES DO BUNKER E CONTRAMEDIDAS -->
    <div class="controls">
        <button class="btn-bio" id="btn-toggle">⏸️ PAUSAR</button>
        <button class="btn-bio" id="btn-reset">🔄 REINICIAR</button>
        <button class="btn-bio" id="btn-uvc">🔮 BARREIRA UVC GERMICIDA</button>
        <button class="btn-bio" id="btn-airlock">🔒 ESCOTILHA HERMÉTICA BSL-4</button>
        <button class="btn-bio" id="btn-vhp">💨 NEBULIZADOR DE PERÓXIDO (VHP)</button>
        <button class="btn-bio" id="btn-plasma">🔥 INCINERADOR DE PLASMA</button>
        <button class="btn-bio btn-hazard" id="btn-outbreak">☣️ DISPARAR NÚCLEO BIO-INVIÁVEL</button>
    </div>

    <!-- TERMINAL BIOMÉDICO LOG -->
    <div class="terminal" id="terminal-log">
        [BSL-4 SYSTEM v9] Biosensores calibrados. Laboratório subterrâneo pronto para contenção.
    </div>
</div>

<script>
    const canvas = document.getElementById('simCanvas');
    const ctx = canvas.getContext('2d');

    // BANCO DE DADOS DOS VÍRUS (DO MAIS LETAL AO MENOS LETAL)
    const VIRUSES = {
        rabies: {
            name: "Vírus da Raiva (Rabies lyssavirus)",
            origin: "Mundial / Mamíferos Silvestres",
            fatality: "99.9%",
            shape: "Formato de Bala (Rhabdovirus)",
            color: "#ff0055",
            bgBg: "#170008",
            reaction: "Ataca o sistema nervoso central com propagação axonal direta.",
            morphType: "bullet"
        },
        ebola: {
            name: "Ebola (Zaire ebolavirus)",
            origin: "Rio Ebola, Zaire (R.D. Congo - 1976)",
            fatality: "90.0%",
            shape: "Filamentoso / Linha Ondulada (Filovirus)",
            color: "#ff2200",
            bgBg: "#1c0400",
            reaction: "Induz febre hemorrágica grave e colapso endotelial vascular.",
            morphType: "filament"
        },
        marburg: {
            name: "Vírus Marburg (Marburg marburgvirus)",
            origin: "Marburg, Alemanha / Uganda (1967)",
            fatality: "88.0%",
            shape: "Filamento em Gancho (Cajado de Pastor)",
            color: "#ff6600",
            bgBg: "#1c0b00",
            reaction: "Causa necrose hepática maciça e falência múltipla de órgãos.",
            morphType: "hook"
        },
        nipah: {
            name: "Vírus Nipah (Nipah henipavirus)",
            origin: "Kampung Sungai Nipah, Malásia (1998)",
            fatality: "75.0%",
            shape: "Esférico Envelopado Revestido",
            color: "#d946ef",
            bgBg: "#17021c",
            reaction: "Provoca encefalite aguda e síndrome respiratória grave.",
            morphType: "enveloped"
        },
        hantavirus: {
            name: "Hantavírus (Orthohantavirus)",
            origin: "Rio Hantan, Coreia do Sul (1976)",
            fatality: "38.0%",
            shape: "Pleomórfico Irregular com Espículas",
            color: "#a855f7",
            bgBg: "#10021c",
            reaction: "Causa Síndrome Pulmonar e Febre Hemorrágica com Síndrome Renal.",
            morphType: "blob"
        },
        smallpox: {
            name: "Variola major (Variola virus)",
            origin: "Antigo Egito / Índia (Eliminado em 1980)",
            fatality: "30.0%",
            shape: "Formato de Tijolo Oval (Poxvirus)",
            color: "#f59e0b",
            bgBg: "#1c1100",
            reaction: "Multiplica-se em macrófagos causando lesões pustulosas profundas.",
            morphType: "brick"
        },
        sarscov2: {
            name: "SARS-CoV-2 (Coronavírus)",
            origin: "Wuhan, China (2019)",
            fatality: "1.5% - 2.0%",
            shape: "Esfera com Espículas Proteicas (Corona)",
            color: "#eab308",
            bgBg: "#1c1600",
            reaction: "Aliga-se aos receptores ACE2 induzindo tempestade de citocinas.",
            morphType: "corona"
        },
        influenza: {
            name: "Influenza A (H1N1 / H5N1)",
            origin: "Aves / Suínos (Pandemias Globais)",
            fatality: "0.1% - 2.5%",
            shape: "Esférico com Proteínas HA e NA",
            color: "#06b6d4",
            bgBg: "#001a21",
            reaction: "Infecção epitelial do trato respiratório por hemaglutinina.",
            morphType: "flu"
        },
        rhinovirus: {
            name: "Rinovírus Humano (Resfriado Comum)",
            origin: "Mundial (Endêmico)",
            fatality: "< 0.001%",
            shape: "Icosaédrico / Geométrico Rígido",
            color: "#10b981",
            bgBg: "#001c0f",
            reaction: "Replicação limitada à mucosa nasal sob baixa temperatura.",
            morphType: "icosahedral"
        }
    };

    let currentKey = 'rabies';
    let currentVirus = VIRUSES.rabies;

    // Estados da Simulação
    let isRunning = true;
    let outbreakActive = false;
    let viralParticles = [];
    let viralLoadInBunker = 0; // cópias/mL

    // Defesas Ativadas do Bunker
    let uvcActive = false;
    let airlockActive = false;
    let vhpActive = false;
    let plasmaActive = false;

    // Coordenadas das Zonas no Canvas
    const AIRLOCK_X = 500;
    const BUNKER_X = 750;
    const BUNKER_Y = 100;
    const BUNKER_W = 300;
    const BUNKER_H = 280;

    function log(msg) {
        const term = document.getElementById('terminal-log');
        term.innerHTML = `> ${msg}<br>` + term.innerHTML;
    }

    function selectVirus(key) {
        currentKey = key;
        currentVirus = VIRUSES[key];

        document.querySelectorAll('.btn-virus').forEach(b => b.classList.remove('selected'));
        event.target.classList.add('selected');

        const container = document.getElementById('main-container');
        container.style.borderColor = currentVirus.color;
        container.style.boxShadow = `0 0 35px ${currentVirus.color}44`;

        const header = document.getElementById('main-header');
        header.style.color = currentVirus.color;
        header.style.borderColor = currentVirus.color;

        document.getElementById('terminal-log').style.color = currentVirus.color + 'aa';

        resetSimulation();
        log(`🔬 Amostra Ativa: ${currentVirus.name}. Origem: ${currentVirus.origin}. Taxa de Letalidade: ${currentVirus.fatality}.`);
    }

    function resetSimulation() {
        outbreakActive = false;
        viralParticles = [];
        viralLoadInBunker = 0;
        updateHUD();
    }

    function updateHUD() {
        document.getElementById('hud-name').innerText = currentVirus.name.split(' (')[0];
        document.getElementById('hud-name').style.color = currentVirus.color;

        document.getElementById('hud-origin').innerText = currentVirus.origin;
        document.getElementById('hud-fatality').innerText = currentVirus.fatality;
        document.getElementById('hud-shape').innerText = currentVirus.shape;

        const airlockElem = document.getElementById('hud-airlock');
        airlockElem.innerText = airlockActive ? "HERMÉTICO / SELADO BSL-4" : "ABERTO / RISCO BIOLÓGICO";
        airlockElem.className = airlockActive ? "hud-value active" : "hud-value alert";

        const loadElem = document.getElementById('hud-load');
        loadElem.innerText = `${Math.round(viralLoadInBunker)} cópias/mL`;
        loadElem.className = viralLoadInBunker > 50 ? "hud-value alert" : "hud-value active";
    }

    // Botões de Ação
    document.getElementById('btn-toggle').addEventListener('click', (e) => {
        isRunning = !isRunning;
        e.target.innerText = isRunning ? "⏸️ PAUSAR" : "▶️ RETOMAR";
    });

    document.getElementById('btn-reset').addEventListener('click', () => {
        resetSimulation();
        log("Câmara de descontaminação purgada e sensores resetados.");
    });

    document.getElementById('btn-uvc').addEventListener('click', (e) => {
        uvcActive = !uvcActive;
        e.target.style.background = uvcActive ? currentVirus.color : "#041209";
        e.target.style.color = uvcActive ? "#000" : currentVirus.color;
        log(uvcActive ? "🔮 RADIAÇÃO UVC C-BAND ATIVADA: Rompendo ligações de RNA/DNA nas extremidades." : "Emissor UVC desligado.");
    });

    document.getElementById('btn-airlock').addEventListener('click', (e) => {
        airlockActive = !airlockActive;
        e.target.style.background = airlockActive ? currentVirus.color : "#041209";
        e.target.style.color = airlockActive ? "#000" : currentVirus.color;
        log(airlockActive ? "🔒 ESCOTILHA DE PRESSÃO BSL-4 SELADA: Bloqueando avanço de aerossóis." : "Airlock aberto.");
    });

    document.getElementById('btn-vhp').addEventListener('click', (e) => {
        vhpActive = !vhpActive;
        e.target.style.background = vhpActive ? currentVirus.color : "#041209";
        e.target.style.color = vhpActive ? "#000" : currentVirus.color;
        log(vhpActive ? "💨 MISTURA VHP (PERÓXIDO DE HIDROGÊNIO VAPORIZADO): Oxidação maciça de capsídeos!" : "Nebulização química desativada.");
    });

    document.getElementById('btn-plasma').addEventListener('click', (e) => {
        plasmaActive = !plasmaActive;
        e.target.style.background = plasmaActive ? currentVirus.color : "#041209";
        e.target.style.color = plasmaActive ? "#000" : currentVirus.color;
        log(plasmaActive ? "🔥 BARREIRA DE PLASMA TÉRMICA (1200°C): Destruição proteica instantânea!" : "Incinerador desativado.");
    });

    document.getElementById('btn-outbreak').addEventListener('click', () => {
        outbreakActive = true;
        for (let i = 0; i < 40; i++) {
            viralParticles.push(createParticle());
        }
        log(`☣️ SURTO BIO-INVIÁVEL GERADO! Aerossóis de ${currentVirus.name} propagando-se em direção ao Bunker!`);
    });

    function createParticle() {
        return {
            x: Math.random() * 150 + 20,
            y: Math.random() * (canvas.height - 100) + 50,
            vx: Math.random() * 2 + 1.2,
            vy: (Math.random() - 0.5) * 1.5,
            size: Math.random() * 6 + 10,
            angle: Math.random() * Math.PI * 2,
            spin: (Math.random() - 0.5) * 0.05,
            integrity: 100
        };
    }

    // Desenho Morfológico Único de Cada Vírus
    function drawVirusShape(ctx, p, type, color) {
        ctx.save();
        ctx.translate(p.x, p.y);
        ctx.rotate(p.angle);
        ctx.strokeStyle = color;
        ctx.fillStyle = color;
        ctx.lineWidth = 1.8;

        if (type === 'bullet') { // Raiva
            ctx.beginPath();
            ctx.moveTo(-p.size, -p.size / 2);
            ctx.lineTo(0, -p.size / 2);
            ctx.arc(0, 0, p.size / 2, -Math.PI / 2, Math.PI / 2);
            ctx.lineTo(-p.size, p.size / 2);
            ctx.closePath();
            ctx.stroke();
            ctx.fillRect(-p.size + 2, -p.size / 2 + 2, p.size - 2, p.size - 4);
        } 
        else if (type === 'filament') { // Ebola
            ctx.beginPath();
            ctx.moveTo(-p.size * 1.5, 0);
            ctx.bezierCurveTo(-p.size, -p.size, p.size, p.size, p.size * 1.5, 0);
            ctx.stroke();
        } 
        else if (type === 'hook') { // Marburg
            ctx.beginPath();
            ctx.moveTo(-p.size * 1.2, 0);
            ctx.lineTo(p.size * 0.8, 0);
            ctx.arc(p.size * 0.8, -p.size * 0.5, p.size * 0.5, Math.PI / 2, -Math.PI / 2, true);
            ctx.stroke();
        } 
        else if (type === 'enveloped') { // Nipah
            ctx.beginPath();
            ctx.arc(0, 0, p.size, 0, Math.PI * 2);
            ctx.stroke();
            for (let a = 0; a < Math.PI * 2; a += Math.PI / 4) {
                let rx = Math.cos(a) * (p.size + 3);
                let ry = Math.sin(a) * (p.size + 3);
                ctx.fillRect(rx - 1, ry - 1, 3, 3);
            }
        } 
        else if (type === 'blob') { // Hantavírus
            ctx.beginPath();
            ctx.ellipse(0, 0, p.size * 1.2, p.size * 0.8, Math.PI / 4, 0, Math.PI * 2);
            ctx.stroke();
        } 
        else if (type === 'brick') { // Variola
            ctx.strokeRect(-p.size, -p.size * 0.6, p.size * 2, p.size * 1.2);
            ctx.fillRect(-p.size * 0.5, -p.size * 0.3, p.size, p.size * 0.6);
        } 
        else if (type === 'corona') { // SARS-CoV-2
            ctx.beginPath();
            ctx.arc(0, 0, p.size * 0.8, 0, Math.PI * 2);
            ctx.stroke();
            for (let a = 0; a < Math.PI * 2; a += Math.PI / 6) {
                let sx = Math.cos(a) * (p.size * 0.8);
                let sy = Math.sin(a) * (p.size * 0.8);
                let ex = Math.cos(a) * (p.size * 1.3);
                let ey = Math.sin(a) * (p.size * 1.3);
                ctx.beginPath();
                ctx.moveTo(sx, sy);
                ctx.lineTo(ex, ey);
                ctx.stroke();
                ctx.beginPath();
                ctx.arc(ex, ey, 2, 0, Math.PI * 2);
                ctx.fill();
            }
        } 
        else if (type === 'flu') { // Influenza
            ctx.beginPath();
            ctx.arc(0, 0, p.size * 0.85, 0, Math.PI * 2);
            ctx.stroke();
            for (let a = 0; a < Math.PI * 2; a += Math.PI / 5) {
                let ex = Math.cos(a) * (p.size * 1.15);
                let ey = Math.sin(a) * (p.size * 1.15);
                ctx.fillRect(ex - 1.5, ey - 1.5, 3, 3);
            }
        } 
        else if (type === 'icosahedral') { // Rinovírus
            ctx.beginPath();
            for (let i = 0; i < 6; i++) {
                let angle = (i * Math.PI) / 3;
                let x = Math.cos(angle) * p.size;
                let y = Math.sin(angle) * p.size;
                if (i === 0) ctx.moveTo(x, y);
                else ctx.lineTo(x, y);
            }
            ctx.closePath();
            ctx.stroke();
        }

        ctx.restore();
    }

    // Atualização do Sistema Físico
    function update() {
        if (!isRunning) return;

        let particlesInBunkerCount = 0;

        for (let i = viralParticles.length - 1; i >= 0; i--) {
            let p = viralParticles[i];
            p.x += p.vx;
            p.y += p.vy;
            p.angle += p.spin;

            // Turbulência sutil
            p.vy += (Math.random() - 0.5) * 0.2;

            // INTERAÇÃO COM BARREIRA UVC
            if (uvcActive && p.x > 300 && p.x < 360) {
                p.integrity -= 3;
                if (Math.random() < 0.2) {
                    log("💥 Capsídeo desintegrado por irradiação UVC!");
                }
            }

            // INTERAÇÃO COM NEBULIZAÇÃO DE VHP
            if (vhpActive && p.x > 380 && p.x < 480) {
                p.integrity -= 4;
            }

            // INTERAÇÃO COM BARREIRA DE PLASMA
            if (plasmaActive && p.x > 480 && p.x < 500) {
                p.integrity -= 25; // Incineração quase instantânea
            }

            // INTERAÇÃO COM ESCOTILHA AIRLOCK BSL-4
            if (airlockActive && p.x >= AIRLOCK_X - 10) {
                p.vx = -Math.abs(p.vx) * 0.5; // Rebound na porta blindada
                p.x = AIRLOCK_X - 12;
            }

            // Destruição por integridade zero
            if (p.integrity <= 0) {
                viralParticles.splice(i, 1);
                continue;
            }

            // Infiltração no Bunker
            if (p.x > BUNKER_X - 50) {
                particlesInBunkerCount++;
            }

            // Saída da tela
            if (p.x > canvas.width + 50) {
                viralParticles.splice(i, 1);
            }
        }

        // Atualização contínua da carga viral interna
        if (particlesInBunkerCount > 0) {
            viralLoadInBunker = Math.min(100000, viralLoadInBunker + particlesInBunkerCount * 12);
        } else {
            viralLoadInBunker = Math.max(0, viralLoadInBunker - 2);
        }

        // Geração contínua se surto ativo
        if (outbreakActive && Math.random() < 0.25 && viralParticles.length < 80) {
            viralParticles.push(createParticle());
        }

        updateHUD();
    }

    // Desenho na Tela (Canvas 2D)
    function draw() {
        // Fundo
        ctx.fillStyle = currentVirus.bgBg;
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // Grade Médica / Bio-Laboratorial
        ctx.strokeStyle = currentVirus.color + '15';
        ctx.lineWidth = 1;
        for (let x = 0; x < canvas.width; x += 40) {
            ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
        }

        // CAMPO UVC GERMICIDA (Se ativo)
        if (uvcActive) {
            let grad = ctx.createLinearGradient(300, 0, 360, 0);
            grad.addColorStop(0, 'rgba(168, 85, 247, 0)');
            grad.addColorStop(0.5, 'rgba(168, 85, 247, 0.4)');
            grad.addColorStop(1, 'rgba(168, 85, 247, 0)');
            ctx.fillStyle = grad;
            ctx.fillRect(300, 0, 60, canvas.height);
            ctx.fillStyle = '#a855f7';
            ctx.font = '10px Consolas';
            ctx.fillText("FEIXE GERMICIDA UVC-C", 280, 20);
        }

        // MÁSCARA DE VHP - PERÓXIDO DE HIDROGÊNIO VAPORIZADO (Se ativo)
        if (vhpActive) {
            ctx.fillStyle = 'rgba(255, 255, 255, 0.08)';
            ctx.fillRect(380, 0, 100, canvas.height);
            ctx.fillStyle = '#ffffff';
            ctx.font = '10px Consolas';
            ctx.fillText("NEBULIZAÇÃO VHP", 385, 35);
        }

        // BARREIRA DE PLASMA (Se ativo)
        if (plasmaActive) {
            ctx.fillStyle = 'rgba(255, 0, 85, 0.6)';
            ctx.shadowColor = '#ff0055';
            ctx.shadowBlur = 15;
            ctx.fillRect(485, 0, 15, canvas.height);
            ctx.shadowBlur = 0;
            ctx.fillStyle = '#ff0055';
            ctx.font = '10px Consolas';
            ctx.fillText("PLASMA 1200°C", 460, 50);
        }

        // DESENHAR ESTRUTURA DO BUNKER BSL-4
        ctx.fillStyle = '#061a0e';
        ctx.fillRect(BUNKER_X - 50, BUNKER_Y, BUNKER_W, BUNKER_H);
        ctx.strokeStyle = currentVirus.color;
        ctx.lineWidth = 3;
        ctx.strokeRect(BUNKER_X - 50, BUNKER_Y, BUNKER_W, BUNKER_H);

        // ESCOTILHA HERMÉTICA AIRLOCK BSL-4
        ctx.fillStyle = airlockActive ? '#10b981' : '#ff0055';
        ctx.fillRect(AIRLOCK_X, BUNKER_Y, 20, BUNKER_H);
        ctx.fillStyle = '#ffffff';
        ctx.font = '10px Consolas';
        ctx.fillText("AIRLOCK BSL-4", AIRLOCK_X - 25, BUNKER_Y - 10);

        // RÓTULO DO LAB INTERNO
        ctx.fillStyle = '#ffffff';
        ctx.font = '11px Consolas';
        ctx.fillText("ÁREA LIMPA / REFUGO DE CONTENÇÃO BSL-4", BUNKER_X - 30, BUNKER_Y + 30);

        // INDICADOR DE CARGA VIRAL INTERNA DENTRO DO BUNKER
        ctx.fillStyle = viralLoadInBunker > 50 ? '#ff0055' : '#10b981';
        ctx.fillText(`NÍVEL DE CONTAMINAÇÃO: ${Math.round(viralLoadInBunker)} UI/mL`, BUNKER_X - 30, BUNKER_Y + 250);

        // DESENHAR PARTÍCULAS VIRAIS
        viralParticles.forEach(p => {
            drawVirusShape(ctx, p, currentVirus.morphType, currentVirus.color);
        });
    }

    // Loop de Animação Principal
    function loop() {
        update();
        draw();
        requestAnimationFrame(loop);
    }

    loop();
</script>
</body>
</html>
