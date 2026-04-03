
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>CityPulse AI | Алматы — Умный город с картой, викториной и AI</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <script src="https://api-maps.yandex.ru/2.1/?apikey=ваш_ключ_яндекс_карт&lang=ru_RU" type="text/javascript"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Inter', sans-serif;
            background: radial-gradient(circle at 20% 30%, #0A0F1A, #03060C);
            color: #EEF5FF;
            overflow-x: hidden;
        }
        .glass {
            background: rgba(10, 22, 40, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(0, 230, 255, 0.3);
            border-radius: 2rem;
        }
        .wrapper {
            max-width: 1600px;
            margin: 0 auto;
            padding: 1.2rem;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            padding: 0.8rem 2rem;
            background: rgba(5, 15, 25, 0.85);
            backdrop-filter: blur(16px);
            border-radius: 3rem;
            margin-bottom: 2rem;
            border-bottom: 2px solid #0ff;
        }
        .logo h1 {
            font-size: 1.8rem;
            background: linear-gradient(135deg, #A5F0FF, #2A9D8F);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 0.7rem;
            margin-bottom: 2rem;
            justify-content: center;
        }
        .tab-btn {
            background: rgba(20,35,55,0.8);
            border: 1px solid #2c6e8f;
            padding: 0.6rem 1.4rem;
            border-radius: 2rem;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            font-size: 0.85rem;
        }
        .tab-btn.active {
            background: #00c8ff;
            color: #02121f;
            box-shadow: 0 0 18px cyan;
        }
        .tab-pane { display: none; animation: fade 0.3s ease; }
        .tab-pane.active-pane { display: block; }
        @keyframes fade { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .grid-2 { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1.5rem; }
        .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem; }
        .grid-4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1.2rem; }
        .card {
            background: rgba(12,22,40,0.75);
            backdrop-filter: blur(8px);
            border-radius: 1.8rem;
            padding: 1.5rem;
            border: 1px solid rgba(0,200,255,0.25);
            margin-bottom: 1rem;
        }
        .metric {
            background: #071a28;
            border-radius: 1.2rem;
            padding: 0.8rem;
            margin: 0.6rem 0;
            border-left: 3px solid #0ff;
        }
        #yandexMap { width: 100%; height: 520px; border-radius: 1.8rem; border: 2px solid cyan; }
        .quiz-option {
            background: #1e2a44;
            margin: 0.5rem;
            padding: 0.8rem;
            border-radius: 2rem;
            cursor: pointer;
            text-align: center;
            transition: 0.1s;
        }
        .quiz-option:hover { background: #2c6e9e; transform: scale(0.98); }
        .chat-fixed {
            position: fixed; bottom: 25px; right: 25px; width: 370px;
            background: #0b1424ee; backdrop-filter: blur(20px);
            border-radius: 1.5rem; border: 1px solid #0ff; z-index: 1000;
        }
        .chat-header {
            background: #0a2a3b; padding: 12px 18px;
            border-radius: 1.5rem 1.5rem 0 0; cursor: pointer;
            display: flex; justify-content: space-between;
        }
        .chat-body { display: flex; flex-direction: column; height: 420px; padding: 12px; }
        .chat-msgs { flex: 1; overflow-y: auto; font-size: 0.85rem; margin-bottom: 10px; }
        .chat-input-area { display: flex; gap: 8px; }
        .chat-input-area input { flex: 1; background: #1a2a3f; border: none; border-radius: 2rem; padding: 10px; color: white; }
        .btn-glow { background: #00c8ff; border: none; padding: 8px 14px; border-radius: 2rem; font-weight: bold; cursor: pointer; }
        @media (max-width: 1000px) { .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; } .chat-fixed { width: 300px; } }
        footer { text-align: center; margin-top: 2rem; font-size: 0.7rem; opacity: 0.6; }
        .legend { display: flex; gap: 1rem; margin-top: 0.5rem; flex-wrap: wrap; }
        .legend span { display: inline-block; width: 20px; height: 20px; border-radius: 50%; margin-right: 5px; }
    </style>
</head>
<body>
<div class="wrapper">
    <div class="header">
        <div class="logo"><h1><i class="fas fa-city"></i> CityPulse AI | Алматы</h1></div>
        <div class="glass" style="padding: 0.3rem 1rem;"><i class="fas fa-brain"></i> AI онлайн | <span id="liveClock"></span></div>
    </div>

    <div class="tabs">
        <div class="tab-btn active" data-tab="dashboard">📊 Дашборд</div>
        <div class="tab-btn" data-tab="yandexmap">🗺️ Карта (зоны)</div>
        <div class="tab-btn" data-tab="quiz">🎮 Викторина</div>
        <div class="tab-btn" data-tab="transport">🚇 Транспорт</div>
        <div class="tab-btn" data-tab="traffic">🚦 Пробки</div>
        <div class="tab-btn" data-tab="ecology">🌿 Экология</div>
        <div class="tab-btn" data-tab="safety">🛡️ Безопасность</div>
        <div class="tab-btn" data-tab="info">📰 Инфо-центр</div>
    </div>

    <!-- Дашборд -->
    <div id="dashboard" class="tab-pane active-pane">
        <div class="grid-2">
            <div class="card">
                <h3>📈 Метрики города</h3>
                <div class="metric">🚦 Загруженность: <span id="trafficVal">52</span>%</div>
                <div class="metric">🌫️ AQI: <span id="aqiVal">44</span></div>
                <div class="metric">🚨 Инциденты: <span id="incidentVal">2</span></div>
                <button id="refreshBtn" class="btn-glow">🔄 Обновить</button>
            </div>
            <div class="card">
                <h3>🤖 AI-рекомендация</h3>
                <p id="aiReco">Система стабильна. Оптимальный режим.</p>
                <canvas id="trafficChart" style="height:150px;"></canvas>
            </div>
        </div>
    </div>

    <!-- Карта с цветными зонами -->
    <div id="yandexmap" class="tab-pane">
        <div class="card">
            <h3><i class="fas fa-map"></i> Карта Алматы — цветные зоны</h3>
            <div class="legend">
                <div><span style="background: #00ff00;"></span> Зелёные зоны (парки, экология)</div>
                <div><span style="background: #ff0000;"></span> Красные зоны (пробки, ДТП)</div>
                <div><span style="background: #0088ff;"></span> Камеры и датчики</div>
            </div>
            <div id="yandexMap"></div>
        </div>
    </div>

    <!-- Викторина про Алматы -->
    <div id="quiz" class="tab-pane">
        <div class="card">
            <h3><i class="fas fa-gamepad"></i> Викторина: Знаешь ли ты Алматы?</h3>
            <div id="quizArea">
                <p id="quizQuestion" style="font-size:1.3rem; margin:1rem 0;">Загрузка...</p>
                <div id="quizOptions"></div>
                <p id="quizResult" style="margin-top:1rem; font-weight:bold;"></p>
                <button id="nextQuizBtn" class="btn-glow">Следующий вопрос →</button>
            </div>
        </div>
    </div>

    <!-- Транспорт -->
    <div id="transport" class="tab-pane">
        <div class="grid-3">
            <div class="card"><h3>🚇 Метро</h3><p>11 станций, 13 км. Планируется расширение до аэропорта.</p></div>
            <div class="card"><h3>🚌 Автобусы</h3><p>120 маршрутов, 1500 машин. 30% электробусы.</p></div>
            <div class="card"><h3>🚲 Вело</h3><p>50 км велодорожек, прокат CityBike.</p></div>
        </div>
    </div>

    <!-- Пробки -->
    <div id="traffic" class="tab-pane">
        <div class="grid-2">
            <div class="card"><h3>🚦 Текущие пробки</h3><p>Балл: <span id="jamScore">5</span>/10. Загруженные участки: пр. Абая, ул. Толе би.</p></div>
            <div class="card"><h3>⏰ Часы пик</h3><p>Утро: 8:00-9:30, вечер: 17:30-19:00.</p></div>
            <div class="card"><h3>🚦 Умные светофоры</h3><p>85 адаптивных светофоров. Снижение заторов на 27%.</p></div>
            <div class="card"><h3>🛣️ Новые развязки</h3><p>Развязка на пр. Аль-Фараби — 2026 год.</p></div>
        </div>
    </div>

    <!-- Экология -->
    <div id="ecology" class="tab-pane">
        <div class="grid-2">
            <div class="card"><h3>🌫️ Качество воздуха</h3><p>AQI сейчас: <span id="aqiInfo">44</span>. Зелёные зоны: Кок-Тюбе, Медеу, Ботанический сад.</p></div>
            <div class="card"><h3>🌳 Парки</h3><p>28 панфиловцев, Первого президента, Botanical Garden.</p></div>
            <div class="card"><h3>🔋 Электротранспорт</h3><p>100 зарядных станций для электромобилей.</p></div>
            <div class="card"><h3>♻️ Переработка</h3><p>35% сортировки мусора, новый завод в 2025.</p></div>
        </div>
    </div>

    <!-- Безопасность -->
    <div id="safety" class="tab-pane">
        <div class="grid-2">
            <div class="card"><h3>📹 Камеры</h3><p>5200 камер с AI-распознаванием.</p></div>
            <div class="card"><h3>🚑 Экстренные службы</h3><p>Полиция: 7 мин, скорая: 9 мин.</p></div>
            <div class="card"><h3>📢 Оповещение</h3><p>SMS и CityPulse при ЧС.</p></div>
            <div class="card"><h3>📉 Преступность</h3><p>Снижение на 18% за год.</p></div>
        </div>
    </div>

    <!-- Инфо-центр -->
    <div id="info" class="tab-pane">
        <div class="grid-3">
            <div class="card"><h3>🏔️ Достопримечательности</h3><p>Медеу, Чимбулак, Кок-Тюбе, Вознесенский собор, Зелёный базар.</p></div>
            <div class="card"><h3>🍽️ Рестораны</h3><p>Auyl, Kishlak, Navat. Бешбармак, плов, баурсаки.</p></div>
            <div class="card"><h3>🏨 Отели</h3><p>Ritz-Carlton, InterContinental, Rahat Palace.</p></div>
            <div class="card"><h3>🎉 События</h3><p>Алматы Марафон, Jazz Festival, Apple Festival.</p></div>
            <div class="card"><h3>🎓 Образование</h3><p>КазНУ, КБТУ, КИМЭП.</p></div>
            <div class="card"><h3>🏥 Медицина</h3><p>SEMA, Центральная больница.</p></div>
        </div>
    </div>
    <footer>⚡ CityPulse AI — карта с цветными зонами, викторина, AI отвечает на всё про Алматы</footer>
</div>

<!-- AI Чат -->
<div class="chat-fixed" id="chatFixed">
    <div class="chat-header" id="chatHeader">
        <span><i class="fas fa-robot"></i> CityPulse AI — спроси про Алматы</span>
        <span><i class="fas fa-chevron-down"></i></span>
    </div>
    <div class="chat-body" id="chatBody">
        <div class="chat-msgs" id="chatMessages">
            <div>🤖 Привет! Я AI-помощник. Задай любой вопрос про Алматы: транспорт, пробки, экология, безопасность, достопримечательности, еда, погода — я отвечу!</div>
        </div>
        <div class="chat-input-area">
            <input type="text" id="chatInput" placeholder="Напишите вопрос...">
            <button id="sendChatBtn" class="btn-glow">➤</button>
        </div>
    </div>
</div>

<script>
    // ========== МЕТРИКИ ==========
    let traffic = 52, aqi = 44, incidents = 2;
    let trafficChart;
    function updateMetrics() {
        document.getElementById('trafficVal').innerText = traffic;
        document.getElementById('aqiVal').innerText = aqi;
        document.getElementById('incidentVal').innerText = incidents;
        document.getElementById('jamScore').innerText = Math.floor(traffic/10);
        document.getElementById('aqiInfo').innerText = aqi;
        let advice = traffic > 75 ? "🚨 Сильные пробки! Объезжайте центр." : (aqi > 85 ? "🌫️ Плохой воздух, носите маски." : "✅ Система стабильна.");
        document.getElementById('aiReco').innerHTML = advice;
        if(trafficChart) {
            let newData = [...trafficChart.data.datasets[0].data.slice(1), traffic];
            trafficChart.data.datasets[0].data = newData;
            trafficChart.update();
        }
    }
    document.getElementById('refreshBtn').addEventListener('click', () => {
        traffic = Math.floor(Math.random() * 85) + 15;
        aqi = Math.floor(Math.random() * 110) + 20;
        incidents = Math.floor(Math.random() * 5);
        updateMetrics();
    });
    function initChart() {
        const ctx = document.getElementById('trafficChart').getContext('2d');
        trafficChart = new Chart(ctx, {
            type: 'line',
            data: { labels: ['10:00','11:00','12:00','13:00','14:00','15:00'], datasets: [{ label: 'Трафик %', data: [32, 45, 51, 48, 58, traffic], borderColor: '#0ff', fill: true }] },
            options: { responsive: true, maintainAspectRatio: true }
        });
    }
    updateMetrics(); initChart();

    // ========== ЯНДЕКС КАРТА С ЦВЕТНЫМИ ЗОНАМИ ==========
    let yandexMap = null;
    function initYandexMap() {
        if(typeof ymaps === 'undefined') { setTimeout(initYandexMap, 500); return; }
        ymaps.ready(function() {
            yandexMap = new ymaps.Map("yandexMap", {
                center: [43.238949, 76.889709],
                zoom: 12,
                controls: ["zoomControl", "fullscreenControl"]
            });
            // 🟢 ЗЕЛЁНЫЕ ЗОНЫ (парки, экология)
            let greenZones = [
                { coords: [43.258, 76.945], name: "Парк 28 гвардейцев-панфиловцев" },
                { coords: [43.232, 76.912], name: "Central Park" },
                { coords: [43.221, 76.896], name: "Ботанический сад" },
                { coords: [43.270, 76.910], name: "Кок-Тюбе (зелёная зона)" },
                { coords: [43.158, 76.990], name: "Медеу (экозона)" }
            ];
            greenZones.forEach(z => {
                let placemark = new ymaps.Placemark(z.coords, { balloonContent: 🌿 ${z.name} — зелёная зона }, { preset: "islands#greenDotIcon" });
                yandexMap.geoObjects.add(placemark);
            });
            // 🔴 КРАСНЫЕ ЗОНЫ (пробки, ДТП, аварийные участки)
            let redZones = [
                { coords: [43.2567, 76.9286], name: "Пробка на пр. Абая" },
                { coords: [43.2231, 76.8542], name: "ДТП, пробка" },
                { coords: [43.241, 76.953], name: "Затор на пр. Райымбека" },
                { coords: [43.205, 76.882], name: "Аварийный участок" },
                { coords: [43.280, 76.920], name: "Пробка в час пик" }
            ];
            redZones.forEach(z => {
                let placemark = new ymaps.Placemark(z.coords, { balloonContent: ⚠️ ${z.name} — высокая загруженность }, { preset: "islands#redIcon" });
                yandexMap.geoObjects.add(placemark);
            });
            // 🔵 СИНИЕ ЗОНЫ (камеры, датчики)
            let blueZones = [
                { coords: [43.270, 76.910], name: "Камера видеонаблюдения" },
                { coords: [43.284, 76.924], name: "Датчик шума" },
                { coords: [43.218, 76.815], name: "Умный светофор" },
                { coords: [43.250, 76.880], name: "Датчик качества воздуха" }
            ];
            blueZones.forEach(z => {
                let placemark = new ymaps.Placemark(z.coords, { balloonContent: 🔵 ${z.name} }, { preset: "islands#blueIcon" });
                yandexMap.geoObjects.add(placemark);
            });
        });
    }

    // ========== ВИКТОРИНА (работает) ==========
    const quizData = [
        { q: "Как называется знаменитый каток в горах Алматы?", opts: ["Медеу", "Шымбулак", "Кок-Тюбе"], correct: 0 },
        { q: "Какая самая длинная улица в Алматы?", opts: ["пр. Абая", "ул. Толе би", "пр. Аль-Фараби"], correct: 2 },
        { q: "Какой парк является самым старым в Алматы?", opts: ["Центральный парк", "Парк 28 панфиловцев", "Ботанический сад"], correct: 1 },
        { q: "Какая гора видна из центра с телевышкой?", opts: ["Пик Талгар", "Кок-Тюбе", "Хан-Тенгри"], correct: 1 },
        { q: "Какой вид транспорта есть в Алматы кроме автобусов?", opts: ["Троллейбус", "Метро", "Трамвай"], correct: 1 },
        { q: "Какой национальный напиток популярен в Алматы?", opts: ["Кумыс", "Шубат", "Чай"], correct: 0 },
        { q: "В каком году открылось метро в Алматы?", opts: ["2011", "2012", "2010"], correct: 0 }
    ];
    let currentQ = 0, score = 0;
    function loadQuiz() {
        const q = quizData[currentQ];
        document.getElementById('quizQuestion').innerText = q.q;
        const optsDiv = document.getElementById('quizOptions');
        optsDiv.innerHTML = '';
        q.opts.forEach((opt, idx) => {
            const btn = document.createElement('div');
            btn.className = 'quiz-option';
            btn.innerText = opt;
            btn.onclick = () => {
                if(idx === q.correct) { score++; document.getElementById('quizResult').innerHTML = ✅ Верно! Счёт: ${score}/${quizData.length}; }
                else { document.getElementById('quizResult').innerHTML = ❌ Неверно! Правильно: ${q.opts[q.correct]}. Счёт: ${score}/${quizData.length}; }
                setTimeout(() => {
                    if(currentQ + 1 < quizData.length) { currentQ++; loadQuiz(); }
                    else { document.getElementById('quizResult').innerHTML = 🏆 Игра окончена! Результат: ${score}/${quizData.length}; document.getElementById('quizOptions').innerHTML = ''; }
                }, 1200);
            };
            optsDiv.appendChild(btn);
        });
    }
    document.getElementById('nextQuizBtn').addEventListener('click', () => { if(currentQ < quizData.length-1) { currentQ++; loadQuiz(); } });
    loadQuiz();

    // ========== AI ПОМОЩНИК (отвечает на ВСЕ вопросы про Алматы) ==========
    function aiAnswer(question) {
        const q = question.toLowerCase();
        if(q.includes('транспорт') || q.includes('метро') || q.includes('автобус')) {
            return "🚍 В Алматы 11 станций метро, 120 автобусных маршрутов, 30% электробусов. AI оптимизирует расписание.";
        }
        if(q.includes('пробк') || q.includes('трафик') || q.includes('затор')) {
            return 🚦 Сейчас загруженность: ${traffic}%. Пробки на пр. Абая и ул. Толе би. Рекомендуем объездные пути.;
        }
        if(q.includes('экологи') || q.includes('воздух') || q.includes('aqi')) {
            return 🌿 AQI сейчас: ${aqi}. ${aqi > 80 ? 'Воздух плохой, носите маски.' : 'Воздух в норме.'} Зелёные зоны: Медеу, Кок-Тюбе, Ботанический сад.;
        }
        if(q.includes('безопасн') || q.includes('преступн') || q.includes('камер')) {
            return "🛡️ 5200 камер с AI, преступность снизилась на 18%. Время прибытия полиции — 7 минут.";
        }
        if(q.includes('погод') || q.includes('дождь') || q.includes('температур')) {
            return "⛅ Завтра +18°C, переменная облачность. Вечером возможен дождь. Будьте осторожны на дорогах!";
        }
        if(q.includes('достопримечат') || q.includes('медеу') || q.includes('чимбулак') || q.includes('кок-тюбе')) {
            return "🏔️ Главные места: Медеу, Чимбулак, Кок-Тюбе, Вознесенский собор, Зелёный базар. Обязательно посетите!";
        }
        if(q.includes('ресторан') || q.includes('еда') || q.includes('кухня') || q.includes('бешбармак')) {
            return "🍽️ Лучшие рестораны: Auyl, Kishlak, Navat. Попробуйте бешбармак, плов, баурсаки и кумыс!";
        }
        if(q.includes('отель') || q.includes('гостиница') || q.includes('жильё')) {
            return "🏨 Отели: Ritz-Carlton, InterContinental, Rahat Palace, а также бюджетные хостелы в центре.";
        }
        if(q.includes('как дела') || q.includes('привет')) {
            return "✨ Привет! Я CityPulse AI. Готов помочь с любым вопросом об Алматы!";
        }
        return ℹ️ Я AI-помощник. Я знаю про транспорт, пробки (сейчас ${traffic}%), экологию (AQI ${aqi}), безопасность, достопримечательности, рестораны, отели и погоду. Уточните вопрос!;
    }

    async function sendChat() {
        const input = document.getElementById('chatInput');
        const msg = input.value.trim();
        if(!msg) return;
        const chatDiv = document.getElementById('chatMessages');
        const userDiv = document.createElement('div');
        userDiv.innerHTML = <strong>🧑‍💻 Вы:</strong> ${msg};
        userDiv.style.margin = "6px 0";
        chatDiv.appendChild(userDiv);
        input.value = '';
        const thinking = document.createElement('div');
        thinking.innerHTML = <em>🤖 AI думает...</em>;
        chatDiv.appendChild(thinking);
        chatDiv.scrollTop = chatDiv.scrollHeight;
        
        const reply = aiAnswer(msg);
        thinking.remove();
        const botDiv = document.createElement('div');
        botDiv.innerHTML = <strong>🤖 CityPulse AI:</strong> ${reply};
        botDiv.style.margin = "6px 0";
        chatDiv.appendChild(botDiv);
        chatDiv.scrollTop = chatDiv.scrollHeight;
    }

    // ========== ВКЛАДКИ ==========
    function initTabs() {
        const btns = document.querySelectorAll('.tab-btn');
        const panes = document.querySelectorAll('.tab-pane');
        btns.forEach(btn => {
            btn.addEventListener('click', () => {
                const tabId = btn.getAttribute('data-tab');
                btns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                panes.forEach(p => p.classList.remove('active-pane'));
                document.getElementById(tabId).classList.add('active-pane');
                if(tabId === 'yandexmap' && !yandexMap) initYandexMap();
                else if(tabId === 'yandexmap' && yandexMap) setTimeout(() => yandexMap.container.fitToViewport(), 100);
            });
        });
    }

    // чат
    let chatVisible = true;
    document.getElementById('chatHeader').addEventListener('click', () => {
        const body = document.getElementById('chatBody');
        chatVisible = !chatVisible;
        body.style.display = chatVisible ? 'flex' : 'none';
    });
    function updateClock() { document.getElementById('liveClock').innerText = new Date().toLocaleTimeString(); }
    setInterval(updateClock, 1000);
    updateClock();
    setInterval(() => { traffic = Math.floor(Math.random() * 85) + 15; updateMetrics(); }, 20000);

    document.getElementById('sendChatBtn').addEventListener('click', sendChat);
    document.getElementById('chatInput').addEventListener('keypress', (e) => { if(e.key === 'Enter') sendChat(); });
    initTabs();
    updateMetrics();
</script>
</body>
</html>
