# Spring Framework

- Framwork to address complexity of JavaEE development
- Offers multiple modules/components

## Core Container

- Dependency Injection Framework.
	- Type1: Interface Injection
	- Type2: Setter Injection
	- Type3: Constructor Injection

Dependeny Injection is a form IoC (Inversion of COntrol) that removes explicit dependence on container API. Java methods are used to inject dependencies	.

- application code is easier to test. Beans have simple, core Java properties and method whhich are easy to test.
- explicit dependencies. All objects needed for execution of a method are known without reading the code.
	
`BeanFactory` to create objects, two modes
- Singleton. There is one shared instance of the object, which will be retrieved on lookup. It is often used for stateless service objects.

- Prototype. Each retrieval will result in creation of an independent object.

The `org.springframeworek.beans.factory.BeanFactory` is a simple interface. The most common use `BeanFactory` implementation is probably	
	
		BeanFactory factory = new XMLBeanFactory(new FileInputSteam("mybean.xml"));

All beans defined in XML are lazily loaded that means that they will only be instantiated when they are needed.

Each bean definition can be a POJO or a `FactoryBean`	

 

## Spring Context

## Spring AOP

## Spring DAO

## Spring ORM

## Spring Web Module
## Spring MVC framework
