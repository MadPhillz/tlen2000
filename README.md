# tlen2000
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>ТЛЕН-2000: Остановка</title>
  
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
    <style>
    @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
    
    
    body {
      filter: contrast(1.2) brightness(0.9) saturate(0.7);
      background-image: repeating-linear-gradient(
        to bottom,
        #000 0px,
        #000 1px,
        #111 2px,
        #111 4px
      );
    
      margin: 0;
      background-color: #000;
      color: #aaa;
      font-family: 'Press Start 2P', monospace;
      background-image: linear-gradient(#111, #000);
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 20px;
    }
    .vhs {
      text-shadow: 0 0 5px #f0f, 0 0 10px #0ff;
      animation: flicker 1.5s infinite alternate;
    }
    @keyframes flicker {
      from { opacity: 1; }
      to { opacity: 0.8; }
    }
    .btn {
      background: #111;
      color: #ccc;
      border: 1px solid #444;
      padding: 10px;
      margin-top: 10px;
      cursor: pointer;
      font-family: inherit;
    }
    .btn:hover {
      background: #222;
    }
    .scene {
      max-width: 800px;
      text-align: center;
    }
  </style>
</head>

<body>
  <div id="intro" style="position:absolute; top:0; left:0; width:100%; height:100%; background:#000; color:#0f0; font-size:18px; display:flex; flex-direction:column; justify-content:center; align-items:center; z-index:1000;">
    <h1 style="text-shadow:0 0 5px #0f0;">🕷 ТЛЕН-2000 🕷</h1>
    <p>нажми любую клавишу, чтобы начать</p>
  </div>

  <div class="scene">
    <h1 class="vhs">ТЛЕН-2000</h1>
    <p id="sceneText">Ты стоишь на старой автобусной остановке. Мерцает фонарь. Внутри — бутылка, газета и тишина, сквозь которую пробивается ветер из прошлого.</p>

    <div id="choices">
      <button class="btn" onclick="choose(1)">1. Осмотреть газету</button>
      <button class="btn" onclick="choose(2)">2. Взять бутылку</button>
      <button class="btn" onclick="choose(3)">3. Прислушаться к ночи</button>
    </div>

    <p id="response"></p>
  </div>

  <audio autoplay loop>
    <source src="postpunk.mp3" type="audio/mpeg">
  </audio>

  <script>
    function choose(option) {
      let res = '';
      let nextScene = false;

      if(option === 1) {
        res = "Газета от 1996 года. Заголовок: 'Пропал подросток в панельном районе. Найден ботинок'.";
        localStorage.setItem('tlen2000_progress', 'gazeta');
        nextScene = true;
      }
      if(option === 2) {
        res = "Бутылка пуста. Но внутри гремит что-то — обрывок записки: 'НЕ ЖДИ НИЧЕГО'.";
        localStorage.setItem('tlen2000_progress', 'butylka');
        nextScene = true;
      }
      if(option === 3) {
        res = "Ветер доносит далёкое пение. Кто-то шепчет твоё имя из темноты.";
        localStorage.setItem('tlen2000_progress', 'noch');
        nextScene = true;
      }

      document.getElementById('response').innerText = res;

      if(nextScene) {
        setTimeout(() => {
          advanceToNext();
        }, 4000);
      }
    }

    function advanceToNext() {
      document.getElementById('sceneText').innerText = "Ты отходишь от остановки, оглядываясь. Позади всё тот же фонарь. Впереди — тёмные силуэты гаражей, за которыми маячит знакомый двор...";
      document.getElementById('response').innerText = '';
      document.getElementById('choices').innerHTML = `
        <button class="btn" onclick="enterGarages()">Идти через гаражи во двор</button>
      `;
    }

    function enterGarages() {
      document.getElementById('sceneText').innerText = "Гаражи будто застыли во времени. Между ржавыми корпусами — мусор, тишина и дымок, ползущий из одной из щелей. Ты идёшь осторожно. Слышны тихие шаги за спиной...";
      document.getElementById('choices').innerHTML = `
        <button class="btn" onclick="intoYard()">Выбраться во двор</button>
      `;
    }

    function intoYard() {
      document.getElementById('sceneText').innerText = "Ты выходишь во двор панельной девятиэтажки. Всё тут знакомо — сломанные качели, обгоревшая урна, следы чьих-то кроссовок на снегу. Ты один, или почти один...";
      document.getElementById('choices').innerHTML = `
        <button class="btn" onclick="goToPlayground()">Осмотреть детскую площадку</button>
        <button class="btn" onclick="enterPorch()">Зайти в подъезд</button>
      `;
    }

    function goToPlayground() {
      document.getElementById('sceneText').innerText = "На площадке — обгрызенные качели и перевёрнутая машинка. Между ними валяется плюшевый мишка. Вокруг бутылки, обёртки, пятна. Кажется, мишка смотрит прямо на тебя...";
      document.getElementById('choices').innerHTML = `
        <button class="btn" onclick="enterPorch()">Оставить мишку и зайти в подъезд</button>
        <button class="btn" onclick="inspectToy()">Поднять игрушку</button>
      `;
    }

    function inspectToy() {
      document.getElementById('sceneText').innerText = "Мишка тёплый. На лапке — пришитый инициал: 'К.Ю.'. Внутри — что-то твёрдое. Ты сжимаешь его... и слышишь тихий хрип. Ты отпускаешь его на землю. Он больше не тёплый.";
      document.getElementById('choices').innerHTML = `
        <button class="btn" onclick="enterPorch()">Зайти в подъезд</button>
      `;
    }

    function enterPorch() {
      document.getElementById('sceneText').innerText = "Подъезд встречает тебя запахом пыли и кошачьей мочи. Лампочка мигает. Объявления шуршат на сквозняке. Что-то шевелится в темноте...";
      document.getElementById('choices').innerHTML = `
        <button class="btn" onclick="inspectCorner()">Заглянуть в тёмный угол</button>
        <button class="btn" onclick="checkMailboxes()">Осмотреть почтовые ящики</button>
        <button class="btn" onclick="knockDoor()">Постучать в дверь</button>
        <button class="btn" onclick="goUpstairs()">Подняться выше</button>
      `;
    }

    function inspectCorner() {
      document.getElementById('sceneText').innerText = "Ты видишь бледное лицо ребёнка, на миг появляющееся в темноте. Оно исчезает, а на полу — старый детский ботинок. Он ещё тёплый...";
    }

    function checkMailboxes() {
      document.getElementById('sceneText').innerText = "На одном из ящиков написано 'УЖЕ НЕ ЖДИ'. Внутри другого — клок волос и открытка с Новым годом. Все ящики скрипят, будто сопротивляются.";
    }

    function knockDoor() {
      document.getElementById('sceneText').innerText = "Ты стучишь. Сначала тишина. Потом — тихое постукивание в ответ. Потом... звук, будто дверь тихо приоткрылась. Но за ней — только тьма.";
    }

    function goUpstairs() {
      document.getElementById('sceneText').innerText = "Ты поднимаешься. Ступени мокрые, словно кто-то только что прошёл босиком. Впереди — ещё этажи, ещё темнее, ещё ближе к тому, что ты пытался забыть.";
      document.getElementById('choices').innerHTML = `<p>Продолжение следует...</p>`;
    }

    
document.addEventListener('keydown', () => {
  const intro = document.getElementById('intro');
  if (intro) intro.style.display = 'none';
}, { once: true });

window.onload = function() {

      const progress = localStorage.getItem('tlen2000_progress');
      if(progress) {
        document.getElementById('response').innerText = "Ты чувствуешь дежавю. Это место будто помнит тебя.";
      }
    }
  </script>
</body>
</html>
