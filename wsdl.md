# WSDL



Web Services Description Language is an XML format used to describe (and locate) web services.



## Syntax



A WSDL document is just a simple XML document. 



	<definitions>

		<types>

		  definition of types........

		</types>

		<message>

		  definition of a message....

		</message>

		<portType>

		  definition of a port.......

		</portType>

		<binding>

		  definition of a binding....

		</biding>

	</definitions>



For example:	

	

	<message name="getTermRequest">

		<part name="term" type="xs:string"/>

	</message>



	<message name="getTermResponse">

		<part name="value" type="xs:string"/>

	</message>



	<portType name="glossaryTerms">

		<operation name="getTerm">

			<input message="getTermRequest"/>

			<output message="getTermResponse"/>

		</operation>

	</portType>

	

### types



Defines the data types that are used by the web service. WSDL uses XML Schema syntax to define data types.



### message



Defines the data elements of an operation. Each message can consist of one or more parts. The parts are much like a parameter in a function call of a traditional programming language.



## Ports



Is the most important element. It describes a web service, the operations and the messages. It can be compared to a class of a traditional programming language. Each operation can be compared to function.



There are four types of operations:

- One-way: The operation can receive a message but will not return a response

- Request-response:	The operation can receive a request and will return a response

- Solicit-response: The operation can send a request and will wait for a response

- Notification: The operation can send a message but will not wait for a response



### One way



	<message name="newTermValues">

	  <part name="term" type="xs:string"/>

	  <part name="value" type="xs:string"/>

	</message>



	<portType name="glossaryTerms">

	  <operation name="setTerm">

		<input name="newTerm" message="newTermValues"/>

	  </operation>

	</portType>



### Request-response



The `getTerm` operation requires an input message called `getTermRequest` with a parameter called `term`. It will return an output message `getTermResponse` with a parameter called `value`.



	<message name="getTermRequest">

	  <part name="term" type="xs:string"/>

	</message>



	<message name="getTermResponse">

	  <part name="value" type="xs:string"/>

	</message>



	<portType name="glossaryTerms">

	  <operation name="getTerm">

		<input message="getTermRequest"/>

		<output message="getTermResponse"/>

	  </operation>

	</portType>



### Bindings



Defines the message format and protocol details for each port.



	<binding type="glossaryTerms" name="b1">

	   <soap:binding style="document"

	   transport="http://schemas.xmlsoap.org/soap/http" />

	   <operation>

		 <soap:operation soapAction="http://example.com/getTerm"/>

		 <input><soap:body use="literal"/></input>

		 <output><soap:body use="literal"/></output>

	  </operation>

	</binding>



The `binding`'s name can be arbitrary. The type must map to a `portType`.

The `soap:binding`'s element defines the transport layer and the type of document. The `operation` defines the operation. You have to specify the action, input and output.

	

Source http://www.w3schools.com/wsdl/

