# Differential Privacy Agenda

* <2019-12-30 Mon> Review Dwork & Roth Book chapter 3
  PDFs/dwork-roth-privacybook.pdf

** Section 3.4 Exponential Mechanism
   - It's designed to produce outputs which are sensitive to their utility function
   - Rather than adding noise directly to outputs (Laplace), it produces a output with probability proportional to (utility score / sensitivity)
     - Higher utility -> better chance to be sampled

** Section 3.5 Composition

** Section 3.6


* <2019-12-30 Mon> Review Differentially Private Data Analysis of Social Networks via Restricted Sensitivity
  https://arxiv.org/pdf/1208.4586

  This paper is a parallel work to Shiva's paper. Their ideas about calculating sensitivity is similar, though the methods to achive them are different.

  Both papers project an arbitrary graph to another graph space, where they can restrict some graph's attributes such as maximum degrees. With the restriction, both sensitivities are bounded to a much smaller level than originial global sensitivity.

  While Shiva's paper uses Lipschitz extension to project a graph to a linear programming problem, this paper's method uses a more abstract method to generalize those projections. As long as there is a way to project the original graph space to a restricted one, and the distances of neighbor graphs after projected are bounded by a constant, it's possible to calculate a restricted sensitivity and use it as an alternative to the global sensitivity.


* <2019-12-30 Mon> Privacy-Preserving Triangle counting in large graphs
PDFs/ding-triangles-cokm18.pdf

  This paper describes a method to release triangle counting in term of a privated historgram of nodes attached to different counts of triangles in a graph.

  The method is to set up an threshold to limit the number of triangles a nodes can be linked to. It is parallel to the restriction of Shiva's and Blocki's papers that restrict the maximum degree of a graph. Instead of projecting the original graph to a bounded graph like the other 2 papers, this method tries to delete edges until the assumption is satisfied. After that, a histogram (or a cumulative function) is constructed and noise is added to each element of the histogram(or the cumulative dist). By Section 3 of Dwork's book, the sensivity of a histogram output is limited to modified cells when adding/removing an element, therefore it's independent on the number of cells of the histogram. In fact, the sensitivity of the histogram is linear to the maximum number of triangle a node can be linked to (which is also equivalent to the square of number of degrees as described Shiva's paper).

  Clustering coefficients are calculated directly from the histogram distribution of nodes attached to different counts of triangles using the same method.

  One drawback of this paper is it doesn't have an accuracy analysis of their method. Deleting edges reduces the number of triangles but there is no formal formula to quantify the approximations.
