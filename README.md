# Guía de uso de servicios IDECOR

Este documento explica cómo acceder a los datos geográficos publicados por **IDECOR** a través de servicios de **Geoserver**, utilizando los estándares **WMS** (Web Map Service) y **WFS** (Web Feature Service).

---

## 📌 Diferencia entre WMS y WFS

| Servicio | Propósito                             | Resultado          |
| -------- | ------------------------------------- | ------------------ |
| **WMS**  | Visualizar mapas ya renderizados      | Imagen (PNG, JPEG) |
| **WFS**  | Obtener datos vectoriales y atributos | GeoJSON, GML, etc. |

- **WMS** se usa en librerías de mapas como Leaflet, OpenLayers, QGIS, etc.
- **WFS** permite consumir los datos como si fuera una API y procesarlos en tu código.

---

## 📍 Ejemplos de capas (WFS - GeoJSON)

| Capa                                  | URL WFS (GeoJSON)                                                                                                                                                           |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cuarteles de bomberos voluntarios** | [URL](https://idecor-ws.mapascordoba.gob.ar/geoserver/idecor/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=idecor:cuarteles_bbvv&outputFormat=application/json) |
| **Centros de salud**                  | [URL](https://idecor-ws.mapascordoba.gob.ar/geoserver/idecor/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=idecor:centros_salud&outputFormat=application/json)  |
| **Areas Quemadas 2025**               | [URL](https://idecor-ws.mapascordoba.gob.ar/geoserver/idecor/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=idecor:centros_salud&outputFormat=application/json)  |

---

## 📦 Ejemplo de consumo en JavaScript

```javascript
// URL WFS de Cuarteles de Bomberos Voluntarios
const url =
  "https://idecor-ws.mapascordoba.gob.ar/geoserver/idecor/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=idecor:cuarteles_bbvv&outputFormat=application/json";

fetch(url)
  .then((res) => res.json())
  .then((data) => {
    console.log("Datos recibidos:", data);
    // data.features es un array con las geometrías y atributos
  })
  .catch((err) => console.error("Error al consumir WFS:", err));
```

## 🗺️ Ejemplo de visualización en Leaflet (WMS)

```javascript
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Mapa WMS IDECOR</title>

  <!-- CSS de Leaflet -->
  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet/dist/leaflet.css"
  />
  <style>
    #map {
      height: 100vh;
      width: 100%;
    }
  </style>
</head>
<body>
  <div id="map"></div>

  <!-- JS de Leaflet -->
  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    // Crear el mapa y centrarlo en Córdoba
    var map = L.map('map').setView([-31.4, -64.2], 8);

    // Capa base de OpenStreetMap
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    // Capa WMS de IDECOR
    L.tileLayer.wms("https://idecor-ws.mapascordoba.gob.ar/geoserver/idecor/cuarteles_bbvv/wms", {
      layers: 'idecor:cuarteles_bbvv',
      format: 'image/png',
      transparent: true,
      attribution: 'IDEcor - Gobierno de Córdoba'
    }).addTo(map);
  </script>
</body>
</html>

```

otro ejemplo mas completo: [maps.html](https://github.com/nicouema/hackaton_idecor/blob/main/maps.html)
