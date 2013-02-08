# GeoJSON #

GeoJSON is a format for encoding geographic data structures. It may represent a geometry, a feature, or a collection of features and supports the following geometry types: `Point`, `LineString`, `Polygon`, `MultiPoint`, `MultiLineString`, `MultiPolygon`, and `GeometryCollection`.

[Specification 1.0](http://www.geojson.org/geojson-spec.html)

## Example ##

A street with some metadata

	{
		"type": "FeatureCollection",
		"features": [
	        {
		        "type": "Feature",
	            "geometry": {
	            	"type": "LineString",
	                "coordinates": [
	                    [
	                        -122.485466,
	                        37.744493
	                    ],
	                    [
	                        -122.485335,
	                        37.742626
	                    ]
	              	]
	            },
	            "properties": {
	                "CLASSCODE": "4",
	                "CNN": 1594000,
	                "STREETNAME": "28TH AVE",
	                "ZIP_CODE": "94116"
	            }
        	}
    	]
	}