# Nashorn #

## JDK 7 Backport ##

You can get Nashorn working with JDK7 by using a backport of it

	hg clone https://bitbucket.org/ramonza/nashorn-backport
	cd nashorn-backport
	ant -f make/build.xml

Upon building it you should get a nashorn.jar file inside the dist folder of your cloned repository.

Now you need to add this jar to your bootclasspath using a VM option similar to this:

	-Xbootclasspath/a:C:/nashorn-backport/dist/nashorn.jar

## Troubleshooting ##

### Error opening connection java.io.FileNotFoundException ###

	Error opening connection java.io.FileNotFoundException: http://oss.sonatype.org/content/repositories/snapshots/org/dynalang/dynalink/0.5-SNAPSHOT/dynalink-0.5-20121218.140128-11.jar

Dynalink 0.5 was released on 2013-02-20. The snapshot dependency is not available form maven central anymore. Please update the build to use Dynalink 0.5.

Apply this patch

	diff -r 3f60c4d6f756 make/project.properties
	--- a/make/project.properties	Thu Dec 27 05:13:04 2012 +0200
	+++ b/make/project.properties	Mon Mar 25 10:28:30 2013 +0100
	@@ -89,9 +89,9 @@
	 # "-SNAPSHOT" suffix and the jar version will have a timestamp in it. When
	 # it's 'release', the version has no suffix, and the jar version is
	 # identical to version - fun with Maven central.
	-dynalink.version=0.5-SNAPSHOT
	-dynalink.version.type=snapshot
	-dynalink.jar.version=0.5-20121218.140128-11
	+dynalink.version=0.5
	+dynalink.version.type=release
	+dynalink.jar.version=0.5
	 dynalink.dir.name=dynalink
	 dynalink.dir=build/${dynalink.dir.name}
	 dynalink.jar=${dynalink.dir}/dynalink.jar
