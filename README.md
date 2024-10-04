## Phasefield_gradient_projection_monolithic_solver
A monolithic phase-field solver based on the gradient projection method to enforce damage irreversibility. 

### Purpose
This repository provides the source code and the input files for the numerical examples used in the paper draft titled “Gradient projection method for enforcing crack irreversibility as box constraints in a robust monolithic phase-field scheme”. The monolithic solver has the following features:

1. It uses the gradient projection method, which is a type of active set method, to enforce the phase-field (damage) irreversibility condition. It treats the phase-field inequality constraints ($d^{(n)} \leq d \leq 1.0$) as box constraints. The gradient projection method is extremely effective and computationally efficient to treat this type of box constraints.
2. The gradient projection method is combined with the limited-memory BFGS method to overcome the numerical difficulties due to the non-convexity of the phase-field energy functional.
3. This monolithic scheme is combined with an adaptive mesh refinement technique to alleviate the heavy computational cost associated with the phase-field fracture simulations.
4. Several 2D and 3D examples under cyclic loading conditions are provided.

### Content
The repository contains the following content:
1. The source code of the gradient projection phase-field monolithic solver.
2. The input files for several 2D and 3D phase-field fracture simulations under cyclic loading conditions.

### Input file options
1. In the input file for each numerical example, under the "Linear solver type", users can choose to use either the direct matrix factorization method or the conjugate gradient method with a preconditioner based on the incomplete LU (ILU) decomposition. According to the author's experiences, the conjugate gradient method (CG) is always faster than the direct matrix factorization.
2. In the input file for each numerical example, under the "Nonlinear solver type", users can choose to use either "**LBFGS**" or "**LBFGSB**". The first option "LBFGS" does not enforce the phase-field irreversibility conditiona at all and should only be used for comparison purpose. The second option "LBFGSB" uses the gradient projection method to enforce the phase-field irreversibility condition as box constraints (the last "B" stands for "box" ). This is the option that should be chosen for any practical phase-field crack simulations.
3. There is a seperate L-BFGS monolithic solver that uses the history variable (the maximum of the positive strain energy) approach to enforce the phase-field irreversibility, see the repository https://github.com/taojinllnl/Phasefield_L-BFGS_monolithic_solver .

### How to compile
The finite element procedure is implemented in [deal.II](https://www.dealii.org/), which is an open source finite element library. In order to use the code (**main.cc**) provided here, deal.II should be configured with MPI and at least with the interfaces to BLAS, LAPACK, Threading Building Blocks (TBB), and UMFPACK. For optional interfaces to other software packages, see https://www.dealii.org/developer/readme.html. **The code is tested to work with the develop ("master") branch (as Aug. 18th, 2024) of the dealii library**. 

Once the deal.II library is compiled, for instance, to "~/dealii-dev/bin/", follow the steps listed below:
1. cd SourceCode
2. cmake -DDEAL_II_DIR=~/dealii-dev/bin/  .
3. make debug or make release
4. make

### Potential compilation and compatibility issues
1. Depending on your compiler, during compilation, you might receive some warning messages like below: `warning: missing braces around initializer for ‘std::array<dealii::LinearOperator<dealii::Vector<double>, dealii::Vector<double>, dealii::internal::LinearOperatorImplementation::EmptyPayload>, 2>’ [-Wmissing-braces]`. These warning messages should not have any negative impact and could be ignored.
2. The code uses the `SolutionTransfer` class during the adaptive mesh refinement technique. The member function `interpolate()` of the `SolutionTransfer` class has different interfaces between the develop ("master") branch and older versions of deal.ii (for instance, 9.4.0). If you use the develop branch, you should use the following interface: `interpolate(std::vector< VectorType > & all_out)`. If you use an older version of deal.ii, you should use the following interface: `interpolate(const std::vector< VectorType > & all_in, std::vector< VectorType > & all_out )`. Please look at the interfaces of the member function `interpolate()` in the `SolutionTransfer` class in the specific version of deal.ii used by you.

### How to run
1. Go into one of the examples folders.
2. For instance a 2D test case: go into simple_shear_cyclic_load/cg_solve/
3. Run via ../../main 2
4. For 3D test cases, run via ../../main 3

### How to cite this work:
TBD
