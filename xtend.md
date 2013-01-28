# Xtend #

## Maven, Eclipse Setup ##

Mostly taken from [here](http://kthoms.wordpress.com/2011/12/08/xtext-2-2-finally-brings-maven-support-for-xtend/) but I had to change some of the dependencies.<>

Add the following repopsitories (I added them to `~/.m2/settings.xml`)

	<repository>
		<id>maven.eclipse.org</id>
		<url>http://maven.eclipse.org/nexus/content/groups/public/</url>
	</repository>
	<repository>
		<id>xtend</id>
		<url>http://build.eclipse.org/common/xtend/maven/</url>
	</repository>
	<repository>
		<id>xtext</id>
		<url>http://build.eclipse.org/common/xtext/maven/maven-snapshot/final/</url>
	</repository>

Add the following dependencies

	<dependency>
		<groupId>com.google.inject</groupId>
		<artifactId>guice</artifactId>
		<version>3.0</version>
	</dependency>

	<dependency>
		<groupId>org.eclipse.xtend2</groupId>
		<artifactId>org.eclipse.xtend2.lib</artifactId>
		<version>2.2.1</version>
	</dependency>

	<dependency>
		<groupId>org.eclipse.xtext</groupId>
		<artifactId>org.eclipse.xtext.xtend2.lib</artifactId>
		<version>2.2.0.v201112121625</version>
	</dependency>

Create `xtend-gen` src directory

	<plugin>
		<groupId>org.codehaus.mojo</groupId>
		<artifactId>build-helper-maven-plugin</artifactId>
		<version>1.7</version>
		<executions>
			<execution>
				<id>add-source</id>
				<phase>generate-sources</phase>
				<goals>
					<goal>add-source</goal>
				</goals>
				<configuration>
					<sources>
						<source>xtend-gen</source>
					</sources>
				</configuration>
			</execution>
		</executions>
	</plugin>

Clean the `xtend-gen` directory

	<plugin>
		<artifactId>maven-clean-plugin</artifactId>
		<version>2.4.1</version>
		<configuration>
			<filesets>
				<fileset>
					<directory>xtend-gen</directory>
					<includes>
						<include>**</include>
					</includes>
				</fileset>
			</filesets>
		</configuration>
	</plugin>

Configure the xtend plugin

	<plugin>
		<groupId>org.eclipse.xtend2</groupId>
		<artifactId>xtend-maven-plugin</artifactId>
		<version>2.2.0</version>
		<executions>
			<execution>
				<goals>
					<goal>compile</goal>
					<goal>testCompile</goal>
				</goals>
				<configuration>
					<outputDirectory>xtend-gen</outputDirectory>
				</configuration>
			</execution>
		</executions>
	</plugin>

## Problems ##

### Plugin execution not covered by lifecycle configuration: org.codehaus.mojo:build-helper-maven-plugin ###

In Eclipse I got the following error message

	Plugin execution not covered by lifecycle configuration: org.codehaus.mojo:build-helper-maven-plugin:1.7:add-source (execution: add-source, phase: generate-sources)	pom.xml	/core	line 5	Maven Project Build Lifecycle Mapping Problem

To solve it open Plugin execution not covered by lifecycle configuration open `Preferences > Maven > Discovery > open catalog` and search for `buildhelper` (no space or hyphen) and install it. You have to accept unsigned content and restart Eclipse.

### Plugin execution not covered by lifecycle configuration: org.eclipse.xtend2:xtend-maven-plugin ###

	Plugin execution not covered by lifecycle configuration: org.eclipse.xtend2:xtend-maven-plugin:2.2.0:compile (execution: default, phase: generate-sources)	pom.xml	/core	line 5	Maven Project Build Lifecycle Mapping Problem

This is a known bug. The [bug report](https://bugs.eclipse.org/bugs/show_bug.cgi?id=366118) has a workarround by adding

	<pluginManagement>
		<plugins>
			<!--This plugin's configuration is used to store Eclipse m2e settings
				only. It has no influence on the Maven build itself. -->
			<!-- https://bugs.eclipse.org/bugs/show_bug.cgi?id=366118 -->
			<plugin>
				<groupId>org.eclipse.m2e</groupId>
				<artifactId>lifecycle-mapping</artifactId>
				<version>1.0.0</version>
				<configuration>
					<lifecycleMappingMetadata>
						<pluginExecutions>
							<pluginExecution>
								<pluginExecutionFilter>
									<groupId>
										org.eclipse.xtend2
									</groupId>
									<artifactId>
										xtend-maven-plugin
									</artifactId>
									<versionRange>
										[2.2.0,)
									</versionRange>
									<goals>
										<goal>compile</goal>
										<goal>testCompile</goal>
									</goals>
								</pluginExecutionFilter>
								<action>
									<ignore></ignore>
								</action>
							</pluginExecution>
						</pluginExecutions>
					</lifecycleMappingMetadata>
				</configuration>
			</plugin>
		</plugins>
	</pluginManagement>

to your pom.
