# mapcookies

MapCookie + Leaflet
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
  var cookie = MapCookie(map, L);
  
  /*
  // Leaflet example: add GeoJSON layer to the map once the file is loaded
  // load GeoJSON from an external file
  $.getJSON("rodents.geojson",function(data){
    // L.geoJson(data).addTo(map);
    cookie.addJsonMap(data);
  });
  */
    
  var jsonLayer = new MapCookie({
    url : '...',
    map : map,
    filetype: 'geojson'
  });
```

MapCookie + Google Maps
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
  
  var jsonLayer = new MapCookie({
    url : '...',
    map : map,
    filetype: 'geojson'
  });
}
```


```javascript
  var jsonLayer = new MapCookie.JsonLayer({
    url : '...',
    map : map,
    filetype: 'geojson'
  });
  
  var shpLayer = new MapCookie.ShapefileLayer({
    url : '...', // zip file with .shp .shx .dbf .prj
    map : map,
    filetype: 'geojson'
  });
  
  MapCookie.AddLayer(jsonLayer);
  MapCookie.AddLayer(shpLayer);
  
  //MapCookie.AddLayers(jsonLayer, shpLayer);
  //MapCookie.Layers.Reorder(shpLayer, jsonLayer);
  
  MapCookie.RemoveLayer(jsonLayer);
  
```

```javascript
  // Mouse events:  the default mouse events include 
  //    selection (single, by rectangle, by circle, by line)
  //    brushing
  
  MapCookie.ActiveLayer;
  
  MapCookie.Events.EnableLinking = true; // false
  
  shpLayer.OnSelection = function(ids) {
    
  };
  
  shpLayer.SetColorScheme(colorScheme);
  
  var field = shpLayer.GetField(fieldName);
  
  MapCookie.MakeQuantileMap( shpLayer, fieldName, k=5);
  
  
  MapCookie.OnSelection = function(layer, ids) {
    // gives your current layer and ids been selected
  };
  
  
```
