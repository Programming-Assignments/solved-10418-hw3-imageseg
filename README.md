Download Link: https://assignmentchef.com/product/solved-10418-hw3-imageseg
<br>
<h1>1      Written Questions</h1>

Answer the following questions in the template provided. Then upload your solutions to Gradescope. You may use L<sup>A</sup>T<sub>E</sub>X or print the template and hand-write your answers then scan it in. Failure to use the template may result in a penalty. There are 52 points and 12 questions. 1.1 Semantic Segmentation

Figure 1.1: Input Image                                  Figure 1.2: Image Segmentation

Let’s assume we have a 4 x 4 grid consisting of RGB pixels. Our task is to perform image segmentation, ie assign a label to each pixel from the set {foreground<em>,</em>background}. Intuitively, neighboring pixels should have similar values in y, i.e. pixels associated with a mountain should form one continuous blob. This knowledge can be naturally modeled via a Potts model which consists of potentials <em>φ</em>(<em>y<sub>i</sub>,x</em>) that encode the likelihood that any given pixel is from our subject and pairwise potentials <em>φ</em>(<em>y<sub>i</sub>,y<sub>j</sub></em>) which will encourage adjacent y’s to have the same value with high probability. If visualized as a graphical model, we would have a node for each pixel and an edge for each pair of pixels that are touching vertically or horizontally but not diagonally. We will refer to these sets as <em>V </em>and <em>E </em>respectively.

As seen in class, solving this task corresponds to finding <em>y </em>such that

<em>y</em>ˆ = argmax<em><sub>y </sub></em>log<em><sub>w </sub>p</em>(<em>y</em>|<em>x</em>)

= argmax<em>y</em>X<em>w</em><em>T </em><em>φ</em>(<em>x</em><em>j</em><em>,y</em><em>j</em>) + X <em>w</em><em>T </em><em>φ</em>(<em>y</em><em>j</em><em>,y</em><em>k</em>)

<em>j                                                     j,k</em>∈<em>E</em>

NOTE: This is not the same model used in the programming section. We will provide the set-up for that separately.

<h2>1.2        MAP Inference with Integer Linear Programming</h2>

In this section, we will formulate our problem as an Integer Linear Program (ILP) and use it to predict the segmentation of a small patch of our image. For convenience, denote the background class with 0 and the foreground class with 1. Furthermore suppose we have variables <em>α </em>and <em>β </em>that we will be optimizating over. Let <em>α<sub>i</sub></em>(<em>y</em>) represent an indictator function that is one when pixel <em>i </em>is labeled class <em>y</em>. Let <em>β<sub>ij</sub></em>(<em>y<sub>n</sub>,y<sub>m</sub></em>) represent an indicator function that is one when pixel <em>i </em>is set to class <em>y<sub>n </sub></em>and pixel <em>j </em>is set to class <em>y<sub>m</sub></em>.

First we will need to translate our requirements into a constraint set.

<ol>

 <li>(2 points) Short answer: Write constraints on the variables <em>α<sub>i</sub></em>(<em>y</em>) such that each pixel <em>i </em>is assigned one and only one class <em>y</em>.</li>

 <li>(4 points) Short answer: Write constraints on the variables <em>β<sub>ij</sub></em>(<em>y<sub>n</sub>,y<sub>m</sub></em>) such that pixels <em>i </em>and <em>j </em>are consistently applied to the same classes <em>y<sub>n </sub></em>and <em>y<sub>m </sub></em>in the pairwise potentials. Note that this will only be defined over <em>ij </em>pairs in our edge set <em>E</em>.</li>

 <li>(2 points) Shortanswer: Using the score functions and goal defined above, write an ILP that represents our constraint set and objective such that if solved we would have a solution to our image segmentation task.</li>

</ol>

Suppose we had the following unary feature functions:

<em>φ</em><sub>1</sub>(yellow pixel<em>,</em>background) = 1 <em>φ</em><sub>2</sub>(yellow pixel<em>,</em>foreground) = 2 <em>φ</em><sub>3</sub>(brown pixel<em>,</em>foreground) = 3 <em>φ</em><sub>4</sub>(brown pixel<em>,</em>background) = 0

In addition suppose we had the following non-zero pairwise feature functions:

<em>φ</em><sub>5</sub>(background<em>,</em>background) = 1 <em>φ</em><sub>6</sub>(foreground<em>,</em>background) = 2 <em>φ</em><sub>7</sub>(background<em>,</em>foreground) = 0 <em>φ</em><sub>8</sub>(foreground<em>,</em>foreground) = 1

Image Patch                               Segmentation

Figure 1.3: Extracted Image Patch and Segmentation

<ol start="4">

 <li>(7 points) Short answer: In this question we will walk through the process of solving the ILP for the 1 × 2 patch in the upper left hand corner of our image shown in figure 3. Assume an initial weight vector <em>w </em>= [1<em>,</em>1<em>,</em>1<em>,…</em>1].

  <ul>

   <li>What are the scores assigned by the model for all possible y configurations shown below?</li>

  </ul></li>

</ol>

<table width="474">

 <tbody>

  <tr>

   <td width="138"> </td>

   <td width="168">Yellow Background</td>

   <td width="168">Yellow Foreground</td>

  </tr>

  <tr>

   <td width="138">Brown Background</td>

   <td width="168">2</td>

   <td width="168">4</td>

  </tr>

  <tr>

   <td width="138">Brown Foreground</td>

   <td width="168">4</td>

   <td width="168">6</td>

  </tr>

 </tbody>

</table>

<ul>

 <li>What are the settings of variable <em>α </em>that solves the above ILP?</li>

 <li>What would be the updated weight vector <em>w </em>after running one iteration of structured perceptron?</li>

</ul>

<ol start="5">

 <li>(5 points) Short answer: Assume the same set-up as seen in question 4. Also assume an initial weight vector <em>w </em>= [1<em>,</em>1<em>,</em>1<em>,…</em>1] and the Hamming loss as <em>l</em>.

  <ul>

   <li>What are the loss augmented scores for all possible y configurations shown below?</li>

  </ul></li>

</ol>

<table width="474">

 <tbody>

  <tr>

   <td width="138"> </td>

   <td width="168">Yellow Background</td>

   <td width="168">Yellow Foreground</td>

  </tr>

  <tr>

   <td width="138">Brown Background</td>

   <td width="168">3</td>

   <td width="168">6</td>

  </tr>

  <tr>

   <td width="138">Brown Foreground</td>

   <td width="168">4</td>

   <td width="168">7</td>

  </tr>

 </tbody>

</table>

<ul>

 <li>What would be the updated weight vector <em>w </em>after running one iteration of structured SVM?</li>

</ul>

<h2>1.3      Loss-Augmented Inference and Learning</h2>

<ol>

 <li>(1 point) Select all that apply: Structured Perceptron differs from Structured SVM in? Training  Inference 2 Evaluation</li>

 <li>(4 points) Suppose you are given an MRF’s factor graph G = (C<em>,ψ,</em><strong>y</strong>) consisting of a set of factors C, potential functions <em>ψ</em>, and variables <strong>y </strong>∈ Y, where Y is the output space. Each factor <em>c </em>∈ C touches a subset of the variables <strong>y</strong>. The probability distribution defined by this graph decomposes multiplicatively according to the factors:</li>

</ol>

Further, you are given a loss function that decomposes additively according to the factors:

<em>`</em>(<strong>y</strong><sup>∗</sup><em>,</em><strong>y</strong>ˆ) = <sup>X</sup><em>`<sub>c</sub></em>(<strong><sup>y</sup></strong><em><sub>c</sub></em><sup>∗</sup><em>,</em><strong>y</strong>ˆ<em><sub>c</sub></em>)

<em>c</em>∈C

Show that you can define a new factor graph G<sup>0 </sup>(with the same factors and variables as G, but different potential functions <em>ψ</em><sup>0</sup>) such that the most probable assignment to <strong>y</strong>ˆ<sup>0 </sup>, argmax<strong><sub>y</sub></strong><sub>∈Y </sub><em>p</em><sub>G</sub>0(<strong>y</strong>) solves the loss-augmented MAP inference problem for G with loss <em>`</em>.

<h2>1.4      Empirical Questions</h2>

The following questions should be completed after you work through the programming portion of this assignment (Section 2).

<ol>

 <li>(12 points) Report the mean IOU and pixel accuracy scores for all models. For this question, you should run the FCN for 2 epochs and the linear SVM and structured SVM for 3 epochs each. <em>Round each value to two significant figures</em></li>

</ol>

<table width="361">

 <tbody>

  <tr>

   <td width="139">Model</td>

   <td width="92">Mean IOU</td>

   <td width="130">Pixel Accuracy</td>

  </tr>

  <tr>

   <td width="139">FCN (Train)</td>

   <td width="92"> </td>

   <td width="130"> </td>

  </tr>

  <tr>

   <td width="139">FCN (Test)</td>

   <td width="92">0.44</td>

   <td width="130">0.67</td>

  </tr>

  <tr>

   <td width="139">FCN+l-SVM (Train)</td>

   <td width="92"> </td>

   <td width="130"> </td>

  </tr>

  <tr>

   <td width="139">FCN+l-SVM (Test)</td>

   <td width="92">0.40</td>

   <td width="130">0.61</td>

  </tr>

  <tr>

   <td width="139">FCN+s-SVM (Train)</td>

   <td width="92"> </td>

   <td width="130"> </td>

  </tr>

  <tr>

   <td width="139">FCN+s-SVM (Test)</td>

   <td width="92">0.41</td>

   <td width="130">0.75</td>

  </tr>

 </tbody>

</table>

<ol start="2">

 <li>(10 points) Plot training and testing curves for the hinge loss (FCN+l-SVM) and structured hinge loss (FCN+s-SVM) respectively over 3 epochs. <em>Note: Your plot must be machine generated.</em></li>

 <li>(3 points) For any image of your choice from the test set, plot the labels predicted by FCN, FCN+lSVM and FCN+s-SVM as grayscale images. Use the visualization function provided in the code to plot images. Do you see any remarkable differences between the model predictions?</li>

 <li>(1 point) Multiple Choice: Did you correctly submit your code to Autolab?</li>

</ol>

Yes

No

<ol start="2">

 <li>(1 point) Numerical answer: How many hours did you spend on this assignment?.</li>

</ol>