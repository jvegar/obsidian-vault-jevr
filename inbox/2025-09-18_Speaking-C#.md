---
date: 2025-09-18
tags:
  -
hubs:
  - "[[csharp]]"
urls:
  - https://learn.microsoft.com/en-us/dotnet/csharp/specification/
  - https://github.com/dotnet/csharplang
  - https://github.com/dotnet/roslyn
  - https://github.com/dotnet/csharpstandard
---
# 2025-09-18_Speaking-C#

## Understanding C# standards
Microsoft has submitted a few versions of C# to ECMA standards bodies.

C# language design: https://github.com/dotnet/csharplang
Compiler implementation: https://github.com/dotnet/roslyn
Standard to describe the language: https://github.com/dotnet/csharpstandard

## Discoverint your C# compiler version
- Roslyn is the .NET language compiler for C# and Visual Basic.

|.NET SDK|Roslyn compiler|Default C# language|
|--------|---------------|-------------------|
|1.0.4   |2.0-2.2        |7.0                |
|--------|---------------|-------------------|
|1.1.4   |2.3-2.4        |7.1                |
|--------|---------------|-------------------|
|2.1.2   |2.6-2.7        |7.2                |
|--------|---------------|-------------------|
|2.1.200 |2.8-2.10       |7.3                |
|--------|---------------|-------------------|
|3.0     |3.0-3.4        |8.0                |
|--------|---------------|-------------------|
|5.0     |3.8            |9.0                |
|--------|---------------|-------------------|
|6.0     |4.0            |10.0               |
|--------|---------------|-------------------|
|7.0     |4.4            |11.0               |
|--------|---------------|-------------------|
|8.0     |4.8            |12.0               |
|--------|---------------|-------------------|
|9.0     |4.12           |13.0               |
|--------|---------------|-------------------|

- The project you create can target older versions of .NET and still use a modern compiler version. For example, if you have installed the .NET 9 SDK or later installed, then you can use C# language features in a console app that targets .NET 8.

## How to determina .NET version

```sh
dotnet --version
```

## Enabling a specific language vesion compiler
- dotnet command-line assumes that you want to use the latest version of a C# language compiler by default.
- To use the improvements of a version you have to add a `<LangVersion>` configuration element to the project file.

```xml
<LangVersion>7.3</LangVersion>
```

- Another potential values for `<LangVersion>`:
    - latestmajor: Uses the highest major number
    - latest: Uses the highest major and highest minor number
    - preview: Uses the highest available preview version

- Examples: 
After creating a new project, you must edit the `.csproj` file and add the `<LangVersion>` element set to `preview` to use the preview C# 14 compiler.

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <LangVersion>preview</LangVersion>
  </PropertyGroup>
</Project>
```

You can use the .NET 10 SDK and its C# 14 compiler while your projects continue to target .NET 9. To do so, set the target framework to `net9.0` and add a `<LangVersion>` element set to `14`, as shown in the following markup.

```csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net9.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <LangVersion>14</LangVersion>
  </PropertyGroup>
</Project>
```

