# Osmosis #

[SVN Repository](http://svn.openstreetmap.org/applications/utils/osmosis/trunk/)

I [created a mirror](http://www.fnokd.com/2008/08/20/mirroring-svn-repository-to-github/) (only for the trunk) on github

	mkdir osmosis
	cd osmosis
	git svn init http://svn.openstreetmap.org/applications/utils/osmosis/trunk
	git svn fetch
	git gc
	hub create
	# repo needs a proper master
	git push origin master
	# create copy of master
	git checkout -b vendor
	git push origin vendor

Preparing it for git flow

	git checkout -b develop
	git flow init
	# just confirm the defaults
	git push origin develop

Prepare feature

	git flow feature start ignore

Building for the first time (takes some time). It requires Java 1.6

	ant publish

Add the generated stuff to `.gitignore`

	git ignore apidb/build/
	git ignore apidb/distrib/
	git ignore apidb/lib/
	git ignore apidb/report/
	git ignore apidb/test/data/input/
	git ignore areafilter/distrib/
	git ignore areafilter/lib/
	git ignore areafilter/report/
	git ignore build-support/ivy/
	git ignore build/
	git ignore core/build/
	git ignore core/distrib/
	git ignore core/lib/
	git ignore core/report/
	git ignore core/src/org/openstreetmap/osmosis/core/OsmosisConstants.java
	git ignore core/src/org/openstreetmap/osmosis/core/plugin/plugin.xml
	git ignore core/test/data/input/
	git ignore dataset/distrib/
	git ignore dataset/lib/
	git ignore dataset/report/
	git ignore extract/distrib/
	git ignore extract/lib/
	git ignore extract/report/
	git ignore package/distrib/
	git ignore package/lib/
	git ignore package/report/
	git ignore pbf/distrib/
	git ignore pbf/lib/
	git ignore pbf/report/
	git ignore pgsimple/distrib/
	git ignore pgsimple/lib/
	git ignore pgsimple/report/
	git ignore pgsnapshot/distrib/
	git ignore pgsnapshot/lib/
	git ignore pgsnapshot/report/
	git ignore replication/build/
	git ignore replication/distrib/
	git ignore replication/lib/
	git ignore replication/report/
	git ignore set/build/
	git ignore set/distrib/
	git ignore set/lib/
	git ignore set/report/
	git ignore set/test/data/input/
	git ignore tagfilter/distrib/
	git ignore tagfilter/lib/
	git ignore tagfilter/report/
	git ignore xml/build/
	git ignore xml/distrib/
	git ignore xml/lib/
	git ignore xml/report/
	git ignore xml/test/data/input/

	git add .gitignore
	git c

Finish the feature

	git flow feature finish ignore
	git push origin develop

The release can be found under

	package

It can be used by calling

	package/bin/osmosis
