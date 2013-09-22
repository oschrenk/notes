# TileMill #

- [TileMill](http://tilemill.com/)

## Create an offline map ##

While I love the feel as well as the underlying technology stack of TileMill, I still get frustrated with how complicated it is to create a good looking map.

A good starting point is [OSM Bright](http://github.com/mapbox/osm-bright).

### Install Requirements ###

	brew install postgresql
	brew install postgis
	brew install osm2pgsql --with-protobuf-c

### Run postgres ###

Make sure that you are running the newly installed `psql`

	$ which psql
	> /usr/local/bin/psql

If this is your first install of postgresql, create a database with

	initdb /usr/local/var/postgres -E utf8

This will create a database in

	/usr/local/var/postgres

Start the database server

	postgres -D /usr/local/var/postgresql

Start a new terminal window

	createdb
	psql -h localhost

### Create database ###

	create database osm;
	\connect osm
	create extension postgis;

### Download the OSM extracts ###

http://download.geofabrik.de/

http://download.bbbike.org/osm/

Go to [http://metro.teczno.com](http://metro.teczno.com) and look for your city. If it’s available, download the `.osm.pbf` version of the extract.

### Import OpenStreetMap data ###

	osm2pgsql -c -G -U <postgres_user> -d <postgis_database> <data.osm.pbf>


### Download & set up OSM Bright ###

	git clone git://github.com/mapbox/osm-bright.git

I delete the old OSM-Project to be sure

	rm -r ~/Documents/MapBox/project/OSMBright

Configure the new project

	cd osm-bright
	cp configure.py.sample configure.py
	./make.py

### Run Tile Mill ###

If you open TileMill, the Projects view should show you a new map. It will take a bit of time to load at first - the project needs to download about 350 MB of additional data. After some waiting you should see the continents appear on the map. Zoom into the area that your imported data covers and you should see streets and cities appear.

Just zoom in on the part of the map you have imported to see if eerything worked fine



### Export ###

1. Click the `Export` button. A drop down menu will appear.
2. Click `MBTiles`. The window will transition to the export tool.
3. Choose a `Filename`. The name of the project will be placed here by default.
4. `Select Zoom levels`. Set the furthest zoom to 1 by dragging the left end to the right. Set the closest zoom to 6 by dragging the right end to the left. The numbers below the slider update as the zoom level is moved. These numbers indicate how large the exported file will be. The wider the bounds and the more zoom levels in a map, the larger the final size of the MBTile file will be.
5. Select the “Center” of the map. This determines the starting center and zoom level of the map when it is first loaded. You can manually enter these values or click a point in the map preview. Zoom to level three and click the center of the United States.
6. Select the map “Bounds”. This is the area of the map to be exported. By default the entire world is selected. If your map is allocated to a smaller region of the globe, you can save processing time and disk space by cropping to that area. This can be done by manually entering values in the Bounds fields, or by holding the SHIFT key and clicking and dragging on the map. Leave the default value.
7. Click “Export”.

## Troubleshooting ##

### psql: FATAL: database “<user>” does not exist ###

It appears that your package manager failed to create the database named `$user` for you. Try

	createdb
	psql -h localhost

### ERROR:  could not open extension control file "/usr/local/Cellar/postgresql/9.3.0/share/postgresql/extension/postgis.control": No such file or directory ###

	osm=# create extension postgis;
	ERROR:  could not open extension control file "/usr/local/Cellar/postgresql/9.3.0/share/postgresql/extension/postgis.control": No such file or directory

I recently upgraded to Postgresql 9.3 but my PostGis 2.1 installation wasn#t aware

	brew remove postgis
	brew install postgis

### Library not loaded: /usr/local/opt/sqlite/lib/libsqlite3.0.8.6.dylib ###

When trying to install

	ERROR:  could not load library "/usr/local/Cellar/postgresql/9.3.0/lib/rtpostgis-2.1.so": dlopen(/usr/local/Cellar/postgresql/9.3.0/lib/rtpostgis-2.1.so, 10): Library not loaded: /usr/local/opt/sqlite/lib/libsqlite3.0.8.6.dylib
	  Referenced from: /usr/local/lib/libspatialite.5.dylib
	  Reason: image not found

This helped

	ln -s /usr/local/opt/sqlite/lib/libsqlite3.0.dylib /usr/local/opt/sqlite/lib/libsqlite3.0.8.6.dylib

### dyld: Library not loaded: /usr/local/lib/libgeos-3.3.8.dylib ###

When executing osm2psql

	brew remove osm2psql
	brew install osm2psql

### ERROR: PBF support has not been compiled into this version of osm2pgsql, please either compile it with pbf support or use one of the other input formats ###

I got

	ERROR: PBF support has not been compiled into this version of osm2pgsql, please either compile it with pbf support or use one of the other input formats

when trying to import the protobuf files.

The solution is to to reinstall osm2pgsql with protobuf support

	brew remove osm2psql
	brew install osm2pgsql --with-protobuf-c

## Resources ##

- [OSM Bright Mac OS X quickstart](http://www.mapbox.com/tilemill/docs/guides/osm-bright-mac-quickstart/)


