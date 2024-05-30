# urlsafe
To handle URL-safe strings in C#, you typically need to ensure that the strings can be safely included in a URL without causing issues due to special characters. This involves encoding and decoding strings appropriately. Here are some common techniques:

### URL Encoding

URL encoding converts characters into a format that can be transmitted over the Internet. For example, spaces are encoded as `%20`.

#### Using `System.Net.WebUtility`

```csharp
using System.Net;

string originalString = "Hello World!";
string urlEncodedString = WebUtility.UrlEncode(originalString);
string urlDecodedString = WebUtility.UrlDecode(urlEncodedString);

Console.WriteLine($"Original: {originalString}");
Console.WriteLine($"URL Encoded: {urlEncodedString}");
Console.WriteLine($"URL Decoded: {urlDecodedString}");
```

### Base64 URL-Safe Encoding

Base64 URL-safe encoding is a variant of Base64 encoding that replaces `+` and `/` with `-` and `_` respectively, and omits padding characters `=`.

#### Implementing Base64 URL-Safe Encoding

```csharp
using System;

public static class Base64UrlEncoder
{
    public static string Encode(string input)
    {
        var bytes = System.Text.Encoding.UTF8.GetBytes(input);
        var base64 = Convert.ToBase64String(bytes);
        return base64.Replace("+", "-").Replace("/", "_").Replace("=", "");
    }

    public static string Decode(string input)
    {
        var base64 = input.Replace("-", "+").Replace("_", "/");
        switch (input.Length % 4)
        {
            case 2: base64 += "=="; break;
            case 3: base64 += "="; break;
        }
        var bytes = Convert.FromBase64String(base64);
        return System.Text.Encoding.UTF8.GetString(bytes);
    }
}

// Example usage
string originalString = "Hello World!";
string encodedString = Base64UrlEncoder.Encode(originalString);
string decodedString = Base64UrlEncoder.Decode(encodedString);

Console.WriteLine($"Original: {originalString}");
Console.WriteLine($"Base64 URL Encoded: {encodedString}");
Console.WriteLine($"Base64 URL Decoded: {decodedString}");
```

### Complete Example

Combining both URL encoding and Base64 URL-safe encoding in a complete program:

```csharp
using System;
using System.Net;

public static class Base64UrlEncoder
{
    public static string Encode(string input)
    {
        var bytes = System.Text.Encoding.UTF8.GetBytes(input);
        var base64 = Convert.ToBase64String(bytes);
        return base64.Replace("+", "-").Replace("/", "_").Replace("=", "");
    }

    public static string Decode(string input)
    {
        var base64 = input.Replace("-", "+").Replace("_", "/");
        switch (input.Length % 4)
        {
            case 2: base64 += "=="; break;
            case 3: base64 += "="; break;
        }
        var bytes = Convert.FromBase64String(base64);
        return System.Text.Encoding.UTF8.GetString(bytes);
    }
}

public class Program
{
    public static void Main()
    {
        // URL Encoding Example
        string originalUrlString = "Hello World!";
        string urlEncodedString = WebUtility.UrlEncode(originalUrlString);
        string urlDecodedString = WebUtility.UrlDecode(urlEncodedString);
        Console.WriteLine($"Original: {originalUrlString}");
        Console.WriteLine($"URL Encoded: {urlEncodedString}");
        Console.WriteLine($"URL Decoded: {urlDecodedString}");

        // Base64 URL-safe Encoding Example
        string originalBase64String = "Hello World!";
        string base64UrlEncodedString = Base64UrlEncoder.Encode(originalBase64String);
        string base64UrlDecodedString = Base64UrlEncoder.Decode(base64UrlEncodedString);
        Console.WriteLine($"Original: {originalBase64String}");
        Console.WriteLine($"Base64 URL Encoded: {base64UrlEncodedString}");
        Console.WriteLine($"Base64 URL Decoded: {base64UrlDecodedString}");
    }
}
```

This program demonstrates both URL encoding and Base64 URL-safe encoding. You can copy this code into a C# project and run it to see how the encoding and decoding processes work.
