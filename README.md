<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EL VIGILANTE MAPS</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        /* Estilo general */
        body {
            margin: 0;
            font-family: 'Poppins', sans-serif;
            background-color: #111; /* Fondo negro */
            color: #fff; /* Letras blancas */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
        }

        /* Estilo para el logo */
        .logo {
            width: 120px; /* Tamaño del logo */
            height: auto;
            display: block;
            margin: 0 auto 10px;
        }

        h1 {
            font-size: 2rem;
            font-weight: 600;
            margin-bottom: 20px;
            color: #fff;
            text-align: center;
        }

        /* Contenedor principal en dos columnas */
        .container {
            display: flex;
            flex-direction: row;
            width: 100%;
            max-width: 1200px;
            height: 80vh;
        }

        .box {
            flex: 1;
            padding: 20px;
            text-align: center;
        }

        /* Primera columna (Mapa) */
        .left {
            background-color: #222; /* Fondo oscuro */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        /* Segunda columna (Imagen Similar) */
        .right {
            background-color: #333; /* Fondo oscuro */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        button {
            background-color: #333;
            color: #fff;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #555;
        }

        img {
            max-width: 100%;
            border-radius: 10px;
            margin-top: 15px;
        }

        canvas {
            display: none;
        }

        /* Responsive: Poner en columna en pantallas pequeñas */
        @media (max-width: 768px) {
            .container {
                flex-direction: column;
                height: auto;
            }
        }
    </style>
</head>
<body>

    <!-- Logo arriba -->
    <img src="img/escudo.png" alt="Escudo de El Vigilante" class="logo">
    
    <h1>EL VIGILANTE MAPS</h1>

    <!-- Contenedor dividido en dos columnas -->
    <div class="container">

        <!-- Sección Izquierda (Mapa) -->
        <div class="box left">
            <h2>Mapa de Ubicación</h2>
            <button onclick="getLocation()">Generar Mapa Satelital</button>
            <div id="output"></div>
            <canvas id="mapCanvas"></canvas>
        </div>

        <!-- Sección Derecha (Imagen Similar o Información Extra) -->
        <div class="box right">
            <h2>Zona Similar</h2>
            <img src="img/escudo.png" alt="Imagen de referencia">
        </div>

    </div>

    <script>
        function getLocation() {
            document.getElementById("output").innerHTML = "<p>Obteniendo ubicación precisa...</p>";
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition, showError, {
                    enableHighAccuracy: true,
                    timeout: 10000,
                    maximumAge: 0,
                });
            } else {
                alert("La geolocalización no es compatible con este navegador.");
            }
        }

        function showPosition(position) {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;
            const date = new Date();
            const formattedDate = date.toLocaleString();
            const zoomLevel = 18;

            const apiKey = "AIzaSyCgBKlv8-PhVtIt-QcZLwR9ZHpSTnugb8M"; // Reemplaza con tu clave API de Google Maps
            const imageUrl = `https://maps.googleapis.com/maps/api/staticmap?center=${lat},${lng}&zoom=${zoomLevel}&size=600x400&maptype=satellite&markers=color:red|${lat},${lng}&key=${apiKey}`;

            // Crear un objeto de imagen
            const img = new Image();
            img.crossOrigin = "anonymous";
            img.src = imageUrl;

            img.onload = function () {
                // Dibujar la imagen en el canvas
                const canvas = document.getElementById("mapCanvas");
                const ctx = canvas.getContext("2d");
                canvas.width = img.width;
                canvas.height = img.height;
                ctx.drawImage(img, 0, 0);

                // Superponer la fecha y hora
                ctx.font = "20px Arial";
                ctx.fillStyle = "black";
                ctx.fillRect(10, img.height - 35, 580, 30);
                ctx.fillStyle = "white";
                ctx.fillText(`Fecha y Hora: ${formattedDate}`, 20, img.height - 15);

                // Mostrar la imagen generada
                const outputDiv = document.getElementById("output");
                const finalImg = new Image();
                finalImg.src = canvas.toDataURL("image/png");
                outputDiv.innerHTML = `
                    <p><strong>Latitud:</strong> ${lat}</p>
                    <p><strong>Longitud:</strong> ${lng}</p>
                    <p><strong>Fecha y Hora:</strong> ${formattedDate}</p>
                    <img src="${finalImg.src}" alt="Mapa con Fecha y Hora">
                `;

                // Descargar la imagen automáticamente
                const a = document.createElement("a");
                a.href = canvas.toDataURL("image/png");
                a.download = `mapa_satelital_${date.toISOString().replace(/[:.-]/g, "_")}.png`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
            };

            img.onerror = function () {
                alert("Error al cargar la imagen del mapa. Verifica tu clave API.");
            };
        }

        function showError(error) {
            switch (error.code) {
                case error.PERMISSION_DENIED:
                    alert("El usuario denegó la solicitud de geolocalización.");
                    break;
                case error.POSITION_UNAVAILABLE:
                    alert("La información de ubicación no está disponible.");
                    break;
                case error.TIMEOUT:
                    alert("La solicitud para obtener la ubicación ha expirado.");
                    break;
                default:
                    alert("Se produjo un error desconocido.");
            }
        }
    </script>

</body>
</html>
