---
date: 2025-09-13
tags:
  -
hubs:
  - "[[]]"
urls:
  -
---
# 2025-09-13_Learning-CSharp

## How to use args in C#
1. Access `args` in the `Main` Method. The `Main` method can directly accept a `string[] args` parameter, and its content can be accessed in a loop.
```cs
    public static void Main(string[] args)
    {
        // Use args here
        foreach (string arg in args)
        {
            Console.WriteLine($"Argument: {arg}");
        }
    }
```


