# Probability — v2.3: Joint, Marginal & Conditional Distributions

Builds on [v1.2](Statv1.2.md) (conditional probability, independence, trees) and [v1.3](Statv1.3.md) (PMF, PDF).
So far every distribution described **one** random variable. Real questions usually involve **two at once**: age *and* height, waiting time *and* customer rating. A **joint distribution** describes both together. From it we can get back each variable on its own (the **marginal**) or one variable given a fixed value of the other (the **conditional**).

---

## 1. Joint distribution (discrete)

### The age and height example

We have 10 children. For each one we record the age X (7, 8 or 9) and the height Y (45, 47 or 49 inches).

**Question:** if we pick a child at random, what is P(age 9 **and** 49 in tall)?

**Doing it in two steps (my original approach):**

- 4 of the 10 children are 9 years old, so P(age 9) = **4/10**.
- Of those 4 nine-year-olds, 3 are 49 in tall, so P(49 | age 9) = **3/4**.
- Multiply: P(9, 49) = 4/10 × 3/4 = **3/10**.

![Chain rule tree](diagrams/36-chain-rule-tree.svg)

**Doing it in one step:** instead of recomputing this for every question, pre-calculate the answer for **every pair** and store it in a grid. That grid **is** the joint distribution.

![Joint table](diagrams/35-joint-table.svg)

| | 45 in | 47 in | 49 in |
|---|---|---|---|
| **age 7** | 2/10 | 1/10 | 0 |
| **age 8** | 0 | 1/10 | 2/10 |
| **age 9** | 0 | 1/10 | **3/10** |

Each cell is **P(X = x, Y = y)**, the probability that *both* things happen. The comma means "and". Now P(9, 49) is a lookup: **3/10**.

A valid joint PMF follows the same two rules as any PMF:

1. every cell is ≥ 0;
2. all the cells together add up to **1**.

> **In plain English:** the joint distribution is a lookup table with one probability per *combination* of values. Pick a row (age) and a column (height), and the cell tells you how likely that exact pair is.

### Independent vs dependent

The two-step calculation above is the **chain rule** (the multiplication rule from v1.2):

$$P(x, y) = P(x)\cdot P(y \mid x)$$

It **always** works, whether or not X and Y are related. When they are **independent**, knowing x tells you nothing about y, so P(y | x) = P(y) and the rule simplifies to

$$P(x, y) = P(x)\cdot P(y) \qquad \text{(independent only)}$$

![Independent vs dependent](diagrams/37-independent-vs-dependent.svg)

- **Two dice:** every cell is 1/6 × 1/6 = 1/36. The grid is exactly "row probability × column probability", so the dice are independent.
- **Age and height:** P(9) × P(49) = 0.4 × 0.5 = 0.20, but the real cell is **0.30**. Older children tend to be taller, so the variables are **dependent**.

To prove independence, the product rule must hold in **every** cell. One failing cell is enough to show dependence.

⚠️ **Correction to my original notes.** I wrote "P(x)·P(y) for independent, P(x)·P(y/x) for dependent".
- The notation is **P(y | x)** with a vertical bar ("y given x"), not P(y/x). A slash looks like division.
- P(x)·P(y | x) isn't only for dependent variables. It is **always** true. For independent variables it just happens that P(y | x) = P(y), so it reduces to P(x)·P(y).

---

## 2. Joint distribution (continuous)

### Waiting time and customer rating

A call centre records, for every call, the **waiting time** X (0 to 10 minutes) and the **customer rating** Y (0 to 10 points). Both are continuous, so instead of a table we have a **joint PDF** f(x, y): a surface floating above the (x, y) plane. It is high where combinations are common and low where they are rare.

As a working example, take

$$f(x, y) = \frac{(10-x)\,y + x\,(10-y)}{5000}, \qquad 0 \le x \le 10,\ 0 \le y \le 10$$

The first term is large for **short waits with high ratings**. The second is large for **long waits with low ratings**.

![Joint PDF heatmap](diagrams/38-joint-pdf-heatmap.svg)

Just like a single PDF, the **height of f is not a probability**. Probability is the **volume** under the surface over a region:

$$P\big((X, Y) \in R\big) = \iint_R f(x, y)\,dx\,dy$$

$$P(a \le X \le b,\ c \le Y \le d) = \int_a^b \int_c^d f(x, y)\,dy\,dx$$

The rules are the 2-D versions of the 1-D ones: f(x, y) ≥ 0 everywhere, and the total volume ∬ f dx dy = 1.

> **In plain English:** for one variable, probability = area under a curve. For two variables, probability = volume under a surface. Pick a patch of the floor, and the amount of "surface material" above that patch is the probability of landing in it.

⚠️ **Note on my original notes.** The draft linked a formula image (`JointDistro.png`) from a local path on my machine, so it doesn't show up in the repo. The two formulas above replace it.

### Worked example: region A

**A** = short wait (0 ≤ x ≤ 2) and high rating (8 ≤ y ≤ 10).

1. **Inner integral, over y** (x is held fixed):
   ∫₈¹⁰ [(10 − x)·y + x·(10 − y)] dy = (10 − x)·18 + x·2 = 180 − 16x.
   (We used ∫₈¹⁰ y dy = (100 − 64)/2 = 18 and ∫₈¹⁰ (10 − y) dy = 20 − 18 = 2.)
2. **Outer integral, over x:** ∫₀² (180 − 16x) dx = 360 − 32 = 328.
3. **Divide by 5000:** P(A) = 328/5000 ≈ **0.066**.

For **B** = long wait (8 ≤ x ≤ 10) and high rating, the same steps give ∫₈¹⁰ (180 − 16x) dx = 72, so P(B) = 72/5000 ≈ **0.014**.

Both squares have the same size, but a high rating is **4.6 times** as likely after a short wait. Waiting time and rating are clearly dependent.

---

## 3. Marginal distributions

The **marginal** distribution of one variable is its distribution on its own, **ignoring** the other. You get it by adding up (or integrating) the joint over every value of the other variable:

$$P(x) = \sum_y P(x, y) \qquad\qquad f_X(x) = \int f(x, y)\,dy$$

In the age and height grid:

- **P(age 9)** = add up the **age-9 row**: 0 + 1/10 + 3/10 = **4/10**.
- **P(height 49)** = add up the **49-in column**: 0 + 2/10 + 3/10 = **5/10**.

The name comes from where these sums are written: in the **margins** of the table (the purple column and the green row in the grid diagram above).

> **In plain English:** to forget about one variable, add up across all of its possible values. Summing a row says "age 9, whatever the height".

**Continuous example.** For the call-centre PDF:

$$f_X(x) = \int_0^{10} \frac{(10-x)\,y + x\,(10-y)}{5000}\,dy = \frac{50\,(10-x) + 50\,x}{5000} = \frac{1}{10}$$

The waiting time on its own is **uniform** on 0 to 10, and by symmetry so is the rating. Yet the two are strongly linked. This is an important lesson:

⚠️ **The marginals do not determine the joint.** If they were independent, the joint would be f_X·f_Y = 1/100, a flat surface. Ours is anything but flat. Two variables can each look completely ordinary on their own and still be tightly connected. You only see the connection in the joint.

My original note said "P(50) is just the sum of the age column of 50". The idea is right. To be precise: the marginal for **height 50** is the sum over **all ages** in the height-50 column.

---

## 4. Conditional distributions

The **conditional** distribution of Y given X = x is the distribution of Y **among only those cases where X = x**:

$$P(y \mid x) = \frac{P(x, y)}{P(x)} \qquad\qquad f(y \mid x) = \frac{f(x, y)}{f_X(x)}$$

> **In plain English:** take the slice of the joint where X = x (one row of the table, or one cut through the surface). It has the right *shape* but doesn't add up to 1, so divide by its total, which is exactly the marginal P(x).

![Conditional slices](diagrams/39-conditional-slices.svg)

### Discrete: height given age 9

The age-9 row is (0, 1/10, 3/10), and its total is P(age 9) = 4/10. Divide by it:

| height | 45 in | 47 in | 49 in |
|---|---|---|---|
| P(h \| age 9) | 0 | (1/10)/(4/10) = **1/4** | (3/10)/(4/10) = **3/4** |

These add up to 1 ✓. This matches the tree: 3 of the 4 nine-year-olds are 49 in tall.

### Discrete: age given height 49 (the other direction)

Use the **column** instead. The 49-in column is (0, 2/10, 3/10), with total 5/10:

P(age 7 | 49) = 0, P(age 8 | 49) = **2/5**, P(age 9 | 49) = **3/5**.

Note that P(49 | 9) = 3/4 but P(9 | 49) = 3/5. **Conditioning is not symmetric**, the same lesson as in Bayes' theorem (v1.2). In fact Bayes falls straight out of the table:

$$P(9 \mid 49) = \frac{P(49 \mid 9)\,P(9)}{P(49)} = \frac{0.75 \times 0.4}{0.5} = 0.6 \;✓$$

### Continuous: rating given waiting time

Since f_X(x) = 1/10, the conditional PDF of the rating is

$$f(y \mid x) = \frac{f(x, y)}{1/10} = \frac{(10-x)\,y + x\,(10-y)}{500}$$

- **Wait 0 min:** f(y | 0) = y/50, which rises steeply, so high ratings are likely.
- **Wait 5 min:** f(y | 5) = 1/10, flat, so every rating is equally likely.
- **Wait 10 min:** f(y | 10) = (10 − y)/50, which falls, so low ratings are likely.

Each slice has area 1 ✓. For example, ∫₀¹⁰ y/50 dy = 100/100 = 1.

### Conditional expectation

A conditional distribution has its own mean, written **E[Y | X = x]**:

- **Height:** E[height | age 9] = 47·(1/4) + 49·(3/4) = **48.5 in**, while E[height | age 7] = (45·2 + 47·1)/3 ≈ **45.7 in**.
- **Rating:** E[Y | X = x] = ∫ y·f(y | x) dy = **6.67 − x/3**. So the expected rating is 6.67 with no wait and drops to 3.33 after a 10-minute wait.

> **In plain English:** E[Y | X = x] is the best guess for Y once you know X. Plotting it against x is exactly what **regression** does: predict one variable from another.

**Independence, once more:** X and Y are independent exactly when every conditional slice looks the same, i.e. P(y | x) = P(y) for every x. In the dice grid every row is the same uniform 1/6. In the age grid the rows have different shapes, so the variables are dependent.

---

## 5. In Python

```python
import pandas as pd

df = pd.DataFrame({"age":    [7, 7, 7, 8, 8, 8, 9, 9, 9, 9],
                   "height": [45, 45, 47, 47, 49, 49, 47, 49, 49, 49]})

joint    = pd.crosstab(df.age, df.height, normalize="all")    # P(age, height)
marg_age = joint.sum(axis=1)                                   # P(age): row sums
cond     = pd.crosstab(df.age, df.height, normalize="index")  # P(height | age): each row / its total
```

`normalize="columns"` gives P(age | height) instead.

---

## Formula cheat sheet

| Idea | Discrete | Continuous | Plain English |
|---|---|---|---|
| Joint | P(x, y) | f(x, y) | how likely the pair (x, y) is |
| Valid | P ≥ 0, Σ Σ P = 1 | f ≥ 0, ∬ f = 1 | non-negative, total 1 |
| Probability of a region | Σ of the cells in R | ∬_R f dx dy | add cells / volume under the surface |
| Marginal | P(x) = Σ_y P(x, y) | f_X(x) = ∫ f(x, y) dy | sum or integrate the other variable away |
| Conditional | P(y \| x) = P(x, y)/P(x) | f(y \| x) = f(x, y)/f_X(x) | take a slice, rescale it to total 1 |
| Chain rule | P(x, y) = P(x)·P(y \| x) | f(x, y) = f_X(x)·f(y \| x) | always true |
| Independence | P(x, y) = P(x)·P(y) everywhere | f(x, y) = f_X(x)·f_Y(y) | the joint is just the product of the margins |
| Conditional mean | E[Y \| x] = Σ y·P(y \| x) | ∫ y·f(y \| x) dy | best guess for Y once x is known |

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>Q1.</b> What does a single cell P(x, y) of a joint table mean, and what must all the cells add up to?</summary>

The probability that X = x **and** Y = y happen together. All the cells add up to **1**.
</details>

<details>
<summary><b>Q2.</b> Using the age and height table, find P(age 8, height 49) and P(age 7, height 49).</summary>

**2/10** and **0**. No 7-year-old in the data is 49 in tall.
</details>

<details>
<summary><b>Q3.</b> Find P(height 47) from the table. What kind of distribution is this?</summary>

Add the 47-in column: 1/10 + 1/10 + 1/10 = **3/10**. It's a **marginal** probability.
</details>

<details>
<summary><b>Q4.</b> Find P(height | age 7) for all three heights.</summary>

The age-7 row is (2/10, 1/10, 0), with total 3/10. Divide: **(2/3, 1/3, 0)**.
</details>

<details>
<summary><b>Q5.</b> Why is P(49 | 9) = 3/4 but P(9 | 49) = 3/5?</summary>

They divide the same cell (3/10) by **different totals**. P(49 | 9) divides by the age-9 row total (4/10). P(9 | 49) divides by the 49-in column total (5/10). Conditioning picks a different slice each time.
</details>

<details>
<summary><b>Q6.</b> Is P(x, y) = P(x)·P(y | x) only for dependent variables?</summary>

**No.** It is always true (the chain rule). For independent variables P(y | x) = P(y), so it simplifies to P(x)·P(y).
</details>

<details>
<summary><b>Q7.</b> A joint table has P(A, B) = 0.12 and marginals P(A) = 0.3 and P(B) = 0.4. Could A and B be independent?</summary>

0.3 × 0.4 = 0.12, so this cell passes. That's **not enough on its own**: the product rule has to hold in **every** cell. (For two yes/no events, if it holds for one cell, it does hold for the other three, so here yes.)
</details>

<details>
<summary><b>Q8.</b> For a joint PDF, is f(1, 2) = 0.3 a probability? How do you get a probability?</summary>

**No.** It's a density, the height of the surface, and can even be bigger than 1. To get a probability, **integrate f over a region**: the volume under the surface.
</details>

<details>
<summary><b>Q9.</b> Show that the call-centre f(x, y) has a total volume of 1.</summary>

∬ (10 − x)·y dy dx = (∫₀¹⁰ (10 − x) dx)(∫₀¹⁰ y dy) = 50 × 50 = 2500. By symmetry the second term also gives 2500. The total is 5000, and 5000/5000 = **1** ✓.
</details>

<details>
<summary><b>Q10.</b> The waiting time and the rating are each uniform on their own. Does that make them independent?</summary>

**No.** Independence needs f(x, y) = f_X(x)·f_Y(y) = 1/100 everywhere, a flat surface. The real f varies a lot (from 0 to 0.02). The marginals throw away exactly the information about how the two variables are linked.
</details>

<details>
<summary><b>Q11.</b> Using f(y | x) = [(10 − x)·y + x·(10 − y)]/500, find P(Y ≥ 8 | X = 0).</summary>

f(y | 0) = y/50. ∫₈¹⁰ y/50 dy = 18/50 = **0.36**. After no wait, there is a 36% chance of a rating of 8 or more (compared with 20% if ratings were uniform).
</details>

<details>
<summary><b>Q12.</b> Find E[height | age 8] from the table.</summary>

The age-8 row is (0, 1/10, 2/10), so the conditional is (0, 1/3, 2/3). E = 47·(1/3) + 49·(2/3) = **48.33 in**.
</details>

<details>
<summary><b>Q13.</b> How can you tell from a joint table, without computing marginals, that two variables are dependent?</summary>

Look at the **shape of the rows**. If the variables are independent, every row is a scaled copy of the same pattern, so after rescaling every conditional P(y | x) is the same. In the age table, the age-7 row leans towards short heights and the age-9 row towards tall ones, so the variables are dependent. (A 0 in a cell whose row and column are both non-zero is also an immediate giveaway.)
</details>
