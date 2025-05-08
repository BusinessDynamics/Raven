<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Raven Intelligence Map</title>
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
  <script src="https://api.mapbox.com/mapbox-gl-js/v2.15.0/mapbox-gl.js"></script>
  <link href="https://api.mapbox.com/mapbox-gl-js/v2.15.0/mapbox-gl.css" rel="stylesheet" />
  <style>
    body { margin: 0; padding: 0; background: #111; }
    #map { position: absolute; top: 0; bottom: 0; width: 100%; }
    #filters {
      position: absolute;
      top: 20px;
      right: 20px;
      background: rgba(0, 0, 0, 0.8);
      color: #fff;
      padding: 10px;
      font-family: sans-serif;
      border-radius: 4px;
      z-index: 1;
    }
    #filters label { display: block; margin-bottom: 5px; }
    #logo {
      position: absolute;
      top: 20px;
      left: 20px;
      z-index: 2;
      width: 160px;
    }
  </style>
</head>
<body>
  <img id="logo" src="https://raw.githubusercontent.com/YOUR_USERNAME/REPO_NAME/main/maps/sprites/raven_logo.svg" alt="Raven Logo" />

  <div id="filters">
    <label><input type="checkbox" id="battlezones" checked> Battlezones</label>
    <label><input type="checkbox" id="incoming" checked> Incoming Supply</label>
    <label><input type="checkbox" id="outgoing" checked> Outgoing Supply</label>
    <label><input type="checkbox" id="internal" checked> Internal Movements</label>
    <label><input type="checkbox" id="chokepoints" checked> Chokepoints</label>
  </div>

  <div id="map"></div>
  <script>
    mapboxgl.accessToken = 'pk.eyJ1IjoiYXNwaWNpdW0iLCJhIjoiY2xvdmFvMHZ5MDlobTJpcGIwamdpcng5bCJ9.EtCrnMH5hV-atD_bhgzy3w';
    const map = new mapboxgl.Map({
      container: 'map',
      style: 'https://raw.githubusercontent.com/YOUR_USERNAME/REPO_NAME/main/maps/styles/raven_dark_red.json',
      center: [36.5, 48.5],
      zoom: 4.5
    });

    const layers = {
      battlezones: 'battlezones.geojson',
      incoming: 'supply_lines_incoming.geojson',
      outgoing: 'supply_lines_outgoing.geojson',
      internal: 'internal_movements.geojson',
      chokepoints: 'chokepoints.geojson'
    };

    for (const [key, file] of Object.entries(layers)) {
      map.on('load', () => {
        map.addSource(key, {
          type: 'geojson',
          data: `https://raw.githubusercontent.com/YOUR_USERNAME/REPO_NAME/main/maps/layers/russia_ukraine/${file}`
        });

        map.addLayer({
          id: key,
          type: key === 'chokepoints' ? 'symbol' : 'line',
          source: key,
          layout: key === 'chokepoints' ? { 'icon-image': ['get', 'icon'] } : {},
          paint: key === 'chokepoints' ? {} : {
            'line-color': '#BD0104',
            'line-width': 2
          }
        });
      });

      document.getElementById(key).addEventListener('change', function(e) {
        const visibility = e.target.checked ? 'visible' : 'none';
        map.setLayoutProperty(key, 'visibility', visibility);
      });
    }
  </script>
</body>
</html>

Style to load: style: 'https://raw.githubusercontent.com/YOUR_USERNAME/REPO_NAME/main/maps/styles/raven_dark_red.json',