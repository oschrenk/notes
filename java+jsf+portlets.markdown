# Bridging JSF and Portlets

*JSF-Portlet Bridges* are solutions to connect JSF and Portlet technology. The goal is to run a JSF application in a PortletContainer - ideally without making changes to the JSF application.

It seems easy as first: JSF can use JSP as a View technology and you can build portlets with JSP. But there are problems. For instance the life cycles: JSF is subdivided into six phases, the portlet in two. How can we map the JSF lifecycle to the portlet lifecycle?

The standard JSR 301 bridges JSF 1.2 and Portlet 1.0 and was finalized 2009. The JSR 329 defines the bridge between JSF 1.2 and Portlet 2.0 and is still in progress (as of April 2010).

## Architecture

Normally a JSF application works like this

1. HTTP GET/POST request
2. FacesServlet (as front controller)
3. Life cycle of JSF
4. Forward to JSF page

The FacesServlet is responsible for building the FacesContext

In a portal container there is no special JSF page for the application. The request is first handled by the portal engine, which in turn forwards it to the portlet container. The portlet container is responsible for the lifecycle of the portlets.

The bridge is the connection between Portlet and JSF environment and works in a way much like the FacesServlet.

For the application is is transparent from where the request came.

## Bridge Interface 

	init (PortletConfig)
	destroy()
	doFacesRequest(ActionRequest, ActionResponse)
	doFacesRequest(RenderRequest, RenderRepsonse)

Usually each portlet has its own instance of a bridge.

### init

Can take various parameter; to be set in the `web.xml`

- `javax.portlet.faces.MAX_MANAGED_REQUEST_SCOPES` How many bridge request scopes can be managed.
- `javax.portlet.faces.RENDER_POLICY` Manage if rendering should be done by a different view handler. Possible values are: `ALWAYS_DELEGATE`, `NEVER_DELEGATE`, `DEFAULT`
- `javax.portlet.faces.LIFECYCLE_ID` Used to define another life cycle (that of the bridge). Defaults to `LifecycleFactory.DEFAULT_LIFECYCLE`

In addition to these parameters you can also set attributes in the portlet context. But as the portlet context is in effect for all portlets, parameters are defined using the name of the portlet

- `javax.portlet.faces.[portletname].excludedRequestAttributes` Some request attributes of the bridge managed scope shouldn't be persisted.
- `javax.portlet.faces.[portletname].preserveActionParams` Boolean. Decides if action parameters are stored for the duration of bridge request scope. If set to `false` thwe action parameters are only preserved int the action request but not in the render request.
- `javax.portlet.faces.[portletname].defaultViewIdMap` Stores a map of each portlet mode to the according JSF page.

### Request Handling

Two methods

	public void doFacesRequest(ActionRequest request, ActionResponse response) 
		throws BridgeDefaultViewNotSpecifiedException, BridgeUninitializedException, BridgeException; 
		public void doFacesRequest(RenderRequest request, RenderResponse response) 
			throws BridgeDefaultViewNotSpecifiedException, BridgeUninitializedException, BridgeException;
			
Similar to the lifecycle of a portlet.

### Destroy

Close resources and stuff.

## GenericFacesPortlet

The bridge itself isn't a portlet. But we something that can live in a portlet container - `GenericFacesPortlet`. Comes with the specification.

The most important task of the GenericFacesPortlet is the instantiation of the bridge.

## Combination of Life cycles

The main problem of bringing the technologies together is bringing the two different life cycles together. As we are searching for a solution that could life within the portlet container, the portlet life cycle will be the defining one. 

JSF doesn't have an explicit phase for initializing and destroying within the context. 

During the rendering phase of the portlet, the render phase of JSF is also called (after the Restore View phase of course)

	Portlet Render Request > JSF Restore View > JSF Render Response > Portlet Render Response 

During the action phase of the portlet JSF phases 1-5 are called

	Portlet Action Request > Restore View > Apply Request Values > Process Validations > Update Model Values > Invoke Application > Portlet Action Response
	
The JSR Bridge is responsible for dividing to call the appropriate JSF phases when portlet render requests or action requests are coming in.

## Bridge Request Scope

Splitting the JSF Lifecycle in two parts doesn't come without problems. As JSG uses request attributes (which are added to the request) to add information such as error messages. These will´be lost as the  there are now two request to be made. Error message that occur during the first request will be lost for the second one.

The solution is that the JSR 301 bridge save the state of the JSF application. If the portlet phase changes (from action request to render request) the bridge has to recover the request attributes. This is called the _Bridge Request Scope_.

For each action request a new bridge request scope will be created. If the action request is done the state will be saved in the bridge managed scope.

If a render request comes in the applicable bridge managed scope is to be found.

### Exclusion of Attributes ###

The bridge buffers request attributes. Sometimes this behaviour isn't needed or is even harmful. You can exclude attributes

- using `javax.portlet.faces.annotation.ExcludeFromManagedRequestScope` annotation
- using `javax.portlet.faces.[portletname].excludedRequestAttributes` attribute
- using `faces-config.xml` 

	<faces-config version="1.2" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:bridge="http://www.apache.org/myfaces/xml/ns/bridge/bridge-extension">
	
	<application>
		<application-extension>
			<bridge:excluded-attributes>
				<bridge:excluded-attribute>
					myComponent.myRequestAttribute
				</bridge:excluded-attribute>
			</bridge:excluded-attributes>
		</application-extension>
	</application>
	
# Appendix

## Terminology

- _Request_: request from browser to server
- _Response_: answer from server to client
- _Portlet Request_: the portal requests the portal container. Two types: _ActionRequest_ and _RenderRequest_
- _Portlet Response_: complement to the Portlet Request. Answer depends on type.
- _Request Scope_: the validity period of a request
- _Servlet Request Scope_: validity period of a request being consumed by servlet container
- _Portlet Request Scope_: Difference between ActionRequest and Render Request. One client request can generate two server side requests.
- _Faces Request Scope_: validity period within JSF. As JSF was developed for the servlet world each JSF life cycle is handled in one request.
- _Bridge Request Scope/Managed Request Scope_: validity period managed by the bridge. Can be longer than portal request, to ensure that data from the action requestt are still available to the render requests.