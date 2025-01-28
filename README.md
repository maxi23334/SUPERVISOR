<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mapa con Ubicación Precisa</title>
</head>
<body>
    <h1>Mapa con Ubicación Precisa</h1>
    <button onclick="getLocation()">Generar Mapa</button>
    <div id="output"></div>
    <canvas id="mapCanvas" style="display: none;"></canvas>
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
            const zoomLevel = 18; // Zoom para mayor detalle

            const apiKey = "AIzaSyCgBKlv8-PhVtIt-QcZLwR9ZHpSTnugb8M"; // Reemplaza con tu clave API de Google Maps
            const imageUrl = `https://maps.googleapis.com/maps/api/staticmap?center=${lat},${lng}&zoom=${zoomLevel}&size=600x400&markers=color:blue|${lat},${lng}&key=${apiKey}`;

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
                    <p><strong>Precisión:</strong> ${position.coords.accuracy} metros</p>
                    <p><strong>Fecha y Hora:</strong> ${formattedDate}</p>
                    <img src="${finalImg.src}" alt="Mapa con Fecha y Hora">
                `;

                // Descargar la imagen automáticamente
                const a = document.createElement("a");
                a.href = canvas.toDataURL("image/png");
                a.download = `mapa_${date.toISOString().replace(/[:.-]/g, "_")}.png`;
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
