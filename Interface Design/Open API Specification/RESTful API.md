# 0 Introduction
[Roy Thomas Fielding](https://en.wikipedia.org/wiki/Roy_Fielding) set up his principle of an architectural design of network-based application software and named it as Representational State Transfer (REST).
> *The following context quotes from https://restfulapi.net.*

# 1 Guiding principles
## 1.1 Client–server
By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.
## 1.2 Stateless
Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.
## 1.3 Cacheable
Cache constraints require that the data within a response to a request be implicitly or explicitly labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later, equivalent requests.
## 1.4 Uniform interface
By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. In order to obtain a uniform interface, multiple architectural constraints are needed to guide the behavior of components. REST is defined by four interface constraints: identification of resources; manipulation of resources through representations; self-descriptive messages; and, hypermedia as the engine of application state.
## 1.5 Layered system
The layered system style allows an architecture to be composed of hierarchical layers by constraining component behavior such that each component cannot “see” beyond the immediate layer with which they are interacting.
## 1.6 Code on demand (optional)
REST allows client functionality to be extended by downloading and executing code in the form of applets or scripts. This simplifies clients by reducing the number of features required to be pre-implemented.

# 2 Key Concept
## 2.1 Resource
The key abstraction of information in REST is a resource. REST uses a resource identifier (URI) to identify the particular resource involved in an interaction between components.
The state of resource at any particular timestamp is known as **resource representation**. A representation consists of data, metadata describing the data and hypermedia links which can help the clients in transition to next desired state.
The data format of a representation is known as a **media type**. The media type identifies a specification that defines how a representation is to be processed – normally one media-type associated with one resource. A truly RESTful API looks like hypertext.