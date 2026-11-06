---
{"dg-publish":true,"permalink":"/4th-year/1st-term/robotics/numerical-examples/2-forward-kinematics/","dg-note-properties":{}}
---


## Forward Kinematics (2-DOF Planar Arm)

### 1. Mathematical Model

For a planar two-link manipulator anchored at the origin $(0, 0)$:
- Link 1 length: $l_1$, joint angle relative to $x$-axis: $\alpha$
- Link 2 length: $l_2$, relative elbow angle: $\beta$

**Elbow Position $(x_1, y_1)$:**
$$x_1 = l_1 \cos\alpha$$
$$y_1 = l_1 \sin\alpha$$

**End-Effector Position $(x, y)$:**
$$x = l_1 \cos\alpha + l_2 \cos(\alpha + \beta)$$
$$y = l_1 \sin\alpha + l_2 \sin(\alpha + \beta)$$

---

### 2. Numerical Example

#### Given Parameters
* **Link lengths:** $l_1 = 2.0\text{ m}$, $l_2 = 1.5\text{ m}$
* **Joint angles:** $\alpha = 30^\circ = \frac{\pi}{6}\text{ rad}$, $\beta = 45^\circ = \frac{\pi}{4}\text{ rad}$

---

#### Step 1: Joint Limits & Configuration Space Check
Given standard kinematic limits:
* $\alpha \in [0, \pi]$ or $\left(-\frac{\pi}{2}, \frac{\pi}{2}\right)$: 
  $$\alpha = 30^\circ \implies 0 \le 30^\circ \le 180^\circ \quad \text{(Valid)}$$
* $\beta \in (-\pi, \pi)$: 
  $$\beta = 45^\circ \implies -180^\circ < 45^\circ < 180^\circ \quad \text{(Valid)}$$

---

#### Step 2: Intermediate Joint Position $(x_1, y_1)$
$$\cos(30^\circ) = \frac{\sqrt{3}}{2} \approx 0.8660, \quad \sin(30^\circ) = 0.5$$

* **$x_1$:**
  $$x_1 = 2.0 \times \cos(30^\circ) = 2.0 \times 0.8660 = 1.7320\text{ m}$$
* **$y_1$:**
  $$y_1 = 2.0 \times \sin(30^\circ) = 2.0 \times 0.5 = 1.0000\text{ m}$$

$$\mathbf{P}_1 = (1.732, 1.000)\text{ m}$$

---

#### Step 3: End-Effector Absolute Angle $(\alpha + \beta)$
$$\theta_{\text{total}} = \alpha + \beta = 30^\circ + 45^\circ = 75^\circ$$
$$\cos(75^\circ) \approx 0.2588, \quad \sin(75^\circ) \approx 0.9659$$

---

#### Step 4: End-Effector Position $(x, y)$
* **$x$ coordinate:**
  $$x = x_1 + l_2 \cos(\alpha + \beta)$$
  $$x = 1.7320 + 1.5 \times \cos(75^\circ)$$
  $$x = 1.7320 + 1.5 \times 0.2588 = 1.7320 + 0.3882 = 2.1202\text{ m}$$

* **$y$ coordinate:**
  $$y = y_1 + l_2 \sin(\alpha + \beta)$$
  $$y = 1.0000 + 1.5 \times \sin(75^\circ)$$
  $$y = 1.0000 + 1.5 \times 0.9659 = 1.0000 + 1.4489 = 2.4489\text{ m}$$

$$\mathbf{P}_{\text{EE}} = (2.120, 2.449)\text{ m}$$