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
            background-color: #f5f5f5;
            color: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
        }

        h1 {
            font-size: 2rem;
            font-weight: 600;
            margin-bottom: 20px;
            color: #111;
        }

        button {
            background-color: #111;
            color: #fff;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            font-size: 1rem;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #333;
        }

        #output {
            margin-top: 20px;
            width: 100%;
            max-width: 600px;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            opacity: 0;
            animation: fadeIn 0.5s forwards;
        }

        img {
            max-width: 100%;
            border-radius: 10px;
            margin-top: 15px;
        }

        canvas {
            display: none;
        }

        /* Animaciones */
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
</head>
<body>
    <h1>EL VIGILANTE MAPS</h1>
    <button onclick="getLocation()">Generar Mapa Satelital</button>
    <div id="output"></div>
    <canvas id="mapCanvas"></canvas>
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
                ctx.fillStyle = "white";
                ctx.fillRect(10, img.height - 35, 580, 30);
                ctx.fillStyle = "black";
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
