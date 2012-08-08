# SOA



Today we are face with the challenge to implement functionality and business process spanning multiple systems. In the past complexity was battled with (forced) harmonization or meticulous planning. Both aren't options today - businesses face quick changes.



SOA (Service Oriented Architecture) is a new approach. There are three key concepts:

1. A *service* capsules implementation details in a way that it exposes itself as functional, self-contained entity.

2. The concept of *interoperability* ensure to connect two systems, but not through harmonization but with respect to the heterogeneity. 

3. *Loose coupling* means to build systems without creating too big dependencies, so that changes or failures don't cripple the system landscape. There are different kinds: separation of data models, asynchronous communication via message queues and other.



SOA isn't a recipe with strict rules. It's a collection of ingredients, you can and have to combine yourself to achieve your goals. SOA is a strategy and can be found in different stages.



1. Business driven, self contained interfaces, allowing to capsule implementation details.

2. The second stage is to realize functions using these services. This can be done with an Enterprise Service Bus, short ESB.

3. On the third stage you model complete business processes with these services (Business Process Management, short BPM).

4. The fourth stage is the enterprise architecture management (EAM), trying to plan and manage the mapping of services to systems.



Whether the last two points are really part of SOA is debatable.



## Guideline



In short SOA should serve as a base for building business processes covering multiple systems.



> Service orientation is a paradigm that frames what you do.

> Service-oriented architecture (SOA) is a type of architecture that results from applying service orientation.



The implementation is not a concrete architecture, it's an architectural style.



> We have been applying service orientation to help organizations consistently deliver sustainable business value, with increased agility and cost effectiveness, in line with changing business needs.



Applying SOA is only valuable, if your business directly benefits from it and doesn't serve as an end in itself.



> Business value over technical strategy



SOA should add value. The only thing that matters, if what you do not only makes sense, but also adds benefit and is appropriate.



> Strategic goals over project-specific benefits



If in doubt, strategic goals are more important that project specific gains. You should aim for sustainability, from which many if not all projects can benefit.



> Intrinsic interoperability over custom integration



Interoperability must be a fundamental part of you SOA strategy. It should be easy and self-evident to use services somewhere else.



> Shared services over specific-purpose implementations



Solutions should follow the principle of reusability. Bu beware: services that can do everything and are therefore "optimal" solutions are often counter productive, unwieldy, complex and slow. Shared services can only be used services.



> Flexibility over optimization



Both are important, but when in doubt choose flexibility over optimization. But keep in mind that too much flexibility means complexity and that complex systems don't add business value.



> Evolutionary refinement over pursuit of initial perfection



Realize that you can't achieve perfection in bigger systems, but strive for it. Try to approach it step by step. Requirements change and with each step you will learn what is wanted, needed and achievable. The same principle can be applied to your SOA landscape. Don't even try to make everything perfect from the beginning, apply, learn and correct.



## Guiding Principles



> Respect the social and power structure of the organization.



Accept that, depending on the existing structures, SOA implementations will differ. 



> Recognize that SOA ultimately demands change on many levels.



The impact of applying SOA is bigger than you will expect. It's not only a technology. It changes processes, business culture, power structures and the personal attitude.



> The scope of SOA adoption can vary. Keep efforts manageable and within meaningful boundaries.



> Products and standards alone will neither give you SOA nor apply the service orientation paradigm for you.



You can't _buy_ SOA. SOA can't be enforced by just using standards. Processes and structures have to change.



> SOA can be realized through a variety of technologies and standards.



There isn't one truth or solution. You can use Web Services or Rest, you can use synchronous or asynchronous services. In fact be prepared to use all: the real world is heterogeneous.



> Establish a uniform set of enterprise standards and policies based on industry, de facto, and community standards.



In short create you own set of standards depending on your needs, but try to use existing standards.

> Pursue uniformity on the outside while allowing diversity on the inside.



Accept that the world is heterogeneous. The usual solution to solve problems by enforcing harmony doesn't work. But only with harmony synergy effects are usable. Find the right balance.



> Identify services through collaboration with business and technology stakeholders.



> Separate the different aspects of a system that change at different rates.



Separation of concerns.



> Maximize service usage by considering the current and future scope of utilization.

> Evolve services and their organization in response to real use.



The sentences seems to contradict. How can you consider the future scope if you should build on past experiences. Find the right mix.



> Verify that services satisfy business requirements and goals.



> Reduce implicit dependencies and publish all external dependencies to increase robustness and reduce the impact of change.



Loose coupling.



> At every level of abstraction, organize each service around a cohesive and manageable unit of functionality.



Important is the possibility to create an overview of all the services on different levels.



# Authors



- Ali Arsanjani

- Grady Booch

- Toufic Boubez

- Paul C. Brown

- David Chappell

- John deVadoss 

- Thomas Erl

- Nicolai Josuttis

- Dirk Krafzig

- Mark Little

- Brian Loesgen

- Anne Thomas Manes 

- Joe McKendrick

- Steve Ross-Talbot

- Stefan Tilkov

- Clemens Utschig-Utschig