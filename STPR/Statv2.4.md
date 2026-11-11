# Probability — v2.4: Covariance, Covariance Matrix & Correlation

Builds on [v2.3](Statv2.3.md) (joint, marginal and conditional distributions) and [v2.1](Statv2.1.md) (mean and variance).
A joint distribution tells us **everything** about how two things are linked. That is often too much detail. Usually we just want **one number** that answers a simple question: when X goes up, does Y usually go up, go down, or not change? That number is the **covariance**. A cleaned-up version of it, with no units, is the **correlation**.

> **Note on my original notes.** The first half of my v2.4 draft (joint, continuous joint, marginal and conditional distributions) repeats v2.3. Those topics are written up fully in [Statv2.3.md](Statv2.3.md), so this file starts at covariance.

---

## 1. Covariance: do two things move together?

### Age vs height, grades and naps

Think about children aged 6 to 10. As they get older:
- they get **taller** (goes up together),
- their **test grades** jump around with no pattern (not linked),
- they take **fewer naps** (one goes up, the other goes down).

Let X = the **age** of a child. Here are three things we could compare it with:

| | age 6 | 7 | 8 | 9 | 10 | mean |
|---|---|---|---|---|---|---|
| Y₁ height (in) | 44 | 46 | 49 | 50 | 51 | 48 |
| Y₂ test grade | 70 | 85 | 60 | 85 | 70 | 74 |
| Y₃ naps per day | 3 | 3 | 2 | 1 | 1 | 2 |

![Three scatters](diagrams/40-cov-three-scatters.svg)

| Pair | Pattern | Covariance |
|---|---|---|
| age vs height | older → taller (go up together) | **> 0** (positive) |
| age vs grades | no trend (not linked) | **≈ 0** (about zero) |
| age vs naps | older → fewer naps (one up, one down) | **< 0** (negative) |

### How to work it out

$$\text{Cov}(X, Y) = \frac{1}{N}\sum_{i=1}^{N} (x_i - \bar x)(y_i - \bar y)$$

What the symbols mean:
- **N** = how many data points we have (here 5 children).
- **xᵢ, yᵢ** = the x and y values of point number i.
- **x̄** ("x-bar") = the mean (average) of the x values. **ȳ** ("y-bar") = the mean of the y values.
- **Σ** (sigma) = "add up all of these".
- **xᵢ − x̄** is called a **deviation**: how far a value is from its average.

The steps:
1. Find the means x̄ and ȳ.
2. For each point, multiply **how far x is from its mean** by **how far y is from its mean**.
3. Take the average of those products.

⚠️ **Correction to my original notes.** I wrote "mx = 0, my = 0". We do **not** assume the means are 0. You subtract the real means, x̄ and ȳ. (Only if the data is already centred, so the means really are 0, does the formula become just the average of xᵢ·yᵢ.)

### Why the sign works: the four corners

Draw one line straight up and one line straight across through the mean point (x̄, ȳ). This cuts the picture into four parts (called **quadrants**):

- **top-right**: x above its mean, y above its mean, so (+)·(+) = **+**
- **bottom-left**: both below their means, so (−)·(−) = **+**
- **top-left** and **bottom-right**: one above, one below, so the product is **−**

![Covariance quadrants](diagrams/41-cov-quadrants.svg)

Each product is the area of a rectangle from the mean point to the data point, with a + or − sign. Covariance is the average of those signed areas.

**Worked example: age vs height** (x̄ = 8, ȳ = 48)

| age | x − 8 | height | y − 48 | product |
|---|---|---|---|---|
| 6 | −2 | 44 | −4 | (−2)·(−4) = +8 |
| 7 | −1 | 46 | −2 | (−1)·(−2) = +2 |
| 8 | 0 | 49 | +1 | 0·1 = 0 |
| 9 | +1 | 50 | +2 | 1·2 = +2 |
| 10 | +2 | 51 | +3 | 2·3 = +6 |
| | | | **sum** | **18** |

- Sum = 8 + 2 + 0 + 2 + 6 = 18.
- Cov = 18 / 5 = **+3.6**.
- All the rectangles are green (positive), so the covariance is clearly positive.

**Naps** (ȳ = 2): the gaps from the mean are 1, 1, 0, −1, −1. The products are −2, −1, 0, −1, −2. Sum = −6, so Cov = −6/5 = **−1.2**.

**Grades** (ȳ = 74): the gaps from the mean are −4, 11, −14, 11, −4. The products are +8, −11, 0, +11, −8. They **cancel out** to exactly **0**.

> **Simple idea:** covariance asks, "when one thing is above its average, is the other one usually above its average too?"
> - Mostly yes → a positive number.
> - Mostly the opposite → a negative number.
> - No pattern → the pluses and minuses cancel → about 0.

**Divide by N or N − 1?**
- Divide by **N** when these points are **all** you care about (the *population* version). Here: 18/5 = 3.6.
- Divide by **N − 1** when the data is a **sample** and you want to guess the covariance of a bigger group. Here: 18/4 = **4.5**.
- This is the same fix as for the sample variance (see [v3.1](Statv3.1.md)).
- In Python, `np.cov` divides by N − 1 by default. Pass `bias=True` to divide by N.

---

## 2. Covariance of a probability distribution

Sometimes we don't have a list of data points. Instead we have a **joint distribution**: a table of how likely each (x, y) pair is. Then each pair is **weighted by its probability** (instead of every point counting 1/N):

$$\text{Cov}(X, Y) = \sum_{x}\sum_{y} P(x, y)\,(x - \mu_X)(y - \mu_Y) = E\big[(X - \mu_X)(Y - \mu_Y)\big]$$

What the symbols mean:
- **P(x, y)** = the probability that X = x and Y = y at the same time.
- **μ_X** (mu X) = the true mean of X. **μ_Y** = the true mean of Y.
- **E[...]** = the expected value, i.e. the probability-weighted average.

### Two players, +$1 or −$1 each

Each of two players either wins $1 (+1) or loses $1 (−1). Each wins half the time, so both means are 0 (μ_X = μ_Y = 0).

Because the means are 0, the product (x − 0)(y − 0) is just x·y:
- **+1** when both get the **same** result,
- **−1** when they get **opposite** results.

![Coin games](diagrams/42-cov-coin-games.svg)

**Game 1: they always move together.** Both win (prob ½) or both lose (prob ½).

Cov = ½·(1·1) + ½·((−1)·(−1)) = ½ + ½ = **+1**

**Game 2: they always move opposite.** X wins and Y loses (½), or the other way round (½).

Cov = ½·(1·(−1)) + ½·((−1)·1) = −½ − ½ = **−1**

**Game 3: two separate coin flips (independent).** All four outcomes have probability ¼.

Cov = ¼(+1) + ¼(+1) + ¼(−1) + ¼(−1) = **0**

⚠️ **Correction to my original notes.** I wrote (−1·−1) + (1·1) = **2** and (−1·1) + (−1·1) = **−2**. That just adds up the products. Each product must be **multiplied by its probability** (½ here), so the right answers are **+1** and **−1**. Adding products without weights only works by luck: when all outcomes are equally likely *and* you then divide by the count.

### Unequal probabilities

Now suppose the players are **mostly** in sync, but not always:

| | Y wins (+1) | Y loses (−1) |
|---|---|---|
| **X wins (+1)** | 0.4 | 0.1 |
| **X loses (−1)** | 0.1 | 0.4 |

Each player still wins half the time (0.4 + 0.1 = 0.5), so both means are still 0. For each cell, multiply probability × x × y:

Cov = 0.4·(+1) + 0.1·(−1) + 0.1·(−1) + 0.4·(+1) = 0.4 − 0.1 − 0.1 + 0.4 = **+0.6**

This is positive, but less than the +1 of Game 1, because sometimes they split.

### The age and height table from v2.3

Using the joint table from [v2.3](Statv2.3.md):

- **Means:**
  - μ_age = 7·0.3 + 8·0.3 + 9·0.4 = 2.1 + 2.4 + 3.6 = **8.1**
  - μ_height = 45·0.2 + 47·0.3 + 49·0.5 = 9 + 14.1 + 24.5 = **47.6**
- **Shortcut formula:** Cov = E[XY] − μ_X·μ_Y.
- **E[XY]** = Σ P(x, y)·x·y = 63 + 32.9 + 37.6 + 78.4 + 42.3 + 132.3 = **386.5**.
- **Cov** = 386.5 − 8.1 × 47.6 = 386.5 − 385.56 = **+0.94**. Older children tend to be taller.

> **Simple idea:** the shortcut E[XY] − μ_X·μ_Y means "average of the product, minus the product of the averages". It is the two-variable version of Var(X) = E[X²] − μ². By hand, it is usually less work.

### Useful facts

- **Cov(X, X) = Var(X).** A variable's covariance with itself is just its variance.
- **Order doesn't matter:** Cov(X, Y) = Cov(Y, X).
- **Independent ⇒ Cov = 0.** ("Independent" means knowing X tells you nothing about Y.) Then P(x, y) = P(x)·P(y), so E[XY] = E[X]·E[Y], and the shortcut gives 0.
- ⚠️ **But Cov = 0 does NOT mean independent.** Example: X is −1, 0 or 1, each with equal chance, and Y = X².
  - Y is fully decided by X, so they are clearly linked.
  - Yet Cov = E[X³] − E[X]·E[X²] = 0 − 0 = **0**.
  - Covariance only notices **straight-line** patterns. Y = X² is a U-shape, not a line.
- **Variance of a sum:** Var(X + Y) = Var(X) + Var(Y) + **2·Cov(X, Y)**.
  - In v2.1 we used Var(X + Y) = Var(X) + Var(Y) for independent Gaussians.
  - This is the general rule. The extra covariance term is zero only when Cov = 0.

---

## 3. The covariance matrix

With two (or more) variables, we can put every pair's covariance into one table. A table of numbers like this is called a **matrix**:

$$\Sigma = \begin{bmatrix} \text{Var}(X) & \text{Cov}(X, Y) \\ \text{Cov}(Y, X) & \text{Var}(Y) \end{bmatrix}$$

(Here **Σ**, a capital sigma, is just the name of the matrix. It is not "add up".)

![Covariance matrix anatomy](diagrams/43-cov-matrix-anatomy.svg)

- The **diagonal** (top-left to bottom-right) pairs each variable with itself, so it holds the **variances**.
- The other two spots hold the covariance of the pair. Since Cov(X, Y) = Cov(Y, X), both spots are equal. So the matrix is a mirror image across the diagonal (**symmetric**).

For the 5 children (age, height): Var(age) = 2, Var(height) = 6.8 and Cov = 3.6, so

$$\Sigma = \begin{bmatrix} 2 & 3.6 \\ 3.6 & 6.8 \end{bmatrix}$$

(Check: age gaps −2, −1, 0, 1, 2 → squares add to 10 → 10/5 = 2. Height gaps −4, −2, 1, 2, 3 → squares add to 34 → 34/5 = 6.8.)

⚠️ **Correction to my original notes.** I wrote M = [[x, xy], [yx, y]]. The diagonal holds the **variances** Var(X) and Var(Y) (that is, Cov(X, X) and Cov(Y, Y)), not x and y themselves. The other entries are Cov(X, Y), not the product xy.

With k variables the matrix is k × k (k rows, k columns). The entry in row i, column j is Cov(Xᵢ, Xⱼ). This matrix is the starting point for the multivariate normal distribution (a bell curve in many dimensions) and for **PCA** (principal component analysis), which finds the directions in which the data spreads out most.

---

## 4. Correlation: covariance without units

**The problem with covariance:** its size depends on the units.
- Measure height in **centimetres** instead of inches.
- Every height gap becomes 2.54 times bigger.
- So the covariance jumps from 3.6 to 3.6 × 2.54 ≈ **9.14**.
- Nothing about the real link changed! From Cov = 9.14 alone, you can't tell if the link is strong or weak.

**The fix:** divide by both standard deviations. The result is the **correlation coefficient**:

$$r = \rho_{XY} = \frac{\text{Cov}(X, Y)}{\sigma_X\,\sigma_Y}$$

What the symbols mean:
- **r** or **ρ** (rho) = the correlation.
- **σ_X** (sigma X) = the standard deviation of X (the square root of its variance). Same for **σ_Y**.

Examples:
- **Age vs height:** r = 3.6 / (√2 · √6.8) = 3.6 / (1.414 × 2.608) = 3.6 / 3.69 ≈ **0.98**.
  - In centimetres: 9.14 / (√2 · 2.54·√6.8) ≈ **0.98**. Unchanged ✓.
- **Age vs naps:** Var(naps) = 0.8, so r = −1.2 / (√2 · √0.8) = −1.2 / 1.265 ≈ **−0.95**.
- **Age vs grades:** r = **0**.
- **The v2.3 table:** r = 0.94 / (√0.69 · √2.44) ≈ 0.94 / 1.30 ≈ **0.72**.

> **Simple idea:** correlation is covariance after turning both variables into z-scores (as in v2.1), so the units disappear. It is always a number between −1 and +1.
> - The **sign** tells you the direction.
> - The **size** tells you how close the points are to a straight line.

![Correlation gallery](diagrams/44-correlation-gallery.svg)

| r | Meaning |
|---|---|
| +1 | perfect rising straight line |
| around +0.8 | strong upward trend, with some scatter |
| 0 | no **straight-line** trend |
| around −0.8 | strong downward trend |
| −1 | perfect falling straight line |

The sign rule is the same as for covariance: r > 0 means they rise together, r < 0 means one falls as the other rises. Correlation and covariance always have the **same sign**. Only the scale is different.

### Two big warnings

1. **r = 0 doesn't mean "not related".** The U-shaped panel in the picture above has r ≈ 0, yet y is almost exactly decided by x. Correlation only measures **straight-line** (linear) links. Always **plot** the data.
2. **Correlation is not causation.** ("Causation" = one thing actually making the other happen.) Ice-cream sales and drownings go up together, because both rise in summer. A hidden third thing (hot weather) drives both. Correlation alone can't tell you which one, if either, causes the other.

### The correlation matrix

Take the covariance matrix. Divide each entry Σᵢⱼ by σᵢσⱼ (the two standard deviations of that pair).
- The diagonal becomes all **1s** (every variable is perfectly correlated with itself).
- The other entries are the r value of each pair.

For age and height:

$$R = \begin{bmatrix} 1 & 0.98 \\ 0.98 & 1 \end{bmatrix}$$

A colour chart (heatmap) of the correlations of many variables, e.g. `df.corr()` in pandas, is one of the first plots people make in data analysis.

---

## 5. In Python

```python
import numpy as np
import pandas as pd

age    = np.array([6, 7, 8, 9, 10])
height = np.array([44, 46, 49, 50, 51])

np.cov(age, height)              # [[2.5, 4.5], [4.5, 8.5]]  (divides by N − 1)
np.cov(age, height, bias=True)   # [[2.0, 3.6], [3.6, 6.8]]  (divides by N)
np.corrcoef(age, height)         # [[1, 0.976], [0.976, 1]]  (same either way)

df = pd.DataFrame({"age": age, "height": height, "naps": [3, 3, 2, 1, 1]})
df.corr()                        # full correlation matrix
```

Dividing by N or N − 1 changes the covariance but **not** the correlation. The same factor appears on the top and on the bottom of r, so it cancels.

---

## Formula cheat sheet

| Idea | Formula | Simple meaning |
|---|---|---|
| Sample covariance | (1/N) Σ (xᵢ − x̄)(yᵢ − ȳ) | average of (gap in x) × (gap in y); use N − 1 to estimate from a sample |
| Covariance of a distribution | Σ Σ P(x, y)(x − μx)(y − μy) | the same, but each pair weighted by its probability |
| Shortcut | Cov = E[XY] − μx μy | average of the product minus product of the averages |
| With itself | Cov(X, X) = Var(X) | variance is a special case of covariance |
| Independence | independent ⇒ Cov = 0 | but Cov = 0 does not prove independence (e.g. Y = X²) |
| Sum | Var(X + Y) = Var X + Var Y + 2 Cov(X, Y) | moving together makes a sum more spread out |
| Covariance matrix | Σᵢⱼ = Cov(Xᵢ, Xⱼ) | table of all pairs; variances on the diagonal; mirror image |
| Correlation | r = Cov(X, Y) / (σx σy) | no units; always between −1 and +1 |
| Correlation matrix | Rᵢⱼ = Σᵢⱼ / (σᵢ σⱼ) | table of all r values; 1s on the diagonal |

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>W1.</b> (Warm-up) Is the covariance of "hours studied" and "exam score" likely positive, negative or about zero?</summary>

**Positive.** Students who study more than average usually score more than average, so the two go up together.
</details>

<details>
<summary><b>W2.</b> (Warm-up) Is the covariance of "outside temperature" and "heating bill" likely positive, negative or about zero?</summary>

**Negative.** When it is warmer than usual, the heating bill is usually lower than usual. One goes up, the other goes down.
</details>

<details>
<summary><b>W3.</b> (Warm-up) What range of values can a correlation r take?</summary>

Always between **−1 and +1**. +1 is a perfect rising line, −1 a perfect falling line, 0 means no straight-line trend.
</details>

<details>
<summary><b>Q1.</b> What does the sign of the covariance tell you? What does it not tell you?</summary>

The sign gives the **direction**: positive means they tend to move together, negative means they move opposite ways. The **size** of the covariance does not tell you how strong the link is, because it depends on the units.
</details>

<details>
<summary><b>Q2.</b> A data point is above the mean in x and below the mean in y. Does it push the covariance up or down?</summary>

**Down.** (+)·(−) is negative. The point is in the top-left or bottom-right corner.
</details>

<details>
<summary><b>Q3.</b> Work out the covariance (÷N) of x = 1, 2, 3 and y = 2, 4, 6.</summary>

x̄ = 2 and ȳ = 4. The gaps are (−1, 0, 1) and (−2, 0, 2), so the products are 2, 0, 2. The sum is 4, and Cov = 4/3 ≈ **1.33**.
</details>

<details>
<summary><b>Q4.</b> Find r for the data in Q3.</summary>

σx = √(2/3) and σy = √(8/3), so σx·σy = √(16/9) = 4/3. r = (4/3)/(4/3) = **1**. The points lie exactly on the line y = 2x.
</details>

<details>
<summary><b>Q5.</b> Why is (−1·−1) + (1·1) = 2 the wrong covariance for "both win or both lose, 50/50"?</summary>

Each product must be weighted by its **probability**: ½·1 + ½·1 = **1**. Covariance is a probability-weighted average, not a plain sum.
</details>

<details>
<summary><b>Q6.</b> Two players: P(both win) = 0.3, P(both lose) = 0.3, P(split) = 0.4 in total (0.2 each way). Each wins half the time. Find Cov.</summary>

The means are 0. Cov = 0.3 + 0.3 − 0.2 − 0.2 = **+0.2**. They are slightly in sync.
</details>

<details>
<summary><b>Q7.</b> X and Y are independent. What is Cov(X, Y)? If Cov(X, Y) = 0, are X and Y always independent?</summary>

Independent ⇒ **Cov = 0**. The other way round is **not** true. For example, X ∈ {−1, 0, 1} and Y = X² have Cov = 0, but Y is fully decided by X.
</details>

<details>
<summary><b>Q8.</b> What is on the diagonal of a covariance matrix, and why is the matrix symmetric (a mirror image)?</summary>

The diagonal holds the **variances** (each variable's covariance with itself). It is symmetric because Cov(X, Y) = Cov(Y, X): multiplying doesn't care about order.
</details>

<details>
<summary><b>Q9.</b> Var(X) = 4, Var(Y) = 9, Cov(X, Y) = 3. Write the covariance matrix and find r.</summary>

Σ = [[4, 3], [3, 9]]. σx = 2 and σy = 3, so r = 3/(2·3) = **0.5**.
</details>

<details>
<summary><b>Q10.</b> Using the numbers from Q9, find Var(X + Y).</summary>

4 + 9 + 2·3 = **19**. If they were independent it would be 13. The positive covariance makes the sum more spread out.
</details>

<details>
<summary><b>Q11.</b> You change height from inches to centimetres. What happens to Cov(age, height) and to r?</summary>

Cov is multiplied by **2.54**. r **doesn't change**, because σ_height also grows by 2.54 and the factor cancels.
</details>

<details>
<summary><b>Q12.</b> A dataset has r = 0. A friend says "so X and Y are not related". What should you check?</summary>

**Plot it.** r = 0 only rules out a **straight-line** trend. A U-shape, a circle or another curved pattern can have r ≈ 0 and still be a strong link.
</details>

<details>
<summary><b>Q13.</b> Cities with more firefighters at a fire have more fire damage (r > 0). Do firefighters cause damage?</summary>

**No.** Correlation is not causation. A third thing, the **size of the fire**, causes both: more firefighters are sent and more damage happens.
</details>

<details>
<summary><b>Q14.</b> Why do np.cov(x, y) and np.cov(x, y, bias=True) give different numbers, while np.corrcoef gives the same r either way?</summary>

The default divides by **N − 1** (a sample estimate), and bias=True divides by **N**. In r, the same factor appears in the covariance on top and in both variances underneath, so it cancels.
</details>
