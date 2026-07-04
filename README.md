<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Priorities - Das Ranking Duell</title>
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
            position: relative;
        }

        /* Top Bar Info */
        .scoreboard-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            background: rgba(255, 255, 255, 0.03);
            padding: 10px 16px;
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.05);
            font-size: 14px;
            font-weight: bold;
            display: none;
        }
        .score-badge { color: var(--accent-neon); }
        .target-badge { color: var(--accent-pink); }

        /* Screens */
        .screen { display: none; text-align: center; }
        .screen.active { display: block; animation: fadeIn 0.4s ease; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Schickes Menü Design */
        .logo-area {
            margin-bottom: 35px;
        }
        .logo-title {
            font-size: 42px;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 4px;
            background: linear-gradient(45deg, var(--accent-neon), var(--accent-purple), var(--accent-pink));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 20px rgba(0, 240, 255, 0.2);
        }
        .logo-subtitle {
            font-size: 13px;
            color: var(--text-muted);
            margin-top: 8px;
            letter-spacing: 1px;
        }

        .menu-card {
            background: rgba(255, 255, 255, 0.02);
            border: 1px solid rgba(255, 255, 255, 0.05);
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 25px;
            text-align: left;
        }
        .menu-card h3 { color: var(--accent-neon); margin-bottom: 10px; font-size: 16px; }
        .menu-card p { color: var(--text-muted); font-size: 14px; line-height: 1.5; }

        /* Inputs & Buttons */
        .input-group {
            margin-bottom: 20px;
            text-align: left;
        }
        .input-group label {
            display: block;
            font-size: 12px;
            text-transform: uppercase;
            color: var(--text-muted);
            margin-bottom: 6px;
            font-weight: bold;
        }
        .input-group input {
            width: 100%;
            background: #181826;
            border: 2px solid #2d2d44;
            padding: 12px;
            border-radius: 10px;
            color: white;
            font-size: 16px;
            font-weight: bold;
            outline: none;
            transition: 0.2s;
        }
        .input-group input:focus { border-color: var(--accent-purple); }

        .btn {
            background: linear-gradient(90deg, var(--accent-purple), var(--accent-pink));
            color: white;
            border: none;
            padding: 16px 28px;
            border-radius: 14px;
            font-size: 16px;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 1px;
            cursor: pointer;
            width: 100%;
            box-shadow: 0 4px 15px rgba(255, 0, 127, 0.3);
            transition: all 0.2s;
        }
        .btn:active { transform: scale(0.98); box-shadow: 0 2px 5px rgba(255, 0, 127, 0.2); }
        .btn-sec { background: #222235; box-shadow: none; border: 1px solid #33334c; }

        /* Listen & Karten */
        .instruction-text {
            font-size: 16px;
            color: var(--text-muted);
            margin-bottom: 24px;
            line-height: 1.5;
        }
        .highlight-name { color: var(--accent-neon); font-weight: bold; }

        .list-box {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 26px;
        }

        .ranking-row {
            display: flex;
            align-items: center;
            gap: 14px;
        }
        .num-indicator {
            font-size: 18px;
            font-weight: 900;
            color: var(--text-muted);
            width: 24px;
            text-align: center;
        }
        .card-item {
            background: linear-gradient(135deg, #1b1b2f, #141424);
            border: 1px solid #2c2c44;
            padding: 16px;
            border-radius: 12px;
            flex-grow: 1;
            font-weight: 600;
            font-size: 16px;
            text-align: left;
            cursor: grab;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            user-select: none;
        }
        .card-item.dragging { opacity: 0.4; border-color: var(--accent-neon); }
        .card-item.over { border: 1px dashed var(--accent-neon); background: #1f2d44; }

        /* Result View */
        .res-row {
            background: #151522;
            padding: 14px;
            border-radius: 10px;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-left: 4px solid var(--accent-pink);
        }
        .res-row.is-correct { border-left-color: var(--success); }
        .badge-pill {
            padding: 4px 10px;
            border-radius: 6px;
            font-size: 12px;
            font-weight: bold;
            background: #222236;
            color: var(--text-muted);
            margin-left: 6px;
        }
        .badge-pill.match { background: var(--success); color: #000; }

        .round-score-banner {
            font-size: 22px;
            font-weight: bold;
            color: var(--accent-neon);
            margin: 20px 0;
        }
    </style>
</head>
<body>

<div class="game-wrapper">
    <!-- Scoreboard oben (wird im Spiel eingeblendet) -->
    <div id="scoreboard" class="scoreboard-bar">
        <div>Runde: <span id="display-round">1</span></div>
        <div>Punkte: <span id="display-score" class="score-badge">0</span> / <span id="display-target" class="target-badge">15</span></div>
    </div>

    <!-- MAIN MENU -->
    <div id="scr-menu" class="screen active">
        <div class="logo-area">
            <h1 class="logo-title">Priorities</h1>
            <div class="logo-subtitle">DAS GAME DER ABSURDEN VERGLEICHE</div>
        </div>
        <div class="menu-card">
            <h3>Spielweise:</h3>
            <p>Spieler A bringt 5 zufällige Nomen heimlich in seine persönliche Reihenfolge. Die Gruppe versucht gemeinsam, genau dieses Ranking zu erraten! Für jeden richtigen Platz gibt es Punkte.</p>
        </div>
        <div class="input-group">
            <label>Name von Spieler A</label>
            <input type="text" id="input-player-name" value="Spieler A">
        </div>
        <div class="input-group">
            <label>Zielpunktzahl für den Sieg</label>
            <input type="number" id="input-target-score" value="15" min="5" max="50">
        </div>
        <button class="btn" onclick="initGame()">Match Starten</button>
    </div>

    <!-- SECRET RANKING -->
    <div id="scr-rank" class="screen">
        <p class="instruction-text">Handy an <span class="highlight-name" id="name-placeholder-1">Spieler A</span> übergeben!<br><small>Bringe die Nomen in deine persönliche Priorität (Oben = Top, Unten = Flop).</small></p>
        <div id="box-rank" class="list-box"></div>
        <button class="btn" onclick="saveSecretRanking()">Prioritäten Speichern</button>
    </div>

    <!-- HANDOVER -->
    <div id="scr-handover" class="screen">
        <div class="logo-area">
            <h2 class="logo-title" style="font-size:32px;">Gespeichert!</h2>
        </div>
        <p class="instruction-text">Gib das Handy jetzt zurück an <span class="highlight-name">die restliche Gruppe</span>.</p>
        <button class="btn" onclick="startGuessingPhase()">Gruppe übernimmt</button>
    </div>

    <!-- GUESS RANKING -->
    <div id="scr-guess" class="screen">
        <p class="instruction-text"><strong>Die Gruppe rät!</strong><br>Wie sieht die Rangliste von <span class="highlight-name" id="name-placeholder-2">Spieler A</span> aus?</p>
        <div id="box-guess" class="list-box"></div>
        <button class="btn" onclick="evaluateRound()">Ergebnis Auswerten</button>
    </div>

    <!-- ROUND RESULT -->
    <div id="scr-result" class="screen">
        <h2 style="margin-bottom: 15px; font-weight: 800;">Auswertung</h2>
        <div id="box-result"></div>
        <div class="round-score-banner" id="round-points-earned">+0 Punkte geholt!</div>
        <button class="btn" id="btn-next-round" onclick="nextRound()">Nächste Runde</button>
    </div>

    <!-- GAME OVER / WINNER -->
    <div id="scr-gameover" class="screen">
        <div class="logo-area">
            <h1 class="logo-title" style="font-size: 36px; color: var(--success);">Sieg!</h1>
        </div>
        <p class="instruction-text" style="font-size: 20px;">Ihr habt die Zielpunktzahl erreicht und das Spiel gewonnen!</p>
        <div class="round-score-banner" id="final-score-text" style="font-size: 28px;">Endstand: 15 Punkte</div>
        <button class="btn btn-sec" onclick="backToMenu()">Hauptmenü</button>
    </div>
</div>

<script>
    // Riesiger Nomen-Pool (Hauptsächlich Nomen)
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
        "Schaukelstuhl", "Schokoladenkuchen", "Hängematte", "Diskokugel", "Glühwürmchen",
        "Wolkenbruch", "Frühstücksei", "Klavierkonzert", "Zirkuszelt", "Sternschnuppe",
        "Gartenparty", "Turnschuh", "Liebesbrief", "Zuckerwatte", "Abenteuerlust"
    ];

    let gameData = {
        playerName: "Spieler A",
        currentScore: 0,
        targetScore: 15,
        currentRound: 1,
        roundWords: [],
        secretRanking: [],
        guessRanking: []
    };

    let dragElement = null;

    function shuffle(arr) { return arr.sort(() => Math.random() - 0.5); }

    function switchScreen(id) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.getElementById('scoreboard').style.display = (id === 'scr-menu' || id === 'scr-gameover') ? 'none' : 'flex';
    }

    function initGame() {
        gameData.playerName = document.getElementById('input-player-name').value || "Spieler A";
        gameData.targetScore = parseInt(document.getElementById('input-target-score').value) || 15;
        gameData.currentScore = 0;
        gameData.currentRound = 1;

        document.getElementById('display-target').innerText = gameData.targetScore;
        document.getElementById('name-placeholder-1').innerText = gameData.playerName;
        document.getElementById('name-placeholder-2').innerText = gameData.playerName;

        startNewRound();
    }

    function startNewRound() {
        document.getElementById('display-round').innerText = gameData.currentRound;
        document.getElementById('display-score').innerText = gameData.currentScore;
        
        // 5 zufällige Nomen ziehen
        let shuffledPool = shuffle([...nomenPool]);
        gameData.roundWords = shuffledPool.slice(0, 5);

        renderList("box-rank", [...gameData.roundWords]);
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
            
            // Desktop Drag & Drop
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
            
            // Mobile Touch Support
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

    function getListOrder(containerId) {
        return Array.from(document.getElementById(containerId).querySelectorAll('.card-item')).map(c => c.innerText);
    }

    function saveSecretRanking() {
        gameData.secretRanking = getListOrder("box-rank");
        switchScreen('scr-handover');
    }

    function startGuessingPhase() {
        // Für die Gruppe die Wörter noch mal durchwürfeln, damit sie raten müssen
        renderList("box-guess", shuffle([...gameData.roundWords]));
        switchScreen('scr-guess');
    }

    function evaluateRound() {
        gameData.guessRanking = getListOrder("box-guess");
        const container = document.getElementById("box-result");
        container.innerHTML = "";
        let pointsWon = 0;

        gameData.secretRanking.forEach((word, index) => {
            let correctPos = index + 1;
            let guessedPos = gameData.guessRanking.indexOf(word) + 1;
            let isMatch = (correctPos === guessedPos);
            
            if (isMatch) pointsWon++;

            let row = document.createElement("div");
            row.className = `res-row ${isMatch ? 'is-correct' : ''}`;
            row.innerHTML = `<span>${word}</span>
                <div>
                    <span class="badge-pill">${gameData.playerName}: ${correctPos}</span>
                    <span class="badge-pill ${isMatch ? 'match' : ''}">Team: ${guessedPos}</span>
                </div>`;
            container.appendChild(row);
        });

        gameData.currentScore += pointsWon;
        document.getElementById('round-points-earned').innerText = `+${pointsWon} Punkte geholt!`;
        document.getElementById('display-score').innerText = gameData.currentScore;

        if (gameData.currentScore >= gameData.targetScore) {
            document.getElementById('btn-next-round').innerText = "Zum Endstand";
        } else {
            document.getElementById('btn-next-round').innerText = "Nächste Runde";
        }
        switchScreen('scr-result');
    }

    function nextRound() {
        if (gameData.currentScore >= gameData.targetScore) {
            document.getElementById('final-score-text').innerText = `Endstand: ${gameData.currentScore} Punkte (Ziel: ${gameData.targetScore})`;
            switchScreen('scr-gameover');
        } else {
            gameData.currentRound++;
            startNewRound();
        }
    }

    function backToMenu() {
        switchScreen('scr-menu');
    }
</script>
</body>
</html>
