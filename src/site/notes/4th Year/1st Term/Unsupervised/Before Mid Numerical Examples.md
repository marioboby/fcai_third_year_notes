---
{"dg-publish":true,"permalink":"/4th-year/1st-term/unsupervised/before-mid-numerical-examples/","dg-note-properties":{}}
---

# Unsupervised Learning – Worked Numerical Examples (Exam Guide)

  



  

This guide collects every numerical example in your six lecture files, in exam order, with the arithmetic shown. I recomputed the K-means, K-means++, DBSCAN and GMM numbers in code and they match the lecture. Items tagged *(extra)* and Part 11 are my own practice numbers, not from the slides.

  

Watch for two source quirks: PC1 is quoted as 79.0% in the k-selection example and 79.8% in the outlier example (different datasets), and the guide's own re-runs (e.g. cold-start totals 73 and 449) differ from the lecture's 84 and 477. Quote the lecture numbers.

  

## Part 1: Foundations (vectors, matrices, basic statistics)

  

These files contain few raw numbers; the exam points are the rules. Numbers marked *(extra)* are mine, added so you can practise the rule.

  

**Vector operations (Lecture 2 file)**

  

| Operation | Example from the notes | Result |

| --- | --- | --- |

| Scaling | 0.5 × \[1, 2\]ᵀ | \[0.5, 1\]ᵀ |

| Addition (head-to-tail) | \[1, 2\] + \[3, 2\] | \[4, 4\] |

| Subtraction | basis of distance between points | difference vector |

  

**Matrix multiplication shape rule.** A (3×4) times B (4×3): inner dimensions 4 and 4 match, so it is allowed; outer dimensions give C (3×3). If the inner numbers differ, the product does not exist.

  

**Dot product and cosine similarity**

  

```latex

A \cdot B = A_1 B_1 + A_2 B_2 + \dots + A_n B_n, \qquad \cos\varphi = \frac{A \cdot B}{\|A\|\,\|B\|}

```

  

*(extra)* A = \[1, 2\], B = \[2, 4\]: A·B = 2 + 8 = 10; ‖A‖ = √5, ‖B‖ = √20; cos φ = 10 / (√5 · √20) = 10 / 10 = 1.0. Same direction, different length: cosine says identical, Euclidean distance √5 ≈ 2.24 says different.

  

**Mean, weighted mean, trimmed mean**

  

```latex

\bar{x} = \frac{\sum x_i}{n}, \qquad \bar{x}_w = \frac{\sum w_i x_i}{\sum w_i}, \qquad \bar{x}_{trim} = \frac{\sum_{i=p+1}^{n-p} x_i}{n-2p}

```

  

*(extra)* Data 2, 3, 4, 5, 100 (a 100 is a typo-style outlier): mean = 114/5 = 22.8; median = 4; trimmed mean with p = 1 drops 2 and 100, giving (3+4+5)/3 = 4. The notes' own outlier story: a height logged as 680 cm wrecks the mean but not the median.

  

**Variance, Bessel's correction, standard deviation**

  

```latex

s^2 = \frac{\sum (x_i - \bar{x})^2}{n-1}, \qquad s = \sqrt{s^2}

```

  

Divide by n−1 because a sample sits closer to its own mean than to the true population mean, so dividing by n underestimates variance. Variance is in squared units (cm²); standard deviation returns to cm.

  

*(extra)* Data 2, 4, 6: mean 4; squared deviations 4 + 0 + 4 = 8; sample variance = 8/2 = 4; s = 2 (population version 8/3 ≈ 2.67).

  

**Box-plot outlier fences (Lecture 2 notes)**

  

```latex

\text{IQR} = Q_3 - Q_1, \quad \text{Lower} = Q_1 - 1.5\,\text{IQR}, \quad \text{Upper} = Q_3 + 1.5\,\text{IQR}

```

  

*(extra)* Q1 = 20, Q3 = 30: IQR = 10; fences 5 and 45; a value of 52 is an outlier.

  

**Population vs sample symbols:** size n vs N; sample statistics vs population parameters. Pearson correlation measures linear relationships (−1 to +1); Spearman uses ranks, so it handles monotonic curves and is robust to outliers.

  

## Part 2: Probability, Bayes, entropy and loss functions (Lectures 3 and 4)

  

**The 500-ratings table.** Y = service level (Normal, Intermediate, Pro), X = rating 1 to 5.

  

| Service | x1 | x2 | x3 | x4 | x5 | Row total |

| --- | --- | --- | --- | --- | --- | --- |

| y1 Normal | 50 | 40 | 30 | 20 | 10 | 150 |

| y2 Intermediate | 20 | 30 | 50 | 50 | 20 | 170 |

| y3 Pro | 5 | 10 | 20 | 60 | 85 | 180 |

| Column total | 75 | 80 | 100 | 130 | 115 | N = 500 |

  

**Joint probability** = cell / N:

  

```latex

p(X=x_5, Y=y_3) = \frac{85}{500} = 0.17

```

  

**Marginal probability** = column (or row) total / N:

  

```latex

p(X=x_1) = \frac{75}{500} = 0.15

```

  

**Conditional probability** = cell / the total of the row (or column) you condition on:

  

```latex

p(X=x_1 \mid Y=y_1) = \frac{50}{150} \approx 0.33, \qquad p(X=x_1 \mid Y=y_3) = \frac{5}{180} \approx 0.027

```

  

**Bayes' theorem** (posterior = likelihood × prior / evidence), derived from p(Y|X)p(X) = p(X|Y)p(Y).

  

*(extra, check with the table)* Find p(y3 | x5). Likelihood p(x5|y3) = 85/180 = 0.472; prior p(y3) = 180/500 = 0.36; evidence p(x5) = 115/500 = 0.23. Posterior = 0.472 × 0.36 / 0.23 = **0.739**, the same as the direct count 85/115.

  

**Independence test** *(extra)*: independent means p(x,y) = p(x)p(y). Here p(x1,y1) = 50/500 = 0.10 but p(x1)·p(y1) = 0.15 × 0.30 = 0.045, so X and Y are **not** independent.

  

**Entropy (Lectures 3 and 4)**

  

```latex

H(p) = -p\log_2 p - (1-p)\log_2(1-p)

```

  

| p | H(p) in bits |

| --- | --- |

| 0.5 | 1.00 (maximum uncertainty) |

| 0.9 or 0.1 | 0.47 *(extra)* |

| 0 or 1 | 0 (certain) |

  

**Logistic regression numbers**

  

- Odds = p / (1−p); logit = ln(odds) = z = wᵀx + b; sigmoid σ(z) = 1 / (1 + e⁻ᶻ).

- σ(0) = 0.5 (the decision boundary wᵀx + b = 0); *(extra)* σ(2) = 0.881, σ(−2) = 0.119.

- Distance to the boundary D = |wᵀx + b| / ‖w‖. *(extra)* w = \[3, 4\], b = −5, x = \[3, 2\]: z = 9 + 8 − 5 = 12, ‖w‖ = 5, so D = 2.4 and p = σ(12) ≈ 1.

  

**Binary cross-entropy** *(extra numbers)*

  

```latex

J = -\frac{1}{m}\sum_i \left[y_i \log \hat{y}_i + (1-y_i)\log(1-\hat{y}_i)\right]

```

  

True y = 1: ŷ = 0.9 gives loss −ln 0.9 = 0.105; ŷ = 0.1 (confident and wrong) gives −ln 0.1 = 2.303. That is why it strictly penalises confident wrong predictions.

  

**Gradient result:** ∂L/∂w = (ŷ − y)·x. *(extra)* ŷ = 0.8, y = 1, x = 2 gives (0.8 − 1)×2 = −0.4 (weight is increased).

  

**Softmax** *(extra)*: logits \[2, 1, 0\] give exp = 7.389, 2.718, 1 (sum 11.107), so probabilities 0.665, 0.245, 0.090.

  

**MLE to MSE:** with Gaussian noise, minimising the negative log-likelihood reduces to minimising Σ(yᵢ − f(xᵢ))², because the σ and π terms are constants. Bernoulli gives binary cross-entropy; categorical gives multiclass cross-entropy.

  

**Distribution to loss table:** real y gives Normal (MSE); real y with outliers gives Laplace; y > 0 gives Exponential; y in {0,1} gives Bernoulli (BCE); y in {1..K} gives Categorical; counts give Poisson; rankings give Plackett-Luce.

  

**Deep networks:** without activations, W\[2\](W\[1\]x + b\[1\]) + b\[2\] collapses to W\_new x + b\_new, one linear function.

  

## Part 3: Curse of dimensionality (Lecture 5)

  

**Unit hypersphere experiment.** Put every sample at ‖x‖ = 1 and look at the distance between random pairs.

  

| Dimension | Shape | Distance between two random points |

| --- | --- | --- |

| 1D | x² = 1, so x = −1 or +1 | exactly 0 or 2 |

| 2D | circle | spread from 0 to 2 |

| 3D | sphere | spread from 0 to 2 |

| d = 10 to 2000 | x₁² + … + x\_d² = 1 | mean rises to √2 ≈ 1.41, spread falls to 0 |

  

**Why exactly √2.** In high dimensions two random unit vectors are almost perpendicular (a·b ≈ 0), so:

  

```latex

\|a-b\|^2 = \|a\|^2 + \|b\|^2 - 2\,a\cdot b = 1 + 1 - 0 = 2 \;\Rightarrow\; \|a-b\| \approx \sqrt{2}

```

  

Nearest and farthest neighbour look the same, so distance-based methods stop working.

  

**Samples needed to fill the space** (10 bins per axis): 1 feature needs 10 samples, 2 need 100, 3 need 1,000, 10 features need 10¹⁰ = ten billion.

  

**Drift apart animation:** average distance to the nearest sample for the same 12 samples: 0.08 (1 feature), 0.25 (2 features), 0.47 (3 features).

  

**Lecture question: kNN with 5,000 noise features.** A kNN that works on 10 features gets 5,000 pure-noise features added.

  

1. Distances: noise adds about the same amount to every distance, so all become alike. Nearest/farthest ratio rises **0.28 to 0.95**.

2. Accuracy: the nearest neighbour is now picked by noise. 1-NN accuracy falls **96.5% to 56.5%**, close to a coin flip (50%).

3. Fix: remove or select features, or reduce them (PCA) before kNN. With 0 noise features accuracy returns to 96.5%.

  

The guide's own re-run (not the lecture's numbers) shows ratios 0.29, 0.43, 0.64, 0.79, 0.90, 0.95 for 0, 10, 50, 200, 1000, 5000 noise features. Quote the lecture numbers in the exam.

  

## Part 4: PCA (Lecture 5)

  

**Algorithm:** (1) centre, and standardise if scales differ; (2) covariance C = (1/n) X̃ᵀX̃; (3) solve Cv = λv from det(C − λI) = 0; (4) sort by λ and choose k; (5) project Z = X̃ V\_k.

  

**Complete hand example: 4 samples, 2 features (already centred)**

  

```latex

X = \begin{bmatrix} 3 & 1 \\ 1 & 3 \\ -3 & -1 \\ -1 & -3 \end{bmatrix}, \qquad C = \tfrac{1}{4}X^TX = \begin{bmatrix} 5 & 3 \\ 3 & 5 \end{bmatrix}

```

  

- C11 = (9 + 1 + 9 + 1)/4 = 5; C22 = (1 + 9 + 1 + 9)/4 = 5; C12 = (3 + 3 + 3 + 3)/4 = 3.

- Characteristic equation: det(C − λI) = (5 − λ)² − 3² = 0, so 5 − λ = ±3, giving **λ₁ = 8, λ₂ = 2**. Quick check: trace 10 = 8 + 2, determinant 25 − 9 = 16 = 8 × 2.

- Eigenvectors: (C − 8I)v = 0 gives **v₁ = \[1, 1\]/√2**; (C − 2I)v = 0 gives **v₂ = \[1, −1\]/√2** (perpendicular, length 1).

- Project on PC1: z = X·v₁ = \[2√2, 2√2, −2√2, −2√2\].

- Check: var(z) = (8 + 8 + 8 + 8)/4 = 8 = λ₁. The eigenvalue is the variance along its eigenvector.

- Information kept: λ₁/(λ₁ + λ₂) = 8/10 = **80%** in one feature instead of two.

- Variance along a line at angle θ is 5 + 3 sin 2θ, largest at θ = 45° (value 8), smallest at 135° (value 2).

- *(extra)* Reconstruction of x₁ = (3, 1): z·v₁ = (2, 2), error (1, −1), squared error 2 = λ₂. The error equals the variance thrown away.

  

**Choosing k: the lecture's eigenvalues 8.72, 2.08, 0.25** (3 features, 300 samples)

  

| Rule | Calculation | Keep |

| --- | --- | --- |

| Fixed number | decided in advance | e.g. 5 of 100 |

| Kaiser (λ above mean λ) | mean = (8.72 + 2.08 + 0.25)/3 = 3.68; only 8.72 exceeds it | 1 PC |

| Cumulative 95% | PC1 = 79.0%; PC1 + PC2 = 97.8% ≥ 95% | 2 PCs |

  

Variance shares: PC1 79.0%, PC2 18.8%, PC3 2.2%; all pairwise dot products are 0.000000 (perpendicular).

  

**Drop vs average vs PCA** (same correlated cloud): dropping x₂ keeps 72% of the variance, averaging (x₁ + x₂ + x₃)/3 keeps 92%, PCA keeps 97%.

  

**Lecture question: add x₃ = x₁ (a copy)**

  

1. Only **2** eigenvalues are non-zero (the cloud lies on a plane, C is 3×3 of rank 2, so λ₃ = 0). The guide's re-run gives λ = 2.82, 0.59, 0.00.

2. PC1 **does** change: lecture \[0.71, 0.71\] to \[0.61, 0.50, 0.61\].

3. Why: feature 1 now counts twice in the variance, so PC1 leans toward it. Drop copies and standardise.

  

**Extreme cases (watch the eigenvalues)**

  

| Situation | What happens |

| --- | --- |

| ρ = 0, equal spread | λ₁ ≈ 1.01, λ₂ ≈ 0.99, PC1 explains about 50%; nothing to compress |

| ρ = +1, points on a line | λ₂ = 0, PC1 holds 100% |

| Feature 2 in grams instead of kg (×1000) | λ₁ ≈ 992,000, λ₂ ≈ 0.65; PC1 becomes feature 2 alone, so standardise first |

| One far-away outlier | PC1 explains 51.8% instead of 79.8%; PCA is not robust |

  

Outlier logic: one point 10 units away adds 100 to the sum of squares, an ordinary point 1 unit away adds 1. (The source quotes 79.0% in one place and 79.8% in another; they come from different datasets, so use each figure with its own example.)

  

**Solving Cv = λv in the notes' steps:** write λv as λIv, move to (C − λI)v = 0, v = 0 is trivial so we need (C − λI) singular, hence det(C − λI) = 0, one λ per feature. Eigenvectors of a symmetric C for different λ are perpendicular.

  

## Part 5: LDA and feature selection (Lecture 5)

  

**LDA key numbers and rules**

  

```latex

S_b v = \lambda S_w v \;\Longleftrightarrow\; \max_w \frac{w^T S_b w}{w^T S_w w}, \qquad w \propto S_w^{-1}(\mu_1 - \mu_0)\ \text{(two classes)}

```

  

- Maximise between-class covariance (signal), minimise within-class covariance (noise). The midpoint of the two projected class means is a ready-made threshold.

- At most (classes − 1) components. *(extra)* 3 classes means at most 2 discriminant directions; 2 classes means 1.

- PCA solves Cv = λv (the identity plays the role of noise); LDA replaces I with S\_w.

  

**Selection vs PCA:** selection keeps original columns (n×4 to n×2 keeping f₁, f₃). PCA mixes all features, e.g. PC1 = 0.58 f₁ + 0.26 f₂ + 0.46 f₃ + 0.62 f₄ (squares sum to 1.0).

  

**Filter: mutual information.** Scores in bits: F1 = 0.62, F2 = 0.03, F3 = 0.60, F4 = 0.01, F5 = 0.37. Threshold 0.2 bits, so **keep F1, F3, F5**. Weakness: F1 and F3 are twins (redundant) yet both survive, because a filter scores features one at a time.

  

```latex

I(X;Y) = H(Y) - H(Y|X) = \sum_{x,y} p(x,y)\log_2\frac{p(x,y)}{p(x)p(y)}

```

  

**Wrapper: Sequential Forward Selection (5-fold CV accuracy)**

  

| Round | Candidates and CV accuracy | Chosen set |

| --- | --- | --- |

| 1 | F1 .90, F2 .57, F3 .89, F4 .44, F5 .81 | {F1} |

| 2 | F1F2 .91, F1F3 .91, F1F4 .90, F1F5 .94 | {F1, F5} |

| 3 | F1F5F2 .95, F1F5F3 .93, F1F5F4 .94 | {F1, F5, F2} |

  

Models trained to pick k of M features: M + (M−1) + … + (M−k+1).

  

| M | k | Calculation | Models |

| --- | --- | --- | --- |

| 5 | 3 | 5 + 4 + 3 | 12 |

| 40 | 10 | 40 + 39 + … + 31 | 355 |

| 1000 | 10 | 1000 + 999 + … + 991 | 9,955 |

  

SFS is greedy: it never removes an early choice. After F1 is chosen, F3 (its twin) adds nothing, so SFS picks F1 but not F3. Backward elimination does not detect twins.

  

**Permutation importance:** accuracy with all 5 features = 94.6%. Shuffle F1 and accuracy drops to 76.7%, so importance of F1 = 94.6 − 76.7 = **17.9 points**.

  

**Embedded: L1 regularisation**

  

```latex

\text{Loss} = \text{Loss}_{data} + \lambda\sum_j |w_j|

```

  

At λ = 0.00 all 5 weights are non-zero (5 features); as λ grows weights hit exactly 0 one by one; at λ = 1.20 only w₁ is left (1 feature). L1 pulls with constant force (gradient ±1), so weights reach exactly 0; L2's pull shrinks with w, so weights only get small.

  

| Family | When | Example | Cost | Weakness |

| --- | --- | --- | --- | --- |

| Filter | before the model | MI | very cheap | blind to redundancy and interactions |

| Wrapper | around the model | SFS | most expensive | many models, greedy |

| Embedded | inside training | L1 | one training run | tied to one model, tune λ |

  

## Part 6: Autoencoders and VAE (Lecture 5)

  

**Toy autoencoder loss.** Architecture 5 → 3 → 2 → 3 → 5. Input x = \[0.9, 0.2, 0.6, 0.4, 0.7\], output x′ = \[0.8, 0.3, 0.6, 0.5, 0.6\].

  

| Feature | x | x′ | Squared error |

| --- | --- | --- | --- |

| 1 | 0.9 | 0.8 | 0.01 |

| 2 | 0.2 | 0.3 | 0.01 |

| 3 | 0.6 | 0.6 | 0.00 |

| 4 | 0.4 | 0.5 | 0.01 |

| 5 | 0.7 | 0.6 | 0.01 |

  

Loss = mean = 0.04 / 5 = **0.008**, then back-propagated through decoder and encoder.

  

**MNIST autoencoder 784 → 256 → 32 → 256 → 784**

  

- Test error mean(x − x′)² = **0.0081**. Tensor shapes for a batch of 64: (64,1,28,28), flatten (64,784), (64,256), latent (64,32), (64,256), (64,784).

- Dense first layer weights: 784 × 256 = 200,704 (about 200,000). *(extra)* With biases the full network has 200,960 + 8,224 + 8,448 + 201,488 = 419,120 parameters.

- ReLU zeroed 37% of the 256 values in one batch and 45% in another; batch MSE 0.0037 and 0.0055; raw outputs ranged from −34.3 to 4.7 before the sigmoid squeezes them to \[0, 1\].

- Latent 32: test MSE 0.0068. Latent 2: test MSE 0.0446 (after 5 epochs). Sharper images with 32 numbers, but only 2 numbers gives a plottable map.

- Second demo: 2,000 samples with 200 numbers each, compressed to a 20-D latent. Reconstruction error 0.0435 (epoch 0), 0.0153 (epoch 120), 0.0062 (epoch about 3,000).

- Hinton and Salakhutdinov (2006): a deep autoencoder squeezed digits to 30 numbers and beat PCA with 30 components.

- Rolled-sheet example: neighbours kept by a flat PCA shadow drop from 100% to 57%.

  

**Dense vs convolution (shifted digit)**

  

| Quantity | Dense | Convolution |

| --- | --- | --- |

| First-layer weights | 784 × 256 ≈ 200,000 | 3 × 3 × 1 × 32 = 288 |

| Correlation, original vs 4-5 px shifted | pixels 0.04; dense code −0.16 (guide re-run) | conv map shifted back = 1.00 |

| Lecture feature maps | none (flat 784) | 32 ch × 14×14, 64 ch × 7×7, 2 ch × 4×4 |

| Result on shifted 7 | faded, ghosted | clean |

  

The decoder uses deconvolution: 4×4 → 7×7 → 14×14 → 28×28.

  

**Variational autoencoder**

  

```latex

z = \mu + \sigma\,\varepsilon,\ \varepsilon\sim N(0,1), \qquad \mathcal{L} = \|x - x'\|^2 + KL\big(N(\mu,\sigma^2)\,\|\,N(0,1)\big), \qquad KL = -\tfrac12\sum_j\left(1 + \log\sigma_j^2 - \mu_j^2 - \sigma_j^2\right)

```

  

- KL = 0 when μ = 0 and σ = 1 (the cloud already equals N(0,1)).

- *(extra)* One dimension with μ = 1, σ = 1: KL = −½(1 + 0 − 1 − 1) = 0.5. With μ = 0, σ = 0.5: KL = −½(1 + ln 0.25 − 0 − 0.25) ≈ 0.32.

- *(extra)* Sampling: μ = 2, σ = 0.5, ε = 1.2 gives z = 2 + 0.5 × 1.2 = 2.6.

- Generation: drop the encoder, draw z \~ N(0,1), decode.

  

**Lecture 6 uses of clustering numbers:** nearest-neighbour search over 3 centres instead of 150 points (compression); distances \[3.6, 2.0, 2.8\] to 3 centres become input features; cluster-then-label spreads 4 labels over 176 unlabelled points to give **180** training points instead of 4.

  

## Part 7: K-means, the 10-point example (Lecture 6)

  

Recomputed in code: every number below matches the lecture.

  

```latex

c_i = \arg\min_j \|x_i - \mu_j\|^2 \ \text{(assign)}, \qquad \mu_j = \frac{1}{|S_j|}\sum_{x_i\in S_j} x_i \ \text{(update)}, \qquad J = \sum_i \|x_i - \mu_{c(i)}\|^2

```

  

| Point | A | B | C | D | E | F | G | H | I | J |

| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| x | 2 | 3 | 3 | 4 | 6 | 6 | 7 | 7 | 8 | 7 |

| y | 6 | 4 | 8 | 7 | 2 | 4 | 3 | 4 | 5 | 6 |

  

k = 2, deliberately poor start: ◆1 = A (2, 6), ◆2 = C (3, 8).

  

**Round 1 assign.** Example for B: distance to ◆1 = √((3−2)² + (4−6)²) = √5 = 2.24; to ◆2 = √(0 + 16) = 4.00, so B joins ◆1.

  

| Point | to ◆1 (2, 6) | to ◆2 (3, 8) | Joins |

| --- | --- | --- | --- |

| A | 0.00 | 2.24 | gold |

| B | 2.24 | 4.00 | gold |

| C | 2.24 | 0.00 | green |

| D | 2.24 | 1.41 | green |

| E | 5.66 | 6.71 | gold |

| F | 4.47 | 5.00 | gold |

| G | 5.83 | 6.40 | gold |

| H | 5.39 | 5.66 | gold |

| I | 6.08 | 5.83 | green |

| J | 5.00 | 4.47 | green |

  

**Update each round (group to average of its points)**

  

| Round | Group | Members | Σx | Σy | n | New centre |

| --- | --- | --- | --- | --- | --- | --- |

| 1 | gold | A B E F G H | 31 | 23 | 6 | (5.17, 3.83) |

| 1 | green | C D I J | 22 | 26 | 4 | (5.50, 6.50) |

| 2 | gold | B E F G H | 29 | 17 | 5 | (5.80, 3.40) |

| 2 | green | A C D I J | 24 | 32 | 5 | (4.80, 6.40) |

| 3 | gold | B E F G H I | 37 | 22 | 6 | (6.17, 3.67) |

| 3 | green | A C D J | 16 | 27 | 4 | (4.00, 6.75) |

| 4 | gold | E F G H I J | 41 | 24 | 6 | (6.83, 4.00) |

| 4 | green | A B C D | 12 | 25 | 4 | (3.00, 6.25) |

  

**Who moves and what J does**

  

| Round | Changed group | J after assign | J after update |

| --- | --- | --- | --- |

| 1 | first assignment | 176.0 | 53.7 |

| 2 | A | 51.4 | 46.0 |

| 3 | I | 41.2 | 36.9 |

| 4 | B, J | 31.9 | 23.6 |

  

Assign once more and nobody moves: **stop**. Final: gold = E F G H I J, centre (6.83, 4.00); green = A B C D, centre (3.00, 6.25). J never increases: 176.0 → 53.7 → 51.4 → 46.0 → 41.2 → 36.9 → 31.9 → 23.6.

  

**Why it stops and why the mean:** for a fixed group the mean minimises Σ(xᵢ − μ)² (derivative −2Σ(xᵢ − μ) = 0), and the assign step also only lowers J. There are finitely many splits, so it must stop, at a local minimum.

  

**Cold start:** with k = 3 on the lecture data, one centre in each natural group gives total 84 (guide re-run 73); two centres in the same group splits that group and merges the others, total 477 (guide re-run 449). Remedies: run several random starts and keep the smallest J, or use K-means++. Live demo: K-means++ 84.6 vs bad random start 234.6.

  

**Outlier drags the mean:** adding one point at x\_out shifts the mean by (x\_out − μ)/(n + 1). Group E F G H I J has mean (6.83, 4.00); adding a point at (9.6, 9.6) gives (7.23, 4.80) (the lecture figure quotes (7.2, 4.8)). Check: (6.83×6 + 9.6)/7 = 7.23 and (4.00×6 + 9.6)/7 = 4.80.

  

## Part 8: K-means++ seeding (Lecture 6)

  

Same 10 points, k = 3, indexed A = 0 … J = 9. Only the start changes; afterwards ordinary K-means.

  

```latex

P(x) = \frac{D(x)^2}{\sum_{x'} D(x')^2}, \qquad D(x) = \text{distance to the closest seed so far}, \qquad u\sim U(0,1)

```

  

Draw rule: walk down the running total of P; the first point whose running total reaches u is the next seed.

  

**Seed 1** (plain random, every point 1/10): u₁ = 0.25, index ⌊0.25 × 10⌋ = 2, so **C (3, 8)**.

  

**Seed 2** (chance ∝ D², ΣD² = 220). Worked entry: D(G)² = (7−3)² + (3−8)² = 16 + 25 = 41, P(G) = 41/220 = 0.186. With u = 0.55 the running total first reaches it at G (0.609), so **seed 2 = G (7, 3)**.

  

| Point | A | B | C | D | E | F | G | H | I | J |

| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| D² to C | 5 | 16 | 0 | 2 | 45 | 25 | 41 | 32 | 34 | 20 |

| P | 0.023 | 0.073 | 0 | 0.009 | 0.205 | 0.114 | 0.186 | 0.145 | 0.155 | 0.091 |

| Running total | 0.023 | 0.095 | 0.095 | 0.105 | 0.309 | 0.423 | 0.609 | 0.755 | 0.909 | 1.000 |

  

**Seed 3** (D² now to the nearer of C and G, ΣD² = 42). With u = 0.30 the running total first reaches it at B (0.500), so **seed 3 = B (3, 4)**.

  

| Point | A | B | C | D | E | F | G | H | I | J |

| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

| D² (nearest seed) | 5 | 16 | 0 | 2 | 2 | 2 | 0 | 1 | 5 | 9 |

| P | 0.119 | 0.381 | 0 | 0.048 | 0.048 | 0.048 | 0 | 0.024 | 0.119 | 0.214 |

| Running total | 0.119 | 0.500 | 0.500 | 0.548 | 0.595 | 0.643 | 0.643 | 0.667 | 0.786 | 1.000 |

  

**Then K-means:** gold = A C D (centre 3.00, 7.00), green = E F G H I J (centre 6.83, 4.00), purple = B (centre 3.00, 4.00). The next assign changes nothing, so it is done after **1 round**. (A is exactly 2.24 from both C and B; the lecture gives it to C.)

  

Why squared distance: D next to C has only 0.9% chance, far points get large chances, yet the choice stays random so one outlier does not always win.

  

## Part 9: DBSCAN (Lecture 6)

  

```latex

N(p) = \{q : \|q-p\|\le\varepsilon\}\ (p \text{ included}), \qquad \text{core: } |N(p)|\ge \text{minPts}

```

  

The 10 points plus K (9, 9); ε = 2.5, minPts = 3. Example: B to A = √(1² + 2²) = 2.24 ≤ 2.5 (inside); B to F = 3 > 2.5 (outside).

  

| Point | Neighbours within ε (distance) | Count | Type |

| --- | --- | --- | --- |

| A | B 2.24, C 2.24, D 2.24 | 4 | core |

| B | A 2.24 | 2 | border |

| C | A 2.24, D 1.41 | 3 | core |

| D | A 2.24, C 1.41 | 3 | core |

| E | F 2.00, G 1.41, H 2.24 | 4 | core |

| F | E 2.00, G 1.41, H 1.00, I 2.24, J 2.24 | 6 | core |

| G | E 1.41, F 1.41, H 1.00, I 2.24 | 5 | core |

| H | E 2.24, F 1.00, G 1.00, I 1.41, J 2.00 | 6 | core |

| I | F 2.24, G 2.24, H 1.41, J 1.41 | 5 | core |

| J | F 2.24, H 2.00, I 1.41 | 4 | core |

| K | none | 1 | noise |

  

Counts include the point itself (recomputed: 4, 2, 3, 3, 4, 6, 5, 6, 5, 4, 1).

  

- **Group 1:** start at core A, add B, C, D. B is border: it joins but does not pass the chain on. Result {A, B, C, D}.

- **Group 2:** start at core E, add F, G, H; F adds I, J; the rest add nothing new. Result {E, F, G, H, I, J}.

- **K** has 1 point and no core nearby: **noise**. No k was given; DBSCAN found 2 groups.

  

**Mixed-density failure** (lecture counts): ε = 0.45 gives 3 groups and 19 noise points; ε = 0.90 gives 1 group and 8 noise; ε = 1.60 gives 1 group and 2 noise. One ε cannot suit a tight and a loose group. (The guide's own re-run differs slightly; quote the lecture numbers.)

  

## Part 10: Gaussian mixtures with EM (Lecture 6)

  

```latex

\text{share}_k(x_i) = \frac{\pi_k N_k(x_i)}{\sum_j \pi_j N_j(x_i)}, \quad N(x;\mu,\sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-(x-\mu)^2/2\sigma^2}

```

  

```latex

\mu_k = \frac{\sum_i \text{share}_{ik}\,x_i}{\sum_i \text{share}_{ik}}, \qquad \sigma_k^2 = \frac{\sum_i \text{share}_{ik}(x_i-\mu_k)^2}{\sum_i \text{share}_{ik}}, \qquad \pi_k = \frac{\sum_i \text{share}_{ik}}{n}

```

  

Data: 1, 2, 3, 7, 8, 10. Poor start on purpose: gold μ = 2, green μ = 4; both σ² = 1; π = ½ each. (Recomputed in code: all values below match.)

  

**Round 1 E-step.** Worked entry x = 3: ½·N\_gold(3) = ½·N\_green(3) = 0.121 (distance 1 from both), so share = 0.121 / (0.121 + 0.121) = 0.50 each.

  

| x | π₁·N₁(x) | π₂·N₂(x) | Share gold | Share green |

| --- | --- | --- | --- | --- |

| 1 | 0.121 | 0.002 | 0.98 | 0.02 |

| 2 | 0.199 | 0.027 | 0.88 | 0.12 |

| 3 | 0.121 | 0.121 | 0.50 | 0.50 |

| 7 | 7.4e-07 | 0.002 | 0.00 | 1.00 |

| 8 | 3.0e-09 | 6.7e-05 | 0.00 | 1.00 |

| 10 | 2.5e-15 | 3.0e-09 | 0.00 | 1.00 |

  

**Round 1 M-step.** Green centre = (0.02·1 + 0.12·2 + 0.50·3 + 1.00·7 + 1.00·8 + 1.00·10) / 3.64 = 7.36. Gold total share 2.36, centre 1.80.

  

| Bell | Total share | Centre μ | Width σ² | Weight π |

| --- | --- | --- | --- | --- |

| gold | 2.36 | 1.80 | 0.59 | 0.39 |

| green | 3.64 | 7.36 | 5.82 | 0.61 |

  

**Rounds to convergence**

  

| After round | Gold μ · σ² · π | Green μ · σ² · π | Green share of x = 1, 2, 3 |

| --- | --- | --- | --- |

| 1 | 1.80 · 0.59 · 0.39 | 7.36 · 5.82 · 0.61 | 0.02, 0.12, 0.50 |

| 2 | 1.92 · 0.64 · 0.45 | 7.80 · 4.16 · 0.55 | 0.03, 0.04, 0.25 |

| 3 | 1.98 · 0.66 · 0.49 | 8.19 · 2.31 · 0.51 | 0.00, 0.01, 0.07 |

| converged (about 6 rounds) | 2.00 · 0.67 · 0.50 | 8.33 · 1.56 · 0.50 | 0.00, 0.00, 0.00 |

  

Final: gold holds 1, 2, 3 (μ = 2.00, σ² = 0.67, π = 0.50); green holds 7, 8, 10 (μ = 8.33, σ² = 1.56, π = 0.50). Green is wider because 10 is far out. Point 3 starts 50/50 and ends almost fully gold.

  

**Remember:** shares of a point add to 1; π is a weight, not 3.14; EM is soft K-means (E = assign with shares, M = update with weighted averages); each round raises the likelihood; it only finds a local optimum, K must be chosen, bells only (cannot follow curved moons).

  

**Method chooser:** round groups of similar size and known k: K-means or K-means++. Any shape and noise, similar density: DBSCAN. Overlapping ovals of different spread and soft membership: GMM.

  

## Part 11: Practice problems (my own, answers checked)

  

Cover the answers and solve each by hand; they use the same rules as the lecture examples.

  

| # | Problem | Answer |

| --- | --- | --- |

| 1 | From the 500-ratings table: p(y2 \| x3), p(x4 \| y3), p(y3). | 50/100 = 0.50; 60/180 = 0.333; 180/500 = 0.36 |

| 2 | Can a (2×5) matrix multiply a (5×3) matrix? A (3×4) by a (3×4)? | First: yes, result 2×3. Second: no, inner dimensions 4 and 3 differ |

| 3 | PCA on C = \[\[4, 2\], \[2, 4\]\]: eigenvalues, PC1, variance kept by PC1. | (4−λ)² = 4 gives λ = 6 and 2; PC1 = \[1, 1\]/√2; 6/8 = 75% |

| 4 | K-means, points 1, 2, 9, 10, k = 2, start centres 1 and 2. Final centres and J. | Round 1: 1 to c₁; 2, 9, 10 to c₂ (centre 7). Round 2: 2 moves to c₁; centres 1.5 and 9.5. J = 0.25 + 0.25 + 0.25 + 0.25 = 1.0 |

| 5 | K-means++: first seed is point 1 (of 1, 2, 9, 10). Chance each remaining point is the next seed. | D² = 1, 64, 81, ΣD² = 146 (seed itself 0): P(2) = 0.007, P(9) = 0.438, P(10) = 0.555 |

| 6 | DBSCAN on 1, 2, 3, 10 with ε = 1.5, minPts = 3. | Counts 2, 3, 2, 1. Only point 2 is core; 1 and 3 are border; 10 is noise; one group {1, 2, 3} |

| 7 | GMM: bell A μ = 0, bell B μ = 2, both σ² = 1, π = ½. Share of A for x = 1 and x = 2. | x = 1: equidistant, 0.50. x = 2: N\_A = 0.054, N\_B = 0.399, share A = 0.054/0.453 = 0.12 |

| 8 | BCE for y = 1 with ŷ = 0.7, and for y = 0 with ŷ = 0.7. | −ln 0.7 = 0.357; −ln 0.3 = 1.204 |

| 9 | SFS models trained to choose k = 5 of M = 20 features. | 20 + 19 + 18 + 17 + 16 = 90 |

| 10 | Eigenvalues 5, 3, 1, 1: Kaiser and 95% rules. | Mean 2.5, Kaiser keeps 2 (5 and 3). Cumulative 50%, 80%, 90%, 100%, so 95% needs 4 |

| 11 | Group mean is 4 with n = 5. Add a point at 16: new mean? | 4 + (16 − 4)/6 = 6 |

| 12 | Sample variance of 2, 4, 6, 8. | Mean 5; squares 9 + 1 + 1 + 9 = 20; 20/3 = 6.67 |

| 13 | Cosine similarity of \[1, 0\] and \[1, 1\]. | 1/(1 × √2) = 0.707 (45°) |

| 14 | Autoencoder x = \[1, 0, 1\], x′ = \[0.8, 0.2, 0.6\]. Loss? | (0.04 + 0.04 + 0.16)/3 = 0.08 |

  

## Part 12: Numbers to memorise

  

| Topic | Key numbers |

| --- | --- |

| Ratings table | N = 500; p(x5, y3) = 0.17; p(x1) = 0.15; p(x1\|y1) ≈ 0.33; p(x1\|y3) ≈ 0.027 |

| Hypersphere | mean distance tends to √2, spread to 0 |

| kNN + 5,000 noise | ratio 0.28 to 0.95; accuracy 96.5% to 56.5% |

| PCA by hand | C = \[\[5, 3\], \[3, 5\]\]; λ = 8, 2; v₁ = \[1, 1\]/√2; 80% kept |

| Choosing k | λ = 8.72, 2.08, 0.25; mean 3.68; 79.0% then 97.8%; keep 1 (Kaiser) or 2 (95%) |

| x₃ = x₁ | 2 non-zero eigenvalues; PC1 \[0.71, 0.71\] to \[0.61, 0.50, 0.61\] |

| MI filter | 0.62, 0.03, 0.60, 0.01, 0.37 bits; threshold 0.2; keep F1, F3, F5 |

| SFS | {F1}, {F1, F5}, {F1, F5, F2}; models 12, 355, 9,955 |

| Permutation | 94.6% to 76.7%, importance 17.9 |

| Autoencoder | toy loss 0.008; MNIST test 0.0081; latent 32 gives 0.0068, latent 2 gives 0.0446 |

| K-means | J: 176.0, 53.7, 51.4, 46.0, 41.2, 36.9, 31.9, 23.6; finals (6.83, 4.00) and (3.00, 6.25) |

| K-means++ | ΣD² = 220 (u = 0.55 gives G); ΣD² = 42 (u = 0.30 gives B); u₁ = 0.25 gives C |

| DBSCAN | counts 4, 2, 3, 3, 4, 6, 5, 6, 5, 4, 1; B border, K noise |

| GMM | round 1: μ 1.80 / 7.36, σ² 0.59 / 5.82, π 0.39 / 0.61; final μ 2.00 / 8.33, σ² 0.67 / 1.56, π 0.50 / 0.50 |