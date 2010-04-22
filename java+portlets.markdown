# Portlets

Portlets are pluggable user interface components that are managed and displayed in a web portal.

## Portal

Portlets are used in the context of a portal website. A portal website can serve as an entry point for specific subject or as a collection of different services. Often these portals offer the possibility to personalize the website. Each user can define its own page, adding elements of infos or services he is interested in. With Web 2.0 technology (AJAX) a portal page can aggregate different services.

- aggregate content
 - uniformed view, user experience
 - single sign on
- personalized experience
 - depends on the portal 
- content management systems
 - some portals integrate simple CMS
- search
 - helps the user to cope with masses of information
- security
 - portals handle multitude of users and roles
 - each user is assigned at least one role

Portlets are fragments within a portal, a collection of portlets is a portal.

The portlets live within the **PortletContainer**. The PortletContainer knows of the life cycle of each portlet and how to deal with requests and responses to and from these portlets.

A portlet is standardized ([JSR-168][jsr-168], [JSR-286][[jsr-286]]), a portal is not. The portal offers single sign on, personalization features or other enterprise services.

[JSR-301][jsr-301] describes the interaction between Portlets and JavaServer Faces

## Portlets

### Window States

The windows state defines the size of the portlet window. Which information is included in which view is up to the developer.

The specification includes three states

1. minimized
2. maximized
3. normal

You can also define custom window states, but it is up top the portal server to use them or not (and if he can interpret them correctly)

### Portlet Mode

Defines the operating state

The specification includes three variants

1. view
2. edit
3. help

The view mode is the default mode. Keep in mind that being in view mode doesn't mean that you can only display information, you can also include web forms or any other kind of data interaction. You can run complete web applications within the view mode.

In the Edit Mode each user can personalize/configure the portlet e.g.. defining his location in a weather portlet.

The Help Mode should be self explanatory. How complete the help mode is is up to the developer.

As with the window states the specification supports custom portlet modes. The custom modes have to be supported be portal server in order to work properly.

Changing the portlet mode is done via the window controls.

A portlet only has to support the View Mode, any other mode is optional. Which modes are supported is define din the descriptor of the portlet.

## Portlet Lifecycle

The lifecycle of a portlet is a little more complet than that of a servlet. 

	Servlet
		init(ServletConfig config) throws ServletException<br />
		service(ServletRequest req, ServletResponse res) throws ServletException, IOException<br />
		void destroy()

This is because on page (which can be rendered throuigh one servlet) can consist of multiple portlets. When clicking/interacting with one portlet, the triggered action has to be processed only by the one portlet.

Therefore the portlet standard defines two phases, the **Render-Phase** and the **Action-Phase**.  

	Portlet  
		init (PortletConfig)
		processAction(ActionRequest, ActionResponse)
		render(RenderRequest, RenderResponse)
		destroy()
		
Similiar to the servlet architecture each Portlet runs `init()` first. At the end of the lifecycle the `destroy()` method is called.

During the lifecycle render and action phase alternate. On the first run only the render action is called, displaying the initial markup.

### init

- is only run once when created
- is called by PortletContainer
- throws `PortletException` - if an exception has occurred that interferes with the portlet's normal operation.
- throws `UnavailableException` - if the portlet cannot perform the initialization at this time.

### render

- creates HTMl Fragment
- is called for each portlet
- to control this phase you can set **Render Parameter** during the __Action  Phase__

### action

- actions can mean anything e.g. sending form data, clicking on a link
- each action is only called for the specific portlet
- after the action phase the render phase is triggered

### destroy

- is called when portlet is removed from PortletContainer
- used for closing resources
- can only be called once

## Portlet Deployment

Every class implementing the Portlet Interface is by definition a portlet. How that Portlet should be deployed was left to the implementation of the Portal Server and is mentioned in the specification.

Normally a `war` (Web archive) is created. The **Portlet-Deployment-Deskriptor** (`portlet.xml`) lives parallel to the `web.xml`. The structure of the `portlet.xml` is defined in [JSR-168][jsr-168]/[JSR-286][jsr-286]

	<portlet>
		<description>HelloWorld description</description>
		<portlet-name>HelloWorld</portlet-name>
		<portlet-class>de.acme.org.HelloWorld</portlet-class>
		<expiration-cache>0</expiration-cache>
		<supports>
			<mime-type>text/html<mime-type>
			<portlet-mode>VIEW</portlet-mode>
		</supports>
		<supported-locale>en</supported-locale>
		<portlet-info>
			<title>Hello World Portlet</title>
			<short-title>Hello</short-title>
			<keywords>Hello,Demo,Example</keywords>
		</portlet-info>
	</portlet>

## Example

Maven

Liferay

Deployment
 
Portlet Tag Library

<portlet:defineObjects/>: This tag must appear to define the following variables in the JSP page: renderRequest, renderResponse, and portletConfig. A JSP using the defineObjects tag may use these variables from scriptlets throughout the page.

<portlet:actionURL/>: The portlet actionURL tag creates a URL that must point to the current portlet and must trigger an action request with the supplied parameter. The parameters may be added to the URL by including the param tag between the actionURL start and end tags:

<portlet:actionURL windowState="maximized" portletMode="edit">
        <portlet:param name="action" value="editStocks">
</portlet:actionURL>
The example creates a URL that brings the portlet into EDIT mode and MAXIMIZED windowState to edit the stock quote list.

## Appendix

### Definitions

#### Portal

The portal is the website in its entirety. It offers access to the content, often with personalization features. A portal is served with a portal server.

#### Portal Page

A Portal Page constitutes one page within the portal. One page can display multiple Portlet Windows. A Portal Page aggregates Portal Windows

#### Portlet Window

Portlet Windows are the frames for displaying the Portlet Instances. The layout of the various windows isn't standardized. A window consists of

- Window Title. Each portlet has meta information about its content and purpose. The title can be defines in the deployment descriptor
- Window Controls. Control the behavior/appearance of the window. It only has indirect control. The portal has the last say.
- Window Fragment. A portlet never fills a complete page but  only a part of the portal page. Portlets are normally only HTMl Fragments

#### Portlet Instance

Is very similar to the idea of a class and an object. Each portlet instance can be configured with its own sets of variables. Portlet instances are not part of the official portlet specification.

[jsr-168]: http://jcp.org/en/jsr/detail?id=168
[jsr-286]: http://jcp.org/en/jsr/detail?id=286
[jsr-301]: http://jcp.org/en/jsr/detail?id=301