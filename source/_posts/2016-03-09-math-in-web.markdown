---
layout: post
title: "Math in Web"
date: 2016-03-09 22:41:47 +0800
comments: true
categories: [mathematic, javascript]
---
There are several frameworks that can be used to display math equation on web page. Usually math equestion written in <a href="https://en.wikipedia.org/wiki/LaTeX">LaTeX</a> format. <a href="https://en.wikipedia.org/wiki/LaTeX">LaTeX</a> is used for the communication and publication of scientific documents in many fields, including mathematics, physics, computer science, statistics etc.

<a href="https://www.mathjax.org/">MathJax</a> and <a href="https://khan.github.io/KaTeX/">KaTeX</a> are two popular javascript frameworks that can render LaTeX expression to beautiful math equation in browser.

The following are examples math LaTeX expression rendered with <a href="https://khan.github.io/KaTeX/">KaTeX</a>.
To use <a href="https://khan.github.io/KaTeX/">KaTeX</a> we just need to include <code>katex.min.css</code> and <code>katex.min.js</code>

``` html KaTeX
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.js"></script>

<div>
<p><span class="katex" id="eq1"></span></p>
<p><span class="katex" id="eq2"></span></p>
<p><span class="katex" id="eq3"></span></p>
<p><span class="katex" id="eq4"></span></p>
<p><span class="katex" id="eq5"></span></p>
<p><span class="katex" id="eq6"></span></p>
<p><span class="katex" id="eq7"></span></p>
</div>

```
``` javascript KaTeX sample
katex.render("c = \\pm\\sqrt{a^2 + b^2}", eq1);
katex.render("f(a,b,c) = (a^2+b^2+c^2)^3", eq2);

katex.render("\\int u \\frac{dv}{dx}\\,dx=uv-\\int \\frac{du}{dx}v\\,dx", eq3);

katex.render("f(x) = \\int_{-\\infty}^\\infty \\hat f(\\xi)\\,e^{2 \\pi i \\xi x}", eq4);
katex.render("\\oint \\vec{F} \\cdot d\\vec{s}=0", eq5);

katex.render("\\left( \\sum_{k=1}^n a_k b_k \\right)^2 \\leq \\left( \\sum_{k=1}^n a_k^2 \\right) \\left( \\sum_{k=1}^n b_k^2 \\right)", eq6);
```


<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.js"></script>



<div>
    <p><span class="katex" id="eq1"></span></p>
    <p><span class="katex" id="eq2"></span></p>
    <p><span class="katex" id="eq3"></span></p>
    <p><span class="katex" id="eq4"></span></p>
    <p><span class="katex" id="eq5"></span></p>
    <p><span class="katex" id="eq6"></span></p>
    <p><span class="katex" id="eq7"></span></p>
</div>
<script>

    katex.render("c = \\pm\\sqrt{a^2 + b^2}", eq1);
    katex.render("f(a,b,c) = (a^2+b^2+c^2)^3", eq2);

    katex.render("\\int u \\frac{dv}{dx}\\,dx=uv-\\int \\frac{du}{dx}v\\,dx", eq3);

    katex.render("f(x) = \\int_{-\\infty}^\\infty \\hat f(\\xi)\\,e^{2 \\pi i \\xi x}", eq4);
    katex.render("\\oint \\vec{F} \\cdot d\\vec{s}=0", eq5);

    katex.render("\\left( \\sum_{k=1}^n a_k b_k \\right)^2 \\leq \\left( \\sum_{k=1}^n a_k^2 \\right) \\left( \\sum_{k=1}^n b_k^2 \\right)", eq6);
</script>

The comparison between <a href="https://www.mathjax.org/">MathJax</a> and <a href="https://khan.github.io/KaTeX/">KaTeX</a> can be found <a href="http://www.intmath.com/cg5/katex-mathjax-comparison.php">here</a>. At the time of writing this blog post, I got <a href="https://khan.github.io/KaTeX/">KaTeX</a> is faster than <a href="https://www.mathjax.org/">MathJax</a>. But <a href="https://khan.github.io/KaTeX/">KaTeX</a> can not render a few expressions that <a href="https://www.mathjax.org/">MathJax</a> can.

The comparison results could be changed in the next. Since both of them are actively developed.
