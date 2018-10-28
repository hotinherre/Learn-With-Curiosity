# Tech talk I watch among Oct to Dec in 2018


## API Throwndown: RPC vs Rest Vs GraphQL

Watch date: 10.26 - 10.28

### RPC

Calling procedure in another computer/server. It is coded as if it were a normal local procedure call.
It is a old school approach, begins in 1970s. But it is still highly adopted in modern microservice architecture.

Advantages:  

1. simple and easy
2. lightweight payloads
3. high performance

Disadvantage:

1. Tight coupling
2. No discoverability, need documentation.
3. Function explosion, when people create mutiple version of same procedure.

cutting edge of RPC:

1. Apache Thrift
2. gRPC(Google)
3. Twirp

### REST

Provide abstraction for resource. Most popular API style.

Pros:
1. reuse HTTP
2. decouple client with backend, easy evolve.

Cons:
1. Big playload.(Plain text headers)
2. No standard spec to build rest. Different engineer has differnt opinions.

### GraphQL

> Ask for exactly what you want.

With SQL like query API, Client only get data they need.  
Bundle mutiple RESTful API in one GraphQL query, save network overhead.

Pros:
1. Low network overhead
2. Typed schema
3. fits graph-like data pretty well

Cons:
1. Trade off more power with more complexity
2. Complex to caching.
3. still early stage

### Use Case

Consider: REST
- focus on object/resource management
- have varied kinds of clients
- need good discoverability and documents

Consider: RPC
- Action oriented
- Simple interactions
- lightweight overhead

Consider: GraphQL
- Complicated Graph-Like Data