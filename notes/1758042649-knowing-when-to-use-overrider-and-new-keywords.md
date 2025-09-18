# Knowing When to Use Overrider and New Keywords

In C#, a method in a derived class can have the same as a method in the base class. You can specify how the methods interact by using the new and override keywords. The `override` modifier extends the base class `virtual` method, and the `new` modifier hides an accessible base class method. The difference is illustrated in the examples in this topic.

In a console application, declare the following two classes, `BaseClass` and `DerivedClass`. `DerivedClass` inherits from `BaseClass`.

```cs
class BaseClass
{
    public void Method1()
    {
        Console.WriteLine("Base - Method");
    }
}
```

In the `Main` method, declare variables `bc`, `dc`, and `bcdc`.
- `bc` is of type `BaseClass`, and its value is of type `BaseClass`.
- `dc` is of type `DerivedClass`, and its value is of type `DerivedClass`. This is the variable to pay attention to.


