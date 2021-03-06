# svm-notes
Notes on how SVM works

Good resources
---
http://web.mit.edu/6.034/wwwbob/svm-notes-long-08.pdf

https://www.quantstart.com/articles/Support-Vector-Machines-A-Guide-for-Beginners

Definitions
---
- Hyperplane: A flat, N-1 dimensional subspace that splits an N-dimensional space into two parts. In 2D space this would be a line (1D object). In 3D space this would be a plane (2D object). For higher dimensional space this object is generally referred to as a hyperplane.
- Margin: For linearly separable data, it is the distance between the hyperplane and the nearest points in the training set. For a 2D feature space, this can be visualized as a street, with the lanes of the street corresponding to the margins of the positive and negative examples.
- Support vectors: The points from the training set sitting at the edges of the margins. These combinations of features are the most difficult to classify, and are ultimately the only points used to define the hyperplane decision boundary.

Maximum Margin Classifier Derivation
---
c<sub>1</sub>x<sub>1</sub> + c<sub>2</sub>x<sub>2</sub> + ... c<sub>N</sub>x<sub>N</sub> + c<sub>N+1</sub> = 0

Note that respresenting the N-1 dimensional hyperplane in N-dimensional space requires N+1 parameters. A hyperplane can be made into a decision boundary by defining ax<sub>1</sub> + bx<sub>2</sub> + c > 0 for positive examples, and ax<sub>1</sub> + bx<sub>2</sub> + c < 0 for negative examples (for the 2D case).

The decision boundary can be expressed in vectorized form for mathematical convinience:

w<sub>0</sub><sup>T</sup>x<sub>i</sub> + b<sub>0</sub> > 0 WHEN y<sub>i</sub> = +1

w<sub>0</sub><sup>T</sup>x<sub>i</sub> + b<sub>0</sub> < 0 WHEN y<sub>i</sub> = -1

The decision boundary condition can be further reduced into a single expression:

y<sub>i</sub>(w<sub>0</sub><sup>T</sup>x<sub>i</sub>) > 1

The goal is to maximize the margin while abiding by the above condition. The distance between any potential support vector and the hyperplane in the 2D case is:

|a<sub>1</sub> + bx<sub>2</sub> + c|/sqrt(a<sup>2</sup> + b<sup>2</sup>)

Therefore, the distance between the hyperplane and the support vectors is:

|w<sub>0</sub><sup>T</sup>x<sub>i</sub>|/||w<sub>0</sub>||

For a support vector, |w<sub>0</sub><sup>T</sup>x<sub>i</sub>| = 1. Therefore, margin = 1/||w<sub>0</sub>||, so we must maximize the total margin (2/||w<sub>0</sub>||), or minimize ||w<sub>0</sub>||, in order to find the optimal hyperplane decision boundary given the condition y<sub>i</sub>(w<sub>0</sub><sup>T</sup>x<sub>i</sub>) > 1.

Minimizing ||w<sub>0</sub>||
---
Let's play a math trick, the reason for which will be apparent later. Instead of minimizing ||w<sub>0</sub>||, let's minimize 0.5*||w<sub>0</sub>||<sup>2</sup>. This is now a quadratic function to be optimized according to the y<sub>i</sub>(w<sub>0</sub><sup>T</sup>x<sub>i</sub>) > 1 boundary condition. Since the boundary condition only considers the support vectors, the final two equations become:

Minimize (f): 0.5*||w<sub>0</sub>||<sup>2</sup>
Condition (g): y<sub>i</sub>(w•x<sub>i</sub>) – b = 1

Note that we are minimizing a quadratic, which is a paraboloid with a guaranteed single, convex global minimum. This set of equations can be solved analytically with the Lagrangian multipler method (not discussed here).

Kernel Trick
---
The above only works and guarantees an optimal solution if the problem is linearly separable. If not, then we need to transform the space to make it linearly separable. But won't this make everything a mess, because we have to compute (k(x<sub>i</sub>)•k(x<sub>j</sub>)) instead of the normal dot product (x<sub>i</sub>•x<sub>j</sub>)? Nope, turns out you only need to compute K(x<sub>i</sub>•x<sub>j</sub>)! Therefore, the kernel function can be highly complex without significant computational consequences. Typical kernel functions include:
- polynomial
- radial (polar coordinates)
- sigmoid
