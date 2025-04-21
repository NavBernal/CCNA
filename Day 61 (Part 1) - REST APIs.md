# REST APIs (Application Programming Interface)
- A software interface that allows two apps to communicate with each other
- Are essential not just for network automation, but for all kinds of apps
- In SDN architecture, they're used to communicate between apps and the SDN controller (via the NBI), and between the SDN controller and the network devices (via the SBI)
- The NBI typically uses REST APIs
- NETCONF and RESTCONF are popular southbound APIs
### CRUD
- CRUD (Create, Read, Update, Delete) refers to the operations we perform using REST APIs
- **Create** operations are used to create new variables and set their initial values
- **Read** operations are used to retrieve the value of a variable
- **Update** operations are used to change the value of a variable
- **Delete** operations are used to delete variables
- HTTP uses *verbs* (aka *methods*) that map to these CRUD operations
- REST APIs typically use HTTP
### HTTP Verbs
![](attachments/f9b44aaf2ed980eb0a829c565bb310ed.png)
### HTTP Request
- When an HTTP client sends a request to an HTTP server, the HTTP header includes information like this:
	- An HTTP verb (i.e. GET)
	- A URI (Uniform Resource Identifier), indicating the resource it's trying to access
![](attachments/1b125fa3448286d63754b478faa5ea1d.png)
- Here's an example of a URI:
![](attachments/c5695e65549203ac1819aae996940618.png)
- The HTTP request can include additional headers which pass additional information to the server
	- Here's a [list](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
![](attachments/fd8f38a9fd1d9d0477074b9e1ad41e5f.png)
- An example would be an **Accept** header, which informs the server about the type(s) of data that can be sent back to the client
	- i.e. `Accept: application/json` or `Accept: application/xml`
- You can also view standard HTTP header fields with some [examples](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- When a REST client makes an API call (request) to a REST server, it'll send an HTTP request like the one above
	- REST APIs don't *have* to use HTTP for communication, although it's the most common choice
### HTTP Response
- The server's response will include a status code indicating if the request succeeded or failed, as well as other details
- The first digit indicates the class of the response:
	- **1xx** *informational* - the request was received, continuing process
		- **102 Processing** indicates that the server has received the request and is processing it, but the response not yet available
	- **2xx** *successful* - the request was successfully received, understood, and accepted
		- **200 OK** indicates that the request succeeded
		- **201 Created** indicates that the request succeeded, and a new resource was created (i.e. in response to POST)
	- **3xx** *redirection* - further action needs to be taken in order to complete the request
		- **301 Moved Permanently** indicates that the requested resource has been moved, and the server indicates its new location
	- **4xx** *client error* - the request contains bad syntax or cannot be fulfilled
		- **403 Unauthorized** means the client must authenticate to get a response
		- **404 Not Found** means the requested resource was not found
	- **5xx** *server error* - the server failed to fulfill an apparently valid request
		- **500 Internal Server Error** means the server encountered something unexpected that it doesn't know how to handle
### REST
- **REST APIs** are also known as **REST-based APIs** or **RESTful APIs**
	- REST isn't a specific API
	- Instead, it describes a set of rules about how the API should work
- The six constraints of RESTful architecture are:
	- Uniform Interface
	- Client-server
	- Stateless
	- Cacheable or non-cacheable
	- Layered system
	- Code-on-demand (optional)
- For applications to communicate over a network, networking protocols must be used to facilitate those communications
	- For REST APIs, HTTP(S) is the most common choice
### Client-Server
- The client uses API calls (HTTP requests) to access the resources on the server
- The separation between the client and server means they can both change and evolve independently of each other
	- When the client application changes or the server application changes, the interface between them must not break
### Stateless
- Each API exchange is a separate event, independent of all past exchanges between the client and server
	- The server doesn't store information about previous requests from the client to determine how it should respond to new requests
- If authentication is required, this means that the client must authenticate with the server for each request it makes
- TCP is an example of a stateful protocol
- UDP is an example of a stateless protocol
- Although REST APIs use HTTP, which uses TCP (stateful) as its Layer 4 protocol, HTTP and REST APIs themselves aren't stateful
	- The functions of each layer are separate!
### Cacheable or Non-Cacheable
- *Caching* refers to storing data for future use
	- For example, your computer might cache many elements of a web page so that it doesn't have to retrieve the entire page every time you visit it
	- This improves performance for the client and reduces the load on the server
- Not all resources have to be cacheable, but cacheable resources MUST be declared as cacheable