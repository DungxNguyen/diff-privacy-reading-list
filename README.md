
# Table of Contents

1.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list](#org932c8e1)
    1.  [Review Dwork & Roth Book chapter 3](#orge41a35c)
        1.  [Section 3.4 Exponential Mechanism](#org1291a9f)
        2.  [Section 3.5 Composition](#orgc2365e5)
        3.  [Section 3.6 Sparse Vector Technique](#org9cda9bd)
2.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists](#org14f4c53)
    1.  [Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity](#orgddeacad)
    2.  [Privacy-Preserving Triangle counting in large graphs](#org3396ef3)



<a id="org932c8e1"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list


<a id="orge41a35c"></a>

## Review Dwork & Roth Book chapter 3

<./PDFs/dwork-roth-privacybook.pdf>


<a id="org1291a9f"></a>

### Section 3.4 Exponential Mechanism

-   It's designed to produce outputs which are sensitive to their utility function
-   Rather than adding noise directly to outputs (Laplace), it produces a output with probability proportional to (utility score / sensitivity)
    -   Higher utility -> better chance to be sampled
-   Exponential Mechanism guarantees the accuracy by a additive factor of the optimal utility score


<a id="orgc2365e5"></a>

### Section 3.5 Composition

-   Composition of Private mechanisms is also private
    -   But: The privacy guarantee degrades (&epsilon; & &delta; terms add up)
-   Define Differential Privacy in terms of distances between distribution
-   KL Distance. Max Distance. Approximate Max Distance


<a id="org9cda9bd"></a>

### Section 3.6 Sparse Vector Technique


<a id="org14f4c53"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists


<a id="orgddeacad"></a>

## Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity

<https://arxiv.org/pdf/1208.4586>

This paper is a parallel work to Shiva's paper. Their (Shiva and Bloki) ideas about calculating sensitivity are similar, though the methods to achive them are different.

Both papers project an arbitrary graph to another graph space, where they can restrict some graph's attributes such as maximum degrees. With the restriction, both sensitivities are bounded to a much smaller level than originial global sensitivity.

While Shiva's paper uses Lipschitz extension to project a graph to a linear programming problem, this paper's method uses a more abstract method to generalize those projections. As long as there is a way to project the original graph space to a restricted one, and the distances of neighbor graphs after projected are bounded by a constant, it's possible to calculate a restricted sensitivity and use it as an alternative to the global sensitivity.


<a id="org3396ef3"></a>

## Privacy-Preserving Triangle counting in large graphs

<./PDFs/ding-triangles-cokm18.pdf>

This paper describes a method to release triangle counting in term of a privated historgram of nodes attached to different counts of triangles in a graph.

The method is to set up an threshold to limit the number of triangles a nodes can be linked to. It is equivalent to the restriction of Shiva's and Blocki's papers that limits the maximum degree of a graph. Instead of projecting the original graph to a bounded graph like the other 2 papers, this method tries to delete edges until the assumption is satisfied. After that, a histogram (or a cumulative function) is constructed and noise is added to each element of the histogram(or the cumulative dist). By Section 3 of Dwork's book, the sensivity of a histogram output is limited to modified cells when adding/removing an element, therefore it's independent on the number of cells of the histogram. In fact, the sensitivity of the histogram is linear to the maximum number of triangle a node can be linked to (which is also equivalent to the square of number of degrees as described Shiva's paper).

Clustering coefficients are calculated directly from the histogram distribution of nodes attached to different counts of triangles using the same method.

One drawback of this paper is it doesn't have an accuracy analysis of their method. Deleting edges reduces the number of triangles but there is no formal formula to quantify the approximations.

The edge deleting method is introduced in Bloki's paper. Blocki's paper also proves the accuracy of this method.

