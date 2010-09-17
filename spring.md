# Spring Framework

- Framwork to address complexity of JavaEE development
- Offers multiple modules/components

## Core Container

- Dependency Injection Framework.
	- Type1: Interface Injection
	- Type2: Setter Injection
	- Type3: Constructor Injection

`BeanFactory` to create objects, two modes
- Singleton
- Prototype

		BeanFactory factory = new XMLBeanFactory(new FileInputSteam("mybean.xml"));

All beans defined in XML are lazily loaded that means that they will only be instantiated when they are needed.
	
## Spring Context

## Spring AOP

## Spring DAO

## Spring ORM

## Spring Web Module

## Spring MVC framework
