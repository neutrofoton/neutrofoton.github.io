---
layout: post
title: "Generic Singleton Pattern in C#"
date: 2013-08-29 15:27:41 +0800
comments: true
categories: [csharp, pattern]
---
In software engineering, the singleton pattern is a design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system. The concept is sometimes generalized to systems that operate more efficiently when only one object exists, or that restrict the instantiation to a certain number of objects. The term comes from the mathematical concept of a singleton.<a href="http://en.wikipedia.org/wiki/Singleton_pattern">[1]</a>

Honestly in my daily coding activity, sometimes I need to implement singleton pattern. Jon wrote a good singleton article in C# <a href="http://www.yoda.arachsys.com/csharp/singleton.html">here</a>. Another case I want to implement singleton pattern in generic way. I convert what has been wrotten by Jon into generic.

``` c# generic singleton
/// <summary>
    /// Generic class which makes non singleton class to be singleton.
    /// Calling singleton instance of class should be Singleton<T>.Instance, otherwise the returned class is not singleton one.    ///
    /// </summary>
    /// <typeparam name="T">Genric class type to be singletonized</typeparam>
    public sealed class Singleton<T> where T : class, new()
    {
        /// <summary>
        /// Private constuctor to avoid this class instantiated
        /// </summary>
        Singleton()
        {
        }

        /// <summary>
        /// Property to get access to singleton instance
        /// </summary>
        public static T Instance
        {
            get
            {
                return Nested.instance;
            }
        }

        /// <summary>
        /// Private nested class which acts as singleton class instantiator. This class should not be accessible outside <see cref="Singleton<T>"/>
        /// </summary>
        class Nested
        {
            /// <summary>
            /// Explicit static constructor to tell C# compiler not to mark type as beforefieldinit
            /// </summary>
            static Nested()
            {
            }

            /// <summary>
            /// Static instance variable
            /// </summary>
            internal static readonly T instance = new T()
        }
    }
```

The following snipped code is a sample how to use the generic singleton class.
``` c# sample
public class Processor
{
    public string Name
    {
        get;
        set;
    }

    public void Execute()
    {
    }
}

public class void Main(string[] args)
{
    Singleton<Processor>.Instance.Execute();
}
```

From the sample above is shown that we can make a non singleton class to be singleton at runtime. But we can still create a non singleton instance as we need. That’s really nice flexible thing.

<h3>Reference</h3>
[1] http://en.wikipedia.org/wiki/Singleton_pattern
