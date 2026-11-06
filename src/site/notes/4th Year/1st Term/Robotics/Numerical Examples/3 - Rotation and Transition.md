---
{"dg-publish":true,"permalink":"/4th-year/1st-term/robotics/numerical-examples/3-rotation-and-transition/","dg-note-properties":{}}
---

## Example 1: Basic Rotation Matrices

**Problem Statement:**
Find the rotation matrices for the following cases:
1. Rotation about the $x$-axis with angle $40^\circ$.
2. Rotation about the $y$-axis with angle $75^\circ$.
3. Rotation about the $z$-axis with angle $25^\circ$.

---

### Solution

#### (a) Rotation about the $x$-axis by $40^\circ$
$$R_x(40^\circ) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(40^\circ) & -\sin(40^\circ) \\ 0 & \sin(40^\circ) & \cos(40^\circ) \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0.76 & -0.64 \\ 0 & 0.64 & 0.76 \end{bmatrix}$$

#### (b) Rotation about the $y$-axis by $75^\circ$
$$R_y(75^\circ) = \begin{bmatrix} \cos(75^\circ) & 0 & \sin(75^\circ) \\ 0 & 1 & 0 \\ -\sin(75^\circ) & 0 & \cos(75^\circ) \end{bmatrix} = \begin{bmatrix} 0.25 & 0 & 0.96 \\ 0 & 1 & 0 \\ -0.96 & 0 & 0.25 \end{bmatrix}$$

#### (c) Rotation about the $z$-axis by $25^\circ$
$$R_z(25^\circ) = \begin{bmatrix} \cos(25^\circ) & -\sin(25^\circ) & 0 \\ \sin(25^\circ) & \cos(25^\circ) & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0.9 & -0.42 & 0 \\ 0.42 & 0.9 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

---

## Example 2: Frame Representation in Space

**Problem Statement:**
The frame $F$ is located at $(3, 5, 7)$ units, with:
- Its $n$-axis parallel to $x$
- Its $o$-axis at $45^\circ$ relative to $y$
- Its $a$-axis at $45^\circ$ relative to $z$

---

### Solution
The orientation represents a rotation about the $x$-axis by $45^\circ$:
$$R = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(45^\circ) & -\sin(45^\circ) \\ 0 & \sin(45^\circ) & \cos(45^\circ) \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0.707 & -0.707 \\ 0 & 0.707 & 0.707 \end{bmatrix}, \quad P = \begin{bmatrix} 3 \\ 5 \\ 7 \end{bmatrix}$$

Constructing the $4 \times 4$ homogeneous transformation matrix $F$:
$$F = \begin{bmatrix} 1 & 0 & 0 & 3 \\ 0 & 0.707 & -0.707 & 5 \\ 0 & 0.707 & 0.707 & 7 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

---

## Example 3: Point Rotation

**Problem Statement:**
Find the coordinates of point $(2, 3, 4)$ relative to the reference frame after a rotation of $45^\circ$ about the $x$-axis.

---

### Solution
$$R = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(45^\circ) & -\sin(45^\circ) \\ 0 & \sin(45^\circ) & \cos(45^\circ) \end{bmatrix} \approx \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0.7 & -0.7 \\ 0 & 0.7 & 0.7 \end{bmatrix}$$

Applying the transformation:
$$T = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 0.7 & -0.7 \\ 0 & 0.7 & 0.7 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \\ 4 \end{bmatrix} = \begin{bmatrix} 2 \\ -0.7 \\ 4.9 \end{bmatrix}$$

---

## Example 4: Vector Rotation

**Problem Statement:**
Find the resulting vector after rotating vector $P_1 = (0, \sqrt{3}, 1)$ by an angle of $30^\circ$ about the $x$-axis.

---

### Solution
$$R = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(30^\circ) & -\sin(30^\circ) \\ 0 & \sin(30^\circ) & \cos(30^\circ) \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \frac{\sqrt{3}}{2} & -\frac{1}{2} \\ 0 & \frac{1}{2} & \frac{\sqrt{3}}{2} \end{bmatrix}$$

Computing $P_2$:
$$P_2 = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \frac{\sqrt{3}}{2} & -\frac{1}{2} \\ 0 & \frac{1}{2} & \frac{\sqrt{3}}{2} \end{bmatrix} \begin{bmatrix} 0 \\ \sqrt{3} \\ 1 \end{bmatrix} = \begin{bmatrix} 0 \\ \frac{3}{2} - \frac{1}{2} \\ \frac{\sqrt{3}}{2} + \frac{\sqrt{3}}{2} \end{bmatrix} = \begin{bmatrix} 0 \\ 1 \\ \sqrt{3} \end{bmatrix}$$

---

## Example 5: Pure Translation of a Frame

**Problem Statement:**
A frame $F$ has been moved $9$ units along the $x$-axis and $5$ units along the $z$-axis of the reference frame. Find the new location of the frame:
$$F = \begin{bmatrix} 0.527 & -0.574 & 0.628 & 5 \\ 0.369 & 0.819 & 0.439 & 3 \\ -0.766 & 0 & 0.643 & 8 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

---

### Solution
$$F_{\text{New}} = \operatorname{Tran}(d_x, d_y, d_z) \times F_{\text{old}} = \operatorname{Tran}(9, 0, 5) \times F_{\text{old}}$$

$$F_{\text{New}} = \begin{bmatrix} 1 & 0 & 0 & 9 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 5 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0.527 & -0.574 & 0.628 & 5 \\ 0.369 & 0.819 & 0.439 & 3 \\ -0.766 & 0 & 0.643 & 8 \\ 0 & 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 0.527 & -0.574 & 0.628 & 14 \\ 0.369 & 0.819 & 0.439 & 3 \\ -0.766 & 0 & 0.643 & 13 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

---

## Example 6: Combined Transformation (Rot Z $\to$ Rot Y $\to$ Trans)

**Problem Statement:**
A point $p = [7, 3, 1]^T$ is attached to a frame $F_{noa}$ and is subjected to:
1. Rotation of $90^\circ$ about the $z$-axis
2. Followed by a rotation of $90^\circ$ about the $y$-axis
3. Followed by a translation of $[4, -3, 7]$

---

### Solution

#### Step 1: Rotation of $90^\circ$ about the $z$-axis
$$P_1 = R_{z, 90^\circ} \times P_{noa} = \begin{bmatrix} 0 & -1 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 7 \\ 3 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 7 \\ 1 \\ 1 \end{bmatrix}$$

#### Step 2: Rotation of $90^\circ$ about the $y$-axis
$$P_2 = R_{y, 90^\circ} \times P_1 = \begin{bmatrix} 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ -1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} -3 \\ 7 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 1 \\ 7 \\ 3 \\ 1 \end{bmatrix}$$

#### Step 3: Translation of $[4, -3, 7]$
$$P_3 = \operatorname{Tran}(4, -3, 7) \times P_2 = \begin{bmatrix} 1 & 0 & 0 & 4 \\ 0 & 1 & 0 & -3 \\ 0 & 0 & 1 & 7 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 \\ 7 \\ 3 \\ 1 \end{bmatrix} = \begin{bmatrix} 5 \\ 4 \\ 10 \\ 1 \end{bmatrix}$$

---

## Example 7: Combined Transformation (Rot Z $\to$ Trans $\to$ Rot Y)

**Problem Statement:**
A point $p = [7, 3, 1]^T$ is attached to a frame $F_{noa}$ and is subjected to:
1. Rotation of $90^\circ$ about the $z$-axis
2. Followed by a translation of $[4, -3, 7]$
3. Followed by a rotation of $90^\circ$ about the $y$-axis

---

### Solution

#### Step 1: Rotation of $90^\circ$ about the $z$-axis
$$P_1 = R_{z, 90^\circ} \times P_{noa} = \begin{bmatrix} 0 & -1 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 7 \\ 3 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 7 \\ 1 \\ 1 \end{bmatrix}$$

#### Step 2: Translation of $[4, -3, 7]$
$$P_2 = \operatorname{Tran}(4, -3, 7) \times P_1 = \begin{bmatrix} 1 & 0 & 0 & 4 \\ 0 & 1 & 0 & -3 \\ 0 & 0 & 1 & 7 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} -3 \\ 7 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 1 \\ 4 \\ 8 \\ 1 \end{bmatrix}$$

#### Step 3: Rotation of $90^\circ$ about the $y$-axis
$$P_3 = R_{y, 90^\circ} \times P_2 = \begin{bmatrix} 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ -1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 \\ 4 \\ 8 \\ 1 \end{bmatrix} = \begin{bmatrix} 8 \\ 4 \\ -1 \\ 1 \end{bmatrix}$$

---

## Pre-Multiplication vs. Post-Multiplication

When compounding successive geometric transformations (rotations and translations), the order of matrix multiplication is determined strictly by the **frame of reference** against which each transformation is defined:

| Feature | Pre-Multiplication | Post-Multiplication |
| :--- | :--- | :--- |
| **Reference System** | **Fixed / Universal Reference Frame** ($x, y, z$) | **Current / Moving Body Frame** ($n, o, a$) |
| **Matrix Placement** | Appended to the **LEFT** of the chain | Appended to the **RIGHT** of the chain |
| **Algebraic Rule** | $T_{\text{new}} = T_{\text{transform}} \times T_{\text{current}}$ | $T_{\text{new}} = T_{\text{current}} \times T_{\text{transform}}$ |
| **Physical Interpretation** | Motion occurs about fixed world axes | Motion occurs about the robot's local body axes |

### Axis Correspondence Convention
For an attached local/tool frame $F_{noa}$:
* $n$-axis: normal axis (corresponds to local $x$)
* $o$-axis: orientation axis (corresponds to local $y$)
* $a$-axis: approach axis (corresponds to local $z$)

---

## Example 8: Transformation Relative to Current (Moving) Frame

### Problem Statement
A point $p = [7, 3, 1]^T$ is attached to a frame $F_{noa}$ and is subjected to the following sequence of transformations **relative to the current moving frame**:
1. A rotation of $90^\circ$ about the $a$-axis.
2. A translation of $[4, -3, 7]$ along the current $n$-, $o$-, and $a$-axes.
3. Followed by a rotation of $90^\circ$ about the current $o$-axis.

Find the final coordinates $p_{xyz}$ relative to the reference frame.

---

### Mathematical Formulation
Because every operation is defined relative to the **current moving frame**, each subsequent operator is **post-multiplied** (appended to the right):

$$p_{xyz} = \operatorname{Rot}(a, 90^\circ) \cdot \operatorname{Trans}(4, -3, 7) \cdot \operatorname{Rot}(o, 90^\circ) \cdot p_{noa}$$

---

### Step-by-Step Numerical Calculation

We evaluate the transformation chain from right to left acting on $p_{noa} = [7, 3, 1, 1]^T$:

#### Step 1: Apply $\operatorname{Rot}(o, 90^\circ)$ to $p_{noa}$
A rotation of $90^\circ$ about the $o$-axis (local $y$-axis):
$$\operatorname{Rot}(o, 90^\circ) = \begin{bmatrix} 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ -1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

$$p_1 = \begin{bmatrix} 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ -1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 7 \\ 3 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 0(7) + 0(3) + 1(1) \\ 0(7) + 1(3) + 0(1) \\ -1(7) + 0(3) + 0(1) \\ 1 \end{bmatrix} = \begin{bmatrix} 1 \\ 3 \\ -7 \\ 1 \end{bmatrix}$$

---

#### Step 2: Apply $\operatorname{Trans}(4, -3, 7)$ to $p_1$
$$\operatorname{Trans}(4, -3, 7) = \begin{bmatrix} 1 & 0 & 0 & 4 \\ 0 & 1 & 0 & -3 \\ 0 & 0 & 1 & 7 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

$$p_2 = \begin{bmatrix} 1 & 0 & 0 & 4 \\ 0 & 1 & 0 & -3 \\ 0 & 0 & 1 & 7 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 \\ 3 \\ -7 \\ 1 \end{bmatrix} = \begin{bmatrix} 1 + 4 \\ 3 - 3 \\ -7 + 7 \\ 1 \end{bmatrix} = \begin{bmatrix} 5 \\ 0 \\ 0 \\ 1 \end{bmatrix}$$

---

#### Step 3: Apply $\operatorname{Rot}(a, 90^\circ)$ to $p_2$
A rotation of $90^\circ$ about the $a$-axis (local $z$-axis):
$$\operatorname{Rot}(a, 90^\circ) = \begin{bmatrix} 0 & -1 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

$$p_{xyz} = \begin{bmatrix} 0 & -1 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 5 \\ 0 \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 0(5) - 1(0) \\ 1(5) + 0(0) \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 0 \\ 5 \\ 0 \\ 1 \end{bmatrix}$$

$$\mathbf{p}_{xyz} = \begin{bmatrix} 0 \\ 5 \\ 0 \end{bmatrix}$$

---

## Example 9: Combined Transformation Relative to Both Frames

### Problem Statement
A frame $B$ is subjected to the following sequence of motions:
1. Rotation about **the fixed $x$-axis by $90^\circ$**.
2. Translation along the **current $o$-axis of $2\text{ inches}$**.
3. Rotation about the **current $a$-axis of $90^\circ$**.
4. Translation along the **fixed reference $y$-axis of $3\text{ inches}$**.

**Tasks:**
1. Write the composite transformation equation describing the motions.
2. Find the final location of a point $^Bp = [1, 3, 2]^T$ relative to the reference frame.

---

### Step 1: Formulate the Matrix Equation

Building the chain around frame $[B]$ step-by-step:
1. **Fixed $x$-axis rotation:** Pre-multiply $\implies \operatorname{Rot}(x, 90^\circ) [B]$
2. **Current $o$-axis translation ($y$-direction):** Post-multiply $\implies \operatorname{Rot}(x, 90^\circ) [B] \operatorname{Trans}(0, 2, 0)$
3. **Current $a$-axis rotation ($z$-axis):** Post-multiply $\implies \operatorname{Rot}(x, 90^\circ) [B] \operatorname{Trans}(0, 2, 0) \operatorname{Rot}(a, 90^\circ)$
4. **Fixed $y$-axis translation:** Pre-multiply $\implies \operatorname{Trans}(0, 3, 0)$ on the far left

$$^{U}T_B = \underbrace{\operatorname{Trans}(0, 3, 0)}_{\text{Pre (ref)}} \cdot \underbrace{\operatorname{Rot}(x, 90^\circ)}_{\text{Pre (ref)}} \cdot [B] \cdot \underbrace{\operatorname{Trans}(0, 2, 0)}_{\text{Post (moving)}} \cdot \underbrace{\operatorname{Rot}(a, 90^\circ)}_{\text{Post (moving)}}$$

---

### Step 2: Numerical Calculation of Point Position

Given $[B] = I_{4 \times 4}$ and $^Bp = [1, 3, 2, 1]^T$:

$$^Up = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 3 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & -1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 2 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0 & -1 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 \\ 3 \\ 2 \\ 1 \end{bmatrix}$$

Evaluating right-to-left:

* **Step A: Apply $\operatorname{Rot}(a, 90^\circ)$ to $^Bp$**
  $$p_A = \begin{bmatrix} 0 & -1 & 0 & 0 \\ 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 \\ 3 \\ 2 \\ 1 \end{bmatrix} = \begin{bmatrix} 0(1) - 1(3) \\ 1(1) + 0(3) \\ 2 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 1 \\ 2 \\ 1 \end{bmatrix}$$

* **Step B: Apply $\operatorname{Trans}(0, 2, 0)$ to $p_A$**
  $$p_B = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 2 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} -3 \\ 1 \\ 2 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 1 + 2 \\ 2 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 3 \\ 2 \\ 1 \end{bmatrix}$$

* **Step C: Pass through identity matrix $[B]$**
  $$p_C = p_B = \begin{bmatrix} -3 \\ 3 \\ 2 \\ 1 \end{bmatrix}$$

* **Step D: Apply $\operatorname{Rot}(x, 90^\circ)$ to $p_C$**
  $$p_D = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & -1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} -3 \\ 3 \\ 2 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ -1(2) \\ 1(3) \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ -2 \\ 3 \\ 1 \end{bmatrix}$$

* **Step E: Apply $\operatorname{Trans}(0, 3, 0)$ to $p_D$**
  $$^Up = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 3 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} -3 \\ -2 \\ 3 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ -2 + 3 \\ 3 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 1 \\ 3 \\ 1 \end{bmatrix}$$

$$\mathbf{^Up} = \begin{bmatrix} -3 \\ 1 \\ 3 \end{bmatrix}$$

$[B]$ represents the **initial pose (position and orientation) of frame $B$ before any new transformations are applied**.

  

It serves two main purposes:

  

### 1. The Physical Anchor / Starting Point

If frame $B$ is already tilted or shifted relative to the universe before the problem starts, $[B]$ contains that initial transformation matrix:

  

$$[B] = \begin{bmatrix} n_x & o_x & a_x & P_x \\ n_y & o_y & a_y & P_y \\ n_z & o_z & a_z & P_z \\ 0 & 0 & 0 & 1 \end{bmatrix}$$

  

- In that specific slide problem, frame $B$ starts out coincident with the reference frame, which is why $[B]$ is written as the $4 \times 4$ **identity matrix** $I$:
    
      
    
    $$[B] = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}$$
    
      
    
- Even though multiplying by $I$ does not change numerical values, writing $[B]$ explicitly preserves the general formula so the same pipeline works if frame $B$ begins at an arbitrary pose.
    
      
    

### 2. The Dividing Boundary Between Pre- and Post-Multiplication

In a combined transformation problem, $[B]$ acts as the **structural boundary** separating operations:

  

$$^{U}T_B = \underbrace{\Big( \dots \text{Fixed Reference Transformations} \dots \Big)}_{\text{Pre-multiplied (Left side)}} \cdot \mathbf{[B]} \cdot \underbrace{\Big( \dots \text{Moving Body Transformations} \dots \Big)}_{\text{Post-multiplied (Right side)}}$$

  

- Everything done relative to the **fixed world axes** stacks outward to the **left** of $[B]$.
    
      
    
- Everything done relative to the **moving local axes** stacks outward to the **right** of $[B]$.