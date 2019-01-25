# [Playlist](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)

## Chapter 2

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

## Chapter 3

- Linear Tranformation
    + Function: Vector Input , Vector Output
        * Visualize as moving input vector to output vector
        * Visualize as moving vector space (end points) to output vector space (end points).
    + Linear: 
        * All lines(collection of points(end points of vector)) must remain lines.
        * Origin must remain same.
    + **In a nutshell, keeping gridlines parallel and evenly spaced**

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

    + If two columns of transfomration vector are linearly dependent
        * The transformation squishes the vector space by 1 dimension.