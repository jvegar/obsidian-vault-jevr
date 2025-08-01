Controllers are responsible for handling incoming **requests** and sending **responses** back to the client.
![[Controllers_1.png]]
A controller's purpose is to handle specific requests for the application. The **routing** mechanism determines which controller will handle each request. Often, a controller has multiple routes, and each route can perform a different action.

To create a basic controller, we use classes and **decorators**. Decorators link classes with the necessary metadata, allowing Nest to create a routing map that connects to their corresponding controllers.

>**HINT**
>To quickly create a CRUD controller with built-in validation, you can use the CLI's CRUD generator `nest g resource [name]`.

### Routing
In the following example, we'll use the `@Controller()` decorator, which is required to define a basic controller. We'll specify an optional route path prefix of `cats`. Using a path prefix in the `@Controller()` decorator helps us group related together and reduces repetitive code. For example, if we want to group router that manage interactions with a cat entity under the `/cats` path, we can specify the `cats` path prefix in the `@Controller()` decorator. This way, don't need to repeat that portion of the path for each route in the file.

```ts
// cats.controller.ts 
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
	@Get()
	findAll(): string {
		return 'This action returns all cats';
	}
}
```

>**HINT**
>To create a controller using the CLI, simply execute the `$ nest g controller [name]` command.

The `@Get()` HTTP request method decorator placed before the `findAll()` method tells Nest to create a handler for a specific endpoint for HTTP requests. This endpoint is defined by the HTTP request method (GET in this case) and the route path. So, what is the route path? The route path for a handler is determined by combining the (optional) prefix declared for the controller with any path specified in the method's decorator. Since we've set a prefix (`cats`) for every route and haven't added any specific path in the method decorator, Nest will map `GET /cats` request to this handler.

As mentioned, the route path includes both the optional controller path prefix **and** any path string specified in the method's decorator. For example, if the controller prefix is `cats` and the method decorator is `@Get('breed')`, the resulting route will be `GET /cats/breed`.

In our example above, when a GET request is made to this endpoint, Nest routes the request to the user-defined `findAll()` method. Note that the method name we choose here is entirely arbitrary. While we must declare a method to bind the route to, Nest doesn't attach any specific significance to the method name.

This method will return a 200 status code along with the associated response, which in this case is just a string. Why does this happen? To explain, we first need to introduce the concept that Nest uses two **different** options for manipulating responses:

| Standar(recommended)                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Using this built-in method, when a request handler returns a JavaScript object or array, it will automatically be serialized to JSON. When it returns a JavaScript primitive types (e.g. `string`, `number`, `boolean`), however, Nest will send just the value without attempting to serialize it. This makes response handling simple: just return the value, and Nest takes care of the rest. |

| Library-specific                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| We can use the library-specific (e.g. Express) **response object**, which can be injected using the `@Res()` decorator in the method handler signature (e.g. `findAll(@Res() reponse)`). With this approach, you have the ability to use the native response handling methods exposed by that object. For example, with Express, you can construct responses using code like `response.status(200).send()`. |
>**WARNING**
>Nest detects when the handler is using either `@Rest` or `@Next()`, indicating you have chosen the library-specific option. If both approaches are used at the same time, the Standard approach is `automatically disabled` for this single route and will no longer work as expected. To use both approaches at the same time (for example, by injecting the response object to only set cookies/headers but still leave the rest to the framework), you must set the `passtrough` option to `true` in the `@Res({ passtrough: true })` decorator.

### Request object
Handlers often need access to the client's **request** details. Nest provides access to the request object from the underlying platform (Express by default). You can access the request object by instructing Nest to inject it using `@Req()` decorator in the handler's signature.

```ts
// cats.controller.ts

import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
	@Get()
	findAll(@Req() request: Request): string {
		return 'This action returns all cats';
	}
}
```

**HINT**
To take advantage of `express` typings (like in the `request: Request` parameter example above), make sure to install the `@types/express` package.

The request object represents the HTTP request and contain properties for the query string, parameters, HTTP headers, and body (read more here). In most cases, you don't need to manually access these properties. Instead, you can use dedicated decorators like `@Body()` or `@Query()`, which are available out of the box. Below is a list of the provided decorators and the corresponding platform-specific objects they represent.


| `@Request(), @Req()`      | `req`                             |
| ------------------------- | --------------------------------- |
| `@Response(), @Res()` *   | `res`                             |
| `@Next()`                 | `next`                            |
| `@Session()`              | `req.session`                     |
| `@Param(key?: string)`    | `req.params`/`req.params[key]`    |
| `@Body(key?: string)`     | `req.body`/`req.body[key]`        |
| `@Query(key?: string)`    | `req.query`/`req.query[key]`      |
| `@Headers(name?: string)` | `req.headers`/`req.headers[name]` |
| `@Ip()`                   | `req.ip`                          |
| `@HostParam()`            | `req.hosts`                       |
For compatibility with typings across underlying HTTP platforms (e.g., Express and Fastify), Nest provides `@Res()` and `@Response()` decorators. `@Res()` is simply an alias for `@Response()`. Both directly expose the underlying native platform `response` object interface. When using them. you should also import the typings for the underlying library (e.g. `@types/express`) to take full advantage. Note that when you inject either `@Res()` and `@Response()` in a method handler, you put Nest into **Library-specific mode** for that handler. and you become responsible for managing the response. When doing so, you must issue some kind of response by making a call on the `response` object (e.g., `res.json(...)` or `res.send(...)`), or the HTTP server will hang.

> **HINT**
> To learn how to create your own custom decorator, visit this chapter.

### Resources
Earlier, we defined an endpoint to fetch the cats resource (**GET** route). We'll typically also want to provide an endpoint that creates new records. For this, let's create the **POST** handler:

```ts
// cats.controller.ts

import { Controller, Get, Post } from '@nestjs/common';

@Controller('cats')
export class CatsController {
	@Post()
	create(): string {
		return 'This action adds a new cat';
	}
	
	@Get()
	findAll(): strin {
	 return 'This actin returns all cats';
	}
}
```

It's that simple. Nest provides decorators for all the standard HTTP methods: `@Get()`, `@Post()`, `@Put()`, `@Patch()`, `@Options()`, and `@Head()`. In addition, `@All()` defines an endpoint that handles all of them.

### Route wildcards
Pattern-based routes are also supported in NestJS. For example, the asteriks (`*`) can be used as wildcard to match any combination of characters in a route at the end of a path. In the following example, the `findAll()` method will be executed for any route that starts with `abcd/`, regardless of the number of characters that follow.

```ts
@Get('abcd/*') 
findAll() {
	return 'This route uses a wildcard';
}
```

The `abcd/*` route path will match `abcd/`, `abcd/123`, `abcd/abc`, and so on. The hyphen (`-`) and the dot (`.`) are interpreted literally by string-based paths.

This approach works on both Express and Fastify. However, with the latest release of Express (v5), the routing system has become more strict. In pure Express, you must use a named wildcard to make the route work -for example, `abcd/*splat`, where `splat` is simply the name of the wildcard parameter and has no special meaning. You can name it anything you like. That said, since Nest provides a compatibility layer for Express, you can still use the asterisk (`*`) as a wildcard.

When it comes to asterisks used in the **middle of a route**, Express requires named wildcards (e.g., `ab(*splat)cd`), while Fastify does not support them at all.

### Status code
As mentioned, the default **status code** for responses is always **200**, except for POST requests, which default to **201**. You can easily change this behavior by using the `@HttpCode(...)` decorator at the handler level.

```ts
@Post()
@HttpCode(204)
create() {
	return 'This action adds a new cat';
}
```

>**HINT**
>Import `HttpCode` from the `@nestjs/common` package.

Often, your status code isn't static but depends on various factors. In that case, you can use a library-specific **response** (inject using `@Res()`) object (or, in case of an error, throw an exception).

### Response headers
To specify a custom response header, you can either use a `@Header()` decorator or a library-specific response object (and all `res.header()` directly).

```ts
@Post()
@Header('Cache-Control', 'no-store')
create() {
	return 'This action adds a new cat';
}
```

>**HINT**
>Import `Header` from the `@nestjs/common` package.

### Redirection
To redirect a response to a specific URL, you can either use a `@Redirect()` decorator or a library-specific response object (and call `res.redirect()` directly).

`@Redirect()` takes two arguments, `url` and `statusCode`, both are optional. The default value of `statusCode` is `302` (`Found`) if omitted.

```ts
@Get()
@Redirect('https://nestjs.com', 301)
```

>**HINT**
>Sometimes you may want to determine the HTTP status code or the redirect URL dynamically. Do this by returning an object following the `httpRedirectResponse` interface (from `@nestjs/common`).

Returned values will override any arguments passed to the `@Redirect()` decorator. For example:

```ts
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs(@Query('version') version) {
	if (version && version === '5') {
		return { url: 'https://docs.nestjs.com/v5/' };
	}
}
```

### Route parameters
Routes with static paths won't work when you need to accept `dynamic data` as part of the request (e.g., `GET /cats/1` to get the cat with id `1`). To define routes with parameters, you can add route parameter **tokens** in the route path to capture the dynamic values from URL. The route parameter token in the `@Get()` decorator example below illustrates this approach. These route parameters can then be accessed using the `@Param()` decorator, which should be added to the method signature.

> **HINT**
> Routes with parameters should be declared after any static paths. This prevents the parameterized paths from intercepting traffic destined for the static paths.

```ts
@Get(':id')
findOne(@Param() params: any): string {
	console.log(params.id);
	return `This action returns a #${params.id} cat`;
}
```

The `@Param()` decorator is used to decorate a method parameter (in the example above, `params`), making the **route** parameters accessible as properties of that decorated method parameter inside the method. As shown in the code, you can access the `id` parameter by referencing `params.id`. Alternatively, you can pass a specific parameter token to the decorator and directly reference the route parameter by name within the method body.

> **HINT**
> Import `Param` from the `@nestjs/common` package.

```ts
@Get(':id')
findOne(@Param('id') id: string): string {
	return `This action returns a #${id} cat`;
}
```

### Sub-domain routing
The `@Controller` decorator can take a `host` option to require that the HTTP host of the incoming requests matches some specific value.

```ts
@Controller({ host: 'admin.example.com' })
export class AdminController {
	@Get()
	index(): string {
		return 'Admin page';
	}
}
```

> **WARNING**
> Since **Fastify** does not support nested routers, if you are using sub-domain routing, it is recommended to use the default Express adapter instead.

Similar to a route `path`, the `host` option can use tokens to capture the dynamic value at that position in the host name. The post parameter token in the `@Controller()` decorator example below demonstrates this usage. Host parameters declared in this way can be accessed using the `@HostParam()` decorator, which should be added to the method signature.

```ts
@Controller({ host: ':account.example.com' })
export class AccountController {
	@Get()
	getInfo(@HostParam('account') accountL string) {
		return account;
	}
}
```

### State sharing
For developers coming from other programming languages, it might be surprising to learn that in Nest, nearly everything is shared across incoming requests. This includes resources like the database connection pool, singleton services with global state, and more. It's important to understand that Node.js doesn't use the request/response Multi-Threaded Stateless Model, where each request is handled by a separate thread. As a result, using singleton instances in Nest is completely **safe** for our applications.

That said, there are specific edge cases where having request-based lifetimes for controllers may be necessary. Examples include per-request caching in GraphQL applications, request tracking, or implementing multi-tenancy. You can learn more about controlling injection scopes here.

### Asynchronicity
We love modern JavaScript, especially its emphasis on **asynchronous** data handling. That's why Nest fully supports `async` functions. Every `async` function must return a `Promise`, which allows you to return a deferred value that Nest can resolve automatically. Here's an example:

```ts
@Get()
async findAll(): Promise<any[]> {
	return [];
}
```

This code is perfectly valid. But Nest takes it a step further by allowing route handlers to return `RxJS observable streams` as well. Nest will handle the subscription internally and resolve the final emitted value once the stream completes.

```ts
@Get()
findAll(): Observable<any[]>  {
	return of([]);
}
```

Both approaches are valid, and you can choose the one that best suits your needs.

### Request payloads
In our previous example, the POST route handler didn't accept any client parameter. Let's fix that by adding the `@Body()` decorator.

Before we proceed (if you're using TypeScript), we need to define the **DTO** (Data Transfer Object) schema. A DTO is an object that specifies how data should be sent over the network. We could define the DTO schema using **TypeScript** interfaces or simple classes. However, we recommend using **classes** here. Why? Classes are part of the JavaScript ES6 standard, so they remain intact as real entities in the compiled JavaScript. In contrast, TypeScript interfaces are removed during transpilation, meaning Nest can't reference them at runtime. This is important because features like **Pipes** rely on having access to the metatype of variables at runtime, which is only possible with classes.

Let's create the `CreateCatDto` class:

```ts
export class CreateCatDto {
	name: string;
	age: number;
	breed: string;
}
```
It has only three basic properties. Thereafter we can use the newly created DTO inside the `CatsController`:

```ts
@Post()
async create(@Body() createCatDto: CreateCatDto) {
	return 'This action adds a new cat';
}
```

> **HINT**
> When handling query parameters in your routes, you can use the `@Query()` decorator to extract them from incoming requests. Let's see how this works in practice.

Consider a route where we want to filter a list of cats based on query parameters lik `age` and `breed`. First, define the query parameters in the `CatsController`:
```ts
@Get()
async findAll(@Query('age') age: number, @Query('breed') breed: string) {
	return `This action returns all cats filtered by age: ${age} and breed: ${breed}`;
}
```
In this example. the `@Query()` decorator is used to extract the values of `age` and `breed` from the query string. For example, a request to:
```sh
GET /cats?age=2&breed=Persion
```
would result in `age` being `2` and `breed` being `Persian`.

If your application requires handling more complex query parameters, such as nested objects or arrays:
```sh
?filter[where][name]=John&filter[where][age]=30
?item[]=1&item[]=2
```

you'll need to configure your HTTP adapter (Express or Fastify) to use an appropriate query parser. In Express, you can use the `extended` parser, which allows for rich query objects:

```ts
const app = await NestFactory.create<NestExpressApplication>(AppModule);
app.set('query parse', 'extended');
```
In Fastify, you can use the `querystringParser` option:

```ts
const app = await NestFactory.create<NestFastifyApplication>(
	AppModule,
	new FastifyAdapter({
		querystringParser: (str) => qs.parse(str),
	}),
);
```

> **HINT**
> `qs` is a querystring parser that supports nesting and arrays. You can install using `npm install qs`.

### Handling errors
There's a separate chapter about handling errors (i.e., working with exceptions) here.

### Full resource sample
Below is an example that demonstrates the use of several available decorators to create a basic controller. This controller provides a few methods to access and manipulate internal data.
```ts
import { Controller, Get, Query, Post, Body, Put, Param, Delete } from ''
```