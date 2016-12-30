---
layout: post
title: "C++ Priority Queue with Comparator"
date: 2016-12-29 14:52:23 +0700
comments: true
categories: [cpp]
---

A <code>priority_queue</code> is categorized as a STL container adaptor. It is like a queue that keeps its element in sorted order. Instead of a strict FIFO ordering, the element at the head of the queue at any given time is the one with the highest priority.

The template class definition of <code>priority_queue</code> is as follow

``` c++ template definition
template <
   class Type,
   class Container=vector<Type>,
   class Compare=less<typename Container::value_type> >
class priority_queue

```

A user-provided compare can be supplied to change the ordering, e.g. using <code>std::greater<T></code> would cause the smallest element to appear as the top(). We also can create custom comparator for our need.

Many samples available on net about <code>priority_queue</code> with default compare parameter. In this article let's create samples by specifying the compare parameter template.

``` c++ priority_queue with std::greater

//helper function displays sorted data
template<class T>
void printQueue(T& q)
{
    while (!q.empty())
    {
        cout << q.top() << endl;
        q.pop();
    }
}

void SamplePriorityQueue()
{
    std::priority_queue<int, std::vector<int>, std::greater<int> > q;

    for(int n : {1,8,5,6,3,4,0,9,7,2})
        q.push(n);

    printQueue(q);
}

```

The code above uses <code>std::greater</code> as a compare parameter template.

``` bash output
0
1
2
3
4
5
6
7
8
9
```

Beside the <code>std::less</code> or <code>std::greater</code>, we can create our custom comparator with lamda or custom class or struct.

``` c++ lamda as compare parameter

void SamplePriorityQueueWithLamda()
{
    // using lambda to compare elements.
    auto compare = [](int lhs, int rhs)
                {
                    return lhs < rhs;
                };

    std::priority_queue<int, std::vector<int>, decltype(compare)> q(compare);

    for(int n : {1,8,5,6,3,4,0,9,7,2})
        q.push(n);


    printQueue(q);
}

```

To use the custom comparator, we just need to pass it as the third parameter of <code>priority_queue</code> template

``` c++ custom comparator

struct CustomCompare
{
    bool operator()(const int& lhs, const int& rhs)
    {
        return lhs < rhs;
    }
};

```

``` c++ sample with custom comparator
void SamplePriorityQueueWithCustomComparator()
{
    priority_queue<int,vector<int>, CustomCompare > pq;

    pq.push(3);
    pq.push(5);
    pq.push(1);
    pq.push(8);

    printQueue(pq);
}

```

The data stored in <code>priority_queue</code> is not limited to basic data type.
We can store object in it. Let's create a sample of it.
Let's say we have a <code>Person</code> class.

``` c++ Person.hpp

#ifndef Person_hpp
#define Person_hpp

#include <stdio.h>
#include <string>

using namespace std;

class Person
{
public:
    Person();
    Person(string name, int age);
    virtual ~Person();

    string getName() const;
    int getAge() const;

    friend bool operator < (const Person& lhs, const Person& rhs);
    friend bool operator > (const Person& lhs, const Person& rhs);

private:
    string name;
    int age;
};

#endif /* Person_hpp */

```

``` c++ Person.cpp
#include "Person.hpp"

bool operator < (const Person& lhs, const Person& rhs)
{
    return lhs.getAge() < rhs.getAge();
}

bool operator > (const Person& lhs, const Person& rhs)
{
    return lhs.getAge() > rhs.getAge();
}

Person::Person()
{
}

Person::Person(string name, int age):name(name), age(age)
{
}

Person::~Person()
{   
}

string Person::getName() const
{
    return name;
}

int Person::getAge() const
{
    return age;
}

```
On the <code>Person</code> class, we have friend overloading methods, right angle bracket and left angle bracket. The methods act as comparation operator. The operator overloading is needed if we want to use <code>std::less</code> or <code>std::greater</code>.

``` c++ sample priority_queue stores object

void SamplePriorityQueueStoreObject()
{
    vector<Person> personVector =
    {
        Person("Person 1", 25),
        Person("Person 2", 17),
        Person("Person 3", 35),
        Person("Person 4", 7),
        Person("Person 5", 50)
    };

    cout << "======== Less Priority Queue ======= " << endl;

    priority_queue<Person, vector<Person>, less<vector<Person>::value_type>> pqueue_less;

    //fill pqueue_less
    for (auto it = personVector.cbegin(); it!=personVector.cend(); it++)
    {
        pqueue_less.push(*it);
    }

    //iterate,display and pop
    while (!pqueue_less.empty())
    {
        Person value = pqueue_less.top();
        cout << value.getName() << " : " << value.getAge() << endl;

        pqueue_less.pop();
    }


    cout << endl << endl;

    cout << "======== Greater Priority Queue ======= " << endl;

    priority_queue<Person, vector<Person>, greater<vector<Person>::value_type>> pqueue_greater;
    //fill pqueue_greater
    for (auto it = personVector.cbegin(); it!=personVector.cend(); it++)
    {
        pqueue_greater.push(*it);
    }

    //iterate,display and pop
    while (!pqueue_greater.empty())
    {
        Person value = pqueue_greater.top();
        cout << value.getName() << " : " << value.getAge() << endl;

        pqueue_greater.pop();
    }
}

```

## References
1. https://support.microsoft.com/en-us/kb/837697
2. http://en.cppreference.com/w/cpp/container/priority_queue
