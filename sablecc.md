## SableCC ##

[SableCC](http://sablecc.org/)

### Commpiler ###

#### Lexer ####

[Lexer](http://sablecc.org/wiki/LexerDetails)

### Integration ###

#### Maven ####

This plugin will process all `*.grammar` files in the sourceDirectory into a common generated sources output directory. This will occur during the generate-resources phase and the sources directory will be added to the project for the compile phase.

	<project>
	   ...
	      <build>
	         <plugins>
	            <plugin>
	               <groupId>org.codehaus.mojo</groupId>
	               <artifactId>sablecc-maven-plugin</artifactId>
	               <executions>
	                 <execution>
	                   <phase>generate-source</phase>
	                   <goals>
	                     <goal>generate</goal>
	                   </goals>
	                 </execution>
	               </executions>
	            </plugin>
	         </plugins>
	         ...
	      </build>
	   ...
	</project>

Options

- sourceDirectory - `src/main/sablecc`
- outputDirectory - `target/generated-sources/sablecc`
- timestampDirectory - `target` (used so grammars are not constantly regenerated)
