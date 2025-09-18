---
date: 2025-09-13
tags:
  - #net-core
hubs:
  - "[[]]"
urls:
  -
---
# 2025-09-13_Learning-Entity-Framework-Core

## WebApplication Class
- Namespace: Microsoft.AspNetCore.Builder
The web application used to configure the HTTP pipeline, and routers.
- Methods:
  - CreateBuilder: Initializes a new instance of the [[1757823016-webapplicationbuilder|WebApplicationBuilder]]class with preconfigured defaults.

## Understanding CreateBuilder() method in AspNetCore

### Introduction
We'll examine source code of Microsoft.AspNetCore from GitHub

```cs
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline

if (app.Environment.IsDevelopment()) {
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapController();
app.Run();
```

The first line starts with the call to the `CreateBuilder()` method of the WebApplication class. The call to the `CreateBuilder()` method retuns us an instance of the WebApplicationBuilder class. You can find the WebApplication, WebApplicationBuilder and the related classes in the figure below.
![[webapplication.png]]

### WebApplication class
The WebApplication class is a __**sealed**__ class and implements the following interfaces:
```
IHost
IApplicationBuilder
IEndpointRouteBuilder
IAsynDisposible
```

This class is used to configure the http pipeline and the routes.

There are multiple overloads of the `CreateBuilder()` method in the WebApplication class that returns an instance of the WebApplicationBuilder class with preconfigured defaults.

```cs
public static WebApplicationBuilder CreateBuilder(string[] args) =>
    new(new() { Args = args });
```

There is also a Create() method that just returns an instance of the WebApplication class instead of the WebApplicationBuilder class.
`
```cs
public static WebApplication Create(string[]? args = null) =>
    new WebApplicationBuilder(new() { Args = args }).Build();
```

The Create() method internally invokes the Build() method of the WebApplicationBuilder class and returns a WebApplication instead. However in our Program.cs class you would find that we are explicitly calling the Build() method after injecting the required services to give us our app instance.

```cs
var app = builder.Build();
```


### WebApplicationBuilder class
Just like the WebApplication class the WebApplicationBuilder is also a [[1757974503-sealed-class|sealed class]] 
