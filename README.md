<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Опрос на дружбу</title>
  <style>
    * { box-sizing: border-box; }
    body { margin: 0; min-height: 100vh; font-family: Arial, sans-serif; color: #fff; background: radial-gradient(circle at top left, rgba(255,87,127,.35), transparent 32%), radial-gradient(circle at bottom right, rgba(91,124,250,.35), transparent 35%), linear-gradient(135deg,#111827,#1f2937); padding: 18px; transition: background 1s ease; }

    .app { width: min(980px, 100%); margin: 56px auto 0; background: rgba(17,24,39,.88); border: 1px solid rgba(255,255,255,.16); border-radius: 28px; padding: 24px; box-shadow: 0 24px 70px rgba(0,0,0,.42); }

    .card { background: rgba(255,255,255,.08); border: 1px solid rgba(255,255,255,.14); border-radius: 22px; padding: 18px; margin-bottom: 16px; transition: transform 0.3s ease, box-shadow 0.3s ease; }
    .card:hover { transform: translateY(-5px); box-shadow: 0 12px 35px rgba(0,0,0,.5); }

    button { width: 100%; padding: 15px; border-radius: 16px; border: none; cursor: pointer; font-size: 16px; margin-top: 12px; }
    .progress-container { width: 100%; background: rgba(255,255,255,.12); border-radius: 14px; margin-bottom: 16px; }
    .progress-bar { width: 0%; height: 16px; background: #5b7cfa; border-radius: 14px; transition: width 0.4s ease; }
    .theme-toggle { position: fixed; bottom: 14px; right: 14px; background: #5b7cfa; color: white; border-radius: 999px; padding: 10px 14px; cursor: pointer; z-index: 100; }
  </style>
</head>
<body>
<main class="app" id="screen"></main>
<div class="theme-toggle" onclick="toggleTheme()">🌓 Тема</div>
<script>
const quiz=[
  {text:'Какой мой любимый напиток?', answers:['Чай','Кола','Сок','Вода'], correctIndex:0},
  {text:'Что я выберу на выходных?', answers:['Погулять','Поспать','Поиграть','Посмотреть фильм'], correctIndex:2}
];

function renderQuiz(){
  const screen=document.getElementById('screen');
  screen.innerHTML='';
  quiz.forEach((q,i)=>{
    const card=document.createElement('div');
    card.className='card';
    card.innerHTML=`<h3>${q.text}</h3>`+q.answers.map((a,ai)=>`<label><input type='radio' name='q${i}' value='${ai}'> ${a}</label>`).join('');
    screen.appendChild(card);
  });
  const prog=document.createElement('div');
  prog.className='progress-container';
  const bar=document.createElement('div');
  bar.className='progress-bar';
  prog.appendChild(bar);
  screen.prepend(prog);
  const btn=document.createElement('button');
  btn.textContent='Узнать результат';
  btn.onclick=showResult;
  screen.appendChild(btn);
}

function showResult(){
  let correct=0;
  quiz.forEach((q,i)=>{
    const sel=document.querySelector(`input[name='q${i}']:checked`);
    if(sel && Number(sel.value)===q.correctIndex) correct++;
  });
  const percent=Math.round((correct/quiz.length)*100);
  renderResult(percent);
  if(percent>=70) createConfetti();
  updateProgress(correct,quiz.length);
}

function renderResult(percent){
  const screen=document.getElementById('screen');
  screen.innerHTML=`<div class='card'><h2>${percent>=90?'Вы лучшие друзья!':percent>=70?'Вы хорошие друзья!':'Можно дружить!'}</h2><p>Совпало ${percent}% ответов.</p><button onclick='renderQuiz()'>Пройти заново</button></div>`;
}

function toggleTheme(){
  document.body.classList.toggle('light-theme');
}

function updateProgress(current,total){
  const bar=document.querySelector('.progress-bar');
  if(bar) bar.style.width=((current/total)*100)+'%';
}

function createConfetti(){
  for(let i=0;i<100;i++){
    const c=document.createElement('div');
    c.style.position='absolute';
    c.style.width=c.style.height=(Math.random()*8+4)+'px';
    c.style.left=Math.random()*window.innerWidth+'px';
    c.style.top=Math.random()*-50+'px';
    c.style.background=`hsl(${Math.random()*360},100%,50%)`;
    document.body.appendChild(c);
    setTimeout(()=>document.body.removeChild(c),1200);
  }
}

renderQuiz();
</script>
</body>
</html>
