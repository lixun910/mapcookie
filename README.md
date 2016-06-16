# PocketMap

Sick of point-click on only one pinpoint or parcel on the map to view the properties? 

Built based on HTML5 Canvas, PocketMap allows you to enable the users exploring the spatial data in browser just like using desktop GIS application.

Maps, tables and charts are rendering incrediably fast, and can be interacted using intuitive mouse operations (selection, brushing etc.)

## PocketMap

To start using PocketMap.js, past this pieace of code within the HEAD tags of your HTML page:
```javascript 
<link rel="stylesheet" href="http://pocketmap.github.io/cdn/css/pocketmap.css" />
<script src="http://pocketmap.github.io/cdn/js/pocketmap.js"></script>
```

Visualize your spatial data in PocketMap style
```
pocketmap.addMapLayer('map', 'http://pocketmap.github.io/test/nat.json');
```

```
pocketmap.addMapLayer('map', 'http://pocketmap.github.io/test/nat.json')
  .done(function(maplayer, table) {
    // maplayer: is the geometric layer with properties and functions defined in pocketmap.mapLayer
    // table: is a json defined in-memory data table associated with the map layer -- each row in the table is mapping to a geometry in maplayer.
    
    // you can add some color to your map by the property values
    maplayer.getFeatures(function(feature) {
      if (feature['south'] == 1) {
        maplayer.setColor(feature, "#00FFFF");
      } else {
        maplayer.setColor(feature, "#FFFFFF");
      }
    });
    
    // or create a professional built-in choropleth map
    maplayer.createQuantileMap('population', 5);
    
    maplayer.createQuantileMap('population', 5, colorbrewer.YlGn);
    
    // you can show the related table in a floating div
    pocketmap.showTable(maplayer);
  });
```

Multi-layer support
```javascript
// if you have more than one layer, they can be overlayed one-by-one.
pocketmap.addMapLayer('map', 'http://pocketmap.github.io/test/nat.json');
pocketmap.addMapLayer('map', 'http://pocketmap.github.io/test/weather.json');

// pocketmap manages the layers, and you can get the mapLayer instance by it's url
var top_map_layer = pocketmap.getTopMapLayer();
var nat_map_layer = pocketmap.getMapLayer('http://pocketmap.github.io/test/nat.json');

var map_layers = pocketmap.GetMapLayers();

pocketmap.showTable(top_map_layer);
pocketmap.showTable(nat_map_layer);

// you can hide/show a mayLayer
nat_map_layer.hide();
nat_map_layer.show();

// to remove a layer
pocketmap.removeMapLayer(); // the top layer will be poped-up and removed

// or you can specify which layer to be removed
pocketmap.removeMapLayer('http://pocketmap.github.io/test/nat.json');

// or get what has been selected
var selected = pocketmap.getSelected();
var nat_selected = nat_map_layer.getSelected();

// you can set selected if you needed
var my_selected = [];
nat_map_layer.getFeatures(function(feature){
  if (feature['population'] < 100000) {
    my_selected.append(feature.id);
  }
});

nat_map_layer.setSelected(my_selected);
```

## PocketMap Plots

```javascript
pocketmap.plots.createHistogram(nat_map_layer, 'population', 10);
pocketmap.plots.createHistogram(nat_map_layer, 'population', 10, colorbrewer.YlGn);

pocketmap.plots.createPie(nat_map_layer, 'education'); // categorical data
```

## PocketMap + Leaflet
```javascript
  // initialize the map
  var map = L.map('map').setView([42.35, -71.08], 13);

  // load a tile layer
  L.tileLayer('http://tiles.mapc.org/basemap/{z}/{x}/{y}.png',
    {
      attribution: 'Tiles by <a href="http://mapc.org">MAPC</a>, Data by <a href="http://mass.gov/mgis">MassGIS</a>',
      maxZoom: 17,
      minZoom: 9
    }).addTo(map);

  // initialize PocketMap for rendering your GeoJSON data
  pocketmap.addMapLayer(map, 'http://pocketmap.github.io/test/nat.json');
```

## PocketMap + Google Maps
```javascript
function initMap() {
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 11,
    center: {lat: 41.876, lng: -87.624}
  });

  /*
  var ctaLayer = new google.maps.KmlLayer({
    url: 'http://googlemaps.github.io/js-v2-samples/ggeoxml/cta.kml',
    map: map
  });
  */
  
  // initialize PocketMap for rendering your GeoJSON data
  pocketmap.addMapLayer(map, 'http://pocketmap.github.io/test/nat.json');
}
```
