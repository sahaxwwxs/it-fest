<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Умный Город Алматы | Цифровая платформа</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background: #f4f7fc; color: #1e2a3e; scroll-behavior: smooth; }
        .header { background: linear-gradient(135deg, #0b2b3b 0%, #1a4a5f 100%); color: white; padding: 1rem 2rem; position: sticky; top: 0; z-index: 1000; box-shadow: 0 8px 20px rgba(0,0,0,0.1); }
        .nav-container { max-width: 1400px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
        .nav-links { display: flex; gap: 1.5rem; list-style: none; }
        .nav-links a { color: white; text-decoration: none; font-weight: 500; font-size: 0.95rem; }
        .hero { background: linear-gradient(rgba(0,20,30,0.7), rgba(0,20,30,0.7)), url('https://images.unsplash.com/photo-1582516598075-4c2412a92c5b?q=80&w=2070&auto=format') center/cover; padding: 4rem 2rem; text-align: center; color: white; }
        .container { max-width: 1300px; margin: 2rem auto; padding: 0 1.5rem; }
        .section-title { font-size: 1.8rem; font-weight: 700; margin-bottom: 1.5rem; border-left: 6px solid #1a4a5f; padding-left: 1rem; }
        .cards-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 1.8rem; margin-bottom: 2rem; }
        .card { background: white; border-radius: 1.5rem; padding: 1.5rem; box-shadow: 0 10px 20px rgba(0,0,0,0.05); transition: 0.2s; }
        .card:hover { transform: translateY(-5px); }
        #cityMap { height: 500px; width: 100%; border-radius: 1.5rem; }
        .chat-widget { position: fixed; bottom: 25px; right: 25px; z-index: 1200; }
        .chat-toggle { background: #1a4a5f; width: 60px; height: 60px; border-radius: 50%; display: flex; align-items: center; justify-content: center; cursor: pointer; color: white; font-size: 1.8rem; box-shadow: 0 5px 15px rgba(0,0,0,0.3); }
        .chat-box { position: absolute; bottom: 80px; right: 0; width: 350px; height: 480px; background: white; border-radius: 1rem; box-shadow: 0 15px 30px rgba(0,0,0,0.2); display: flex; flex-direction: column; overflow: hidden; border: 1px solid #ddd; }
        .chat-header { background: #1a4a5f; color: white; padding: 0.8rem; display: flex; justify-content: space-between; }
        .chat-messages { flex: 1; padding: 1rem; overflow-y: auto; background: #f9fafb; display: flex; flex-direction: column; gap: 10px; }
        .bubble { padding: 0.6rem 1rem; border-radius: 15px; max-width: 85%; font-size: 0.9rem; line-height: 1.4; }
        .user-msg { align-self: flex-end; background: #1a4a5f; color: white; border-bottom-right-radius: 2px; }
        .bot-msg { align-self: flex-start; background: #e5e7eb; color: #1e293b; border-bottom-left-radius: 2px; }
        .chat-input-area { display: flex; padding: 10px; border-top: 1px solid #eee; }
        .chat-input-area input { flex: 1; border: none; background: #f1f5f9; padding: 10px 15px; border-radius: 20px; outline: none; }
        .chat-input-area button { background: #1a4a5f; color: white; border: none; padding: 0 15px; margin-left: 5px; border-radius: 50%; cursor: pointer; }
        .hidden { display: none; }
        .quiz-options button { margin: 5px; padding: 8px 15px; border-radius: 20px; border: 1px solid #ddd; cursor: pointer; background: #fff; transition: 0.2s; }
        .quiz-options button:hover { background: #f0f4f8; }
    </style>
</head>
<body>

<header class="header">
    <div class="nav-container">
        <div class="logo">
            <h1>Smart Almaty <i class="fas fa-microchip"></i></h1>
        </div>
        <ul class="nav-links">
            <li><a href="#services">Услуги</a></li>
            <li><a href="#map">Карта</a></li>
            <li><a href="#game">Игра</a></li>
            <li><a href="#transport">Транспорт</a></li>
        </ul>
    </div>
</header>

<section class="hero">
    <h2>Цифровая Экосистема Алматы</h2>
    <p>Ваш город в одном окне: от транспорта до экологии.</p>
</section>

<div class="container">
    <div id="services">
        <div class="section-title">Городские сервисы</div>
        <div class="cards-grid">
            <div class="card"><i class="fas fa-bus fa-2x"></i><h3>Транспорт</h3><p>Маршруты в реальном времени.</p></div>
            <div class="card"><i class="fas fa-tree fa-2x"></i><h3>Экология</h3><p>Мониторинг качества воздуха.</p></div>
            <div class="card"><i class="fas fa-hospital fa-2x"></i><h3>Медицина</h3><p>Запись к врачу онлайн.</p></div>
        </div>
    </div>

    <div id="map">
        <div class="section-title">Интерактивная карта</div>
        <div id="cityMap"></div>
    </div>

    <div id="game" class="card" style="margin-top: 2rem; background: #eef2f5;">
        <div class="section-title">Эко-Квест</div>
        <div id="quizBlock">
            <p id="quizQuestion" style="font-weight: 600; font-size: 1.1rem;"></p>
            <div id="quizOptions" class="quiz-options"></div>
            <p id="quizFeedback" style="margin-top: 10px; font-weight: 500;"></p>
            <button id="nextQuizBtn" style="margin-top:10px; background:#1a4a5f; color:white; border:none; padding:8px 20px; border-radius:20px; cursor:pointer;">Далее</button>
        </div>
    </div>

    <div id="transport" style="margin-top: 2rem;">
        <div class="section-title">Транспорт онлайн</div>
        <div class="card" id="transportData">
            <p><i class="fas fa-traffic-light"></i> <strong>Загруженность:</strong> 3 балла</p>
            <p><i class="fas fa-parking"></i> <strong>Парковки:</strong> 245 мест свободно</p>
            <button onclick="refreshTransport()" style="margin-top:10px; background:#1a4a5f; color:white; border:none; padding:8px 20px; border-radius:20px; cursor:pointer;">Обновить данные</button>
        </div>
    </div>
</div>

<div class="chat-widget">
    <div class="chat-toggle" id="chatToggle"><i class="fas fa-comment-dots"></i></div>
    <div id="chatBox" class="chat-box hidden">
        <div class="chat-header">
            <span><i class="fas fa-robot"></i> Almaty AI</span>
            <span id="closeChat" style="cursor:pointer;">&times;</span>
        </div>
        <div class="chat-messages" id="chatMessages">
            <div class="bot-msg bubble">Привет! Я ИИ-помощник Алматы. Чем могу помочь? (транспорт, погода, экология)</div>
        </div>
        <div class="chat-input-area">
            <input type="text" id="chatInput" placeholder="Ваш вопрос...">
            <button id="sendChatBtn"><i class="fas fa-paper-plane"></i></button>
        </div>
    </div>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
    // 1. КАРТА
    const map = L.map('cityMap').setView([43.2567, 76.9286], 13);
    L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png').addTo(map);
    L.marker([43.2500, 76.9560]).addTo(map).bindPopup('Кок-Тобе');

    // 2. ТРАНСПОРТ (Исправлено)
    function refreshTransport() {
        const data = document.getElementById('transportData');
        const jams = Math.floor(Math.random() * 10) + 1;
        const spots = Math.floor(Math.random() * 500);
        data.innerHTML = `
            <p><i class="fas fa-traffic-light"></i> <strong>Загруженность:</strong> ${jams} баллов</p>
            <p><i class="fas fa-parking"></i> <strong>Парковки:</strong> ${spots} мест свободно</p>
            <button onclick="refreshTransport()" style="margin-top:10px; background:#1a4a5f; color:white; border:none; padding:8px 20px; border-radius:20px; cursor:pointer;">Обновить данные</button>
        `;
    }

    // 3. ВИКТОРИНА
    const quizData = [
        { q: "Экологичный транспорт Алматы?", a: ["Авто", "Велошеринг", "Мотоцикл"], c: 1 },
        { q: "Высота телебашни Кок-Тобе?", a: ["200м", "372м", "500м"], c: 1 }
    ];
    let curQ = 0;
    function loadQuiz() {
        const q = quizData[curQ];
        document.getElementById('quizQuestion').innerText = q.q;
        const opt = document.getElementById('quizOptions');
        opt.innerHTML = '';
        q.a.forEach((text, i) => {
            const btn = document.createElement('button');
            btn.innerText = text;
            btn.onclick = () => {
                const feed = document.getElementById('quizFeedback');
                if(i === q.c) feed.innerHTML = "✅ Правильно!";
                else feed.innerHTML = "❌ Ошибка!";
                Array.from(opt.children).forEach(b => b.disabled = true);
            };
            opt.appendChild(btn);
        });
        document.getElementById('quizFeedback').innerHTML = '';
    }
    document.getElementById('nextQuizBtn').onclick = () => {
        curQ = (curQ + 1) % quizData.length;
        loadQuiz();
    };
    loadQuiz();

    // 4. ИИ-ЧАТ (Полностью исправлено)
    const chatInput = document.getElementById('chatInput');
    const chatMsgs = document.getElementById('chatMessages');

    function sendBotMessage(text) {
        const div = document.createElement('div');
        div.className = 'bot-msg bubble';
        div.innerText = text;
        chatMsgs.appendChild(div);
        chatMsgs.scrollTop = chatMsgs.scrollHeight;
    }

    function handleUserMsg() {
        const val = chatInput.value.trim().toLowerCase();
        if(!val) return;

        const uDiv = document.createElement('div');
        uDiv.className = 'user-msg bubble';
        uDiv.innerText = chatInput.value;
        chatMsgs.appendChild(uDiv);
        chatInput.value = '';
        chatMsgs.scrollTop = chatMsgs.scrollHeight;

        setTimeout(() => {
            if(val.includes('транспорт')) sendBotMessage("🚍 Автобусы ходят по расписанию, метро до 00:00.");
            else if(val.includes('погода')) sendBotMessage("☀️ В Алматы сегодня +22°C, отличный день для прогулки!");
            else if(val.includes('экология')) sendBotMessage("🌿 AQI сегодня 55 (Норма). Воздух чистый.");
            else sendBotMessage("🤖 Я вас понял, но пока могу отвечать только про транспорт, погоду и экологию.");
        }, 600);
    }

    document.getElementById('sendChatBtn').onclick = handleUserMsg;
    chatInput.onkeydown = (e) => { if(e.key === 'Enter') handleUserMsg(); };
    document.getElementById('chatToggle').onclick = () => document.getElementById('chatBox').classList.toggle('hidden');
    document.getElementById('closeChat').onclick = () => document.getElementById('chatBox').classList.add('hidden');
</script>

</body>
</html>
