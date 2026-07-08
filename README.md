<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>بازی کلمات درهم ریخته 🎈</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', 'Comic Sans MS', cursive, sans-serif;
            background: linear-gradient(145deg, #f9e3b3, #ffe5b4);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        .game-container {
            max-width: 700px;
            width: 100%;
            background: rgba(255, 242, 204, 0.85);
            backdrop-filter: blur(2px);
            border-radius: 80px 80px 40px 40px;
            padding: 30px 25px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2), 0 0 0 4px #ffd966, 0 0 0 8px #ffb347;
            border: 2px solid #ffffff88;
            position: relative;
            overflow: hidden;
        }
        .floating-toys {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }
        .toy {
            position: absolute;
            font-size: 2.8rem;
            animation: floatToy 8s infinite alternate ease-in-out;
            opacity: 0.6;
            text-shadow: 0 8px 15px rgba(0,0,0,0.1);
        }
        .toy:nth-child(1) { top: 8%; left: 5%; animation-duration: 11s; }
        .toy:nth-child(2) { top: 70%; left: 85%; animation-duration: 9s; animation-delay: 1s; }
        .toy:nth-child(3) { top: 40%; left: 88%; animation-duration: 12s; animation-delay: 2s; }
        .toy:nth-child(4) { top: 85%; left: 10%; animation-duration: 10s; animation-delay: 0.5s; }
        .toy:nth-child(5) { top: 15%; left: 80%; animation-duration: 14s; animation-delay: 1.8s; }
        .toy:nth-child(6) { top: 55%; left: 2%; animation-duration: 13s; animation-delay: 3s; }
        @keyframes floatToy {
            0% { transform: translateY(0px) rotate(0deg); }
            100% { transform: translateY(-40px) rotate(8deg); }
        }
        .content { position: relative; z-index: 10; }
        .header {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            background: #fce8b2;
            padding: 12px 20px;
            border-radius: 60px;
            border: 2px solid #f7b731;
            box-shadow: inset 0 -4px 0 #d49b2a;
            margin-bottom: 25px;
        }
        .teacher-name {
            background: #ffb347;
            padding: 6px 20px;
            border-radius: 50px;
            color: #2d1f0c;
            font-weight: bold;
            font-size: 1.2rem;
            box-shadow: 0 4px 0 #b16f1a;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .student-name {
            background: #ffd966;
            padding: 6px 18px;
            border-radius: 50px;
            font-weight: bold;
            color: #2d1f0c;
            font-size: 1.1rem;
            box-shadow: 0 4px 0 #b68f3a;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .student-name input {
            border: none;
            background: transparent;
            font-weight: bold;
            font-size: 1rem;
            width: 110px;
            border-bottom: 2px dashed #7a5a1a;
            color: #2d1f0c;
            outline: none;
            padding: 0 5px;
        }
        .student-name input::placeholder {
            color: #5f471a;
            opacity: 0.7;
        }
        .question-box {
            background: #fffaec;
            border-radius: 100px;
            padding: 25px 10px;
            margin: 20px 0 25px;
            text-align: center;
            border: 3px solid #ff9f1c;
            box-shadow: 0 8px 0 #b86f1a, inset 0 -4px 0 #f0c27a;
        }
        .scrambled-word {
            font-size: 3.2rem;
            font-weight: 800;
            letter-spacing: 18px;
            color: #3d2b0e;
            text-shadow: 3px 3px 0 #fddc8b;
            word-break: keep-all;
            padding: 10px 0;
            background: #fff2d1;
            border-radius: 60px;
            display: inline-block;
            padding: 15px 40px;
            box-shadow: inset 0 -6px 0 #dba35a;
            margin-bottom: 8px;
        }
        .options {
            display: flex;
            justify-content: center;
            gap: 35px;
            margin: 25px 0 15px;
            flex-wrap: wrap;
        }
        .option-btn {
            background: #fce692;
            border: none;
            padding: 16px 40px;
            border-radius: 80px;
            font-size: 1.9rem;
            font-weight: bold;
            color: #2c1c06;
            box-shadow: 0 8px 0 #b67d2e, 0 5px 15px rgba(0,0,0,0.2);
            cursor: pointer;
            transition: 0.07s linear;
            border: 2px solid #ffe09c;
            min-width: 140px;
            letter-spacing: 2px;
        }
        .option-btn:active { transform: translateY(6px); box-shadow: 0 2px 0 #b67d2e; }
        .option-btn.correct-glow { background: #8bc34a; box-shadow: 0 0 20px #a3d86a; border-color: #4caf50; color: #1d3d0b; }
        .option-btn.wrong-glow { background: #e57373; box-shadow: 0 0 20px #f28b82; border-color: #c62828; color: #4a0e0e; }
        .info-panel {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #fde7b0;
            padding: 10px 25px;
            border-radius: 50px;
            margin: 20px 0 10px;
            border: 2px solid #efb153;
        }
        .score, .question-counter {
            font-weight: bold;
            font-size: 1.2rem;
            background: #fecb6e;
            padding: 6px 20px;
            border-radius: 40px;
            box-shadow: inset 0 -3px 0 #b57d2c;
        }
        .reset-btn {
            background: #ff8a5c;
            border: none;
            padding: 10px 25px;
            border-radius: 40px;
            font-weight: bold;
            font-size: 1.1rem;
            box-shadow: 0 5px 0 #a55327;
            color: #1f1305;
            cursor: pointer;
            transition: 0.06s linear;
            border: 1px solid #ffbb77;
        }
        .reset-btn:active { transform: translateY(5px); box-shadow: 0 1px 0 #a55327; }
        .result-card {
            background: #ffecbb;
            border-radius: 70px;
            padding: 25px 15px;
            margin-top: 20px;
            border: 4px solid #fcb045;
            text-align: center;
            box-shadow: 0 10px 0 #b87d2b;
            display: none;
        }
        .result-card.show { display: block; animation: popIn 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        @keyframes popIn { 0% { transform: scale(0.5); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }
        .result-card h2 { font-size: 2.4rem; color: #412f0e; }
        .result-card .emoji-big { font-size: 4.5rem; line-height: 1.4; }
        .result-card .report {
            background: #fef7d7;
            border-radius: 40px;
            padding: 20px;
            margin: 15px 0;
            border: 2px solid #dba158;
        }
        .celebration { font-size: 2rem; animation: bounce 0.8s infinite alternate; }
        @keyframes bounce { 0% { transform: translateY(0); } 100% { transform: translateY(-12px); } }
        .option-btn.disabled-btn { opacity: 0.6; filter: grayscale(0.4); pointer-events: none; }
        @media (max-width: 550px) {
            .scrambled-word { font-size: 2.5rem; letter-spacing: 12px; padding: 12px 20px; }
            .option-btn { font-size: 1.5rem; padding: 14px 20px; min-width: 110px; }
            .header { flex-direction: column; gap: 10px; }
        }
    </style>
</head>
<body>
<div class="game-container">
    <div class="floating-toys">
        <span class="toy">🧸</span><span class="toy">🐼</span><span class="toy">🐨</span>
        <span class="toy">🦊</span><span class="toy">🐥</span><span class="toy">🐻‍❄️</span>
    </div>

    <div class="content">
        <div class="header">
            <div class="teacher-name"><i class="fas fa-chalkboard-teacher"></i> آموزگار: شایسته صفی</div>
            <div class="student-name"><i class="fas fa-user-graduate"></i> 
                <input type="text" id="studentNameInput" placeholder="نام دانش‌آموز" value="">
            </div>
        </div>

        <div class="question-box">
            <div class="scrambled-word" id="scrambledDisplay">ا ن ب س م</div>
            <div style="margin-top: 12px; font-size: 1.2rem; color: #5d3b14;">
                <i class="fas fa-puzzle-piece"></i> کدام گزینه درست است؟
            </div>
        </div>

        <div class="options" id="optionsContainer">
            <button class="option-btn" id="option1">مناسب</button>
            <button class="option-btn" id="option2">نابسام</button>
        </div>

        <div class="info-panel">
            <span class="score" id="scoreDisplay">🌟 0</span>
            <span class="question-counter" id="questionCounter">📌 1 / 20</span>
            <button class="reset-btn" id="resetGameBtn"><i class="fas fa-undo-alt"></i> بازی نو</button>
        </div>

        <div class="result-card" id="resultCard">
            <div class="emoji-big">🎉🎊🏆</div>
            <h2 id="resultTitle">آفرین! عالی بود!</h2>
            <div class="report" id="reportCard">
                <p id="resultMessage">تو کل کلمات رو درست گفتی!</p>
                <p style="font-size: 1.2rem;"><span id="finalScoreText">0</span> از ۲۰</p>
            </div>
            <div class="celebration" id="celebrationEmoji">🥳🤩🎈</div>
            <button class="reset-btn" style="margin-top: 12px; background: #f3b33d;" id="playAgainBtn">بازی دوباره 🎮</button>
        </div>
    </div>
</div>
<script>
    (function() {
        // ۲۰ کلمه آسان پایه دوم - با حروف درهم جدا شده
        const wordBank = [
            { correct: "مدرسه", scrambled: "م د ر س ه" },
            { correct: "کتاب", scrambled: "ت ا ب ک" },
            { correct: "مداد", scrambled: "د ا د م" },
            { correct: "نوشته", scrambled: "ن و ش ت ه" },
            { correct: "بازی", scrambled: "ی ز ا ب" },
            { correct: "خورشید", scrambled: "خ و ر ش ی د" },
            { correct: "گلستان", scrambled: "گ ل س ت ا ن" },
            { correct: "پروانه", scrambled: "پ ر و ا ن ه" },
            { correct: "شکلات", scrambled: "ش ک ل ا ت" },
            { correct: "دوست", scrambled: "د و س ت" },
            { correct: "ماهی", scrambled: "م ا ه ی" },
            { correct: "آسمان", scrambled: "آ س م ا ن" },
            { correct: "چراغ", scrambled: "چ ر ا غ" },
            { correct: "درخت", scrambled: "د ر خ ت" },
            { correct: "کفش", scrambled: "ف ش ک" },
            { correct: "لباس", scrambled: "ل ب ا س" },
            { correct: "نان", scrambled: "ن ا ن" },
            { correct: "خانه", scrambled: "خ ا ن ه" },
            { correct: "گربه", scrambled: "گ ر ب ه" },
            { correct: "سگ", scrambled: "گ س" }
        ];

        // تولید دو گزینه نادرست (متفاوت از کلمه درست)
        function getWrongOptions(correctWord) {
            const pool = wordBank.map(item => item.correct).filter(w => w !== correctWord);
            // شافل کردن
            for (let i = pool.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [pool[i], pool[j]] = [pool[j], pool[i]];
            }
            const wrongs = [];
            for (let i = 0; i < pool.length && wrongs.length < 2; i++) {
                if (!wrongs.includes(pool[i])) wrongs.push(pool[i]);
            }
            // اگر کمتر از دو تا بود، پر کن
            while (wrongs.length < 2) {
                const fallback = "چیزی";
                if (!wrongs.includes(fallback) && fallback !== correctWord) wrongs.push(fallback);
            }
            return wrongs;
        }

        // عناصر DOM
        const scrambledDisplay = document.getElementById('scrambledDisplay');
        const option1 = document.getElementById('option1');
        const option2 = document.getElementById('option2');
        const scoreDisplay = document.getElementById('scoreDisplay');
        const questionCounter = document.getElementById('questionCounter');
        const resultCard = document.getElementById('resultCard');
        const finalScoreText = document.getElementById('finalScoreText');
        const resultMessage = document.getElementById('resultMessage');
        const resultTitle = document.getElementById('resultTitle');
        const celebrationEmoji = document.getElementById('celebrationEmoji');
        const studentInput = document.getElementById('studentNameInput');
        const resetBtn = document.getElementById('resetGameBtn');
        const playAgainBtn = document.getElementById('playAgainBtn');

        let questions = [];
        let currentIndex = 0;
        let score = 0;
        let gameOver = false;
        let isAnswered = false;

        // شافل کردن حروف درهم (برای تنوع بیشتر)
        function shuffleScrambled(scrambledStr) {
            let letters = scrambledStr.split(' ');
            for (let i = letters.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [letters[i], letters[j]] = [letters[j], letters[i]];
            }
            return letters.join(' ');
        }

        function loadQuestion(index) {
            if (gameOver) return;
            if (index >= questions.length) {
                endGame();
                return;
            }
            isAnswered = false;
            const q = questions[index];
            scrambledDisplay.textContent = q.scrambled;

            // گرفتن گزینه‌های نادرست (متفاوت)
            const wrongs = getWrongOptions(q.correct);
            let options = [q.correct, ...wrongs];
            // شافل کردن گزینه‌ها
            for (let i = options.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [options[i], options[j]] = [options[j], options[i]];
            }
            // اطمینان از اینکه گزینه درست حتماً در گزینه‌ها هست
            if (!options.includes(q.correct)) {
                options[0] = q.correct;
            }
            option1.textContent = options[0];
            option2.textContent = options[1];
            // حذف کلاس‌های اضافی
            option1.classList.remove('correct-glow', 'wrong-glow', 'disabled-btn');
            option2.classList.remove('correct-glow', 'wrong-glow', 'disabled-btn');
            questionCounter.textContent = `📌 ${index+1} / ${questions.length}`;
        }

        function endGame() {
            gameOver = true;
            const studentName = studentInput.value.trim() || 'دانش‌آموز';
            const total = questions.length;
            const percent = Math.round((score / total) * 100);
            let msg = '', title = '', emoji = '';
            if (percent === 100) {
                title = '🎊 عالی‌ترین! 🎊';
                msg = `آفرین ${studentName}! همه رو درست گفتی! تو یک قهرمانی!`;
                emoji = '🏆🎖️💥';
            } else if (percent >= 80) {
                title = '😍 خیلی خوب!';
                msg = `${studentName} عزیز، خیلی خوب بود! فقط ${total - score} تا رو کم آوردی!`;
                emoji = '🌟🎈✨';
            } else if (percent >= 50) {
                title = '👍 خوب بود!';
                msg = `${studentName} جان، خوب بود! می‌تونی بهتر بشی!`;
                emoji = '💪📚🌈';
            } else {
                title = '💪 تلاش کن!';
                msg = `${studentName} عزیزم، تمرین بیشتر باعث میشه عالی شی! ناامید نشو!`;
                emoji = '🌱🧸💖';
            }
            resultTitle.textContent = title;
            resultMessage.textContent = msg;
            finalScoreText.textContent = `${score} از ${total}`;
            celebrationEmoji.textContent = emoji;
            resultCard.classList.add('show');
            option1.classList.add('disabled-btn');
            option2.classList.add('disabled-btn');
        }

        function checkAnswer(selectedText) {
            if (gameOver || isAnswered) return;
            const currentQ = questions[currentIndex];
            const isCorrect = (selectedText === currentQ.correct);
            if (isCorrect) {
                score++;
                scoreDisplay.textContent = `🌟 ${score}`;
                if (option1.textContent === selectedText) option1.classList.add('correct-glow');
                else option2.classList.add('correct-glow');
            } else {
                if (option1.textContent === selectedText) option1.classList.add('wrong-glow');
                else option2.classList.add('wrong-glow');
                // نمایش پاسخ درست
                if (option1.textContent === currentQ.correct) option1.classList.add('correct-glow');
                else if (option2.textContent === currentQ.correct) option2.classList.add('correct-glow');
            }
            isAnswered = true;
            option1.classList.add('disabled-btn');
            option2.classList.add('disabled-btn');

            setTimeout(() => {
                if (gameOver) return;
                if (currentIndex + 1 >= questions.length) {
                    endGame();
                } else {
                    currentIndex++;
                    loadQuestion(currentIndex);
                }
            }, 950);
        }

        function initGame() {
            // ساخت سوالات با درهم‌سازی جدید
            questions = wordBank.map(item => ({
                correct: item.correct,
                scrambled: shuffleScrambled(item.scrambled)
            }));
            // شافل کردن ترتیب سوالات
            for (let i = questions.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [questions[i], questions[j]] = [questions[j], questions[i]];
            }
            currentIndex = 0;
            score = 0;
            gameOver = false;
            isAnswered = false;
            scoreDisplay.textContent = '🌟 0';
            resultCard.classList.remove('show');
            option1.classList.remove('disabled-btn', 'correct-glow', 'wrong-glow');
            option2.classList.remove('disabled-btn', 'correct-glow', 'wrong-glow');
            loadQuestion(0);
        }

        // رویدادها
        option1.addEventListener('click', function() {
            if (!gameOver && !isAnswered) checkAnswer(this.textContent);
        });
        option2.addEventListener('click', function() {
            if (!gameOver && !isAnswered) checkAnswer(this.textContent);
        });

        resetBtn.addEventListener('click', initGame);
        playAgainBtn.addEventListener('click', initGame);

        // مقداردهی اولیه
        initGame();

        // آهنگ شاد کودکانه (Web Audio API)
        (function playHappyTune() {
            try {
                const AudioCtx = window.AudioContext || window.webkitAudioContext;
                const ctx = new AudioCtx();
                const notes = [523, 587, 659, 698, 784, 880, 988, 1047];
                let index = 0;
                function playNote() {
                    if (index >= notes.length) index = 0;
                    const osc = ctx.createOscillator();
                    const gain = ctx.createGain();
                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.type = 'triangle';
                    osc.frequency.value = notes[index] * 1.2;
                    gain.gain.setValueAtTime(0.08, ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.25);
                    osc.start(ctx.currentTime);
                    osc.stop(ctx.currentTime + 0.25);
                    index++;
                }
                setInterval(playNote, 700);
                setTimeout(playNote, 200);
            } catch (e) { /* fallback */ }
        })();
    })();
</script>
</body>
</html>
