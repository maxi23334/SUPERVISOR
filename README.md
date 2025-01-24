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

            document.getElementById("output").innerHTML = `
                <p><strong>Latitud:</strong> ${lat}</p>
                <p><strong>Longitud:</strong> ${lng}</p>
            `;

            const location = { lat, lng };
            map = new google.maps.Map(document.getElementById("map"), {
                center: location,
                zoom: 15,
            });

            // Personalizar el marcador con un pin azul
            const pinIcon = {
                url: "https://maps.google.com/mapfiles/ms/icons/blue-dot.png", // URL del pin azul
                scaledSize: new google.maps.Size(40, 40), // Tamaño del ícono
            };

            new google.maps.Marker({
                position: location,
                map: map,
                icon: pinIcon, // Asigna el ícono personalizado
                title: "Tu ubicación",
            });
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
