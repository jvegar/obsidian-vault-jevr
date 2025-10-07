# Chapter 01 : The Node.js Platform

## How Node.js works

### I/O is slow
- I/O is the slowest among fundamental operations
	- Accessing RAM is in the order of nanoseconds (10E-9 seconds)
	- Accessing **disk or network** is in the order of milliseconds (10E-3 seconds)

### Blocking I/O
- Calling an I/O request will block the execution of a thread until operation completes.
- The following pseudocode shows a typical blocking thread performed against a socket.
```js
data = socket.read()
// data is available
print(data)
```
- It's trivial to notice that a web server that is implemented using blocking I/O will not be able to handle multiple connections in the same thread.
- **Traditional approach**: Use a separate thread (or process) to handle each concurrent connection. This way, a thread blocked on an I/O operation will not impact the availability of the other connections, because they are handled in separate threads.
![[Pasted image 20250730103301.png]]

This figure emphasis on the amount of time each thread is idle and waiting for new data to be received from the associated connection. We could see how many times a thread has to block in order to wait for the result of an I/O operation. Unfortunately, **a thread is not cheap in terms of system resources**. It consumes memory and causes context switches.

### Non-blocking I/O
In addition to blocking I/O, most modern operating systems support another mechanism to access resources, called non-blocking I/O. In this operating mode, the system call always return immediately without waiting for the data to be read or written. If no results are available at the moment of the call, the function will simply return a predefined constant, indicating that there is no data available to return at that moment.
The most basic pattern to deal with this type of non-blocking I/O is to actively poll the resource within a loop until some actual data is returned. This is call busy-waiting. The following pseudocode shows you how it's possible to read from multiple resources using non-blocking I/O and an active polling loop.

```js
resources = [socketA, socketB, fileA]
while (!resources.isEmpty()) {
	for (resource of resources) {
		// try to read
		data = resource.read()
		if (data === NO_DATA_AVAILABLE) {
			// there is no data to read at the moment
			continue
		}
		if (data === RESOURCE_CLOSED) {
			// the resource was closed, remove it from the list
			resources.remove(i)	
		} else {
			consumeData(data)
		}
	}
}
```
Polling algorithms usually result in a huge amount of wasted CPU time.

### Event demultiplexing
[[Busy-waiting]] is definitely not an ideal technique for processing non-blocking resources, but luckily, most modern operating systems provide a native mechanism to handle concurrent- non-blocking resources in an efficient way. We are talking about the **Synchronous Event [[Demultiplexer]]** (also know as the **event notification interface**).

This watches multiple resources and returns a new event (or set of events) when a read or write operation executed over one of those resources completes. The advantage here is that the synchronous event demultiplexer is, of course, synchronous, so it blocks until there are new events to process.

```js
watchedList.add(socketA, FOR_READ)                            // (1)
watchedList.add(fileB, FOR_READ)
while (events = demultiplexer.watch(watchedList)) {           // (2)
  // event loop
  for (event of events) {                                     // (3)
    // This read will never block and will always return data
    data = event.resource.read()
    if (data === RESOURCE_CLOSED) {
      // the resource was closed, remove it from the watched list
      demultiplexer.unwatch(event.resource)
    } else {
      // some actual data was received, process it
      consumeData(data)
    }
  }
}
```

Let's see what happens in the preceding pseudocode:
1. The resources are added to a data structure, associating each one of them with a specific operation. (in our example, a `read`)
2. The demultiplexer is set up with the group of resources to be watched. The call to `demultiplexer.watch` is synchronous and blocks until any of the watched resources are ready for `read`.
When this ocurrs, the event demultiplexer returns from the call and a new set of events is available to be processed.
3. Each event returned by the event demultiplexer is processed. At this point, the resource associated with each event is guaranteed to be ready to read and to not block during the operation.
When all the events are processed, the flow will block again on the event demultiplexer until new events are again available to be processed. This is call the **event loop**.

It's interesting to see that, with this pattern, we can now handle several I/O operations inside a single thread, without using the busy-waiting technique. It should now be clearer why we are talking about demultiplexing; using just a single thread, we can deal with multiple resources. Figure 1.2. will help you visualize what's happening in a web server that uses a synchronous event demultiplexer and a single thread to handle multiple concurrent connections:

![[event-demultiplexer.png]]

As this shows, using only one thread does not impair our ability to run  multiple I/O-bound tasks concurrently. The tasks are spread over time, instead of being spread across multiple threads. This has the clear advantage of minimizing the total idle time of the thread, as is clearly shown in Figure 1.2.

But this is not the only reason for choosing this I/O model. In fact, having a single thread also has a beneficial impact on the way programmers approach concurrency in general. Throughout the book, you will see how the absense of in-process race conditions and multiple threads to synchronize allows us to use much simpler concurrency strategies.

### The Reactor Pattern

It is a specialization of the algorithms presented in the previous sections. The main idea behind the reactor pattern is to have a handler with each I/O operation. A handler in Node.js is represented by a `callback` (or `cb` for short) function.

The handler will be invoked as soon as an event is produced and processed by the event loop. The structure of the reactor pattern is shown in the following figure.
![[the-reactor-pattern.png]]

This is what happens in an application using the reactor pattern:
1. The application generates a new I/O operation by submitting a request to the **Event Demultiplexer**. The application also specifies a handler, which will be invoked when the operation completes. Submitting a new request to the **Event Demultiplexer** is a non-blocking call and it immediately returns control to the application.
2. When a set of I/O operations completes, the **Event Demultiplexer** pushes a set of corresponding events into the **Event Queue**.
3. At this point, the **Event Loop** iterates over the items of the **Event Queue**.
4. For each event, the associated handler is invoked.
5. The handler, which is a part of the application code, gives back control to the **Event Loop** when its execution completes (5a). While the handler executes, it can request new asynchronous operations (5b), causing new items to be added to the **Event Demultiplexer** (1).
6. When all items in the **Event Queue** are processed, the **Event Loop** blocks again on the **Event Demultiplexer**, which then triggerts another cycle when a new event is available.

The asynchronous behavior has now become clear. The application expresses intereset in accessing a resource at one point in time (without blocking) and provides a handler, which will then be invoked at another point in time when the operation completes.

### Libuv, the I/O engine of Node.js

Each operating system has its own interface for the event demultiplexer: `epoll` on Linux, `kqueue` on macOS, and the I/O completion port (IOCP) API on Windows. On top of that, each I/O operation can behave quite differently depending on the type of resource, even within the same operating system. In Unix operating systems, for example, regular filesystem files do not support non-blocking operations, so in order to simulate non-blocking behavior, it is necessary to use a separate thread outside the event loop.
All these incosistencies across and within the different operating systems required a higher-level abstraction to be built for the event demultiplexer. This is exactly why the Node.js core team created a native library called **libuv**, with the objetive to make Node.js compatible with all the major operating systems and normalize the non-blocking behavior of the different types of resource. Libuv represent the low level I/O engine of Node.js and is probably the most important component that Node.js is built on.
Other than abstracting the underlying system calls, libuv also implements the reactor pattern, thus providing an API for creating event loops, managing the event queue, running asynchronous I/O operations, and queing other types of task.

### The recipe of Node.js
The reactor pattern and libuv are the basic building blocks of Node.js but we need three more components to build the full platform:
- A set of bindings responsible for wrapping and exposing libuv and other low-level functionalities to JavaScript.
- V8, the JavaScript engine originally developed by Google for the Chrome browser. This is one fo the reasons why Node.js is so fast and efficient. V8 is acclaimed for its revolutionary design, its speed, and for its efficient memory management.
- A core JavaScript library that implements the high-level Node.js API.

This is the recipe for creating Node.js, and the following image represents its final architecture:
![[the-nodejs-internal-components.png]]

This concludes our journey through the internal mechanism of Node.js. Next, we'll take a look at some important aspects to take into consideration when working with JavaScript in Node.js.

## JavaScriptin Node.js

One important consequence of the architecture we have just analyzed is that the JavaScript we use in Node.js is somewhat different from the JavaScript we use in the browser.
The most obvious difference is that in Node.js we don't have a DOM and we don't have a `window` or a `document`. On the other hand, Node.js has access to a set of services offered by the underlying operating system that are not available in the browser. In fact, the browser has to implement a set of safety measures to make sure that the underlying system is not compromised by a rougue web application. The browser provides a higher-level abstraction over the operating system resources, which makes it easier to control and contain the code that runs in it, which will also inevitably limit its capabilities. In turn, in Node.js we can virtually have access to all the services exposed by the operating system.
In this overview, we'll take a look at some key facts to keep in mid when using JavaScript in Node.js.

## Run the latest JavaScript with confidence
One of the main pain points of using JavaScript in the browser is that our code will likely run on a variety of devices and browsers. Dealing with different browser means dealing with JavaScript runtimes  that may miss some of the newest features og both the language ot the web platform. Luckily, today this problem can be somewhat mitigated by the use of transpiles and polyfills. Nonetheless, this brings its own set of disadvantages and not everything can be polyfilled.

All these inconveniences don't apply when developing applications on Node.js. In fact, our Node.js applications will most likely run on a system and a Node.js runtime that are well known in advance. This makes a huge difference as it allows us to target our cide for a specific JavaScript and Node.js version, with the absolute guarantee that we won't have any surprises when we run it on production.

This factor, in combination with the fact that Node.js ships with very recent versions of V8, means that we can use with confidence most fo the features of the latest ECMAScript specification (ES for short; this is the standard on which the JavaScript language is based) without the need for any extra transpilation step.

Please bear in mind, though, that if we are developing a library meant to be used by third parties, we still have to take into account that our code may run on different versions of Node.js. The general pattern in this case is to target the oldest active **long-term** (LTS) release and specify the `engines` section in our `package.json`, so that the package manager will warn the user if they are trying to instll a package that is not compatible with their version of Node.js.

### The module system
From its inception, Node.js shipped with a module system, even when JavaScript still had no official support for any form of it. The original Node.js module system is called CommonJS and it ses the `require` keyword to import functions, variables, and classes exported by built-in modules located on the device's filesystem.
CommonJS was a revolution for the JavaScript world in general, as it started to get popular even in the client-side world, where it is used in combination with a module bundler (such as Webpack or Rollup) to produce code bundles that are easily executable by the browser. CommonJS was a necessary component for Node.js to allow developers to create large and better organized applications on a par with other server-side platforms.
Today, JavaScript has the so-called ES modules syntax (the `import` keyword may be more familiar) from which Node.js inherits just the syntax, as the underlying implementations is somewhat different from that of the browser. In fact, while the browser mainly deals with remote modules, Node.js, at least for now, can only deal with modules located on the local filesystem.
We'll talk about modules in more detail in the next chapter.

### Full access to operating system services
As we already mentioned, even if Node.js uses JavaScript, it doesn't run inside the boundaries of a browser. This allows Node.js to have binding for all the major services offered by the underlying operating system.
For example, we can access any file on the filesystem (subject to any operating system-level permission) thanks to the `fs` module, or we can write applications that use low-level TCP or UDP sockets thanks to the `net` and `dgram` modules. We can create HTTP(S) servers (wit th `http` and `https` modules) or use the standard encryption and hashing algorithms of OpenSSL (with the `crypto` module). We can also access some of the V8 internals (the `v8` module) or run code in a different V8 context (with the `vm` module).
We cal also run other processses (with the `child_process` module) or retrieve our own application's process information using the `process` global variable. In particular, from the `process` global variable, we can get a list of the environment variables assigned to the process (with `process.env`) or the command-line arguments passed to the application at the moment of its launch (with `process.argv`).
Throughout the book, you'll have the opportunity to use many of the modules here, but for a complete reference, you can check the official Node.js documentation.

### Running native code
One of the most powerful capabilities offered by Node.js is certainly the possibility to create userland modules that can bind to native code. This gives to the platform a tremendous advantage as it allow us to reuse existing or new components written in C/C++. Node.js officially provides great support for implementing native modules thanks to the N-API interface.
But what's the advantage? First of all, it allows us to reuse with little effort a vast amount of existing open source libraries, and mos importantly, it allows a company to reuse its own C/C++ legacy code without the need to migrate it.
Another important consideration is that native code is still necessary to access low-level features such as communicating with hardware drivers or with hardware ports (for example, USB or serial). In fact, thanks to its ability to link to native code, Node.js has become popular in the world of the **Internet of things (IoT)** and homemade robotics.
Finally, even though V8 is very (very) fast at executing JavaScript, it still has a performance penalty to pay compared to executing native code. In everyday computing, this is rarely  an issue, bur for CPU-intensive applications, such as those with a lot of data processing and manipulation, delegating the work to native code can make tons of sense.
We should als mention that, nowadays, most JavaScript virtual machines (VMs)(and also Node.js) support WebAssemblu (Wasm), a low-level instruction format that allows us to compile languages other than JavaScript (such as C++ or Rust) into a format that is understandable by JavaScript VMs. This bring many of the advantages we have mentioned, without the need to directly interface with native code.
