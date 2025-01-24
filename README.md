<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mapa con Ubicación</title>
    <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCgBKlv8-PhVtIt-QcZLwR9ZHpSTnugb8M"></script>
    <style>
        #map {
            height: 500px;
            width: 100%;
        }
    </style>
</head>
<body>
    <h1>Mapa con Tu Ubicación</h1>
    <button onclick="getLocation()">Mostrar Mi Ubicación</button>
    <div id="output"></div>
    <div id="map"></div>

    <script>
        let map;

        // Función para obtener la ubicación
        function getLocation() {
            document.getElementById("output").innerHTML = "<p>Obteniendo ubicación...</p>";
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition, showError);
            } else {
                alert("La geolocalización no es compatible con este navegador.");
            }
        }

        // Mostrar la ubicación y enviar datos a Google Sheets
        function showPosition(position) {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;
            const date = new Date();

            document.getElementById("output").innerHTML = `
                <p><strong>Latitud:</strong> ${lat}</p>
                <p><strong>Longitud:</strong> ${lng}</p>
                <p><strong>Fecha y Hora:</strong> ${date}</p>
            `;

            const location = { lat, lng };
            map = new google.maps.Map(document.getElementById("map"), {
                center: location,
                zoom: 15,
            });

            const pinIcon = {
                url: "https://maps.google.com/mapfiles/ms/icons/blue-dot.png",
                scaledSize: new google.maps.Size(40, 40),
            };

            new google.maps.Marker({
                position: location,
                map: map,
                icon: pinIcon,
                title: "Tu ubicación",
            });

            // Enviar datos a Google Sheets
            const apiUrl = "https://script.google.com/macros/s/AKfycbzrFi2t59v21nkTAlj30-PNxQLP0j0Z6z3rawiPFAd1vNEXbHWKFYv-33QCLuJaRduEwQ/exec"; // Reemplaza con tu enlace API
            fetch(apiUrl, {
                method: "POST",
                body: JSON.stringify({
                    lat: lat,
                    lng: lng,
                    date: date.toISOString(),
                }),
                headers: {
                    "Content-Type": "application/json",
                },
            })
                .then((response) => response.text())
                .then((data) => {
                    console.log("Datos enviados correctamente:", data);
                })
                .catch((error) => {
                    console.error("Error al enviar los datos:", error);
                });
        }

        // Manejo de errores de geolocalización
        function showError(error) {
            switch (error.code) {
                case error.PERMISSION_DENIED:
                    alert("El usuario denegó la solicitud de geolocalización.");
                    break;
                case error.POSITION_UNAVAILABLE:
                    alert("La información de ubicación no está disponible.");
                    break;
                case error.TIMEOUT:
                    alert("La solicitud de ubicación tardó demasiado.");
                    break;
                default:
                    alert("Se produjo un error desconocido.");
            }
        }
    </script>
</body>
</html>

