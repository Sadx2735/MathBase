# Probability — v1.3: Random Variables & Distributions

Builds on [v1.1](Statv1.1.md) (basics, counting) and [v1.2](Statv1.2.md) (Bayes).
This part answers one question: **"How do we list *all* the possible results of an experiment, and how likely each one is?"**

---

## 1. Random variable

A coin lands on "heads" or "tails". Those are words, not numbers. We can't add or average words. So we turn each result into a number.

A **random variable X** is a rule that gives each result of an experiment a **number**.

**Examples:**
- Flip one coin. Let X = number of heads.
  - X can be 0 or 1. We write X ∈ {0, 1} ("∈" means "is one of").
  - P(X = 0) = 0.5 and P(X = 1) = 0.5.
- Flip 10 coins. Let X = number of heads.
  - X can be 0, 1, 2, …, up to 10. So X ∈ {0, 1, …, 10}.

![Random variable mapping outcomes to numbers](diagrams/11-random-variable-mapping.svg)

> **Simple idea:** the coin doesn't know any numbers. X is a label we stick on each result, so we can do maths with it: add, average, draw a chart.

### Discrete vs continuous

There are two kinds of random variable.

- **Discrete** = you can list the values one by one, like 0, 1, 2, 3. Think "counting".
- **Continuous** = any value in a range is possible, like 1.2, 1.22, 1.222… Think "measuring".

| | Discrete | Continuous |
|---|---|---|
| Values | you can **list** them (0, 1, 2, …) | **any** value in a range (1.2, 1.22, 1.222…) |
| Examples | number of heads, a dice roll, emails per hour | height, how long a call lasts, temperature |
| Described by | a **PMF**: the probability of each value | a **PDF**: a density curve (section 5) |

---

## 2. Probability distribution (PMF)

A **distribution** is a full list: every value X can take, and the probability of each one.

**Example: flip three coins.** Let X = number of heads. There are 8 equally likely results (2 × 2 × 2 = 8).

| Results | X | How many results | P(X = x) |
|---|---|---|---|
| TTT | 0 | 1 | 1/8 |
| HTT, THT, TTH | 1 | 3 | 3/8 |
| HHT, HTH, THH | 2 | 3 | 3/8 |
| HHH | 3 | 1 | 1/8 |

Check: 1/8 + 3/8 + 3/8 + 1/8 = 8/8 = 1 ✅

For a discrete X, this table is called the **Probability Mass Function (PMF)**. We write it as p_X(x):

$$p_X(x) = P(X = x), \qquad p_X(x) \ge 0, \qquad \sum_x p_X(x) = 1$$

- p_X(x) = the probability that X equals the value x.
- Every probability is 0 or more (never negative).
- Σ (sigma) means "add them all up". All the probabilities together must add up to 1.

> **Simple idea:** think of 1 kg of sand shared out between the values. The PMF says how much sand sits on each value. All the sand together is exactly 1, because *something* always happens.

---

## 3. Bernoulli distribution: one yes/no trial

A **Bernoulli trial** is **one** try with only two results:
- **success**, written as 1, with probability p,
- **failure**, written as 0, with probability 1 − p.

$$P(X = 1) = p, \qquad P(X = 0) = 1 - p$$

**Examples:**
- One coin toss: heads = success, p = 0.5.
- Does this email get clicked? p = 0.03.
- Is this part broken (defective)? p = 0.01.

| | |
|---|---|
| Mean | p |
| Variance | p(1 − p) |

> **Simple idea:** Bernoulli is the smallest building block, like a single brick. The binomial distribution (next) is just **n Bernoulli trials added up**.

---

## 4. Binomial distribution: how many successes in n trials?

**Question:** you flip a coin 5 times. What is the chance of getting exactly 2 heads?

We answer it in three steps.

### Step 1: the chance of *one exact* order

Let p = P(heads). The chance of the exact order **H H T T T** is:

$$p \cdot p \cdot (1-p) \cdot (1-p) \cdot (1-p) = p^2 (1-p)^3$$

> **Why multiply?** The flips don't affect each other (they are **independent**, v1.1). So the chance of "this AND this AND this…" is all the chances multiplied together. If p were 1, each factor would be 1 and change nothing. With p = 0.5, every flip "costs" a factor of 0.5.

### Step 2: count how many orders give the same result

2 heads in 5 flips can happen in **10** different orders (HHTTT, HTHTT, HTTHT, …). Each order has the same chance: (1/2)⁵ = 1/32.

![Binomial coefficient arrangements](diagrams/13-binomial-coefficient.svg)

To count the orders we use the **binomial coefficient**, read "n choose x":

$$\binom{n}{x} = \frac{n!}{x!\,(n-x)!}$$

- **n!** ("n factorial") = n × (n − 1) × … × 1. It counts the orders if all n items were different.
- **÷ x!** because the x heads all look the same. Swapping two heads gives the same sequence.
- **÷ (n − x)!** because the tails all look the same too.

**Small check with the 3 symbols "1 2 2":**
- 3! = 6 orders if we pretend the two 2s are different: 122, 122, 212, 221, 212, 221.
- But only **3** are really different: 122, 212, 221.
- Formula: 3! / (1! · 2!) = 6 / 2 = **3** ✅
- (The 1! is for the single "1". It equals 1, but it belongs in the formula.)

### Step 3: put them together

$$\boxed{P(X = x) = \binom{n}{x}\, p^x\, (1-p)^{n-x}}, \qquad x = 0, 1, \dots, n$$

- n = number of trials (flips).
- x = number of successes we want (heads).
- p = chance of success in one trial.

> **Simple idea:** (how many orders give x successes) × (the chance of any one of those orders).

![Binomial PMF for 5 flips](diagrams/12-binomial-pmf-n5.svg)

**Examples (fair coin, p = ½)**

| Question | Working | Answer |
|---|---|---|
| 2 heads in 5 flips | C(5,2) · (½)⁵ = 10 × 1/32 = 10/32 | ≈ 0.313 |
| 10 heads in 10 flips | C(10,10) · (½)¹⁰ = 1 × 1/1024 = 1/1024 | ≈ 0.001 |
| 9 heads in 10 flips | C(10,9) · (½)⁹ · (½)¹ = 10 × 1/1024 = 10/1024 | ≈ 0.0098 |

(C(n, x) is just another way to write "n choose x".)

⚠️ **Correction to my original notes.** For 9 heads I wrote only `0.5 × 0.5⁹`. That is the chance of **one** order. But the single tail can be in any of the 10 places. So we must multiply by C(10,9) = 10.

| | |
|---|---|
| Mean | n·p |
| Variance | n·p·(1 − p) |

**Example:** 10 fair flips → mean = 10 × 0.5 = 5 heads on average.

```python
from math import comb
def binom_pmf(x, n, p):
    return comb(n, x) * p**x * (1 - p)**(n - x)

binom_pmf(2, 5, 0.5)   # 0.3125
```

---

## 5. Continuous random variables and the PDF

Think about how long a phone call lasts. It could be 1 minute, 1.1, 1.01, 1.001… There are endless possible values.

For a continuous X, the chance of hitting **one exact value** is **0**.
- Example: P(a call lasts *exactly* 1.000000… minutes) = 0.
- Why? There are infinitely many possible values, all sharing a total chance of 1. Each single point gets nothing.

So we stop asking about one point. We ask about a **range** instead:

**chance of a range = area under a curve**

![PMF vs PDF](diagrams/14-pmf-vs-pdf.svg)

This curve f(x) is called the **Probability Density Function (PDF)**. It must follow three rules:

$$f(x) \ge 0 \text{ for all } x, \qquad \int_{-\infty}^{\infty} f(x)\,dx = 1, \qquad P(a \le X \le b) = \int_a^b f(x)\,dx$$

- The curve is never below 0.
- The total area under the curve is 1. (∫ is the **integral**, which here just means "area under the curve".)
- The chance that X is between a and b = the area under the curve from a to b.

> **Simple idea:** "density" means probability **per unit of length**. It's like the density of a metal: mass per unit of volume. To get a real probability, you multiply density by a width, which gives an area.

| | PMF (discrete) | PDF (continuous) |
|---|---|---|
| What it gives | the real probability P(X = x) | a density, *not* a probability |
| A single point | can be more than 0 | always 0 |
| Can a value be bigger than 1? | no | **yes** (e.g. Uniform(0, 0.5) has height 2) |
| Total | the sum = 1 | the area = 1 |

### CDF: cumulative distribution function

The **CDF** answers: "what is the chance X is **at most** x?" ("Cumulative" means "added up so far".)

$$F(x) = P(X \le x)$$

- It always goes up from 0 to 1. It never goes down.
- For continuous X: **F′(x) = f(x)**. In words: the PDF is the **slope** of the CDF. (F′ means "the slope of F".)
- P(a ≤ X ≤ b) = F(b) − F(a). In words: (chance up to b) minus (chance up to a).

---

## 6. Uniform distribution: every value equally likely

X ~ Uniform(a, b) means every value between a and b is equally likely. ("~" means "follows the distribution".)

$$f(x) = \frac{1}{b-a} \;\text{ for } a \le x \le b, \qquad F(x) = \frac{x-a}{b-a}$$

- f(x) is flat: the same height 1/(b − a) everywhere. That height makes the total area = 1.
- F(x) = the fraction of the range you have covered so far.

**Example:** a call ends at a random moment within 10 minutes. So X ~ Uniform(0, 10), and the height is 1/10.

- P(call ends between minute 2 and minute 5) = (5 − 2)/10 = 3/10 = **0.3**
- P(call ends in the first minute) = 1/10 = **0.1**
- P(call ends at exactly minute 5) = **0** (a single point)

![Uniform PDF and CDF](diagrams/15-uniform-pdf-cdf.svg)

> **Why is the CDF a straight line?** Its slope is the PDF. The PDF is the same everywhere (1/10). So area piles up at the same speed all the way, which gives a straight line.

| | |
|---|---|
| Mean | (a + b)/2 |
| Variance | (b − a)²/12 |

**Example:** for Uniform(0, 10), mean = (0 + 10)/2 = 5 minutes.

---

## 7. Normal (Gaussian) distribution

Measure the heights of many adults. Most are near the average. Very tall and very short people are rare. Draw this and you get a **bell-shaped curve**. That curve is the **normal** (or **Gaussian**) distribution.

$$f(x) = \frac{1}{\sigma\sqrt{2\pi}}\; e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

- **μ (mu) = the mean.** It is the centre of the bell.
- **σ (sigma) = the standard deviation.** It is how wide the bell is.
- e ≈ 2.718 and π ≈ 3.1416 are fixed numbers.

You don't need to memorise this formula. Just remember: centre μ, width σ.

⚠️ **Correction to my original notes.** I had written `1/sqrt(2π·std) · e^(−0.5·(x−μ)²/2)`. The correct version:
- has **σ√(2π)** at the front, with σ **outside** the square root,
- has **2σ²** at the bottom of the power.

Another way to write the power: −½·((x − μ)/σ)².

### The 68–95–99.7 rule

For any normal curve:

![Normal distribution and standardisation](diagrams/16-normal-standardize.svg)

| Range | Chance of landing inside |
|---|---|
| μ ± 1σ | ≈ 68% |
| μ ± 2σ | ≈ 95% |
| μ ± 3σ | ≈ 99.7% |

**Example:** heights with μ = 170 cm and σ = 10 cm. About 68% of people are between 170 − 10 = 160 cm and 170 + 10 = 180 cm.

### Standard normal and standardisation (z-scores)

The **standard normal** is N(0, 1): a normal with μ = 0 and σ = 1. N(μ, σ²) is short for "normal with mean μ and variance σ²".

We can turn any normal into the standard normal with the **z-score**:

$$z = \frac{x - \mu}{\sigma}$$

> **Simple idea:** z = "how many standard deviations is x away from the mean?"
> Example with μ = 2 and σ = 2.5: x = 7 gives z = (7 − 2)/2.5 = 5/2.5 = **2**. So 7 is 2 standard deviations above average.

This is called **standardisation**.

### "Is standardisation used to check whether two distributions are the same?"

**Not quite.**
- Standardisation does **not** test whether two distributions are the same.
- It **removes the differences in centre (μ) and width (σ)**. Then values on different scales can be compared.
- After standardising, *any* two normal distributions look the same: N(0, 1).
- To check if data really *is* normal, or if two datasets have the same shape, use a **Q-Q plot** (a chart comparing the two shapes) or a test such as **Kolmogorov–Smirnov (KS)**.

**Where standardisation is used:**

| Use | How z helps |
|---|---|
| **Comparing scores across exams** | Maths 80 (μ 70, σ 5) gives z = 2. Physics 85 (μ 75, σ 10) gives z = 1. Maths is *relatively* better, even though the number is lower. |
| **Looking up probabilities** | Every normal question becomes a question about N(0, 1). So one z-table (or `norm.cdf`) answers them all. |
| **Finding outliers** (strange values) | Flag points with \|z\| > 3. That happens only 0.3% of the time by chance. |
| **Scaling features in machine learning** | Height in cm and salary in ₹ get the same scale. Then no feature dominates gradient descent or k-NN distances. |
| **Growth charts and percentiles** | A child's height or weight is given as a z-score or percentile for their age. |
| **IQ** | Set up so that μ = 100 and σ = 15. IQ 130 → z = 2 → top ~2.3%. |
| **Quality control (Six Sigma)** | A process is "in control" if measurements stay within μ ± 3σ. |

**Where the normal shows up:** heights, weights, IQ, measurement errors, exam scores, sensor noise. Also, by the *Central Limit Theorem* (see [v3.1](Statv3.1.md)), averages of many random things are close to normal.

---

## 8. Sampling: making random numbers from a distribution

**Goal:** a computer can easily give you **uniform** random numbers u between 0 and 1. How do you turn them into numbers from *any* distribution, like a normal?

**Inverse transform sampling** (three steps):

1. Pick a random u from Uniform(0, 1).
2. On the CDF chart, go **across** from u to the CDF curve, then **down** to the x-axis.
3. The x you land on is your sample. We write it as x = F⁻¹(u) (F⁻¹ = "the CDF run backwards").

![Inverse transform sampling](diagrams/17-inverse-transform-sampling.svg)

> **Why does it work?** Where the PDF is high, the CDF is steep. A steep CDF sends lots of u values onto a small range of x. So samples bunch up exactly where they *should*.

**Easy case:** for the uniform, F⁻¹(u) = a + u(b − a).

**Hard case:** for the **normal**, the CDF has **no simple formula**. So F⁻¹ is worked out by the computer step by step (that's what `scipy.stats.norm.ppf` does). In practice, libraries often use faster tricks such as the Box–Muller transform.

```python
import numpy as np
from scipy.stats import norm

u = np.random.rand(5)
x = norm.ppf(u, loc=2, scale=2.5)   # inverse CDF → samples from N(2, 2.5²)

# or simply
x = np.random.normal(2, 2.5, size=5)
```

---

## Use cases at a glance

| Distribution | Type | Describes | Real examples |
|---|---|---|---|
| **Bernoulli(p)** | discrete | one yes/no event | click or no click, pass or fail, broken or OK |
| **Binomial(n, p)** | discrete | number of successes in n independent yes/no tries | broken parts in a batch of 100, heads in 10 flips, buyers out of 1000 visitors, A/B tests |
| **Uniform(a, b)** | continuous | "anything in the range is equally likely" | random arrival in a time window, rounding error, random number generators |
| **Normal(μ, σ)** | continuous | values bunched around an average | heights, IQ, measurement noise, exam scores, errors in ML models |

---

## Formula cheat sheet

| Idea | Formula | Simple meaning |
|---|---|---|
| PMF | p(x) = P(X = x), Σ p(x) = 1 | the chance of each value; all add to 1 |
| PDF | P(a ≤ X ≤ b) = ∫ₐᵇ f(x) dx | chance = area under the curve |
| CDF | F(x) = P(X ≤ x), F′ = f | chance added up so far |
| Bernoulli | P(1) = p, P(0) = 1 − p | one yes/no try |
| Binomial | C(n,x) pˣ (1−p)ⁿ⁻ˣ | number of orders × chance of one order |
| Binomial coefficient | n! / (x!(n−x)!) | all orders ÷ swaps that look the same |
| Uniform | f = 1/(b−a) | flat, so chance = width/(b − a) |
| Normal | (1/σ√2π) e^(−(x−μ)²/2σ²) | a bell centred at μ, width σ |
| z-score | z = (x − μ)/σ | how many SDs from the mean |
| Inverse transform | x = F⁻¹(u) | uniform u → any distribution |

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>W1.</b> Roll one die. Let X = the number shown. What values can X take, and what is P(X = 4)?</summary>

X ∈ {1, 2, 3, 4, 5, 6}. Each is equally likely, so P(X = 4) = **1/6**.
</details>

<details>
<summary><b>W2.</b> A test score has μ = 50 and σ = 10. What is the z-score of 70?</summary>

z = (70 − 50)/10 = 20/10 = **2**. The score is 2 standard deviations above average.
</details>

<details>
<summary><b>Q1.</b> Discrete or continuous? (a) the number of cars passing in an hour, (b) the time until the next car, (c) a shoe size, (d) the exact weight of an apple.</summary>

(a) discrete · (b) continuous · (c) discrete (sizes come in fixed steps) · (d) continuous
</details>

<details>
<summary><b>Q2.</b> You roll two dice. X = their sum. What are P(X = 7) and P(X = 12)? Do all the P(X = x) values add up to 1?</summary>

P(X = 7) = 6/36 = 1/6 · P(X = 12) = 1/36. Yes: the 11 values (2 to 12) add up to 36/36 = 1.
</details>

<details>
<summary><b>Q3.</b> Work out C(6, 3). Why do you divide by 3! twice?</summary>

C(6,3) = 6!/(3!·3!) = 720/(6·6) = **20**. There are 6! orders of 6 items. The 3 heads look the same (÷3!), and the 3 tails look the same (÷3!).
</details>

<details>
<summary><b>Q4.</b> What is the chance of exactly 3 heads in 6 fair coin flips?</summary>

C(6,3)·(½)⁶ = 20/64 = **0.3125**
</details>

<details>
<summary><b>Q5.</b> 10% of parts are broken and you pick 10. What is P(none broken)? What is P(at least one broken)?</summary>

P(0) = 0.9¹⁰ ≈ **0.349**. P(at least 1) = 1 − 0.349 ≈ **0.651** (the "1 minus the opposite" trick from v1.1).
</details>

<details>
<summary><b>Q6.</b> X is Binomial(n = 20, p = 0.3). What are its mean and variance?</summary>

Mean = 20 × 0.3 = **6**. Variance = 20 × 0.3 × 0.7 = **4.2** (so σ ≈ 2.05).
</details>

<details>
<summary><b>Q7.</b> How is Bernoulli related to binomial?</summary>

Bernoulli is a binomial with n = 1 (just one try). A binomial is the sum of n independent Bernoulli(p) tries.
</details>

<details>
<summary><b>Q8.</b> Why is P(X = 3.0) = 0 for a continuous X, even though 3.0 can happen?</summary>

Chance = area under the PDF. A single point has width 0, so its area is 0. Infinitely many values share the total chance of 1.
</details>

<details>
<summary><b>Q9.</b> A PDF has height f(x) = 2 at some point. Is that a mistake?</summary>

**No.** Density is not probability. Example: Uniform(0, 0.5) has height 2 everywhere on its range, and its area is 0.5 × 2 = 1 ✅. Only the *area* must be at most 1.
</details>

<details>
<summary><b>Q10.</b> A bus comes every 15 minutes and you arrive at a random time. What is P(you wait at most 5 minutes)? What is the average wait?</summary>

Wait ~ Uniform(0, 15). P(wait ≤ 5) = 5/15 = **1/3**. Mean = (0 + 15)/2 = **7.5 min**.
</details>

<details>
<summary><b>Q11.</b> Heights are N(μ = 170 cm, σ = 10 cm). Roughly what share of people are between 150 and 190 cm? And above 190 cm?</summary>

150 to 190 is μ ± 2σ, so **≈ 95%**. Above 190 cm is half of the other 5%, so **≈ 2.5%**.
</details>

<details>
<summary><b>Q12.</b> You got 80 in Maths (class μ = 70, σ = 5) and 85 in Physics (μ = 75, σ = 10). Which subject did you do better in, compared to the class?</summary>

z_maths = (80 − 70)/5 = **2**. z_physics = (85 − 75)/10 = **1**. **Maths** is better compared to the class, even though the raw score is lower.
</details>

<details>
<summary><b>Q13.</b> IQ ~ N(100, 15). What is the z-score of IQ 130, and roughly what share of people score higher?</summary>

z = (130 − 100)/15 = **2**. Above it: ≈ (100% − 95%)/2 = **≈ 2.5%** (exact: 2.28%).
</details>

<details>
<summary><b>Q14.</b> True or false: if two datasets both have mean 0 and SD 1 after standardising, they come from the same distribution.</summary>

**False.** Standardising *always* gives mean 0 and SD 1, whatever the shape. It only removes differences in centre and width. To compare shapes, use a Q-Q plot or a KS test.
</details>

<details>
<summary><b>Q15.</b> Using inverse transform sampling with X ~ Uniform(0, 10), what sample do you get for u = 0.37?</summary>

F(x) = x/10 = 0.37, so **x = 3.7**.
</details>

<details>
<summary><b>Q16.</b> In inverse transform sampling for a normal, why do most samples land near μ?</summary>

The CDF is steepest around μ, where the PDF is highest. A wide band of u values maps onto a narrow band of x values there. So samples pile up near μ.
</details>
