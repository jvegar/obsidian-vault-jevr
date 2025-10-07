---
date: 2025-10-02
tags:
  - #command-line
hubs:
  - "[[csharp]]"
urls:
  - https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/ide0028
---
# 2025-10-02_Use-collection-initializers-or-expressions

This style rule concerns the use of [[1759441088-collection-initializer|collection initializer]] and, if you're using C# 12 or later, [[1759431694-collection-expressions|collection expressions]] for collection initialization.

In .NET 8 (C# 12) and later versions, if you have the `dotnet_style_prefer_collection_expression` option set to `true`, the code fix in Visual Studio converts your collection initialization code to use a collection expression (`List<int> list = [1, 2, 3];`). In Visual Basic and in .NET 7 (C# 11) and earlier versions, the code fix converts your code to use a collection initializer (`List<int> list = new List<int> { 1, 2, 3};`)
