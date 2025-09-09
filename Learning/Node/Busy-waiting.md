Most basic pattern for dealing with [[Non-blocking IO]] is to **actively poll** the resource within a loop until some actual data is returned.
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
			//some data was received, process it
			consumeData(data)
		}
	}
}
```