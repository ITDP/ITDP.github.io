<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>Display a map</title>
<meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
<script src="https://api.mapbox.com/mapbox-gl-js/v1.10.1/mapbox-gl.js"></script>
<link href="https://api.mapbox.com/mapbox-gl-js/v1.10.1/mapbox-gl.css" rel="stylesheet" />
<style>
	body { margin: 0; padding: 0; }
	#map { position: absolute; top: 0; bottom: 0; width: 100%; }
#menu {
background: #fff;
position: absolute;
z-index: 1;
top: 10px;
right: 10px;
border-radius: 3px;
width: 120px;
border: 1px solid rgba(0, 0, 0, 0.4);
font-family: 'Open Sans', sans-serif;
}

#menu a {
font-size: 13px;
color: #404040;
display: block;
margin: 0;
padding: 0;
padding: 10px;
text-decoration: none;
border-bottom: 1px solid rgba(0, 0, 0, 0.25);
text-align: center;
}

#menu a:last-child {
border: none;
}

#menu a:hover {
background-color: #f8f8f8;
color: #404040;
}

#menu a.active {
background-color: #3887be;
color: #ffffff;
}

#menu a.active:hover {
background: #3074a4;
}

.mapboxgl-popup {
max-width: 400px;
font: 12px/20px 'Helvetica Neue', Arial, Helvetica, sans-serif;
}

</style>
</head>
<body>
<nav id="menu"></nav>
<div id="map"></div>
<script>
	mapboxgl.accessToken = 'pk.eyJ1IjoidGF5bG9yLXJlaWNoIiwiYSI6ImNrN2RzNW5uMzAxNXcza28zZ2JybHlteGwifQ.MTEb0vrjtqfEnpxb3yj5TQ';
var map = new mapboxgl.Map({
  hash: true,
  container: 'map', // container id
  style: 'mapbox://styles/taylor-reich/ckc22ya6f39xo1iqqbxkgnc7v', // stylesheet location
  center: [18.501,-33.960], // starting position [lng, lat]
  zoom: 9.75 // starting zoom
});


map.on('load', function() {
// Create a popup, but don't add it to the map yet.
var popup = new mapboxgl.Popup({
closeButton: false,
closeOnClick: false
});
});

map.on('mouseenter', 'boundaries_invis', function(e) {
// Change the cursor style as a UI indicator.
map.getCanvas().style.cursor = 'pointer';

var coordinates = e.features[0].geometry.coordinates.slice();
var description = "test";

// Ensure that if the map is zoomed out such that multiple
// copies of the feature are visible, the popup appears
// over the copy being pointed to.
while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
}

// Populate the popup and set its coordinates
// based on the feature found.
popup
.setLngLat(coordinates)
.setHTML(description)
.addTo(map);
});

map.on('mouseleave', 'boundaries_invis', function() {
map.getCanvas().style.cursor = '';
popup.remove();
});

// enumerate ids of the layers
var toggleableLayerIds = ['Services','Car-free Places','Density'];

// set up the corresponding toggle button for each layer
for (var i = 0; i < toggleableLayerIds.length; i++) {
var id = toggleableLayerIds[i];

var link = document.createElement('a');
link.href = '#';
link.className = 'active';
link.textContent = id;

link.onclick = function(e) {
var clickedLayer = this.textContent;
e.preventDefault();
e.stopPropagation();

var visibility = map.getLayoutProperty(clickedLayer, 'visibility');

// toggle layer visibility by changing the layout object's visibility property
if (visibility === 'visible' || visibility == undefined) {
map.setLayoutProperty(clickedLayer, 'visibility', 'none');
this.className = '';
} else {
this.className = 'active';
map.setLayoutProperty(clickedLayer, 'visibility', 'visible');
}
};

var layers = document.getElementById('menu');
layers.appendChild(link);
};

var id = 'Blocks';

var link = document.createElement('a');
link.href = '#';
link.className = 'active';
link.textContent = id;

link.onclick = function(e) {
var clickedLayer = this.textContent;
e.preventDefault();
e.stopPropagation();

var visibility = map.getLayoutProperty(clickedLayer, 'visibility');

// toggle layer visibility by changing the layout object's visibility property
if (visibility === 'visible' || visibility == undefined ) {
map.setLayoutProperty(clickedLayer, 'visibility', 'none');
map.setLayoutProperty('Block Patches', 'visibility', 'none');
this.className = '';
} else {
this.className = 'active';
map.setLayoutProperty(clickedLayer, 'visibility', 'visible');
map.setLayoutProperty('Block Patches', 'visibility', 'visible');
}
};

var layers = document.getElementById('menu');
layers.appendChild(link);


</script>

</body>
</html>
