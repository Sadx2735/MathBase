# Probability — v1.2: Bayes' Theorem

Builds on [v1.1](Statv1.1.md) (conditional probability and the multiplication rule).
Bayes' theorem answers one question: **"I've just seen some new evidence. How much should I change what I believe?"**

**Everyday example:** you hear a car alarm outside. Is someone stealing a car? Probably not. Car alarms go off all the time for no reason, and thefts are rare. Bayes' theorem is the maths behind that common-sense feeling.

**Symbols used in this part:**
- **P(A)** = the probability (chance) that A happens, a number from 0 to 1.
- **P(A | B)** = the chance of A **given** B. Read the bar "|" as "given that" or "if we know". Example: P(sick | +) = the chance you are sick, given that your test was positive.
- **A ∩ B** = "A **and** B both happen".
- **Aᶜ** (or A') = "A does **not** happen" (the complement). Example: if A = sick, then Aᶜ = healthy.

---

## 1. The starting example: a rare disease and a good test

- 1 in 10,000 people have the disease.
- The test is **99% accurate**:
  - it says "sick" for 99% of sick people,
  - it says "healthy" for 99% of healthy people.
- You test **positive** (+). What is the chance you are really sick?

Most people guess 99%. Let's **count** instead. Imagine a group of 1,000,000 people:

- Sick: 1,000,000 ÷ 10,000 = **100** people.
  - Test + : 99% of 100 = **99**
  - Test − : 1% of 100 = **1**
- Healthy: 1,000,000 − 100 = **999,900** people.
  - Test + : 1% of 999,900 = **9,999** (false alarms)
  - Test − : 99% of 999,900 = **989,901**

![Disease frequency tree](diagrams/08-bayes-disease-tree.svg)

| | Test + | Test − | Total |
|---|---|---|---|
| **Sick** | 99 | 1 | 100 |
| **Healthy** | 9,999 | 989,901 | 999,900 |
| **Total** | **10,098** | 989,902 | 1,000,000 |

You tested positive, so you are somewhere in the **Test +** column. Only look at that column:

- Positive people in total: 99 + 9,999 = 10,098
- Of those, really sick: 99

$$P(\text{sick} \mid +) = \frac{99}{99 + 9{,}999} = \frac{99}{10{,}098} \approx 0.0098 \approx \mathbf{1\%}$$

> **Simple idea:** the test is good, but the disease is very rare. There are so many healthy people that even 1% of them (9,999) is far more than 99% of the few sick people (99). So most positive results are false alarms.

---

## 2. Building the formula

The multiplication rule from v1.1 says "A and B" can be worked out starting from **either** event:

$$P(A \cap B) = P(A) \cdot P(B \mid A) \qquad\text{and}\qquad P(A \cap B) = P(B) \cdot P(A \mid B)$$

> **Simple idea:** "A and B both happen" is the same as "A happens, then B happens given A". It is also the same as "B happens, then A happens given B". Same overlap, two ways to get there.

Both right-hand sides equal the same thing. So set them equal, then divide both sides by P(B):

- P(B) · P(A | B) = P(A) · P(B | A)
- P(A | B) = P(A) · P(B | A) / P(B)

$$\boxed{P(A \mid B) = \frac{P(A) \cdot P(B \mid A)}{P(B)}}$$

> **Simple idea:** Bayes' theorem turns a probability around. You know "B given A" and you want "A given B". Multiply by P(A), then divide by P(B).

![Anatomy of Bayes' formula](diagrams/09-bayes-formula-anatomy.svg)

Each piece has a name:

| Piece | Name | Simple meaning | Disease example |
|---|---|---|---|
| P(A) | **Prior** | what you believed *before* the evidence | P(sick) = 0.0001 |
| P(B \| A) | **Likelihood** | how often you'd see the evidence *if A is true* | P(+ \| sick) = 0.99 |
| P(B) | **Evidence** | how often you see the evidence *at all* | P(+) = 0.010098 |
| P(A \| B) | **Posterior** | what you believe *after* the evidence | P(sick \| +) ≈ 0.0098 |

("Prior" = before, "posterior" = after.)

### Where does P(B) come from? (Law of total probability)

A positive test can happen in two ways:
- the person is sick **and** tests positive, **or**
- the person is healthy **and** tests positive.

These two can't both be true at once, so we just add them:

$$P(B) = P(A) \cdot P(B \mid A) + P(A^c) \cdot P(B \mid A^c)$$

> **Simple idea:** total positive tests = (sick people who test positive) + (healthy people who test positive).

**Disease example, step by step:**
- Sick and positive: 0.0001 × 0.99 = 0.000099
- Healthy and positive: 0.9999 × 0.01 = 0.009999
- P(+) = 0.000099 + 0.009999 = **0.010098**

(Check with the table: 10,098 out of 1,000,000 = 0.010098. ✓)

⚠️ **Correction to my original notes.** I wrote `P(B) = P(B)*P(B|A) + P(A')*P(B|A')`. The first part must be **P(A)** · P(B|A), not P(B) · P(B|A). Each part is always "chance of the situation × chance of the evidence in that situation".

### The full working form

Put P(B) into the bottom of Bayes' formula:

$$P(A \mid B) = \frac{P(A)\,P(B \mid A)}{P(A)\,P(B \mid A) + P(A^c)\,P(B \mid A^c)} = \frac{\text{term}_1}{\text{term}_1 + \text{term}_2}$$

- **term₁** = the "true positives" (A is true and we see the evidence).
- **term₂** = the "false alarms" (A is false but we still see the evidence).

> **Simple idea:** the answer is just "what share of all the evidence came from A". Real hits divided by (real hits + false alarms).

---

## 3. "Why do we need Bayes if we can just count?"

Good question. **Counting *is* Bayes' theorem.** The 1,000,000-people table is the same formula, just using whole numbers instead of decimals:

| Counting step | Formula piece |
|---|---|
| 1,000,000 × 1/10,000 = 100 sick | P(A) |
| 100 × 99% = 99 sick and positive | P(A) · P(B \| A) = term₁ |
| 999,900 × 1% = 9,999 healthy and positive | P(Aᶜ) · P(B \| Aᶜ) = term₂ |
| 99 / (99 + 9,999) | term₁ / (term₁ + term₂) |

So why learn the formula at all?

1. **The facts usually come "the wrong way round".** Labs tell you P(positive | sick), which is how good the test is. Patients want P(sick | positive). You can't count that directly unless someone gives you the full table. Bayes builds the table for you from the prior and the likelihoods.
2. **Counting needs a nice round group size.** With ugly numbers like 0.0000137, picking "an imaginary 1,000,000 people" gets messy. The formula works with any numbers.
3. **Updating step by step.** When new evidence arrives (a second test, a second word in an email), today's answer (posterior) becomes tomorrow's starting belief (prior). See section 5.
4. **Computers need it.** Spam filters and machine-learning classifiers combine thousands of clues. You can't draw a tree that big, but the formula handles it easily.

> **Rule of thumb:**
> - On paper: use the counting table. It's easy to follow and hard to get wrong.
> - Use the formula when the numbers are ugly, when there are many clues, or when you're writing code.

---

## 4. Example: a spam filter with one word

An email contains the word **"lottery"**. Is it spam?

| Given | Value | Simple meaning |
|---|---|---|
| P(spam) | 0.2 | 20% of all emails are spam (**prior**) |
| P(not spam) | 0.8 | 80% are normal |
| P(lottery \| spam) | 0.7 | 70% of spam emails contain "lottery" |
| P(lottery \| not spam) | 0.125 | 12.5% of normal emails contain "lottery" |

**Step 1: true hits (term₁).**

$$\text{term}_1 = P(\text{spam}) \cdot P(\text{lottery} \mid \text{spam}) = 0.2 \times 0.7 = 0.14$$

> So 14% of all emails are spam that contains "lottery".

**Step 2: false alarms (term₂).**

$$\text{term}_2 = P(\text{not spam}) \cdot P(\text{lottery} \mid \text{not spam}) = 0.8 \times 0.125 = 0.10$$

> So 10% of all emails are normal emails that contain "lottery".

**Step 3: share that is spam.**

$$P(\text{spam} \mid \text{lottery}) = \frac{0.14}{0.14 + 0.10} = \frac{0.14}{0.24} \approx \mathbf{0.583}$$

> **Simple idea:** "lottery" shows up in 24% of all emails. Out of those, 14 parts in 24 are spam. Seeing the word moved our belief from **20% → 58%**.

Counting version: out of 100 emails, 14 are spam with "lottery" and 10 are normal with "lottery". So 14 out of 24 ≈ 58%.

**Prior → Evidence → Posterior**

| Prior (before) | Evidence (what we saw) | Posterior (after) |
|---|---|---|
| P(spam) = 0.2 | the email contains "lottery" | P(spam \| lottery) ≈ 0.583 |

The posterior only looks at a *smaller group*: the emails that contain "lottery". Just like in section 1, where we only looked at the Test + column.

---

## 5. Two words: "lottery" and "won" (Naive Bayes)

Now the email contains **both** "lottery" and "won". We need two more facts:

| Given | Value | Simple meaning |
|---|---|---|
| P(won \| spam) | 0.5 | half of spam emails contain "won" |
| P(won \| not spam) | 0.05 | 5% of normal emails contain "won" |

**The assumption (this is the "naive" part):** inside each group (spam, or not spam), words show up **independently**. "Independent" means one word doesn't change the chance of the other.

$$P(\text{lottery} \cap \text{won} \mid \text{spam}) = P(\text{lottery} \mid \text{spam}) \cdot P(\text{won} \mid \text{spam})$$

> **Simple idea:** if we already know an email is spam, seeing "lottery" doesn't change the chance it also says "won". That's not quite true in real life, but it works surprisingly well and keeps the maths simple.

So each term just gets one more number multiplied on:

**term₁ (spam):** 0.2 × 0.7 × 0.5 = 0.14 × 0.5 = **0.07**

$$\text{term}_1 = P(\text{spam}) \cdot P(\text{lottery} \mid \text{spam}) \cdot P(\text{won} \mid \text{spam}) = 0.2 \times 0.7 \times 0.5 = 0.07$$

**term₂ (not spam):** 0.8 × 0.125 × 0.05 = 0.10 × 0.05 = **0.005**

$$\text{term}_2 = P(\text{not spam}) \cdot P(\text{lottery} \mid \text{not spam}) \cdot P(\text{won} \mid \text{not spam}) = 0.8 \times 0.125 \times 0.05 = 0.005$$

**Answer:**

$$P(\text{spam} \mid \text{lottery} \cap \text{won}) = \frac{0.07}{0.07 + 0.005} = \frac{0.07}{0.075} \approx \mathbf{0.933}$$

> **Simple idea:** each word is a new clue. "Lottery" pushed spam from 20% to 58%. "Won" pushed it further, to **93%**.

![Spam updating pipeline](diagrams/10-spam-update-pipeline.svg)

**Same answer, two ways.** You can use both words at once (above). Or you can go one word at a time: take the answer after "lottery" (0.583) and use it as the new prior. Then not-spam = 1 − 0.583 = 0.417.

- term₁ = 0.583 × 0.5 ≈ 0.2917
- term₂ = 0.417 × 0.05 ≈ 0.0208

$$\frac{0.583 \times 0.5}{0.583 \times 0.5 + 0.417 \times 0.05} = \frac{0.2917}{0.2917 + 0.0208} \approx 0.933 \;\checkmark$$

**General pattern for n words** (w₁, …, wₙ are the words; **∏** (capital pi) means "multiply all of these together", like Σ means "add them all"):

$$P(\text{spam} \mid w_1, \dots, w_n) = \frac{P(\text{spam}) \prod_i P(w_i \mid \text{spam})}{P(\text{spam}) \prod_i P(w_i \mid \text{spam}) + P(\text{not spam}) \prod_i P(w_i \mid \text{not spam})}$$

> **Simple idea:** for each group, multiply the starting chance by the chance of every word. Then ask what share of the total belongs to spam.

---

## Formula cheat sheet

| Idea | Formula | Simple meaning |
|---|---|---|
| Multiplication rule | P(A ∩ B) = P(A)·P(B\|A) = P(B)·P(A\|B) | the same overlap, reached from either side |
| Total probability | P(B) = P(A)·P(B\|A) + P(Aᶜ)·P(B\|Aᶜ) | add up every way B can happen |
| Bayes' theorem | P(A\|B) = P(A)·P(B\|A) / P(B) | turn the condition around |
| Working form | term₁ / (term₁ + term₂) | real hits ÷ (real hits + false alarms) |
| Naive Bayes | multiply the likelihoods of each clue | assumes clues are independent inside each group |

**Common traps**

- **P(A | B) is not the same as P(B | A).** "Most spam contains 'lottery'" is not the same as "most 'lottery' emails are spam".
- **Forgetting the prior (base-rate fallacy).** "Base rate" = how common something is to begin with. A 99% test for a 1-in-10,000 disease still gives only about 1% P(sick | +).
- In P(B), each part is **chance of the situation × likelihood**. Never P(B) × something.

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>W1.</b> In words, what is the difference between the prior and the posterior?</summary>

The **prior** is what you believe *before* seeing the evidence. The **posterior** is what you believe *after* seeing it. Example: P(spam) = 0.2 before, P(spam | lottery) ≈ 0.583 after.
</details>

<details>
<summary><b>W2.</b> In the disease example, why is P(sick | +) so low even though the test is 99% accurate?</summary>

Because the disease is so rare. Healthy people far outnumber sick people, so 1% of the healthy (9,999 false alarms) is much bigger than 99% of the sick (99 real cases). 99/10,098 ≈ 1%.
</details>

<details>
<summary><b>Q1.</b> A disease affects 1% of people. A test catches 90% of sick people. It wrongly says "positive" for 5% of healthy people. You test positive. What is P(sick)?</summary>

term₁ = 0.01 × 0.90 = 0.009 · term₂ = 0.99 × 0.05 = 0.0495
P = 0.009 / (0.009 + 0.0495) = 0.009 / 0.0585 ≈ **0.154 (15.4%)**
Counting version: out of 10,000 people, 90 are real positives and 495 are false alarms, so 90/585.
</details>

<details>
<summary><b>Q2.</b> In Q1, you take a second, independent test and it is <i>also</i> positive. What is P(sick) now?</summary>

Use 0.154 as the new prior (so healthy = 0.846).
term₁ = 0.154 × 0.90 = 0.1385 · term₂ = 0.846 × 0.05 = 0.0423
P ≈ 0.1385 / 0.1808 ≈ **0.766 (≈ 77%)**. A second positive is much more convincing than the first.
</details>

<details>
<summary><b>Q3.</b> For the disease in section 1 (1 in 10,000, 99% accurate), what is P(healthy | test −)?</summary>

Look at the Test − column: 1 sick and 989,901 healthy.
P = 989,901 / 989,902 ≈ **0.999999**. A negative result is extremely reassuring.
</details>

<details>
<summary><b>Q4.</b> Box 1 has 3 red balls and 1 blue. Box 2 has 1 red and 3 blue. You pick a box at random and draw a red ball. What is P(it came from Box 1)?</summary>

term₁ = 0.5 × 3/4 = 0.375 · term₂ = 0.5 × 1/4 = 0.125
P = 0.375 / (0.375 + 0.125) = 0.375 / 0.5 = **0.75**
</details>

<details>
<summary><b>Q5.</b> 30% of emails are spam. "Free" appears in 60% of spam and 10% of normal emails. What is P(spam | "free")?</summary>

term₁ = 0.3 × 0.6 = 0.18 · term₂ = 0.7 × 0.1 = 0.07
P = 0.18 / 0.25 = **0.72**
</details>

<details>
<summary><b>Q6.</b> Carrying on from Q5: "click" appears in 40% of spam and 5% of normal emails. An email has both "free" and "click". What is P(spam)? (Assume the words are independent inside each group.)</summary>

term₁ = 0.3 × 0.6 × 0.4 = 0.072 · term₂ = 0.7 × 0.1 × 0.05 = 0.0035
P = 0.072 / 0.0755 ≈ **0.954**
</details>

<details>
<summary><b>Q7.</b> A factory has two machines. Machine A makes 60% of items, and 2% of its items are faulty. Machine B makes 40%, and 5% of its items are faulty. A random item is faulty. What is P(it came from B)?</summary>

term_B = 0.4 × 0.05 = 0.02 · term_A = 0.6 × 0.02 = 0.012
P(B | faulty) = 0.02 / (0.02 + 0.012) = 0.02 / 0.032 = **0.625**
B makes fewer items but more of the faulty ones.
</details>

<details>
<summary><b>Q8.</b> It rains on 20% of days. On rainy days, the forecast had said "rain" 80% of the time. On dry days, it had said "rain" 10% of the time. Today the forecast says rain. What is P(rain)?</summary>

term₁ = 0.2 × 0.8 = 0.16 · term₂ = 0.8 × 0.1 = 0.08
P = 0.16 / 0.24 = **2/3 ≈ 0.667**
</details>

<details>
<summary><b>Q9.</b> True or false: if P(B | A) = 0.99, then P(A | B) must be high.</summary>

**False.** It also depends on the prior P(A) and on P(B | Aᶜ). In the disease example P(+ | sick) = 0.99, but P(sick | +) ≈ 0.01.
</details>

<details>
<summary><b>Q10.</b> In a spam filter, what happens to the posterior if a word appears <b>equally often</b> in spam and in normal emails?</summary>

The word's likelihood is the same number in both terms, so it cancels out. The posterior **stays equal to the prior**. The word tells us nothing.
</details>

<details>
<summary><b>Q11.</b> Why is Naive Bayes called "naive"? Give an example where its assumption fails.</summary>

It assumes words are independent *inside each group*. In real life, words like "lottery" and "won", or "New" and "York", often appear together. Multiplying their chances then counts the same clue twice.
</details>

<details>
<summary><b>Q12.</b> Write P(A | B) using only P(A), P(B | A) and P(B | Aᶜ).</summary>

$$P(A \mid B) = \frac{P(A)\,P(B \mid A)}{P(A)\,P(B \mid A) + (1 - P(A))\,P(B \mid A^c)}$$
</details>
