<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Activity: What's the matter? - I have a ... | Interactive Drag & Drop</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none; /* avoid selection while dragging */
        }

        body {
            background: linear-gradient(135deg, #d9f0f7 0%, #b2dfee 100%);
            font-family: 'Segoe UI', 'Comic Neue', 'Comic Sans MS', 'Chalkboard SE', cursive, sans-serif;
            margin: 0;
            padding: 24px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .worksheet {
            max-width: 1000px;
            width: 100%;
            background: #fffef7;
            border-radius: 64px;
            box-shadow: 0 20px 35px rgba(0, 0, 0, 0.2), inset 0 1px 0 rgba(255,255,240,0.8);
            padding: 20px 28px 35px 28px;
            transition: all 0.2s;
        }

        .header {
            text-align: center;
            margin-bottom: 20px;
        }

        .header h1 {
            font-size: 2rem;
            background: #ffe3b5;
            display: inline-block;
            padding: 8px 28px;
            border-radius: 60px;
            color: #2c4763;
            margin: 0 0 8px 0;
            box-shadow: inset 0 -2px 0 #ffc97a;
        }

        .subhead {
            font-size: 1.15rem;
            background: #d9f0fa;
            display: inline-block;
            padding: 5px 24px;
            border-radius: 50px;
            color: #1c5a7a;
            font-weight: bold;
        }

        .nurse-area {
            background: #fef0df;
            border-radius: 48px;
            padding: 12px 20px;
            margin-bottom: 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
            box-shadow: inset 0 1px 3px #fff3e0, 0 4px 8px rgba(0,0,0,0.05);
        }

        .nurse-icon {
            font-size: 52px;
            background: white;
            border-radius: 50%;
            padding: 10px;
        }

        .nurse-text {
            font-size: 1.3rem;
            font-weight: bold;
            color: #2b4f2b;
            background: #fff3cf;
            padding: 6px 20px;
            border-radius: 40px;
        }

        /* sentences container */
        .sentences-container {
            display: flex;
            flex-direction: column;
            gap: 24px;
            margin: 20px 0 20px;
        }

        .sentence-card {
            background: #ffffffea;
            border-radius: 48px;
            padding: 18px 24px;
            box-shadow: 0 8px 0 #d4b48c;
            border: 2px solid #ffdfb3;
        }

        .sentence-text {
            font-size: 1.5rem;
            font-weight: 500;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 8px;
            row-gap: 12px;
            line-height: 1.4;
        }

        .static-part {
            background: #f7f2e4;
            padding: 5px 14px;
            border-radius: 40px;
            display: inline-block;
        }

        .blank {
            background: #fffbee;
            border: 3px dashed #fcb375;
            border-radius: 48px;
            min-width: 160px;
            padding: 8px 20px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-weight: bold;
            font-size: 1.2rem;
            transition: all 0.1s;
        }

        .blank.drop-over {
            background: #ffecb3;
            border-color: #f29b2e;
            border-style: solid;
        }

        .filled-word {
            background: #b9dc9c;
            color: #2a572a;
            padding: 6px 22px;
            border-radius: 40px;
            font-size: 1.2rem;
            font-weight: bold;
            display: inline-block;
            box-shadow: inset 0 1px 2px white, 0 2px 4px rgba(0,0,0,0.1);
        }

        .empty-hint {
            color: #ad8b62;
            font-style: italic;
            font-weight: normal;
            font-size: 0.9rem;
        }

        .emoji-hint {
            font-size: 1.3rem;
            margin-left: 8px;
            opacity: 0.7;
        }

        /* draggable bank */
        .drag-bank {
            background: #fef7e5;
            border-radius: 56px;
            padding: 16px 20px;
            margin-top: 20px;
            box-shadow: inset 0 1px 4px #fff6cf, 0 6px 14px rgba(0,0,0,0.1);
        }

        .bank-title {
            text-align: center;
            font-size: 1.6rem;
            font-weight: bold;
            background: #ffd58c;
            display: inline-block;
            width: 100%;
            border-radius: 60px;
            padding: 6px;
            margin-bottom: 20px;
        }

        .words-flex {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 18px;
            margin-bottom: 10px;
        }

        .drag-word {
            background: #ffe0ae;
            padding: 12px 28px;
            font-size: 1.35rem;
            font-weight: bold;
            border-radius: 60px;
            cursor: grab;
            display: inline-block;
            box-shadow: 0 5px 0 #b57c3a;
            transition: 0.05s linear;
            color: #2c3820;
            border: 1px solid #ffd89a;
        }

        .drag-word:active {
            cursor: grabbing;
        }

        .drag-word.dragging {
            opacity: 0.4;
        }

        .feedback-area {
            margin: 20px 0 10px;
            text-align: center;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }

        .message-box {
            background: #fff0cf;
            padding: 8px 24px;
            border-radius: 40px;
            font-weight: bold;
            font-size: 1rem;
            color: #2f6b47;
        }

        .reset-btn {
            background: #f5ac4e;
            border: none;
            font-size: 1.1rem;
            font-weight: bold;
            padding: 8px 30px;
            border-radius: 40px;
            cursor: pointer;
            font-family: inherit;
            transition: 0.05s linear;
            box-shadow: 0 3px 0 #9b5e2c;
            color: #2d2412;
        }

        .reset-btn:active {
            transform: translateY(2px);
            box-shadow: 0 1px 0 #9b5e2c;
        }

        footer {
            text-align: center;
            font-size: 0.8rem;
            color: #735f42;
            margin-top: 20px;
        }

        @media (max-width: 680px) {
            .worksheet {
                padding: 16px;
            }
            .sentence-text {
                font-size: 1.2rem;
            }
            .drag-word {
                padding: 8px 20px;
                font-size: 1.1rem;
            }
            .blank {
                min-width: 130px;
            }
        }
    </style>
</head>
<body>

<div class="worksheet">
    <div class="header">
        <h1>🩺 What's the matter? 🤒</h1>
        <div class="subhead">Tell the nurse: "I have a ______." ✨</div>
    </div>

    <div class="nurse-area">
        <div class="nurse-icon">👩‍⚕️💬</div>
        <div class="nurse-text">"Tell me, what’s the matter?"</div>
        <div class="nurse-icon">📝🩺</div>
    </div>

    <!-- 6 sentences: "I have a ______ ." -->
    <div class="sentences-container" id="sentencesContainer"></div>

    <!-- Draggable words: only one word per pain -->
    <div class="drag-bank">
        <div class="bank-title">📌 Drag the correct word 📌</div>
        <div class="words-flex" id="draggableWordsBank"></div>
        <div style="text-align:center; font-size:0.85rem; margin-top: 12px;">💡 Each word can be used only once. Drop it into the blank space.</div>
    </div>

    <div class="feedback-area">
        <div class="message-box" id="globalMessage">⭐ Drag a word to complete each sentence! ⭐</div>
        <button class="reset-btn" id="resetAllBtn">⟳ Reset All</button>
    </div>
    <footer>🏥 Example: "I have a headache." → Now tell the nurse about your pain!</footer>
</div>

<script>
    // ---- 6 different pains (each is a single word, except "sore throat" is two words but that's fine; it's one illness)
    // According to request: only one word like toothache, headache, stomach-ache, earache, backache, sore throat.
    const painData = [
        { id: 0, pain: "toothache", hintEmoji: "🦷" },
        { id: 1, pain: "headache", hintEmoji: "🤕" },
        { id: 2, pain: "stomach-ache", hintEmoji: "🤢" },
        { id: 3, pain: "earache", hintEmoji: "👂💢" },
        { id: 4, pain: "backache", hintEmoji: "🔙😣" },
        { id: 5, pain: "sore throat", hintEmoji: "🗣️🔥" }
    ];

    // State for each sentence: filledPain (null or string), matched (bool), correctPain
    let sentencesState = [];
    let availablePains = [];     // list of pain strings not yet used correctly

    function initGame() {
        sentencesState = painData.map((item) => ({
            correctPain: item.pain,
            filledPain: null,
            matched: false
        }));
        availablePains = painData.map(item => item.pain);
        renderAll();
        updateGlobalMessage("🔄 Game reset! Drag the right word into each blank.", false);
    }

    function updateGlobalMessage(msg, isSuccess = false) {
        const msgDiv = document.getElementById('globalMessage');
        if (msgDiv) {
            msgDiv.innerText = msg;
            if (isSuccess) {
                msgDiv.style.background = "#c8e6b5";
                msgDiv.style.color = "#1f5a1a";
            } else {
                msgDiv.style.background = "#fff0cf";
                msgDiv.style.color = "#2f6b47";
            }
        }
    }

    function checkCompletion() {
        const allMatched = sentencesState.every(s => s.matched === true && s.filledPain !== null);
        if (allMatched) {
            updateGlobalMessage("🎉 PERFECT! You told the nurse about all your pains! Excellent work! 🎉", true);
        } else {
            const matchedCount = sentencesState.filter(s => s.matched).length;
            updateGlobalMessage(`✅ Completed: ${matchedCount} / ${sentencesState.length}. Keep matching!`, false);
        }
    }

    function renderAll() {
        renderSentences();
        renderDraggableBank();
    }

    function renderSentences() {
        const container = document.getElementById('sentencesContainer');
        if (!container) return;
        container.innerHTML = '';

        sentencesState.forEach((state, idx) => {
            const painItem = painData[idx];
            const correctWord = painItem.pain;
            const isFilled = state.matched && state.filledPain !== null;
            const displayedWord = state.filledPain;

            const card = document.createElement('div');
            card.className = 'sentence-card';

            const sentenceDiv = document.createElement('div');
            sentenceDiv.className = 'sentence-text';

            // Parts: "I have a" [blank] "."
            const part1 = document.createElement('span');
            part1.className = 'static-part';
            part1.innerText = "I have a";

            const blankSpan = document.createElement('span');
            blankSpan.className = 'blank';
            blankSpan.setAttribute('data-sentence-idx', idx);
            blankSpan.setAttribute('data-correct', correctWord);

            if (isFilled && displayedWord) {
                const wordSpan = document.createElement('span');
                wordSpan.className = 'filled-word';
                wordSpan.innerText = displayedWord;
                blankSpan.appendChild(wordSpan);
            } else {
                const hintSpan = document.createElement('span');
                hintSpan.className = 'empty-hint';
                hintSpan.innerText = '______ ?';
                blankSpan.appendChild(hintSpan);
            }

            const periodSpan = document.createElement('span');
            periodSpan.className = 'static-part';
            periodSpan.innerText = ".";

            const emojiHint = document.createElement('span');
            emojiHint.className = 'emoji-hint';
            emojiHint.innerText = painItem.hintEmoji;

            sentenceDiv.appendChild(part1);
            sentenceDiv.appendChild(blankSpan);
            sentenceDiv.appendChild(periodSpan);
            sentenceDiv.appendChild(emojiHint);
            card.appendChild(sentenceDiv);
            container.appendChild(card);

            // Drag & drop events for blank
            blankSpan.addEventListener('dragover', (e) => {
                e.preventDefault();
                if (!blankSpan.classList.contains('drop-over')) blankSpan.classList.add('drop-over');
            });
            blankSpan.addEventListener('dragleave', () => {
                blankSpan.classList.remove('drop-over');
            });
            blankSpan.addEventListener('drop', (e) => {
                e.preventDefault();
                blankSpan.classList.remove('drop-over');
                const sentenceIndex = parseInt(blankSpan.getAttribute('data-sentence-idx'));
                const draggedPain = e.dataTransfer.getData('text/plain');
                if (!draggedPain) return;

                const currentState = sentencesState[sentenceIndex];
                if (currentState.matched === true) {
                    updateGlobalMessage(`❌ This sentence already has "${currentState.filledPain}". Use Reset if you want to change.`, false);
                    return;
                }

                // check if the word is still available (not used correctly elsewhere)
                if (!availablePains.includes(draggedPain)) {
                    updateGlobalMessage(`⚠️ "${draggedPain}" has already been used! Each word only once.`, false);
                    return;
                }

                const correctForThis = painData[sentenceIndex].pain;
                if (draggedPain === correctForThis) {
                    // Correct match
                    const painIndex = availablePains.indexOf(draggedPain);
                    if (painIndex !== -1) availablePains.splice(painIndex, 1);
                    sentencesState[sentenceIndex].filledPain = draggedPain;
                    sentencesState[sentenceIndex].matched = true;
                    renderAll();
                    updateGlobalMessage(`✅ Correct! "I have a ${draggedPain}." Great!`, false);
                    checkCompletion();
                } else {
                    updateGlobalMessage(`❌ Oops! "${draggedPain}" is not the right pain for this sentence. Try again.`, false);
                    blankSpan.style.backgroundColor = "#ffe0e0";
                    setTimeout(() => {
                        if (blankSpan) blankSpan.style.backgroundColor = "";
                    }, 300);
                }
            });
        });
    }

    function renderDraggableBank() {
        const bankContainer = document.getElementById('draggableWordsBank');
        if (!bankContainer) return;
        bankContainer.innerHTML = '';

        // show only words that are still available (not yet matched)
        const remaining = [...availablePains];

        if (remaining.length === 0) {
            const completeMsg = document.createElement('div');
            completeMsg.style.background = "#c8e6b5";
            completeMsg.style.padding = "12px 20px";
            completeMsg.style.borderRadius = "50px";
            completeMsg.style.fontWeight = "bold";
            completeMsg.innerText = "✨ All pains diagnosed! ✨ You're doing great! Click Reset to practice again.";
            bankContainer.appendChild(completeMsg);
            return;
        }

        remaining.forEach(painWord => {
            const dragEl = document.createElement('div');
            dragEl.className = 'drag-word';
            dragEl.setAttribute('draggable', 'true');
            dragEl.setAttribute('data-pain', painWord);
            dragEl.innerText = painWord;   // only the word, no duplication

            // optional small emoji helper
            let emoji = '';
            if (painWord === 'toothache') emoji = '🦷';
            else if (painWord === 'headache') emoji = '🤕';
            else if (painWord === 'stomach-ache') emoji = '🤢';
            else if (painWord === 'earache') emoji = '👂💢';
            else if (painWord === 'backache') emoji = '🔙😣';
            else if (painWord === 'sore throat') emoji = '🗣️🔥';
            if (emoji) {
                const smallEmoji = document.createElement('span');
                smallEmoji.innerText = ` ${emoji}`;
                smallEmoji.style.fontSize = '1.2rem';
                dragEl.appendChild(smallEmoji);
            }

            dragEl.addEventListener('dragstart', (e) => {
                e.dataTransfer.setData('text/plain', painWord);
                e.dataTransfer.effectAllowed = 'copy';
                dragEl.classList.add('dragging');
            });
            dragEl.addEventListener('dragend', (e) => {
                dragEl.classList.remove('dragging');
            });
            bankContainer.appendChild(dragEl);
        });
    }

    function resetGame() {
        initGame();
    }

    // Prevent default dragover/drop on body
    function setupGlobalEvents() {
        document.body.addEventListener('dragover', (e) => {
            e.preventDefault();
        });
        document.body.addEventListener('drop', (e) => {
            e.preventDefault();
        });
    }

    // Initialize everything
    initGame();
    setupGlobalEvents();

    const resetBtn = document.getElementById('resetAllBtn');
    if (resetBtn) resetBtn.addEventListener('click', resetGame);
</script>
</body>
</html>
