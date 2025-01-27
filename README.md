<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mapa con Ubicación</title>
</head>
<body>
    <h1>Mapa con Tu Ubicación</h1>
    <button onclick="getLocation()">Mostrar Mi Ubicación</button>
    <div id="output"></div>
    <script>
        function getLocation() {
            document.getElementById("output").innerHTML = "<p>Obteniendo ubicación...</p>";
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition, showError);
            } else {
                alert("La geolocalización no es compatible con este navegador.");
            }
        }

        function showPosition(position) {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;
            const date = new Date().toISOString();

            // Construir la URL de Static Maps API
            const apiKey = "AIzaSyCgBKlv8-PhVtIt-QcZLwR9ZHpSTnugb8M"; // Reemplaza con tu clave API
            const imageUrl = `https://maps.googleapis.com/maps/api/staticmap?center=${lat},${lng}&zoom=15&size=600x400&markers=color:blue|${lat},${lng}&key=${apiKey}`;

            // Mostrar la imagen en el navegador
            const outputDiv = document.getElementById("output");
            outputDiv.innerHTML = `
                <p><strong>Latitud:</strong> ${lat}</p>
                <p><strong>Longitud:</strong> ${lng}</p>
                <p><strong>Fecha y Hora:</strong> ${date}</p>
                <img src="${imageUrl}" alt="Mapa con tu ubicación">
            `;

            // Descargar automáticamente la imagen
            const a = document.createElement("a");
            a.href = imageUrl;
            a.download = `mapa_${date}.png`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
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


