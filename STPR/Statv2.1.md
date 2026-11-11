# Probability — v2.1: Expected Value, Median, Mode & Variance

Builds on [v1.3](Statv1.3.md) (random variables and distributions).
In v1.x we asked *which* outcomes can happen. Now we ask two new questions:
- **Where is the centre?** What is a "typical" value?
- **How spread out is it?** Are values close together or all over the place?

---

## 1. Expected value (the mean)

The **expected value**, written **E[X]** or **μ** (mu), is the **long-run average** of X.
- X = a **random variable**, a number that comes from chance (like a dice roll).
- If you repeat the experiment many, many times and average the results, you get E[X].

### Example: ages of 10 kids

- 3 kids are aged 0
- 2 kids are aged 1
- 4 kids are aged 2
- 1 kid is aged 3

**Way 1: plain average.** Add all 10 ages and divide by 10.

$$\frac{0+0+0+1+1+2+2+2+2+3}{10} = \frac{13}{10} = 1.3$$

**Way 2: weighted average.** Multiply each age by its share of kids (its probability), then add.

| Age x | Share P(x) | x · P(x) |
|---|---|---|
| 0 | 3/10 | 0 |
| 1 | 2/10 | 0.2 |
| 2 | 4/10 | 0.8 |
| 3 | 1/10 | 0.3 |
| **Total** | 1 | **1.3** |

$$\tfrac{3}{10}\cdot 0 + \tfrac{2}{10}\cdot 1 + \tfrac{4}{10}\cdot 2 + \tfrac{1}{10}\cdot 3 = 1.3$$

Both ways give the same answer: **1.3**.

![Mean as balance point](diagrams/18-mean-balance-point.svg)

Think of the ages as blocks on a seesaw. The mean is the point where the seesaw **balances**.

**The formula:**

$$E[X] = \sum_x x \cdot P(X = x) \qquad\qquad E[X] = \int_{-\infty}^{\infty} x \, f(x)\, dx$$

- Σ (sigma) = "add up over all values".
- P(X = x) = the probability that X equals x.
- Left formula: for **discrete** X (separate values like 0, 1, 2, 3).
- Right formula: for **continuous** X (any value in a range, like height). Here ∫ (integral) is the continuous version of "add up", and f(x) is the **PDF** (probability density function, the curve from v1.3).

> **Simple idea:** multiply each value by how likely it is, then add them all up. For a continuous X, "add up" becomes an integral.

⚠️ **Correction to my original notes.** The continuous formula uses the **PDF f(x)**, not the CDF F(x). (The CDF is the running total of probability, which is a different thing.)

**Moving weight moves the mean.**
- Move probability towards bigger values → the mean goes **up**.
- Move it towards smaller values → the mean goes **down**.
- A block far from the centre has more **leverage**, like a child sitting at the very end of a seesaw. So one extreme value can pull the mean a long way.

### Expected value as a "fair price"

A game: you win **$10 on heads** and **$0 on tails**.

$$E[\text{win}] = 10 \times 0.5 + 0 \times 0.5 = 5 + 0 = \$5$$

> **Simple idea:** if you play many times, you win $5 per game on average. So paying up to $5 to play is fair. If you pay more than $5, you lose money in the long run.

### Sum of expectations (linearity)

Another game, in two parts:
- Flip a coin. Win $1 on heads. Average win = 1 × 0.5 = **$0.5**.
- Roll a die. Win the number rolled. Average win = (1+2+3+4+5+6)/6 = **$3.5**.

$$E[\text{coin} + \text{die}] = E[\text{coin}] + E[\text{die}] = 0.5 + 3.5 = \$4$$

The general rule:

$$E[X + Y] = E[X] + E[Y]$$

> **Simple idea:** the average of a total = the total of the averages. This is **always** true, even when X and Y affect each other (are *dependent*). Variance is fussier, as section 3 and section 5 show.

---

## 2. Median and mode

The mean is not the only kind of "centre". There are three common ones:

| Measure | What it is | How to find it |
|---|---|---|
| **Mean** | the balance point | Σ x·P(x) |
| **Median** | the middle value: half the data is below, half is above | sort the data. Odd count: take the middle one. Even count: average the two middle ones |
| **Mode** | the most common (most likely) value | the highest point of the PMF or PDF |

(PMF = probability mass function, the bar chart of probabilities for a discrete X.)

### Salary example

10 people: 9 earn ₹20,000 and 1 earns ₹20,00,000 (twenty lakh).

**Mean:**
- 9 × 20,000 = 1,80,000
- 1,80,000 + 20,00,000 = 21,80,000
- 21,80,000 / 10 = **₹2,18,000**

Nobody in the group actually earns this!

**Median:** sort the 10 salaries. The two middle ones (5th and 6th) are both 20,000, so the median = **₹20,000**. That's the typical person.

![Mean, median, mode on skewed data](diagrams/19-mean-median-mode-skew.svg)

⚠️ **Correction to my original notes.** I wrote "the mean is the correct measure" here. That's not always true. For **skewed** data (lopsided, with a long tail on one side) like salaries or house prices, the **median** is a better "typical" value.
- One huge value drags the mean a long way, because the mean uses every point's **distance** from the centre (each point gets weight 1/N).
- The median only cares about the **order** of the values, so one huge value barely moves it.

> **Simple idea (rule of thumb):**
> - Symmetric data (same shape on both sides): mean ≈ median ≈ mode. Any of them is fine.
> - Right-skewed data (a long tail of big values): mode < median < mean. Report the **median**.

**Multimodal** = more than one peak. Example: the heights of a mixed group of adults and children. There's a peak for kids and another for adults. A single "centre" here can mislead you, so always draw the data.

---

## 3. Variance and standard deviation

Two games, both with **mean 0** (on average you neither win nor lose):

- **Game A:** win or lose $1
- **Game B:** win or lose $100

Same centre, but Game B is far riskier. We need a number for **spread**: how far values usually are from the mean.

![Same mean, different variance](diagrams/20-variance-same-mean.svg)

The **deviation** of a value is how far it is from the mean: x − E[X].

**Attempt 1: average the deviations x − E[X].**
For Game B: (+100 − 0) = +100 and (−100 − 0) = −100. Their average is 0.
Deviations **always cancel out to 0**, so this tells us nothing.

**Attempt 2: average the absolute deviations |x − E[X]|.**
|·| means "drop the minus sign". This works, but it is awkward in algebra and calculus.

**What we actually use: the average of the *squared* deviations.**

$$\text{Var}(X) = E\big[(X - \mu)^2\big] = \sum_x (x - \mu)^2 P(x)$$

$$\text{SD}(X) = \sigma = \sqrt{\text{Var}(X)}$$

- **Var(X)** = the **variance**.
- **SD(X)** or **σ** (sigma) = the **standard deviation**, the square root of the variance.

> **Simple idea:** variance = "on average, how far from the mean, squared".
> - Squaring makes every deviation positive, so nothing cancels.
> - Squaring also punishes big misses more than small ones.
> - Variance is in **squared units** (dollars²), which is hard to picture. So we take the square root to get the SD, back in plain **dollars**.

| | Game A | Game B |
|---|---|---|
| Var | (1² + 1²)/2 = (1 + 1)/2 = **1** | (100² + 100²)/2 = (10,000 + 10,000)/2 = **10,000** |
| SD | √1 = **$1** | √10,000 = **$100** |

### Shortcut formula

This is often easier to calculate:

$$\text{Var}(X) = E[X^2] - (E[X])^2$$

In words: "the average of the squares, minus the square of the average".

**Ages example** (from section 1, where E[X] = 1.3):

| Age x | x² | P(x) | x² · P(x) |
|---|---|---|---|
| 0 | 0 | 0.3 | 0 |
| 1 | 1 | 0.2 | 0.2 |
| 2 | 4 | 0.4 | 1.6 |
| 3 | 9 | 0.1 | 0.9 |
| | | **E[X²]** | **2.7** |

- Var = 2.7 − 1.3² = 2.7 − 1.69 = **1.01**
- SD = √1.01 ≈ **1.005**

**Link to the normal distribution (v1.3):** for a normal (bell) curve:
- about **68%** of values lie within μ ± 1σ
- about **95%** within μ ± 2σ
- about **99.7%** within μ ± 3σ

So the SD is the natural "ruler" for measuring spread.

---

## 4. Linear functions: what happens to E and Var for aX + b?

A **linear function** of X looks like Y = aX + b: multiply by a number a, then add a number b.

Everyday examples:
- Converting °C to °F: Y = 1.8X + 32.
- A salary after a raise.

![Linear transformation](diagrams/21-linear-transform.svg)

$$E[aX + b] = a\,E[X] + b \qquad \text{Var}(aX + b) = a^2\,\text{Var}(X) \qquad \text{SD}(aX + b) = |a|\,\text{SD}(X)$$

**Why?**

- **+ b** slides every value by the same amount. The centre moves, but the gaps from the centre stay the same. So the **variance doesn't change**.
- **× a** stretches every gap from the centre by a. Variance is a *squared* gap, so it grows by **a²**.

> **Simple idea:** adding a number moves the whole thing sideways but doesn't make it wider. Doubling every value doubles the spread (SD × 2), which makes the variance 4 times bigger (× 4).

**Example:** temperatures in °C have mean 20 and SD 5. Convert to °F:
- Mean = 1.8 × 20 + 32 = 36 + 32 = **68**
- SD = 1.8 × 5 = **9**

The +32 has no effect on the spread.

---

## 5. Sum of Gaussians: why Var(T + L) = Var(T) + Var(L)

(A **Gaussian** is another name for a normal distribution.)

**Setup:** your trip to work has two parts. Total time **R = T + L**, where

- T = train time ~ N(30, 4²) minutes (mean 30, SD 4)
- L = walk time ~ N(10, 3²) minutes (mean 10, SD 3)
- T and L are **independent** (a slow train tells you nothing about the walk)

The "~ N(μ, σ²)" notation means "is normally distributed with mean μ and variance σ²".

![Sum of Gaussians](diagrams/22-sum-of-gaussians.svg)

### The rules

**Mean:** just add.

$$E[R] = E[T] + E[L] = 30 + 10 = 40$$

**Variance:** add the variances (not the SDs).

$$\text{Var}(R) = \text{Var}(T) + \text{Var}(L) = 4^2 + 3^2 = 16 + 9 = 25 \;\Rightarrow\; \text{SD}(R) = \sqrt{25} = 5$$

**Shape:** a special property of the normal: **a sum of independent normals is also normal**. So R ~ N(40, 5²).

### Why do the *variances* add, and not the SDs?

Expand the definition of variance:

$$\text{Var}(T + L) = E\big[(T - \mu_T + L - \mu_L)^2\big] = \text{Var}(T) + \text{Var}(L) + 2\,\underbrace{E\big[(T-\mu_T)(L-\mu_L)\big]}_{\text{Cov}(T,\,L)}$$

- The extra "cross term" is the **covariance**, Cov(T, L). It measures whether T and L tend to be high together or low together.
- If T and L are **independent**, Cov = 0. The cross term disappears, and the variances simply add.

> **Simple idea:** some days the train is late but the walk is quick, so the delays **partly cancel**. Because of that, the total is *less* spread out than "worst + worst" (4 + 3 = 7). Independent spreads combine like the sides of a right triangle: √(4² + 3²) = √25 = 5. So SDs add like **Pythagoras**, and their squares (the variances) add normally.

### Things that trip people up

| Situation | Variance | Why |
|---|---|---|
| T + L (independent) | Var T + Var L | the cross term is 0 |
| T − L (independent) | Var T **+** Var L | Var(−L) = (−1)²·Var L = Var L. Subtracting still *adds* uncertainty |
| T + L (dependent) | Var T + Var L + 2 Cov(T, L) | e.g. a rainy day slows both, so Cov > 0 and the spread is bigger |
| 2X (one variable doubled) | 4 Var X | the same randomness counted twice; it can never cancel |
| X₁ + X₂ (two independent copies) | 2 Var X | the two copies partly cancel |
| average of n independent copies | Var X / n | averaging shrinks the spread, so SD = σ/√n |

**Using it:** what's the chance the trip takes more than 50 min?
1. z-score: (50 − 40)/5 = 10/5 = **2**.
2. P(R > 50) = P(Z > 2) ≈ **2.3%**.

---

## 6. Standardisation of a distribution

Standardising means converting a value into a **z-score** (from v1.3): "how many SDs above or below the mean is it?"

$$Z = \frac{X - \mu}{\sigma}$$

Section 4's rules explain why this works. Z is just aX + b with a = 1/σ and b = −μ/σ. So:

$$E[Z] = \frac{E[X] - \mu}{\sigma} = 0 \qquad\qquad \text{Var}(Z) = \frac{\text{Var}(X)}{\sigma^2} = 1$$

![Standardisation](diagrams/23-standardization.svg)

> **Simple idea:**
> 1. Subtract the mean. This slides the centre to 0.
> 2. Divide by the SD. This changes the units to "number of SDs".
>
> The **shape stays the same**. Only the ruler changes. If X is normal, Z is the standard normal N(0, 1).

**What it's good for:**

- **Comparing things on different scales.** A trip of 50 min is z = +2, unusually slow. A test score of 80 when μ = 70 and σ = 5 is z = (80 − 70)/5 = +2, equally unusual.
- **One table for every normal.** Any normal probability turns into a lookup in the N(0, 1) table.
- **Spotting outliers** (strange, extreme values). Flag points with |z| > 3.
- **Machine learning.** Features measured in different units (cm, kg, ₹) get put on the same scale.

Standardising works for *any* distribution: the result always has mean 0 and SD 1. But it **doesn't** turn a skewed distribution into a normal one, because the shape stays the same.

---

## Formula cheat sheet

| Idea | Formula | Simple meaning |
|---|---|---|
| Expected value | E[X] = Σ x·P(x) or ∫ x f(x) dx | weighted average, the balance point |
| Linearity | E[X + Y] = E[X] + E[Y] | always true, even for dependent X and Y |
| Variance | Var(X) = E[(X − μ)²] = E[X²] − μ² | average squared distance from the mean |
| Standard deviation | σ = √Var(X) | spread in the original units |
| Linear function | E[aX+b] = aE[X]+b, Var(aX+b) = a²Var(X) | shifting moves it, scaling stretches it |
| Sum (independent) | Var(X ± Y) = Var(X) + Var(Y) | variances add, SDs don't |
| Sum (general) | Var(X+Y) = Var X + Var Y + 2Cov(X,Y) | the extra term measures "moving together" |
| Sum of normals | N(μ₁,σ₁²) + N(μ₂,σ₂²) = N(μ₁+μ₂, σ₁²+σ₂²) | if independent, the result is normal too |
| Standardise | Z = (X − μ)/σ | mean 0, variance 1, same shape |

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>W1.</b> (Warm-up) Find the mean and median of 2, 3, 10.</summary>

Mean = (2 + 3 + 10)/3 = 15/3 = **5**. Median = the middle value = **3**. The 10 pulls the mean up but not the median.
</details>

<details>
<summary><b>W2.</b> (Warm-up) A value has variance 16. What is its SD?</summary>

SD = √16 = **4**.
</details>

<details>
<summary><b>Q1.</b> What is the expected value of one roll of a fair die? What is its variance?</summary>

E = (1+2+3+4+5+6)/6 = 21/6 = **3.5**.
E[X²] = (1+4+9+16+25+36)/6 = 91/6 ≈ 15.17, so Var = 15.17 − 3.5² = 15.17 − 12.25 = **35/12 ≈ 2.92**.
</details>

<details>
<summary><b>Q2.</b> A game costs $3. You roll a die and win $1 for each dot. Is it worth playing?</summary>

E[win] = $3.5 and the cost is $3, so you gain **+$0.50 per game** on average. In the long run it's worth playing.
</details>

<details>
<summary><b>Q3.</b> House prices on a street are 40, 45, 50, 55 and 500 (lakhs). Find the mean and the median. Which describes a typical house?</summary>

Mean = 690/5 = **138**. Median = **50**. The median is the typical value. The mean is pulled up by the one mansion.
</details>

<details>
<summary><b>Q4.</b> Why do we square the deviations instead of just averaging x − μ?</summary>

The plain deviations always add up to 0, because the pluses and minuses cancel. Squaring makes them all positive (and counts big misses more), and it's easy to work with in algebra.
</details>

<details>
<summary><b>Q5.</b> X has mean 10 and SD 2. What are the mean and SD of Y = 3X − 4?</summary>

E[Y] = 3·10 − 4 = **26**. SD(Y) = |3|·2 = **6** (Var = 36). The −4 doesn't change the spread.
</details>

<details>
<summary><b>Q6.</b> X has variance 9. What is Var(−X)? What is Var(X + 100)?</summary>

Var(−X) = (−1)²·9 = **9**. Var(X + 100) = **9**. Flipping and sliding don't change the spread.
</details>

<details>
<summary><b>Q7.</b> T ~ N(30, 4²) and L ~ N(10, 3²) are independent. What is the distribution of T − L?</summary>

Mean = 30 − 10 = 20. Var = 16 + 9 = 25. So **T − L ~ N(20, 5²)**. The variances still add when you subtract.
</details>

<details>
<summary><b>Q8.</b> Why is Var(2X) different from Var(X₁ + X₂), where X₁ and X₂ are independent copies of X?</summary>

Var(2X) = 4 Var X: one source of randomness, doubled. It can't cancel itself.
Var(X₁ + X₂) = 2 Var X: two separate sources. Their ups and downs partly cancel.
</details>

<details>
<summary><b>Q9.</b> A box holds 100 items. Each weighs N(50 g, 2²), independently. What is the distribution of the total weight?</summary>

Mean = 100·50 = 5000 g. Var = 100·4 = 400, so SD = **20 g**. The total is **N(5000, 20²)**. Notice the SD grows like √n, not like n.
</details>

<details>
<summary><b>Q10.</b> In Q9, what is the SD of the <i>average</i> weight of the 100 items?</summary>

Var(average) = Var(X)/n = 4/100 = 0.04, so SD = **0.2 g** (same as σ/√n = 2/10).
</details>

<details>
<summary><b>Q11.</b> Is Var(X + Y) = Var X + Var Y always true? Give a case where it fails.</summary>

**No. It needs Cov(X, Y) = 0** (independence is enough). Example: Y = X gives Var(2X) = 4 Var X, not 2 Var X. Another: the morning and evening commute on the same rainy day are both slow, so Cov > 0.
</details>

<details>
<summary><b>Q12.</b> Your trip R ~ N(40, 5²). What is P(R < 35)? What is P(30 < R < 50)?</summary>

P(R < 35) = P(Z < −1) ≈ **16%**. P(30 < R < 50) = P(−2 < Z < 2) ≈ **95%**.
</details>

<details>
<summary><b>Q13.</b> After standardising any dataset, what are its mean and SD? Does it become normal?</summary>

Mean **0** and SD **1**, always. It does **not** become normal, because the shape (lopsidedness, number of peaks) stays the same.
</details>

<details>
<summary><b>Q14.</b> Ravi scored 72 in a test with μ = 60 and σ = 8. Priya scored 88 in a test with μ = 80 and σ = 4. Who did relatively better?</summary>

z_Ravi = (72 − 60)/8 = 12/8 = **1.5**. z_Priya = (88 − 80)/4 = 8/4 = **2**. **Priya** did relatively better.
</details>

<details>
<summary><b>Q15.</b> E[X + Y] = E[X] + E[Y] works even when X and Y are dependent, but the variance rule needs independence. Why?</summary>

Averaging a sum never multiplies X and Y together, so the mean rule always works. Variance squares the sum, (X + Y)², and that creates an extra 2XY term. That term only averages to zero when Cov(X, Y) = 0.
</details>
