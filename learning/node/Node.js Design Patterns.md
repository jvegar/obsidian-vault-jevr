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




