Provided by the `alaiojs` package, are the two objects `API` and `JsonRPC`. An explanation of their parameters and utilization is provided below.

## JsonRpc
The `JsonRpc` object takes the node you wish to connect to in the form of a string as a required constructor argument, as well as an optional `fetch` object (see [CommonJS](01_commonjs.md) for an example).  

Note that reading blockchain state requires only an instance of `JsonRpc` connected to a node and not the `Api` object.

## Api
To send transactions and trigger actions on the blockchain, you must have an instance of `Api`. This `Api` instance is required to receive a SignatureProvider object in it's constructor.

The SignatureProvider object must contain the private keys corresponding to the actors and permission requirements of the actions being executed.

## JsSignatureProvider

A SignatureProvider is required for the API constructor. SignatureProviders enable the `dist/alaiojs-api-interfaces.SignatureProvider` interface. For the sole purpose of development, a `JsSignatureProvider` object is also provided via the `dist/alaiojs-jssig` import to stand-in for an easy option for a signature provider during development, but only should be used in the development environment since it isn't secure.

It is highly suggested that you use a secure vault outside of the context of the webpage (which will also enable the `alaiojs-api-interfaces.SignatureProvider` interface) to guarantee security when transactions are being signed.
