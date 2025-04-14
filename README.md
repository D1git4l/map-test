<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Live FsHub Map</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
  <style>
    #map { height: 100vh; }
  </style>
</head>
<body>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    const map = L.map('map').setView([48.8566, 2.3522], 5); // Centré sur la France

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 18,
    }).addTo(map);

    // 👇 À PERSONNALISER
    const airlineCode = 'FFA'; // Remplace par le code de ta compagnie
    const apiKey = 'VOTRE_CLE_API_ICI';

    fetch(`https://api.fshub.io/api/v3/airlines/${airlineCode}/flights/live`, {
      headers: { 'x-api-key': apiKey }
    })
    .then(response => response.json())
    .then(data => {
      if (data.length === 0) {
        alert("Aucun vol en cours.");
      }
      data.forEach(flight => {
        const { latitude, longitude } = flight.location;
        const marker = L.marker([latitude, longitude]).addTo(map);
        marker.bindPopup(`<b>${flight.pilot_name}</b><br>${flight.aircraft_registration}`);
      });
    })
    .catch(err => {
      console.error(err);
      alert("Erreur lors de la récupération des données.");
    });
  </script>
</body>
</html>
