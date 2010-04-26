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

### Post-Redirect-Get

If you carefully examine the example of the static navigation you will see that the URL in the address bar lags behind. It shows the URL of the last visited URL instead of the one currently displayed. Some people think of this as a bug in JSF or sthe browser or even as an architectural problem within JSF.

Per default JSF handles URLs only afterwards and doesn't forward the browser the current/correct URL. This is because navigation is handled on the server with JSF - when sumbmitting a form JSF return the originating address. On the server the request will be forwarded to the following page.

This has some disadvantages
- it's confusing
- you can't boomark a page

To circumvent this problem you can add a redirection rule.

	<navigation-case>
		<from-outcome>chapter_3</from-outcome>
		<to-view-id>/faces/navigation/chapter_3.jsp</to-view-id>
		<redirect/>
	</navigation-case> 

This uses the POST-Redirect-GET Pattern

## Conversion and Validation

Until now we assumed that every data we received via for input was correct. What if the user enters wrong data?

JSF solves that problem with its own concepts. If the data doesn't isn't correct JSF throws a `javax.faces.convert.ConverterException`. To display error messages you can use `<h:messages />`, a placeholder for all error messages that occured while loading this page.

### Conversion

If you use only digits as your input and the method expects an `int`, JSF will convert the number implicitly. Sometimes this isn't possible. 

A good example when a conversion can't be decided automatically is when you are using a date or a time, you have to coerce it. An example

	<h:outputText value="#{PersonBean.birthday}" >
		<f:convertDateTime type="date" dateStyle="full"/>
	</h:outputText>
		
	<h:inputText value="#{PersonBean.birthday}">
		<f:convertDateTime type="date" dateStyle="short"/>
	</h:inputText>

There are many different prebuilt converters, even for floating point numbers. Keep in mind that when you use converter you (almost) always have to convert in two directions, when you convert a String into the needed datatype and the other way when displaying it.

### Validation

Even if the Convererts can ensure a syntactic correct data type, it can't ensure semantic correctness. A *validator* can do that. Each validator is called _after_ the conversion.

There are many prebuilt validators. Examples:

	<h:inputText value="#{PersonBean.lastname}">
		<f:validateLength minimum="3" maximum="10" />
	</h:inputText>

For validating more complex data you will have to write your own methods. I suggest putting it in a ManagedBean called `***Controller.java` with content similiar to this

	public class PersonController {
		public void validateMail(FacesContext jsfContext, UIComponent component,
				Object value) throws ValidatorException {
			String input = value.toString();
			boolean valid = input.indexOf("@") > 0;		
			if (!valid) {
				throw new ValidatorException(
						new FacesMessage("Not a valid address"));
			}
		}
	}
	
Just wire it up in your `faces-config.xml` (I set the scope to `application`) and use it like so:

	<h:inputText value="#{PersonBean.email}" validator="#{PersonController.validateMail}" />
	
As you may have noticed the `ValidatorException` takes a `FacesMessage` as its argument. A FacesMessages can and should be constructed using this 

	FacesMessage message = new FacesMessages("Short Message/Detail", "Long Message/Summary", FacesMessage.SEVERITY_WARN)

You can use other `SEVERITY` levels. The short message and long messaged can be displayed with:

	<h:messages showDetail="true" showSummary="true" />

## Event Handling

### ActionEvents and ActionListener

You can write `ActionListener` by implementing `javax.faces.event.ActionListener`. Here is one that writes some info to StandardOutput

	public class DemoActionListener implements ActionListener {
		public void processAction(ActionEvent event)
				throws AbortProcessingException {
			UIComponent component = event.getComponent();
			int phase = event.getPhaseId().getOrdinal();
			System.out.println("Command triggered by: " + component.getId()
					+ " in phase" + phase);
		}
	}
	
Use it by registering it:

	<h:commandButton action="#{CalculatorBean.calculate}"
			value="Calculate the sum with Event">
		<f:actionListener type="demo.jsf.DemoActionListener" />
	</h:commandButton>
	
Of course you can register multiple ActionListener.

Sometimes there is the question why there are ActionListener and Action methods. For me an action method results in a change of the navigation and therefore is visible for the user and an ActionListener adds functionality on the server side.

### ValueChangeEvents und ValueChangeListener

You can also monitor changes of values by adding a `ValueChangeListener` to a component

	public class DemoValueChangeListener implements ValueChangeListener {
		public void processValueChange(ValueChangeEvent event)
				throws AbortProcessingException {
			System.out.println(
					"ValueChange: \n" +
					"Old value:" + event.getOldValue() + "\n" + 
					"New value:" + event.getNewValue());
		}
	}

You add it via

	<h:inputText value="#{PersonBean.firstname}">
		<f:valueChangeListener type="demo.jsf.DemoValueChangeListener" />
	</h:inputText>

Of course these events are only fired if the value of the component has really changed that means that converting and validating the data was successful.

# UI Components

There are *Components* and *Renderer*. Components describes the pure UI functionality, whereas the Renderer are for formatting the output.

While a component can render some of its output, its conceptually better to differentiate between the two concerns.

For example

the `UIOutput` component is used by the javax.faces.Text renderer. By convention the tag is called `<h:outputText>`.

## IDs

Each component is accessible with an `id`. The tag `<h:inputText id="myInput">` has the `id` `myInput`. This id is unique but only within the _NamingContainer_. For the HTML page JSF creates a _ClientID_ which is unique within the scope of the HTML page. The Client is formed by concatenating all IDs within the naming container and using an `: to delimit them`, for example:

	<h:form id="myForm">
		<h:inputText id="myInputField" />
		<h:CommandButton id="myButton" value="Send" action="#{ComponentNaviController.navigateInView}" />
	</h:form>

The button would have the ID myForm:myButton within the HTML page, given that there aren't any more NamingContainer involved.

If a component doesn't have an ID, an ID will automatically be generated by calling `UIViewRoot.createUniqueId()`.

## ComponentTree

JSF also stores a ComponentTree on the server side. TheComponentTree will be transformed to HTML Output with the Renderer. The tree stores more information that you can display our output in HTML (you can't have `ValueChangeListener` or Converter or Validators).

	View
	 - Form
	    - Input
	    - Button
	 - Form
	    - Input
	    - Checkbox
	    - Button 

The first element is always of type `UIViewRoot`. Mostly you don't have to care about the component tree. Sometimes you have to that - normally in an action method:

	FacesContext jsfContext = FacesContext.getCurrentInstance();
	UIViewRoot viewRoot = jsfContext.getViewRoot();

	UIComponent formComponent = viewRoot.findComponent("calculator");
	UIComponent component = formComponent.findComponent("summand1");

Of course you can search for the `summand1` element directly from the `viewRoot`, but then you have to use the full client id `calculator:summand1`.

# Appendix

## FAQ/Problems

### java.lang.RuntimeException: Cannot find FacesContext

You are calling a page that uses JSF components/tags but isn't served through a `FacesServlet`

### Calling a page results in an empty page

This page will result in an empty page without an error message:

	<%@ taglib uri="http://java.sun.com/jsf/html" prefix="h"%>
	<%@ taglib uri="http://java.sun.com/jsf/core" prefix="f"%>
	<html>
	<head>
	<meta http-equiv="content-type" content="text/html; charset=iso-8859-1">
	<title>Calculator sample</title>
	</head>
	<f:view>
		<h:form>
		Summand 1:
		<h:inputText value="#{CalculatorBean.summand1}" />	
		Summand 2:
		<h:inputText value="#{CalculatorBean.summand2}" />	
		Sum is: <h:outputText value="#{CalculatorBean.sum}" />
		<h:commandButton action="#{CalculatorBean.calculate}"
				value="Calculate the sum" />
		</h:form>
	</f:view>
	<h:messages />
	</html>
	
The error was using `<h:messages />` out of the `<f:view>` scope.