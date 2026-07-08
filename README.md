from pathlib import Path
html = r'''<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>بازی کلمات درهم‌ریخته - پایه دوم</title>
  <style>
    :root{
      --bg1:#ffefb0; --bg2:#ffd6e7; --bg3:#c8f7ff; --card:#ffffffee;
      --primary:#ff5fa2; --secondary:#6c63ff; --success:#2ecc71; --danger:#ff4d4d;
      --text:#233; --shadow:0 10px 30px rgba(0,0,0,.12);
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family:Tahoma, Arial, sans-serif; color:var(--text);
      background: linear-gradient(135deg,var(--bg1),var(--bg2),var(--bg3));
      min-height:100vh; overflow-x:hidden;
    }
    .bg-hearts::before, .bg-hearts::after{
      content:""; position:fixed; inset:auto; pointer-events:none; opacity:.18;
      width:180px; height:180px; border-radius:50%; filter: blur(14px);
      background: radial-gradient(circle at 30% 30%, #fff 0 12%, transparent 13%),
                  radial-gradient(circle at 70% 25%, #fff 0 10%, transparent 11%),
                  radial-gradient(circle at 50% 70%, #fff 0 18%, transparent 19%);
      animation: float 8s ease-in-out infinite;
      z-index:0;
    }
    .bg-hearts::before{top:10px; left:10px}
    .bg-hearts::after{bottom:10px; right:10px; animation-delay:2s}
    @keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-16px)}}
    .container{position:relative; z-index:1; max-width:980px; margin:0 auto; padding:20px}
    .card{
      background:var(--card); backdrop-filter: blur(8px);
      border-radius:28px; box-shadow:var(--shadow); padding:22px; margin:16px 0;
      border:3px solid rgba(255,255,255,.7);
    }
    h1{margin:0 0 10px; color:#ff2f7d; font-size:clamp(26px,4vw,40px)}
    h2,h3{margin:10px 0}
    .teacher{font-size:1.1rem; background:#fff3; padding:10px 14px; border-radius:18px; display:inline-block}
    .row{display:grid; grid-template-columns:1fr; gap:14px}
    .input, select, button{
      width:100%; border:none; border-radius:18px; padding:14px 16px; font-size:1rem;
      outline:none;
    }
    .input, select{background:#fff; box-shadow: inset 0 0 0 2px rgba(0,0,0,.05)}
    button{cursor:pointer; font-weight:700; transition:.2s; background:linear-gradient(135deg,var(--secondary),var(--primary)); color:#fff}
    button:hover{transform:translateY(-2px); filter:brightness(1.05)}
    button.secondary{background:linear-gradient(135deg,#00c2ff,#38d39f)}
    .hidden{display:none}
    .game-top{display:flex; flex-wrap:wrap; gap:12px; justify-content:space-between; align-items:center}
    .badge{background:#fff; padding:10px 14px; border-radius:999px; box-shadow:var(--shadow); font-weight:700}
    .word-box{
      background:linear-gradient(135deg,#fff,#fff7fd); border-radius:24px; padding:20px; text-align:center;
      border:3px dashed #ff9ac2; margin-top:10px;
    }
    .scramble{font-size:clamp(24px,4vw,38px); letter-spacing:.22em; word-spacing:.22em; font-weight:700; color:#4a2d73}
    .options{display:grid; grid-template-columns:repeat(2,minmax(0,1fr)); gap:12px; margin-top:16px}
    .opt{background:#fff; color:#333; border:2px solid #ffd0e3}
    .opt.correct{background:#dbffe9; border-color:#35c26b}
    .opt.wrong{background:#ffe1e1; border-color:#ff6b6b}
    .feedback{font-size:1.1rem; font-weight:700; min-height:34px; margin-top:10px}
    .progress-wrap{background:#fff; border-radius:999px; height:18px; overflow:hidden; box-shadow:inset 0 0 0 2px rgba(0,0,0,.05)}
    .progress{height:100%; width:0%; background:linear-gradient(90deg,#ff7eb3,#7afcff,#feff9c); transition:width .35s}
    .profile{display:grid; grid-template-columns:repeat(2,minmax(0,1fr)); gap:12px}
    .small{font-size:.95rem; opacity:.9}
    .emoji-band{font-size:2rem; animation: bounce 1.6s ease-in-out infinite; text-align:center}
    @keyframes bounce{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
    audio{width:100%; margin-top:10px}
    .footer{font-size:.9rem; opacity:.8; text-align:center; margin-top:10px}
    @media (max-width:640px){ .options, .profile{grid-template-columns:1fr} }
  </style>
</head>
<body class="bg-hearts">
  <div class="container">
    <div class="card" id="startCard">
      <div class="emoji-band">🧸🎈🌈✨🦄🎉</div>
      <h1>بازی کلمات درهم‌ریخته دو گزینه‌ای</h1>
      <div class="teacher">نام آموزگار: <strong>شایسته صفی</strong></div>
      <p class="small">سلام عزیزم! اول نام دانش‌آموز را بنویس تا بازی شروع شود.</p>
      <div class="row">
        <input id="studentName" class="input" type="text" placeholder="نام دانش‌آموز را وارد کن..." />
        <button onclick="startGame()">شروع بازی</button>
      </div>
    </div>

    <div class="card hidden" id="gameCard">
      <div class="game-top">
        <div class="badge">دانش‌آموز: <span id="showName"></span></div>
        <div class="badge">سؤال <span id="qNum">1</span> از 20</div>
        <div class="badge">امتیاز: <span id="score">0</span></div>
      </div>
      <div style="margin-top:12px">
        <div class="progress-wrap"><div class="progress" id="progress"></div></div>
      </div>
      <div class="word-box">
        <div class="small">کلمه درهم‌ریخته:</div>
        <div class="scramble" id="scramble"></div>
        <div class="options" id="options"></div>
        <div class="feedback" id="feedback"></div>
      </div>
      <div style="display:flex; gap:12px; margin-top:14px; flex-wrap:wrap">
        <button class="secondary" onclick="nextQuestion()" id="nextBtn" disabled>سؤال بعدی</button>
        <button onclick="restartGame()">شروع دوباره</button>
      </div>
      <audio controls loop autoplay>
        <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_4f2d9d1e87.mp3?filename=happy-birthday-to-you-110645.mp3" type="audio/mpeg">
        مرورگر شما از پخش صوت پشتیبانی نمی‌کند.
      </audio>
      <div class="footer">موسیقی شاد کودکانه بی‌کلام برای فضای بازی</div>
    </div>

    <div class="card hidden" id="resultCard">
      <div class="emoji-band">🎉🏆🌟👏🎊</div>
      <h2 id="resultTitle"></h2>
      <div class="profile">
        <div class="badge">امتیاز نهایی: <span id="finalScore"></span> / 20</div>
        <div class="badge">درصد موفقیت: <span id="percent"></span>%</div>
      </div>
      <div class="card" style="margin-top:14px; background:#fff">
        <h3>کارنامه</h3>
        <p id="report"></p>
      </div>
      <div id="cheer" style="font-size:1.2rem; font-weight:700; color:#ff2f7d; text-align:center; margin:14px 0"></div>
      <button onclick="restartGame()">بازی دوباره</button>
    </div>
  </div>

<script>
const questions = [
  {scramble:'م ا ن ب س', options:['مداد','بنام'], answer:'مداد'},
  {scramble:'ک ا ت ب', options:['کتاب','کباب'], answer:'کتاب'},
  {scramble:'س ی ب', options:['سیب','سبز'], answer:'سیب'},
  {scramble:'گ ل', options:['گل','گاو'], answer:'گل'},
  {scramble:'ب ا ر ا ن', options:['باران','بنا'], answer:'باران'},
  {scramble:'م ا د ر', options:['مادر','مدرسه'], answer:'مادر'},
  {scramble:'د و س ت', options:['دوست','دست'], answer:'دوست'},
  {scramble:'ش ا د ی', options:['شادی','شیشه'], answer:'شادی'},
  {scramble:'ن ا ن', options:['نان','نارنج'], answer:'نان'},
  {scramble:'پ ا ر', options:['پر','پار'], answer:'پر'},
  {scramble:'ه و ا', options:['هوا','هدیه'], answer:'هوا'},
  {scramble:'ل ی و ن', options:['لیوان','لباس'], answer:'لیوان'},
  {scramble:'م ر غ', options:['مرغ','مرد'], answer:'مرغ'},
  {scramble:'د ر خ ت', options:['درخت','دریا'], answer:'درخت'},
  {scramble:'م ا ه', options:['ماه','ماهی'], answer:'ماه'},
  {scramble:'س ت ا ر ه', options:['ستاره','سنگ'], answer:'ستاره'},
  {scramble:'ب ا ز ی', options:['بازی','باز'], answer:'بازی'},
  {scramble:'پ ن ج', options:['پنج','پلو'], answer:'پنج'},
  {scramble:'ر ا ه', options:['راه','رود'], answer:'راه'},
  {scramble:'ک و ه', options:['کوه','کوچه'], answer:'کوه'}
];
let current = 0, score = 0, answered = false, student = '';
function startGame(){
  student = document.getElementById('studentName').value.trim() || 'دانش‌آموز عزیز';
  document.getElementById('showName').textContent = student;
  document.getElementById('startCard').classList.add('hidden');
  document.getElementById('gameCard').classList.remove('hidden');
  loadQuestion();
}
function loadQuestion(){
  answered = false;
  document.getElementById('nextBtn').disabled = true;
  const q = questions[current];
  document.getElementById('qNum').textContent = current + 1;
  document.getElementById('score').textContent = score;
  document.getElementById('scramble').textContent = q.scramble;
  document.getElementById('feedback').textContent = '';
  const opts = document.getElementById('options');
  opts.innerHTML = '';
  q.options.forEach(opt => {
    const b = document.createElement('button');
    b.className = 'opt';
    b.textContent = opt;
    b.onclick = () => checkAnswer(b, opt);
    opts.appendChild(b);
  });
  document.getElementById('progress').style.width = ((current)/questions.length*100) + '%';
}
function checkAnswer(btn, opt){
  if(answered) return;
  answered = true;
  const q = questions[current];
  const buttons = [...document.querySelectorAll('.opt')];
  buttons.forEach(b => b.disabled = true);
  if(opt === q.answer){
    score++;
    btn.classList.add('correct');
    document.getElementById('feedback').textContent = 'آفرین! جواب درست بود! 🌟';
  } else {
    btn.classList.add('wrong');
    buttons.find(b => b.textContent === q.answer)?.classList.add('correct');
    document.getElementById('feedback').textContent = 'اشکالی ندارد، دوباره با دقت نگاه کن 💖';
  }
  document.getElementById('score').textContent = score;
  document.getElementById('nextBtn').disabled = false;
}
function nextQuestion(){
  if(current < questions.length - 1){
    current++;
    loadQuestion();
  } else {
    endGame();
  }
}
function endGame(){
  document.getElementById('gameCard').classList.add('hidden');
  document.getElementById('resultCard').classList.remove('hidden');
  const percent = Math.round((score / questions.length) * 100);
  document.getElementById('finalScore').textContent = score;
  document.getElementById('percent').textContent = percent;
  let title = 'عالی بودی!';
  let report = 'تو با دقت و تلاش بازی را انجام دادی.';
  let cheer = 'هورااا! تو یک ستاره‌ی کلمات هستی! ✨🏆';
  if(score >= 18){ title = 'فوق‌العاده!'; report = 'کارنامه: بسیار عالی - تسلط خیلی خوب روی کلمات.'; }
  else if(score >= 14){ title = 'خیلی خوب!'; report = 'کارنامه: خوب - عملکردت عالی بود و کمی تمرین بیشتر هم مفید است.'; }
  else if(score >= 10){ title = 'آفرین!'; report = 'کارنامه: قابل قبول - با تمرین بیشتر بهتر هم می‌شوی.'; }
  else { title = 'ادامه بده!'; report = 'کارنامه: نیاز به تمرین بیشتر - تو حتماً می‌توانی بهتر شوی.'; cheer = 'تو می‌توانی! فقط کمی بیشتر تمرین کن و دوباره بدرخش! 🌈'; }
  document.getElementById('resultTitle').textContent = title + ' ' + student;
  document.getElementById('report').textContent = report;
  document.getElementById('cheer').textContent = cheer;
}
function restartGame(){ location.reload(); }
</script>
</body>
</html>'''

path = Path('/mnt/data/word_game_grade2.html')
path.write_text(html, encoding='utf-8')
print(f'sandbox:{path}')
