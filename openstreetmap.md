# OpenStreetMap #

- [maps.stamen](http://maps.stamen.com/) Map Tiles

## Extrakte ##

- [Geofabrik, OpenStreetMap planet.osm extracts](http://download.geofabrik.de/openstreetmap/)

## Editors ##

- [iD](https://github.com/systemed/iD) Editor in JavaScript in the browser.

## Dataformat ##

A [Node](http://wiki.openstreetmap.org/wiki/Node) consists of a single geospatial point using a latitude and longitude.

	<node id="25496583" lat="51.5173639" lon="-0.140043" version="1" changeset="203496" user="80n" uid="1238" visible="true" timestamp="2007-01-28T11:40:26Z">
	    <tag k="highway" v="traffic_signals"/>
	</node>

A [way](http://wiki.openstreetmap.org/wiki/Way) is an **ordered** list of nodes which normally also has at least one tag or is included within a Relation. A way can have between 2 and 2,000 nodes, although it's possible that faulty ways with zero or a single node exist. A way can be open or closed. A closed way is one whose last node on the way is also the first on that way. A closed way may be interpreted either as a closed polyline, or an area, or both

	<way id="5090250" visible="true" timestamp="2009-01-19T19:07:25Z" version="8" changeset="816806" user="Blumpsy" uid="64226">
		<nd ref="822403"/>
		<nd ref="21533912"/>
		<nd ref="821601"/>
		<nd ref="21533910"/>
		<nd ref="135791608"/>
		<nd ref="333725784"/>
		<nd ref="333725781"/>
		<nd ref="333725774"/>
		<nd ref="333725776"/>
		<nd ref="823771"/>
		<tag k="highway" v="residential"/>
		<tag k="name" v="Clipstone Street"/>
		<tag k="oneway" v="yes"/>
	</way>

A [Tag](http://wiki.openstreetmap.org/wiki/Tag) consists of `Key` and a `Value` and are used to describe elements (nodes, ways and relations) or changesets. Both the key and value are free format text fields, although in practice there are agreed conventions of how tags are used for most common purposes.

## Map features##

OpenStreetMap has free tagging system which allows the map to contain unlimited data about its elements.

A good overview of the tags that are used by conventions can be found [here](http://wiki.openstreetmap.org/wiki/Map_Features)

### Streets ###

Taking `bremen.osm` from Geofabrik as an example I extracted some metadata in order to understand the OpenStreetMaps tag conventions

[highway](http://wiki.openstreetmap.org/wiki/Map_Features#Highway)

	abandoned
	bridleway
	bus_guideway
	construction
	cycleway
	driveway
	footway
	footway;steps
	living_street
	motorway
	motorway_link
	path
	pedestrian
	platform
	primary
	primary_link
	proposed
	residential
	road
	secondary
	secondary_link
	service
	steps
	street_lamp
	tertiary
	tertiary_link
	track
	trunk
	trunk_link
	turning_circle
	unclassified