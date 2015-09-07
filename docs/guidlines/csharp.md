### Coding style guidelines – general

The most general guideline is that we use all the VS default settings in terms of code formatting, except that we put `System` namespaces before other namespaces (this used to be the default in VS, but it changed in a more recent version of VS).

1. Use four spaces of indentation (no tabs)
2. Use `_camelCase` for private members
3. Avoid `this.` unless absolutely necessary
4. Always specify member visiblity, even if it's the default (i.e. `private string _foo;` not `string _foo;`)


### Usage of the var keyword

The `var` keyword is to be used as much as the compiler will allow. For example, these are correct:

```c#
var fruit = "Lychee";
var fruits = new List<Fruit>();
var flavor = fruit.GetFlavor();
string fruit = null; // can't use "var" because the type isn't known (though you could do (string)null, don't!)
const string expectedName = "name"; // can't use "var" with const
```

The following are incorrect:

```c#
string fruit = "Lychee";
List<Fruit> fruits = new List<Fruit>();
FruitFlavor flavor = fruit.GetFlavor();
```


### Use C# type keywords in favor of .NET type names

When using a type that has a C# keyword the keyword is used in favor of the .NET type name. For example, these are correct:

```c#
public string TrimString(string s) {
    return string.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}

var intTypeName = nameof(Int32); // can't use C# type keywords with nameof
```

The following are incorrect:

```c#
public String TrimString(String s) {
    return String.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}
```

### Hashing
https://github.com/aspnet/Identity/blob/dev/src/Microsoft.AspNet.Identity/PasswordHasher.cs
### Encryption
https://github.com/aspnet/DataProtection
http://docs.asp.net/en/latest/security/data-protection/using-data-protection.html
https://www.nuget.org/packages/Microsoft.AspNet.DataProtection/1.0.0-beta7
https://www.nuget.org/packages/Microsoft.AspNet.DataProtection.Interfaces/1.0.0-beta4