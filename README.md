# Dimensional Analysis
This interactive Jupyter-notebook (Python 3) determines the dimensionless parameters for a given set of physical variables in accordance with the Buckingham $\Pi$ theorem.

## Requirements
* numpy
* pandas
* sympy

## Usage
Enter a single string with symbols of physical variables (separated by spaces). Choose the symbols from a collection of available variables here: [physical-variables.tsv](physical-variables.tsv). The code determines the dimensional matrix and the number $d$ of independent dimensionless parameters that can be formed from these variables. Finally, if dimensionless parameters could be found, one specific set (of infinitely many possible solutions) is presented in a visually appealing way.

#### Input
```python
variables = 'F_D u eta rho D'
```
#### Result
>$
\Pi_1 = \frac{F_D \rho}{\eta^2}\\
\Pi_2 = \frac{D \rho u}{\eta}
$

In this example the symbols represent following physical variables
 
| Symbol  | Description                | SI unit |             
|---------|----------------------------|---------|            
| F_D     | Drag force                 |N        |            
| u       | Flow velocity              |m / s    |            
| eta     | Dynamic viscosity of fluid |Pa s     |            
| rho     | Density of fluid           |kg / m^3 |            
| D       | Diameter of sphere         |m        |

In most cases you should be able to run the whole notebook at once  
$\rightarrow$ **"Restart the kernel, then re-run the whole notebook"**

In some special cases, however, the user is prompted either to rearrange the symbols in the input string or to delete a row from the dimensional matrix. However, the user is guided in doing so!

## Procedure
The file [physical-variables.tsv](physical-variables.tsv) contains a table of physical variables and their asscociated dimensions. The dimensions are given as exponents to the base quantities: $L, M, T, I, \Theta, N, J$.

Excerpt:

| Symbol   | Quantity                        | L  | M  | T  | I  | Theta | N  | J | SI unit     |
|----------|---------------------------------|----|----|----|----|-------|----|---|-------------|
| l        | Length                          | 1  | 0  | 0  | 0  | 0     | 0  | 0 | m           |
| m        | Mass                            | 0  | 1  | 0  | 0  | 0     | 0  | 0 | kg          |
| t        | Time                            | 0  | 0  | 1  | 0  | 0     | 0  | 0 | s           |
| I        | Electric current                | 0  | 0  | 0  | 1  | 0     | 0  | 0 | A           |
| T        | Temperature                     | 0  | 0  | 0  | 0  | 1     | 0  | 0 | K           |
| n        | Amount of substance             | 0  | 0  | 0  | 0  | 0     | 1  | 0 | mol         |
| I_v      | Luminous intensity              | 0  | 0  | 0  | 0  | 0     | 0  | 1 | cd          |
| D        | Diameter                        | 1  | 0  | 0  | 0  | 0     | 0  | 0 | m           |
| c        | Speed of light                  | 1  | 0  | -1 | 0  | 0     | 0  | 0 | m / s       |
| g        | Gravitational acceleration      | 1  | 0  | -2 | 0  | 0     | 0  | 0 | m / s^2     |
| nu       | Kinematic viscosity             | 2  | 0  | -1 | 0  | 0     | 0  | 0 | m^2 / s     |
| P        | Power                           | 2  | 1  | -3 | 0  | 0     | 0  | 0 | W           |
| rho      | Density                         | -3 | 1  | 0  | 0  | 0     | 0  | 0 | kg / m^3    |
| p        | Pressure                        | -1 | 1  | -2 | 0  | 0     | 0  | 0 | Pa          |
| m_dot    | Mass flow rate                  | 0  | 1  | -1 | 0  | 0     | 0  | 0 | kg / s      |
| k_th     | Thermal conductivity            | 1  | 1  | -3 | 0  | -1    | 0  | 0 | W / (m K)   |
| c_p      | Specific heat capacity          | 2  | 0  | -2 | 0  | -1    | 0  | 0 | J / (kg K)  |
| q_dot    | Heat flux                       | 0  | 1  | -3 | 0  | 0     | 0  | 0 | W / m^2     |
| h        | Heat transfer coefficient       | 0  | 1  | -3 | 0  | -1    | 0  | 0 | W / (m^2 K) |
| a_th     | Thermal diffusivity             | 2  | 0  | -1 | 0  | 0     | 0  | 0 | m^2 / s     |

The table is unsorted and incomplete, but will be extended from time to time. The values are tab-separated. Thus, on github the table should be nicely rendered and searchable. Some variables have symbols as they are commonly used in Germany (for my own convenience). A **unique symbol** must be chosen, when a new physical variable is added! Or, of course, you could alter the table or make an entirely new table with your own symbols.

### Dimensional Matrix
The code looks up the dimensions of the entered symbols in the table above and builds the corresponding dimensional matrix. For the given example, the dimensional matrix is:

| Symbol | F_D | u  | eta | rho | D |
|--------|-----|----|-----|-----|---|
| L      | 1   | 1  | -1  | -3  | 1 |
| M      | 1   | 0  | 1   | 1   | 0 |
| T      | -2  | -1 | -1  | 0   | 0 |

From the rank of the dimensional matrix (here: $r=3$) and the number of variables/symbols (here: $n=5$) the number of dimensionless parameters is determined by $d = n - r$ (here: $d = 2$).

In most cases the rank of the dimensional matrix will be the number of involved base quantities (here: $L$, $M$, $T$) and therefore equal to the number of rows $m$ in the dimensional matrix. However, in some cases the rank is lower than the number of rows. In this case one or more rows must be deleted (without reducing the rank thereby), until the number of rows equals the rank ($m = r$)!

### Buckingham $\Pi$ Theorem
The Buckingham $\Pi$ theorem states (in generel), that if there is a physically meaningful problem involving a certain number $n$ of physical variables

$f(p_1, p_2, p_3, ..., p_n)=0$

the problem can be simplified and rewritten in terms of a set of $d$ dimensionless parameters

$f(\Pi_1, \Pi_2, \Pi_3, ..., \Pi_d)=0$ &nbsp; with $d<n$

where $\Pi$ is a dimensionless parameter constructed from the physical variables $p_1, p_2, p_3, ..., p_n$   

$ 
\begin{align}
& \Pi_1 = p_1^{k_{(1)1}} \cdot p_2^{k_{(1)2}} \cdot p_3^{k_{(1)3}} \cdot \; ... \; \cdot p_n^{k_{(1)n}}\\
& \Pi_2 = p_1^{k_{(2)1}} \cdot p_2^{k_{(2)2}} \cdot p_3^{k_{(2)3}} \cdot \; ... \; \cdot p_n^{k_{(2)n}}\\
& ... \\
& \Pi_d = p_1^{k_{(d)1}} \cdot p_2^{k_{(d)2}} \cdot p_3^{k_{(d)3}} \cdot \; ... \; \cdot p_n^{k_{(d)n}}
\end{align}
$

This Jupyter-notebook can find solutions for these $k$-exponents and thus provide one possible set of dimensionless parameters.
However, since we are dealing with an underdetermined system of equations, the calculated result is only one solution of an infinite number of possible solutions!

For a detailed description of this procedure please referre to ["Dimensional Analysis for Engineers" by Simon, Weigand and Gomaa](https://doi.org/10.1007/978-3-319-52028-5)

### Sequential Order of Symbols in the Input-String
When more than one dimensionless parameter is expected (i.e. $d >1$) the order of the symbols in the input string is very important. Different orders will result in different sets of dimensionless parameters. These sets are all valid solutions, but not all sets are equally practical or useful to work with going forward.

#### Dependent / Independent Variables
A rule of thumb is that the dependent variable should be placed in the leftmost position and the independent variables in the rightmost position. In the example above the drag force $F_D$ is the dependent variable and the remaining variables are all independent. Think of a wind tunnel test that examines the drag force of a sphere. The researchers can directly influence the independent variables. They can change the flow velocity $u$ and the fluid properties $\eta$ and $\rho$ independently; and of course they can investigate spheres of different sizes $D$. The drag force is influenced by these independent variables and is therefore a dependent variable.

This has the advantage that the dependent variable, when placed in the leftmost position, appears in only one of the dimensionless parameters and can thus be conveniently isolated.

#### Property of Rightmost Rectangular Submatrix
In fact, the $d$ leftmost variables will all appear only in one dimensionless parameter. On the other hand the $r$ rightmost variables might appear in all dimensionless parameters. Roughly speaking, the $r$ rightmost variables are used to de-dimensionalize the $d$ leftmost variables. Therefore, the dimensional vectors of the $r$ rightmost variables must be linear independent, so that they can *build* all the required dimensions in the given dimensional vector space. For example, if our physics problem contains the variables $T_{wall}$ and $T_{air}$, we must not put these two variables (of the same dimension) inside the $r$ rightmost positions, as this would result in linear dependent vectors!

The procedure solves a linear system of equations by means of matrix inversion. The matrix in question (the rightmost rectangular submatrix of the dimensional matrix) must not be singular, i.e. its determinant must not be zero! 

**If this is not the case (i.e. the submatrix is singular), then the symbols in the input-string must be rearanged before we can proceed!**

For a detailed instruction on how the variables should be ordered, please referre to ["Applied Dimensional Analysis and Modeling" by Szirtes and Rózsa](https://doi.org/10.1016/B978-0-12-370620-1.X5000-X), which I highly recommend.

## Examples
### Drag Force of Sphere - Version 2
Here the string contains the same variables as the example above. However, in the sequential order the positions of **u** and **eta** are switched.

#### Input
```python
variables = 'F_D eta u rho D'
```
#### Result
>$
{\Pi_1}^* = \frac{F_D}{D^2 \rho u^2}\\
{\Pi_2}^* = \frac{\eta}{D \rho u}
$

The $d$ leftmost variables are $F_D$ and $\eta$. Therefore, $F_D$ appears only in ${\Pi_1}^*$ and $\eta$ appears only in ${\Pi_2}^*$. The independent variables $u$, $\rho$ and $D$ appear in both dimensionless parameters. 

Both resulting sets of dimensionless parameter are equally valid. However, in the second set we recognize that ${\Pi_1}^*$ represents (except for a constant) the commonly used drag coefficient $c_D$. In the first example we directly obtained the Reynolds number ($\Pi_2 =  Re$), whereas in the second example we obtained the reciprocal of the Reynolds number (${\Pi_2}^* =  \frac{1}{Re}$).

If we went with the first sequential order, all hope is not lost. We could still arrive at the  form of the commonly used drag coefficient. Since the square of a dimensionless parameter is still dimensionless and also the product of two dimensionless parameters is still dimensionless, we can combine the parameters to obtain a new (equally valid) parameter:

>$
\Pi_1 \cdot \frac{1}{{\Pi_2}^2} = \frac{F_D \rho}{\eta^2} \cdot \frac{\eta^2}{D^2 \rho^2 u^2} = \frac{F_D}{D^2 \rho u^2} = {\Pi_1}^*
$

### Forced Convection Heat Transfer
We are interessted in the heat transfer coefficient $h$ (dependent variable) for a cooling channel with the diameter $D$ (independent variable), the flow velocity $u$ (independent variable) and following fluid properties: specific heat capacity $c_p$, density $\rho$, dynamic viscosity $\eta$ and thermal conductivity $k_{th}$ (all independent variables).

#### Input
```python
variables = 'h u c_p rho eta k_th D'
```
#### Result
>$
\Pi_1 = \frac{D h}{k_{th}}\\
\Pi_2 = \frac{D \rho u}{\eta}\\
\Pi_3 = \frac{c_p \eta}{k_{th}}
$

Just by looking at the involved variables and their corresponding dimensions, we were able fo find three famous dimensionless numbers: The Nusselt number ($\Pi_1 = Nu$), the Reynolds number ($\Pi_2 = Re$) and the Prandtl number ($\Pi_3 = Pr$). Isn't that faszinating?

## License
This project is licensed under the terms of the MIT license, see [LICENSE.md](LICENSE.md).

## References
* [Simon, Volker; Weigand, Bernhard; Gomaa, Hassan (2017): Dimensional Analysis for Engineers. Cham: Springer International Publishing](https://doi.org/10.1007/978-3-319-52028-5)

* [Szirtes, Thomas; Rózsa, Pál (2007): Applied dimensional analysis and modeling. 2. ed. Amsterdam: Elsevier/Butterworth-Heinemann](https://doi.org/10.1016/B978-0-12-370620-1.X5000-X)

