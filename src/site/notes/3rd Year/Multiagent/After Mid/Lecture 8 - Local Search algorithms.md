---
{"dg-publish":true,"permalink":"/3rd-year/multiagent/after-mid/lecture-8-local-search-algorithms/","dg-note-properties":{}}
---


In the previous lectures, the _path_ to the goal was the entire point of the search (e.g., finding the shortest sequence of boat trips or puzzle slides). In local search, the path is completely irrelevant; the only thing that matters is the final state or configuration itself.

Because they don't keep track of the paths they traverse, local search algorithms have two massive advantages: they use very little memory (usually a constant amount), and they can find reasonable solutions in exceptionally large or infinite (continuous) state spaces where systematic algorithms like A* would completely crash.

Here is a detailed breakdown of the four algorithms covered:

### 1. Hill-Climbing Search (Greedy Local Search)

Imagine the state space as a landscape of hills and valleys, where elevation represents the objective function (the "fitness" or quality of the state). Hill-climbing operates by looking at its immediate neighbors and simply moving to the highest one. It does not think ahead about where to go next.

- **The Flaw:** While it makes rapid progress initially because it is easy to improve a bad state, it frequently gets stuck.
    
- **Local Maxima:** It reaches a peak that is higher than its immediate neighbors, but lower than the true global maximum. Because all immediate steps go "down," the algorithm halts, permanently trapped.
    
- **Plateaux & Ridges:** It can also get completely lost wandering on flat areas (plateaux) or struggle to navigate narrow sequences of local maxima (ridges).
    

### 2. Simulated Annealing

To fix the "getting stuck" problem of Hill-Climbing, Simulated Annealing introduces controlled randomness. It is inspired by metallurgy, where metals are heated to high temperatures and gradually cooled to allow the material to reach a low-energy, stable crystalline state.

- **The Mechanism:** Instead of always picking the _best_ move, it picks a _random_ move. If the move improves the situation, it is accepted. If the move is _worse_, it might still be accepted based on a specific probability: $e^{\Delta E/T}$.
    
- **The Role of Temperature ($T$):** The search begins with a high temperature. When $T$ is high, the probability of accepting a bad move is close to 1, allowing the algorithm to freely explore and bounce out of local maxima. As the algorithm progresses, the temperature slowly drops based on a cooling schedule. When $T$ becomes low, it strictly exploits good moves, behaving just like hill-climbing to settle on the absolute highest peak.
    

### 3. Local Beam Search

Instead of keeping just one node in memory, Local Beam Search keeps track of $k$ randomly generated states. At each step, it generates all successors for _all_ $k$ states, pools them together into one complete list, and selects the absolute best $k$ successors to continue the search.

### 4. Genetic Algorithms (GAs)

A Genetic Algorithm is a variant of stochastic beam search inspired by natural selection. Instead of just modifying single states, it generates successors by combining two "parent" states.

- **Population:** The set of candidate solutions.
    
- **Fitness Function:** Evaluates how "good" a solution is (e.g., measuring the speed of a rabbit).
    
- **Selection & Crossover:** Parents with superior fitness are selected to combine their "genes" to produce offspring, with the hope that the combination yields even higher fitness.
    
- **Mutation:** Occasional random changes are applied to the offspring to maintain diversity in the population and prevent the algorithm from stagnating.
    
---
## Code Example for Genetic Algo (8-Queen Problem)

```python
def create_gene():
    gen = tuple([np.random.randint(1, 9) for i in range(8)])
    return gen

def calculate_fitness(gen):
    fitness = 28

    for i in range(8):
        for j in range(i + 1, 8):
            if gen[i] == gen[j] or abs(gen[i] - gen[j]) == abs(i - j):
                fitness -= 1

    return fitness

def create_population(max_population : int):
    populasi = {}
    target_len = 8

    for i in range(max_population):
        gen = create_gene()
        genFitness = calculate_fitness(gen)
        populasi[gen] = genFitness
    return populasi

def selection(populasi : dict):
    pop = dict(populasi)
    parent = {}

    for i in range(4):
        gen = bestgen(pop)  
        genfitness = pop[gen]
        parent[gen] = genfitness
        del pop[gen]  
    return parent

def crossover(parent : dict):
    children = {}
    cp = 4  
    
    for i in range(4):
        gen = tuple(list(parent)[i][:cp] + list(parent)[3-i][cp:])
        genfitness = calculate_fitness(gen)
        children[gen] = genfitness

    return children

def mutation(children : dict, mutation_rate : float):
    mutant = {}
    
    for i in range(len(children)):    
        data = list(list(children)[i])

        for j in range(len(data)):
            if np.random.rand() <= mutation_rate:
                data[j] = np.random.randint(1,9)  

        gen = tuple(data)
        genfitness = calculate_fitness(gen)
        mutant[gen] = genfitness
    return mutant

def regeneration(mutant, populasi):
    # Remove worst genes from population (equal to number of mutants)
    for i in range(len(mutant)):
        bad_gen = min(populasi, key=populasi.get)  
        del populasi[bad_gen]
        
    populasi.update(mutant)
    return populasi

def bestgen(parent):
    gen = max(parent, key=parent.get)
    return gen

def bestfitness(parent):
    fitness = parent[bestgen(parent)]
    return fitness
```



