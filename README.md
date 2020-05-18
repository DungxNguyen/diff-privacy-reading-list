
# Table of Contents

1.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-05-18 Mon&gt; </span></span> Differentially Private SQL](#org58b2ac7)
2.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> DPNE: Differentially Private Network Embedding](#org1e57426)
3.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey](#orge953ab0)
    1.  [Section 3.1](#org874c97e)
        1.  [Interactive Privacy via the Median Mechanism (Roth'10)](#orgfd85df1)
        2.  [A multiplicative weights mechanism for privacy-preserving data analysis (Hardt'10)](#org5d49d98)
        3.  [Iterative constructions and private data release (Gupta'12)](#orga12f09d)
        4.  [Exploiting metric structure for efficient private query release (Huang'14)](#org3bb4fab)
    2.  [](#orga443948)
    3.  [Section 3.2](#org0fc6feb)
4.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list](#orgc8d5ce8)
    1.  [Review Dwork & Roth Book chapter 3](#org456fb07)
        1.  [Section 3.4 Exponential Mechanism](#orgfc272d7)
        2.  [Section 3.5 Composition](#org60d2c08)
        3.  [Section 3.6 Sparse Vector Technique](#orgfc7b0bb)
5.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists](#orgad1f2f3)
    1.  [Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity](#orge7ad189)
    2.  [Privacy-Preserving Triangle counting in large graphs](#orgde6b1f9)



<a id="org58b2ac7"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-05-18 Mon&gt; </span></span> Differentially Private SQL

-   Assumption: DP at user level. Each user can be related to multiple entries.
-   Issues the paper want to solve:
    -   A user contributes to multiple rows in the same partition (1) and multiple partitions (3)
    -   The group-by results may expose a group key (2): among 2 neighbors, a group key can exist in one and not the other.
        -   Resolve using threshold technique (Sparse vector technique in Dwork's book)
-   The library on github only implements primitive functions, other features have not been implemented yet.
-   Key designs:
    -   Column to map a user -> row (additional column of userid)
    -   Primitive queries are computed using Laplace noises, with global sensitivities calculated by bounds of the inputs (as query parameters) 
        -   Some bounds can be inferred from data (5.1.1)
        -   Primitime queries can not create share-owned rows
    -   What is "reservoir sampling"?
        -   Guess: sampling with a constraint that each partition can have at most some fixed numbers of rows.
    -   Reservoir sampling in the 1st phase (U) guarantees a fixed global stability
    -   Main idea: Decompose a join/group queries by to join/group by each user first, then join/group the intermediate results.
    -   A private query is divided into 2 phases:
        -   The 1st phase non-privately aggregate by users x keys and limits the # of output rows for each user (by reservoir sampling)
        -   The 2nd phase use primitive private functions to aggregate the (by user's output) and use threshold to report final outputs
-   (1) is solved by forcing the aggregation in the 1st phase to group by userid x keys -> each (userid x keys) can create at most 1 row in U.
-   (2) is solved by threshold at the end
-   (3) is solved by reservoir sampling how many rows each userid have in U


<a id="org1e57426"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> DPNE: Differentially Private Network Embedding

-   Edge Privacy
-   Naive baseline
-   Laplace mechanism


<a id="orge953ab0"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey


<a id="org874c97e"></a>

## Section 3.1

The goal of papers this section is to maximize the number of (interactive) queries given a fixed amount of privacy loss and accuracy (usefulness). Traditionally, using Laplace mechanism, the magnitude of noise must scale linearly to the number of answered queries to preserve privacy. Given a fixed level of usefulness, Laplace mechanism can only answer a linear number of queries to the database size. There are several common settings of papers in this sections:

-   Interactive: the k-th query is released after answers from 1, .., k-1th queries are known
-   Database D can be changed any time in between queries


<a id="orgfd85df1"></a>

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


<a id="org5d49d98"></a>

### A multiplicative weights mechanism for privacy-preserving data analysis (Hardt'10)

-   Answer counting queries
-   View databases as histograms or distributions (databases must be transformed to histogram forms)
-   Easy query: if noisy answer by Laplace mechanism is not far from fractional database, release the answer of the fractional database
-   Hard query: return the noisy answer by Laplace mechanism
    -   At each iteration, update a fractional database, which is a multiplicative weighted version of all previous iterations


<a id="orga12f09d"></a>

### Iterative constructions and private data release (Gupta'12)

-   Iterative database construction
-   Data: Graphs. Queries: cut functions
-   For every queries:
    -   If noisy answer is not far from answer of the database sequence, output the answer of database sequence and keep the database sequence the same for next iteration
    -   If noisy answer is far from answer of the database sequence, update the database sequence to satisfy:
        -   Difference between the answers of a query of the last database and others are large
        -   Difference between the noisy answer of a query of the last database to the answers of others are small


<a id="org3bb4fab"></a>

### Exploiting metric structure for efficient private query release (Huang'14)

-   Settings: Database is a collection of points in metric space
-   Queries: average distance from an arbitrary point to some points in a database
-   The algorithm works for L1 distance query
-   Using online learning:
    -   Map queries to answers
    -   Update the mapping when the function is not correct
-   This mechanism does not use Laplace mechanism


<a id="orga443948"></a>

## 


<a id="org0fc6feb"></a>

## Section 3.2


<a id="orgc8d5ce8"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list


<a id="org456fb07"></a>

## Review Dwork & Roth Book chapter 3

<./PDFs/dwork-roth-privacybook.pdf>


<a id="orgfc272d7"></a>

### Section 3.4 Exponential Mechanism

-   It's designed to produce outputs which are sensitive to their utility function
-   Rather than adding noise directly to outputs (Laplace), it produces a output with probability proportional to (utility score / sensitivity)
    -   Higher utility -> better chance to be sampled
-   Exponential Mechanism guarantees the accuracy by a additive factor of the optimal utility score


<a id="org60d2c08"></a>

### Section 3.5 Composition

-   Composition of Private mechanisms is also private
    -   But: The privacy guarantee degrades (&epsilon; & &delta; terms add up)
-   Define Differential Privacy in terms of distances between distribution
-   KL Distance. Max Distance. Approximate Max Distance


<a id="orgfc7b0bb"></a>

### Section 3.6 Sparse Vector Technique


<a id="orgad1f2f3"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists


<a id="orge7ad189"></a>

## Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity

<https://arxiv.org/pdf/1208.4586>

This paper is a parallel work to Shiva's paper. Their (Shiva and Bloki) ideas about calculating sensitivity are similar, though the methods to achive them are different.

Both papers project an arbitrary graph to another graph space, where they can restrict some graph's attributes such as maximum degrees. With the restriction, both sensitivities are bounded to a much smaller level than originial global sensitivity.

While Shiva's paper uses Lipschitz extension to project a graph to a linear programming problem, this paper's method uses a more abstract method to generalize those projections. As long as there is a way to project the original graph space to a restricted one, and the distances of neighbor graphs after projected are bounded by a constant, it's possible to calculate a restricted sensitivity and use it as an alternative to the global sensitivity.


<a id="orgde6b1f9"></a>

## Privacy-Preserving Triangle counting in large graphs

<./PDFs/ding-triangles-cokm18.pdf>

This paper describes a method to release triangle counting in term of a privated historgram of nodes attached to different counts of triangles in a graph.

The method is to set up an threshold to limit the number of triangles a nodes can be linked to. It is equivalent to the restriction of Shiva's and Blocki's papers that limits the maximum degree of a graph. Instead of projecting the original graph to a bounded graph like the other 2 papers, this method tries to delete edges until the assumption is satisfied. After that, a histogram (or a cumulative function) is constructed and noise is added to each element of the histogram(or the cumulative dist). By Section 3 of Dwork's book, the sensivity of a histogram output is limited to modified cells when adding/removing an element, therefore it's independent on the number of cells of the histogram. In fact, the sensitivity of the histogram is linear to the maximum number of triangle a node can be linked to (which is also equivalent to the square of number of degrees as described Shiva's paper).

Clustering coefficients are calculated directly from the histogram distribution of nodes attached to different counts of triangles using the same method.

One drawback of this paper is it doesn't have an accuracy analysis of their method. Deleting edges reduces the number of triangles but there is no formal formula to quantify the approximations.

The edge deleting method is introduced in Bloki's paper. Blocki's paper also proves the accuracy of this method.

