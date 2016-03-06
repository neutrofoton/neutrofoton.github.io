---
layout: post
title: "Json Serializer"
date: 2012-08-03 20:10:27 +0800
comments: true
categories: c#
---

<code>Json or JavaScript Object Notation, is a text-based open standard designed for human-readable data interchange. It is derived from the JavaScript scripting language for representing simple data structures and associative arrays, called objects. Despite its relationship to JavaScript, it is language-independent, with parsers available for many languages [wikipedia]</code>

In this post will talk about Json serialize/deserialize with Data Contract Serializer and <a href="http://json.codeplex.com/">Json.Net</a> library.


<h2>Data Contract Serializer</h2>

<code>DataContractSerializer</code> is a C# class that serializes and deserializes an instance of a type into an XML stream or document using a supplied data contract. It is used by WCF framework as default serializer. The class of instance that will be serialized should have <code>DataContract</code> attribute and its members should have <code>DataMember</code> attribute.

<code>DataContractSerializer</code> can also serialize object into Json instead of Xml.The following code is a sample class which an instance of it will be serialized into Json via <code>DataContractSerializer</code>.

``` c# sample contract model

[DataContract]
public class Product
{
    [DataMember(Order = 0)]
    public string Name
    {
        get;
        set;
    }

    [DataMember(Order = 1)]
    public DateTime Created
    {
        get;
        set;
    }

    [DataMember(Order = 2)]
    public decimal Price
    {
        get;
        set;
    }

    [DataMember(Order = 3)]
    public string[] Sizes
    {
        get;
        set;
    }
}
```
In this case I create a class that wraps serialize and deserialize process using Data Contract Serializer

``` c# generic json serializer

public class JsonDataContractSerializer
{
    public static string Serialize<T>(T obj) where T : class, new()
    {
        System.Runtime.Serialization.Json.DataContractJsonSerializer serializer =
            new System.Runtime.Serialization.Json.DataContractJsonSerializer(obj.GetType());
        using (MemoryStream ms = new MemoryStream())
        {
            serializer.WriteObject(ms, obj);
            string retVal = Encoding.Default.GetString(ms.ToArray());

            return retVal;
        }
    }

    public static T Deserialize<T>(string json) where T : class, new()
    {
        using (MemoryStream ms = new MemoryStream(Encoding.Unicode.GetBytes(json)))
        {
            System.Runtime.Serialization.Json.DataContractJsonSerializer serializer =
                new System.Runtime.Serialization.Json.DataContractJsonSerializer(typeof(T));
            return serializer.ReadObject(ms) as T;

        }
    }
}
```


``` c# sample using it the JsonDataContractSerializer
static void Main(string[] args)
{
    var product = new Product()
    {
        Name = "Geeks T-shirt",
        Created = DateTime.Now,
        Price = 100,
        Sizes = new string[] { "Small", "Medium", "Large" }
    };

    string output = JsonDataContractSerializer.Serialize<Product>(product);

    Console.WriteLine(output);

    Console.ReadKey();
}
```

``` text output result
{
  "Name": "Geeks T-shirt",
  "Created": "\/Date(1344061690773+0800)\/",
  "Price": 100,
  "Sizes": [
    "Small",
    "Medium",
    "Large"
  ]
}
```

<h2>Json.NET Serializer</h2>
Json.NET is a JSON framework for .NET which is written in C# language. The features mentioned on the project site host are :

1. Flexible JSON serializer for converting between .NET objects and JSON
2. LINQ to JSON for manually reading and writing JSON
3. High performance, faster than .NET’s built-in JSON serializers
4. Write indented, easy to read JSON
5. Convert JSON to and from XML
6. Supports .NET 2, .NET 3.5, .NET 4, Silverlight, Windows Phone and Windows 8.

At the time writing this post, I have not tested/used all those features. In this part of article I will try to serialize our previous Product class. Meanwhile we don’t need any <code>DataContract</code> or <code>DataMember</code> attribute in class and properties.

``` c# poco
public class Product
{
    public string Name
    {
        get;
        set;
    }

    public DateTime Created
    {
        get;
        set;
    }

    public decimal Price
    {
        get;
        set;
    }

    public string[] Sizes
    {
        get;
        set;
    }
}
```

``` c# sample use json.net
class Program
{
    static void Main(string[] args)
    {
        var product = new Product()
        {
            Name = "Geeks T-shirt",
            Created = DateTime.Now,
            Price = 100,
            Sizes = new string[] { "Small", "Medium", "Large" }
        };

        string output = JsonConvert.SerializeObject(product);

        Console.WriteLine(output);
        Console.ReadKey();
    }
}
```

``` text output
{
  "Name": "Geeks T-shirt",
  "Created": "2012-08-04T16:51:26.1700499+08:00",
  "Price": 100,
  "Sizes": [
    "Small",
    "Medium",
    "Large"
  ]
}
```

To get the microsoft date time format in Json result we can set the DateFormatHandling on <code>JsonSerializerSettings</code> to <code>MicrosoftDateFormat</code> :

``` c# sample
class Program
{
    static void Main(string[] args)
    {
        var product = new Product()
        {
            Name = "Geeks T-shirt",
            Created = DateTime.Now,
            Price = 100,
            Sizes = new string[] { "Small", "Medium", "Large" }
        };

        JsonSerializerSettings microsoftDateFormatSettings = new JsonSerializerSettings
        {
            DateFormatHandling = DateFormatHandling.MicrosoftDateFormat
        };

        string output = JsonConvert.SerializeObject(product, microsoftDateFormatSettings);

        Console.WriteLine(output);
        Console.ReadKey();
    }
}
```
From the two serialize way above, Json.NET provide cleaner way, since it does not need any attributes in the target class. We just need to provide POCO (Plain CLR Object).

Anyway the json result of the sample actually only one line of json text. To make it readable we can use online <a href="http://jsonprettyprint.com/">Json formatter</a> which may help us to check/analyse the json text before deploying/using in production.
