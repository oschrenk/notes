# Creating a JSF-Portlet Application #

	mvn archetype:generate\
	 -DinteractiveMode=false\
	 -DarchetypeGroupId=org.apache.maven.archetypes\
	 -DarchetypeArtifactId=maven-archetype-portlet\
	 -DgroupId=com.acme\
	 -DartifactId=project
	
	cd project
	git init
	touch README.markdown

	touch .gitignore
	echo ".project" >> .gitignore
	echo ".classpath" >> .gitignore
	echo ".settings" >> .gitignore
	echo "target" >> .gitignore

	git add .gitignore
	git add README.markdown
	git add pom.xml
	git add src/
	git commit . -m "Initial release"
	
Import into Eclipse via _m2eclipse_ using `File > Import ... > Maven > Existing Maven Projects, Next, Browse ..., (browse to project), Finish`

## Creating JBOSS JSF+Portlet ##

	mvn archetype:generate\
	 -DinteractiveMode=false\
	 -DarchetypeCatalog=http://repository.jboss.com/maven2/archetype-catalog.xml\
	 -DarchetypeGroupId=org.jboss.portletbridge.archetypes\
	 -DarchetypeArtifactId=1.2-basic\
	 -DgroupId=com.acme\
	 -DartifactId=project2