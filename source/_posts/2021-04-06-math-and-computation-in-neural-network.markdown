---
layout: post
title: "Math and Computation in Neural Network"
date: 2021-04-06 08:10:48 +0700
comments: true
categories: [mathematic, machine learning]
---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.5.1/katex.min.js"></script>


An Artificial Neural Network (ANN) is a series of algorithms that endeavors to recognize underlying relationships in a set of data through a process that mimics the way the human brain operates[1]. Neuron typically has a weight that adjusts as learning proceeds. This article focuses on the basic mathematical and computation process in the ANN.

# Artificial Neuron
An artificial neuron is a mathematical function that models biological neurons. A Neuron can receive input from other neurons. The neuron inputs are weighted with <span class="katex" id="omega1"></span> and summed (<span class="katex" id="sigma1"></span>) them before being passed through into an activation function. Figure 1 shows the structure of an artificial neuron.


<img class="center" src="/images/post/2021-04-06-neuron.png" width="70%" height="70%">

<center><small>Figure 1: Artificial Neuron <br/>
        </small>
</center>
<br/>
ANN is a supervised learning that its operation involves a dataset. The dataset is labeled and split into two part at least, namely training and testing. We have expected output (from the label) for each input. During the training process, the parameter weights and biases are updated in order to achieve a result that is close to the labeled data. To identify model performance, we evaluate the model to the testing dataset to verify how good the trained model is. To get more detail how ANN learn, let begin with the last two neurons connection.

<img class="center" src="/images/post/2021-04-06-simple-neuron-connection.png" width="40%" height="40%">

<center><small>Figure 2: Simple Two Neuron Connection<br/>
        </small>
</center>

<span  class="eq-expression katex" id="symbol_activation_neuron_a" ></span> is a neuron with an activation <br/>
<span  class="eq-expression katex" id="symbol_layer_L" ></span> is an index of layer. It is not an exponential.

<script>
    katex.render("a ", symbol_activation_neuron_a);
    katex.render("L ", symbol_layer_L);
</script>

In the training phase, the sample data is fed through the ANN. The outcome of the ANN is inspected and compared to the expected result (the label). The difference between the outcome and expected result is called Cost/Error/Loss. There are several cost functions can be used to evaluate the cost in analyzing model performance. One of the common ones is the quadratic cost function.

The cost for the network in Figure 2 in form of quadratic cost function is:

<div class="eq">
        <span  class="eq-expression katex" id="cost_formula_last_layer_one_neuron" ></span>
        <span class="eq-no">Eq (1)</span>
</div>

The total cost of a feed through the ANN are:
<div class="eq">
        <span  class="eq-expression katex" id="cost_formula_last_layer_all_neuron" ></span>
        <span class="eq-no">Eq (2)</span>
</div>


Equation (1) and (2) show that the closer the outcome of ANN to the expected result, the smaller the cost will be. The cost can be thought as a mathematical function of weights and biases  <span  class="eq-expression katex" id="cost_as_func_of_weight_bias" ></span>. Hence, the fittest model can be achieved by minimizing the cost to find the suitable weights and biases.

<script>
        katex.render("C_{0} = (a^{(L)} - y)^2", cost_formula_last_layer_one_neuron);
        katex.render("Cost (C) = \\sum_j (a_j - y_j)^2", cost_formula_last_layer_all_neuron);
        katex.render("C(\\omega_1, b_1, \\omega_2, b_2, \\omega_3, b_3 )", cost_as_func_of_weight_bias);
</script>

# Gradient Decent

<img class="center" src="/images/post/2021-04-06-derivative-direction.png" width="40%" height="40%">

<center><small>Figure 3: Gradient vector in the direction of vector U<br/>
        </small>
</center>
<br/>
In multivariable calculus, gradient of a function presented as <span  class="eq-expression katex" id="grad_f" ></span>. Gradient of a function <span  class="eq-expression katex" id="func_f" ></span> in the direction of vector <span  class="eq-expression katex" id="vector_u" ></span> in Figure 3 expressed as:



<div class="eq">
        <span  class="eq-expression katex" id="grad_f_direction_u" ></span>
        <span class="eq-no">Eq (3)</span>
</div>

If <span  class="eq-expression katex" id="vector_u_1" ></span> is a unit vector, the <span  class="eq-expression katex" id="vector_u_length" ></span>. Thus, the Equation (3) can be written as


<div class="eq">
        <span  class="eq-expression katex" id="grad_f_direction_u_unit" ></span>
        <span class="eq-no">Eq (4)</span>
</div>

The maximum result of Equation (4) gained at <span  class="eq-expression katex" id="phi_equal_0" ></span>. Since the <span  class="eq-expression katex" id="vector_u_2" ></span> is a unit vector, it can be evaluated from <span  class="eq-expression katex" id="grad_f_1" ></span> as


<div class="eq">
        <span  class="eq-expression katex" id="u_unit_from_grad_f" ></span> <br/><br/>
        <span  class="eq-expression katex" id="grad_f_direction_its_self" ></span>
</div>

The minimum slope / steepest descent is gained at the opposite of the steepest ascent (at <span  class="eq-expression katex" id="phi_equal_pi" ></span>)

<div class="eq">
        <span  class="eq-expression katex" id="u_unit_from_grad_f_direction_opposite" ></span> <br/><br/>
        <span  class="eq-expression katex" id="grad_f_direction_its_self_direction_opposite" ></span>
</div>

Since ANN is essentially minimizing the cost function, the gradient descent is used as basic idea to find the best fit ANN parameters (weights and biases) in the learning process.

<div class="eq">
        <span  class="eq-expression katex" id="weight_update" ></span>
        <span class="eq-no">Eq (5)</span>
        
</div>

<div class="eq">
        <span  class="eq-expression katex" id="weight_update_matrix" ></span>
</div>

The same method also implemented to get bias update.
<div class="eq">
        <span  class="eq-expression katex" id="bias_update" ></span>
        <span class="eq-no">Eq (6)</span>
</div>
<span  class="eq-expression katex" id="eta_1" ></span> is a learning rate. It is an additional value added how much the gradient vector we use to change the current value of weights and biases to the new ones. If <span  class="eq-expression katex" id="eta_2" ></span> is too small, adjustment of weight will slow. The convergency to local minimum will longer. However, if it is too high, the search of local minimum might be oscillated or reach overshoot. Ilustration of gradient descent in 3 dimention plot ilustrated in the Figure 4. Of course it is hard to plot the gradient descent that cover all weights of the ANN.

<img class="center" src="/images/post/2021-04-06-gradient-descent-ilustration.png" width="70%" height="70%">

<center><small>Figure 4: Gradient descent ilustration<br/>
        </small>
</center>
<br/>
Iterative training will let the cost function gradient move in the direction of green line to the local minimum as ilustrated in Figure 4. In fact, gaining global minimum is the ideal condition. However it is very difficult since we start the movement from a random location (random weight at initial training). Reaching local minimum is acceptable in finding the optimal model. If we want a better model, we can retrain the ANN by generating new random or setting different initial values of weights and biases.

<script>
    katex.render("\\nabla f", grad_f);
    katex.render("\\nabla f", grad_f_1);

    katex.render("f", func_f);
    katex.render("\\vec{u}", vector_u);
    katex.render("\\vec{u}", vector_u_1); 
    katex.render("\\vec{u}", vector_u_2); 

    katex.render("D_u f = \\nabla f \\cdot \\vec{u} = \\vert \\nabla f \\vert  \\vert \\vec{u} \\vert cos \\phi", grad_f_direction_u);

    katex.render("\\vert \\vec{u} \\vert = 1", vector_u_length);

     katex.render("D_u f = \\nabla f \\cdot \\vec{u} = \\vert \\nabla f \\vert  cos \\phi", grad_f_direction_u_unit);

     katex.render("\\phi = 0", phi_equal_0); 

     katex.render("\\vec{u} = \\frac{\\nabla f}{\\vert \\nabla f \\vert}", u_unit_from_grad_f); 

     katex.render("\\therefore D_{\\frac{\\nabla f}{\\vert \\nabla f \\vert}} f = \\nabla f \\cdot \\frac{\\nabla f}{\\vert \\nabla f \\vert} = \\frac{\\vert \\nabla f \\vert ^ 2}{\\vert \\nabla f \\vert} = \\vert \\nabla f \\vert \\to maximum \\space slope ", grad_f_direction_its_self);

     katex.render("\\phi = \\pi", phi_equal_pi);

     katex.render("\\vec{u} = \\frac{\\nabla f}{\\vert \\nabla f \\vert} \\cdot (-1) \\to opposite \\space direction", u_unit_from_grad_f_direction_opposite); 

     katex.render("\\therefore D_{\\frac{\\nabla f}{\\vert \\nabla f \\vert}_{descent}} f = \\nabla f \\cdot \\lbrack - \\frac{\\nabla f}{\\vert \\nabla f \\vert} \\rbrack = -\\frac{\\vert \\nabla f \\vert ^ 2}{\\vert \\nabla f \\vert} = - \\vert \\nabla f \\vert \\to minimum \\space slope ", grad_f_direction_its_self_direction_opposite);

     katex.render("\\omega^{+} = \\omega - \\eta \\nabla C", weight_update);
     katex.render("b^{+}  = b - \\eta \\nabla C", bias_update);

     katex.render("\\begin{bmatrix}  \\omega_{1}^{+} \\\\  \\\\ \\omega_{2}^{+} \\\\ \\\\ \\vdots \\\\ \\\\  \\omega_{n}^{+} \\end{bmatrix} = \\begin{bmatrix}  \\omega_{1} \\\\  \\\\ \\omega_{2} \\\\ \\\\ \\vdots \\\\ \\\\  \\omega_{n} \\end{bmatrix} -  \\eta \\begin{bmatrix}  \\frac{\\partial C}{\\partial \\omega_{1}} \\\\  \\\\ \\frac{\\partial C}{\\partial \\omega_{2}} \\\\ \\\\ \\vdots \\\\ \\\\  \\frac{\\partial C}{\\partial \\omega_{n}} \\end{bmatrix}", weight_update_matrix);

    katex.render("\\eta", eta_1);
    katex.render("\\eta", eta_2);
</script>

# Mathematical Notion

Previously has been discussed that the basic idea to find the optimal model is the gradient of a multivariable function. The topic of the gradient in calculus cannot be separated from the discussion of function theory and derivative. Since an ANN can have several <span  class="eq-expression katex" id="L_layer" ></span>  layers, the chaining function and chaining derivative are needed in analyzing its mathematical notion. The [3Blue1Brown team](https://www.youtube.com/watch?v=tIeHLnjs5U8&ab_channel=3Blue1Brown) has a good illustration in describing chain function and derivative in ANN graphically. This article adopts the illustration in describing the chaining rule.

By using the neuron topology in Figure 1, we can identify that:
<div class="eq">
        <span  class="eq-expression katex" id="aL_func_omega_bias" ></span>
        <span class="eq-no">Eq (7)</span>
</div>


Let define <span class="eq-expression katex" id="a_z_0"></span> to simplify mathematical expression.

<div class="eq">
        <span  class="eq-expression katex" id="zL_func_omega_bias" ></span>
        <span class="eq-no">Eq (8)</span>
</div>
<script>
    katex.render("L", L_layer);
    katex.render("a^{L} = \\sigma(\\omega^{(L)} a^{(L-1)} + b^{(L)})", aL_func_omega_bias);
    katex.render("z^{(L)} = \\omega^{(L)} a^{(L-1)} + b^{(L)}", zL_func_omega_bias);
    
</script>

Thus,
<div class="eq">
        <span  class="eq-expression katex" id="aL_func_sigma_z" ></span>
        <span class="eq-no">Eq (9)</span>
</div>
<script>
    katex.render("a^{(L)} = \\sigma(z^{(L)})", aL_func_sigma_z);
</script>

Correlation between variables described graphically as:

<img class="center" src="/images/post/2021-04-06-one-neuron-chain-rule.png" width="40%" height="40%">

<center><small>Figure 5: A cost chaining rule in a layer<br/>
        </small>
</center>
<br/>
The graphical correlation can be extended to the previous neuron.

<img class="center" src="/images/post/2021-04-06-two-neuron-chain-rule.png" width="40%" height="40%">

<center><small>Figure 6: A cost chaining rule in two layers<br/>
        </small>
</center>
<br/>
The sensitivity of cost function to the change of weight is expressed as:
<div class="eq">
        <span  class="eq-expression katex" id="dir_partial_C0_to_omega_chain" ></span>
        <span class="eq-no">Eq (10)</span>
</div>
<script>
    katex.render("\\frac{\\partial C_{0}}{\\partial \\omega^{(L)}} = \\frac{\\partial C_{0}}{\\partial a^{(L)}}  \\frac{\\partial a^{(L)}}{\\partial z^{(L)}}  \\frac{\\partial z^{(L)}}{\\partial \\omega^{(L)}} " , dir_partial_C0_to_omega_chain);
</script>


From Equation (1) and (10)


<div class="eq">
        <span  class="eq-expression katex" id="dir_partial_C0_to_omega_result" ></span>
</div>
<script>
    katex.render("\\frac{\\partial C_{0}}{\\partial a^{(L)}} = 2(a^{(L)} - y) " , dir_partial_C0_to_omega_result);
</script>

The coeficient of 2 indicates that deviation between <span  class="eq-expression katex" id="symbol_aL" ></span> and <span  class="eq-expression katex" id="symbol_y" ></span> significantly gives impact to the <span  class="eq-expression katex" id="symbol_partial_C_to_a" ></span>

<script>
    katex.render("a^{(L)}" , symbol_aL);
    katex.render("(y)" , symbol_y);
    katex.render("\\frac{\\partial C_{0}}{\\partial a^{(L)}}" , symbol_partial_C_to_a);
</script>

From Equation (9) and (10)

<div class="eq">
        <span  class="eq-expression katex" id="from_eq_9_10" ></span>
</div>
<script>
    katex.render("\\frac{\\partial a^{(L)}}{\\partial z^{(L)}} = \\sigma'(z^{(L)}) " , from_eq_9_10);
</script>

From Equation (8) and (10)
<div class="eq">
        <span  class="eq-expression katex" id="from_eq_8_10" ></span>
</div>
<script>
    katex.render("\\frac{\\partial z^{(L)}}{\\partial \\omega^{(L)}} = a^{(L-1)} " , from_eq_8_10);
</script>

So mathematical expression in Equation (10) can be written as:
<div class="eq">
        <span  class="eq-expression katex" id="from_eq_10_extend" ></span>
</div>

<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_omega_substitute" ></span>
        <span class="eq-no">Eq (11)</span>
</div>
<script>
    katex.render("\\frac{\\partial C_{0}}{\\partial \\omega^{(L)}} = \\frac{\\partial C_{0}}{\\partial a^{(L)}}  \\frac{\\partial a^{(L)}}{\\partial z^{(L)}}  \\frac{\\partial z^{(L)}}{\\partial \\omega^{(L)}} " , from_eq_10_extend);

    katex.render("\\frac{\\partial C_{0}}{\\partial \\omega^{(L)}} =  2(a^{(L)} - y)  \\sigma'(z^{(L)})   a^{(L-1)}" , partial_C_to_omega_substitute);
</script>


Since the cost can be thought as a function of weights and biases, the gradient of cost function of each training can be expressed in partial derivative of all weightes and biases. Thus we can present it as a vector matrix.

<div class="eq">
        <span  class="eq-expression katex" id="grad_C_in_matrix" ></span>
        <span class="eq-no">Eq (12)</span>
</div>
<script>
   katex.render("\\nabla C=\\begin{bmatrix}  \\frac{\\partial C}{\\partial \\omega^{(1)}} \\\\  \\\\ \\frac{\\partial C}{\\partial b^{(1)}} \\\\ \\\\ \\vdots \\\\ \\\\  \\frac{\\partial C}{\\partial \\omega^{(L)}} \\\\ \\\\  \\frac{\\partial C}{\\partial b^{(L)}} \\\\  \\end{bmatrix}", grad_C_in_matrix);
</script>

The sensitivity of cost function to weight can be extended to analyze sensitivity of cost function to bias.

<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_bias_chain" ></span>
        <span class="eq-no">Eq (13)</span>
</div>
<script>
    katex.render("\\frac{\\partial C_{0}}{\\partial b^{(L)}} = \\frac{\\partial C_{0}}{\\partial a^{(L)}}  \\frac{\\partial a^{(L)}}{\\partial z^{(L)}}  \\frac{\\partial z^{(L)}}{\\partial b^{(L)}} " , partial_C_to_bias_chain);
</script>


From Equation (8):
<div class="eq">
        <span  class="eq-expression katex" id="partial_Z_to_bias" ></span>
</div>
<script>
    katex.render("\\frac{\\partial z^{(L)}}{\\partial b^{(L)}} = 1" , partial_Z_to_bias);
</script>

Thus,
<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_bias_chain_2" ></span> <br/><br/>
        <span  class="eq-expression katex" id="partial_C_to_bias_substitute" ></span>
</div>
<script>
    katex.render("\\frac{\\partial C_{0}}{\\partial b^{(L)}} = \\frac{\\partial C_{0}}{\\partial a^{(L)}}  \\frac{\\partial a^{(L)}}{\\partial z^{(L)}}  \\frac{\\partial z^{(L)}}{\\partial b^{(L)}} " , partial_C_to_bias_chain_2);

    katex.render("\\frac{\\partial C_{0}}{\\partial b^{(L)}} = 2 (a^{(L)} - y) \\cdot \\sigma'(z^{(L)}) \\cdot 1 " , partial_C_to_bias_substitute);

</script>

<br/>

To get a more comprehensive neuron connection, Figure 7 denotes ANN with a more detailed subscript and superscript that show index neuron order and layer.

<img class="center" src="/images/post/2021-04-06-neuron-relation-with-index.png" width="60%" height="60%">

<center><small>Figure 7: ANN with indexed neurons<br/>
        </small>
</center>
<br/>
<div class="eq">
        <span  class="eq-expression katex" id="z_jL" ></span>
</div>
<script>
    katex.render("z_j^{(L)} = \\omega_{j0}^{(L)}a_0^{(L-1)} + \\omega_{j1}^{(L)}a_1^{(L-1)} + \\omega_{j2}^{(L)}a_2^{(L-1)} + b_j^{(L)}" , z_jL);
</script>

So that,
<div class="eq">
        <span  class="eq-expression katex" id="a_jL_function_activation" ></span>
</div>
<script>
    katex.render("a_j^{(L)} = \\sigma( z_j^{(L)})" , a_jL_function_activation);
</script>

Figure 7 shows that  <span  class="eq-expression katex" id="a_k_01" ></span> impacts the value of <span  class="eq-expression katex" id="a_k_02" ></span> and <span  class="eq-expression katex" id="a_k_03" ></span>. Thus, the rate of changes <span  class="eq-expression katex" id="C_0" ></span> to <span  class="eq-expression katex" id="a_k_01_a" ></span> is evaluated as (sum over layer L):


<div class="eq">
        <span  class="eq-expression katex" id="partial_C_ak_sum_L_layer" ></span>
        <span class="eq-no">Eq (14)</span>
</div>
<script>
    katex.render("\\frac{\\partial C_0}{\\partial a_k^{(L-1)}} = \\sum_{j=0}^{L-1} \\frac{\\partial C_0}{\\partial a_j^{(L)}} \\frac{\\partial a_j^{(L)}}{\\partial z_j^{(L)}} \\frac{\\partial z_j^{(L)}}{\\partial a_k^{(L-1)}}" , partial_C_ak_sum_L_layer);
</script>

Generally, Equation (11) can be written in a fully indexed notation as:

<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_omega_jk" ></span>
</div>
<script>
    katex.render("\\frac{\\partial C}{\\partial \\omega_{jk}^{(L)}} = \\frac{\\partial C}{\\partial a_j^{(L)}} \\frac{\\partial a_j^{(L)}}{\\partial z_j^{(L)}} \\frac{\\partial z_j^{(L)}}{\\partial \\omega_{jk}^{(L)}}" , partial_C_to_omega_jk);
</script>

<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_omega_jk_substitute" ></span>
        <span class="eq-no">Eq (15)</span>
</div>
<script>
    katex.render("\\frac{\\partial C}{\\partial \\omega_{jk}^{(L)}} = \\frac{\\partial C}{\\partial a_j^{(L)}}  \\sigma'(z_j^{(L)}) a_k^{(L-1)}" , partial_C_to_omega_jk_substitute);
</script>

Component <span  class="eq-expression katex" id="symbol_partial_C_to_omega_jk" ></span> of Equation (15) can be evaluated using the same approach as Equation (14)
<script>
    katex.render(" \\frac{\\partial C}{\\partial a_j^{(L)}}" , symbol_partial_C_to_omega_jk);
</script>

<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_omega_jk_sum_all_layer_L" ></span>
</div>
<script>
    katex.render("\\frac{\\partial C}{\\partial a_j^{(L)}} = \\sum_{j=0}^{(L+1) -1} \\lbrack \\frac{\\partial C}{a_j^{(L+1)}} \\rbrack  \\lbrack \\sigma'(z_j^{(L+1)}) \\rbrack \\lbrack \\omega_j^{(L+1)} \\rbrack" , partial_C_to_omega_jk_sum_all_layer_L);
</script>

If the cost function <span  class="eq-expression katex" id="C_def" ></span> defined as <span  class="eq-expression katex" id="C_def_1" ></span>, then

<div class="eq">
        <span  class="eq-expression katex" id="partial_C_to_omega_j_last_layer" ></span>
</div>
<script>
    katex.render("\\frac{\\partial C}{\\partial a_j^{(L)}} = 2(a_j^{(L)} - y_j)" , partial_C_to_omega_j_last_layer);
</script>

<script>
    katex.render("\\omega", omega1);
    katex.render("\\Sigma", sigma1);

    katex.render("a_k^{(L-1)}", a_k_01);
    katex.render("a_k^{(L-1)}", a_k_01_a);
    katex.render("a_0^{(L)}", a_k_02);
    katex.render("a_1^{(L)}", a_k_03);

    katex.render("z^{(L)}", a_z_0);
    katex.render("C_0", C_0);

    katex.render("C", C_def);
    katex.render("(a_j^{(L)} - y_j)^2", C_def_1);
    katex.render("\\frac{\\partial C}{\\partial a_j^{(L)}} ", C_to_aj);

</script>

# Numerical Computation

To complement the explanation about neural networks, in this post we will use an example provided by [Tobias Hill](https://machinelearning.tobiashill.se/part-2-gradient-descent-and-backpropagation/) with a slight modification in notation to meet our notation convention in the previous chapter. The neural network structure showed in Figure 8.

<img class="center" src="/images/post/2021-04-06-nn-numerical.png" width="80%" height="80%">

<center><small>Figure 8: Neural network with numeric attributes<br/>
        </small>
</center>
<br/>
The activation function we use is sigmoid, and the learning rate of <span  class="eq-expression katex" id="eta_0_point_1" ></span>

<div class="eq">
        <span  class="eq-expression katex" id="sigmoid_func" ></span>
        <span class="eq-no">Eq (16)</span>
</div>
<script>
    katex.render("\\sigma(z) = \\frac{1}{1+e^{-z}}" , sigmoid_func);
    katex.render("\\eta = 0.1" , eta_0_point_1);
</script>

<div class="eq">
        <span  class="eq-expression katex" id="sigmoid_derivative" ></span>
        <span class="eq-no">Eq (17)</span>
</div>
<script>
    katex.render("\\sigma'(z) = \\sigma(z)(1 - \\sigma(z))" , sigmoid_derivative);
</script>

The cost function of the ANN is evaluated with Equation (1).

## Feed Forward
First of all, Let's evaluate output of neuron by implmenting sigmoid activation fuction as showed in Equation (16).

<div class="">
        <span  class="eq-expression katex" id="eq_ff" ></span> <br/>
        <span  class="eq-expression katex" id="eq_ff_h1" ></span> <br/>
        <span  class="eq-expression katex" id="eq_ff_h2" ></span> <br/>
        <span  class="eq-expression katex" id="eq_ff_a1" ></span> <br/>
        <span  class="eq-expression katex" id="eq_ff_a2" ></span>
</div> 
<script>
    katex.render("h_1 = \\sigma(0.3 \\cdot 2 + (-0.4)\\cdot 3 + 0.25) = 0.41338242108267" , eq_ff_h1);
    katex.render("h_2 = \\sigma(0.2 \\cdot 2 + 0.6 \\cdot 3 + 0.45) = 0.934010990508781s" , eq_ff_h2);
    katex.render("a_1 = \\sigma(0.7 \\cdot h_1 + 0.5 \\cdot h_2 + 0.15) = 0.712257432295742" , eq_ff_a1);
    katex.render("a_2 = \\sigma((-0.3) \\cdot h_1 + (-0.1) \\cdot h_2 + 0.35) = 0.533097573871501" , eq_ff_a2);
    
</script>

The result and expected values of the ANN can expressed as matrix vector.
<div class="eq">
        <span  class="eq-expression katex" id="eq_ff_expected" ></span> <br/>
        <span  class="eq-expression katex" id="eq_ff_output" ></span>
        
</div>
<script>
        katex.render("Expected (y) =\\begin{bmatrix}  1.0 \\\\  \\\\ 0.2 \\\\  \\end{bmatrix}", eq_ff_expected);
        katex.render("Output (a) =\\begin{bmatrix}  0.712257432295742 \\\\  \\\\ 0.533097573871501 \\\\  \\end{bmatrix}", eq_ff_output);

   
</script>

The total cost of first feed to the ANN is evaluated with Equation (2):
<div class="">
        <span  class="eq-expression katex" id="eq_ff_cost_0" ></span> <br/>
        <span  class="eq-expression katex" id="eq_ff_cost_1" ></span> <br/>
</div>
<script>
        katex.render("C = (1-0.712257432295742)^2 + (0.2-0.533097573871501)^2", eq_ff_cost_0);
        katex.render("C = 0.19374977898811957", eq_ff_cost_1);
</script>

<br/>
Equation (2) shows that <span  class="eq-expression katex" id="cost_1" ></span> is essentially a function of <span  class="eq-expression katex" id="func_of_expected_actual" ></span> or <span  class="eq-expression katex" id="func_of_weight_bias_act_z_x_y" ></span>

<script>
        katex.render("C", cost_1);
        katex.render("C(y,a)", func_of_expected_actual);
        katex.render("C(\\omega, b, \\sigma, z, x, y)", func_of_weight_bias_act_z_x_y);
</script>



## Back Propagation
<h3 style="color: #2292A7;"> Weight of <span  class="eq-expression katex" id="omega_5" ></span> </h3>
<div>
        <span  class="eq-expression katex" id="cost_all"  ></span> <br/>
        <span  class="eq-expression katex" id="a1_back"  ></span> <br/>
        <span  class="eq-expression katex" id="z1_back"  ></span> <br/>

        <br/>
        <span  class="eq-expression katex" id="partial_dir_C_to_a1"  ></span> <br/>
        <span  class="eq-expression katex" id="partial_dir_a1_to_z1"  ></span> <br/>
        <span  class="eq-expression katex" id="partial_dir_z1_to_omega5"  ></span>
</div>
<br/>
Thus,

<span  class="eq-expression katex" id="partial_dir_cost_to_omega5"  ></span> <br/><br/>
<span  class="eq-expression katex" id="weight_5_update" ></span> <br/>

By using the same method, we can evaluate the update the other weights and biases in the last layer.
<br/>
<span  class="eq-expression katex" id="weight_6_update" ></span> <br/>
<span  class="eq-expression katex" id="weight_7_update" ></span> <br/>
<span  class="eq-expression katex" id="weight_8_update" ></span> <br/>

To update bias, we use the similar way as updating weight.
<span  class="eq-expression katex" id="partial_dir_cost_to_bias3"  ></span> <br/><br/>


<span  class="eq-expression katex" id="bias_3_update" ></span> <br/>
<span  class="eq-expression katex" id="bias_4_update" ></span> <br/>

<script>
        katex.render("\\omega_5", omega_5);
        katex.render("C = (a_1 - y_1)^2 + (a_2 - y_2)^2", cost_all);
        katex.render("a_1 = \\sigma(z_{a_1})", a1_back);
        katex.render("z_{a_1} = b_3 + \\omega_5 \\cdot h_1 + \\omega_7 \\cdot h_2", z1_back);

        katex.render("\\frac{\\partial C}{\\partial a_1} = 2(a_1 - y_1) = -0.5754851354085166", partial_dir_C_to_a1);
        katex.render("\\frac{\\partial a_1}{\\partial z_{a_1}} = \\sigma'(z_{a_1}) = 0.220793265944315", partial_dir_a1_to_z1);
        katex.render("\\frac{\\partial z_{a_1}}{\\partial \\omega_5} = h_1 = 0.41338242108267", partial_dir_z1_to_omega5);

       
        katex.render("\\frac{\\partial C}{\\partial \\omega_5} = \\frac{\\partial C}{\\partial a_1} \\frac{\\partial a_1}{\\partial z_{a_1}} \\frac{\\partial z_{a_1}}{\\partial \\omega_5} = -0.052525710835624", partial_dir_cost_to_omega5);
        
        katex.render("\\therefore \\omega_5^{+} = \\omega_5 - \\eta \\frac{\\partial C}{\\partial \\omega_5} = 0.70525257108356", weight_5_update);

        katex.render("\\omega_6^{+}  = -0.3064179482696209", weight_6_update);
        katex.render("\\omega_7^{+}  = 0.511867846503068", weight_7_update);
        katex.render("\\omega_8^{+}  = -0.511867846503068", weight_8_update);

        katex.render("\\frac{\\partial C}{\\partial b_3} = \\frac{\\partial C}{\\partial a_1} \\frac{\\partial a_1}{\\partial z_{a_1}} \\frac{\\partial z_{a_1}}{\\partial b_3}", partial_dir_cost_to_bias3);

        

        katex.render("b_3^{+}  = 0.1627063242549253", bias_3_update);
        katex.render("b_4^{+}  = 0.33447454961240974", bias_4_update);

</script>

<h3 style="color: #2292A7;"> Weight of <span  class="eq-expression katex" id="omega_1" ></span> </h3>


<span  class="eq-expression katex" id="z_h1"  ></span> <br/>
<span  class="eq-expression katex" id="h1"  ></span> <br/><br/>

<span  class="eq-expression katex" id="partial_c_to_za1"  ></span> <br/><br/>
<span  class="eq-expression katex" id="partial_c_to_za2"  ></span> <br/>

<br/>

<span  class="eq-expression katex" id="partial_dir_cost_to_omega1"  ></span> <br/><br/>
<span  class="eq-expression katex" id="partial_dir_cost_to_omega1_elaborate1"  ></span> <br/><br/>
<span  class="eq-expression katex" id="partial_dir_cost_to_omega1_elaborate2"  ></span> <br/><br/>
<span  class="eq-expression katex" id="partial_dir_cost_to_omega1_elaborate3"  ></span> <br/><br/>

<span  class="eq-expression katex" id="weight_1_update" ></span> <br/>

<script>
        katex.render("\\omega_1", omega_1);

        katex.render("z_{h_1} = b_1 + x_1 \\cdot \\omega_1 + x_2 \\cdot \\omega_3", z_h1);
        katex.render("h_1 = \\sigma(z_{h_1})", h1);

        katex.render("\\frac{\\partial C}{\\partial z_{a_1}} = \\frac{\\partial C}{\\partial a_1} \\frac{\\partial a_1}{\\partial z_{a_1}} = 2 (a_1 - y_1) \\sigma'(z_{a_1})", partial_c_to_za1);

        katex.render("\\frac{\\partial C}{\\partial z_{a_2}} = \\frac{\\partial C}{\\partial a_2} \\frac{\\partial a_2}{\\partial z_{a_2}} = 2 (a_2 - y_2) \\sigma'(z_{a_2})", partial_c_to_za2);


        katex.render("\\frac{\\partial C}{\\partial \\omega_1} =\\frac{\\partial ( C_{a_1} + C_{a_1})}{\\partial h_1} \\frac{\\partial h_1}{\\partial z_{h_{1}}} \\frac{\\partial z_{h_{1}}}{\\partial \\omega_1} = \\lbrack \\frac{\\partial C_{a_1}}{\\partial h_1} + \\frac{\\partial C_{a_2}}{\\partial h_1} \\rbrack \\frac{\\partial h_1}{\\partial z_{h_{1}}} \\frac{\\partial z_{h_{1}}}{\\partial \\omega_1}", partial_dir_cost_to_omega1);

        katex.render("\\space \\space \\space \\space \\space \\space = \\lbrack \\frac{\\partial C_{a_1}}{\\partial h_1} + \\frac{\\partial C_{a_2}}{\\partial h_1} \\rbrack \\frac{\\partial h_1}{\\partial z_{h_{1}}} \\frac{\\partial z_{h_{1}}}{\\partial \\omega_1}", partial_dir_cost_to_omega1_elaborate1);

        katex.render("\\space \\space \\space \\space \\space \\space = \\lbrack \\frac{\\partial C}{\\partial z_{a_1}} \\frac{\\partial z_{a_1}}{\\partial h_1} + \\frac{\\partial C}{\\partial z_{a_1}} \\frac{\\partial z_{a_1}}{\\partial h_1} \\rbrack \\frac{\\partial h_1}{\\partial z_{h_{1}}} \\frac{\\partial z_{h_{1}}}{\\partial \\omega_1}", partial_dir_cost_to_omega1_elaborate2);

        katex.render("\\space \\space \\space \\space \\space \\space = \\lbrack \\frac{\\partial C}{\\partial z_{a_1}} \\omega_5 + \\frac{\\partial C}{\\partial z_{a_1}} \\omega_6 \\rbrack \\sigma'(z_{h_1}) x_1 = -0.064945998937865", partial_dir_cost_to_omega1_elaborate3);

        katex.render("\\therefore \\omega_1^{+} = \\omega_1 - \\eta \\frac{\\partial C}{\\partial \\omega_1} = 0.3064945998937866", weight_1_update);
</script>


# References
1. [Neural Network, James Chen](https://www.investopedia.com/terms/n/neuralnetwork.asp)
2. [Gradient Descent and Backpropagation, Tobias Hill](https://machinelearning.tobiashill.se/part-2-gradient-descent-and-backpropagation/)
3. [How to Code a Neural Network with Backpropagation In Python (from scratch), Jason Brownlee](https://machinelearningmastery.com/implement-backpropagation-algorithm-scratch-python/)
4. [Backpropagation calculus-Deep learning, 3Blue1Brown](https://www.youtube.com/watch?v=tIeHLnjs5U8&ab_channel=3Blue1Brown)


