# [Videos By 3Blue1Brown](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)

## Chapter 2: Linear Combinations, span and bases

- Bases of coordinate system
    + (x, y): coordinates are scalar
    + What these scalars scale are the basis vectors
        * e.g i-hat and j-hat

- Linear Combination of **v** and **w**
    + a * **v** + b * **w**
        + a and b are scalars 

- Difficult to think about collection of vectors
    + We think of them as points (tip of the vector)

- Span of **v** and **w**
    + Set of all their linear combinations.
    + Span of most pairs of 2D vectors is all vectors of 2D space: The Entire sheet of 2D space.
        + But when they line up span is all vectors in their directions. (A line).

- Span of 2 vectors in 3D space
    + A sheet (plane)

- Span of 3 vectors in 3D space
    + If 3rd vector in span of the first two: Span is a sheet
    + Else: The 3D space itself.

- Linearly Dependent:
    + **a** is linearly dependent on **b** and **c** if **a** can be expressed as the linear combination of **b** and **c**.
        * That is **a** lies in the span of **b** and **c**
    + **a** is linearly independent on **b** and **c** if **a** cannot be expressed as the linear combination of **b** and **c**.

**The basis of a vector space is a set of linearly independent vectors that span the full space**

## Chapter 3: Matrices as linear transformations

- Linear Transformation
    + Function: Vector Input , Vector Output
        * Visualize as moving input vector to output vector
        * Visualize as moving vector space (end points) to output vector space (end points).
    + Linear: 
        * All lines(collection of points(end points of vector)) must remain lines.
        * Origin must remain same.
    + **In a nutshell, keeping grid lines parallel and evenly spaced**

- Linear Transformation of Vector space 
    + Depends only on Linear Transformation of basis vectors.
    + E.g **v** = a **i** + b **j** [a, b]
    * Let us do some linear transformation
        - **v** becomes **V**
        - **i** becomes **I**
        - **j** becomes **J**
        - Linear transformation ensures
            + **V** = a **I** + b **J**

    + So all we need to know is **I** and **J**
        * Let **I** = p **i** + q **j** or `[p, q]`<sup>T</sup>
        * Let **J** = r **i** + s **j** or `[p, q]`<sup>T</sup>

    + Therefore Entire transformation is determined by 
        ```
        [p , r]  
        [q , s]
        ```

    + Now
        * **V** = a **I** + b **J**
        * **V** = a (p **i** + q **j**) + b (r **i** + s **j**)
        * **V** = (a*p + b*r) **i** + (a*q + b*s) **j**
        ```
          [p, r] x [a] = [a*p + r*b]     
          [q, s]   [b]   [q*a + s*b]
        ```
    + **Linear Transformation of vector is multiplying it with a matrix**

    + E.g Rotating space by 90 anti clockwise
        * **i** becomes **j**  or `[0, 1]`
        * **j** becomes **-i** or `[-1, 0]`
        * So transformation matrix will be:
            ```
            [0  -1]
            [1   0]
            ```

    + Shear Transformation
        ```
        [1 1]
        [0 1]
        ```
    + If two columns of transformation vector are linearly dependent
        * The transformation squishes the vector space by 1 dimension.

## Chapter 4 and 5: Matrix Multiplication as composition and The Determinant

- Composition of Transformation.
    - rotation followed by shear
    - ShearMatrix * (RotationMatrix * Vector)
    - (ShearMatrix * RotationMatrix) * Vector
    - **Matrix Multiplication is Composition of Transformation**

- Order of Transformation Matters.
- (A B) C = A (B C)
    + Order in which transformations are applied is same for both:
        * C -> B -> A

- **The factor by which capacity of a given region changes on applying a transformation is called the determinant of the transformation.**

- Capacity: Volume for 3D space, Area for 2D space
    
- Determinant = 0.
    - Transformation squishes the space into lower dimensional space of capacity zero. 
    - Implies Area of any region on transformation becomes zero. That  implies transformation of the 2D space is a line or point. Transformation squishes vector space. This implies some columns might be linearly dependent.

- Determinant < 0. Implies flipping of the orientation.
    - 2D speed is flipped.
    - 3D right hand chirality to left hand chirality or vice versa. 

## Chapter 6: Inverse Matrices, column space and null space

- Solving System of Linear Equation
    + A **x** = **v**. Solve for x.
        * Find the original vector which when transformed using transformation matrix A gives **v**.
    
    + What if det(A) = 0.
        * This implies A squishes the space (2D) into lower dimension (line).
        * If **v** lies on the lower dimensional space (line)  
            - Multiple Original vectors can transform into **v**.
            - Infinite Solutions.
        * If **v** does not lies on the lower dimensional space
            - No solution.

    + det(A) != 0
        * Space does not get squished.
        * There exists a unique vector that becomes **v** on applying A.
        * Rewinding A transformation is itself a unique transformation called A<sup>-1</sup>.
        * A<sup>-1</sup> is like undoing A.
        * So applying trasformation A followed by A<sup>-1</sup> has no affect on a vector.
        * A A<sup>-1</sup> = I (Identity Transformation)
            * I dentity Transformation has no effect. Implie **i** and **j** remain where they are.
        ```
            [ 1 0 ]
            [ 0 1 ]            
        ```
- Column Space
    + Transformed vector space.
    + span of the column of the transformation matrix.
        * Because they are new **I** and **J** (basis).

- Rank
    - IF a transformation squishes the vector space to a k-dimensional space.
        + the rank of the transformation matrix is **k**.
    - Or it is the number of dimension of the column space.

- If Rank < Full Rank (Number of dimension in input space)
    + Some Set of vectors must become zero vectors.
    + 3D spaces changes to 2D plane on Applying A. then there exist a line of vectors in 3D space that gets squished to zero vectors.
    + 3D spaces changes to 1D lIne on Applying A. then there exist a whole plane of vectors in 3D space that gets squished to zero vectors.
    + This set of vectors that land on origin are called "Null Space" or "Kernel" of the transformation matrix.
