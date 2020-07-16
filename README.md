
# Table of Contents

1.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-07-16 Thu&gt;</span></span>](#org235ea9e)
    1.  [Last meeting:](#orgb68fb81)
    2.  [Reading: Differentially Private Combinatorial Optimization](#org16f1cff)
2.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-08 Mon&gt; </span></span> Dwork's book chapter 7](#org03d1cbb)
    1.  [Subsample and aggregate](#orgd5021e2)
    2.  [Propose test release](#orgda78dfa)
    3.  [Stability and privacy](#org02264fe)
3.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-07 Sun&gt; </span></span> Dwork's book chapter 4: Linear queries with correlated error](#orgc79b63d)
        1.  [Database update mechanism](#org51bd547)
4.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-07 Sun&gt; </span></span> Karwa: Private Analysis of Graph Structure (also has a journal version)](#org603d83d)
5.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-07 Sun&gt; </span></span> Erdos-Reyni paper](#org1f781ea)
6.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-05-18 Mon&gt; </span></span> Differentially Private SQL](#org76ae0f4)
7.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> DPNE: Differentially Private Network Embedding](#org15098d0)
8.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey paper](#org72b1470)
    1.  [Section 3.1](#orge8a46d2)
        1.  [Interactive Privacy via the Median Mechanism (Roth'10)](#orgf984907)
        2.  [A multiplicative weights mechanism for privacy-preserving data analysis (Hardt'10)](#orgcb7cebd)
        3.  [Iterative constructions and private data release (Gupta'12)](#org39b0618)
        4.  [Exploiting metric structure for efficient private query release (Huang'14)](#orgdf820c2)
    2.  [](#org75a9ebe)
    3.  [Section 3.2](#org6d3a65c)
9.  [<span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list](#orgc31d8af)
    1.  [Review Dwork & Roth Book chapter 3](#orgf6a53f3)
        1.  [Section 3.4 Exponential Mechanism](#orgc74b9c5)
        2.  [Section 3.5 Composition](#orgd9dae9f)
        3.  [Section 3.6 Sparse Vector Technique](#org656beeb)
10. [<span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists](#orgfc1ed0b)
    1.  [Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity](#orge45aa72)
    2.  [Privacy-Preserving Triangle counting in large graphs](#org895cadc)



<a id="org235ea9e"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-07-16 Thu&gt;</span></span>


<a id="orgb68fb81"></a>

## Last meeting:

-   IPDPS Target (Deadline October 2020): Fast private alg
-   Consider high-order local sensitivity of path counting


<a id="org16f1cff"></a>

## Reading: Differentially Private Combinatorial Optimization

-   General Idea:
    -   For a combinatorial algorithm, a solution is a combination of elements that optimize some metrics
    -   Then use an approximation algorithm to generate a set of combinations close enough to the optimal solution with have high probabilities
    -   Then use the exponential mechanism to output the private solution from the set of candidates
-   Private Min Cut alg
    -   Step 2: Propose a set of added edges
    -   Step 3: Use exponential mechanism, privatly choose a set of added egdes so that its min cut > \(4 ln n / \epsilon\)
    -   Step 4: Use exponential mechanism to return a cut based on its cost
    -   Note:
        -   The number of cuts within an additive factor is bounded (Kar93)
        -   There are exponential number of cuts in step 4. However, Kar93 can output a cut within a constant factor with some polynomial probabilities. Then, a polynomial runs of Kar93 can output all the cuts w.h.p.


<a id="org03d1cbb"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-08 Mon&gt; </span></span> Dwork's book chapter 7

-   Apply when the worst case sensitivity is unbound or difficult to analyze


<a id="orgd5021e2"></a>

## Subsample and aggregate


<a id="orgda78dfa"></a>

## Propose test release

-   Same as Tutorial section 3.2


<a id="org02264fe"></a>

## Stability and privacy

-   Distance to instability: Same as Tutorial section 3.3
    -   It uses Pertubation Stabilitity: Only feasible if a function f(x) is stable (local sensitivity is 0)
    -   Idea:
        -   If their neighbors to some distances are all stable, we can release f(x) privately
        -   If not, release Null
    -   Generally difficult to compute. Several pratical functions: median, mode
-   A<sub>samp</sub>: Bootstrapping thu Subsample and aggregate
    -   It uses q-subsampling stability: f(x) = f(\hat{x}) with prob > 3/4 when elements of \hat{x} are sampled independently with prob q
    -   sampled subsets are not necessarily disjoint
    -   Idea:
        -   If f(x) is q-subsampling stabability
        -   Then we sample x<sub>1</sub>, x<sub>2</sub>, &#x2026; x<sub>m</sub> samples from x and produces z<sub>i</sub> = f(x<sub>i</sub>)
        -   Then the function "returns the mode of z<sub>1</sub>, z<sub>2</sub> &#x2026;, z<sub>m</sub>" is Pertubation Stable (on x) then releasing the mode of z<sub>i</sub> is differentially private
        -   W.h.p, the mode of z<sub>i</sub> is the same with f(x)


<a id="orgc79b63d"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-07 Sun&gt; </span></span> Dwork's book chapter 4: Linear queries with correlated error

-   Assumption: 
    -   databases are represenented as histogram of the universe.
    -   queries are linear queries:
        -   no correlation between elements of a database
        -   map each element to a value in the interval [0, 1]
-   Introduce methods to analyze/answer multiple queries
    -   to reduce noise <-> better accuracy and save privacy "budget" <-> more queries
        -   many papers prefer the term "budget", though Dwork book rarely mentions this term
        -   instead, they use "privacy level" or (&epsilon;, &delta;)-privacy guarantee
    -   by correlating noise (not adding noises independently)
-   Idea: If 2 queries are linearly correlated, then noises are not necessarily indepedent
-   Offline: SmallDB mechanism
    -   Main idea: Based on exponential mechanism, output a small sampled database from the same universe with the original, with probability propotional to how close it is from the original database (closeness is based on differences in queries' outputs)
    -   Privacy is derived from exponential mechanism
    -   The remaining is utility (accuracy):
        -   If we randomly sample elements of the original database, with high probability the sample database is close to the original (on outputs of queries)
        -   Other than that, the accuracy is based on exponential mechanism
        -   Also, we can use VC-Dimension of Q to replace log|Q|, in which we have tighter bound on accuracy
-   Online: Multiplicative mechanism
    -   Main idea: the algorithm maintains a synthetic database and learn it along the way with queries. Only learning task uses noisy mechanisms and consumes privacy cost.
    -   Interactively, for each query:
        -   Compare the outputs of the synthetic database to the noisy output of the real database (via Laplace mechanism)
            -   If they're close, return the synthetic database
            -   If not, return the noisy output and update the synthetic database
    -   Database update mechanism:


<a id="org51bd547"></a>

### TODO Database update mechanism

-   The number of updates is bounded:
    -   We start at a fixed divergence (between the synthetic database & the real database)
    -   Each update reduce the divergence
    -   The divergence is non-negative
    -   Then we have at most some amount of updates
-   After the updates reach it optimal, then the the distance of synthetic database and real database (based on queries' outputs) are all small
-   The threshold to update/release is based on Numeric Sparse (Sparse Vector technique)
    -   Accuracy then follows Numeric Spars

-   Online vs Offline:


<a id="org603d83d"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-07 Sun&gt; </span></span> Karwa: Private Analysis of Graph Structure (also has a journal version)

-   Edge Privacy
-   Subgraph counting:
    -   Exponential Random Graph Models use sub graph counts
    -   Calculating clustering coefficient (for analyzing network data)
-   Smooth sensitivity is NP-hard for some functions
    -   Counting triangles has efficient smooth sensitivity
-   Rastogi's paper: calculate high probability bound on local sensitivity
-   1.2: some works about node and edge privacy (check if necessary)
-   Laplace vs Cauchy
-   Smooth sensitivity for k-star counting
    -   Exercise: calculate &beta;-smooth sensitivity of counting triangles (Nissim claim 3.13 full version)
-   Decompose smooth sensitivity: Decompose by distance or Decompose by elements (edge)
-   Privately bound on local sensitivity for k-triangle counting
    -   They find a bound on the local sensitivity of the local sensitivity (with privacy)


<a id="org1f781ea"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-06-07 Sun&gt; </span></span> Erdos-Reyni paper

-   To estimate the edge density of a degree-concentrated graph
-   By estimating the number of edges privately
-   Question: Compare with Shiva's section 4 about counting egdes
-   Main idea: create a function to approximate number of edges so that the function has low local sensitivity. Then prove that the local sensitivity satisfies smooth sensitivity. In order for the function to have low(bounded) local sensitivity, the degrees must be concentrated.
-   For Erdos-Reyni graphs, the concentration of node degrees can be estimated -> then apply the method above


<a id="org76ae0f4"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-05-18 Mon&gt; </span></span> Differentially Private SQL

-   Assumption: DP at user level. Each user can be related to multiple entries -> simplest case: multiple rows belong to one user but a single row only belongs to one user.
-   Issues the paper want to solve:
    -   A partition of database is a distinct combination of several keys
    -   A user contributes to multiple rows in the same partition (1) and multiple partitions (3)
    -   The group-by results may expose a group key (2): among 2 neighbors, a group key can exist in one and not the other -> expose user with unique partition keys. -> naive solution: count all possible combinations, but it depends on the domain of each keys, sometimes unbounded (unlimited) combinations.
        -   Resolve using threshold technique (Sparse vector technique in Dwork's book), only report if the noisy counts are above some threshold
-   The library on github only implements primitive (sum, average, median) functions, other features have not been implemented yet.
-   Main idea: Decompose a join/group queries by to join/group by each user first, then join/group the intermediate results -> user-disjoint queries.
    -   A private query is divided into 2 phases:
        -   The 1st phase non-privately aggregate by users x keys and limits the # of output rows for each user (by reservoir sampling)
        -   The 2nd phase use primitive private functions to aggregate the (by user's output) and use threshold to report final outputs
-   Some important designs:
    -   Column to map a user -> row (additional column of userid)
    -   Primitive queries are computed using Laplace noises, with global sensitivities calculated by bounds of the inputs (as query parameters) 
        -   Some bounds of the values can be inferred from data (5.1.1)
        -   Primitime queries can not create share-owned rows (no row can contains data from 2 or more users)
    -   What is "reservoir sampling"?
        -   Guess: sampling with a constraint that each partition can have at most some fixed numbers of rows.
    -   Reservoir sampling in the 1st phase (U) guarantees a fixed global stability
-   (1) is solved by forcing the aggregation in the 1st phase to group by cross product of "userid x keys" -> each combination of (userid x other partition keys) can create at most 1 row in U.
-   (2) is solved by threshold method
-   (3) is solved by reservoir sampling how many rows each userid have in U (max number of combinations can share a user id)


<a id="org15098d0"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> DPNE: Differentially Private Network Embedding

-   Edge Privacy
-   Naive baseline
-   Laplace mechanism


<a id="org72b1470"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-02-20 Thu&gt; </span></span> Survey paper


<a id="orge8a46d2"></a>

## Section 3.1

The goal of papers this section is to maximize the number of (interactive) queries given a fixed amount of privacy loss and accuracy (usefulness). Traditionally, using Laplace mechanism, the magnitude of noise must scale linearly to the number of answered queries to preserve privacy. Given a fixed level of usefulness, Laplace mechanism can only answer a linear number of queries to the database size. There are several common settings of papers in this sections:

-   Interactive: the k-th query is released after answers from 1, .., k-1th queries are known
-   Database D can be changed any time in between queries


<a id="orgf984907"></a>

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


<a id="orgcb7cebd"></a>

### A multiplicative weights mechanism for privacy-preserving data analysis (Hardt'10)

-   Answer counting queries
-   View databases as histograms or distributions (databases must be transformed to histogram forms)
-   Easy query: if noisy answer by Laplace mechanism is not far from fractional database, release the answer of the fractional database
-   Hard query: return the noisy answer by Laplace mechanism
    -   At each iteration, update a fractional database, which is a multiplicative weighted version of all previous iterations


<a id="org39b0618"></a>

### Iterative constructions and private data release (Gupta'12)

-   Iterative database construction
-   Data: Graphs. Queries: cut functions
-   For every queries:
    -   If noisy answer is not far from answer of the database sequence, output the answer of database sequence and keep the database sequence the same for next iteration
    -   If noisy answer is far from answer of the database sequence, update the database sequence to satisfy:
        -   Difference between the answers of a query of the last database and others are large
        -   Difference between the noisy answer of a query of the last database to the answers of others are small


<a id="orgdf820c2"></a>

### Exploiting metric structure for efficient private query release (Huang'14)

-   Settings: Database is a collection of points in metric space
-   Queries: average distance from an arbitrary point to some points in a database
-   The algorithm works for L1 distance query
-   Using online learning:
    -   Map queries to answers
    -   Update the mapping when the function is not correct
-   This mechanism does not use Laplace mechanism


<a id="org75a9ebe"></a>

## 


<a id="org6d3a65c"></a>

## Section 3.2


<a id="orgc31d8af"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2020-01-06 Mon&gt; </span></span> Reading list


<a id="orgf6a53f3"></a>

## Review Dwork & Roth Book chapter 3

<./PDFs/dwork-roth-privacybook.pdf>


<a id="orgc74b9c5"></a>

### Section 3.4 Exponential Mechanism

-   It's designed to produce outputs which are sensitive to their utility function
-   Rather than adding noise directly to outputs (Laplace), it produces a output with probability proportional to (utility score / sensitivity)
    -   Higher utility -> better chance to be sampled
-   Exponential Mechanism guarantees the accuracy by a additive factor of the optimal utility score


<a id="orgd9dae9f"></a>

### Section 3.5 Composition

-   Composition of Private mechanisms is also private
    -   But: The privacy guarantee degrades (&epsilon; & &delta; terms add up)
-   Define Differential Privacy in terms of distances between distribution
-   KL Distance. Max Distance. Approximate Max Distance


<a id="org656beeb"></a>

### Section 3.6 Sparse Vector Technique


<a id="orgfc1ed0b"></a>

# <span class="timestamp-wrapper"><span class="timestamp">&lt;2019-12-30 Mon&gt; </span></span> Reading lists


<a id="orge45aa72"></a>

## Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity

<https://arxiv.org/pdf/1208.4586>

This paper is a parallel work to Shiva's paper. Their (Shiva and Bloki) ideas about calculating sensitivity are similar, though the methods to achive them are different.

Both papers project an arbitrary graph to another graph space, where they can restrict some graph's attributes such as maximum degrees. With the restriction, both sensitivities are bounded to a much smaller level than originial global sensitivity.

While Shiva's paper uses Lipschitz extension to project a graph to a linear programming problem, this paper's method uses a more abstract method to generalize those projections. As long as there is a way to project the original graph space to a restricted one, and the distances of neighbor graphs after projected are bounded by a constant, it's possible to calculate a restricted sensitivity and use it as an alternative to the global sensitivity.


<a id="org895cadc"></a>

## Privacy-Preserving Triangle counting in large graphs

<./PDFs/ding-triangles-cokm18.pdf>

This paper describes a method to release triangle counting in term of a privated historgram of nodes attached to different counts of triangles in a graph.

The method is to set up an threshold to limit the number of triangles a nodes can be linked to. It is equivalent to the restriction of Shiva's and Blocki's papers that limits the maximum degree of a graph. Instead of projecting the original graph to a bounded graph like the other 2 papers, this method tries to delete edges until the assumption is satisfied. After that, a histogram (or a cumulative function) is constructed and noise is added to each element of the histogram(or the cumulative dist). By Section 3 of Dwork's book, the sensivity of a histogram output is limited to modified cells when adding/removing an element, therefore it's independent on the number of cells of the histogram. In fact, the sensitivity of the histogram is linear to the maximum number of triangle a node can be linked to (which is also equivalent to the square of number of degrees as described Shiva's paper).

Clustering coefficients are calculated directly from the histogram distribution of nodes attached to different counts of triangles using the same method.

One drawback of this paper is it doesn't have an accuracy analysis of their method. Deleting edges reduces the number of triangles but there is no formal formula to quantify the approximations.

The edge deleting method is introduced in Bloki's paper. Blocki's paper also proves the accuracy of this method.

