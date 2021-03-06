# GeoJSON for Google Maps

version 2.0

## Overview

**GeoJSON for Google Maps** enables you to create [Google Maps API v3 overlays](https://developers.google.com/maps/documentation/javascript/reference) from [GeoJSON](http://geojson.org) objects and create GeoJSON objects from Google Maps overlays.

## google.maps.geojson.from(geojson, [options])

Convert GeoJSON objects into Google Maps overlays. 

**geojson** - A supported and valid GeoJSON object. See the [official GeoJSON spec](http://geojson.org/geojson-spec.html) for more details, including the list of [examples](http://geojson.org/geojson-spec.html#appendix-a-geometry-examples).
    
**options** - (optional) Options for the specific type of Google Maps vector (Marker, Polyline, Polygon). If none specified, boring black vectors and red markers will be created.


    // Convert the GeoJSON into a Google Maps Marker
    var marker = google.maps.geojson.from(geojson);
    
    // Add the marker to the map
    marker.setMap(map);


### Conversions

<table>
<tr><th>GeoJSON</th><th>Google Maps</th></tr>
<tr><td>Point</td><td>google.maps.Marker</td></tr>
<tr><td>LineString</td><td>google.maps.Polyline</td></tr>
<tr><td>Polygon</td><td>google.maps.Polygon</td></tr>
<tr><td>MultiPoint</td><td>Array of google.maps.Marker</td></tr>
<tr><td>MultiLineString</td><td>Array of google.maps.Polyline</td></tr>
<tr><td>MultiPolygon</td><td>Array of google.maps.Polygon</td></tr>
<tr><td>Feature</td><td>google.maps.[Marker,Polyline,Polygon] (depends on Feature geometry type)</td></tr>
<tr><td>FeatureCollection</td><td>Array of google.maps.[Marker,Polyline,Polygon] (depends on Feature geometry type)</td></tr>
<tr><td>GeometryCollection</td><td>Array of google.maps.[Marker,Polyline,Polygon] (depends on geometry type)</td></tr>
</table>

## google.maps.geojson.to(overlays)

Convert Google Maps overlays into the most simple GeoJSON object possible.

**overlays** - An array of Google Maps overlays. Only Marker, Polyline, and Polygon are supported.

    // Convert Google Maps polygon to a GeoJSON Polygon
    var geojson = google.maps.geojson.to(polygon);

### Conversions

<table>
<tr><th>Google Maps</th><th>GeoJSON</th></tr>
<tr><td>google.maps.Marker</td><td>Point</td></tr>
<tr><td>google.maps.Polyline</td><td>LineString</td></tr>
<tr><td>google.maps.Polygon</td><td>Polygon</td></tr>
<tr><td>Array of google.maps.Marker</td><td>MultiPoint</td></tr>
<tr><td>Array of google.maps.Polyline</td><td>MultiLineString</td></tr>
<tr><td>Array of google.maps.Polygon</td><td>MultiPolygon</td></tr>
<tr><td>Mixed array of supported types</td><td>GeometryCollection</td></tr>
</table>

If an array containing only one Google Maps overlay is given then it will be distilled 
down to the most simple GeoJSON object possible. For example, if given an array with
just one google.maps.Marker, a GeoJSON Point will be returned instead of a MultiPoint.

## Error Handling

`from` checks for properly formatted GeoJSON. 

`to` returns an error object for unsupported Google Maps Overlay types.

    {
      "type": "Error",
      "message": "Invalid GeoJSON object: Feature object missing \"properties\" or \"geometry\" member."
    }

It would be in your best interest to first check the returned object to see if it's error free.
  
    var overlay = google.maps.geojson.from(myGeoJSON, options);
  
    if (overlay.type === "Error"){
      // Handle the error.
    } else {
      overlay.setMap(myMap);
    }

## Contributors

* [Jason Sanford](https://github.com/JasonSanford)
* [Grady McCrackin](https://github.com/gmccrackin)
* [Justin York](https://github.com/justincy)