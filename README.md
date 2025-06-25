# Kasino 
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Full Demo Casino</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: #0a1e3a;
            color: white;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        header {
            background: #0f2d5a;
            padding: 10px 0;
            text-align: center;
            border-bottom: 3px solid #ffcc00;
        }
        .auth-form {
            background: #0f2d5a;
            padding: 20px;
            border-radius: 10px;
            max-width: 400px;
            margin: 50px auto;
        }
        input {
            display: block;
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            border: none;
        }
        button {
            background: #ffcc00;
            color: #000;
            border: none;
            padding: 10px 15px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }
        button:hover {
            background: #e6b800;
        }
        .game-section {
            display: none;
            background: #0f2d5a;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
        }
        .roulette-wheel {
            width: 300px;
            height: 300px;
            margin: 20px auto;
            border-radius: 50%;
            background: #d22b2b;
            position: relative;
            overflow: hidden;
        }
        .ball {
            width: 20px;
            height: 20px;
            background: white;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .slot-machine {
            width: 300px;
            height: 200px;
            margin: 20px auto;
            background: #1a3d6d;
            border: 5px solid #ffcc00;
            display: flex;
            justify-content: space-around;
            align-items: center;
        }
        .slot-reel {
            width: 80px;
            height: 160px;
            background: white;
            border: 2px solid #000;
            overflow: hidden;
        }
        .slot-symbol {
            height: 80px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 30px;
        }
        .blackjack-table {
            width: 500px;
            height: 300px;
            margin: 20px auto;
            background: #0a5c36;
            border: 10px solid #8b4513;
            border-radius: 10px;
            position: relative;
        }
        .card {
            width: 60px;
            height: 90px;
            background: white;
            border-radius: 5px;
            margin: 5px;
            display: inline-flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
            color: black;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
        }
        .user-panel {
            position: fixed;
            top: 10px;
            right: 10px;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <header>
        <h1>🎰 Full Demo Casino</h1>
    </header>

    <div class="container">
        <!-- Форма авторизации -->
        <div id="authSection" class="auth-form">
            <h2>Вход / Регистрация</h2>
            <input type="text" id="username" placeholder="Логин">
            <input type="password" id="password" placeholder="Пароль">
            <button onclick="login()">Войти</button>
            <button onclick="register()">Зарегистрироваться</button>
        </div>

        <!-- Игровой раздел (скрыт до авторизации) -->
        <div id="gameSection" class="game-section" style="display: none;">
            <div class="user-panel">
                Игрок: <span id="currentUser"></span><br>
                Баланс: <span id="balance">1000</span> ₽
            </div>

            <h2>Выберите игру:</h2>
            <button onclick="showGame('roulette')">Рулетка</button>
            <button onclick="showGame('slots')">Слоты</button>
            <button onclick="showGame('blackjack')">Блэкджек</button>

            <!-- Рулетка -->
            <div id="rouletteGame" class="game" style="display: none;">
                <h2>Рулетка</h2>
                <div class="roulette-wheel" id="wheel">
                    <div class="ball"></div>
                </div>
                <div class="bet-controls">
                    <input type="number" id="rouletteBet" min="10" max="1000" value="50">
                    <button onclick="placeBet('red')">Красное (x2)</button>
                    <button onclick="placeBet('black')">Чёрное (x2)</button>
                    <button onclick="placeBet('green')">Зелёное (x14)</button>
                </div>
                <div id="rouletteResult"></div>
            </div>

            <!-- Слоты -->
            <div id="slotsGame" class="game" style="display: none;">
                <h2>Слот-машина</h2>
                <div class="slot-machine">
                    <div class="slot-reel" id="reel1"></div>
                    <div class="slot-reel" id="reel2"></div>
                    <div class="slot-reel" id="reel3"></div>
                </div>
                <div class="bet-controls">
                    <input type="number" id="slotsBet" min="10" max="1000" value="50">
                    <button onclick="spinSlots()">Крутить!</button>
                </div>
                <div id="slotsResult"></div>
            </div>

            <!-- Блэкджек -->
            <div id="blackjackGame" class="game" style="display: none;">
                <h2>Блэкджек</h2>
                <div class="blackjack-table">
                    <div id="dealerHand" style="margin-top: 20px;"></div>
                    <div id="playerHand" style="margin-top: 50px;"></div>
                </div>
                <div class="bet-controls">
                    <input type="number" id="blackjackBet" min="10" max="1000" value="50">
                    <button onclick="startBlackjack()">Начать игру</button>
                    <button id="hitBtn" style="display: none;" onclick="hit()">Ещё</button>
                    <button id="standBtn" style="display: none;" onclick="stand()">Хватит</button>
                </div>
                <div id="blackjackResult"></div>
            </div>
        </div>
    </div>

    <script>
        // База данных пользователей (в реальном проекте - серверная часть)
        let users = JSON.parse(localStorage.getItem('casinoUsers')) || [];
        let currentUser = null;
        let balance = 1000;

        // Символы для слотов
        const slotSymbols = ['🍒', '🍋', '🍊', '🍇', '🔔', '⭐', '7️⃣'];

        // Карты для блэкджека
        const suits = ['♥', '♦', '♣', '♠'];
        const ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
        let deck = [];
        let playerHand = [];
        let dealerHand = [];
        let blackjackGameActive = false;
        let currentBlackjackBet = 0;

        // Авторизация
        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            const user = users.find(u => u.username === username && u.password === password);
            if (user) {
                currentUser = user;
                balance = user.balance;
                document.getElementById('currentUser').textContent = user.username;
                document.getElementById('balance').textContent = balance;
                document.getElementById('authSection').style.display = 'none';
                document.getElementById('gameSection').style.display = 'block';
            } else {
                alert('Неверный логин или пароль!');
            }
        }

        // Регистрация
        function register() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            if (users.some(u => u.username === username)) {
                alert('Имя пользователя уже занято!');
                return;
            }
            
            const newUser = {
                username,
                password,
                balance: 1000
            };
            
            users.push(newUser);
            localStorage.setItem('casinoUsers', JSON.stringify(users));
            alert('Регистрация успешна! Теперь войдите.');
        }

        // Показать выбранную игру
        function showGame(game) {
            document.querySelectorAll('.game').forEach(g => g.style.display = 'none');
            document.getElementById(`${game}Game`).style.display = 'block';
        }

        // ======= РУЛЕТКА =======
        function placeBet(color) {
            const betAmount = parseInt(document.getElementById('rouletteBet').value);
            
            if (betAmount > balance) {
                alert("Недостаточно средств!");
                return;
            }

            // Спин рулетки (0-36)
            const result = Math.floor(Math.random() * 37);
            let winColor;

            if (result === 0) {
                winColor = 'green';
            } else if (result % 2 === 0) {
                winColor = 'black';
            } else {
                winColor = 'red';
            }

            // Анимация
            const wheel = document.getElementById('wheel');
            wheel.style.transform = `rotate(${Math.random() * 3600}deg)`;

            setTimeout(() => {
                if (color === winColor) {
                    let multiplier = (color === 'green') ? 14 : 2;
                    const winAmount = betAmount * multiplier;
                    balance += winAmount;
                    document.getElementById('rouletteResult').innerHTML = `Выигрыш: ${winAmount} ₽! Выпало: ${result} (${winColor})`;
                } else {
                    balance -= betAmount;
                    document.getElementById('rouletteResult').innerHTML = `Проигрыш: ${betAmount} ₽. Выпало: ${result} (${winColor})`;
                }

                updateBalance();
            }, 2000);
        }

        // ======= СЛОТЫ =======
        function spinSlots() {
            const betAmount = parseInt(document.getElementById('slotsBet').value);
            
            if (betAmount > balance) {
                alert("Недостаточно средств!");
                return;
            }

            balance -= betAmount;
            updateBalance();

            // Анимация вращения
            const reels = [document.getElementById('reel1'), document.getElementById('reel2'), document.getElementById('reel3')];
            const spins = [0, 0, 0];
            const spinInterval = setInterval(() => {
                for (let i = 0; i < 3; i++) {
                    spins[i]++;
                    reels[i].innerHTML = `<div class="slot-symbol">${slotSymbols[spins[i] % slotSymbols.length]}</div>`;
                }
            }, 100);

            // Остановка через 2 секунды
            setTimeout(() => {
                clearInterval(spinInterval);
                
                // Фиксируем результат
                const results = [
                    Math.floor(Math.random() * slotSymbols.length),
                    Math.floor(Math.random() * slotSymbols.length),
                    Math.floor(Math.random() * slotSymbols.length)
                ];
                
                for (let i = 0; i < 3; i++) {
                    reels[i].innerHTML = `<div class="slot-symbol">${slotSymbols[results[i]]}</div>`;
                }

                // Проверка выигрыша
                if (results[0] === results[1] && results[1] === results[2]) {
                    const winAmount = betAmount * 10;
                    balance += winAmount;
                    document.getElementById('slotsResult').innerHTML = `ДЖЕКПОТ! Выигрыш: ${winAmount} ₽!`;
                } else if (results[0] === results[1] || results[1] === results[2]) {
                    const winAmount = betAmount * 3;
                    balance += winAmount;
                    document.getElementById('slotsResult').innerHTML = `Выигрыш: ${winAmount} ₽!`;
                } else {
                    document.getElementById('slotsResult').innerHTML = `Повезёт в следующий раз!`;
                }

                updateBalance();
            }, 2000);
        }

        // ======= БЛЭКДЖЕК =======
        function startBlackjack() {
            const betAmount = parseInt(document.getElementById('blackjackBet').value);
            
            if (betAmount > balance) {
                alert("Недостаточно средств!");
                return;
            }

            currentBlackjackBet = betAmount;
            balance -= betAmount;
            updateBalance();

            // Создаем колоду и перемешиваем
            deck = [];
            for (let suit of suits) {
                for (let rank of ranks) {
                    deck.push({ suit, rank });
                }
            }
            deck = shuffleDeck(deck);

            // Раздаем карты
            playerHand = [deck.pop(), deck.pop()];
            dealerHand = [deck.pop(), deck.pop()];

            renderHands();
            document.getElementById('hitBtn').style.display = 'inline-block';
            document.getElementById('standBtn').style.display = 'inline-block';
            blackjackGameActive = true;
            document.getElementById('blackjackResult').innerHTML = '';
        }

        function hit() {
            if (!blackjackGameActive) return;
            
            playerHand.push(deck.pop());
            renderHands();
            
            if (calculateHandValue(playerHand) > 21) {
                document.getElementById('blackjackResult').innerHTML = 'Перебор! Вы проиграли.';
                endBlackjackGame(false);
            }
        }

        function stand() {
            if (!blackjackGameActive) return;
            
            // Дилер добирает карты до 17+
            while (calculateHandValue(dealerHand) < 17) {
                dealerHand.push(deck.pop());
            }
            
            renderHands(true);
            const playerValue = calculateHandValue(playerHand);
            const dealerValue = calculateHandValue(dealerHand);
            
            if (dealerValue > 21 || playerValue > dealerValue) {
                const winAmount = currentBlackjackBet * 2;
                balance += winAmount;
                document.getElementById('blackjackResult').innerHTML = `Вы выиграли ${winAmount} ₽!`;
            } else if (playerValue === dealerValue) {
                balance += currentBlackjackBet;
                document.getElementById('blackjackResult').innerHTML = 'Ничья! Ставка возвращена.';
            } else {
                document.getElementById('blackjackResult').innerHTML = 'Вы проиграли.';
            }
            
            endBlackjackGame();
        }

        function renderHands(showDealerCard = false) {
            const dealerHandDiv = document.getElementById('dealerHand');
            const playerHandDiv = document.getElementById('playerHand');
            
            dealerHandDiv.innerHTML = '<h3>Дилер:</h3>';
            playerHandDiv.innerHTML = '<h3>Игрок:</h3>';
            
            // Карты дилера
            dealerHand.forEach((card, index) => {
                if (index === 0 && !showDealerCard) {
                    dealerHandDiv.innerHTML += '<div class="card">?</div>';
                } else {
                    dealerHandDiv.innerHTML += `<div class="card">${card.rank}${card.suit}</div>`;
                }
            });
            
            // Карты игрока
            playerHand.forEach(card => {
                playerHandDiv.innerHTML += `<div class="card">${card.rank}${card.suit}</div>`;
            });
            
            if (showDealerCard) {
                dealerHandDiv.innerHTML += `<p>Очки: ${calculateHandValue(dealerHand)}</p>`;
            }
            playerHandDiv.innerHTML += `<p>Очки: ${calculateHandValue(playerHand)}</p>`;
        }

        function calculateHandValue(hand) {
            let value = 0;
            let aces = 0;
            
            for (let card of hand) {
                if (card.rank === 'A') {
                    value += 11;
                    aces++;
                } else if (['K', 'Q', 'J'].includes(card.rank)) {
                    value += 10;
                } else {
                    value += parseInt(card.rank);
                }
            }
            
            while (value > 21 && aces > 0) {
                value -= 10;
                aces--;
            }
            
            return value;
        }

        function shuffleDeck(deck) {
            for (let i = deck.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [deck[i], deck[j]] = [deck[j], deck[i]];
            }
            return deck;
        }

        function endBlackjackGame() {
            blackjackGameActive = false;
            document.getElementById('hitBtn').style.display = 'none';
            document.getElementById('standBtn').style.display = 'none';
            updateBalance();
        }

        // Обновление баланса
        function updateBalance() {
            document.getElementById('balance').textContent = balance;
            if (currentUser) {
                const userIndex = users.findIndex(u => u.username === currentUser.username);
                if (userIndex !== -1) {
                    users[userIndex].balance = balance;
                    localStorage.setItem('casinoUsers', JSON.stringify(users));
                }
            }
        }
    </script>
</body>
</html>
