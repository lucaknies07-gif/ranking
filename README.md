<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ranking Party-Spiel</title>
    <style>
        :root {
            --bg-color: #121214;
            --card-bg: #1e1e24;
            --accent-color: #6200ee;
            --accent-hover: #7722ff;
            --text-color: #ffffff;
            --text-muted: #a0a0a5;
            --success: #03dac6;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            padding: 16px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 500px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 10px;
        }

        h1 { font-size: 26px; color: var(--text-color); margin-bottom: 4px; }
        .subtitle { font-size: 14px; color: var(--text-muted); }

        .screen {
            background-color: var(--card-bg);
            border-radius: 16px;
            padding: 20px;
            box-shadow: 0 8px 24px rgba(0,0,0,0.3);
            display: none;
        }

        .screen.active { display: block; }

        .instruction {
            font-size: 16px;
            margin-bottom: 20px;
            line-height: 1.5;
            text-align: center;
        }

        .important { color: var(--success); font-weight: bold; }

        .list-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 24px;
        }

        .rank-row {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .number-badge {
            background-color: var(--accent-color);
            color: white;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            flex-shrink: 0;
        }

        .item-card {
            background-color: #2a2a32;
            padding: 14px 16px;
            border-radius: 8px;
            flex-grow: 1;
            cursor: grab;
            border: 2px solid transparent;
            user-select: none;
            font-weight: 500;
        }

        .item-card.dragging { opacity: 0.5; background-color: #3a3a45; }
        .item-card.over { border-color: var(--success); }

        button {
            background-color: var(--accent-color);
            color: white;
            border: none;
            padding: 14px 20px;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }

        .result-comparison {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }

        .result-row {
            background-color: #2a2a32;
            padding: 12px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .result-row.correct { border-left: 5px solid var(--success); }
        .result-row.wrong { border-left: 5px solid #ff5555; }
        .badge-container { display: flex; gap: 6px; }

        .small-badge {
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: bold;
        }

        .badge-target { background-color: var(--accent-color); color: white; }
        .badge-guess { background-color: #444; color: white; }
        .badge-guess.correct-bg { background-color: var(--success); color: #121214; }
        .score-display { font-size: 24px; font-weight: bold; text-align: center; margin: 20px 0; color: var(--success); }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>Concept Match</h1>
        <div class="subtitle">Das unlogische Ranking-Spiel am selben Handy</div>
    </header>

    <div id="screen-start" class="screen active">
        <p class="instruction">Ein Party-Spiel für eine Gruppe an einem einzigen Smartphone.</p>
        <button onclick="startNewGame()">Spiel starten</button>
    </div>

    <div id="screen-rank" class="screen">
        <p class="instruction">Übergib das Handy an <span class="important">Spieler A</span>.<br>Niemand sonst darf hingucken!</p>
        <p class="instruction" style="font-size: 14px; color: var(--text-muted);">Bringe die 5 Begriffe in DEINE persönliche Reihenfolge (1 = Top, 5 = Flop).</p>
        <div id="rank-list" class="list-container"></div>
        <button onclick="submitSecretRanking()">Ranking speichern</button>
    </div>

    <div id="screen-handover" class="screen">
        <p class="instruction">Dein Ranking ist gespeichert!</p>
        <p class="instruction">Übergib das Handy jetzt an <span class="important">die restliche Gruppe</span>.</p>
        <button onclick="startGuessing()">Gruppe ist bereit!</button>
    </div>

    <div id="screen-guess" class="screen">
        <p class="instruction"><strong>Die Gruppe rät!</strong><br>Wie hat Spieler A diese Begriffe wohl gerankt?</p>
        <div id="guess-list" class="list-container"></div>
        <button onclick="submitGuess()">Ergebnis auflösen</button>
    </div>

    <div id="screen-result" class="screen">
        <h2 style="text-align: center; margin-bottom: 15px;">Auflösung</h2>
        <div id="result-container" class="result-comparison"></div>
        <div id="score-text" class="score-display">0 / 5 Richtige!</div>
        <button onclick="startNewGame()">Nächste Runde</button>
    </div>
</div>

<script>
    // Du kannst diese Liste beliebig mit deinen eigenen Begriffen erweitern!
    const begriffsPool = [
        "Zähneputzen", "Steuererklärung", "Urlaub am Strand", "Montagmorgen", 
        "Döner mit allem", "Ein Einhorn reiten", "Kalt duschen", "Stau auf der Autobahn", 
        "Socken mit Löchern", "Hauptgewinn im Lotto", "Katzenvideos", "Zahnarztbesuch",
        "Kaffee am Morgen", "WLAN-Ausfall", "Frisch bezogenes Bett", "Hausarbeit machen",
        "Hundewelpen", "Spargel schälen", "Eine Nacht durchmachen", "Sich stoßen am Zeh"
    ];

    let aktuelleBegriffe = [], geheimnisRanking = [], gruppenRanking = [];

    function shuffle(array) { return array.sort(() => Math.random() - 0.5); }

    function zeigeScreen(screenId) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(screenId).classList.add('active');
    }

    function startNewGame() {
        const gemischt = shuffle([...begriffsPool]);
        aktuelleBegriffe = gemischt.slice(0, 5);
        baueListe("rank-list", [...aktuelleBegriffe]);
        zeigeScreen("screen-rank");
    }

    function baueListe(containerId, begriffe) {
        const container = document.getElementById(containerId);
        container.innerHTML = "";
        
        begriffe.forEach((begriff, index) => {
            const row = document.createElement("div");
            row.className = "rank-row";
            row.innerHTML = `<div class="number-badge">${index + 1}</div>`;
            
            const card = document.createElement("div");
            card.className = "item-card";
            card.innerText = begriff;
            card.setAttribute("draggable", "true");
            
            // Drag and Drop (PC)
            card.addEventListener('dragstart', function() { this.classList.add('dragging'); dragSrcElement = this; });
            card.addEventListener('dragover', function(e) { e.preventDefault(); this.classList.add('over'); });
            card.addEventListener('dragleave', function() { this.classList.remove('over'); });
            card.addEventListener('drop', function(e) {
                e.stopPropagation();
                if (dragSrcElement !== this) {
                    let tmp = this.innerText; this.innerText = dragSrcElement.innerText; dragSrcElement.innerText = tmp;
                }
            });
            card.addEventListener('dragend', function() { document.querySelectorAll('.item-card').forEach(c => c.classList.remove('over', 'dragging')); });
            
            // Touch-Logik für Smartphones (Finger-Verschieben)
            card.addEventListener('touchstart', function() { this.classList.add('dragging'); }, {passive: true});
            card.addEventListener('touchmove', function(e) {
                e.preventDefault();
                let touch = e.touches[0];
                let overEl = document.elementFromPoint(touch.clientX, touch.clientY);
                document.querySelectorAll('.item-card').forEach(c => c.classList.remove('over'));
                if (overEl && overEl.classList.contains('item-card') && overEl !== this) overEl.classList.add('over');
            }, {passive: false});
            card.addEventListener('touchend', function(e) {
                this.classList.remove('dragging');
                let touch = e.changedTouches[0];
                let overEl = document.elementFromPoint(touch.clientX, touch.clientY);
                if (overEl && overEl.classList.contains('item-card') && overEl !== this) {
                    let tmp = overEl.innerText; overEl.innerText = this.innerText; this.innerText = tmp;
                }
                document.querySelectorAll('.item-card').forEach(c => c.classList.remove('over'));
            });

            row.appendChild(card);
            container.appendChild(row);
        });
    }

    let dragSrcElement = null;

    function getAktuelleReihenfolge(containerId) {
        return Array.from(document.getElementById(containerId).querySelectorAll('.item-card')).map(c => c.innerText);
    }

    function submitSecretRanking() {
        geheimnisRanking = getAktuelleReihenfolge("rank-list");
        zeigeScreen("screen-handover");
    }

    function startGuessing() {
        baueListe("guess-list", shuffle([...aktuelleBegriffe]));
        zeigeScreen("screen-guess");
    }

    function submitGuess() {
        gruppenRanking = getAktuelleReihenfolge("guess-list");
        const container = document.getElementById("result-container");
        container.innerHTML = "";
        let punkte = 0;

        geheimnisRanking.forEach((begriff, index) => {
            let korrekterPlatz = index + 1;
            let gruppenPlatz = gruppenRanking.indexOf(begriff) + 1;
            let istRichtig = (korrekterPlatz === gruppenPlatz);
            if (istRichtig) punkte++;

            let row = document.createElement("div");
            row.className = `result-row ${istRichtig ? 'correct' : 'wrong'}`;
            row.innerHTML = `<span>${begriff}</span>
                <div class="badge-container">
                    <span class="small-badge badge-target">A: ${korrekterPlatz}</span>
                    <span class="small-badge badge-guess ${istRichtig ? 'correct-bg' : ''}">Team: ${gruppenPlatz}</span>
                </div>`;
            container.appendChild(row);
        });

        document.getElementById("score-text").innerText = `${punkte} / 5 Richtige!`;
        zeigeScreen("screen-result");
    }
</script>
</body>
</html>
