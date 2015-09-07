### Coding style guidelines – general

The most general guideline is that we use all the VS default settings in terms of code formatting, except that we put `System` namespaces before other namespaces (this used to be the default in VS, but it changed in a more recent version of VS).

1. Use four spaces of indentation (no tabs)
2. Use `_camelCase` for private members
3. Avoid `this.` unless absolutely necessary
4. Always specify member visiblity, even if it's the default (i.e. `private string _foo;` not `string _foo;`)


### Usage of the var keyword

The `var` keyword is to be used as much as the compiler will allow. For example, these are correct:

```csharp   
    var fruit = "Lychee";
    var fruits = new List<Fruit>();
    var flavor = fruit.GetFlavor();
    string fruit = null; // can't use "var" because the type isn't known (though you could do (string)null, don't!)
    const string expectedName = "name"; // can't use "var" with const
```

The following are incorrect:

```csharp
string fruit = "Lychee";
List<Fruit> fruits = new List<Fruit>();
FruitFlavor flavor = fruit.GetFlavor();
```


### Use C# type keywords in favor of .NET type names

When using a type that has a C# keyword the keyword is used in favor of the .NET type name. For example, these are correct:

```csharp
    public string TrimString(string s) {
    return string.IsNullOrEmpty(s)
        ? null
        : s.Trim();
    }
    var intTypeName = nameof(Int32); // can't use C# type keywords with nameof
```

The following are incorrect:

```csharp
public String TrimString(String s) {
    return String.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}
```

### Hashing
https://github.com/aspnet/Identity/blob/dev/src/Microsoft.AspNet.Identity/PasswordHasher.cs

We will be using the Pbkdf2 hashing.

```csharp
            string password = Console.ReadLine();
            // generate a 128-bit salt using a secure PRNG
            byte[] salt = new byte[128 / 8];
            using (var rng = RandomNumberGenerator.Create())
            {
                rng.GetBytes(salt);
            }
            Console.WriteLine($"Salt: {Convert.ToBase64String(salt)}");

            // derive a 256-bit subkey (use HMACSHA1 with 10,000 iterations)
            string hashed = Convert.ToBase64String(KeyDerivation.Pbkdf2(
                password: password,
                salt: salt,
                prf: KeyDerivationPrf.HMACSHA1,
                iterationCount: 10000,
                numBytesRequested: 256 / 8));
            Console.WriteLine($"Hashed: {hashed}");
```

### Encryption

For encryption we will use Aes256 with salt.

https://gist.github.com/JonHaywood/996aa010e9a3858339c3#file-aes256encryptionservice
```csharp
            var service = new Aes256EncryptionService();
            var key = service.GenerateKey();

            var plainText = password;
            var encryptedData = service.EncryptStringToBytes(key, plainText);
            var decryptedData = service.DecryptBytesToString(key, encryptedData);
```