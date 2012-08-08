# Java Web Services



## Web Services



A Web service is a set of related application functions that can be  invoked over the Internet. 



- Web services are *self-contained*. On the client side, no additional software is required.  A programming language with XML and HTTP client support is enough to get you started.  On the server side, a Web server and servlet engine are required.  The client and server can be implemented in different environments.



- Web services are *self-describing*. The client and server need to recognize only the format and content of request and response messages.  The definition of the message format travels with the message; no external metadata repositories or code generation tools are required.



- Web services are *modular*. Simple Web services can be aggregated to form more complex Web services either by using workflow techniques or by calling lower layer Web services from a web service implementation.



- Web services are *platform independent*. Web services are based on a concise set of open, XML-based standards designed to promote interoperability between a Web service and clients across a variety of computing platforms and programming languages.



Web services might be anything, for example, theatre review articles, weather reports, credit checks, stock quotations, travel advisories, or airline travel reservation processes. Each of these self-contained business services is an application that can easily integrate with other services, from the same or different companies, to create a complete business process. This interoperability allows businesses to dynamically publish, discover, and bind a range of Web services through the Internet



A network component in a Web Services architecture can play one or more fundamental roles: service provider, service broker, and service client.



* Service providers create and deploy their Web services and can publish the availability of their WSDL-described services through a service registry.

* Service brokers register and categorize published services and provide search services. 

* Service clients use broker services to discover a needed WSDL-described service and then bind to and call the service provider.



### Creating Web Services



Web services can be created using two methods:

1. _bottom-up_ development (or _Code first_)

2. _top-down_ development (or _WSDL first_)



Although bottom-up Web service development may be faster and easier, especially if you are new to Web services, the top-down approach is the recommended way of creating a Web service.



#### Bottom Up



When creating a Web service using a bottom-up approach, first you create a Java bean or EJB bean and then use the Web services wizard to create the WSDL file and Web service.



#### Top Down



When creating a Web service using a top-down approach, first you design the implementation of the Web service by creating a WSDL file. You can do this using the WSDL Editor. You can then use the Web services wizard to create the Web service and skeleton Java classes to which you can add the required code.



By creating the WSDL file first you will ultimately have more control over the Web service, and can eliminate interoperability issues that may arise when creating a Web service using the bottom-up method.



## JAX-WS 



The Java API for XML Web Services helps building web services that communicate via XML.



In JAX-WS, a RPC is represented by an XML based protocol such as SOAP. SOAP defines structure and rules for the RPCs. 



JAX-WS hides the complexity of SOAP. On the server side the developer specifies the remote procedures by defining methods in a Java Interface (and writing at least one class implementing it).



### Creating a JAX-WS Top-Down/WSDL first



- Create Java artifacts from a WSDL file using `wsimport`

- Implement the Java-Bean that includes your Business logic.

- Assemble WAR file.



## Appendix



### JAX-WS Annotations



#### Service Endpoint



Annotating a class with `javax.jws.WebService` defines it as a web service end point. You may explicitly implement the `endPointInterface` but normally you can depend on the implicit implementation.



You use the endpoint implementation and the `wsgen` tool to generate the web service artifacts and stubs.



	package com.acme;



	import javax.jws.WebService;



	@WebService()

	public class Hello {

	  private String message = new String("Hello, ");



	  public void Hello() {}



	  @WebMethod()

	  public String sayHello(String name) {

		return message + name + ".";

	  }

	} 



### Sources 

	

[eclipse.ws.overview]:http://help.eclipse.org/help33/index.jsp?topic=/org.eclipse.jst.ws.doc.user/concepts/cws.html