# JavaServer Faces

JSF is a Java-based Web application framework for creating web-based user interfaces.

JSF is a request-driven MVC web framework

## In Eclipse

Create a new project

	File > New > Dynamic Web Project 

- Set `Dynamic web module version` to `2.5`
- Set `Configuration` to `JavaServer Faces 1.2 Project` 

Using the configuration creates the following `web.xml`

	<servlet>
		<servlet-name>Faces Servlet</servlet-name>
		<servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>Faces Servlet</servlet-name>
		<url-pattern>/faces/*</url-pattern>
	</servlet-mapping>

Eclipse also creates the file `faces-config.xml` next to the `web.xml` containing:

	<?xml version="1.0" encoding="UTF-8"?>
	<faces-config
	    xmlns="http://java.sun.com/xml/ns/javaee"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-facesconfig_1_2.xsd"
	    version="1.2">
	</faces-config>

The servlet mapping creates a central servlet that handles all requests to `/faces/*`. In that regards the servlet uses the _FrontController_ Design Pattern: Each request is handled by one central instance which then delegates.

The `faces-config.xml` is empty for now.



## Bean Managment

With regard to the MVC (ModelViewController) Pattern the _Bean Managment_ is the modell. Beans are used for persisting data during a request or a user session.

JSF takes the role of managing the beans.

You add a bean by adding it to `faces-config.xml`:

	<managed-bean>
		<managed-bean-name>PersonBean</managed-bean-name>
		<managed-bean-class>demo.jsf.Person</managed-bean-class>
		<managed-bean-scope>session</managed-bean-scope>
	</managed-bean>

and writing the class as a JavaBean

The scope defines how objects are created and how long they live. The session scope creates a new object for each user's http session.

All beans are being lazy instantiated, meaning that they are only created on first access.

## Scopes

- _Application_ Managed Bean will be created once for the VM. All users share the same object. Good for configuration parameters
- _Session_ Managed Beanwill be created for each HTTP session.
- _Request_ Objects live only the time of the request. For the next request a new object will be created. If possible objects should live in the request scope as too much objects in the session are unwieldy.
	- _None_ Not really a scope. Very short lifetime. Object will be created for each call () e.g. a method call). After that it goes to garbage collection. Used very little in practice

## Showing the code

Here is a simple code fragment for showing the bean.

	<%@ taglib uri="http://java.sun.com/jsf/html" prefix="h" %>
	<%@ taglib uri="http://java.sun.com/jsf/core" prefix="f" %>
	<html>
	<head>
		<meta http-equiv="content-type" content="text/html; charset=iso-8859-1">
		<title>Buch Portlets und JSF</title>
		<link rel="stylesheet" type="text/css" href="/DemoJSF/default.css" />
	</head>
	<f:view>  
	  	<h2>Beispiel Bean Management:</h2>
	    <h:form> 
			Guten Tag,  
			<h:outputText value="#{PersonBean.firstname} " /> 
					<h:outputText value="#{PersonBean.lastname}" />
			<br><br>
			Bitte geben Sie neue Werte ein:
			<br><br>
			Vorname: <h:inputText value="#{PersonBean.firstname}" />
			<br>
			Nachname: <h:inputText value="#{PersonBean.lastname}" />
			<br><br>
		  	<h:commandButton action="success" value="Submit" />
	  </h:form>
	</f:view>
	</html>

The expression `#{PersonBean.firstname}` is a good example for the _Expression Language_ to call Managed Beans. The first part (`PersonBean`) is the identifier used in the `faces-config.xml` and the second part describes the member. Following the Bean Convention it expects `getFirstname`. As you bind a value to the component these expressions are also called _ValueExpression_

## AfterConstruct, PreDestroy Annotations

You can annotate methods in the bean with

- `@AfterConstruct` Method is called directly after constructor
- `@PreDestroy` Called before destroying object

You can annotate methods which follow these rules:
- method has no arguments
- returns void
- throws no checked exceptions

## Navigation

Each paging, each navigation rule is defined in the central xml file (normally `faces-config.xml`).

	<navigation-rule>
		<from-view-id>/faces/navigation/index.jsp</from-view-id>
		<navigation-case>
			<from-outcome>chapter_1</from-outcome>
			<to-view-id>/faces/navigation/chapter_1.jsp</to-view-id>
		</navigation-case>
	</navigation-rule>

Here page changes from `/faces/navigation/index.jsp` with the _outcome_ `chapter_1` are redirected to `/faces/navigation/chapter_1.jsp`

Outcomes can be created via

	<h:commandButton action="chapter_1" value="Chapter 1" />

Outcomes that don't match a rule the same page is loaded.

### Dynamic Navigation

The above rules are pretty static. Sometimes you need a more dynamic approach, for example if the outcome is dependent on the output.

Just use a bean!

	<h:commandButton action="#{Clazz.getCompletNavigationResult}" value="Chapter 1" />

	class Clazz {
		...
		public String getCompletNavigationResult () {
			if (false) {
				return "no_way"
			}
			
			return "yay";
		}
	}

This is a good example for a `MethodExpression` as we refer to a method and not a member.


## Conversion and Validation

## Internalization

## Event Handling

# UI Components

# Appendix

## FAQ/Problems

### java.lang.RuntimeException: Cannot find FacesContext