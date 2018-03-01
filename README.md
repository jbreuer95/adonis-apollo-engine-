# Adonis Apollo Engine
Apollo Engine middleware and provider for AdonisJs

## Important!
When Apollo Engine encounters a SIGUSR2 event (The one that nodemon uses to restart) it dumps a stack trace in the console and doesn't shut down.  
To get around this issue you have to tell nodemon to use the SIGUSR1 event. You can do this by adding a nodemon.json like this to the root of your project

```
{
  "signal": "SIGUSR1"
}
```
Be careful because SIGUSR1 is used for node debugging and could introduce some weird behaviour
## Prerequisites
This package assumes you are using [apollo-server-adonis](https://www.npmjs.com/package/apollo-server-adonis)  
It might work with [adonis-apollo-server](https://www.npmjs.com/package/adonis-apollo-server) but I haven't tested it

## Installation

Run `adonis install adonis-apollo-engine`, this will install the dependency and create a default config file.

If you install this package with `npm install` please add a config/apolloEngine.js file with the following content:
```js
'use strict'

module.exports = {
  engineConfig: {
    apiKey: 'APOLLO-ENGINE-API-KEY'
  }
}

```

## Registering provider

Make sure you register the provider inside the `start/app.js` file before making use of the body parser.

```js
const providers = [
  'adonis-apollo-engine/providers/ApolloEngineProvider'
]
```

## Registering middleware

Next step is to register a global middleware which will route the proxy requests. The middleware will be registered inside `start/kernel.js` file.  
Make sure it is the `FIRST` middleware!

```js
const globalMiddleware = [
  'Adonis/Apollo/Middleware',
]
```

That's all you need to do 😎

## Config

The config file `config/apolloEngine.js`  contains the minimum to get started.

See https://github.com/apollographql/apollo-engine-js  
and https://www.apollographql.com/docs/engine/proto-doc.html  
for available options

## Production
Using middleware as a proxy, in general, is fine for small to medium projects.  
For the best performance please run a dedicated docker proxy
[see here](https://www.apollographql.com/docs/engine/setup-node.html#standalone-docker-container)
