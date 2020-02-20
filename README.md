
# Table of Contents

1.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey](#org5daa16e)
    1.  [Section 3.1](#org21b92e8)
        1.  [Interactive Privacy via the Median Mechanism (Roth'10)](#orgba542b0)
        2.  [](#org4f5d55b)
    2.  [](#org5efafb1)
    3.  [Section 3.2](#orge4d7301)
2.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list](#org032510f)
    1.  [Review Dwork & Roth Book chapter 3](#orga5c6c85)
        1.  [Section 3.4 Exponential Mechanism](#orgf6681d4)
        2.  [Section 3.5 Composition](#org411cddb)
        3.  [Section 3.6 Sparse Vector Technique](#org85415d9)
3.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists](#org7a51854)
    1.  [Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity](#orga2eae00)
    2.  [Privacy-Preserving Triangle counting in large graphs](#org65bcf3c)



<a id="org5daa16e"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey


<a id="org21b92e8"></a>

## Section 3.1

The goal of papers this section is to maximize the number of (interactive) queries given a fixed amount of privacy loss and accuracy (usefulness). Traditionally, using Laplace mechanism, the magnitude of noise must scale linearly to the number of answered queries to preserve privacy. Given a fixed level of usefulness, Laplace mechanism can only answer a linear number of queries. There are several common settings of papers in this sections:

-   Interactive: the k-th query is released after answers from 1, .., k-1th queries are known
-   Database D can be changed any time in between queries


<a id="orgba542b0"></a>

### Interactive Privacy via the Median Mechanism (Roth'10)

-   All queries are fractional queries (answer which fraction of database D has some properties)
-   Initiate the database set of all databases of size n
-   Divide queries into 2 categories: easy & hard
-   Easy queries are answered by the median answers of
    -   previous databases, which are consistent in the answers of previous hard queries
-   Hard queries are answered by Laplace mechanism
    -   Remove databases from the database set which are inconsistent with the new answer
-   Stop when there is not enough database in the set
-   The mechanism can answer O(logklog|X|) hard queries


<a id="org4f5d55b"></a>

### 


<a id="org5efafb1"></a>

## 


<a id="orge4d7301"></a>

## Section 3.2


<a id="org032510f"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list


<a id="orga5c6c85"></a>

## Review Dwork & Roth Book chapter 3

<./PDFs/dwork-roth-privacybook.pdf>


<a id="orgf6681d4"></a>

### Section 3.4 Exponential Mechanism

-   It's designed to produce outputs which are sensitive to their utility function
-   Rather than adding noise directly to outputs (Laplace), it produces a output with probability proportional to (utility score / sensitivity)
    -   Higher utility -> better chance to be sampled
-   Exponential Mechanism guarantees the accuracy by a additive factor of the optimal utility score


<a id="org411cddb"></a>

### Section 3.5 Composition

-   Composition of Private mechanisms is also private
    -   But: The privacy guarantee degrades (&epsilon; & &delta; terms add up)
-   Define Differential Privacy in terms of distances between distribution
-   KL Distance. Max Distance. Approximate Max Distance


<a id="org85415d9"></a>

### Section 3.6 Sparse Vector Technique


<a id="org7a51854"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists


<a id="orga2eae00"></a>

## Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity

<https://arxiv.org/pdf/1208.4586>

This paper is a parallel work to Shiva's paper. Their (Shiva and Bloki) ideas about calculating sensitivity are similar, though the methods to achive them are different.

Both papers project an arbitrary graph to another graph space, where they can restrict some graph's attributes such as maximum degrees. With the restriction, both sensitivities are bounded to a much smaller level than originial global sensitivity.

While Shiva's paper uses Lipschitz extension to project a graph to a linear programming problem, this paper's method uses a more abstract method to generalize those projections. As long as there is a way to project the original graph space to a restricted one, and the distances of neighbor graphs after projected are bounded by a constant, it's possible to calculate a restricted sensitivity and use it as an alternative to the global sensitivity.


<a id="org65bcf3c"></a>

## Privacy-Preserving Triangle counting in large graphs

<./PDFs/ding-triangles-cokm18.pdf>

This paper describes a method to release triangle counting in term of a privated historgram of nodes attached to different counts of triangles in a graph.

The method is to set up an threshold to limit the number of triangles a nodes can be linked to. It is equivalent to the restriction of Shiva's and Blocki's papers that limits the maximum degree of a graph. Instead of projecting the original graph to a bounded graph like the other 2 papers, this method tries to delete edges until the assumption is satisfied. After that, a histogram (or a cumulative function) is constructed and noise is added to each element of the histogram(or the cumulative dist). By Section 3 of Dwork's book, the sensivity of a histogram output is limited to modified cells when adding/removing an element, therefore it's independent on the number of cells of the histogram. In fact, the sensitivity of the histogram is linear to the maximum number of triangle a node can be linked to (which is also equivalent to the square of number of degrees as described Shiva's paper).

Clustering coefficients are calculated directly from the histogram distribution of nodes attached to different counts of triangles using the same method.

One drawback of this paper is it doesn't have an accuracy analysis of their method. Deleting edges reduces the number of triangles but there is no formal formula to quantify the approximations.

The edge deleting method is introduced in Bloki's paper. Blocki's paper also proves the accuracy of this method.

