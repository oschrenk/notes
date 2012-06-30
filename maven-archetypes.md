# Maven Archetypes #

[Maven Arcehtype Specification](http://maven.apache.org/archetype/maven-archetype-plugin/specification/archetype-metadata.html)


To create a new archetype

	mkdir archetype
	cd archetype
	mkdir -p src/main/resources/META-INF/maven/
	mkdir -p src/main/resources/archetype-resources
	touch pom.xml
	touch src/main/resources/META-INF/maven/archetype-metadata.xml

A minimal `archetype-metadata.xml` file looks like:

	<?xml version="1.0" encoding="UTF-8"?>
	<archetype-descriptor name="basic">
	  <fileSets>
	    <fileSet filtered="true" packaged="true">
	      <directory>src/main/java</directory>
	      <includes>
	        <include>**/*.java</include>
	      </includes>
	    </fileSet>
	  </fileSets>
	</archetype-descriptor>

This example above shows:

- the archetype name is basic (it should match the `artifactId` in the `pom.xml`)
- the archetype defines a single fileset:
	- the fileset will take all the files in `archetype-resources/src/main/java` that match the `**/*.java` pattern
	- the selected files will be generated using the Velocity engine (`filtered=true`)
	- the files will be generated in the `src/main/java` directory of the generated project in the same directory as in the JAR file, but with that directory prepended by the package property.


Create your prototype files under `src/main/resources/archetype-resources`

	touch src/main/resources/archetype-resources/pom.xml

Install it
	
	mvn install

Use it

	mvn archetype:generate                    \
	  -DarchetypeGroupId=com.oschrenk.maven   \
	  -DarchetypeArtifactId=default-archetype \
	  -DarchetypeVersion=1.0

It interactively asks for `groupId`, `artifactId`, `version` and `package`, fill it out appropiately.

## Configuring the archetype ##

## FAQ ##

- [How to create empty folders with maven archetype?](http://stackoverflow.com/questions/2786966/how-to-create-empty-folders-with-maven-archetype)
