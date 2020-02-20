
# Table of Contents

1.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey](#org1659e35)
    1.  [Section 3.1](#orgd0e5018)
        1.  [Interactive Privacy via the Median Mechanism (Roth'10)](#org2d471e4)
        2.  [A multiplicative weights mechanism for privacy-preserving data analysis (Hardt'10)](#org38797ce)
        3.  [Iterative constructions and private data release (Gupta'12)](#orgca3ea45)
        4.  [Exploiting metric structure for efficient private query release (Huang'14)](#org8a4a345)
    2.  [](#org3148a3b)
    3.  [Section 3.2](#org9d63c87)
2.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list](#org8c7acf8)
    1.  [Review Dwork & Roth Book chapter 3](#org9fb120d)
        1.  [Section 3.4 Exponential Mechanism](#org3e4db62)
        2.  [Section 3.5 Composition](#org18c31bc)
        3.  [Section 3.6 Sparse Vector Technique](#org7f3f81c)
3.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists](#orgd329e11)
    1.  [Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity](#org4bf8c84)
    2.  [Privacy-Preserving Triangle counting in large graphs](#orgc44648a)



<a id="org1659e35"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey


<a id="orgd0e5018"></a>

## Section 3.1

The goal of papers this section is to maximize the number of (interactive) queries given a fixed amount of privacy loss and accuracy (usefulness). Traditionally, using Laplace mechanism, the magnitude of noise must scale linearly to the number of answered queries to preserve privacy. Given a fixed level of usefulness, Laplace mechanism can only answer a linear number of queries to the database size. There are several common settings of papers in this sections:

-   Interactive: the k-th query is released after answers from 1, .., k-1th queries are known
-   Database D can be changed any time in between queries


<a id="org2d471e4"></a>

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


<a id="org38797ce"></a>

### A multiplicative weights mechanism for privacy-preserving data analysis (Hardt'10)

-   Answer counting queries
-   View databases as histograms or distributions (databases must be transformed to histogram forms)
-   Easy query: if noisy answer by Laplace mechanism is not far from fractional database, release the answer of the fractional database
-   Hard query: return the noisy answer by Laplace mechanism
    -   At each iteration, update a fractional database, which is a multiplicative weighted version of all previous iterations


<a id="orgca3ea45"></a>

### Iterative constructions and private data release (Gupta'12)

-   Iterative database construction
-   Data: Graphs. Queries: cut functions
-   For every queries:
    -   If noisy answer is not far from answer of the database sequence, output the answer of database sequence and keep the database sequence the same for next iteration
    -   If noisy answer is far from answer of the database sequence, update the database sequence to satisfy:
        -   Difference between the answers of a query of the last database and others are large
        -   Difference between the noisy answer of a query of the last database to the answers of others are small


<a id="org8a4a345"></a>

### Exploiting metric structure for efficient private query release (Huang'14)

-   Settings: Database is a collection of points in metric space
-   Queries: average distance from an arbitrary point to some points in a database
-   The algorithm works for L1 distance query
-   Using online learning:
    -   Map queries to answers
    -   Update the mapping when the function is not correct
-   This mechanism does not use Laplace mechanism


<a id="org3148a3b"></a>

## 


<a id="org9d63c87"></a>

## Section 3.2


<a id="org8c7acf8"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list


<a id="org9fb120d"></a>

## Review Dwork & Roth Book chapter 3

<./PDFs/dwork-roth-privacybook.pdf>


<a id="org3e4db62"></a>

### Section 3.4 Exponential Mechanism

-   It's designed to produce outputs which are sensitive to their utility function
-   Rather than adding noise directly to outputs (Laplace), it produces a output with probability proportional to (utility score / sensitivity)
    -   Higher utility -> better chance to be sampled
-   Exponential Mechanism guarantees the accuracy by a additive factor of the optimal utility score


<a id="org18c31bc"></a>

### Section 3.5 Composition

-   Composition of Private mechanisms is also private
    -   But: The privacy guarantee degrades (&epsilon; & &delta; terms add up)
-   Define Differential Privacy in terms of distances between distribution
-   KL Distance. Max Distance. Approximate Max Distance


<a id="org7f3f81c"></a>

### Section 3.6 Sparse Vector Technique


<a id="orgd329e11"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists


<a id="org4bf8c84"></a>

## Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity

<https://arxiv.org/pdf/1208.4586>

This paper is a parallel work to Shiva's paper. Their (Shiva and Bloki) ideas about calculating sensitivity are similar, though the methods to achive them are different.

Both papers project an arbitrary graph to another graph space, where they can restrict some graph's attributes such as maximum degrees. With the restriction, both sensitivities are bounded to a much smaller level than originial global sensitivity.

While Shiva's paper uses Lipschitz extension to project a graph to a linear programming problem, this paper's method uses a more abstract method to generalize those projections. As long as there is a way to project the original graph space to a restricted one, and the distances of neighbor graphs after projected are bounded by a constant, it's possible to calculate a restricted sensitivity and use it as an alternative to the global sensitivity.


<a id="orgc44648a"></a>

## Privacy-Preserving Triangle counting in large graphs

<./PDFs/ding-triangles-cokm18.pdf>

This paper describes a method to release triangle counting in term of a privated historgram of nodes attached to different counts of triangles in a graph.

The method is to set up an threshold to limit the number of triangles a nodes can be linked to. It is equivalent to the restriction of Shiva's and Blocki's papers that limits the maximum degree of a graph. Instead of projecting the original graph to a bounded graph like the other 2 papers, this method tries to delete edges until the assumption is satisfied. After that, a histogram (or a cumulative function) is constructed and noise is added to each element of the histogram(or the cumulative dist). By Section 3 of Dwork's book, the sensivity of a histogram output is limited to modified cells when adding/removing an element, therefore it's independent on the number of cells of the histogram. In fact, the sensitivity of the histogram is linear to the maximum number of triangle a node can be linked to (which is also equivalent to the square of number of degrees as described Shiva's paper).

Clustering coefficients are calculated directly from the histogram distribution of nodes attached to different counts of triangles using the same method.

One drawback of this paper is it doesn't have an accuracy analysis of their method. Deleting edges reduces the number of triangles but there is no formal formula to quantify the approximations.

The edge deleting method is introduced in Bloki's paper. Blocki's paper also proves the accuracy of this method.

