<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Оракул судьбы</title>
    <style>
        /* Базовые стили */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: #0a0a0a;
            color: #fff;
            overflow-x: hidden;
            position: relative;
            min-height: 100vh;
        }

        /* Анимации фона */
        .background-numbers {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .number-particle {
            position: absolute;
            font-size: 24px;
            color: rgba(255, 255, 255, 0.2);
            animation: float 10s linear infinite;
        }

        @keyframes float {
            0% { transform: translateY(0) translateX(0); }
            50% { transform: translateY(-50px) translateX(100px); }
            100% { transform: translateY(0) translateX(-100px); }
        }

        /* Основной контент */
        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            text-align: center;
        }

        h1 {
            font-size: 2rem;
            color: #4CAF50;
            text-shadow: 0 0 30px rgba(76, 175, 80, 0.7);
            margin-bottom: 20px;
        }

        /* Поля ввода */
        .date-inputs {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 20px 0;
        }

        .date-inputs input {
            width: 80px;
            padding: 15px;
            font-size: 18px;
            text-align: center;
            border: none;
            border-radius: 5px;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            transition: 0.3s;
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

        /* Кнопки */
        button {
            padding: 15px 30px;
            font-size: 16px;
            color: #fff;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
            margin: 10px 0;
        }

        button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.5);
        }

        /* Модальные окна */
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
            padding: 30px;
            border-radius: 15px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 0 40px rgba(255, 255, 255, 0.3);
            border: 2px solid transparent;
            animation: floatModal 3s infinite;
        }

        .modal-content h2 {
            color: #ffeb3b;
            text-shadow: 0 0 20px rgba(255, 235, 59, 0.7);
            margin-bottom: 20px;
            font-size: 2rem;
        }

        .modal-content p {
            color: #e0e0e0;
            line-height: 1.6;
            font-size: 16px;
            text-shadow: 0 0 5px rgba(255, 255, 255, 0.2);
        }

        /* Кнопки управления модальным окном */
        .modal-controls {
            position: absolute;
            top: 10px;
            width: 100%;
            display: flex;
            justify-content: space-between;
            pointer-events: none;
        }

        .return-button, .close-modal {
            font-size: 24px;
            color: #fff;
            cursor: pointer;
            transition: 0.3s;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
            pointer-events: auto;
        }

        .return-button:hover, .close-modal:hover {
            transform: translateX(-5px);
            text-shadow: -5px 0 10px rgba(255, 255, 255, 0.5);
        }

        /* Расширенное модальное окно */
        .extended-modal-content {
            max-width: 450px;
        }

        .extended-modal-content input {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            font-size: 14px;
            text-align: center;
            border: none;
            border-radius: 5px;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            transition: 0.3s;
        }

        /* Адаптивность */
        @media (max-width: 600px) {
            h1 {
                font-size: 1.5rem;
            }

            .date-inputs input {
                width: 60px;
                font-size: 16px;
            }

            button {
                font-size: 14px;
                padding: 12px 20px;
            }

            .modal-content {
                padding: 20px;
            }

            .modal-content h2 {
                font-size: 1.5rem;
            }

            .modal-content p {
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <!-- Летающие числа -->
    <div class="background-numbers" id="backgroundNumbers"></div>

    <!-- Основной контент -->
    <div class="container">
        <h1>Оракул судьбы</h1>

        <!-- Поля ввода -->
        <div class="date-inputs">
            <input type="text" id="day" placeholder="ДД" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <span class="date-separator">.</span>
            <input type="text" id="month" placeholder="ММ" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            <span class="date-separator">.</span>
            <input type="text" id="year" placeholder="ГГГГ" maxlength="4" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
        </div>

        <button onclick="showPrediction()">Прочитать судьбу</button>
        <button class="extended-button" onclick="openExtendedModal()">Расширенный прогноз</button>
    </div>

    <!-- Модальное окно -->
    <div class="modal-overlay" id="modal">
        <div class="modal-content">
            <div class="modal-controls">
                <span class="return-button" onclick="closeModal()">&#8592;</span>
                <span class="close-modal" onclick="closeModal()">&times;</span>
            </div>
            <h2 id="modalTitle"></h2>
            <p id="modalText"></p>
        </div>
    </div>

    <!-- Расширенное модальное окно -->
    <div class="modal-overlay" id="extendedModal">
        <div class="modal-content extended-modal-content">
            <div class="modal-controls">
                <span class="return-button" onclick="closeExtendedModal()">&#8592;</span>
                <span class="close-modal" onclick="closeExtendedModal()">&times;</span>
            </div>
            <h2>Расширенный прогноз</h2>
            <input type="text" id="country" placeholder="Страна рождения">
            <input type="text" id="place" placeholder="Место рождения">
            <input type="text" id="firstName" placeholder="Имя">
            <input type="text" id="lastName" placeholder="Фамилия">
            <input type="text" id="middleName" placeholder="Отчество">
            <div class="date-row">
                <input type="text" id="extDay" placeholder="День" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
                <input type="text" id="extMonth" placeholder="Месяц" maxlength="2" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
                <input type="text" id="extYear" placeholder="Год" maxlength="4" oninput="this.value = this.value.replace(/[^0-9]/g, '')">
            </div>
            <button onclick="generateBigPrediction()">Начать предсказание</button>
        </div>
    </div>

    <!-- Большое модальное окно -->
    <div class="modal-overlay" id="bigModal">
        <div class="modal-content">
            <div class="modal-controls">
                <span class="close-modal" onclick="closeBigModal()">&times;</span>
            </div>
            <h2>Ваш психологический портрет</h2>
            <p id="bigPredictionText"></p>
        </div>
    </div>

    <script>
        const destinyDescriptions = {
            1: {
                title: "Число Судьбы: 1 - Пионер",
                text: "Вы создатель новых путей и разрушитель старых границ."
            },
            2: {
                title: "Число Судьбы: 2 - Мост",
                text: "Вы гармонизируете противоречия и соединяете сердца."
            },
            3: {
                title: "Число Судьбы: 3 - Визионер",
                text: "Творчество течёт через вас рекой, но берегитесь рассеянности."
            },
            4: {
                title: "Число Судьбы: 4 - Страж",
                text: "Вы хранитель древних истин и строитель нерушимых основ."
            },
            5: {
                title: "Число Судьбы: 5 - Вихрь",
                text: "Свобода для вас - не выбор, а дыхание."
            },
            6: {
                title: "Число Судьбы: 6 - Садовник",
                text: "Вы родились, чтобы заботиться о других."
            },
            7: {
                title: "Число Судьбы: 7 - Мудрец",
                text: "Вы видите скрытые узоры мира."
            },
            8: {
                title: "Число Судьбы: 8 - Властелин",
                text: "Вы создаёте империи, но помните: настоящая власть в умении отпускать."
            },
            9: {
                title: "Число Судьбы: 9 - Пророк",
                text: "Вы храните память о будущем."
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
        }

        // Закрытие модального окна
        function closeModal() {
            document.getElementById('modal').style.display = 'none';
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
            return sum % 9 || 9;
        }

        // Открытие расширенного модального окна
        function openExtendedModal() {
            document.getElementById('extendedModal').style.display = 'block';
        }

        // Закрытие расширенного модального окна
        function closeExtendedModal() {
            document.getElementById('extendedModal').style.display = 'none';
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
            return sum % 9 || 9;
        }

        // Создаем числа при загрузке
        createBackgroundNumbers();
    </script>
</body>
</html>
