---
{"dg-publish":true,"permalink":"/4th-year/1st-term/bci/before-mid/lecture-6-signal-classification/","dg-note-properties":{}}
---

# Decoding Brain Signals & Proving What’s Real: Phase, Machine Learning, and EEG Statistics

Finding a pattern in brainwaves is only half the battle; the other half is proving that the pattern is genuine and not just a lucky fluke in noisy data. This guide walks through how we measure timing consistency across trials, decode brain activity over time using machine learning, and use rigorous statistical tests—from classic paired tests to modern cluster permutations—to separate real neural signals from random chance.

  

## 1. Measuring Timing Consistency: Inter-Trial Phase Consistency (ITPC)

When a person hears a sound or imagines moving a hand over and over again, we want to know whether their brainwaves line up at the exact same stage of the wave cycle every single time.

  

### How ITPC Works

- **Phase ($\theta$):** The exact point in a wave's cycle at a specific moment in time (like whether a wave is currently at its peak, its trough, or halfway between).
    
      
    
- **Inter-Trial Phase Consistency (ITPC):** A score from $0$ to $1$ that measures how tightly the phases of a brainwave line up across repeated trials at one specific time point and frequency.
    
      
    - **Ignoring loudness (magnitude):** Using **Euler's formula**, $e^{i\theta} = \cos\theta + i\sin\theta$, we represent each trial's phase as a **unit vector**—an arrow with a length of exactly $1$ pointing at that trial's angle on a circle. Setting every arrow's length to $1$ deliberately strips away how strong (loud) the signal is so we look _only_ at timing.
        
          
        
    - **Averaging the arrows:** We average all the trial arrows together and measure the length of the resulting average arrow.
        
          
        - **Perfect consistency ($\text{length} = 1.00$):** If two trials both have a phase of $0$, their arrows point the exact same way: $(1 + 1) / 2 = 1$.
            
              
            
        - **Zero consistency ($\text{length} = 0.00$):** If one trial is at $0$ and the other is at $\pi$ (pointing directly opposite), they cancel out completely: $(1 + (-1)) / 2 = 0$.
            
              
            
        - **Partial consistency (e.g., $\text{length} = 0.50$):** Arrows clustered loosely in one general area produce a medium-length average arrow.
            
              
            
    - Core Rule: The direction (angle) of the final average arrow does not matter at all—only its **length** tells you the consistency.
        
          
        
    - **Analogy:** Imagine 30 people standing at the center of a field, each taking one step forward in the direction they are facing. If everyone faces north, the group's average position moves a full step north ($\text{ITPC} = 1$). If everyone faces random directions, they spread out evenly and their average position stays right at the center ($\text{ITPC} = 0$).
        
          
        

### Phase Consistency vs. Signal Power

- Repeating the vector-averaging process for every time point and every frequency builds a **time-frequency ITPC plot**, which reveals something very different from a standard **power plot** (which measures signal strength).
    
      
    
- Critical Distinction: High power change does **not** automatically mean high phase consistency. A brain rhythm can drop or spike dramatically in strength without its waves starting at the exact same angle on every trial.
    
      
    
- **Case Study:** In left-hand motor imagery data from electrode C2 (BCI Competition II, data set III, Graz University of Technology):
    
      
    - **After the warning beep ($2.4\text{ s}, 8\text{ Hz}$):** The brain reacts immediately to the sharp sound, resetting the wave timing on every trial. The arrows bunch together, giving a high ITPC ($\text{length} = 0.45$).
        
          
        
    - **During motor imagery ($5\text{ s}, 10\text{ Hz}$):** Even though the **mu rhythm** drops strongly in power as the person imagines moving their hand, the waves aren't locked to a strict starting clock. The arrows point all around the circle, leaving ITPC near zero ($\text{length} = 0.02$).
        
          
        

## 2. Decoding Brain Activity Over Time

Once we extract features from EEG (like how strong the mu and beta brainwaves are over the motor cortex), we can train machine learning classifiers to guess what the person is doing—such as moving or imagining their left versus right hand.

  

### Classification Across Time (Moment-by-Moment Decoding)

- **Classification across time:** A method where we train and test a separate machine learning model at every single time point $t$ to see _when_ the brain's signal separates two conditions.
    
      
    - **The 3D Data Tensor:** EEG features are stacked into a 3D block shaped as $\text{trials} \times \text{features} \times \text{time}$.
        
          
        
    - **Step-by-step workflow (10-fold cross-validation):**
        
          
        1. Split the trials into 10 equal parts (**folds**). In each fold, 90% of trials are used to train the model and the remaining 10% are held out to test it, so every trial gets tested once.
            
              
            
        2. Slice the 3D block at one specific time point $t$ (giving a 2D table of $\text{trials} \times \text{features}$).
            
              
            
        3. Train a classifier (like **logistic regression**) on the training slice at time $t$ and predict labels on the test slice at that same time $t$.
            
              
            
        4. Record the accuracy (or AUC) as a single dot on a graph where the x-axis is time and the y-axis is accuracy.
            
              
            
        5. Move to the next time slice, repeat from scratch, and average the curves across all 10 folds (or across all participants).
            
              
            
    - **Example:** In the Graz dataset ($132 \text{ trials} \times 4 \text{ features} \times 64 \text{ time points}$, using C3/C4 mu and beta log power), Fold 1 trains on $118$ trials and tests on $14$ trials. At $t = 0.500\text{ s}$ (before the task starts), the model gets $6/14 = 42.9\%$ accuracy; at $t = 0.625\text{ s}$, it gets $7/14 = 50.0\%$ (pure chance for two classes). In a separate PhysioNet dataset of 20 participants opening and closing their left or right fist, each participant gets their own noisy curve, and averaging them reveals a clear group accuracy curve hovering above the $50\%$ chance line after the cue.
        
          
        

### All Time Points at Once vs. One Time Point at a Time

- You have two ways to feed time-varying EEG features into a classifier, and each answers a different question:
    
      
    - **One classifier on all time points:** Flatten every time point's features into one long input vector (e.g., $4 \text{ power values} \times 37 \text{ time points} = 148 \text{ features}$).
        
          
        - **What it tells you:** **How well** you can decode the trial overall, summarized as a single accuracy score ($86.5\% \pm 2.4\%$ across 10 folds).
            
              
            
    - **One classifier at every time point:** Feed only the $4$ features available at time $t$.
        
          
        - **What it tells you:** **When** the brain activity actually differs between classes, shown as a curve over time (peaking at $83.5\%$ at $3.9\text{ s}$).
            
              
            
    - **Analogy:** Feeding all time points at once is like watching a whole movie and giving it one final review score. Training a classifier at every time point is like rating the movie minute-by-minute to see _exactly when_ the plot gets exciting.
        
          
        

### Temporal Generalization (Does the Brain Pattern Last?)

- **Temporal generalization:** An extension of across-time decoding where a model trained at time $t$ is tested not just at $t$, but at _every other_ time point $t'$.
    
      
    - **Building the 2D matrix:** Each training time $t$ creates one row of accuracies across all test times $t'$.
        
          
        - **The diagonal ($t' = t$):** Identical to standard classification across time.
            
              
            
        - **Off the diagonal ($t' \neq t$):** Tests whether the exact neural pattern learned at moment $t$ is still active and separating the classes at moment $t'$.
            
              
            
    - **How to read the heatmap:**
        
          
        - A broad square of high accuracy: The brain holds onto the **same stable pattern** over a long stretch of time (for instance, sustained motor imagery from $3.5\text{ s}$ to $8\text{ s}$).
            
              
            
        - A thin diagonal stripe: The brain pattern is **constantly changing** from moment to moment, so a decoder trained at one second fails a fraction of a second later.
            
              
            
    - **Analogy:** Think of training a facial-recognition lock on a photo of someone at age 20. If it still unlocks for them at ages 25, 30, and 35, their features are stable (a square). If they wear a completely different disguise every day, the lock only works on the exact day the photo was taken (a thin diagonal).
        
          
        

### Sequence Modeling with LSTMs

- **Long Short-Term Memory (LSTM) network:** A type of neural network designed to read a trial step-by-step in order, rather than looking at isolated snapshots.
    
      
    - **How it processes a trial:** At each time step $t$, the LSTM takes in the current features ($x_t$, the 4 power values) and updates two internal tracks—its **memory ($c_t$)** and its **output ($h_t$)**—using learned **forget, input, and output gates** that decide what past information to keep, update, or pass along. After the final step $T$, the last output $h_T$ makes the left/right prediction.
        
          
        
    - Practical Warning: Complex sequence models need lots of training data. On the Graz dataset (4 power values, 8 steps/second from $3\text{–}8.5\text{ s}$, 32 LSTM units), the LSTM only achieved **$80.4\%$** accuracy—losing to the simple linear classifier on all time points (**$86.5\%$**)—because **$132$ trials are simply too few** for a deep network to learn reliably.
        
          
        
    - **Analogy:** Hiring a brilliant chef with 50 customizable kitchen gadgets (an LSTM) to make a sandwich when you only have 3 ingredients (132 trials) often turns out worse than just using a butter knife (logistic regression).
        
          
        

## 3. Measuring Spread & The Logic of Statistical Testing

Suppose Pipeline A gets $90\%$ accuracy and Pipeline B gets $91\%$. Is Pipeline B genuinely better, or did it just get lucky? Averages alone cannot answer that question.

  

### Why the Mean Alone Lies: Variance, SD, and Standard Error

- The **mean** tells you the center of your data, but it completely hides how spread out the individual scores are.
    
      
    - **Example:** Consider two groups of 10 numbers:
        
          
        - **Group A:** `49, 50, 51, 50, 49, 51, 50, 50, 49, 51` $\rightarrow$ $\text{mean} = 50$.
            
              
            
        - **Group B:** `0, 100, 0, 100, 0, 100, 0, 100, 0, 100` $\rightarrow$ $\text{mean} = 50$.
            
              
            
- To capture spread and uncertainty, we use three linked formulas:
    
      
    - **Variance ($s^2$):** The average squared distance of data points from the mean:
        
          
        
        $$s^2 = \frac{\sum (x - \bar{x})^2}{n - 1}$$
        
    - **Standard Deviation ($s$):** The square root of variance ($s = \sqrt{s^2}$), bringing the spread back into the original units. Group A has $s = 0.82$; Group B has $s = 52.70$.
        
          
        
    - **Standard Error of the Mean ($SE$):** Measures how shaky or precise our estimated mean is:
        
          
        
        $$SE = \frac{s}{\sqrt{n}}$$
        
        - For $n = 10$, Group A has $SE = 0.82 / \sqrt{10} = 0.26$, while Group B has $SE = 52.70 / \sqrt{10} = 16.67$.
            
              
            
        - On EEG plots, the **shaded band** around a mean curve represents $\text{mean} \pm SE$ at each time point. Because $\sqrt{n}$ is in the denominator, **more participants (larger $n$) automatically shrink the $SE$**, making our estimate of the mean tighter.
            
              
            
    - **Analogy:** Standard deviation measures how much individual players' heights vary on a basketball team. Standard error measures how confident you are in your estimate of the _team's average height_—and the more players you measure, the tighter that confidence gets.
        
          
        

### Foundations of Hypothesis Testing

- **Null Hypothesis ($H_0$):** The skeptical starting assumption that there is **no real difference** between conditions ($\mu_D = 0$).
    
      
    
- **Alternative Hypothesis ($H_1$):** The claim that there **is a real difference** ($\mu_D \neq 0$).
    
      
    
- **Significance Level ($\alpha$):** Usually set to $0.05$ ($5\%$). This is the maximum risk we are willing to accept of raising a **false alarm**—declaring a difference real when $H_0$ is actually true.
    
      
    
- **p-value:** The probability of seeing a result at least as extreme as the one we got, _assuming $H_0$ is true_.
    
      
    
- **Decision Rule:**
    
      
    - If $p < \alpha$: **Reject $H_0$** — the difference is statistically significant.
        
          
        
    - If $p \ge \alpha$: **Fail to reject $H_0$** — we don't have enough evidence. Warning: Failing to reject $H_0$ does **not** prove that $H_0$ is true (absence of proof is not proof of absence).
        
          
        
- **Analogy:** Think of a courtroom trial. The defendant is presumed innocent ($H_0$). A tiny $p$-value means the evidence is extremely unlikely if the defendant were innocent, so we reject innocence. A large $p$-value leads to a "not guilty" verdict—it means there wasn't enough evidence to convict, not that innocence was mathematically proven.
    
      
    

## 4. Comparing Two Paired Conditions: Paired t-test vs. Wilcoxon

In BCI and neuroscience, we constantly measure the **same participants** twice (e.g., pre-test vs. post-test, or Pipeline A vs. Pipeline B). Because each person acts as their own control, we compute one difference score $D$ per person and test whether those differences are centered around zero.

  

### The Paired t-test

- **Paired t-test:** A test built on the **mean of the differences** ($D = \text{Condition 1} - \text{Condition 2}$), assuming those differences follow a roughly bell-shaped (normal) distribution.
    
      
    - **Formula:** You can compute the $t$-statistic directly from the mean and SD of the differences, or via the raw-sum shortcut:
        
          
        
        $$t = \frac{\bar{D}}{s_D / \sqrt{n}} = \frac{\sum D}{\sqrt{\frac{n \sum D^2 - (\sum D)^2}{n - 1}}}$$
        
    - **Case Study (Pre-test vs. Post-test, $n = 14$):**
        
          
        - Summing the 14 individual differences ($D = \text{pre} - \text{post}$) gives $\sum D = -115$ and $\sum D^2 = 1513$ (mean $\bar{D} = -8.21$, $s_D = 6.61$).
            
              
            
        - Plugging into the formula:
            
              
            
            $$t = \frac{-115}{\sqrt{(14 \times 1513 - (-115)^2) / 13}} = \frac{-115}{\sqrt{612.08}} = \frac{-115}{24.74} = -4.648$$
            
    - **Reading the t-table & Tails:**
        
          
        - **Degrees of freedom ($df$):** $df = n - 1 = 13$.
            
              
            
        - **Two-tailed test ($\alpha = 0.05$):** Checks for a difference in _either_ direction, putting $2.5\%$ risk in each tail ($t_{\text{crit}} = 2.160$). Since $\vert{}t\vert{} = 4.648 > 2.160$, we reject $H_0$ ($p = 0.0005$).
            
              
            
        - **One-tailed test ($\alpha = 0.05$):** Checks only _one_ direction, putting all $5\%$ in one tail ($t_{\text{crit}} = 1.771$). Strict Rule: You must pick the direction _before_ seeing the data. If you predicted $\text{pre} > \text{post}$ (a positive $t$), then getting $t = -4.648$ would **not** count as significant at all!
            
              
            

### Where the t-test Breaks: Outliers & Non-Normal Data

- Because the t-test relies on the mean and standard deviation, **one extreme outlier** can stretch the spread so wildly that it destroys the test's ability to see a real effect.
    
      
    
- **Example (The Outlier Paradox):**
    
      
    - Take 13 paired measurements where Condition 2 is mostly higher than Condition 1. Initially, the t-test gives $t = 2.306$ ($p = 0.040$, significant) and the Wilcoxon test gives $W = 16$ ($p = 0.040$, significant).
        
          
        
    - Now, change just the single largest value in $x_2$ from $36.8$ up to $100$. The gap between the two conditions just became **even bigger**—yet the t-test's spread explodes, dropping $t$ to $1.485$ and pushing $p$ to **$0.163$ (no longer significant!)**. Meanwhile, the Wilcoxon test doesn't flinch ($p = 0.040$).
        
          
        
- **Best Practice:** Always check normality first (using a histogram, Q-Q plot, or Shapiro-Wilk test). If your data is skewed or has outliers, handle the outliers carefully or switch to a rank-based test.
    
      
    

### The Wilcoxon Signed-Rank Test

- **Wilcoxon signed-rank test:** A non-parametric (distribution-free) alternative to the paired t-test that replaces raw difference values with their **ranks**, testing whether the **median difference is zero**.
    
      
    - **Step-by-step workflow (13 pairs example):**
        
          
        1. Calculate each difference ($x_2 - x_1$) and take its absolute value ($\vert{}\text{diff}\vert{}$).
            
              
            
        2. Rank the $\vert{}\text{diff}\vert{}$ values from smallest ($1$) to largest ($13$).
            
              
            
        3. Re-attach the original $+$ or $-$ sign to each rank.
            
              
            
        4. Sum the positive ranks ($T_+ = 75$) and the negative ranks ($T_- = 8 + 5 + 3 = 16$). _(Sanity check: $T_+ + T_- = n(n+1)/2 = 13 \times 14 / 2 = 91$, and $75 + 16 = 91$.)_
            
              
            
        5. Take the smaller sum as your test statistic: $W = \min(T_+, T_-) = 16$.
            
              
            
        6. Look up the critical value ($n = 13, \alpha = 0.05$ two-tailed $\rightarrow W_{\text{crit}} = 17$).
            
              
            
    - Watch Out: Unlike the t-test where a _larger_ number is better, in the Wilcoxon test a **smaller** $W$ is more extreme (because it means almost all differences pointed the same way). You reject $H_0$ when $W \le W_{\text{crit}}$ ($16 \le 17 \rightarrow$ reject $H_0$).
        
          
        
    - **Analogy:** If you want to know whether a new running shoe makes people faster, the t-test cares _how many seconds_ faster each person ran (so one person tripping and taking 5 minutes ruins the average). The Wilcoxon test just puts everyone's time improvements in order from 1st to 13th—whether the biggest improvement was 23 seconds or 100 seconds, it is still just "rank 13."
        
          
        

### Paired t-test vs. Wilcoxon at a Glance

|**Feature**|**Paired t-test**|**Wilcoxon Signed-Rank Test**|
|---|---|---|
|**What it uses**|Exact numerical values of the differences|Ranks of the absolute differences ($\vert{}\text{diff}\vert{}$)|
|**Null Hypothesis ($H_0$)**|Mean difference $= 0$|Median difference $= 0$|
|**Assumptions**|Differences are roughly normal|Symmetric differences; no normality needed|
|**Effect of an outlier**|Heavily pulls the mean and spread|Almost unaffected (only its rank counts)|
|**Reject $H_0$ when...**|$\Vert{}t\Vert{} > t_{\text{crit}}$|$W \le W_{\text{crit}}$|
|**On clean, normal data**|Slightly more statistical power|Almost as good|

## 5. Solving the Multiple Comparisons Problem: Cluster-Based Permutation

When you compare two EEG conditions across an entire timeline (say, $200$ time points), you aren't running one statistical test—you are running $200$ separate tests.

  

### The Trap of Many Tests & Why Bonferroni Is Too Harsh

- **The Multiple Comparisons Problem:** If you run $200$ tests on pure random noise at $\alpha = 0.05$, roughly $5\%$ of them—about **$10$ time points**—will pop up as "statistically significant" purely by chance.
    
      
    
- **Bonferroni Correction:** Divides $\alpha$ by the total number of tests $m$ (e.g., $0.05 / 200 = 0.00025$).
    
      
    - While this wipes out false positives ($0$ left in the noise example), it assumes all $200$ tests are completely independent.
        
          
        
    - Why it fails for EEG: Neighboring time points in a brainwave are smooth and heavily connected—they are **not** independent. Bonferroni becomes way too strict and blinds you to real brain effects.
        
          
        
    - **Analogy:** If you buy 200 lottery tickets, you're much more likely to win something by luck than if you buy 1. Bonferroni fixes this by making the jackpot 200 times harder to win—which works for separate lotteries, but is unfair when adjacent time points are really part of the same single brain event.
        
          
        

### Cluster-Based Permutation Testing (Step-by-Step)

Instead of testing isolated dots in time, **cluster-based permutation testing** groups neighboring significant points into **clusters** and asks: _"How often would pure random shuffling produce a cluster this big?"_

  

- **Case Study:** Comparing C3 beta power ($16\text{–}26\text{ Hz}$, % change from baseline) across 20 participants (PhysioNet dataset) while **executing** a fist movement (Condition 1) vs. **imagining** it (Condition 2).
    
      
    
- **Part A: Find the Real Clusters**
    
      
    1. For each participant, compute the difference curve $D = C_1 - C_2$.
        
          
        
    2. At every time point, run a paired t-test across the 20 participants (e.g., at $t = 1.50\text{ s}$, $\bar{D} = -20.15, s_D = 21.99 \rightarrow t = -4.10$; at $t = 4.00\text{ s}$, $t = -0.54$).
        
          
        
    3. Apply a threshold ($\vert{}t\vert{} > 2.093$ for $df = 19, \alpha = 0.05$ two-sided).
        
          
        
    4. Group any neighboring time points that cross the threshold into a **cluster**.
        
          
        
    5. Compute each cluster's **cluster mass**—the sum of all $t$-values inside that cluster. In our real data, we find four clusters with masses of $3$, $-5$, **$-158$**, and $-34$.
        
          
        
- **Part B: Build the Fake (Null) World via Permutations**
    
      
    1. Under $H_0$ (executing and imagining don't differ), the labels "Condition 1" and "Condition 2" are interchangeable.
        
          
        
    2. Randomly **swap the two condition labels inside individual participants**—never across different participants. Swapping a participant's labels is mathematically identical to simply flipping the sign of their difference curve $D$. (With 20 participants, there are $2^{20} \approx 1\text{ million}$ possible sign-flip combinations; we randomly sample $5,000$ permutations, always starting from the original data.)
        
          
        
    3. For _every_ permutation, recompute the $t$-curve, find any clusters, and keep **only the single largest cluster mass** (e.g., Perm #1: $27.9$; Perm #2: $0$; Perm #3: $11.7$).
        
          
        
- **Part C: Judge Your Real Clusters**
    
      
    - Plot a histogram of all $5,000$ "largest fake cluster masses."
        
          
        
    - The **p-value** for any real cluster is simply the fraction of those $5,000$ random permutations that produced a largest mass $\ge$ your observed cluster mass.
        
          
        
    - **Result:** Our huge observed cluster (mass $158$) beats almost every fake permutation ($p = 0.001 < 0.05 \rightarrow$ **significant!**). The smaller clusters fall right inside the random noise distribution and are rejected: $3$ ($p = 0.46$), $5$ ($p = 0.39$), and $34$ ($p = 0.06$).
        
          
        
- **Analogy:** Imagine trying to prove a streak of sunny days in July isn't random. You shuffle the calendar 5,000 times and record the longest accidental sunny streak from each shuffle. If your real July streak beats $99.9\%$ of the shuffled streaks, you know it's real.
    
      
    

### Extending Clusters to Time-Frequency and Space

- The exact same cluster-permutation engine scales effortlessly beyond a 1D timeline:
    
      
    - **Time-Frequency Clusters:** Compute a $t$-map across every time-and-frequency pixel. Connected pixels that cross the threshold in _both_ time and frequency form 2D islands (clusters), revealing, for example, a clear drop in beta power when executing vs. imagining at C3.
        
          
        
    - **Spatial Clusters:** Using the physical layout of the EEG cap, neighboring electrodes that cross the threshold together form spatial clusters on the scalp.
        
          
        

## 6. Quick-Reference Summary: Decoding & Statistics

|**Method**|**Core Idea**|**Key "Watch Out"**|
|---|---|---|
|**ITPC**|Length of the average unit vector $\left\vert{}\text{mean}(e^{i\theta})\right\vert{}$ across trials|Measures phase timing only, not signal power|
|**Across-Time Decoding**|Train and test one classifier at every single time point|Shows _when_ classes differ, not overall peak decoding|
|**Temporal Generalization**|Train at time $t$, test at every time $t'$|Distinguishes lasting patterns (square) from changing ones (diagonal)|
|**LSTM**|Sequence model reading the trial step-by-step with gates|Needs many trials; easily outperformed by linear models on small datasets|
|**Mean $\pm$ SE**|Center of data plus precision band ($SE = s / \sqrt{n}$)|The mean alone hides wild variability|
|**Paired t-test**|Tests whether the mean of paired differences is $0$|Sensitive to non-normality and extreme outliers|
|**Wilcoxon Signed-Rank**|Tests ranks of absolute differences (median $= 0$)|Reject $H_0$ when $W$ is **smaller** ($W \le W_{\text{crit}}$)|
|**Cluster Permutation**|Compares observed cluster mass against random label swaps|Always keep only the **largest** cluster mass per permutation|

## Professor's Synthesis

What ties this entire toolkit together is a single, humbling truth about neuroscience and machine learning: **brains are messy, and small datasets love to fool us.** Whether you are looking at a gorgeous spike in decoding accuracy, a sharp drop in beta power, or a $1\%$ bump between two ML pipelines, your job as a researcher isn't just to extract a pattern—it's to torture-test it. Use ITPC to distinguish timing from power, use temporal generalization to see if your decoder is reading a fleeting spark or a sustained thought, respect the limits of deep models when you only have a hundred trials, and never trust a $p$-value until you've checked for outliers and multiple comparisons. Master these habits, and you won't just build models that _look_ smart—you'll build science that actually holds up.