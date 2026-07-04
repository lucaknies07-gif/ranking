<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Priorities">
    
    <title>Priorities - Das Gruppen Duell</title>
    <style>
        :root {
            --bg-main: #0a0a0f;
            --bg-card: #12121c;
            --accent-neon: #00f0ff;
            --accent-purple: #9d00ff;
            --accent-pink: #ff007f;
            --text-main: #ffffff;
            --text-muted: #8fa0dd;
            --success: #39ff14;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 16px;
            overflow-x: hidden;
        }

        .game-wrapper {
            width: 100%;
            max-width: 450px;
            background: linear-gradient(145deg, #141423, #0b0b13);
            border: 2px solid #1f1f38;
            border-radius: 24px;
            padding: 24px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.6), 0 0 30px rgba(157, 0, 255, 0.1);
        }

        /* Screens */
        .screen { display: none; text-align: center; }
        .screen.active { display: block; animation: fadeIn 0.3s ease; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Logo & Header */
        .logo-area { margin-bottom: 25px; }
        .logo-title {
            font-size: 38px;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 3px;
            background: linear-gradient(45deg, var(--accent-neon), var(--accent-purple), var(--accent-pink));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .logo-subtitle { font-size: 12px; color: var(--text-muted); margin-top: 6px; }

        /* Spieler-Eingabe-Liste */
        .player-input-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
            max-height: 200px;
            overflow-y: auto;
            padding-right: 4px;
        }
        .player-row {
            display: flex;
            gap: 10px;
        }
        input[type="text"] {
            width: 100%;
            background: #181826;
            border: 2px solid #2d2d44;
            padding: 12px;
            border-radius: 10px;
            color: white;
            font-size: 15px;
            font-weight: bold;
            outline: none;
        }
        input[type="text"]:focus { border-color: var(--accent-neon); }

        .btn-small {
            background: #222235;
            color: white;
            border: 1px solid #33334c;
            padding: 10px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 14px;
        }

        .btn {
            background: linear-gradient(90deg, var(--accent-purple), var(--accent-pink));
            color: white;
            border: none;
            padding: 16px 28px;
            border-radius: 14px;
            font-size: 16px;
            font-weight: bold;
            text-transform: uppercase;
            cursor: pointer;
            width: 100%;
            box-shadow: 0 4px 15px rgba(255, 0, 127, 0.3);
            margin-top: 10px;
        }

        /* Listen & Drag-Karten */
        .instruction-text { font-size: 16px; color: var(--text-muted); margin-bottom: 20px; line-height: 1.5; }
        .highlight-name { color: var(--accent-neon); font-weight: bold; font-size: 20px; display: block; margin-top: 5px; }

        .list-box { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
        .ranking-row { display: flex; align-items: center; gap: 12px; }
        .num-indicator { font-size: 18px; font-weight: 900; color: var(--text-muted); width: 20px; }
        
        .card-item {
            background: linear-gradient(135deg, #1b1b2f, #141424);
            border: 1px solid #2c2c44;
            padding: 14px;
            border-radius: 12px;
            flex-grow: 1;
            font-weight: 600;
            font-size: 16px;
            text-align: left;
            cursor: grab;
        }
        .card-item.dragging { opacity: 0.4; }
        .card-item.over { border: 1px dashed var(--accent-neon); }

        /* Result View */
        .res-row {
            background: #151522;
            padding: 12px;
            border-radius: 10px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-left: 4px solid var(--accent-pink);
        }
        .res-row.is-correct { border-left-color: var(--success); }
        .badge-pill { padding: 4px 8px; border-radius: 6px; font-size: 12px; font-weight: bold; background: #222236; color: var(--text-muted); margin-left: 4px; }
        .badge-pill.match { background: var(--success); color: #000; }

        /* Rangliste / Tabelle */
        .leaderboard-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            background: rgba(255, 255, 255, 0.02);
            border-radius: 12px;
            overflow: hidden;
        }
        .leaderboard-table th, .leaderboard-table td {
            padding: 12px 16px;
            text-align: left;
        }
        .leaderboard-table th { background: #1c1c30; color: var(--accent-neon); font-size: 12px; text-transform: uppercase; }
        .leaderboard-table tr { border-bottom: 1px solid #1f1f35; }
        .leaderboard-table tr:last-child { border-bottom: none; }
        .rank-num { font-weight: bold; color: var(--accent-pink); }
    </style>
</head>
<body>

<div class="game-wrapper">

    <div id="scr-menu" class="screen active">
        <div class="logo-area">
            <h1 class="logo-title">Priorities</h1>
            <div class="logo-subtitle">DAS KREIS-DUELL DER REIHENFOLGEN</div>
        </div>
        <p class="instruction-text" style="font-size: 14px;">Trage alle Spieler ein. Jede Runde wechselt der Befragte im Kreis!</p>
        
        <div id="player-inputs" class="player-input-list">
            <div class="player-row"><input type="text" class="player-name-field" value="Spieler 1"></div>
            <div class="player-row"><input type="text" class="player-name-field" value="Spieler 2"></div>
        </div>
        
        <div style="display: flex; gap: 10px; margin-bottom: 20px;">
            <button class="btn-small" style="flex: 1;" onclick="addPlayerField()">+ Spieler</button>
            <button class="btn-small" style="flex: 1;" onclick="removePlayerField()">- Entfernen</button>
        </div>
        
        <button class="btn" onclick="setupGame()">Spiel Starten</button>
    </div>

    <div id="scr-rank" class="screen">
        <p class="instruction-text">Niemand hingucken! Handy abgeben an: <span class="highlight-name" id="current-giver-name">Spieler X</span></p>
        <p class="instruction-text" style="font-size: 13px; margin-top: -10px;">Sortiere die Begriffe nach deinen Vorlieben (1 = Top, 5 = Flop).</p>
        <div id="box-rank" class="list-box"></div>
        <button class="btn" onclick="saveSecretRanking()">Reihenfolge einloggen</button>
    </div>

    <div id="scr-handover" class="screen">
        <div class="logo-area"><h2 class="logo-title" style="font-size:30px;">Gespeichert!</h2></div>
        <p class="instruction-text">Gib das Handy jetzt zurück an die **gesamte Gruppe** (inklusive dem Befragten!).</p>
        <button class="btn" onclick="startGuessingPhase()">Gruppe ist bereit</button>
    </div>

    <div id="scr-guess" class="screen">
        <p class="instruction-text"><strong>Die Gruppe rät gemeinsam!</strong><br>Wie hat <span class="highlight-name" id="guess-target-name">Spieler X</span> gerankt?</p>
        <div id="box-guess" class="list-box"></div>
        <button class="btn" onclick="evaluateRound()">Auflösen & Werten</button>
    </div>

    <div id="scr-result" class="screen">
        <h2 style="margin-bottom: 10px; font-weight: 800;">Vergleich</h2>
        <div id="box-result"></div>
        <p class="instruction-text" id="points-summary" style="margin: 15px 0 5px; color: var(--accent-neon); font-weight: bold;"></p>
        <button class="btn" onclick="showLeaderboard()">Zur Punkte-Tabelle</button>
    </div>

    <div id="scr-leaderboard" class="screen">
        <div class="logo-area"><h2 class="logo-title" style="font-size:28px;">Zwischenstand</h2></div>
        <p class="instruction-text" style="font-size: 13px;">Alle (inkl. Befragter) bekommen 1 Punkt pro Treffer!</p>
        
        <table class="leaderboard-table">
            <thead>
                <tr>
                    <th style="width: 50px;">Platz</th>
                    <th>Spieler</th>
                    <th style="text-align: right;">Punkte</th>
                </tr>
            </thead>
            <tbody id="leaderboard-body"></tbody>
        </table>
        
        <button class="btn" onclick="advanceTurn()">Nächster Spieler</button>
    </div>
</div>

<script>
    const nomenPool = [
        "Käsebrot", "Lottogewinn", "Zahnarztbesuch", "Dönerknoblauch", "Sockenloch", 
        "Strandurlaub", "Montagmorgen", "Kaffeevollautomat", "Steuererklärung", "Hundewelpe",
        "WLAN-Ausfall", "Regenschauer", "Kaltgetränk", "Strafzettel", "Mückenstich",
        "Schokopudding", "Weckergeräusch", "Sofa-Nachmittag", "Fitnessstudio", "Kinofilm",
        "Traumauto", "Zahnseide", "Lieblingssong", "Dauerregen", "Bananenschale",
        "Taschengeld", "Mittagsschlaf", "Spargelzeit", "Kühlschrankleere", "Verkehrsstau",
        "Katzenvideo", "Gewitterblitz", "Pizzaabend", "Kabeldrecksalat", "Sommerhitze",
        "Kuscheldecke", "Eierkocher", "Meeresrauschen", "Wollpullover", "Taschenlampe",
        "Bratwurstbude", "Sonnenbrand", "Heizungsausfall", "Glücksgefühle", "Gummibärchen",
        "Schlüsselverlust", "Kopfkissen", "Achterbahnfahrt", "Taschentuch", "Flohmarkt",
        "Baustellenlärm", "Feierabend", "Heißluftballon", "Popcornmaschine", "Regenbogen",
        "Müllabfuhr", "Kekskrümel", "Schneeballschlacht", "Campingzelt", "Dschungelreise",
        "Einkaufswagen", "Luftmatratze", "Geburtstagstorte", "Lagerfeuer", "Seifenblase",
        "Schaukelstuhl", "Schokoladenkuchen", "Hängematte", "Diskogugel", "Glühwürmchen",
        "Wolkenbruch", "Frühstücksei", "Klavierkonzert", "Zirkuszelt", "Sternschnuppe",
        "Gartenparty", "Turnschuh", "Liebesbrief", "Zuckerwatte", "Abenteuerlust"
    ];

    let players = []; 
    let activePlayerIndex = 0;
    let roundWords = [];
    let secretRanking = [];
    let guessRanking = [];
    let dragElement = null;

    function shuffle(arr) { return arr.sort(() => Math.random() - 0.5); }

    function addPlayerField() {
        const container = document.getElementById('player-inputs');
        const count = container.children.length + 1;
        const row = document.createElement('div');
        row.className = 'player-row';
        row.innerHTML = `<input type="text" class="player-name-field" value="Spieler ${count}">`;
        container.appendChild(row);
    }

    function removePlayerField() {
        const container = document.getElementById('player-inputs');
        if (container.children.length > 2) {
            container.removeChild(container.lastChild);
        }
    }

    function switchScreen(id) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
    }

    function setupGame() {
        players = [];
        const fields = document.querySelectorAll('.player-name-field');
        fields.forEach(f => {
            let name = f.value.trim();
            if (name) players.push({ name: name, score: 0 });
        });

        if (players.length < 2) {
            alert("Bitte trage mindestens 2 Spieler ein!");
            return;
        }

        activePlayerIndex = 0;
        startPlayerTurn();
    }

    function startPlayerTurn() {
        let currentGiver = players[activePlayerIndex].name;
        document.getElementById('current-giver-name').innerText = currentGiver;
        document.getElementById('guess-target-name').innerText = currentGiver;

        let shuffled = shuffle([...nomenPool]);
        roundWords = shuffled.slice(0, 5);

        renderList("box-rank", [...roundWords]);
        switchScreen('scr-rank');
    }

    function renderList(containerId, words) {
        const container = document.getElementById(containerId);
        container.innerHTML = "";
        
        words.forEach((word, index) => {
            const row = document.createElement("div");
            row.className = "ranking-row";
            row.innerHTML = `<div class="num-indicator">${index + 1}</div>`;
            
            const card = document.createElement("div");
            card.className = "card-item";
            card.innerText = word;
            card.setAttribute("draggable", "true");
            
            // Drag Drop
            card.addEventListener('dragstart', function() { this.classList.add('dragging'); dragElement = this; });
            card.addEventListener('dragover', function(e) { e.preventDefault(); this.classList.add('over'); });
            card.addEventListener('dragleave', function() { this.classList.remove('over'); });
            card.addEventListener('drop', function(e) {
                e.stopPropagation();
                if (dragElement !== this) {
                    let tmp = this.innerText; this.innerText = dragElement.innerText; dragElement.innerText = tmp;
                }
            });
            card.addEventListener('dragend', function() { document.querySelectorAll('.card-item').forEach(c => c.classList.remove('over', 'dragging')); });
            
            // Touch Logik
            card.addEventListener('touchstart', function() { this.classList.add('dragging'); }, {passive: true});
            card.addEventListener('touchmove', function(e) {
                e.preventDefault();
                let t = e.touches[0];
                let target = document.elementFromPoint(t.clientX, t.clientY);
                document.querySelectorAll('.card-item').forEach(c => c.classList.remove('over'));
                if (target && target.classList.contains('card-item') && target !== this) target.classList.add('over');
            }, {passive: false});
            card.addEventListener('touchend', function(e) {
                this.classList.remove('dragging');
                let t = e.changedTouches[0];
                let target = document.elementFromPoint(t.clientX, t.clientY);
                if (target && target.classList.contains('card-item') && target !== this) {
                    let tmp = target.innerText; target.innerText = this.innerText; this.innerText = tmp;
                }
                document.querySelectorAll('.card-item').forEach(c => c.classList.remove('over'));
            });

            row.appendChild(card);
            container.appendChild(row);
        });
    }

    function saveSecretRanking() {
        secretRanking = Array.from(document.getElementById("box-rank").querySelectorAll('.card-item')).map(c => c.innerText);
        switchScreen('scr-handover');
    }

    function startGuessingPhase() {
        renderList("box-guess", shuffle([...roundWords]));
        switchScreen('scr-guess');
    }

    function evaluateRound() {
        guessRanking = Array.from(document.getElementById("box-guess").querySelectorAll('.card-item')).map(c => c.innerText);
        const container = document.getElementById("box-result");
        container.innerHTML = "";
        let hits = 0;

        let giverName = players[activePlayerIndex].name;

        secretRanking.forEach((word, index) => {
            let correctPos = index + 1;
            let guessedPos = guessRanking.indexOf(word) + 1;
            let isMatch = (correctPos === guessedPos);
            if (isMatch) hits++;

            let row = document.createElement("div");
            row.className = `res-row ${isMatch ? 'is-correct' : ''}`;
            row.innerHTML = `<span>${word}</span>
                <div>
                    <span class="badge-pill">${giverName}: ${correctPos}</span>
                    <span class="badge-pill ${isMatch ? 'match' : ''}">Gruppe: ${guessedPos}</span>
                </div>`;
            container.appendChild(row);
        });

        // Punkte-Verteilung: Alle aktiven Spieler bekommen die Punkte auf ihr Konto!
        players.forEach(p => {
            p.score += hits;
        });

        document.getElementById('points-summary').innerText = `Volltreffer: ${hits} / 5 richtig! +${hits} Punkte für JEDEN!`;
        switchScreen('scr-result');
    }

    function showLeaderboard() {
        // Sortiere Spieler nach Punkten für die Tabelle (höchste zuerst)
        let sortedPlayers = [...players].sort((a,b) => b.score - a.score);
        const tbody = document.getElementById('leaderboard-body');
        tbody.innerHTML = "";

        sortedPlayers.forEach((p, idx) => {
            let tr = document.createElement('tr');
            tr.innerHTML = `
                <td class="rank-num">#${idx + 1}</td>
                <td style="font-weight:600;">${p.name} ${p.name === players[activePlayerIndex].name ? '👤' : ''}</td>
                <td style="text-align:right; font-weight:bold; color:var(--accent-neon);">${p.score}</td>
            `;
            tbody.appendChild(tr);
        });

        switchScreen('scr-leaderboard');
    }

    function advanceTurn() {
        // Nächster Spieler im Kreis
        activePlayerIndex = (activePlayerIndex + 1) % players.length;
        startPlayerTurn();
    }
</script>
</body>
</html>
