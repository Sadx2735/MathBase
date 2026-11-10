# Probability — v2.2: Skewness, Kurtosis, Quantiles, Box Plots, KDE, Violin & Q–Q Plots

Builds on [v2.1](Statv2.1.md) (mean, variance, standardisation).
The mean and the SD describe the **centre** and the **width** of a distribution. They say nothing about whether it is **lopsided** or has **rare extreme values**. This part adds two numbers for that (skewness and kurtosis), and then some tools for **looking** at real data: quantiles, box plots, KDE, violin plots and Q–Q plots.

---

## 1. Moments: one family of summary numbers

Mean and variance are the first two members of a family called **moments**. The k-th moment is "the average of X to the power k":

| Name | Formula | What it measures |
|---|---|---|
| 1st raw moment | E[X] = Σ x·P(x) | **location**: the mean μ |
| 2nd central moment | E[(X − μ)²] | **spread**: the variance σ² |
| 3rd standardised moment | E[((X − μ)/σ)³] | **lopsidedness**: the skewness |
| 4th standardised moment | E[((X − μ)/σ)⁴] | **tail weight**: the kurtosis |

> **In plain English:**
> - *raw* = use X as it is;
> - *central* = subtract the mean first, so the position doesn't matter;
> - *standardised* = also divide by σ, so the units and the width don't matter.
>
> After standardising, skew and kurtosis are pure numbers that only describe **shape**.

⚠️ **Correction to my original notes.** I wrote the first moment as p(x₁) + p(x₂) + … That just adds up to 1. Each probability must be multiplied by its value: **E[X] = x₁·p₁ + x₂·p₂ + …**. In the same way, the second moment is x₁²·p₁ + x₂²·p₂ + …

---

## 2. Skewness: the third moment

### Lottery vs insurance

- **Lottery ticket (you buy it):** pay $1. With 1% chance you win $100, a net **+$99**. With 99% chance you lose **−$1**.
- **Insurance (you sell it):** a client pays you $1. With 99% chance nothing happens, so **+$1**. With 1% chance you pay out $100, a net **−$99**.

These are mirror images of each other. Check the first two moments:

| | Lottery | Insurance |
|---|---|---|
| Mean | 0.99·(−1) + 0.01·99 = **0** | 0.99·1 + 0.01·(−99) = **0** |
| Variance E[X²] | 0.99·1 + 0.01·9801 = **99** | **99** |
| SD | **≈ 9.95** | **≈ 9.95** |
| E[X³] | −0.99 + 0.01·970,299 = **+9702** | **−9702** |
| Skew = E[X³]/σ³ | 9702 / 985 ≈ **+9.85** | **−9.85** |

![Lottery vs insurance](diagrams/24-lottery-vs-insurance-skew.svg)

The mean, the variance and the SD are identical, but the two bets feel completely different. The lottery has a small, steady loss with a rare big **win**. Insurance has a small, steady gain with a rare big **loss**. Only the **third** moment tells them apart.

**Why the third power works:** cubing **keeps the sign**. A big deviation to the right, (+99)³, adds a huge positive number. A big deviation to the left, (−99)³, adds a huge negative one. Squaring would make both positive, and the difference would be lost.

$$\text{skew} = E\left[\left(\frac{X-\mu}{\sigma}\right)^3\right]$$

> **In plain English:** skew is positive when the far-away values are mostly on the right (a long right tail), negative when they are on the left, and 0 for a symmetric distribution.

![Skew shapes](diagrams/25-skew-shapes.svg)

| Skew | Shape | Mean vs median | Examples |
|---|---|---|---|
| > 0 | long **right** tail | mean > median | salaries, house prices, waiting times |
| = 0 | symmetric | mean = median | heights, measurement errors |
| < 0 | long **left** tail | mean < median | scores on an easy exam, age at retirement |

This links to v2.1: the tail **pulls the mean** towards it, while the median barely moves.

---

## 3. Kurtosis: the fourth moment

### Which game is riskier?

- **Game 1:** win $1 with probability 1/2, lose $1 with probability 1/2.
- **Game 2:**
  - win 10¢ with probability 100/202, lose 10¢ with probability 100/202;
  - win $10 with probability 1/202, lose $10 with probability 1/202.

| Moment | Game 1 | Game 2 |
|---|---|---|
| Mean | 0 | 0 |
| Variance | 1 | 2·(100/202)·0.01 + 2·(1/202)·100 = **1** |
| Skew | 0 (symmetric) | 0 (symmetric) |
| E[X⁴] = kurtosis (σ = 1) | **1** | 2·(100/202)·0.0001 + 2·(1/202)·10,000 ≈ **99** |

![Kurtosis games](diagrams/26-kurtosis-games.svg)

The first three moments are identical, but Game 2 hides rare **$10 swings**. The fourth power blows up big deviations: a $10 deviation counts **10⁴ = 10,000 times** as much as a $1 deviation. Those rare tail outcomes therefore dominate the 4th moment.

$$\text{kurtosis} = E\left[\left(\frac{X-\mu}{\sigma}\right)^4\right]$$

> **In plain English:** kurtosis asks how much of the variance comes from rare, extreme values rather than from many moderate ones. High kurtosis means fat tails: usually calm, occasionally wild.

- A **normal** distribution has kurtosis **3**. Many libraries report **excess kurtosis** = kurtosis − 3, so that the normal scores 0. Check which one you're reading: `scipy.stats.kurtosis` returns the excess by default.
- Kurtosis is about the **tails**, not about how pointy the peak is. That's a common myth.

### Where kurtosis is useful

![Heavy tails](diagrams/27-heavy-tails.svg)

Both curves have the **same mean and SD**. The orange one (a Laplace distribution, kurtosis 6) lands beyond ±3σ about **5× more often** than the normal does. If you model **stock returns, insurance claims or server response times** as normal, you will badly underestimate how often disasters happen. A high kurtosis in your data is the warning sign.

---

## 4. Quantiles

The **q-quantile** is the value with a fraction q of the data **below** it.

- The **median** is the 0.5-quantile: half the data below, half above.
- The **quartiles** split the data into four equal-count pieces: Q1 (25%), Q2 = median (50%) and Q3 (75%).
- **Percentiles** use hundredths. "You scored in the 90th percentile" means 90% of people scored lower than you.

### Newspaper budget example

The sorted data is 8.7, 14.2, 18.3, 18.4 (n = 4).

- **Median:** n is even, so average the two middle values (positions 2 and 3): (14.2 + 18.3)/2 = **16.25**.
- **Q1:** a quarter of 4 values is exactly 1 value, so 8.7 is the natural "lowest 25%". Software usually **interpolates** between neighbours instead. NumPy's default gives Q1 = 8.7 + 0.75·(14.2 − 8.7) = **12.825**.

⚠️ **Note on my original notes.** "Q1 = 8.7" is one valid convention. With so few points, the different rules disagree a lot. With hundreds of points, they all agree closely. Quote which tool or rule you used.

![Quantiles](diagrams/28-quantiles.svg)

**For a distribution:** a quantile reads the **CDF backwards**. Go across from 0.25 on the y-axis until you hit the curve, then drop down: that x is Q1. In symbols, Q(q) = F⁻¹(q). This is the same inverse CDF that v1.3 used for inverse-transform sampling.

**Why quantiles are useful:** like the median, they are **robust**. One crazy value can move the mean and the SD a long way, but it barely moves Q1, the median or Q3.

---

## 5. IQR and the box plot

**IQR (inter-quartile range) = Q3 − Q1**, the width of the **middle 50%** of the data.

For a hill-shaped (normal) distribution, the middle 50% sits in the centre, with 25% in each tail. Q1 and Q3 are at **μ ± 0.67σ**, so the **IQR ≈ 1.35σ**.

![IQR and fences on a normal](diagrams/29-iqr-fences-normal.svg)

### Building a box plot

Data (n = 11): 2, 4, 5, 6, 7, 8, 9, 10, 11, 13, 30

1. **Quartiles** (NumPy's rule): Q1 = **5.5**, median = **8**, Q3 = **10.5**, so the IQR = **5**.
2. **Box:** from Q1 to Q3, with a line at the median.
3. **Fences:** these are invisible limits.
   - lower fence = Q1 − 1.5·IQR = 5.5 − 7.5 = **−2**
   - upper fence = Q3 + 1.5·IQR = 10.5 + 7.5 = **18**
4. **Whiskers:** they go out to the **most extreme real data point inside the fences**. Here that is 2 on the left and 13 on the right.
5. **Outliers:** any point beyond a fence is drawn as its own dot. Here that is **30**.

![Box plot anatomy](diagrams/30-boxplot-anatomy.svg)

⚠️ **Correction to my original notes.** I wrote that the whisker ends are min(Q3 + 1.5·IQR, x_max) and max(Q1 − 1.5·IQR, x_min). The idea is right, but a whisker always ends **on an actual data point**:

- upper whisker = the largest x with x ≤ Q3 + 1.5·IQR;
- lower whisker = the smallest x with x ≥ Q1 − 1.5·IQR.

So in the example the upper whisker stops at 13, not at the fence value 18.

### Why is a box plot useful?

- **Five numbers in one glance:** min, Q1, median, Q3 and max. You can see the centre, the spread and any skew. A median that sits off-centre in the box, or one whisker much longer than the other, means skew.
- **Outliers are flagged automatically.**
- **It is great for comparing groups:** draw 10 box plots side by side, e.g. salaries per department, and differences jump out.

### Why 1.5 × IQR?

It's a rule of thumb from John Tukey, and it's tuned so that it **rarely cries wolf on normal data**:

- The fence is at Q3 + 1.5·IQR ≈ 0.67σ + 1.5·1.35σ ≈ **2.7σ**.
- Only about **0.7%** of normal data lies beyond ±2.7σ, so anything out there is genuinely unusual.
- A multiplier of 1 would flag about 4.3% of perfectly normal data. A multiplier of 3 would miss real problems.

**Why not just use mean ± 3·SD?** Because the outlier **inflates the SD that is supposed to catch it**. In the data above, the mean is 9.5 and the SD is 7.2, so mean + 3·SD ≈ **31**, and **30 would not be flagged**. Q1 and Q3 hardly notice the 30, so the IQR fences still catch it.

> **In plain English:** the box is where the typical half of the data lives. Points more than one-and-a-half box-widths beyond the box are suspicious.

---

## 6. Kernel density estimation (KDE)

**The problem:** we have a list of data points and want a **smooth PDF** for them. A histogram is a blocky answer, and its shape depends on where you happen to put the bin edges. KDE is the smooth version.

**The recipe:**

1. Put a small **bump** (a "kernel", usually a tiny normal curve) **centred on every data point**.
2. Give every bump the same area, **1/n**, so that the total area is 1, just as a PDF needs.
3. **Add the bumps up.** Where many points crowd together, the bumps stack up into a high peak. A lonely point gives a small, low hump.

![KDE bumps](diagrams/31-kde-bumps.svg)

$$\hat f(x) = \frac{1}{n}\sum_{i=1}^{n} K_h(x - x_i) = \frac{1}{n\,h}\sum_{i=1}^{n} K\!\left(\frac{x - x_i}{h}\right)$$

> **In plain English:** to get the estimated density at x, look at every data point, ask "how close is it to x?" (a nearby point contributes a lot, a far one almost nothing), and average those contributions. h is the width of each bump.

⚠️ **Correction to my original notes.** I wrote "multiply them by the value of the points". The bumps are **not** multiplied by the data values. Each bump is **centred at** a data point and scaled by **1/n**, so every point gets an equal vote. (In a *weighted* KDE you could give some points more weight, but that's an extra choice, not the default.)

### The one knob: the bandwidth h

![KDE bandwidth](diagrams/32-kde-bandwidth.svg)

| h | Result |
|---|---|
| too small | a spike on each point: you're plotting the noise |
| about right | the real bumps show and the noise is smoothed out |
| too large | everything blurs into one hump, and real structure (two groups) disappears |

The common default is **Silverman's rule**, h ≈ 1.06·σ̂·n^(−1/5). It's good for roughly normal data and over-smooths data with several peaks. Seaborn's `kdeplot` and SciPy's `gaussian_kde` pick h automatically, and you can override it (`bw_adjust` in seaborn).

---

## 7. Violin plots

A **violin plot = box plot + KDE**. The KDE is drawn **mirrored on both sides** of the box, so the **width of the violin at any height shows how much data sits at that value**.

![Box vs violin](diagrams/33-box-vs-violin.svg)

**Why bother?** A box plot only keeps five numbers, so it can hide the shape completely. In the example the data is two separate groups (think: weights of cats *and* dogs in one column).

- The box plot looks like an ordinary single hump, and its **median falls in the gap where there is no data at all**.
- The violin shows the two groups immediately.

> **Rule of thumb:** use box plots to compare many groups quickly, and violin plots when the **shape** matters (several peaks, gaps, skew).

---

## 8. Q–Q plots: "is my data normal?"

A **Q–Q (quantile–quantile) plot** compares the quantiles of your data with the quantiles of a reference distribution, usually the standard normal.

**How it's made:**

1. **Standardise** the data, z = (x − mean)/SD, and **sort** it.
2. The i-th smallest value is the data's (i − 0.5)/n quantile. That is its y-coordinate.
3. Its x-coordinate is where a **perfect normal** would put that same quantile: Φ⁻¹((i − 0.5)/n).
4. Plot the points and add the line y = x.

![Q-Q plots](diagrams/34-qq-plots.svg)

**How to read it:**

| Pattern | Meaning |
|---|---|
| points on the line | data ≈ normal ✓ |
| curve bending **up** on the right (and flat on the left) | right-skewed: the big values are bigger than a normal allows |
| curve bending **down** on the left | left-skewed |
| **S-shape**: low end below the line, high end above it | **fat tails** (high kurtosis): both extremes are more extreme than normal |
| inverted S | thin tails (fewer extremes than normal) |

**Why standardise first?** After standardising, a perfect normal lands exactly on **y = x**, so you only have to judge one reference line. Without it, normal data still forms a **straight** line, just with slope σ and intercept μ. It's straightness that matters.

> **In plain English:** a Q–Q plot lines up your sorted data against where a normal distribution says each value should be. If every point is where it should be, the dots form a straight line. Wherever they leave the line, that's the part of your data that isn't normal.

Q–Q plots are the standard check before using methods that assume normality: t-tests, linear-regression residuals, z-score outlier rules. In Python: `scipy.stats.probplot(data, plot=plt)` or `statsmodels.api.qqplot(data, line="45")`.

---

## Formula cheat sheet

| Idea | Formula | Plain English |
|---|---|---|
| k-th raw moment | E[Xᵏ] = Σ xᵏ·P(x) | average of X to the k-th power |
| Skewness | E[((X − μ)/σ)³] | sign shows which side the long tail is on |
| Kurtosis | E[((X − μ)/σ)⁴] | how much of the variance comes from rare extremes; normal = 3 |
| Excess kurtosis | kurtosis − 3 | normal = 0, fat tails > 0 |
| q-quantile | Q(q) = F⁻¹(q) | value with a fraction q of the data below it |
| IQR | Q3 − Q1 | width of the middle 50%; ≈ 1.35σ for normal data |
| Outlier fences | Q1 − 1.5·IQR, Q3 + 1.5·IQR | ≈ ±2.7σ for normal data; 0.7% beyond |
| KDE | f̂(x) = (1/n) Σ K_h(x − xᵢ) | a bump of area 1/n on each point, added up |
| Silverman's h | 1.06·σ̂·n^(−1/5) | default bandwidth |
| Q–Q point | (Φ⁻¹((i − 0.5)/n), z₍ᵢ₎) | on the line y = x means normal |

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>Q1.</b> Name what the 1st, 2nd, 3rd and 4th moments measure.</summary>

1st: location (mean). 2nd (central): spread (variance). 3rd (standardised): lopsidedness (skew). 4th (standardised): tail weight (kurtosis).
</details>

<details>
<summary><b>Q2.</b> X = −1 with probability 0.9 and X = 9 with probability 0.1. Find the mean and the sign of the skew.</summary>

Mean = −0.9 + 0.9 = **0**. E[X³] = 0.9·(−1) + 0.1·729 = **+72**, so the skew is **positive**: a rare big value on the right, like a lottery.
</details>

<details>
<summary><b>Q3.</b> Why can't the 2nd moment tell a lottery from insurance, but the 3rd can?</summary>

Squaring makes every deviation positive, so (+99)² and (−99)² look the same. Cubing keeps the sign, so a rare big win gives a large + and a rare big loss gives a large −.
</details>

<details>
<summary><b>Q4.</b> What is the skew of any symmetric distribution, and why?</summary>

**0.** For every deviation +d there is an equally likely −d, and (+d)³ + (−d)³ = 0.
</details>

<details>
<summary><b>Q5.</b> Salaries are right-skewed. Which is bigger, the mean or the median?</summary>

The **mean**. The long right tail of very high salaries pulls the mean up, while the median stays with the typical person.
</details>

<details>
<summary><b>Q6.</b> Games 1 and 2 (section 3) have the same mean, variance and skew. Which has the higher kurtosis, and what does that mean in practice?</summary>

**Game 2** (≈ 99 vs 1). Most of its variance comes from rare $10 swings, so it is usually calm but occasionally hits you hard. That is tail risk.
</details>

<details>
<summary><b>Q7.</b> Is kurtosis a measure of how "peaked" a distribution is?</summary>

**No.** That's a common myth. Kurtosis is driven by the **tails**, i.e. how much probability sits far from the mean. Two distributions can have similar peaks and very different kurtosis.
</details>

<details>
<summary><b>Q8.</b> A library reports kurtosis = 0 for your data. Is it flat?</summary>

Probably not. Most libraries (e.g. SciPy) report **excess** kurtosis = kurtosis − 3. A value of 0 means the tails are like a **normal** distribution's.
</details>

<details>
<summary><b>Q9.</b> Find the median of 3, 7, 1, 9, 4. Then find it after adding 100 to the list.</summary>

Sorted: 1, 3, 4, 7, 9, so the median is **4**.
With 100 added: 1, 3, 4, 7, 9, 100, so the median is (4 + 7)/2 = **5.5**. The median barely moved, even though 100 is extreme.
</details>

<details>
<summary><b>Q10.</b> For the data 1, 2, 3, 4, 5, 6, 7, 8, 9, 40, use Q1 = 3.25 and Q3 = 7.75. Find the IQR, the fences, the whiskers and any outliers.</summary>

IQR = 4.5, so 1.5·IQR = 6.75.
Fences: 3.25 − 6.75 = **−3.5** and 7.75 + 6.75 = **14.5**.
Whiskers: **1** and **9** (the last real points inside the fences).
Outlier: **40**.
</details>

<details>
<summary><b>Q11.</b> Why is the multiplier 1.5 and not 1 or 3?</summary>

For normal data the 1.5·IQR fences sit at about ±2.7σ, so only about **0.7%** of genuinely normal points get flagged. A multiplier of 1 flags about 4% (too many false alarms). A multiplier of 3 sits near ±4.7σ and misses real outliers.
</details>

<details>
<summary><b>Q12.</b> Why can the "mean ± 3·SD" rule miss an outlier that the IQR rule catches?</summary>

The outlier itself inflates the mean and, even more, the SD, which widens the very limits meant to catch it. Quartiles are robust: one extreme point barely moves Q1 or Q3. In section 5, 30 lies inside mean + 3·SD ≈ 31 but outside the IQR fence of 18.
</details>

<details>
<summary><b>Q13.</b> In a KDE, why does each bump have area 1/n? What happens as h → 0 and as h → ∞?</summary>

n bumps of area 1/n add up to a total area of **1**, which a PDF must have.
As **h → 0**, you get a spike at each data point, which just reproduces the data (noise).
As **h → ∞**, you get one wide, flat hump that hides all structure.
</details>

<details>
<summary><b>Q14.</b> What does a KDE give you that a histogram doesn't?</summary>

A **smooth** curve that doesn't depend on where you put the bin edges. You still have to choose a bandwidth, which plays the role of the bin width.
</details>

<details>
<summary><b>Q15.</b> When can a box plot mislead you, and what fixes it?</summary>

When the data has **several peaks or gaps**. The box plot only keeps five numbers, so two clusters can look like one hump, with the median in an empty gap. A **violin plot** (or a KDE or histogram) shows the real shape.
</details>

<details>
<summary><b>Q16.</b> On a Q–Q plot against the normal, the points follow the line in the middle but rise above it on the right and fall below it on the left (an S-shape). What does that tell you?</summary>

The data has **fat tails** (high kurtosis). Both extremes are more extreme than a normal would produce, which is typical of stock returns.
</details>

<details>
<summary><b>Q17.</b> Your data isn't standardised, but its Q–Q plot is a straight line with slope 4 and intercept 10. Is it normal? What are μ and σ?</summary>

**Yes, it's roughly normal.** Straightness is what matters. The intercept gives **μ ≈ 10** and the slope gives **σ ≈ 4**.
</details>
