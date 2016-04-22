# MapCookie

Sick of point-click on only one pinpoint or parcel on the map to view the properties? 

Built based on HTML5 Canvas, MapCookie allows you to enable the users exploring the spatial data in browser just like using desktop GIS application.

Maps, tables and charts are rendering incrediably fast, and can be interacted using intuitive mouse operations (selection, brushing etc.)

## MapCookie

To start using MapCookie.js, past this pieace of code within the HEAD tags of your HTML page:
```javascript 
<link rel="stylesheet" href="http://mapcookie.github.io/cdn/css/mapcookie.css" />
<script src="http://mapcookie.github.io/cdn/js/mapcookie.js"></script>
```

Visualize your spatial data in MapCookie style
```
mapcookie.addMapLayer('map', 'http://mapcookie.github.io/test/nat.json');
```

```
mapcookie.addMapLayer('map', 'http://mapcookie.github.io/test/nat.json')
  .done(function(maplayer, table) {
    // maplayer: is the geometric layer with properties and functions defined in mapcookie.mapLayer
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
    mapcookie.showTable(maplayer);
  });
```

Multi-layer support
```javascript
// if you have more than one layer, they can be overlayed one-by-one.
mapcookie.addMapLayer('map', 'http://mapcookie.github.io/test/nat.json');
mapcookie.addMapLayer('map', 'http://mapcookie.github.io/test/weather.json');

// mapcookie manages the layers, and you can get the mapLayer instance by it's url
var top_map_layer = mapcookie.getTopMapLayer();
var nat_map_layer = mapcookie.getMapLayer('http://mapcookie.github.io/test/nat.json');

var map_layers = mapcookie.GetMapLayers();

mapcookie.showTable(top_map_layer);
mapcookie.showTable(nat_map_layer);

// you can hide/show a mayLayer
nat_map_layer.hide();
nat_map_layer.show();

// to remove a layer
mapcookie.removeMapLayer(); // the top layer will be poped-up and removed

// or you can specify which layer to be removed
mapcookie.removeMapLayer('http://mapcookie.github.io/test/nat.json');

// or get what has been selected
var selected = mapcookie.getSelected();
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

## MapCookie Plots

```javascript
mapcookie.plots.createHistogram(nat_map_layer, 'population', 10);
mapcookie.plots.createHistogram(nat_map_layer, 'population', 10, colorbrewer.YlGn);

mapcookie.plots.createPie(nat_map_layer, 'education'); // categorical data
```

## MapCookie + Leaflet
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

  // initialize MapCookie for rendering your GeoJSON data
  mapcookie.addMapLayer(map, 'http://mapcookie.github.io/test/nat.json');
```

## MapCookie + Google Maps
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
  
  // initialize MapCookie for rendering your GeoJSON data
  mapcookie.addMapLayer(map, 'http://mapcookie.github.io/test/nat.json');
}
```
