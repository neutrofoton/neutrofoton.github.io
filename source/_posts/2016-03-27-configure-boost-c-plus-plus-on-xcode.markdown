---
layout: post
title: "Configure Boost ( C++ Libraries ) on Xcode, Code::Blocks and Visual Studio"
date: 2016-03-27 21:38:49 +0800
comments: true
categories: [xcode, code block, visual studio, osx, linux, windows, c++]
---

{% img left /images/post/2016-03-27-boost.png %}
<a href="http://www.boost.org/">Boost</a> is a set of libraries for the C++ programming language that provide support for tasks and structures such as linear algebra, pseudorandom number generation, multithreading, image processing, regular expressions, and unit testing. It contains over eighty individual libraries.<a href="https://en.wikipedia.org/wiki/Boost_(C%2B%2B_libraries)">[2]</a>

The other interesting points of <a href="http://www.boost.org/">Boost</a> are :
<ol>
<li>Open source</li>
<li>Cross platform</li>
<li>Complement to STL rather than a replacement</li>
<li>Many of <a href="http://www.boost.org/">Boost</a> developers are on the C++ standard committee</li>
<li>Well documented</li>
<li>Most of the Boost libraries are licensed under the <a href="https://en.wikipedia.org/wiki/Boost_(C%2B%2B_libraries)#License">Boost Software License</a>, designed to allow <a href="http://www.boost.org/">Boost</a> to be used with both free and proprietary software projects</li>
</ol>

<h2>Installation Boost</h2>
Before jumping into steps of configuring <a href="http://www.boost.org/">Boost</a> on various IDE, let's begin with <a href="http://www.boost.org/">Boost</a> installation. To be noted that on this post I run Xcode on OS X, <a href="http://www.codeblocks.org/">Code::Blocks</a>  on Linux (Ubuntu) and Visual Studio on Windows. The detail environments I use are :
<ol>
<li>OS X 10.11.4 El Capitan</li>
<li>Ubuntu 14.04.4 LTS</li>
<li>Xcode Version 7.2</li>
<li>Code::Blocks 13.12, gcc 4.8.4</li>
<li>Visual Studio 2013</li>
<li>Boost 1.60.0</li>
</ol>


<h4>OS X and Linux (Ubuntu)</h4>
There are several ways of <a href="http://www.boost.org/">Boost</a> installation. Instead of build from source code, we can use package manager such as <a href="https://www.macports.org/">MacPorts</a>, <a href="http://brew.sh/">Homebrew</a>, <a href="https://en.wikipedia.org/wiki/Advanced_Packaging_Tool">Advance Package Tool</a>, etc. In this post we will build <a href="http://www.boost.org/">Boost</a> from source code. The installation steps (from source code) on OS X and Ubuntu are the similar. To make it consistent, I use the same installation path for OS X and Ubuntu that is <code>/usr/local/boost_1_60_0</code>. You can use different path if you want. The steps are :
<ol>
<li>Download boost library from <a href="boost.org">Boost website</a></li>
<li>Extract it.</li>
<li>Open terminal, navigate to the extracted directory</li>
<li>Create directory on <code>/usr/local/boost_1_60_0</code>, and ensure IDE has access to the directory. On my case I don't need this step on OS X, but on ubuntu it does.
``` bash
sudo mkdir /usr/local/boost_1_60_0
sudo chmod 777 -r boost_1_60_0
```
</li>
<li>
Run command :
``` bash
./bootstrap.sh --prefix=/usr/local/boost_1_60_0
./b2 install
```
This last step quite take time. So you can have coffee while waiting for it :)
</li>
</ol>

Once the installation finish, we should have generated directory. They are <code>/usr/local/boost_1_60_0/include</code> contains header files and <code>/usr/local/boost_1_60_0/lib</code> contains libraries.

<h4>Windows</h4>
The <a href="http://www.boost.org/">Boost</a> installation step on Windows is also similar to the installation step on OS X and Ubuntu.
The steps are :
<ol>
<li>Download boost library from <a href="boost.org">Boost website</a></li>
<li>Extract it to C:\\boost_1_60_0 </li>
<li>Open Visual Studio command prompt. I use Visual Studio 2013 x86 Native Tools Command Prompt native tool (I have not test using default Windows Command Prompt)
``` bat
C:\> cd C:\boost_1_60_0
C:\boost_1_60_0> bootstrap.bat
C:\boost_1_60_0> .\b2
```
As on OS X and Ubuntu, the last step quite take time.
</li>
</ol>

<h2>Configure <a href="http://www.boost.org/">Boost</a> on IDE(s)</h2>
Before create C++ projects on various IDE, let's create a simple C++ hello world code that use Boost libraries. To simplify the test, I grab sample code from <a href="http://stackoverflow.com/questions/999120/c-hello-world-boost-tee-example-program">here</a>   
``` cpp Hello World
#include <iostream>
#include <fstream>
using namespace std;

#include <boost/iostreams/tee.hpp>
#include <boost/iostreams/stream.hpp>
using namespace boost;
using namespace boost::iostreams;

int main(int argc, const char * argv[]) {

    typedef tee_device<ostream, ofstream> TeeDevice;
    typedef stream<TeeDevice> TeeStream;

    ofstream ofs("/Users/neutro/Workspace/cpp/sample.txt");

    TeeDevice my_tee(cout, ofs);
    TeeStream my_split(my_tee);

    my_split << "Hello, World!\n";

    my_split.flush();
    my_split.close();

    return 0;
}
```
The snipped code above just print text and write it to a text file. We just want to ensure the IDE's compiler can compile and build the code that includes <a href="http://www.boost.org/">Boost</a> libraries.

<ul>
  <li>
    <h4>Xcode</h4>
    To include Boost libraries on Xcode project :
    <ol>
    <li>Select Xcode project > Build Setting</li>
    <li>Add <code>/usr/local/boost_1_60_0/include/</code> to the Header Search Paths</li>
    <li>Add <code>/usr/local/boost_1_60_0/lib/</code> to the Library Search Paths</li>
    </ol>

    {% img center /images/post/2016-03-27-xcode.png %}

  </li>
  <li>
    <h4>Code::Blocks</h4>
    To include Boost libraries on Code::Blocks project :
    <ol>
    <li>Right Click on Code::Blocks project > Build Option</li>
    <li>
    Select Compiler tab, add <code>/usr/local/boost_1_60_0/include/</code>
    {% img center /images/post/2016-03-27-codeblocks1.png %}
    </li>
    <li>
    Select Linker tab, add <code>/usr/local/boost_1_60_0/lib/</code>
    {% img center /images/post/2016-03-27-codeblocks2.png %}
    </li>
    </ol>
  </li>
  <li>
    <h4>Visual Studio</h4>
    To include Boost libraries on Visual C++ project :
    <ol>
    <li>Right Click on VC++ project > Properties</li>
    <li>Select VC++ Directories on the left pane</li>
    <li>Add <code>C:\boost_1_60_0</code> on Include Directories item</li>
    <li>Add <code>C:\boost_1_60_0\stage\lib</code> on Include Directories item</li>
    <li>Click OK to close the dialog</li>
    </ol>
    {% img center /images/post/2016-03-27-vs.png %}
  </li>
</ul>

The last is rebuild the above code on selected IDE. We should not got any errors once the IDE can detect the <a href="http://www.boost.org/">Boost</a> directory path.
 
<h4>Reference<h4>
<ol type="1">
<li>
http://www.boost.org/
</li>
<li>
<a href="https://en.wikipedia.org/wiki/Boost_(C%2B%2B_libraries)">Wikipedia</a>
</li>
</ol>
