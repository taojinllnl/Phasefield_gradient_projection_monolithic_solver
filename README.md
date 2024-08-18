## Phasefield_gradient_projection_monolithic_solver
A monolithic phase-field solver based on the gradient projection method to enforce damage irreversibility. 

### Purpose
This repository provides the source code and the input files for the numerical examples used in the paper draft titled “Gradient projection method for enforcing crack irreversibility as box constraints in a robust monolithic phase-field scheme”. The monolithic solver has the following features:

1. It uses the gradient projection method, which is a type of active set method, to enforce the phase-field (damage) irreversibility condition. It treats the phase-field inequality constraints ($d_n \leq d \leq 1.0$) as box constraints. The gradient projection method is extremely effective and computationally efficient to treat this type of box constraints.
2. The gradient projection method is combined with the limited-memory BFGS method to overcome the numerical difficulties due to the non-convexity of the phase-field energy functional.
3. This monolithic scheme is combined with an adaptive mesh refinement technique to alleviate the heavy computational cost associated with the phase-field fracture simulations.
4. Several 2D and 3D examples under cyclic loading conditions are provided.
