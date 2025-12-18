<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Exercise: Anatomy & Professional Life</title>
    <style>
        :root {
            --primary-color: #008080;
            --secondary-color: #20b2aa;
            --accent-color: #3cb371;
            --light-bg: #f0fff0;
            --dark-text: #333;
            --success-color: #28a745;
            --error-color: #dc3545;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0; padding: 0;
            background-color: var(--light-bg);
            color: var(--dark-text);
            line-height: 1.6;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            padding: 15px 20px;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
        }

        .score-display {
            position: absolute;
            top: 15px; right: 20px;
            font-weight: bold;
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px 10px; border-radius: 5px;
        }

        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .nav-buttons {
            display: flex; justify-content: center;
            gap: 10px; margin-bottom: 20px; flex-wrap: wrap; 
        }

        .nav-button {
            padding: 10px 15px; border: none; border-radius: 8px;
            cursor: pointer; background-color: var(--secondary-color);
            color: white; font-weight: bold; flex-grow: 1; min-width: 150px;
        }

        .nav-button.active { background-color: var(--primary-color); }

        .exercise-section {
            background-color: white; padding: 25px;
            border-radius: 12px; box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }

        h2 { color: var(--primary-color); border-bottom: 3px solid var(--secondary-color); padding-bottom: 10px; }

        .vocab-item { display: flex; padding: 15px 0; border-bottom: 1px dashed #ccc; align-items: center; }
        .word-col { flex: 0 0 200px; font-size: 1.2em; font-weight: bold; color: var(--primary-color); display: flex; align-items: center; gap: 10px; }
        .details-col { flex: 1; }

        .word-bank { display: flex; flex-wrap: wrap; gap: 10px; padding: 15px; margin-bottom: 20px; border-radius: 8px; background: linear-gradient(45deg, var(--secondary-color), var(--accent-color)); }
        .word-bank-item { background: white; padding: 5px 10px; border-radius: 5px; font-weight: bold; }

        .gap-sentence { margin-bottom: 15px; padding: 10px; border-left: 5px solid var(--secondary-color); background: #f9f9f9; }
        .gap-input { width: 140px; padding: 5px; border: 2px solid #ccc; border-radius: 4px; text-align: center; }

        .matching-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .match-item { padding: 12px; border: 2px solid #ddd; border-radius: 8px; cursor: pointer; }
        .matched { background-color: var(--success-color) !important; color: white !important; }

        .mic-btn { background: var(--primary-color); color: white; border: none; padding: 8px; border-radius: 50%; cursor: pointer; }
        .speaker-btn { background: var(--secondary-color); color: white; border: none; padding: 5px 8px; border-radius: 5px; cursor: pointer; font-size: 0.8em; }
    </style>
</head>
<body>

    <header>
        <h1>English Vocabulary Practice</h1>
        <div class="score-display">Score: <span id="current-score">0</span> / 24</div>
    </header>

    <div class="container">
        <div class="nav-buttons">
            <button class="nav-button active" onclick="showSection('vocab-list-section', this)">1. 📚 Vocabulary List</button>
            <button class="nav-button" onclick="showSection('writing-gaps-section', this)">2. ✍️ Fill in Gaps</button>
            <button class="nav-button" onclick="showSection('matching-section', this)">3. 🔗 Match Definitions</button>
            <button class="nav-button" onclick="showSection('pronunciation-section', this)">4. 🗣️ Pronunciation</button>
        </div>

        <div id="vocab-list-section" class="exercise-section">
            <h2>📚 Vocabulary List</h2>
            <div id="vocab-list-content"></div>
        </div>

        <div id="writing-gaps-section" class="exercise-section" style="display: none;">
            <h2>✍️ Fill in the Gaps (Writing)</h2>
            <div class="word-bank" id="writing-word-bank"></div>
            <div id="writing-gaps-content"></div>
            <button onclick="checkWritingGaps()" style="background: var(--success-color); color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer;">Check Answers</button>
        </div>

        <div id="matching-section" class="exercise-section" style="display: none;">
            <h2>🔗 Match Definitions</h2>
            <div class="matching-grid" id="matching-grid"></div>
        </div>

        <div id="pronunciation-section" class="exercise-section" style="display: none;">
            <h2>🗣️ Pronunciation Practice</h2>
            <p><em>Use the word bank below to guess the missing word for each sentence.</em></p>
            <div class="word-bank" id="pron-word-bank"></div>
            <div id="pronunciation-content"></div>
        </div>
    </div>

    <script>
        const vocabData = [
            {
                word: "confident",
                meaning: "Feeling sure of oneself and one's abilities.",
                example: "She felt **confident** about her presentation.",
                writingSentence: "To succeed, you need to appear **___** and professional.",
                definition: "Being certain of your own abilities.",
                pronSentence: "He is a very **___** speaker."
            },
            {
                word: "freelancer",
                meaning: "A person who works independently for different companies.",
                example: "As a **freelancer**, he chooses his own hours.",
                writingSentence: "Working as a **___** requires excellent time-management.",
                definition: "A person who is self-employed and not committed to one employer.",
                pronSentence: "She has been a successful **___** for years."
            },
            {
                word: "enormous",
                meaning: "Extremely large in size or amount.",
                example: "They live in an **enormous** house.",
                writingSentence: "The project was an **___** undertaking.",
                definition: "Very large in size, quantity, or extent.",
                pronSentence: "That is an **___** elephant!"
            },
            {
                word: "share",
                meaning: "To give a portion of something to others.",
                example: "It is polite to **share** your snacks.",
                writingSentence: "We decided to **___** the costs of the trip.",
                definition: "To divide and distribute something.",
                pronSentence: "Please **___** the data with the team."
            },
            {
                word: "elbow",
                meaning: "The joint between the upper and lower parts of the arm.",
                example: "I hit my **elbow** on the table.",
                writingSentence: "He wore a brace on his **___** to protect the joint.",
                definition: "The joint bending the arm.",
                pronSentence: "Rest your **___** on the armrest."
            },
            {
                word: "wrist",
                meaning: "The joint connecting the hand with the forearm.",
                example: "He wears his watch on his left **wrist**.",
                writingSentence: "Typing for too long can cause pain in your **___**.",
                definition: "The joint between the hand and the arm.",
                pronSentence: "She sprained her **___** while playing tennis."
            },
            {
                word: "thumb",
                meaning: "The short, thick first digit of the human hand.",
                example: "He gave a 'thumbs up' to say okay.",
                writingSentence: "The **___** is essential for gripping objects.",
                definition: "The first digit of the hand, set apart from fingers.",
                pronSentence: "Keep your **___** out of the way."
            },
            {
                word: "aunt",
                meaning: "The sister of one's father or mother.",
                example: "My **aunt** lives in London.",
                writingSentence: "I am going to visit my **___** this weekend.",
                definition: "The sister of your mother or father.",
                pronSentence: "My favorite **___** is visiting us."
            }
        ];

        let writingResults = new Array(vocabData.length).fill(false);
        let matchResults = new Array(vocabData.length).fill(false);
        let pronResults = new Array(vocabData.length).fill(false);

        function shuffle(array) {
            return array.sort(() => Math.random() - 0.5);
        }

        function speakWord(text) {
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'en-US';
            window.speechSynthesis.speak(utterance);
        }

        function showSection(id, btn) {
            document.querySelectorAll('.exercise-section').forEach(s => s.style.display = 'none');
            document.getElementById(id).style.display = 'block';
            document.querySelectorAll('.nav-button').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }

        function setup() {
            // Vocab List
            const list = document.getElementById('vocab-list-content');
            list.innerHTML = vocabData.map(i => `
                <div class="vocab-item">
                    <div class="word-col">
                        ${i.word} 
                        <button class="speaker-btn" onclick="speakWord('${i.word}')">🔊 Listen</button>
                    </div>
                    <div class="details-col">
                        <p>${i.meaning}<br><small>Example: ${i.example}</small></p>
                    </div>
                </div>
            `).join('');

            // Word Banks (Shuffled)
            const shuffledWords = shuffle(vocabData.map(i => i.word));
            const bankHTML = shuffledWords.map(w => `<div class="word-bank-item">${w}</div>`).join('');
            document.getElementById('writing-word-bank').innerHTML = bankHTML;
            document.getElementById('pron-word-bank').innerHTML = bankHTML;

            // Writing Content
            const gaps = document.getElementById('writing-gaps-content');
            gaps.innerHTML = vocabData.map((i, idx) => {
                const parts = i.writingSentence.split('___');
                return `<div class="gap-sentence">${parts[0]}<input type="text" class="gap-input" data-idx="${idx}" data-ans="${i.word.toLowerCase()}">${parts[1]}<span id="fb-${idx}"></span></div>`;
            }).join('');

            // Matching Grid
            const grid = document.getElementById('matching-grid');
            const words = vocabData.map((i, idx) => ({t: i.word, id: idx, type: 'w'}));
            const defs = vocabData.map((i, idx) => ({t: i.definition, id: idx, type: 'd'}));
            const items = shuffle([...words, ...defs]);
            grid.innerHTML = items.map(i => `<div class="match-item" data-id="${i.id}" data-type="${i.type}" onclick="handleMatch(this)">${i.t}</div>`).join('');

            // Pronunciation Content (Hidden word clue)
            const pron = document.getElementById('pronunciation-content');
            pron.innerHTML = vocabData.map((i, idx) => `
                <div class="gap-sentence">
                    ${i.pronSentence.replace(i.word, '_____')} 
                    <button class="mic-btn" onclick="testPron('${i.word}', ${idx})">🎤</button>
                    <span id="pron-fb-${idx}" style="color: #666; font-size: 0.9em; margin-left: 10px;">Guess the word and speak!</span>
                </div>
            `).join('');
        }

        function checkWritingGaps() {
            document.querySelectorAll('.gap-input').forEach(input => {
                const idx = input.dataset.idx;
                if (input.value.trim().toLowerCase() === input.dataset.ans) {
                    writingResults[idx] = true;
                    document.getElementById(`fb-${idx}`).textContent = " ✅";
                } else {
                    document.getElementById(`fb-${idx}`).textContent = " ❌";
                }
            });
            updateScore();
        }

        let selected = null;
        function handleMatch(el) {
            if (el.classList.contains('matched')) return;
            if (!selected) {
                selected = el; el.style.background = '#c8e6c9';
            } else {
                if (selected.dataset.id === el.dataset.id && selected.dataset.type !== el.dataset.type) {
                    selected.classList.add('matched'); el.classList.add('matched');
                    matchResults[el.dataset.id] = true; updateScore();
                }
                selected.style.background = ''; selected = null;
            }
        }

        function testPron(word, idx) {
            const rec = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            rec.lang = 'en-US';
            rec.start();
            document.getElementById(`pron-fb-${idx}`).textContent = "Listening...";
            rec.onresult = (e) => {
                const say = e.results[0][0].transcript.toLowerCase();
                if (say.includes(word.toLowerCase())) {
                    document.getElementById(`pron-fb-${idx}`).textContent = "✅ Correct! (" + word + ")";
                    document.getElementById(`pron-fb-${idx}`).style.color = "var(--success-color)";
                    pronResults[idx] = true; updateScore();
                } else {
                    document.getElementById(`pron-fb-${idx}`).textContent = "❌ Try again!";
                    document.getElementById(`pron-fb-${idx}`).style.color = "var(--error-color)";
                }
            };
        }

        function updateScore() {
            const s = writingResults.filter(x => x).length + matchResults.filter(x => x).length + pronResults.filter(x => x).length;
            document.getElementById('current-score').textContent = s;
        }

        window.onload = setup;
    </script>
</body>
</html>
