<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Pou Avanzado</title>
    <style>
        * {
            box-sizing: border-box;
        }
        body {
            font-family: 'Comic Sans MS', cursive;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background: linear-gradient(135deg, #fad0c4 0%, #ffd1ff 100%);
            padding: 20px;
        }
        #game-container {
            text-align: center;
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            max-width: 100%;
            width: 400px;
        }
        #pou {
            width: 150px;
            height: 150px;
            background-color: #a0a0a0;
            border-radius: 50%;
            margin: 20px auto;
            position: relative;
            cursor: pointer;
            transition: transform 0.3s ease;
        }
        #pou:hover {
            transform: scale(1.1);
        }
        #pou img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            object-fit: cover;
        }
        .stat {
            margin: 10px 0;
            font-size: 14px;
        }
        .stat-bar {
            width: 100%;
            height: 15px;
            background-color: #ddd;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .stat-fill {
            height: 100%;
            width: 100%;
            background-color: #4CAF50;
            transition: width 0.3s ease;
        }
        #actions {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: 20px;
        }
        button {
            margin: 5px;
            padding: 8px 12px;
            font-size: 14px;
            cursor: pointer;
            background-color: #ff9800;
            border: none;
            border-radius: 5px;
            color: white;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #f57c00;
        }
        #minigame {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 10px;
            background-color: white;
            font-size: 14px;
        }
        #inventory {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: 20px;
        }
        .item {
            width: 40px;
            height: 40px;
            margin: 5px;
            background-color: #f1f1f1;
            border-radius: 5px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
            cursor: pointer;
            transition: transform 0.2s ease;
        }
        .item:hover {
            transform: scale(1.1);
        }
        #level-info {
            font-size: 16px;
            font-weight: bold;
            margin-top: 10px;
        }
        #coins {
            font-size: 16px;
            color: #ffd700;
            margin-top: 5px;
        }
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }
        .bouncing {
            animation: bounce 0.5s;
        }
        #mute-button {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 24px;
            cursor: pointer;
            background: none;
            border: none;
            color: #333;
        }
        @media (max-width: 480px) {
            #game-container {
                padding: 10px;
            }
            #pou {
                width: 120px;
                height: 120px;
            }
            button {
                padding: 6px 10px;
                font-size: 12px;
            }
            .stat, #level-info, #coins {
                font-size: 12px;
            }
            .item {
                width: 30px;
                height: 30px;
                font-size: 16px;
            }
        }
    </style>
</head>
<body>
    <div id="game-container">
        <button id="mute-button">🔊</button>
        <div id="pou">
            <img src="fotos/godo-removebg-preview.png" alt="Pou">
        </div>
        <div id="level-info">Nivel: <span id="level">1</span> (XP: <span id="xp">0</span>/100)</div>
        <div id="coins">🪙 <span id="coins-amount">100</span></div>
        <div id="stats">
            <div class="stat">
                Hambre: <div class="stat-bar"><div id="hunger-bar" class="stat-fill"></div></div>
            </div>
            <div class="stat">
                Felicidad: <div class="stat-bar"><div id="happiness-bar" class="stat-fill"></div></div>
            </div>
            <div class="stat">
                Energía: <div class="stat-bar"><div id="energy-bar" class="stat-fill"></div></div>
            </div>
            <div class="stat">
                Higiene: <div class="stat-bar"><div id="hygiene-bar" class="stat-fill"></div></div>
            </div>
            <div class="stat">
                Salud: <div class="stat-bar"><div id="health-bar" class="stat-fill"></div></div>
            </div>
        </div>
        <div id="actions">
            <button onclick="feed()">Alimentar</button>
            <button onclick="play()">Jugar</button>
            <button onclick="sleep()">Dormir</button>
            <button onclick="clean()">Limpiar</button>
            <button onclick="exercise()">Ejercicio</button>
            <button onclick="shop()">Tienda</button>
        </div>
        <div id="inventory">
            <!-- Los items se añadirán dinámicamente -->
        </div>
        <div id="minigame">
            <h3>Mini-juego: Adivina el número</h3>
            <p>Adivina un número entre 1 y 10:</p>
            <input type="number" id="guess" min="1" max="10">
            <button onclick="guessNumber()">Adivinar</button>
            <p id="minigame-result"></p>
        </div>
    </div>

    <script>
        let stats = {
            hunger: 100,
            happiness: 100,
            energy: 100,
            hygiene: 100,
            health: 100
        };
        let level = 1;
        let xp = 0;
        let coins = 100;
        let inventory = [];
        let isMuted = false;

        const sounds = {
            feed: new Audio("musica/fondo.mpeg"), // Base64 encoded eating sound
            play: new Audio("musica/nom.mp3"), // Base64 encoded playing sound
            levelUp: new Audio("musica/nivel.mp3") // Base64 encoded level up sound
        };

        function playSound(soundName) {
            if (!isMuted) {
                sounds[soundName].play();
            }
        }

        function updateStats() {
            for (let stat in stats) {
                stats[stat] = Math.max(0, stats[stat] - 0.2);
                document.getElementById(`${stat}-bar`).style.width = `${stats[stat]}%`;
            }
            updatePouAppearance();
        }

        function updatePouAppearance() {
            const pou = document.getElementById('pou');
            if (Object.values(stats).some(value => value < 30)) {
                pou.style.filter = 'grayscale(100%) brightness(50%)';
            } else if (Object.values(stats).some(value => value < 50)) {
                pou.style.filter = 'sepia(100%) brightness(70%)';
            } else {
                pou.style.filter = 'brightness(100%)';
            }
        }

        function addXP(amount) {
            xp += amount;
            if (xp >= 100) {
                level++;
                xp -= 100;
                playSound('levelUp');
                alert(`¡Felicidades! Has subido al nivel ${level}`);
            }
            document.getElementById('level').textContent = level;
            document.getElementById('xp').textContent = xp;
        }

        function updateCoins(amount) {
            coins += amount;
            document.getElementById('coins-amount').textContent = coins;
        }

        function feed() {
            playSound('feed');
            stats.hunger = Math.min(100, stats.hunger + 20);
            stats.energy = Math.max(0, stats.energy - 5);
            addXP(5);
            animatePou();
        }

        function play() {
            playSound('play');
            stats.happiness = Math.min(100, stats.happiness + 20);
            stats.energy = Math.max(0, stats.energy - 10);
            stats.hygiene = Math.max(0, stats.hygiene - 5);
            addXP(10);
            animatePou();
        }

        function sleep() {
            stats.energy = Math.min(100, stats.energy + 30);
            stats.hunger = Math.max(0, stats.hunger - 10);
            addXP(5);
            animatePou();
        }

        function clean() {
            stats.hygiene = Math.min(100, stats.hygiene + 30);
            stats.energy = Math.max(0, stats.energy - 5);
            addXP(5);
            animatePou();
        }

        function exercise() {
            stats.health = Math.min(100, stats.health + 20);
            stats.energy = Math.max(0, stats.energy - 15);
            stats.hunger = Math.max(0, stats.hunger - 10);
            addXP(15);
            animatePou();
        }

        function shop() {
            alert(`Tienes ${coins} monedas. ¡Próximamente podrás comprar items!`);
        }

        function guessNumber() {
            const guess = document.getElementById('guess').value;
            const correct = Math.floor(Math.random() * 10) + 1;
            const result = document.getElementById('minigame-result');
            
            if (guess == correct) {
                result.textContent = '¡Correcto! Has ganado 10 monedas.';
                stats.happiness = Math.min(100, stats.happiness + 10);
                updateCoins(10);
                addXP(20);
                playSound('play');
            } else {
                result.textContent = `Incorrecto. El número era ${correct}.`;
            }
        }

        function animatePou() {
            const pou = document.getElementById('pou');
            pou.classList.add('bouncing');
            setTimeout(() => pou.classList.remove('bouncing'), 500);
        }

        function addToInventory(item) {
            inventory.push(item);
            updateInventoryDisplay();
        }

        function updateInventoryDisplay() {
            const inventoryElement = document.getElementById('inventory');
            inventoryElement.innerHTML = '';
            inventory.forEach(item => {
                const itemElement = document.createElement('div');
                itemElement.className = 'item';
                itemElement.textContent = item.emoji;
                itemElement.title = item.name;
                itemElement.onclick = () => useItem(item);
                inventoryElement.appendChild(itemElement);
            });
        }

        function useItem(item) {
            // Implementar efectos de los items
            alert(`Usaste ${item.name}`);
            const index = inventory.indexOf(item);
            if (index > -1) {
                inventory.splice(index, 1);
            }
            updateInventoryDisplay();
        }

        document.getElementById('mute-button').addEventListener('click', function() {
            isMuted = !isMuted;
            this.textContent = isMuted ? '🔇' : '🔊';
        });

        setInterval(updateStats, 1000);

        // Permitir que el usuario cambie la imagen del Pou
        document.getElementById('pou').addEventListener('click', function() {
            const imageUrl = prompt('Introduce la URL de la nueva imagen para tu Pou:');
            if (imageUrl) {
                document.querySelector('#pou img').src = imageUrl;
            }
        });

        // Añadir algunos items de ejemplo al inventario
        addToInventory({ name: "Manzana", emoji: "🍎" });
        addToInventory({ name: "Pelota", emoji: "⚽" });
        addToInventory({ name: "Libro", emoji: "📚" });

        // Inicializar la visualización de monedas
        updateCoins(0);
    </script>
</body>
</html>
