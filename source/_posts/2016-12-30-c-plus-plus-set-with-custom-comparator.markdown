---
layout: post
title: "C++ Set with Custom Comparator"
date: 2016-12-30 09:42:04 +0700
comments: true
categories: [cpp]
---

{% img left /images/logo/cpp-logo.png %}

<code>std::set</code> is a C++ STL container that store unique elements following a specific order. It is defined in the <code>set</code> header file.
<br/>

Benefits and Features of <code>std::set</code>[3]:
<ol>
<li>It’s doesn’t allow duplicate elements i.e. it only contains unique elements</li>
<li>
<code>std::set</code> can contain element of any specified type in template argument
</li>
<li>
<code>std::set</code> internally store elements in balanced binary tree
</li>
<li>
 By default <code>std::set</code> uses the operator <code> < </code> for comparing two elements and but if user passes the external sorting criteria i.e. comparator then it uses it instead of default operator <code> < </code>.
</li>
<li>
<code>std::set</code> will keep the inserted elements in sorted order based on the assigned sorting criteria i.e. either by default criteria operator <code> < </code> or by passed comparator (if passed).
</li>

In this post the samples only limited to <code>std::set</code> that use custom comparator and store complex object instead of basic data type. The complex object that we will use is reusing class <code>Person</code> on previous post.

Let's create custom class that handles comparation process

``` c++ CustomCompare

#ifndef CustomCompare_h
#define CustomCompare_h

#include "Person.hpp"

struct CustomCompare
{
    bool operator()(const int& lhs, const int& rhs)
    {
        return lhs < rhs;
    }

    bool operator()(const Person& lhs, const Person& rhs)
    {
        return lhs.getAge() < rhs.getAge();
    }
};

#endif /* CustomCompare_h */

```

The following code is an example how to use the comparator class in <code>std:set</code>
``` c++ sample how to use
void SampleSetWithCustomCompare()
{
    set<Person,CustomCompare> setOfPersons;

    setOfPersons.insert(Person("Person 1", 25));
    setOfPersons.insert(Person("Person 2", 16));
    setOfPersons.insert(Person("Person 3", 28));
    setOfPersons.insert(Person("Person 4", 9));

    for(set<Person,CustomCompare>::iterator it = setOfPersons.begin(); it!=setOfPersons.end(); ++it)
    {
        cout << it->getName() << " , age : " << it->getAge()<< endl;
    }

}
```

## References
1. http://www.cplusplus.com/reference/set/set/
2. http://www.wrox.com/WileyCDA/WroxTitle/Professional-C-2nd-Edition.productCd-0470932449.html
3. http://thispointer.com/stdset-tutorial-part-1-set-usage-details-with-default-sorting-criteria/
