---
{"dg-publish":true,"permalink":"/4th-year/1st-term/ga-ns/expanded-explanations/lecture-3/","dg-note-properties":{}}
---



# Stanford CS236 — Deep Generative Models I

## Lecture 3: Autoregressive Models

المحاضرة كلها اتفرغت هنا من البلاي ليست لو حد عايز شرح مفصل

> [!Abstract]
> 
> This lecture introduces **autoregressive models**, the first major family of generative models discussed in the course. The central idea is to use the **probability chain rule** to decompose a complicated joint probability distribution into a sequence of simpler conditional probability distributions.
> 
> The lecture develops this idea from simple logistic regression models to neural autoregressive models, masked neural networks, and recurrent neural networks. It also explains the fundamental tradeoff of autoregressive models: they can evaluate probabilities efficiently and can represent very flexible distributions, but generation is inherently sequential.

---

## 1. Generative Modeling: Where We Are Starting

Before introducing autoregressive models, it is useful to recall the general problem of generative modeling.

Suppose we have data sampled independently and identically distributed from some unknown probability distribution:

$$  
x^{(1)}, x^{(2)}, \ldots, x^{(N)} \sim P_{\text{data}}(x)  
$$

The actual data distribution $P_{\text{data}}$ is unknown.

We only have access to samples from it.

### The general recipe

To train a generative model, we need to:

1. Define a **family of probability distributions** over the same space as the data.
    
2. Parameterize this family using some parameters, denoted by:
    

$$  
\theta  
$$

3. Define some notion of similarity or divergence between the model distribution and the data distribution.
    
4. Optimize $\theta$ so that the model distribution becomes a good approximation of the data distribution.
    

In this lecture, the parameters $\theta$ will typically be the parameters of a neural network.

### Example:

If we are modeling images, our model defines a probability distribution over possible images:

$$  
P_\theta(x)  
$$

The goal is for:

$$  
P_\theta(x) \approx P_{\text{data}}(x)  
$$

---

# 2. What Can We Do Once We Have a Probability Distribution?

Having a generative model gives us several useful capabilities.

## 2.1 Sampling

We can sample a new data point:

$$  
x \sim P_\theta(x)  
$$

This is what allows the model to **generate new data**.

For example, if $P_\theta(x)$ models handwritten digits, we can sample from it to generate new handwritten digits.

---

## 2.2 Evaluating Probabilities

We may want to evaluate:

$$  
P_\theta(x)  
$$

for a particular input $x$.

This is useful because probability evaluation gives us a natural way to train a model using **maximum likelihood**.

The idea is to choose parameters $\theta$ that maximize the probability assigned to the observed training data.

For a dataset $\mathcal{D}$:
$$
\arg\max_\theta  
P_\theta(\mathcal{D})  
$$

More commonly, because the examples are assumed to be independent:
$$
\arg\max_\theta  
\prod_{n=1}^{N}  
P_\theta(x^{(n)})  
$$

or equivalently, using log-probabilities:
$$
\arg\max_\theta  
\sum_{n=1}^{N}  
\log P_\theta(x^{(n)})  
$$

The details of learning will be discussed in the next lecture.

---

## 2.3 Anomaly Detection

Another application is **anomaly detection**.

Given an input $x$, we can ask:

> How likely is this input under the learned distribution?

That means evaluating:

$$  
P_\theta(x)  
$$

If an input receives a very low probability, it may indicate that the input is unusual relative to the data distribution.

### Example:

Suppose a generative model was trained on natural images.

If it receives an unusual image, we might find that:

$$  
P_\theta(x)  
$$

is relatively low.

This could potentially indicate that the input is an anomaly.

The lecture also mentions the possibility of identifying things such as adversarial examples if they differ sufficiently from the natural data distribution.

---

## 2.4 Unsupervised Representation Learning

Generative models can also potentially learn useful representations.

To model a complicated data distribution successfully, the model may need to discover structure in the data.

For some generative model families, useful features can emerge naturally as a byproduct of learning the data distribution.

The lecture notes, however, that this can be somewhat tricky for autoregressive models.

---

# 3. The Main Idea: Autoregressive Models

The central idea behind autoregressive models is the **probability chain rule**.

The chain rule allows us to take a complicated joint distribution over many variables and rewrite it as a product of simpler conditional distributions.

This is the fundamental mathematical trick behind autoregressive modeling.

---

# 4. The Chain Rule of Probability

Suppose we have four random variables:

$$  
X_1, X_2, X_3, X_4  
$$

The joint probability can always be written as:
$$
P(X_1)  
P(X_2\mid X_1)  
P(X_3\mid X_1,X_2)  
P(X_4\mid X_1,X_2,X_3)  
$$

This is not an approximation.

It is an **exact factorization**.

### Important point

The chain rule works for **any probability distribution**.

We do not need to assume that the variables are independent.

We do not need to assume that the distribution has a particular structure.

---

# 5. The Ordering Is Not Unique

The chain rule also allows us to choose different orderings.

For example, instead of ordering the variables as:

$$  
X_1 \rightarrow X_2 \rightarrow X_3 \rightarrow X_4  
$$

we could choose:

$$  
X_4 \rightarrow X_3 \rightarrow X_2 \rightarrow X_1  
$$

and write:
$$
P(X_4)  
P(X_3\mid X_4)  
P(X_2\mid X_4,X_3)  
P(X_1\mid X_4,X_3,X_2)  
$$

Both factorizations are exactly correct.

### Important distinction

The chain rule itself does **not care** about the ordering.

However, the ordering can have a major effect on how easy the individual conditional distributions are to model.

---

## Analogy:

Imagine predicting a sequence.

If there is a natural direction in the data, predicting the next element from the previous elements may be relatively easy.

For a time series, for example, the ordering is naturally:

$$  
X_1 \rightarrow X_2 \rightarrow X_3 \rightarrow \cdots  
$$

because time provides a natural ordering.

For an image, however, there is no obvious causal direction.

---

# 6. Why Ordering Matters

Although every ordering gives an exact factorization, some orderings may make the resulting conditional distributions easier to learn.

If the data has some natural causal structure, following that structure may simplify prediction.

However:

> The chain rule remains mathematically valid regardless of the ordering.

This distinction becomes particularly important for images.

---

# 7. Bayesian Networks vs. Neural Autoregressive Models

The lecture contrasts two ways of dealing with the conditional distributions produced by the chain rule.

## 7.1 Bayesian Networks

Bayesian networks exploit conditional independence assumptions.

Instead of representing every possible conditional probability explicitly, they assume that certain variables are conditionally independent of others.

This can significantly reduce the complexity of the representation.

However, representing complicated conditional distributions using explicit tables does not scale well when the number of variables becomes large.

---

## 7.2 Neural Autoregressive Models

The other approach is to use a neural network.

Instead of storing a conditional distribution as a giant lookup table, we learn a function:

$$  
f_\theta(\cdot)  
$$

that maps the variables we are conditioning on to the parameters of the conditional distribution for the next variable.

Conceptually:

$$  
\text{previous variables}  
\rightarrow  
\text{neural network}  
\rightarrow  
\text{parameters of next conditional}  
$$

The neural network therefore approximates the complicated conditional distributions.

---

# 8. Why Neural Networks Help

Suppose we have:

$$  
P(X_i \mid X_1,\ldots,X_{i-1})  
$$

This conditional could potentially have a very complicated relationship between the conditioning variables and the output probability.

A lookup table may require an enormous number of entries.

A neural network instead attempts to learn a flexible function that maps the inputs to the appropriate conditional distribution.

The lecture emphasizes the expressive power of deep neural networks: in principle, sufficiently powerful neural networks can approximate very complicated functions.

This is one of the reasons neural autoregressive models are practical.

---

# 9. Logistic Regression as the Basic Building Block

The conditional distribution in an autoregressive model looks very similar to an ordinary supervised learning problem.

Suppose we want to predict a binary variable $Y$ given features:

$$  
X  
$$

Logistic regression models:

$$  
P(Y=1\mid X)  
$$

using a sigmoid applied to a linear combination:

\sigma(\alpha^\top X+b)  
$$

where:

- $\alpha$ is the vector of coefficients.
    
- $b$ is the bias.
    
- $\sigma(\cdot)$ is the sigmoid function.
    

The sigmoid is:

\frac{1}{1+e^{-z}}  
$$

The important idea is that logistic regression predicts **one variable given other variables**.

That is exactly the type of conditional prediction required by an autoregressive model.

---

# 10. Neural Networks as More Flexible Conditional Models

Logistic regression assumes a relatively simple dependency between the input variables and the conditional probability.

A neural network can model a more nonlinear relationship.

Conceptually:

$$  
X  
\rightarrow  
\text{linear transformation}  
\rightarrow  
\text{nonlinearity}  
\rightarrow  
\text{linear transformation}  
\rightarrow  
\cdots  
\rightarrow  
\text{output}  
$$

The final output can be transformed into a probability distribution using functions such as:

- **Sigmoid** for binary variables.
    
- **Softmax** for categorical variables.
    

The advantage is greater flexibility.

The cost is:

- More parameters.
    
- More memory.
    
- Potentially more data required for learning.
    

---

# 11. Binarized MNIST Example

The lecture uses **binarized MNIST** as a concrete example.

MNIST consists of handwritten digits.

For simplicity, imagine that every pixel is binary:

$$  
x_i \in {0,1}  
$$

where, conceptually:

- $0$ = black.
    
- $1$ = white.
    

Each MNIST image has:

$$  
28 \times 28  
$$

pixels.

Therefore, each image contains:

$$  
28 \times 28 = 784  
$$

random variables.

So the generative model needs to define:

$$  
P(X_1,X_2,\ldots,X_{784})  
$$

---

# 12. The Goal on MNIST

The objective is to learn a probability distribution over these $784$ binary variables.

We want:

$$  
P_\theta(x_1,\ldots,x_{784})  
$$

to approximate the distribution that generated the training images.

Ideally, when we sample from the model:

$$  
x \sim P_\theta(x)  
$$

we obtain images that resemble the handwritten digits from the training distribution.

---

# 13. Applying the Chain Rule to an Image

We first need to choose an ordering for the pixels.

The lecture uses a **raster scan ordering**.

That means we move through the image:

1. From the top-left.
    
2. Across the first row.
    
3. Continue across subsequent rows.
    
4. Finish at the bottom-right.
    

Conceptually:

$$  
X_1 \rightarrow X_2 \rightarrow \cdots \rightarrow X_{784}  
$$

Then the joint distribution becomes:
$$
\prod_{i=1}^{784}  
P(X_i\mid X_1,\ldots,X_{i-1})  
$$

This is an exact factorization.

---

# 14. Why This Is Powerful

Originally, we had to model a complicated joint distribution over $784$ variables:

$$  
P(X_1,\ldots,X_{784})  
$$

The chain rule converts this into a sequence of conditional prediction problems:

$$  
P(X_1)  
$$

then:

$$  
P(X_2\mid X_1)  
$$

then:

$$  
P(X_3\mid X_1,X_2)  
$$

and so on until:

$$  
P(X_{784}\mid X_1,\ldots,X_{783})  
$$

Each individual problem predicts only **one variable**.

This converts a difficult high-dimensional density modeling problem into many familiar conditional prediction problems.

---

# 15. The First Pixel

The first pixel has no previous pixels to condition on.

Therefore, we simply model:

$$  
P(X_1)  
$$

Since $X_1$ is binary, this only requires one probability parameter.

For example:

$$  
P(X_1=1)=p  
$$

and:

$$  
P(X_1=0)=1-p  
$$

---

## Example:

The top-left pixel in a handwritten digit image is very likely to be black.

If we interpret:

$$  
0=\text{black}  
$$

then the probability of the pixel being white may be relatively small.

---

# 16. The Second Pixel

For the second pixel, we need:

$$  
P(X_2\mid X_1)  
$$

For example, we could use logistic regression to predict whether $X_2$ is black or white based on $X_1$.

---

# 17. The Third Pixel

For the third pixel:

$$  
P(X_3\mid X_1,X_2)  
$$

Now two previous pixels are available as conditioning variables.

The process continues.

For the final pixel:

$$  
P(X_{784}\mid X_1,\ldots,X_{783})  
$$

the model can condition on every preceding pixel.

---

# 18. Multiple Classification Problems

An important insight is that we do **not** have one ordinary classification problem.

We effectively have a sequence of classification problems:

$$  
P(X_1)  
$$

$$  
P(X_2\mid X_1)  
$$

$$  
P(X_3\mid X_1,X_2)  
$$

$$  
\vdots  
$$

$$  
P(X_{784}\mid X_1,\ldots,X_{783})  
$$

Each conditional is potentially a different prediction problem.

Therefore, a naive implementation could use different parameters for every position.

---

# 19. Logistic Autoregressive Model

Suppose we use logistic regression for each conditional.

For each position $i$, we could have a different coefficient vector:

$$  
\alpha_i  
$$

Then:
$$
\sigma(\alpha_i^\top X_{<i}+b_i)  
$$

where the notation:

$$  
X_{<i}  
$$

means all variables that come before $X_i$ in the chosen ordering.

For example:

$$  
X_{<3}=(X_1,X_2)  
$$

---

# 20. What Does $X_{<i}$ Mean?

The notation:

$$  
X_{<i}  
$$

means:

> all variables whose indices are strictly smaller than $i$.

Therefore:

$$  
X_{<1}  
$$

contains no variables.

Meanwhile:

$$  
X_{<4}=(X_1,X_2,X_3)  
$$

---

# 21. Fully Visible Sigmoid Belief Network

The lecture connects this simple logistic autoregressive construction to an earlier generative model called a **Fully Visible Sigmoid Belief Network**.

It is useful as a conceptual example because it demonstrates how a joint distribution can be constructed from a sequence of simple classification models.

However, this simple model is not particularly powerful.

The important educational value is understanding the connection between:

> **joint probability modeling → chain rule → conditional prediction → logistic regression.**

---

# 22. Evaluating the Probability of an Image

Suppose we are given a particular image:

$$  
x=(x_1,\ldots,x_{784})  
$$

How do we calculate its probability under the autoregressive model?

We simply use the chain-rule factorization:
$$
\prod_{i=1}^{784}  
P(x_i\mid x_{<i})  
$$

Each factor is evaluated using the corresponding conditional model.

---

## Example:

Suppose, for simplicity, we have only four variables and observe:

$$  
x=(1,1,0,0)  
$$

Then:

$$  
P(x_1,x_2,x_3,x_4)  
$$

is calculated as:

$$  
P(x_1)  
P(x_2\mid x_1)  
P(x_3\mid x_1,x_2)  
P(x_4\mid x_1,x_2,x_3)  
$$

We evaluate each conditional and multiply the results.

---

# 23. Sampling from an Autoregressive Model

A major advantage of the autoregressive factorization is that it also gives us a direct sampling procedure.

Suppose the model is already trained.

We want:

$$  
x\sim P_\theta(x)  
$$

We can generate the variables one at a time.

### Step 1

Sample the first variable:

$$  
x_1\sim P(X_1)  
$$

### Step 2

Now that $x_1$ is known, sample:

$$  
x_2\sim P(X_2\mid x_1)  
$$

### Step 3

Now sample:

$$  
x_3\sim P(X_3\mid x_1,x_2)  
$$

Continue until:

$$  
x_{784}\sim  
P(X_{784}\mid x_1,\ldots,x_{783})  
$$

The complete generated image is:

$$  
x=(x_1,\ldots,x_{784})  
$$

---

# 24. The Sequential Generation Bottleneck

This sampling procedure reveals the major weakness of autoregressive models.

We cannot generally generate $x_i$ before knowing the previous values:

$$  
x_1,\ldots,x_{i-1}  
$$

Therefore generation is inherently sequential.

We have to go:

$$  
x_1  
\rightarrow  
x_2  
\rightarrow  
x_3  
\rightarrow  
\cdots  
\rightarrow  
x_D  
$$

This can become very slow when $D$ is large.

---

# 25. Conditional Sampling and Inpainting

Autoregressive models can also be used to generate missing portions of data.

For example, if some pixels are already known, we can condition on them and generate the remaining pixels according to the model.

This provides a natural mechanism for tasks such as image completion or inpainting.

---

# 26. The Parameter-Count Problem

The naive logistic autoregressive model has another serious issue.

For each pixel $X_i$, we potentially have a different coefficient vector:

$$  
\alpha_i  
$$

The number of inputs grows with $i$.

For the final pixel, the model may depend on:

$$  
X_1,\ldots,X_{783}  
$$

Thus, the number of parameters can grow very quickly with the dimensionality of the data.

The lecture discusses this as one reason why we need more sophisticated architectures.

---

# 27. Why Simple Logistic Regression Is Not Enough

The quality of the generative model depends on whether the conditional distributions can be represented well.

In the logistic model, we assume a relatively simple relationship:
$$
\sigma(\alpha_i^\top X_{<i}+b_i)  
$$

But real images contain complicated dependencies.

For example, whether one pixel should be white or black may depend on complex structures spread across many other pixels.

A simple linear model may not be expressive enough.

---

# 28. Neural Autoregressive Density Estimation

The natural solution is to replace the simple logistic regression models with neural networks.

Instead of:

$$  
X_{<i}  
\rightarrow  
\text{linear model}  
\rightarrow  
\sigma  
$$

we can use:

$$  
X_{<i}  
\rightarrow  
\text{neural network}  
\rightarrow  
\text{conditional distribution}  
$$

The neural network can represent much more complicated dependencies.

---

# 29. Weight Sharing

A major improvement is to avoid having completely separate models for every position.

Instead, we can design architectures that **share parameters**.

The idea is:

> Use the same learned computation repeatedly rather than learning a completely independent model for every conditional.

This dramatically reduces the number of parameters.

---

# 30. Different Types of Variables

The output distribution depends on the type of random variable being modeled.

## Binary Variables

For a binary variable:

$$  
X_i\in{0,1}  
$$

we can use a Bernoulli distribution.

A neural network can output a probability:

$$  
p_i=P(X_i=1\mid X_{<i})  
$$

and then use a sigmoid to ensure:

$$  
0\leq p_i\leq1  
$$

---

## Categorical Variables

If $X_i$ can take one of $K$ categories, the model can output $K$ numbers and apply a softmax:
$$
\frac{e^{z_k}}  
{\sum_{j=1}^{K}e^{z_j}}  
$$

The resulting probabilities satisfy:
$$
1  
$$

---

## Continuous Variables

For continuous data, the conditional distribution cannot simply be represented using a finite categorical probability table.

One possible approach is to model the conditional distribution using a **mixture of Gaussians**.

Conceptually:
$$
\sum_{k=1}^{K}  
\pi_k  
\mathcal{N}  
\left(  
X_i;\mu_k,\sigma_k^2  
\right)  
$$

where the neural network predicts parameters such as:

$$  
\pi_k,\quad \mu_k,\quad \sigma_k  
$$

for the mixture components.

---

# 31. The Connection to Autoencoders

The lecture then moves toward a connection between autoregressive models and neural network architectures that resemble autoencoders.

The key question is:

> Can we construct one neural network that produces all of the conditional distributions simultaneously?

Naively, this would cause a problem.

---

# 32. The Problem of Information Leakage

Suppose we build a neural network that takes an input vector:

$$  
x=(x_1,x_2,\ldots,x_D)  
$$

and produces outputs:

$$  
\hat{x}_1,\hat{x}_2,\ldots,\hat{x}_D  
$$

If the network is allowed to use every input to predict every output, then $\hat{x}_i$ can directly depend on $x_i$.

That is **cheating** from the autoregressive perspective.

For an autoregressive model, the prediction of $X_i$ must only depend on variables that come before it:

$$  
X_{<i}  
$$

It must not depend on:

$$  
X_i,X_{i+1},\ldots,X_D  
$$

---

# 33. Masking

The solution is to **mask the neural network connections**.

Masking means setting certain weights to zero so that particular information pathways are impossible.

For example, if the ordering is:

$$  
X_1\rightarrow X_2\rightarrow X_3  
$$

then the network should ensure:

$$  
\hat{X}_1  
\text{ depends on no previous input}  
$$

$$  
\hat{X}_2  
\text{ depends only on }X_1  
$$

$$  
\hat{X}_3  
\text{ depends only on }X_1,X_2  
$$

This produces an autoregressive dependency structure.

---

# 34. Masking Must Be Applied During Training

The lecture emphasizes that masking is not something we apply only after training.

It must be part of the architecture **during training**.

Otherwise, the network could learn to exploit information that it would not have at generation time.

For example, if predicting $X_i$ were allowed to depend on the actual $X_i$, the network could simply copy the answer.

Therefore, the architecture must prevent this information flow from the beginning.

---

# 35. Masked Neural Networks

For each hidden unit, we can keep track of which inputs it is allowed to depend on.

Each unit can effectively receive an ordering label.

The masks are then constructed recursively.

If a hidden unit is allowed to depend only on certain earlier variables, connections into subsequent layers must preserve that restriction.

Mathematically, this can be achieved by multiplying the ordinary weight matrix by a binary mask:
$$
W\odot M  
$$

where:

- $W$ is the original weight matrix.
    
- $M$ is a binary mask.
    
- $\odot$ denotes element-wise multiplication.
    

A masked-out connection effectively has weight:

$$  
0  
$$

---

# 36. Why Masking Works

The purpose of masking is to preserve the invariant:

> The output corresponding to $X_i$ can only depend on variables preceding $X_i$ in the selected ordering.

Therefore:

$$  
P(X_i\mid X_{<i})  
$$

can be computed without allowing the network to see future variables.

This gives us an autoregressive model using a **single neural network**.

---

# 37. A Major Benefit: Parallel Evaluation

This architecture has an important advantage.

During training, the network can produce the conditional distributions for all positions in a **single forward pass**.

Instead of having completely separate models:

$$  
f_1,f_2,\ldots,f_D  
$$

we can use one masked network:

$$  
f_\theta(x)  
$$

that outputs the necessary conditional parameters.

---

# 38. Training vs. Generation

This distinction is extremely important.

### During training

The masked architecture allows us to compute many conditional probabilities simultaneously.

Therefore training can be highly parallelized.

### During generation

We still do not know future values.

We must generate:

$$  
x_1  
\rightarrow  
x_2  
\rightarrow  
x_3  
\rightarrow  
\cdots  
$$

Therefore generation remains sequential.

### Key takeaway

**Masking removes the unnecessary sequential computation during training, but it does not remove the fundamental sequential dependency during generation.**

---

# 39. Choosing the Ordering

The ordering remains an important problem.

For images, there may be no uniquely correct ordering.

One possibility is:

$$  
X_1,X_2,\ldots,X_D  
$$

Another could use a completely different permutation.

If we have a known temporal or causal structure, the ordering may be obvious.

Otherwise, choosing a good ordering can be difficult.

---

# 40. Learning the Ordering

In principle, we could search over possible orderings.

But if there are $D$ variables, there are:

$$  
D!  
$$

possible orderings.

That makes searching over all possible orderings extremely difficult.

Moreover, the ordering is a discrete object, which makes optimization difficult.

The lecture notes that there has been research into learning autoregressive models together with the ordering.

---

# 41. Random Orderings and Ensembles

One strategy is to choose an ordering randomly.

Another possibility is to use multiple orderings or an ensemble of models.

However, there is no universally obvious method for choosing the perfect ordering when the data does not provide a natural structure.

---

# 42. What Happens If a Variable Has No Previous Inputs?

Suppose the ordering makes some variable appear first.

That variable cannot depend on any previous variables.

Therefore its prediction must effectively be based on its prior distribution.

For example:

$$  
P(X_i)  
$$

If a particular value occurs most frequently in the training data, the model can learn to favor that value.

If the objective requires modeling the entire distribution, the model can learn the corresponding distribution.

The prediction is fixed because there is no previous evidence available.

---

# 43. Redundant Hidden Units

The lecture also addresses a question about multiple hidden units having the same set of allowed inputs.

Even if several hidden units depend on the same input variables, they are not necessarily redundant.

They can have different weights and therefore learn different features or transformations of those same inputs.

For example, several units may receive the same input $x$, but compute:

$$  
h_1=f(w_1x+b_1)  
$$

and:

$$  
h_2=f(w_2x+b_2)  
$$

These can capture different aspects of the same input.

---

# 44. Relation to the Reconstruction Objective

A masked autoregressive network can look similar to an autoencoder.

However, the important change is not necessarily the loss itself.

The crucial difference is the **information allowed to flow through the network**.

When predicting $X_i$, the network is prevented from directly accessing $X_i$ or future variables.

Instead, it must predict based on:

$$  
X_{<i}  
$$

This gives the model exactly the autoregressive conditional structure we want.

---

# 45. The Connection to Language Models

The same principle appears in language models.

When predicting a token, the model must not look at future tokens.

For a sequence:

$$  
x_1,x_2,\ldots,x_T  
$$

we factorize:
$$
\prod_{t=1}^{T}  
P(x_t\mid x_1,\ldots,x_{t-1})  
$$

The model must therefore be masked so that the prediction at position $t$ cannot see positions after $t$.

This is conceptually the same autoregressive constraint.

---

# 46. RNNs as Autoregressive Models

Another approach is to use **recurrent neural networks**, or RNNs.

The fundamental problem remains the same:

> Predict one variable using all previous variables in a chosen ordering.

The problem is that the history becomes longer and longer.

An RNN addresses this by maintaining a hidden state that summarizes the history.

---

# 47. The Hidden State

Let the hidden state at time $t$ be:

$$  
h_t  
$$

The hidden state summarizes the information observed so far.

The next hidden state can be computed recursively:

f_\theta(h_t,x_{t+1})  
$$

A simple implementation could look conceptually like:

\sigma  
\left(  
W_hh_t  
+  
W_xx_{t+1}  
+  
b  
\right)  
$$

The exact RNN architecture can vary, but the important idea is the recursive update.

---

# 48. Why RNNs Are Attractive

Suppose the sequence has length $T$.

We do not need a completely different set of parameters for every position.

The same parameters can be reused:

$$  
W_h,\quad W_x,\quad b  
$$

at every timestep.

This is **extreme weight sharing**.

The number of learnable parameters therefore does not grow with the sequence length.

---

# 49. Hidden State as a Summary of History

The hidden state acts as a compressed representation:

$$  
h_t  
\approx  
\text{summary of }x_1,\ldots,x_t  
$$

Then the model can use $h_t$ to predict the next variable.

Conceptually:

$$  
(x_1,\ldots,x_t)  
\rightarrow  
h_t  
\rightarrow  
P(X_{t+1}\mid x_1,\ldots,x_t)  
$$

---

# 50. Producing the Conditional Distribution

Once we have $h_t$, we can transform it into the parameters needed for the next random variable.

For example:

$$  
h_t  
\rightarrow  
\text{linear transformation}  
\rightarrow  
\text{output logits}  
\rightarrow  
\text{softmax}  
$$

For a categorical variable:
$$
\operatorname{softmax}(Wh_t+b)_k  
$$

The same idea can be adapted for binary variables or continuous distributions.

---

# 51. Is an RNN a Markov Model?

A question raised in the lecture is whether this constitutes a Markov assumption.

A standard first-order Markov model might assume:
$$
P(X_{t+1}\mid X_t)  
$$

An RNN does **not** necessarily make that assumption.

Instead, the hidden state:

$$  
h_t  
$$

can depend recursively on the entire previous history.

Therefore:
$$
f(x_1,\ldots,x_t)  
$$

in principle.

The RNN can therefore capture dependencies involving the entire history, although its ability to preserve that information in practice is limited.

---

# 52. Character-Level Language Modeling

The lecture gives text generation as an example.

Imagine a tiny vocabulary containing four characters:

$$  
{h,e,l,o}  
$$

Each character can be represented using one-hot encoding.

For example:

$$  
h=[1,0,0,0]  
$$

$$  
e=[0,1,0,0]  
$$

and similarly for the other characters.

---

# 53. Autoregressive Factorization for Text

For a sequence of characters:

$$  
x_1,x_2,\ldots,x_T  
$$

the probability becomes:
$$
\prod_{t=1}^{T}  
P(x_t\mid x_1,\ldots,x_{t-1})  
$$

The RNN provides the hidden representation used to compute each conditional probability.

---

# 54. Character Prediction

Suppose the current hidden state is:

$$  
h_t  
$$

The network maps it to four output values, one for each possible character.

For example:

$$  
z=W h_t+b  
$$

where:

$$  
z\in\mathbb{R}^4  
$$

Then we apply softmax:

$$  
p=\operatorname{softmax}(z)  
$$

giving four probabilities whose sum is:

$$  
\sum_{k=1}^{4}p_k=1  
$$

If the correct next character is $e$, the model should learn to assign a high probability to the $e$ entry.

---

# 55. Training the RNN Language Model

The parameters of the RNN are learned so that the model assigns high probability to the observed training sequences.

For a sequence:

$$  
x_1,\ldots,x_T  
$$

the model tries to maximize:

$$  
P(x_1,\ldots,x_T)  
$$

or equivalently:
$$
\sum_{t=1}^{T}  
\log P(x_t\mid x_1,\ldots,x_{t-1})  
$$

This is again maximum-likelihood-style training.

---

# 56. The Main Advantage of RNNs

The same RNN parameters are reused throughout the sequence.

Instead of having:

$$  
f_1,f_2,\ldots,f_T  
$$

we have one recurrent computation:

$$  
h_{t+1}=f_\theta(h_t,x_{t+1})  
$$

This allows the model to operate on sequences of arbitrary length.

---

# 57. Shakespeare Example

The lecture describes training a relatively simple, three-layer RNN on the complete works of Shakespeare at the **character level**.

After training, the model can generate text character by character.

The results can surprisingly resemble Shakespearean writing.

### Example:

Even though the model works only with individual characters, it can learn patterns involving:

- Valid words.
    
- Grammar-like structures.
    
- Punctuation.
    
- Character sequences.
    
- Some aspects of Shakespeare's writing style.
    

This is a striking demonstration of what a relatively simple generative model can learn from raw sequences.

---

# 58. Wikipedia Example

The same type of RNN can be trained on Wikipedia.

After training, it can generate artificial Wikipedia-like pages.

### Example:

The generated text may contain:

- Headings.
    
- Markdown-like formatting.
    
- Links.
    
- Brackets.
    
- Apparently plausible article structures.
    

The model can even learn to close brackets after opening them.

This is interesting because the model has only a single hidden state carrying information from the previous characters.

---

# 59. Baby Names Example

The lecture also describes training an RNN on baby names.

Once trained, the model can sample new names.

The generated names may not necessarily be real names, but they can resemble the patterns of names in the training data.

This demonstrates that the model is learning statistical structure in character sequences.

---

# 60. Why Did These Simple Models Work Surprisingly Well?

Even character-level RNNs with relatively simple architectures can learn surprisingly rich patterns.

The model is not explicitly given rules such as:

> "These characters form a word."

Instead, these patterns emerge from learning the probability distribution of the observed sequences.

The model learns:

$$  
P(x_t\mid x_1,\ldots,x_{t-1})  
$$

and therefore indirectly learns regularities in the data.

---

# 61. The Fundamental RNN Bottleneck

Despite these impressive results, RNNs have a major weakness.

The entire history must effectively be represented through the hidden state:

$$  
h_t  
$$

This means that the model has to carry information about everything relevant from:

$$  
x_1,\ldots,x_t  
$$

inside a single vector.

This creates a **bottleneck**.

---

# 62. The Sequential Computation Bottleneck

There is an additional major problem.

During training, the RNN recurrence must be unrolled:

$$  
h_1  
\rightarrow  
h_2  
\rightarrow  
h_3  
\rightarrow  
\cdots  
\rightarrow  
h_T  
$$

Each state depends on the previous state.

Therefore the computation cannot be fully parallelized across time.

This makes RNNs slow for long sequences.

---

# 63. Why This Matters for Modern Language Models

Modern GPUs are extremely good at parallel computation.

But an RNN requires:

$$  
h_{t+1}  
$$

before it can compute:

$$  
h_{t+2}  
$$

Therefore the model has to proceed step by step.

This prevents it from taking full advantage of modern parallel hardware.

The lecture identifies this sequential computation as one of the key reasons RNNs are no longer the basis of state-of-the-art language models.

---

# 64. The Two Major Autoregressive Bottlenecks

We can summarize the two major limitations discussed in the lecture.

### Bottleneck 1 — Generation

Generation requires sequential sampling:

$$  
x_1\rightarrow x_2\rightarrow\cdots\rightarrow x_D  
$$

This is fundamentally tied to the autoregressive factorization.

### Bottleneck 2 — RNN Training

For RNNs specifically, the recurrence also creates sequential computation during training:

$$  
h_1\rightarrow h_2\rightarrow\cdots\rightarrow h_T  
$$

Masked feed-forward architectures can alleviate the second problem during training, but not the first problem during generation.

---

# 65. The Big Picture

The entire lecture can be viewed as a progression.

### Step 1 — Start with the joint distribution

We want:

$$  
P(X_1,\ldots,X_D)  
$$

### Step 2 — Apply the chain rule
$$
\prod_{i=1}^{D}  
P(X_i\mid X_{<i})  
$$

### Step 3 — Turn each conditional into a prediction problem

Each term:

$$  
P(X_i\mid X_{<i})  
$$

is a conditional prediction problem.

### Step 4 — Use simple models

For example:

$$  
\text{logistic regression}  
$$

### Step 5 — Make the conditional models more expressive

Use neural networks.

### Step 6 — Share parameters

Avoid learning a completely separate model for every position.

### Step 7 — Use masking

Build one neural network that can produce all conditional distributions without looking at future variables.

### Step 8 — Use RNNs

Alternatively, summarize the history using a recurrent hidden state.

---

# 66. Autoregressive Models: The Core Mental Model

The simplest mental model is:

> **Predict one part of the data from the parts that came before it.**

For a vector:

$$  
x=(x_1,x_2,\ldots,x_D)  
$$

the model learns:

$$  
P(x_1)  
$$

then:

$$  
P(x_2\mid x_1)  
$$

then:

$$  
P(x_3\mid x_1,x_2)  
$$

and so forth.

The complete probability is obtained by multiplying these conditionals:
$$
\prod_{i=1}^{D}  
P(x_i\mid x_{<i})  
$$

---

# 67. Important Conceptual Distinction: Exact Factorization vs. Approximation

This distinction is extremely important.

The chain-rule factorization itself is **exact**:
$$
\prod_i P(x_i\mid x_{<i})  
$$

There is no approximation here.

The approximation enters when we choose a particular model family to represent the conditionals.

For example:

$$  
P(X_i\mid X_{<i})  
\approx  
\sigma(\alpha_i^\top X_{<i}+b_i)  
$$

if we use logistic regression.

Or we might use a neural network:

$$  
P(X_i\mid X_{<i})  
\approx  
f_\theta(X_{<i})  
$$

So:

> **Chain rule gives us the exact decomposition; the neural network approximates the individual conditional distributions.**

---

# 68. Another Important Distinction: Training vs. Generation

Autoregressive models have very different computational characteristics during training and generation.

## Training

We already know the entire training example:

$$  
x_1,x_2,\ldots,x_D  
$$

Therefore, with an appropriate masked architecture, many conditional predictions can be computed in parallel.

## Generation

We do not know the future values.

We must first generate:

$$  
x_1  
$$

then use it to generate:

$$  
x_2  
$$

then use:

$$  
x_1,x_2  
$$

to generate:

$$  
x_3  
$$

and continue sequentially.

Thus:

> **Autoregressive models can often be trained efficiently in parallel, but sampling remains sequential.**

---

# 69. Connection to Large Language Models

The lecture begins by pointing out that autoregressive modeling is the foundation behind large language models such as ChatGPT.

The same basic probability factorization applies to text:
$$
\prod_{t=1}^{T}  
P(x_t\mid x_1,\ldots,x_{t-1})  
$$

At every position, the model predicts the next token from the preceding context.

The central autoregressive idea therefore remains:

$$  
\text{past context}  
\rightarrow  
\text{next-token probability distribution}  
$$

The specific neural architecture used in modern systems is different from the RNNs discussed in this lecture, but the autoregressive probabilistic formulation is the same fundamental idea.

---

# 70. Key Vocabulary

|Term|Meaning|
|---|---|
|**Autoregressive model**|A model that predicts each variable conditioned on variables that precede it in a chosen ordering.|
|**Chain rule**|A probability identity that decomposes a joint distribution into conditional distributions.|
|**Ordering**|The sequence in which variables are modeled.|
|**Conditional distribution**|The probability distribution of one variable given other variables.|
|**Raster scan**|An image ordering that proceeds from the top-left toward the bottom-right.|
|**Weight sharing**|Reusing the same parameters across multiple positions.|
|**Masking**|Restricting neural-network connections so that outputs cannot depend on forbidden future variables.|
|**RNN**|A recurrent neural network that maintains a hidden state summarizing previous inputs.|
|**Hidden state**|A vector intended to summarize the sequence history seen so far.|
|**Softmax**|A function that converts logits into a categorical probability distribution.|
|**Sigmoid**|A function that maps a scalar to a value between $0$ and $1$.|
|**Maximum likelihood**|A learning objective that seeks parameters assigning high probability to observed data.|
|**Fully Visible Sigmoid Belief Network**|An early/simple generative model illustrating autoregressive conditional prediction.|

---

# 71. Essential Equations

### Chain rule
$$
\prod_{i=1}^{D}  
P(X_i\mid X_1,\ldots,X_{i-1})  
$$

or using shorthand:
$$
\prod_{i=1}^{D}  
P(X_i\mid X_{<i})  
$$

---

### Logistic regression conditional
$$
\sigma(\alpha_i^\top X_{<i}+b_i)  
$$

---

### Sigmoid
$$
\frac{1}{1+e^{-z}}  
$$

---

### Softmax
$$
\frac{e^{z_k}}  
{\sum_j e^{z_j}}  
$$

---

### Mixture of Gaussians
$$
\sum_{k=1}^{K}  
\pi_k  
\mathcal{N}  
\left(  
X_i;\mu_k,\sigma_k^2  
\right)  
$$

---

### Masked weights
$$
W\odot M  
$$

---

### RNN recurrence
$$
f_\theta(h_t,x_{t+1})  
$$

A simple example is:
$$
\sigma  
\left(  
W_hh_t+W_xx_{t+1}+b  
\right)  
$$

---

### Autoregressive language model
$$
\prod_{t=1}^{T}  
P(x_t\mid x_1,\ldots,x_{t-1})  
$$

---

# 72. Lecture-Level Questions and Answers

### Q1. Why can any joint distribution be represented by an autoregressive factorization?

Because the probability chain rule provides an exact factorization:
$$
\prod_{i=1}^{D}  
P(X_i\mid X_{<i})  
$$

No independence assumption is required.

---

### Q2. Does the ordering affect whether the chain rule is valid?

No.

Any ordering produces a mathematically valid factorization.

However, the ordering can affect how difficult the resulting conditional prediction problems are.

---

### Q3. Why is a neural network useful for autoregressive modeling?

Because the conditional distributions can be very complicated.

A neural network provides a flexible function for approximating:

$$  
P(X_i\mid X_{<i})  
$$

---

### Q4. Why can't we simply use a lookup table?

For high-dimensional data, the number of possible configurations becomes enormous.

A neural network provides a much more compact way to represent complicated dependencies.

---

### Q5. Why is sampling sequential?

Because $X_i$ depends on previous variables:

$$  
X_1,\ldots,X_{i-1}  
$$

Those variables must be generated before we can generally generate $X_i$.

---

### Q6. Why do we need masking?

Without masking, the network could use future variables or even the target variable itself to make a prediction.

That would violate the autoregressive constraint.

---

### Q7. Is masking only useful during generation?

No.

Masking is important during training because the network must learn under the same dependency restrictions.

However, the main computational benefit of masked architectures is that training can be parallelized.

Generation remains sequential.

---

### Q8. Why are RNN parameters independent of sequence length?

Because the same recurrent parameters are reused at every timestep:

$$  
h_{t+1}=f_\theta(h_t,x_{t+1})  
$$

The sequence can therefore be arbitrarily long without requiring a separate parameter set for every timestep.

---

### Q9. Is an RNN simply a first-order Markov model?

No.

A first-order Markov model would depend only on the immediately previous variable.

An RNN's hidden state can, in principle, summarize information from the entire history.

---

### Q10. What is the major weakness of RNNs?

The recurrence must be evaluated sequentially:

$$  
h_1\rightarrow h_2\rightarrow\cdots\rightarrow h_T  
$$

This makes training slow and limits parallelization.

Additionally, compressing the entire history into a single hidden vector creates an information bottleneck.

---

# 73. Conceptual Progression of the Lecture

The lecture follows a very important conceptual progression:

$$  
\boxed{  
\text{Joint Distribution}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Chain Rule}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Conditional Prediction Problems}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Logistic Regression}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Neural Networks}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Weight Sharing + Masking}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Recurrent Neural Networks}  
}  
$$

$\downarrow$

$$  
\boxed{  
\text{Autoregressive Sequence Modeling}  
}  
$$

This progression is the central story of the lecture.

---

# 74. Professor's Note — So What?

The deepest idea to take away from this lecture is that **autoregressive modeling is fundamentally a clever decomposition of a difficult probability problem into a sequence of familiar prediction problems**.

Instead of trying to directly model an enormous joint distribution such as:

$$  
P(X_1,\ldots,X_D)  
$$

we use the chain rule to write it as:
$$
\prod_{i=1}^{D}  
P(X_i\mid X_{<i})  
$$

Now every factor asks a much simpler question:

> **Given what I have already seen, what is the probability distribution of the next thing?**

That question is exactly the kind of problem that supervised learning already knows how to solve.

This is where the power of the idea comes from. Logistic regression can solve the conditional prediction problem in a simple setting. Neural networks can make the conditional model much more expressive. Masking lets us construct a single network that respects the autoregressive dependency structure. RNNs provide another way to summarize an ever-growing history.

At the same time, the lecture exposes the fundamental price of this decomposition: **generation is sequential**. No matter how clever the architecture is, an autoregressive model generally has to establish earlier values before generating later ones.

So the central mental model to remember is:

$$  
\boxed{  
\text{Generative modeling}  
\rightarrow  
\text{factorize the joint}  
\rightarrow  
\text{predict one variable at a time}  
}  
$$

And that idea is much bigger than the specific models discussed in this lecture. It is one of the fundamental ideas behind modern probabilistic sequence modeling and large language models.