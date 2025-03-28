#azure #serverless
- Serverless solution that allows you write less code, maintain less infrastructure, and save on costs.
- The cloud provides all the up-to-date resources needed to keep you applications running.
- The features allows you:
	- **Use your preferred language**
	- **Automate deployment**
	- **Troubleshoot a function**
	- **Take advantage of flexible pricing options**

### Example scenario
- You own a business and want to send a holiday greeting email to your customers.
- Instead of building a website and deploying and hosting it just for one feature, you can use Azure functions, add your email sign-in details in the function, and then deploy it to Azure.
- The functions connects to the data source, gets your customer's email information, and sends them an email on a scheduled date and time.

### What is Azure Functions?

- It's a cloud-based compute service that provide event-driven and scalable serverless compute for Azure.
- You can use it to run your code when you need it to run. For example, your code can run as the resulta of an event or change, such as when a message arrives in a queue or when a stored object is updated. You can also define a scheduled interval for your code to run by using `cron` rules.

### Use triggers to decide when to run code
- Azure Functions lets you define triggers, which start the execution of your code.
- They can also process inputs for passing data into your functions.
- Each function can have only one trigger.
- Some of the triggers supported:
	- Storage
	- Events
	- HTTP code
	- Queues
	- Timer
### Use binding to connect to data sources
- Binding are a way to simplify coding for input and output data.
- While you can use a client SDK to connect to services from your function code, Azure Functions provides binding to simplify these connections.
- They are a connection code you don't have to write.
- They can integrate with many services on Azure and solve integration problems and automate business process.
- They come in two flavors, input and output.
	- An output binding provides a way to write data to the data destination
	- An input binding can be used to pass data to your function from a data source different than the one that triggered the function.

### Features
- Flexible hosting plans
   - Consumption plan
   - Premium plan
   - Dedicated plan
- Dynamic scaling
- Event based architecture

## How Azure Functions works

### Scaling function apps
- The context in which your functions run is called a _function app_. A function app is a unit of deployment, management, and scale for your functions. The functions within a function app all share the same settings and connections.
- In the consumption and premium plans, Azure Functions scales CPU and memory resources by adding more function app instances. The number of instances is determined based on the number of events that trigger a function. Because all of the functions within a function app share the resources within an app instance, they scale at the same time.
### Azure Functions monitoring
- Azure Functions offers built-in integration with Azure Application Insights to monitor functions. Application Insights collect log, performance, and error data. It helps you detect performance anomalies, diagnose issues, and better understand how your functions are used.
- ![[Pasted image 20250218113532.png]]
### Azure Functions Components
- Function triggers
	- They are what cause a function to run. Defines how a function is invoked.
- Function bindings
	- Binding to a function is way of declaratively connecting another resource to the function. 
- Function runtime
	- Supports several version of the runtime host. Functions also support many different runtimes such as .NET Core, Node.JavaScript, Java, PowerShell, and Python.
- API Management (APIM)
- Deployment slots
- Function app configuration