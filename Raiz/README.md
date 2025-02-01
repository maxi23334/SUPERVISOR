<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EL VIGILANTE - Inicio</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        body {
            margin: 0;
            font-family: 'Poppins', sans-serif;
            background-color: #111;
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            text-align: center;
        }

        .logo {
            width: 120px;
            margin-bottom: 20px;
        }

        h1 {
            font-size: 2rem;
            margin-bottom: 20px;
        }

        .menu {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        button {
            background-color: #333;
            color: #fff;
            border: none;
            border-radius: 5px;
            padding: 15px 30px;
            font-size: 1.2rem;
            cursor: pointer;
            transition: background-color 0.3s ease;
            width: 250px;
        }

        button:hover {
            background-color: #555;
        }
    </style>
</head>
<body>

    <img src="img/escudo.png" alt="Escudo" class="logo">
    <h1>EL VIGILANTE</h1>

    <div class="menu">
        <button onclick="location.href='mapa.html'">🚪 Ingreso y Salida</button>
        <button onclick="location.href='rondas.html'">📝 Rondas</button>
    </div>

</body>
</html>
