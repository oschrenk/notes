# Software Architecture #

## Design Architectures ##

- [Martin Fowler on GUI Architectures](http://martinfowler.com/eaaDev/uiArchs.html)

### MVC ###

	         +------------+
	         |   Model    |
	         +------------+
	        /\ .          /\
	        / .            \
	       / .              \
	      / .                \
	     / \/                 \
	+------------+ <------ +------------+
	|    View    |         | Controller |
	+------------+ ......> +------------+

Model–View–Controller (MVC) is a software design for interactive computer user interfaces that separates the representation of information from the user's interaction with it. The model consists of application data and business rules, and the controller mediates input, converting it to commands for the model or view.[3] A view can be any output representation of data, such as a chart or a diagram. Multiple views of the same data are possible, such as a pie chart for management and a tabular view for accountants.

In addition to dividing the application into three kinds of component, the MVC design defines the interactions between them.[4]

- A controller can send commands to its associated view to change the view's presentation of the model (for example, by scrolling through a document). It can send commands to the model to update the model's state (e.g. editing a document).
- A model notifies its associated views and controllers when there has been a change in its state. This notification allows the views to produce updated output, and the controllers to change the available set of commands. A passive implementation of MVC omits these notifications, because the application does not require them or the software platform does not support them.
- A view requests from the model the information that it needs to generate an output representation.
With the responsibilities of each component thus defined, MVC allows different views and controllers to be developed for the same model. It also allows the creation of general-purpose software frameworks to manage the interactions.

Source: [Wikipedia](http://en.wikipedia.org/wiki/Model-view-controller)

### MOVE ##

	           +------------+
	           |   Model    |
	           +------------+
	           /\           \
	           .             \
	          .               \
	         .                 \
	        .                  \/
	+------------+ <------ +------------+
	| Operation  |         |    View    |
	+------------+ ......> +------------+

[Propsoal](http://cirw.in/blog/time-to-move-on) by [Conrad Irwin](http://cirw.in/) 

**MOVE**. **M**odels, **O**perations, **V**iews, and **E**vents.

- Models encapsulate everything that your application knows.
- Operations encapsulate everything that your application does.
- Views mediate between your application and the user.
- Events are used to join all these components together safely.

#### Models ####

The archetypal model is a "user" object. It has at the very least an email address, and probably also a name and a phone number.

In a MOVE application models only wrap knowledge. That means that, in addition to getters and setters, they might contain functions that let you check "is this the user's password?", but they don't contain functions that let you save them to a database or upload them to an external API. That would be the job of an operation.

#### Operations ####

A common operation for applications is logging a user in. It's actually two sub-operations composed together: first get the email address and password from the user, second load the "user" model from the database and check whether the password matches.

Operations are the doers of the MOVE world. They are responsible for making changes to your models, for showing the right views at the right time, and for responding to events triggered by user interactions. In a well factored application, each sub-operation can be run independently of its parent; which is why in the diagram events flow upwards, and changes are pushed downwards.

What's exciting about using operations in this way is that your entire application can itself be treated as an operation that starts when the program boots. It spawns as many sub-operations as it needs, where each concurrently existing sub-operation is run in parallel, and exits the program when they are all complete.

#### Views ####

The login screen is a view which is responsible for showing a few text boxes to the user. When the user clicks the "login" button the view will yield a "loginAttempt" event which contains the username and password that the user typed.

Everything the user can see or interact with should be powered by a view. They not only display the state of your application in an understandable way, but also simplify the stream of incoming user interactions into meaningful events. Importantly views don't change models directly, they simply emit events to operations, and wait for changes by listening to events emitted by the models.

#### Events ####

The "loginAttempt" event is emitted by the view when the user clicks login. Additionally, when the login operation completes, the "currentUser" model will emit an event to notify your application that it has changed.

Listening on events is what gives MOVE (and MVC) the inversion of control that you need to allow models to update views without the models being directly aware of which views they are updating. This is a powerful abstraction technique, allowing components to be coupled together without interfering with each other.

## Messaging ##

### Message Queue ###

Also known as _point-to-point messaging_

Messages are sent to a queue. Normally they will be persisted for later consumption.

The queue can have many consumers, but a particular message message will only be consumed by one. Senders (also known as producers) are completely decoupled from the receivers (also known as consumers).

The consumer consumes the message and acknowledges the message. Once acknowledged the message is removed from the queue. If the system crashes before it receives the acknowledgment the message will still be available and will be delivered again.

[Messaging concepts][#jboss.messaging]

### Publisher/Subscriber ###

_Publish/subscribe_ (or _pub/sub_) is a messaging pattern in which the senders are sending their messages to an entity on the server, often called a topic or channel. 

A topic can be subscribed to by different consumers. Each subscriber receives a copy of each message sent to the topic. This is different from the message queue pattern where each message is only consumed by a single consumer.

An example of publish-subscribe messaging would be a news feed. As news articles are created by different editors around the world they are sent to a news feed topic. There are many subscribers around the world who are interested in receiving news items - each one creates a subscription and the messaging system ensures that a copy of each news message is delivered to each subscription.

[Messaging concepts][#jboss.messaging]

## Enterprise Service Bus ##

A bus is a system that forwards data to other parts. An Enterprise Service Bus (ESB) is an architectural concept for distributed systems following SOA.

## Resources ##

[#jboss.messaging]: [Messaging concepts](http://docs.jboss.org/jbossmessaging/docs/usermanual-2.0.0.beta1/html/messaging-concepts.html) 