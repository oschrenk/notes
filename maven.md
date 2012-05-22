# Maven #

- [Available Plugins](http://maven.apache.org/plugins/index.html)
- [Descriptor reference](http://maven.apache.org/ref/3.0.3/maven-model/maven.html)
- [Subscribe to Announcements](mailto:announce-subscribe@maven.apache.org) Updates to core and plugins

- create base directories `mkdir -p src/{main,test}/{java,resources}`

## Installation ##

    $ brew install maven

## Configuration ##

The settings can be found under `~/.m2/settings.xml`

### Custom Properties Precedence ###

I found a [blog entry](http://davidmarquis.posterous.com/maven-custom-properties-precedence) that has solved the (documentation) problem and found the following precedence:

1. System properties: set with `-Dxyz=value` on the command line.
2. From currently active profile(s): `settings.xml` in user home directory first, then `profiles.xml` in project root directory, then in profiles defined in your `pom.xml`. If many profiles are active, and a property is defined in more than one of those, the order of precedence is based on the last profile in which this property is defined, in alphabetical order of profile name.
3. In the properties section of your `pom.xml`.
4. Lastly, in properties defined in filters. If a property is defined in multiple filters, then the last one (in order of appearance in your filters section) has precedence over the others.

### Proxy ###

    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          http://maven.apache.org/xsd/settings-1.0.0.xsd"> 
      ...
      <proxies>
       <proxy>
          <active>true</active>
          <protocol>http</protocol>
          <host>proxy.acme.com</host>
          <port>3128</port>
          <username></username>
          <password></password>
          <nonProxyHosts>www.google.com|*.somewhere.com</nonProxyHosts>
        </proxy>
      </proxies>
      ...
    </settings>

### Set the local repository ###

As I use multiple user on my machine for different clients (it helps keep my mind sane) and don't want to have a repository for each user I decided to move it to a shared location on my hard drive. Normally it is found in `~/.m2/repository`

To change the path to the repository open your settings under `~/.m2/settings.xml` and add/change the following

    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          http://maven.apache.org/xsd/settings-1.0.0.xsd">
      ...
    	<localRepository>/path/to/shared/repository</localRepository>
      ...
    </settings>

## Usage ##

### Create a new Maven Project ###

Create a basic layout and pom.xml file

    $ mvn archetype:create -DgroupId=com.acme -DartifactId=project

You get a pom file like this:

    <?xml version="1.0"?>
    	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    	<modelVersion>4.0.0</modelVersion>
    	<groupId>com.acme</groupId>
    	<artifactId>project</artifactId>
    	<packaging>jar</packaging>
    	<version>1.0-SNAPSHOT</version>
    	<name>project</name>
    	<url>http://maven.apache.org</url>
    	<dependencies>
    		<dependency>
    			<groupId>junit</groupId>
    			<artifactId>junit</artifactId>
    			<version>3.8.1</version>
    			<scope>test</scope>
    		</dependency>
    	</dependencies>
    </project>

Where

*   `groupId` is the unique identifier of your organization
*   `artifactId` is the name of your project
*   `version` is the version number
*   `packaging` defaults to jar
*   `name` is a descriptive name

### Install a 3-rd party library ###

[Mavenizer](http://mavenizer.sourceforge.net/) is a tool that can help
you mavenize 3rd party libraries

This is a **very** simple example as the library doesn't have any external dependencies. Beware that the following code copies the mavenized project to your local maven repository.

    $ mkdir mavenize
    $ cd mavenize
    $ wget http://downloads.sourceforge.net/project/mavenizer/Mavenizer%20CLI/1.0.0-alpha-1/mavenizer-cli-1.0.0-alpha-1-jar-with-dependencies.jar
    $ wget http://downloads.sourceforge.net/project/java-base64/Java%20Base64/1.3.1/javabase64-1.3.1.zip
    $ unzip javabase64-1.3.1.zip
    $ rm javabase64-1.3.1.zip
    $ mv javabase64-1.3.1/javabase64-1.3.1.jar .
    $ rm -r javabase64-1.3.1
    $ java -jar mavenizer-cli-1.0.0-alpha-1-jar-with-dependencies.jar analyse -workdir javabase64-mavenize javabase64-1.3.1.jar
    $ java -jar mavenizer-cli-1.0.0-alpha-1-jar-with-dependencies.jar generate -workdir javabase64
    $ cp -r javabase64/repository/ ~~/.m2/repository/

Normally you will have more complex cases. Try to track down which libraries are used and put them next to library to be mavenized. Best to put all files in a directory and call mavenizer on that directory. The analyzer will generate a `mavenizer.xml` file, which you have to go trough yourself. Hopefully you can replace the dependency with known artifacts.

#### A simple (read lazy but quick) way ####

The [Mavenize](#mavenize) way is the better (more complete/correct) choice but sometimes you can't be bothered with dependency hell. Just use the following command and change according to your needs:

    $ mvn install:install-file -Dfile=library.jar -DgroupId=com.company -DartifactId=libraryname -Dversion=1.2.3 -Dpackaging=jar

### Deploy a 3rd party library ###

	mvn deploy:deploy-file -DgroupId=<group-id> \
	  -DartifactId=<artifact-id> \
	  -Dversion=<version> \
	  -Dpackaging=<type-of-packaging> \
	  -Dfile=<path-to-file> \
	  -DrepositoryId=<id-to-map-on-server-section-of-settings.xml> \
	  -Durl=<url-of-the-repositor-to-deploy>

## Site ##

- [Fluido Skin](http://maven.apache.org/skins/maven-fluido-skin/)

## FAQ/Problems ##

### MacRoman encoding creeping into build process ###

Maven uses the platform encoding when building by default. It will produces warnings like that

	WARNING Using platform encoding
	(MacRoman actually) to copy↩  
	filtered resources, i.e. build is platform dependent!

Add a `project.build.sourceEncoding` element and a `project.reporting.outputEncoding` to your `pom.xml`.

    <project
      xmlns="http://maven.apache.org/POM/4.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 ↩
    http://maven.apache.org/maven-v4_0_0.xsd">

    `<modelVersion>4.0.0</modelVersion>
    	<groupId>com.example</groupId>
    	<artifactId>mywebapp</artifactId>
    	<packaging>war</packaging>
    	<version>1.21</version>
    	<name>mywebapp</name>`

    `<!-- snip -->
    	<properties>
        	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        	<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    	</properties>
    <!-- snip -->
    </project>`

### Change source level of maven project ###

Add the following snippet to your `pom.xml`

    <build>
    	<plugins>
    		<plugin>
    			<groupId>org.apache.maven.plugins</groupId>
    			<artifactId>maven-compiler-plugin</artifactId>
    			<configuration>
    				<source>1.5</source>
    				<target>1.5</target>
    			</configuration>
    		</plugin>
    	</plugins>
    </build>

Change the `source` and `target` tags according to your needs

### To change source encoding ###

	<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<encoding>iso-8859-1</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>


### Maven, Junit tests and resource loading ###

I always forget.
	
	private static final String PATH="/path/to/file.csv";
	[...]
	final File file=new File(this.getClass().getResource(PATH).getFile());

This way the classloader finds the proper resource