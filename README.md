# PyIntegration
PyIntegration is an enterprise integration framework that provides an out-of-the-box solution for the patterns in the now-classic Enterprise Integration Patterns book.
## Background
PyIntegration extends [Pykka](https://www.pykka.org/ "Pykka")  into the enterprise domain where Pykka offers the message-driven model and PyIntegration provides the inversion of control for runtime concerns, such as when certain business logic should execute and where the response should be sent. It supports routing and transformation of messages so that different transports and different data formats can be integrated without impacting testability. In other words, the messaging and integration concerns are handled by the framework, so business components are further isolated from the infrastructure and developers are relieved of complex integration responsibilities.

PyIntegration's design is inspired by the well-known Enterprise Integration Patterns as described in the book of the same name by Gregor Hohpe and Bobby Woolf (Addison Wesley, 2004). Developers who have read that book should be immediately comfortable with the PyIntegration concepts and terminology.
## Goals and Principles
PyIntegration is motivated by the following goals:
*   Provide a simple model for implementing complex enterprise integration solutions.
*   Facilitate asynchronous and message-driven behavior.

PyIntegration is guided by the following principles:
*	Components should be loosely coupled for modularity and testability.
*	The framework should enforce separation of concerns between business logic and integration logic.
*	Extension points should be abstract in nature but within well-defined boundaries to promote reuse and portability.

## Main Components
From the vertical perspective, a layered architecture facilitates separation of concerns, and interface-based contracts between layers promote loose coupling. Message-driven architectures add a horizontal perspective, yet these same goals are still relevant. Just as "layered architecture" is an extremely generic and abstract paradigm, messaging systems typically follow the similarly abstract "pipes-and-filters" model. The "filters" represent any component that is capable of producing and/or consuming messages, and the "pipes" transport the messages between filters so that the components themselves remain loosely-coupled. It is important to note that these two high-level paradigms are not mutually exclusive. The underlying messaging infrastructure that supports the "pipes" should still be encapsulated in a layer whose contracts are defined as interfaces. Likewise, the "filters" themselves would typically be managed within a layer that is logically above the application’s service layer, interacting with those services through interfaces much in the same way that a web-tier would.
### Message
In PyIntegration, a Message is a generic wrapper for any Python object combined with metadata used by the framework while handling that object. It consists of a payload and headers. The payload can be of any type and the headers hold commonly required information such as id, timestamp, correlation id, and return address. Headers are also used for passing values to and from connected transports. For example, when creating a Message from a received File, the file name may be stored in a header to be accessed by downstream components. Likewise, if a Message’s content is ultimately going to be sent by an outbound Mail adapter, the various properties (to, from, cc, subject, etc.) may be configured as Message header values by an upstream component. Developers can also store any arbitrary key-value pairs in the headers.
### Message Channel
A Message Channel represents the "pipe" of a pipes-and-filters architecture. Producers send Messages to a channel, and consumers receive Messages from a channel. The Message Channel therefore decouples the messaging components, and also provides a convenient point for interception and monitoring of Messages.

A Message Channel may follow either Point-to-Point or Publish/Subscribe semantics. With a Point-to-Point channel, at most one consumer can receive each Message sent to the channel. Publish/Subscribe channels, on the other hand, will attempt to broadcast each Message to all of its subscribers. PyIntegration supports both of these.

Whereas "Point-to-Point" and "Publish/Subscribe" define the two options for how many consumers will ultimately receive each Message, there is another important consideration: should the channel buffer messages? In PyIntegration, Pollable Channels are capable of buffering Messages within a queue. The advantage of buffering is that it allows for throttling the inbound Messages and thereby prevents overloading a consumer. However, as the name suggests, this also adds some complexity, since a consumer can only receive the Messages from such a channel if a poller is configured. On the other hand, a consumer connected to a Subscribable Channel is simply Message-driven.


