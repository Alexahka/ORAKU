<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Оракул судьбы</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: 'Arial', sans-serif;
            background: #0a0a0a;
            overflow: hidden;
            position: relative;
        }

        /* Летающие числа */
        .background-numbers {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .number-particle {
            position: absolute;
            font-size: 24px;
            color: rgba(255, 255, 255, 0.2);
            animation: float 10s linear infinite;
        }

        /* Основной контент */
        .container {
            position: relative;
            z-index: 2;
            max-width: 600px;
            margin: 100px auto;
            text-align: center;
        }

        /* Контейнер для ввода даты */
        .date-inputs {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 30px 0;
        }

        .date-inputs input {
            width: 80px;
            padding: 15px;
            font-size: 20px;
            text-align: center;
            border: none;
            border-radius: 5px;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            transition: 0.3s;
            font-family: 'Courier New', monospace;
            letter-spacing: 2px;
        }

        .date-inputs input:focus {
            background: rgba(255, 255, 255, 0.2);
            outline: none;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
        }

        .date-separator {
            color: #fff;
            font-size: 24px;
            margin-top: 10px;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
        }

        button {
            padding: 18px 40px;
            font-size: 18px;
            color: #fff;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
            font-weight: bold;
        }

        button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.5);
        }

        /* Модальное окно */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 999;
            animation: fadeIn 0.5s;
        }

        .modal-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #1a1a1a;
            padding: 40px;
            border-radius: 15px;
            width: 80%;
            max-width: 500px;
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
            border: 2px solid transparent;
            animation: floatModal 3s infinite;
        }

        .modal-content h2 {
            color: #ffeb3b;
            text-shadow: 0 0 20px rgba(255, 235, 59, 0.7);
            margin-bottom: 20px;
            font-size: 2.2em;
            animation: glow 2s infinite;
        }

        .modal-content p {
            color: #e0e0e0;
            line-height: 1.6;
            font-size: 18px;
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.2);
            position: relative;
            padding: 20px;
        }

        /* Кнопки управления модальным окном */
        .modal-controls {
            position: absolute;
            top: 15px;
            width: 100%;
            display: flex;
            justify-content: space-between;
            pointer-events: none;
        }

        .return-button {
            position: relative;
            left: -15px;
            font-size: 28px;
            color: #fff;
            cursor: pointer;
            transition: 0.3s;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
            pointer-events: auto;
        }

        .return-button:hover {
            transform: translateX(-5px);
            text-shadow: -5px 0 10px rgba(255, 255, 255, 0.5);
        }

        .close-modal {
            position: relative;
            right: -15px;
            font-size: 28px;
            color: #fff;
            cursor: pointer;
            transition: 0.3s;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
            pointer-events: auto;
        }

        .close-modal:hover {
            transform: rotate(180deg);
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
        }

        /* Эффект старинного пергамента */
        .parchment {
            background: #2a2a2a;
            border-radius: 10px;
            padding: 20px;
            position: relative;
            margin: 20px 0;
        }

        .parchment::before {
            content: '';
            position: absolute;
            top: -10px;
            left: -10px;
            right: -10px;
            bottom: -10px;
            border: 2px dashed rgba(255, 255, 255, 0.3);
            border-radius: 15px;
        }

        /* Анимации */
        @keyframes float {
            0% { transform: translateY(0) translateX(0); }
            50% { transform: translateY(-50px) translateX(100px); }
            100% { transform: translateY(0) translateX(-100px); }
        }

        @keyframes floatModal {
            0%, 100% { transform: translate(-50%, -50%) rotate(-1deg); }
            50% { transform: translate(-50%, -50%) rotate(1deg); }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes glow {
            0%, 100% { text-shadow: 0 0 20px rgba(255, 235, 59, 0.7); }
            50% { text-shadow: 0 0 40px rgba(255, 235, 59, 1); }
        }

        .glowing-border {
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 15px rgba(255, 255, 255, 0.3); }
            50% { box-shadow: 0 0 30px rgba(255, 255, 255, 0.6); }
            100% { box-shadow: 0 0 15px rgba(255, 255, 255, 0.3); }
        }

        /* Анимация появления текста */
        .reveal-text {
            opacity: 0;
            animation: reveal 2s forwards;
            animation-delay: 0.5s;
        }

        @keyframes reveal {
            0% { opacity: 0; transform: translateY(20px); }
            100% { opacity: 1; transform: translateY(0); }
        }

        /* Кнопка расширенного прогноза */
        .extended-button {
            margin-top: 20px;
            padding: 15px 30px;
            font-size: 18px;
            color: #fff;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
            animation: pulseExtended 2s infinite;
        }

        .extended-button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.5);
        }

        @keyframes pulseExtended {
            0% { box-shadow: 0 0 15px rgba(255, 255, 255, 0.3); }
            50% { box-shadow: 0 0 30px rgba(255, 255, 255, 0.6); }
            100% { box-shadow: 0 0 15px rgba(255, 255, 255, 0.3); }
        }

        /* Модальное окно для расширенного прогноза */
        .extended-modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 999;
            animation: fadeIn 0.5s;
        }

        .extended-modal-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #1a1a1a;
            padding: 40px;
            border-radius: 15px;
            width: 80%;
            max-width: 600px;
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
            border: 2px solid transparent;
            animation: floatModal 3s infinite;
        }

        .extended-modal-content h2 {
            color: #ffeb3b;
            text-shadow: 0 0 20px rgba(255, 235, 59, 0.7);
            margin-bottom: 20px;
            font-size: 2.2em;
            animation: glow 2s infinite;
        }

        .extended-modal-content input {
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            font-size: 18px;
            text-align: center;
            border: none;
            border-radius: 5px;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            transition: 0.3s;
        }

        .extended-modal-content input:focus {
            background: rgba(255, 255, 255, 0.2);
            outline: none;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
        }

        .extended-modal-content button {
            margin-top: 20px;
            padding: 15px 30px;
            font-size: 18px;
            color: #fff;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
        }

        .extended-modal-content button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.5);
        }

        /* Окно с большим описанием */
        .big-modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 999;
            animation: fadeIn 0.5s;
        }

        .big-modal-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #1a1a1a;
            padding: 40px;
            border-radius: 15px;
            width: 80%;
            max-width: 800px;
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
            border: 2px solid transparent;
            animation: floatModal 3s infinite;
        }

        .big-modal-content h2 {
            color: #ffeb3b;
            text-shadow: 0 0 20px rgba(255, 235, 59, 0.7);
            margin-bottom: 20px;
            font-size: 2.2em;
            animation: glow 2s infinite;
        }

        .big-modal-content p {
            color: #e0e0e0;
            line-height: 1.6;
            font-size: 18px;
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.2);
        }
    </style>
</head>
<body>
    <!-- Летающие числа -->
    <div class="background-numbers" id="backgroundNumbers"></div>

    <!-- Основной контент -->
    <div class="container">
        <h1 style="color: #4CAF50; text-shadow: 0 0 30px rgba(76, 175, 80, 0.7);">Оракул судьбы</h1>
        
        <!-- Поля ввода -->
        <div class="date-inputs">
            <input type="text" id="day" placeholder="ДД" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <span class="date-separator">.</span>
            <input type="text" id="month" placeholder="ММ" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <span class="date-separator">.</span>
            <input type="text" id="year" placeholder="ГГГГ" maxlength="4" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
        </div>

        <button onclick="showPrediction()">Прочитать судьбу</button>

        <!-- Кнопка расширенного прогноза -->
        <button class="extended-button" onclick="openExtendedModal()">Расширенный прогноз</button>
    </div>

    <!-- Модальное окно -->
    <div class="modal-overlay" id="modal">
        <div class="modal-content glowing-border">
            <div class="modal-controls">
                <span class="return-button" onclick="closeModal()">&#8592;</span>
                <span class="close-modal" onclick="closeModal()">&times;</span>
            </div>
            
            <h2 id="modalTitle"></h2>
            <div class="parchment">
                <p class="reveal-text" id="modalText"></p>
            </div>
        </div>
    </div>

    <!-- Модальное окно для расширенного прогноза -->
    <div class="extended-modal-overlay" id="extendedModal">
        <div class="extended-modal-content glowing-border">
            <h2>Расширенный прогноз</h2>
            <input type="text" id="country" placeholder="Страна рождения">
            <input type="text" id="place" placeholder="Место рождения">
            <input type="text" id="firstName" placeholder="Имя">
            <input type="text" id="lastName" placeholder="Фамилия">
            <input type="text" id="middleName" placeholder="Отчество">
            <input type="text" id="extDay" placeholder="День" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <input type="text" id="extMonth" placeholder="Месяц" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <input type="text" id="extYear" placeholder="Год" maxlength="4" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <button onclick="generateBigPrediction()">Начать предсказание</button>
        </div>
    </div>

    <!-- Модальное окно с большим описанием -->
    <div class="big-modal-overlay" id="bigModal">
        <div class="big-modal-content glowing-border">
            <h2>Ваш психологический портрет</h2>
            <p id="bigPredictionText"></p>
            <button onclick="closeBigModal()">Закрыть</button>
        </div>
    </div>

    <script>
        const destinyDescriptions = {
            1: {
                title: "Число Судьбы: 1 - Пионер",
                text: "Ваша душа несёт в себе искру первооткрывателя. Вы создатель новых путей и разрушитель старых границ. Остерегайтесь гордыни, но не бойтесь идти против течения. Ваша миссия - зажигать звёзды там, где другие видят лишь тьму."
            },
            2: {
                title: "Число Судьбы: 2 - Мост",
                text: "Вы - живая связь между мирами. Ваш дар гармонизировать противоречия и соединять сердца. Избегайте самоуничижения, но помните: ваша сила в умении слышать тишину между слов. Вы танцуете в ритме вселенной."
            },
            3: {
                title: "Число Судьбы: 3 - Визионер",
                text: "Ваш разум - призма, расщепляющая реальность на тысячи оттенков. Творчество течёт через вас рекой, но берегитесь рассеянности. Вы рождены, чтобы превращать мечты в материальные формы, как алхимик света."
            },
            4: {
                title: "Число Судьбы: 4 - Страж",
                text: "Ваша сила - в непоколебимости духа. Вы хранитель древних истин и строитель нерушимых основ. Не дайте рутине затушить огонь любопытства. Ваша крепость - это маяк для заблудших."
            },
            5: {
                title: "Число Судьбы: 5 - Вихрь",
                text: "Свобода для вас - не выбор, а дыхание. Вы летите над мирами, собирая опыты как пыльцу. Но помните: бабочка, слишком долго порхающая, никогда не откладывает яйца. Найдите свою точку опоры."
            },
            6: {
                title: "Число Судьбы: 6 - Садовник",
                text: "Ваше сердце - плодородная почва, где растут чужие мечты. Вы лечите трещины в душах, но забываете о своей. Ваша любовь - это магия, способная воскрешать пустыни. Не растворяйтесь в других."
            },
            7: {
                title: "Число Судьбы: 7 - Мудрец",
                text: "Вы видите узоры, скрытые от обычных глаз. Ваш путь - спираль познания, где каждое падение есть подъём. Остерегайтесь холодного рационализма. Истина, которую вы ищете, уже смотрит на вас из зеркала."
            },
            8: {
                title: "Число Судьбы: 8 - Властелин",
                text: "Ваша энергия - смерч, объединяющий материю и дух. Вы создаёте империи, но помните: настоящая власть в умении отпускать. Ваш крест - баланс между тьмой и светом в самом себе."
            },
            9: {
                title: "Число Судьбы: 9 - Пророк",
                text: "Вы храните память о будущем. Ваша жизнь - послание потомкам. Не бойтесь выглядеть безумцем: те, кто смеётся сейчас, будут молиться на ваши слова завтра. Но не забывайте - пророк должен уметь слушать тишину."
            }
        };

        // Генерация летающих чисел
        function createBackgroundNumbers() {
            const container = document.getElementById('backgroundNumbers');
            for (let i = 0; i < 100; i++) {
                const num = document.createElement('div');
                num.className = 'number-particle';
                num.textContent = Math.floor(Math.random() * 9) + 1;
                num.style.left = Math.random() * 100 + 'vw';
                num.style.top = Math.random() * 100 + 'vh';
                num.style.fontSize = (Math.random() * 24 + 12) + 'px';
                num.style.animationDuration = (Math.random() * 5 + 5) + 's';
                num.style.animationDelay = (Math.random() * 5) + 's';
                container.appendChild(num);
            }
        }

        // Показ модального окна
        function showPrediction() {
            if (!validateDate()) {
                alert('Введите полную дату рождения!');
                return;
            }

            const number = calculateDestinyNumber();
            const data = destinyDescriptions[number];
            
            document.getElementById('modalTitle').textContent = data.title;
            document.getElementById('modalText').textContent = data.text;
            document.getElementById('modal').style.display = 'block';
            
            // Анимация появления текста
            setTimeout(() => {
                document.getElementById('modalText').classList.add('reveal-text');
            }, 500);
        }

        // Закрытие модального окна
        function closeModal() {
            document.getElementById('modal').style.display = 'none';
            document.getElementById('modalText').classList.remove('reveal-text');
        }

        // Валидация даты
        function validateDate() {
            const day = document.getElementById('day').value;
            const month = document.getElementById('month').value;
            const year = document.getElementById('year').value;

            return day.length === 2 && month.length === 2 && year.length === 4;
        }

        // Расчет числа судьбы
        function calculateDestinyNumber() {
            const day = document.getElementById('day').value.padStart(2, '0');
            const month = document.getElementById('month').value.padStart(2, '0');
            const year = document.getElementById('year').value.padStart(4, '0');
            
            const fullDate = day + month + year;
            const sum = fullDate.split('').reduce((acc, num) => acc + +num, 0);
            
            return sum % 9 || 9; // Корректное вычисление однозначного числа
        }

        // Открытие модального окна расширенного прогноза
        function openExtendedModal() {
            document.getElementById('extendedModal').style.display = 'block';
        }

        // Генерация большого предсказания
        function generateBigPrediction() {
            const country = document.getElementById('country').value || "Неизвестная страна";
            const place = document.getElementById('place').value || "Неизвестное место";
            const firstName = document.getElementById('firstName').value || "Имя";
            const lastName = document.getElementById('lastName').value || "Фамилия";
            const middleName = document.getElementById('middleName').value || "Отчество";
            const day = document.getElementById('extDay').value.padStart(2, '0');
            const month = document.getElementById('extMonth').value.padStart(2, '0');
            const year = document.getElementById('extYear').value.padStart(4, '0');

            if (!validateExtendedDate()) {
                alert('Введите полную дату рождения!');
                return;
            }

            const destinyNumber = calculateDestinyNumberFromExtended();
            const description = destinyDescriptions[destinyNumber];

            const bigPrediction = `
                <strong>Полное имя:</strong> ${firstName} ${middleName} ${lastName}<br>
                <strong>Место рождения:</strong> ${place}, ${country}<br>
                <strong>Число судьбы:</strong> ${destinyNumber}<br><br>
                <strong>Общий прогноз:</strong><br>${description.text}<br><br>
                <strong>Психологический портрет:</strong><br>
                Вы родились в ${place}, что говорит о вашей связи с природой этого места.
                Ваше имя (${firstName}) несет в себе энергию творчества и лидерства.
                Ваша фамилия (${lastName}) указывает на семейные ценности и традиции.
                Отчество (${middleName}) подчеркивает влияние вашего рода на вашу личность.<br><br>
                <strong>Советы:</strong><br>
                1. Развивайте свои сильные стороны, связанные с числом судьбы (${destinyNumber}).<br>
                2. Учитывайте влияние места рождения (${place}) на ваш характер.<br>
                3. Поддерживайте связь с семейными традициями.<br>
                4. Используйте свое имя как источник силы.<br>
            `;

            document.getElementById('bigPredictionText').innerHTML = bigPrediction;
            document.getElementById('extendedModal').style.display = 'none';
            document.getElementById('bigModal').style.display = 'block';
        }

        // Закрытие большого модального окна
        function closeBigModal() {
            document.getElementById('bigModal').style.display = 'none';
        }

        // Валидация даты для расширенного прогноза
        function validateExtendedDate() {
            const day = document.getElementById('extDay').value;
            const month = document.getElementById('extMonth').value;
            const year = document.getElementById('extYear').value;

            return day.length === 2 && month.length === 2 && year.length === 4;
        }

        // Расчет числа судьбы для расширенного прогноза
        function calculateDestinyNumberFromExtended() {
            const day = document.getElementById('extDay').value.padStart(2, '0');
            const month = document.getElementById('extMonth').value.padStart(2, '0');
            const year = document.getElementById('extYear').value.padStart(4, '0');
            
            const fullDate = day + month + year;
            const sum = fullDate.split('').reduce((acc, num) => acc + +num, 0);
            
            return sum % 9 || 9; // Корректное вычисление однозначного числа
        }

        // Создаем числа при загрузке
        createBackgroundNumbers();
    </script>
</body>
</html>
