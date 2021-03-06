[![Build Status](https://travis-ci.org/teads/hapiour.svg?branch=master)](https://travis-ci.org/teads/hapiour)
[![npm version](https://img.shields.io/npm/v/hapiour.svg)](https://www.npmjs.com/package/hapiour)
[![dependencies](https://david-dm.org/teads/hapiour.svg)](https://david-dm.org/teads/hapiour)

# hapiour

Typescript decorators for Hapi

# Install

```bash
npm install --save hapi
npm install --save hapiour
```

# Example

## Files
```
src/
  -> main.ts
  -> app.ts
  -> beer.module.ts
```

## Declare your app
### src/app.ts
```js
import {Server} from 'hapi'
import {App, IApp, Modules} from 'hapiour'
import {Beer} from './beer.module'

@App({
  port: 3000
})
@Modules([Beer])
export class MyApp implements IApp {

  private server: Server

  public constructor(server: Server) {
    this.server = server
  }

  public onInit(): void {
  }

  public onStart(): void {
    console.log('Server running at:', this.server.info.uri)
  }

}
```

## Declare a module
### src/beer.module.ts
```js
import {Route, Modules, Module} from 'hapiour'
import {Request, IReply} from 'hapi'

@Module({
  basePath: '/beer'
})
export class Beer {

  public constructor() {}

  @Route({
    method: 'GET',
    path: '',
    config: {}
  })
  public get(request: Request, reply: IReply) {
    reply({
      'data': 'Hey I\'m alive'
    })
  }

}
```

## Bootstrap your app
### src/main.ts
```js
import {bootstrap} from 'hapiour'
import {MyApp} from './app'

bootstrap(MyApp)
```

## API
### Decorators
#### Class Decorators
- `@App(config: Hapi.IServerConnectionOptions)` : Declare a new App (correspond to a new Hapi.Server instance).
- `@Module(config: IModuleConfig)` : Declare a new Module (class containing routes).
- `@Modules(modules: Array<Module>)` : Assign an array of modules to an App or a Module.

#### Method decorators
- `@Route(config: Hapi.IRouteConfiguration)` : Declare a new Route inside a Module. The target method will become the route handler.

### Interfaces

#### IApp
- `constructor(server: Hapi.Server)` : App will be constructed with Hapi server instance as first argument.
- `onInit()`: Method called when Hapi server initialization is done.
- `onStart()`: Method called when Hapi server is started.

#### IModuleConfig
- `basePath: string` : Base path applied to all contained routes in the module.

### Methods
- `bootstrap(...apps: Array<IApp>)` : Bootstrap your apps.
