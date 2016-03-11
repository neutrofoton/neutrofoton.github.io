---
layout: post
title: "Generic Singleton Pattern in Java"
date: 2013-08-29 15:38:32 +0800
comments: true
categories: [java, pattern]
---
In my previous post about singleton pattern in C#, it is very nice flexible singleton code which make us be able to create a singleton instance of a non singleton class. But we can still be able to create a non singleton instance of the target class.

In this post I want to implement it in Java. As far as I know even though both C# and Java have generic feature, generic in C# quite different from the one in Java. While converting of what I did about generic singleton in C# to Java, I feel that the way I do in Java is not as elegant as in C#. The following code is the generic singleton code I write in Java.

``` java generic singleton
import java.util.HashMap;
import java.util.Map;

public class Singleton{

    private static final Singleton instance = new Singleton();

    @SuppressWarnings( "rawtypes")
    private Map<Class,Object> mapHolder = new HashMap<Class,Object>();

    private Singleton() {}


    @SuppressWarnings("unchecked")
    public static <T> T getInstance(Class<T> classOf) throws InstantiationException, IllegalAccessException {


        synchronized(instance){

            if(!instance.mapHolder.containsKey(classOf)){

                T obj = classOf.newInstance();  

                instance.mapHolder.put(classOf, obj);
            }

            return (T)instance.mapHolder.get(classOf);          

        }

    }

    public Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException();
    }   
}
```

The following code show how to use the generic singleton class. It’s quite similar to the C# version.

``` java sample
public class Starter {

    /**
     * @param args
     * @throws InvocationTargetException
     * @throws IllegalArgumentException
     * @throws SecurityException
     * @throws NoSuchMethodException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    public static void main(String[] args) throws InstantiationException, IllegalAccessException  {
        // TODO Auto-generated method stub

        Employee emp1 = new Employee();
        emp1.setName("neutro");

        Employee emp2 = new Employee();
        emp2.setName("neutro");

        boolean isEqual = emp1.equals(emp1);

        isEqual = emp1==emp2;       
        System.out.println("Is Equal Test1 = "+ isEqual);

        Singleton.getInstance(Employee.class).setName("Hello");
        emp1 = Singleton.getInstance(Employee.class);
        emp2 = Singleton.getInstance(Employee.class);

        isEqual = emp1==emp2;       
        System.out.println("Is Equal Test2 = "+ isEqual);

        System.out.println("emp1 = "+ emp1.getName());
        System.out.println("emp2 = "+ emp2.getName());


        Singleton.getInstance(Departement.class).setName("Information Technology");
        Departement dpt1 = Singleton.getInstance(Departement.class);
        Departement dpt2 = Singleton.getInstance(Departement.class);

        isEqual = dpt1==dpt2;       
        System.out.println("Is Equal Test3 = "+ isEqual);

        System.out.println("dpt1 = "+ dpt1.getName());
        System.out.println("dpt2 = "+ dpt2.getName());

    }
}
```

Again, if we see from the sample code above it seems that generic singleton in C# is more elegant than in Java.
The main thing that I should do is I have to use <code>HashMap</code> collection to hold any instance of what that want to be as singleton and reuse for the future need in the life cycle of my application.

Anyway, if you have better and much more elegant solution how to implement generic singleton pattern in Java I am very appreciate for any suggestion. Finally I would like to thank you very much for visiting and reading my blog, and have a nice day.

<h3>Update March 11, 2016</h3>
> If you want to the java singleton above, please ensure you have remove / clean up method to clean up <code>HashMap</code> contents if the singleton objects are not needed anymore. At least at the end of application lifecycle, the <code>HashMap</code> and its contents should be removed completely from memory.
