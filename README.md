# index.html.
Plataforma de apoyo al combate de incendios forestales
// Plataforma SIG online para monitoreo de incendios y vegetación en Chiloé
// Usando Leaflet.js para mapas interactivos

// Crear el mapa centrado en Chiloé
var map = L.map('map').setView([-42.628, -73.689], 10);

// Agregar capa base (OpenStreetMap)
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
}).addTo(map);

// Capa Sentinel-2 procesada (ejemplo con URL de geoservidor o GEE TileLayer)
var sentinelLayer = L.tileLayer('URL_DE_IMAGEN_SATELITAL', {
    attribution: 'Sentinel-2 Imagery'
}).addTo(map);

// Capa de incendios activos (puede usar datos FIRMS de NASA)
var fireLayer = L.geoJSON(null, {
    style: { color: 'red', weight: 2 }
}).addTo(map);

// Capa de NDVI (vegetación)
var ndviLayer = L.tileLayer('URL_DE_NDVI', {
    attribution: 'NDVI Sentinel-2'
});

// Botón para actualizar datos
L.control({ position: 'topright' }).onAdd = function () {
    var btn = L.DomUtil.create('button', 'update-btn');
    btn.innerHTML = 'Actualizar';
    btn.onclick = function () {
        cargarDatos();
    };
    return btn;
}.addTo(map);

// Función para cargar datos de incendios (ejemplo con API de FIRMS)
function cargarDatos() {
    fetch('https://firms.modaps.eosdis.nasa.gov/api/area_fire_data.geojson')
        .then(response => response.json())
        .then(data => {
            fireLayer.clearLayers();
            fireLayer.addData(data);
        });
}

// Agregar opción de capas
L.control.layers({
    'Sentinel-2': sentinelLayer,
    'NDVI Vegetación': ndviLayer
}, {
    'Incendios Activos': fireLayer
}).addTo(map);

// Llamar a la función de carga al inicio
cargarDatos();

console.log('Plataforma SIG de monitoreo de incendios y vegetación lista.');
