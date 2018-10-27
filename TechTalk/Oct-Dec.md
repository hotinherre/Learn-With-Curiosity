# Tech talk I watch among Oct to Dec in 2018


## API Throwndown: RPC vs Rest Vs GraphQL

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

