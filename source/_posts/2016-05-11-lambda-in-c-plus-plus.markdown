---
layout: post
title: "Lambda in C++"
date: 2016-05-11 23:15:18 +0700
comments: true
categories: [cpp]
---

{% img left /images/logo/lambda.png %}
C++11 adds a new feature called lambda expressions. This allows us to write anonymous functions inline, removing the need to write a separate function or to write a function object, and makes code easier to understand.
For those who are familiar with C# lambda expression, C++ lambda expression is similar. But lambda in C++ has slightly different syntax.

``` text C++11 Lambda Syntax
[capture_block](parameters) mutable exception_specification -> return_type {body}
```
<ol>
<li> <strong><code>capture_block</code></strong> is a comma-separated list of zero or more captures, optionally beginning with a capture-default. Capture list can be passed as follows (see below for the detailed description):
<ol>
<li><code>[a,&b]</code> where a is captured by value and b is captured by reference.</li>
<li><code>[this]</code> captures the this pointer by value</li>
<li><code>[&]</code> captures all automatic variables odr-used in the body of the lambda by reference</li>
<li><code>[=]</code> captures all automatic variables odr-used in the body of the lambda by value</li>
<li><code>[x]</code> captures only x by value and nothing else.</li>
<li><code>[]</code> captures nothing</li>
</ol>
</li>
<li>
<strong><code>parameters</code></strong> is (optional) list of parameters for the lambda expression.
</li>
<li>
<strong><code>mutable</code></strong> is (optional) if variables from the enclosing scope are captured by value, a copy of those variables will become available in the body of the lambda expression. Those copies are marked as const by default, meaning the lambda body cannot change the value of those copies. If the lambda expression is marked as mutable, those copies are not const and the body can modify those local copies.
</li>
<li>
<strong><code>exception_specification</code></strong> is (optional) and can be used to specify which exceptions the body of the lambda expression can throw.
</li>
<li>
<strong><code>return_type</code></strong> is Return type. If not present it's implied by the function return statements (or void if it doesn't return any value)
</li>
</ol>

<h3>Lambda and Direct Invoke</h3>
The following snipped code is an example of lambda expression.

``` cpp example lambda and directly invoked

string result = [](const string& str) -> string {return "Hi " + str;}("neutro");
cout << result << endl;

```
The lambda expression above shows that it has a <code>const string&</code> parameter and return a <code>string</code> type. To execute the lambda is by placing round brackets <code>()</code> and put object that will be passed to inside it. The <code>"neutro"</code> literal string will substitute <code>str</code> parameter. The output of the above code is :

``` text output
Hi neutro
```

<h3>Lambda as Variable</h3>

Pointer to a lambda expression can be stored and executed through the function pointer. C++ provides <code>std::function</code> which is a STL template class that provides a very convenient wrapper to a simple function, to a functor or to a lambda expression. We can also use the C++11 <code>auto</code> keyword to make it easier.

To make it clear the following code contains couples of various lambda expressions and store them in variables of type <code>auto</code> or its equivalent in <code>std::function</code> STL template class.

``` cpp lambda as variable
    auto lambda1 = [] { cout <<"Hello lambda without parameter 1" << endl; };
    lambda1();

    auto lambda2 = [](void) { cout <<"Hello lambda without parameter 2" << endl; };
    lambda2();

    //auto lambda3 = [](void) -> void { cout <<"Hello lambda without parameter 2" << endl; };
    function<void()> lambda3 = [](void) -> void { cout <<"Hello lambda without parameter 2" << endl; };
    lambda3();

    //auto lambda4 = [](const string& str) -> string {return "Hello from " + str;};
    function<string(const string&)> lambda4 = [](const string& str) -> string {return "Hello from " + str;};

    string result = lambda4("neutro");

    cout << result << endl;
```
The snipped code above shows how lambda expressions stored in variable of auto type or theirs equivalent in <code>std::function</code> type and how to invoke the lambda. <a href="https://oopscenities.net/2012/02/24/c11-stdfunction-and-stdbind/">Here</a> is a good article about <code>std::function</code>.

<h3>Lambda Capture Block</h3>
Lambda Capture Block basically describes how we want to capture variables from the enclosing scope. Capturing a variable means that the variable becomes available inside the body of the lambda. The detail variant of capture block has been described on previous section. The following code shows various scenario how to play with capture block.

``` cpp lambda capture block
int variableA = 1;
int variableB = 1;
int variableC = 1;

cout << "Init Values : "<<endl;
cout << "variableA : "<<variableA <<endl;
cout << "variableB : "<<variableB <<endl;
cout << "variableC : "<<variableC <<endl;

cout << endl;
//--------------------------------------
cout << "---## lamda 1 ##---"<<endl;
auto lambda1 = [=](int param)-> int     //==> [=] lambda has default access to variable is by value
            {
                param = param * 2;

                cout << "param inside lambda1 : "<<param << endl;
                return param;
            };

lambda1(variableA);
cout << "After lamda1, variableA : " <<variableA <<endl;

cout<<endl;

//--------------------------------------------
cout << "---## lamda 2 ##---"<<endl;
auto lambda2 = [&](int param) -> int      //==> [&] lambda has default access to variable is by reference
            {
                param = param * 2;
                variableB = variableB * 2;

                cout << "param inside lambda2 : "<<param << endl;
                cout << "variableB inside lambda2 : "<<variableB << endl;

                return param;
            };

lambda2(variableA);
cout << "After lamda2, variableA : " <<variableA <<endl;
cout << "After lamda2, variableB : " <<variableB <<endl;

cout<<endl;

//-------------------------------------
cout << "---## lamda 3 ##---"<<endl;
auto lambda3 = [&](int* param) -> int
{
    *param = (*param) * 2;
    variableB = variableB * 2;

    cout << "param inside lambda3 : "<<*param << endl;
    cout << "variableB inside lambda3 : "<<variableB << endl;

    return *param;
};

int result3 = lambda3(&variableA);
cout << "After lamda3, result3 : " <<result3 <<endl;
cout << "After lamda3, variableA : " <<variableA <<endl;
cout << "After lamda3, variableB : " <<variableB <<endl;

cout<<endl;


//--------------------------------
cout << "---## lamda 4 ##---"<<endl;
auto lambda4 = [=, &variableB, &variableC](int param) -> int //==> [=, &variableB, &variableC] captures by value by default,
                                             // except variables variableB and variableC, which are captured by reference.
{
    param = (param) * 2;
    variableB = variableB * 2;
    variableC = variableC * 2;

    cout << "param inside lambda4 : "<<param << endl;
    cout << "variableB inside lambda4 : "<<variableB << endl;
    cout << "variableC inside lambda4 : "<<variableC << endl;

    return param;
};

lambda4(variableA);

cout << "After lamda4, variableA : " <<variableA <<endl;
cout << "After lamda4, variableB : " <<variableB <<endl;
cout << "After lamda4, variableC : " <<variableC <<endl;

cout << endl;

//--------------------------------
cout << "---## lamda 5 ##---"<<endl;
auto lambda5 = [variableC](int param) mutable -> int //==> [variableC] captures only variableC by value and nothing else.
{
    param = (param) * 2;

    variableC = variableC * 2; //should mark as mutable lambda, since Cannot assign to a variable captured by copy in a non-mutable lambda
    //variableB = variableB * 2; //ERROR : variableB not captured, since only capture specific variable that's variableC

    ///
    cout << "param inside lambda5 : "<<param << endl;
    cout << "variableC inside lambda5 : "<<variableC << endl;

    return param;
};

lambda5(variableA);

cout << "After lamda5, variableA : " <<variableA <<endl;
cout << "After lamda5, variableC : " <<variableC <<endl;

```

The output of the sample code is :

``` text output
Init Values :
variableA : 1
variableB : 1
variableC : 1

---## lamda 1 ##---
param inside lambda1 : 2
After lamda1, variableA : 1

---## lamda 2 ##---
param inside lambda2 : 2
variableB inside lambda2 : 2
After lamda2, variableA : 1
After lamda2, variableB : 2

---## lamda 3 ##---
param inside lambda3 : 2
variableB inside lambda3 : 4
After lamda3, result3 : 2
After lamda3, variableA : 2
After lamda3, variableB : 4

---## lamda 4 ##---
param inside lambda4 : 4
variableB inside lambda4 : 8
variableC inside lambda4 : 2
After lamda4, variableA : 2
After lamda4, variableB : 8
After lamda4, variableC : 2

---## lamda 5 ##---
param inside lambda5 : 4
variableC inside lambda5 : 4
After lamda5, variableA : 2
After lamda5, variableC : 2
```
<h4>Reference<h4>
<ol type="1">
<li><a href="http://www.wrox.com/WileyCDA/WroxTitle/Professional-C-2nd-Edition.productCd-0470932449.html">Wrox Professional C++</a></li>
<li>http://en.cppreference.com/w/cpp/language/lambda</li>
<li>https://oopscenities.net/2012/02/24/c11-stdfunction-and-stdbind/</li>
<li>http://www.drdobbs.com/cpp/lambdas-in-c11/240168241?pgno=1</li>
</ol>
