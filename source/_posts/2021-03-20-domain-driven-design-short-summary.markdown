---
layout: post
title: "Domain Driven Design Short Summary"
date: 2021-03-20 15:49:56 +0700
comments: true
categories: [pattern]
---

I believe there are tons of articles and books out there that discuss about Domain Driven Design (DDD for short). This post essentially is taken from my note archieved when I explored about DDD from several resources. I rewrite it in here as a refresh for myself mainly. 

Domain Driven Design (DDD) is the concept that the structure and language of software code (class names, class methods, class variables) should match the business domain[1].  We don't need to kill a mosquito with a cannon. We need to choose the fittest method in solving a problem. As well as DDD, it is not always be a fit method in solving application design. A software application has several attributes. Some of them deal with[3]: 

- Amount of Data
- Performance
- Business Logic complexity
- Tehnical Complexity.

From the fourth software attributes, DDD is the most suitable with an application that has a *complex business logic*. DDD is designed to tackle the complexity of business rules. The main goals of DDD are[1]:

- Placing the project's primary focus on the core domain and domain logic;
- Basing complex designs on a model of the domain;
- Initiating a creative collaboration between technical and domain experts to iteratively refine a conceptual model that addresses particular domain problems.

One of good resources about DDD is [DDD course by Vladimir Khorikov](https://www.pluralsight.com/courses/domain-driven-design-in-practice). I also cited a few/snippet code from his course to make my summary about DDD more clearer to understand.

# Terms in DDD
## Ubiquitous Languange
*Ubiquitous Languanges* brides the gab between developer and business expert/ domain expert/ subject matter expert (SME). The *Ubiquitous Languange* notion come up to avoid misunderstanding between them. For example, the developer has a class <code>Product</code> that represent business term *Product* (both product and package). On the other hand the business expert treat *Product* and *Package* as different things. By the condition, it is needed the same languange to avoid misunderstanding. 

## Bounded Context
Bounded Context is a central pattern in DDD. It is the focus of DDD's strategic design section which is all about dealing with large models and teams. DDD deals with large models by dividing them into different *Bounded Contexts* and being explicit about their interrelationships [Martin Fowler][8].

*Bounded Context* notion comes up to make clear boundries between different parts of the system. Let's say our system consist of *Sales* and *Support*, we can seperate Product into *Sales* and *Support* context to make clear boundries between the two.

{% img center /images/post/2021-03-20-fig00.png %}

<center><small>Figure 1: Bounded Contexts <br/>
        Source: https://martinfowler.com/bliki/BoundedContext.html</small>
</center>

<p/>

Figure 1 shows that *Bounded Context* aims for separating the model and explicitly drawn the boundaries between pieces. The reason for context separation is that as the application grows, it becomes harder to maintain a single unified model as it becomes larger and more people get involved in the development process. 

## Sub Domain
The difference between *Sub Domain* and *Bounded Context* is that the *Sub Domain* is a *Problem Space*. Meanwhile *Bounded Context* is a *Solution Space*. *Sub Domain* and *Bounded Context* have a **1-to-1** relation. It means that a *Sub Domain* should be covered with exactly a *Bounded Context*.

{% img center /images/post/2021-03-20-fig03.png %}

<center><small>Figure 2: Sub Domain - Bounded Contexts</small>
</center>

Since *Sub Domain* is a *Problem Space*, it should be defined by *Business Expert* or *Domain Experts*. The *Sub Domain* topic usually will come up when we have a talk with the *Domain Expert* while gathering requirement.

## Core Domain
The notion that focuses on *Domain Model* which is the most important part of the system.


# Onion Architecture
The implementation of DDD in onion architecture[3][5][6] showed in the Figure 3.

{% img center /images/post/2021-03-20-fig01.png %}

<center>
    <small>Figure 3: DDD in onion architecture </small>
</center>
<br/>

Figure 3 shows that the core of the architecture is the *Domain Model*. The *Domain Model* is isolated from others for seperation of consern purpose. It consists of *Entities, Value Objects, Domain Events,* and *Agregates*. They are most important part in DDD since they represent business logic implementation. 

The outer layer in the onion architecture depends on the inner one. The inner layer is isolated from the outer layer that make the inner layer does not know the outer one. Figure 3 denotes that the *Domain Model* does not know how it is peristed to database. Since persisting model to database is handled by *Repository* on the outer layer. 


# Testing in TDD
*Unit Test* is one of important part in software development. Test coverage that reaches 100% has high effort. In practice the most important that should cover most unit test is the code base *(Domain Model)* at the inner most layer in the onion architecture. The closer to 100%, the better the *Unit Test* coverage will be. Meanwhile for the outer layer, the test schenario could be coveraged by integration test.


# Entity vs Value Object
The difference between Entity and Value Object can be identified from several ways:

- Type of Equality
- Immutability
- Lifespan

## 1. Type of equality
*Type Equality* is applied to the objects which have the same type. It is classified into 3 categories:

- _Identifier Equality_<br/>
Two objects A and B are identified have *Identifier Equality* if they have the same *Id*.

- _Reference Equality_<br/>
Two objects A and B are identified have *Reference Equality* if they point to the same *memory address*.

- _Structural Equality_<br/>
Two objects A and B are identified have *Structural Equality* if *all the member values are matched*.

The 3 types of Type Equality is summarized in the following Figure. 

{% img center /images/post/2021-03-20-fig02.png %}

<center><small>Figure 4: Type Equality classification</small>
</center>

*Identifier Equality* usually referes to *Entity*. Meanwhile *Structural Equality* refers to *Value Object*. The *Reference Equality* can be applied to *Entity* or *Value Object*. In practice, *Value Object* does not has an Id.

## 2. Immutability
The characteristic of *Entity*: 

- Having identity *Id*
- Mutable

The characteristic of *Value Object*:

- Having no identity *Id*
- Immutable

## 3. Lifespan
From the lifespan point of view, *Value Object* can not live by its own. It can be owned by one or several identities.
For example <code>Address</code> object can not stay by its own. It should be belonged to <code>Person</code> object. From persistance perspective, *Value Object* does not have its own table in database.

# How to Recognize Entity or Value Object
It is not always clear having specific characteristic if a term or notion in a business process is an *Entity* or *Value Object*. It depends on the business process itself. A good approach to identify whether a notion of object is an *Entity* or *Value Object* is by comparing to *integer*[3].

```csharp

//Integer
public void MethodA(){
    int value1 = 5;
    int value2 = 5;
}

//Value Object
public void MethodB(){

    //money1 and money2 essentially the same. 
    //since they have the same nominal in business process context.
    //programmatically they both have the same structure equality
    Money money1 = new Money(5);
    Money money2 = new Money(5);
}
```

Ideally most business logic elements are identified as *Value Objects*. The *Entity* act as a wrapper of *Value Object(s)*. However, don't hesitate to refactor *Value Object* to *Entity* or vise versa if we identify it should be.


``` csharp Entity Base Class
    public abstract class Entity
    {
        public virtual long Id { get; protected set; }

        public override bool Equals(object obj)
        {
            var other = obj as Entity;

            if (ReferenceEquals(other, null))
                return false;

            if (ReferenceEquals(this, other))
                return true;

            if (GetRealType() != other.GetRealType())
                return false;

            if (Id == 0 || other.Id == 0)
                return false;

            return Id == other.Id;
        }

        public static bool operator ==(Entity a, Entity b)
        {
            if (ReferenceEquals(a, null) && ReferenceEquals(b, null))
                return true;

            if (ReferenceEquals(a, null) || ReferenceEquals(b, null))
                return false;

            return a.Equals(b);
        }

        public static bool operator !=(Entity a, Entity b)
        {
            return !(a == b);
        }

        public override int GetHashCode()
        {
            return (GetRealType().ToString() + Id).GetHashCode();
        }

        private Type GetRealType()
        {
            return NHibernateProxyHelper.GetClassWithoutInitializingProxy(this);
        }
    }
```

``` csharp Value Object base class
    public abstract class ValueObject<T>
        where T : ValueObject<T>
    {
        public override bool Equals(object obj)
        {
            var valueObject = obj as T;

            if (ReferenceEquals(valueObject, null))
                return false;

            return EqualsCore(valueObject);
        }


        protected abstract bool EqualsCore(T other);


        public override int GetHashCode()
        {
            return GetHashCodeCore();
        }


        protected abstract int GetHashCodeCore();


        public static bool operator ==(ValueObject<T> a, ValueObject<T> b)
        {
            if (ReferenceEquals(a, null) && ReferenceEquals(b, null))
                return true;

            if (ReferenceEquals(a, null) || ReferenceEquals(b, null))
                return false;

            return a.Equals(b);
        }


        public static bool operator !=(ValueObject<T> a, ValueObject<T> b)
        {
            return !(a == b);
        }
    }
```

# Agregate
*Aggregate* is a pattern in DDD. It is a cluster of domain objects that can be treated as a single unit[7]. *Aggregate* is an encapsulation of *Entities* and/or *Value Objects* (domain objects). An *Entity* can belong to a single *Agregate* only. Meanwhile a *Value Object* can belong to multiple *Agregates*. 

An *Agregate* contains a set of operations which those domain objects can be operated on. An *Agregate* also act as a single operation unit. Application layer should reload it from database, then perform action and store it back as a single object. Hence, the *Agregate* should not be too large. Commonly it contains maximum 3 *Entities*. On the contrary to *Entity*, we can have as many *Value Objects* in an *Agregate* as we want.

``` csharp Agregate Root
    public abstract class AggregateRoot : Entity
    {
        
    }
```

``` csharp Repository for Database Operation
    public abstract class Repository<T> where T : AggregateRoot
    {
        public T GetById(long id)
        {
            throw new NotImplementedException("load data from database");
        }

        public void Save(T aggregateRoot)
        {
            throw new NotImplementedException("operation insert to database");
        }
    }

```

## Domain Event
*Domain Event* represents an event that's significant to the *Domain Model*. It's important to distinguish *Domain Event* to *System Event*. *System Event* deals with infrastructure event such as button click, timer tick, window close etc. On the other hand, *Domain Event* describe occasion which is important to the *Domain*. For example, when a button clicked *(System Event)*, it calls a domain operation. Domain operation trigger an event to change balance values in other *Bounded Context* for example head office. 

*Domain Event* is often used to[3]:

- Decouple *Bounded Context*
- Facilitate communication between *Bounded Context*
- Decouple *Entities* within a *Bounded Context*

The guidelines in implementing *Domain Event* are[3]:

- Naming should be in *Past Tense*. Example: <code>BalanceChangedEvent</code>
- Passing data as small/little as possible. Don't pass data/information more than needed. 
- We should not pass *Entity* to an *Event*. Since it will produce additonal point of coupling. We should use primitive data type instead.

# References
1. https://en.wikipedia.org/wiki/Domain-driven_design
2. [Domain Driven Design: Tackling Complexity in the Heart of Software](https://www.amazon.com/gp/product/0321125215)
3. https://www.pluralsight.com/courses/domain-driven-design-in-practice
4. https://www.baeldung.com/spring-data-ddd
5. https://www.baeldung.com/spring-boot-clean-architecture
6. https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/
7. https://martinfowler.com/bliki/DDD_Aggregate.html
8. https://martinfowler.com/bliki/BoundedContext.html
