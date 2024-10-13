<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Pou Mejorado</title>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
            text-align: center;
        }

        header {
            background-color: #ffcc99;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        h1 {
            margin: 0;
            color: #444;
        }

        #monedas-container, #nivel-container {
            font-size: 1em; /* Cambiado para pantallas pequeñas */
            color: #333;
        }

        main {
            margin-top: 20px;
            padding: 0 10px; /* Añadido padding para espacios */
        }

        #pou-container {
            margin: 0 auto;
            display: flex;
            justify-content: center;
        }

        #pou-img {
            width: 150px; /* Ajustado para pantallas pequeñas */
            height: 150px; /* Ajustado para pantallas pequeñas */
            border-radius: 50%;
            animation: bounce 2s infinite;
        }

        @keyframes bounce {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-20px);
            }
        }

        .barra {
            width: 80%;
            background-color: #ddd;
            border-radius: 10px;
            margin: 10px auto;
            padding: 5px;
        }

        .nivel {
            width: 100%;
            height: 20px;
            background-color: green;
            border-radius: 10px;
        }

        button {
            padding: 10px 15px; /* Ajustado para pantallas pequeñas */
            background-color: #ff6600;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
        }

        button:hover {
            background-color: #ff4500;
        }

        /* Tienda */
        #tienda {
            margin-top: 30px;
        }

        #articulos-tienda {
            display: flex;
            flex-wrap: wrap; /* Permitir que los artículos se ajusten */
            justify-content: center; /* Centramos los artículos */
            margin-top: 10px;
        }

        .articulo {
            width: 150px;
            padding: 10px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            text-align: center;
            margin: 10px; /* Añadido margen para separación */
        }

        .articulo img {
            width: 100px;
            height: 100px;
            margin-bottom: 10px;
        }

        /* Media queries para pantallas pequeñas */
        @media (max-width: 600px) {
            header {
                padding: 15px;
            }

            #monedas-container, #nivel-container {
                font-size: 0.9em; /* Ajustar tamaño de fuente para pantallas pequeñas */
            }

            #pou-img {
                width: 120px; /* Ajuste adicional para pantallas pequeñas */
                height: 120px; /* Ajuste adicional para pantallas pequeñas */
            }

            button {
                padding: 8px 12px; /* Reducción en el padding de botones */
                font-size: 0.9em; /* Ajuste de tamaño de fuente en botones */
            }

            .articulo {
                width: 80%; /* Hacer los artículos más anchos en pantallas pequeñas */
                margin: 5px; /* Reducir el margen */
            }
        }

    </style>
</head>
<body>
    <header>
        <h1>Mi Pou</h1>
        <div id="monedas-container">
            💰 Monedas: <span id="monedas">100</span>
        </div>
        <div id="nivel-container">
            🏆 Nivel: <span id="nivel">1</span>
        </div>
    </header>

    <main>
        <!-- Contenedor de Pou -->
        <div id="pou-container">
            <img id="pou-img" src="fotos/godo-removebg-preview.png" alt="Pou">
        </div>

        <!-- Estado de Pou -->
        <div id="estado">
            <div class="barra">🍕 Comida: <div class="nivel" id="comidaNivel"></div></div>
            <div class="barra">🎮 Diversión: <div class="nivel" id="diversionNivel"></div></div>
            <div class="barra">🛌 Energía: <div class="nivel" id="energiaNivel"></div></div>
            <div class="barra">🚿 Higiene: <div class="nivel" id="higieneNivel"></div></div>
            <div class="barra">⚕️ Salud: <div class="nivel" id="saludNivel"></div></div>
        </div>

        <!-- Botones de acciones -->
        <div id="acciones">
            <button onclick="alimentar()">🍕 Alimentar</button>
            <button onclick="jugar()">🎮 Jugar</button>
            <button onclick="limpiar()">🚿 Limpiar</button>
            <button onclick="dormir()">🛌 Dormir</button>
            <button onclick="curar()">⚕️ Curar</button>
        </div>

        <!-- Sección de tienda -->
        <section id="tienda">
            <h2>Tienda</h2>
            <div id="articulos-tienda">
                <!-- Artículos de la tienda -->
                <div class="articulo">
                    <p>Gorra</p>
                    <img src="fotos/gorra.png" alt="Gorra">
                    <button onclick="comprarArticulo('gorra')">Comprar - 50 monedas</button>
                </div>
                <div class="articulo">
                    <p>Comida Especial</p>
                    <img src="fotos/comida-removebg-preview.png" alt="Comida">
                    <button onclick="comprarArticulo('comida')">Comprar - 30 monedas</button>
                </div>
                <div class="articulo">
                    <p>Decoración Casa</p>
                    <img src="decoracion.png" alt="Decoración">
                    <button onclick="comprarArticulo('decoracion')">Comprar - 100 monedas</button>
                </div>
            </div>
        </section>

        <!-- Minijuegos -->
        <section id="minijuegos">
            <h2>Minijuegos</h2>
            <button onclick="minijuegoSalto()">🏃‍♂️ Juego de Salto</button>
            <button onclick="minijuegoMemoria()">🧠 Juego de Memoria</button>
        </section>
    </main>

    <script>
        // Variables iniciales del estado de Pou
        let comida = 100;
        let diversion = 100;
        let energia = 100;
        let higiene = 100;
        let salud = 100;
        let monedas = 100;
        let nivel = 1;

        // Función para actualizar las barras de estado
        function actualizarEstado() {
            document.getElementById('comidaNivel').style.width = comida + '%';
            document.getElementById('diversionNivel').style.width = diversion + '%';
            document.getElementById('energiaNivel').style.width = energia + '%';
            document.getElementById('higieneNivel').style.width = higiene + '%';
            document.getElementById('saludNivel').style.width = salud + '%';
            document.getElementById('monedas').textContent = monedas;
            document.getElementById('nivel').textContent = nivel;

            if (comida <= 0 || diversion <= 0 || energia <= 0 || higiene <= 0 || salud <= 0) {
                alert("¡Pou está triste! Debes cuidarlo.");
            }
        }

        // Funciones para las acciones
        function alimentar() {
            if (comida < 100 && monedas >= 10) {
                comida += 10;
                monedas -= 10;
            }
            actualizarEstado();
        }

        function jugar() {
            if (diversion < 100 && energia > 10) {
                diversion += 20;
                energia -= 10;
            }
            actualizarEstado();
        }

        function limpiar() {
            if (higiene < 100) {
                higiene += 20;
            }
            actualizarEstado();
        }

        function dormir() {
            if (energia < 100) {
                energia += 30;
            }
            actualizarEstado();
        }

        function curar() {
            if (salud < 100 && monedas >= 15) {
                salud += 15;
                monedas -= 15;
            }
            actualizarEstado();
        }

        // Funciones de la tienda
        function comprarArticulo(articulo) {
            switch (articulo) {
                case 'gorra':
                    if (monedas >= 50) {
                        monedas -= 50;
                        alert("¡Has comprado una gorra!");
                    } else {
                        alert("¡No tienes suficientes monedas!");
                    }
                    break;
                case 'comida':
                    if (monedas >= 30) {
                        monedas -= 30;
                        alert("¡Has comprado comida especial!");
                    } else {
                        alert("¡No tienes suficientes monedas!");
                    }
                    break;
                case 'decoracion':
                    if (monedas >= 100) {
                        monedas -= 100;
                        alert("¡Has comprado decoración!");
                    } else {
                        alert("¡No tienes suficientes monedas!");
                    }
                    break;
            }
            actualizarEstado();
        }

        // Minijuegos (placeholder)
        function minijuegoSalto() {
            alert("¡Minijuego de salto aún no implementado!");
        }

        function minijuegoMemoria() {
            alert("¡Minijuego de memoria aún no implementado!");
        }

        // Actualiza el estado al cargar la página
        actualizarEstado();
    </script>
</body>
</html>
