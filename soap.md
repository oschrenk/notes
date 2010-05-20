# SOAP

Simple Object Access Protocol (SOAP) is a XML-based communication protocol for exchanging information between two applications over HTTP. It is a protocol for accessing a Web Service.

It doesn't rely on a specific tranport layer. In practice mainly HTTP is used.

## Syntax 

	<?xml version="1.0"?>
	<soap:Envelope
	xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
	soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">
	<soap:Header>
	...
	</soap:Header>
	<soap:Body>
	...
	  <soap:Fault>
	  ...
	  </soap:Fault>
	</soap:Body>
	</soap:Envelope>
	
The `envelope` is the root element of the SOAP message. SOAP defines three attributes in the default namespace (`http://www.w3.org/2001/12/soap-envelope`). These attributes are: `mustUnderstand`, `actor`, and `encodingStyle`.

The optional header element describes application specific information about the message, for instance authentification. If it is ued, it must be the first element. Each (immediate) child element must be namespace qualified.

	<soap:Header>
	  <m:Trans xmlns:m="http://www.w3schools.com/transaction/"
	  soap:mustUnderstand="1">234
	  </m:Trans>
	</soap:Header>
	
### Header
	
#### mustUnderstand

The `mustUnderstand` attribute indicates whether an elemnt is mandatory to be processes or not.

	soap:mustUnderstand="0|1"
	
#### actor

The idea is that a SOAP message can pass multiple stations along the way and may be processed by them. Each station can be identified using an URI

	soap:actor="URI"
	
Sometimes these parts aren't intended to be read globally but only by some specific station. The station has to remove the conent before forwarding it along the path.

	<soap:Header>
	  <m:Trans xmlns:m="http://www.w3schools.com/transaction/"
	  soap:actor="http://www.w3schools.com/appml/">234
	  </m:Trans>
	</soap:Header>
	
#### encodingStyle

Is used to define the data types used in the document. 

	soap:encodingStyle="URI"
	
### Body

Required. Contains the message. Immediate children may be namespace qualified.

	<soap:Body>
	  <m:GetPrice xmlns:m="http://www.w3schools.com/prices">
		<m:Item>Apples</m:Item>
	  </m:GetPrice>
	</soap:Body>

`GetPrice` is application specific.
	
A response to the above rquest could contain

	<soap:Body>
	  <m:GetPriceResponse xmlns:m="http://www.w3schools.com/prices">
		<m:Price>1.90</m:Price>
	  </m:GetPriceResponse>
	</soap:Body>

### Faults

Optional. Muste be child of body. Can only appear once. Indicates error.

Example from [here](http://www.herongyang.com/Web-Services/SOAP-Fault-Message-Structure.html):

	<?xml version='1.0' ?>
	<env:Envelope xmlns:env="http://www.w3.org/2003/05/soap-envelope"
				xmlns:rpc='http://www.w3.org/2003/05/soap-rpc'>
	  <env:Body>
	   <env:Fault>
		 <env:Code>
		   <env:Value>env:Sender</env:Value>
		   <env:Subcode>
			<env:Value>rpc:BadArguments</env:Value>
		   </env:Subcode>
		 </env:Code>
		 <env:Reason>
		  <env:Text xml:lang="en-US">Processing error</env:Text>
		  <env:Text xml:lang="fr-CA">Error</env:Text>
		 </env:Reason>
		 <env:Detail>
		  <e:myFaultDetails 
			xmlns:e="http://travelcompany.example.org/faults">
			<e:message>Name does not match card number</e:message>
			<e:errorcode>999</e:errorcode>
		  </e:myFaultDetails>
		 </env:Detail>
	   </env:Fault>
	 </env:Body>
	</env:Envelope>




### HTTP

A SOAP request could be an HTTP POST or an HTTP GET request. 

	POST /item HTTP/1.1
	Content-Type: application/soap+xml; charset=utf-8
	Content-Length: 250
	
## Testing

[SoapUI][soapui] is a testing tool for SOA.

[soapui]: http://www.soapui.org/
