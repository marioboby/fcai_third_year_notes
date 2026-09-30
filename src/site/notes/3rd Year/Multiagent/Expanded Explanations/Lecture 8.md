---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/expanded-explanations/lecture-8/","dg-note-properties":{}}
---


# Genetic Algorithm Example (from Quiz 1)

Suppose a genetic algorithm uses chromosomes of the form x = `abcdefgh` with a fixed length
of **8 genes**. Each gene can be any digit between 0 and 9. Let the fitness of individual x be
calculated as:
$$f (x) = (a + b)(c + d) + (e + f )(g + h)$$
and let the initial population consist of four individuals with the following chromosomes:
- x1 = 2 3 9 2 1 2 8 5
- x2 = 6 5 4 1 3 5 3 2
- x3 = 8 7 1 2 6 6 0 1
- x4 = 4 1 8 5 2 0 9 4

if individual operations are defined as:-
- Cross operation: a one point crossover at the middle point.
- Mutation operation: third digit only in last generated oﬀspring.
- Roulette wheel : when it rotated the outcome will be {3, 1, 4, 2, 1, 2, 4, 3, 4, 1, 4, 5, 1, 3, 2, 6, 4, 2, 3, 5}

(a) Perform two complete cycles of genetic algorithm, each generation consists of 6 chromosomes.

---
## Sol

_(Note: Because the problem states "mutate" without giving a specific deterministic rule like "flip" or "+1", I will change the target digit to a `0` when a mutation occurs, just to make the change obvious)._

### 1. Initial Setup: The True Fitness Values

We calculate the fitness using the formula $f(x) = (a + b)(c + d) + (e + f)(g + h)$.

- **$x_1$** (2 3 9 2 1 2 8 5): $f(x_1) = (2+3)(9+2) + (1+2)(8+5) = 55 + 39 =$ **94**
    
- **$x_2$** (6 5 4 1 3 5 3 2): $f(x_2) = (6+5)(4+1) + (3+5)(3+2) = 55 + 40 =$ **95**
    
- **$x_3$** (8 7 1 2 6 6 0 1): $f(x_3) = (8+7)(1+2) + (6+6)(0+1) = 45 + 12 =$ **57**
    
- **$x_4$** (4 1 8 5 2 0 9 4): $f(x_4) = (4+1)(8+5) + (2+0)(9+4) = 65 + 26 =$ **91**
    

**Initial Ranking (Highest to Lowest):**

- Index 1: **$x_2$** (95)
    
- Index 2: **$x_1$** (94)
    
- Index 3: **$x_4$** (91)
    
- Index 4: **$x_3$** (57)
    

### 2. First Generation Cycle

We need 6 chromosomes for the new generation. We will keep the highest-performing chromosome directly (Elitism) and generate 5 new offspring.

#### **Selection (First 10 Roulette Wheel Outputs: `[3, 1, 4, 2, 1, 2, 4, 3, 4, 1]`)**

Mapping these numbers to our new indices gives us the breeding pairs:

- (3, 1) $\rightarrow$ (**$x_4$**, **$x_2$**)
    
- (4, 2) $\rightarrow$ (**$x_3$**, **$x_1$**)
    
- (1, 2) $\rightarrow$ (**$x_2$**, **$x_1$**)
    
- (4, 3) $\rightarrow$ (**$x_3$**, **$x_4$**)
    
- (4, 1) $\rightarrow$ (**$x_3$**, **$x_2$**)
    

#### **Crossover Operation**

We split each parent at the middle point (after the 4th digit) and combine the left side of the first parent with the right side of the second.

- ($x_4$, $x_2$) $\rightarrow$ Left of $x_4$ (`4 1 8 5`), Right of $x_2$ (`3 5 3 2`) $\rightarrow$ **$x_5$ = 4 1 8 5 3 5 3 2**
    
- ($x_3$, $x_1$) $\rightarrow$ Left of $x_3$ (`8 7 1 2`), Right of $x_1$ (`1 2 8 5`) $\rightarrow$ **$x_6$ = 8 7 1 2 1 2 8 5**
    
- ($x_2$, $x_1$) $\rightarrow$ Left of $x_2$ (`6 5 4 1`), Right of $x_1$ (`1 2 8 5`) $\rightarrow$ **$x_7$ = 6 5 4 1 1 2 8 5**
    
- ($x_3$, $x_4$) $\rightarrow$ Left of $x_3$ (`8 7 1 2`), Right of $x_4$ (`2 0 9 4`) $\rightarrow$ **$x_8$ = 8 7 1 2 2 0 9 4**
    
- ($x_3$, $x_2$) $\rightarrow$ Left of $x_3$ (`8 7 1 2`), Right of $x_2$ (`3 5 3 2`) $\rightarrow$ **$x_9$ = 8 7 1 2 3 5 3 2**
    

#### **Mutation Operation**

We mutate the third digit of the _last_ generated offspring, which is $x_9$.

- $x_9$ is `8 7 1 2 3 5 3 2`. The third digit is `1`.
    
- Mutating that digit (let's set it to `0`) gives us our final mutated offspring: **$x_9'$ = 8 7 0 2 3 5 3 2**
    

#### **Final First Generation:**

We pull down our elite champion, **$x_2$**, to join the 5 offspring.

The pool is: **$x_2$**, **$x_5$**, **$x_6$**, **$x_7$**, **$x_8$**, **$x_9'$**

### 3. Second Generation Cycle

We repeat the evaluation and selection process on the new generation.

**Calculate New Fitness Values & Rank:**

- $f(x_2)$ = **95**
    
- $f(x_5)$ = $(4+1)(8+5) + (3+5)(3+2) = 65 + 40 =$ **105** _(New Champion!)_
    
- $f(x_6)$ = $(8+7)(1+2) + (1+2)(8+5) = 45 + 39 =$ **84**
    
- $f(x_7)$ = $(6+5)(4+1) + (1+2)(8+5) = 55 + 39 =$ **94**
    
- $f(x_8)$ = $(8+7)(1+2) + (2+0)(9+4) = 45 + 26 =$ **71**
    
- $f(x_9')$ = $(8+7)(0+2) + (3+5)(3+2) = 30 + 40 =$ **70**
    

**Generation 1 Ranking:**

- Index 1: **$x_5$** (105)
    
- Index 2: **$x_2$** (95)
    
- Index 3: **$x_7$** (94)
    
- Index 4: **$x_6$** (84)
    
- Index 5: **$x_8$** (71)
    
- Index 6: **$x_9'$** (70)
    

**Selection (Remaining Roulette Wheel Outputs: `[4, 5, 1, 3, 2, 6, 4, 2, 3, 5]`)**

Mapping these numbers to our Gen 1 indices:

- (4, 5) $\rightarrow$ (**$x_6$**, **$x_8$**)
    
- (1, 3) $\rightarrow$ (**$x_5$**, **$x_7$**)
    
- (2, 6) $\rightarrow$ (**$x_2$**, **$x_9'$**)
    
- (4, 2) $\rightarrow$ (**$x_6$**, **$x_2$**)
    
- (3, 5) $\rightarrow$ (**$x_7$**, **$x_8$**)
    

**Crossover Operation**

- ($x_6$, $x_8$) $\rightarrow$ Left of $x_6$ (`8 7 1 2`), Right of $x_8$ (`2 0 9 4`) $\rightarrow$ **$x_{10}$ = 8 7 1 2 2 0 9 4**
    
- ($x_5$, $x_7$) $\rightarrow$ Left of $x_5$ (`4 1 8 5`), Right of $x_7$ (`1 2 8 5`) $\rightarrow$ **$x_{11}$ = 4 1 8 5 1 2 8 5**
    
- ($x_2$, $x_9'$) $\rightarrow$ Left of $x_2$ (`6 5 4 1`), Right of $x_9'$ (`3 5 3 2`) $\rightarrow$ **$x_{12}$ = 6 5 4 1 3 5 3 2**
    
- ($x_6$, $x_2$) $\rightarrow$ Left of $x_6$ (`8 7 1 2`), Right of $x_2$ (`3 5 3 2`) $\rightarrow$ **$x_{13}$ = 8 7 1 2 3 5 3 2**
    
- ($x_7$, $x_8$) $\rightarrow$ Left of $x_7$ (`6 5 4 1`), Right of $x_8$ (`2 0 9 4`) $\rightarrow$ **$x_{14}$ = 6 5 4 1 2 0 9 4**
    

**Mutation Operation**

We mutate the third digit of $x_{14}$.

- $x_{14}$ is `6 5 4 1 2 0 9 4`. The third digit is `4`.
    
- Mutating that digit (changing to `0`) gives us: **$x_{14}'$ = 6 5 0 1 2 0 9 4**
    

**Final Second Generation:**

We pull down our Generation 1 elite champion, **$x_5$**, to join the pool.

The final 6 chromosomes are: **$x_5$**, **$x_{10}$**, **$x_{11}$**, **$x_{12}$**, **$x_{13}$**, **$x_{14}'$**

---

# What is roulette wheel array exactly

The numbers in the roulette wheel array do not refer to the literal chromosome names (like x1 or x2). Instead, they represent the **rank index** of the chromosomes after they have been sorted by their fitness score from highest to lowest.

The wheel is simply an array of predefined, 1-based indices pointing to a fitness-ranked list. When the printed solution deviates from this logic, it is due to human transcription errors by whoever wrote the key.

---

# Another Example (from Final 23)

Suppose a genetic algorithm uses chromosomes of the form x = `abcdefghi` with a fixed length
of nine genes. Each gene can be 0 or 1. Let the fitness of individual x be calculated as:

$$f(x) = a + b + c + d + e + f + g + h + i$$ 
> Simply the number on 1's 

and let the initial population consist of four individuals with the following chromosomes:

x1 = 100010111
x2 = 100000001
x3 = 010101010
x4 = 010100110
x5 = 001100111
x6 = 110110110

if individual operations are defined as:-
- **Cross operation**: a onepoint crossover at the middle point.
- **Mutation operation**: third digit only in last generated offspring.
- **Roulette wheel** : when it rotated the outcome will be {3, 1, 4, 2, 1, 2, 4, 3, 4, 1, 4, 5, 1, 3, 2, 6, 4, 2, 3, 5}

(a) Perform two complete cycles of genetic algorithm, each generation consists of 6 chromo-
somes .

---

## Sol

Before we begin the calculations, we need to address two contradictions in the problem statement itself to ensure the logic holds up:

1. **The Population Contradiction:** The prompt explicitly states the initial population consists of _"four individuals"_ but then lists six ($x_1$ through $x_6$). Furthermore, the first half of the roulette wheel array (`3, 1, 4, 2, 1, 2...`) only contains numbers up to 4. This confirms the wheel was mathematically designed for a starting population of 4. Therefore, I will use **$x_1, x_2, x_3, x_4$** as the initial population and discard $x_5$ and $x_6$ as typos from the author.
    
2. **The Crossover Point:** The chromosomes have an odd number of genes (9). We must define a strict "middle" point. For this solution, the split will happen **after the 4th gene** (Left side = 4 genes, Right side = 5 genes).
    
3. **The Mutation Rule:** Because the genes are binary (0 or 1), a mutation is a simple bit-flip.
    
### 1. Initial Setup: The True Fitness Values

The fitness function is $f(x) = a+b+c+d+e+f+g+h+i$, which simply means counting the number of `1`s in the binary string.

- **$x_1$** (`1 0 0 0 1 0 1 1 1`): $f(x_1) = 1+0+0+0+1+0+1+1+1 =$ **5**
    
- **$x_2$** (`1 0 0 0 0 0 0 0 1`): $f(x_2) = 1+0+0+0+0+0+0+0+1 =$ **2**
    
- **$x_3$** (`0 1 0 1 0 1 0 1 0`): $f(x_3) = 0+1+0+1+0+1+0+1+0 =$ **4**
    
- **$x_4$** (`0 1 0 1 0 0 1 1 0`): $f(x_4) = 0+1+0+1+0+0+1+1+0 =$ **4**
    

**Initial Ranking (Highest to Lowest)**

_(Note: We use standard stable sorting to resolve the tie between $x_3$ and $x_4$, maintaining their original order of appearance)._

- Index 1: **$x_1$** (Fitness: 5)
    
- Index 2: **$x_3$** (Fitness: 4)
    
- Index 3: **$x_4$** (Fitness: 4)
    
- Index 4: **$x_2$** (Fitness: 2)
    

### 2. First Generation Cycle

To create a generation of 6, we carry over the best performer (Elitism) and generate 5 new offspring.

**Selection (First 10 Roulette Wheel Outputs: `[3, 1, 4, 2, 1, 2, 4, 3, 4, 1]`)**

Mapping these numbers to our ranked indices gives us the breeding pairs:

- (3, 1) $\rightarrow$ (**$x_4$**, **$x_1$**)
    
- (4, 2) $\rightarrow$ (**$x_2$**, **$x_3$**)
    
- (1, 2) $\rightarrow$ (**$x_1$**, **$x_3$**)
    
- (4, 3) $\rightarrow$ (**$x_2$**, **$x_4$**)
    
- (4, 1) $\rightarrow$ (**$x_2$**, **$x_1$**)
    

**Crossover Operation (Split after 4th gene)**

- ($x_4$, $x_1$) $\rightarrow$ Left of $x_4$ (`0101`) + Right of $x_1$ (`10111`) $\rightarrow$ **$x_5$ = 0 1 0 1 1 0 1 1 1**
    
- ($x_2$, $x_3$) $\rightarrow$ Left of $x_2$ (`1000`) + Right of $x_3$ (`01010`) $\rightarrow$ **$x_6$ = 1 0 0 0 0 1 0 1 0**
    
- ($x_1$, $x_3$) $\rightarrow$ Left of $x_1$ (`1000`) + Right of $x_3$ (`01010`) $\rightarrow$ **$x_7$ = 1 0 0 0 0 1 0 1 0**
    
- ($x_2$, $x_4$) $\rightarrow$ Left of $x_2$ (`1000`) + Right of $x_4$ (`00110`) $\rightarrow$ **$x_8$ = 1 0 0 0 0 0 1 1 0**
    
- ($x_2$, $x_1$) $\rightarrow$ Left of $x_2$ (`1000`) + Right of $x_1$ (`10111`) $\rightarrow$ **$x_9$ = 1 0 0 0 1 0 1 1 1**
    

**Mutation Operation**

We mutate the third digit of the _last_ generated offspring, $x_9$.

- $x_9$ is `1 0 0 0 1 0 1 1 1`. The third digit is a `0`.
    
- Flipping it to `1` gives us the mutated offspring: **$x_9'$ = 1 0 1 0 1 0 1 1 1**
    

**Final First Generation:**

Bringing down the elite champion **$x_1$**, our Gen 1 pool is: **$x_1$, $x_5$, $x_6$, $x_7$, $x_8$, $x_9'$**

### 3. Second Generation Cycle

We repeat the evaluation and selection process on the Gen 1 pool.

**Calculate New Fitness Values & Rank:**

- $f(x_1)$ = **5**
    
- $f(x_5)$ = 0+1+0+1+1+0+1+1+1 = **6** _(New Champion!)_
    
- $f(x_6)$ = 1+0+0+0+0+1+0+1+0 = **3**
    
- $f(x_7)$ = 1+0+0+0+0+1+0+1+0 = **3**
    
- $f(x_8)$ = 1+0+0+0+0+0+1+1+0 = **3**
    
- $f(x_9')$ = 1+0+1+0+1+0+1+1+1 = **6**
    

**Generation 1 Ranking (Highest to Lowest, Stable Sort):**

- Index 1: **$x_5$** (Fitness: 6)
    
- Index 2: **$x_9'$** (Fitness: 6)
    
- Index 3: **$x_1$** (Fitness: 5)
    
- Index 4: **$x_6$** (Fitness: 3)
    
- Index 5: **$x_7$** (Fitness: 3)
    
- Index 6: **$x_8$** (Fitness: 3)
    

**Selection (Remaining Roulette Wheel Outputs: `[4, 5, 1, 3, 2, 6, 4, 2, 3, 5]`)**

Mapping these numbers to our Gen 1 indices:

- (4, 5) $\rightarrow$ (**$x_6$**, **$x_7$**)
    
- (1, 3) $\rightarrow$ (**$x_5$**, **$x_1$**)
    
- (2, 6) $\rightarrow$ (**$x_9'$**, **$x_8$**)
    
- (4, 2) $\rightarrow$ (**$x_6$**, **$x_9'$**)
    
- (3, 5) $\rightarrow$ (**$x_1$**, **$x_7$**)
    

**Crossover Operation**

- ($x_6$, $x_7$) $\rightarrow$ Left of $x_6$ (`1000`) + Right of $x_7$ (`01010`) $\rightarrow$ **$x_{10}$ = 1 0 0 0 0 1 0 1 0**
    
- ($x_5$, $x_1$) $\rightarrow$ Left of $x_5$ (`0101`) + Right of $x_1$ (`10111`) $\rightarrow$ **$x_{11}$ = 0 1 0 1 1 0 1 1 1**
    
- ($x_9'$, $x_8$) $\rightarrow$ Left of $x_9'$ (`1010`) + Right of $x_8$ (`00110`) $\rightarrow$ **$x_{12}$ = 1 0 1 0 0 0 1 1 0**
    
- ($x_6$, $x_9'$) $\rightarrow$ Left of $x_6$ (`1000`) + Right of $x_9'$ (`10111`) $\rightarrow$ **$x_{13}$ = 1 0 0 0 1 0 1 1 1**
    
- ($x_1$, $x_7$) $\rightarrow$ Left of $x_1$ (`1000`) + Right of $x_7$ (`01010`) $\rightarrow$ **$x_{14}$ = 1 0 0 0 0 1 0 1 0**
    

**Mutation Operation**

We mutate the third digit of $x_{14}$.

- $x_{14}$ is `1 0 0 0 0 1 0 1 0`. The third digit is a `0`.
    
- Flipping it to `1` gives us: **$x_{14}'$ = 1 0 1 0 0 1 0 1 0**
    

**Final Second Generation:**

We bring down our Gen 1 elite champion, **$x_5$**, to join the pool. The final 6 chromosomes resulting from the second cycle are: **$x_5$, $x_{10}$, $x_{11}$, $x_{12}$, $x_{13}$, $x_{14}'$**