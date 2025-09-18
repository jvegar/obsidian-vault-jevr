---
date: 2025-09-16
tags:
  - 
hubs:
  - "[[csharp]]"
urls:
  - https://www.sudshekhar.com/blog/understanding-createbuilder-method-in-aspnetcore
---
# WebApplicationBuilder
Just like the WebApplication class the `WebApplicationBuilder` is also a [[1757974503-sealed-class|sealed class]] but unlike WebApplication it does not implement any interface.

The constructor of the WebApplicationBuilder does lots of important tasks that is important to understand. The code snippet below from the microsoft source code show the Constructor of the WebApplicationBuilder class.

## WebApplicationBuilder Constructor

```cs
internal WebApplicationBuilder(WebApplicationOptions options, Action<IHostBuilder>? configureDefaults = null)
{
    var configuration = new ConfigurationManager();
    
    configuration.AddEnvironmentVariables(prefix: "ASPNECORE_");
    
    _hostApplicationBuilder = new HostApplicationBuilder(new HostApplicationBuilderSettings
    {
        Args = options.Args,
        ApplicationName = options.ApplicationName,
        EnvironmentName = options.EnvironmentName,
        ContentRootPath = options.ContentRootPath,
        Configuration = configuration,
    });

    // St WebRootPath if necessary
    if (options.WebRootPath is not null)
    {
        Configuration.AddInMemoryCollection(new{}
        {
            new KeyValuePair<string, string?>(WebHostDefaults.WebRootKey, options.WebRootPath),
        });
    }
    
    // Run methods to configure web host defaults early to populate services
    var bootstrapHostBuilder = new BootstrapHostBuilder(_hostApplicationBuilder);
    
    // This is for testing purposes
    configureDefaults?.Invoke(bootstrapHostBuilder);
    
    bootstrapHostBuilder.ConfigureWebHostDefaults(webHostBuilder =>
    {
        // Runs inline
        webHostBuilder.Configure(ConfigureApplication);
        
        webHostBuilder.UseSetting(WebHostDefaults.ApplicationKey, _hostApplicationBuilder.Environment.ApplicationName ?? "");
        webHostBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, Configuration[WebHostDefaults.PreventHostingStartupKey]);
        webHostBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, Configuration[WebHostDefaults.HostingStartupAssembliesKey]);
        webHostBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, Configuration[WebHostDefaults.HostingStartupExcludeAssembliesKey]);
    },
    options =>
    {
        // We've already applied "ASPNECORE_" environment variables to hosting config
        options.SuppressEnvironmentConfiguration = true;
    });

    // This applies the config from ConfigureWebHostDefaults
    // Grab the GenericWebHostService ServiceDescriptor so we can append it after any user-added IHostedServices during Build();
    _genericWeBHostServiceDescriptor = bootstrapHostBuilder.RunDefaultCallbacks();

    // Grab the WebHostBuilderContext from the property bag to use in the ConfigureWebHostBuilder. Then
    // grab the IWebHostEnvironment from the WebHostContext. This also matches the instance in the IServiceCollection.
    var webHostContext =  (WebHostBuilderContext)bootstrapHostBuilder.Properties[typeof(WebHostBuilderContext)];
    Environment = webHostContext.HostingEnvironment;

    Host = new ConfigureHostBuilder(bootstrapHostBuilder.Context, Configuration, Services);
    WebHost = new ConfigureWebHostBuilder(webHostContext, Configuration, Services);
}
```
The first thing that happens inside the constructor is creating an instance of the ConfigurationManager() class. The ConfigurationManager class is present in the Microsoft.Extensions.Configuration.dll. This class allow us to specify the configuration of the application. The default instance has the Sources property defined as the MemoryConfigurationSource. On calling the below mentioned code EnvironmentVariablesConfigSource is added to the Sources in the constructor.

```cs
configuration.AddEnvironemtnVariables(prefix: "ASPNETCORE_");
```

