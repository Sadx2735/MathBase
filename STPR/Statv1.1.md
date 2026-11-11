# Probability — v1.1

Notes on probability basics: sample spaces, complements, the sum rule, independence, the birthday problem and conditional probability.
**Probability** is a number from 0 to 1 that says how likely something is. 0 means "never", 1 means "always", and 0.5 means "half the time".

---

## 1. Basics

Three words we use all the time:

- An **experiment** is anything with an uncertain result. Examples: flipping a coin, rolling a die, picking a kid at random.
- The **sample space (S)** is the list of **all** possible results. For one coin, S = {H, T} (H = heads, T = tails).
- An **event (A)** is the results we care about. For example, "the coin shows H".

**Symbols:**
- **P(A)** = the probability that event A happens.
- **n(A)** = how many results are in A ("n" for "number").
- **n(S)** = how many results there are in total.

When every result is **equally likely**, probability is just counting:

$$P(A) = \frac{n(A)}{n(S)} = \frac{\text{favourable outcomes}}{\text{total outcomes}}$$

("Favourable" just means "the results we want".)

**Example (school):** a class has 10 kids. 3 play soccer and 7 don't. Pick one kid at random.

- Results we want: 3 (the soccer kids).
- Total results: 10 (all kids).

$$P(\text{soccer}) = \frac{3}{10} = 0.3 = 30\%$$

![Sample space and complement](diagrams/01-sample-space-complement.svg)

> **Simple idea:** if everything is equally likely, probability = (what you want) ÷ (everything that can happen).

### Coins and dice

| Experiment | Sample space size | Event | Probability |
|---|---|---|---|
| 1 coin | 2 (H, T) | H | 1/2 |
| 2 coins | 2 × 2 = 4 (HH, HT, TH, TT) | both the same (HH or TT) | 2/4 = 1/2 |
| 3 coins | 2³ = 8 | HHH | 1/8 |
| 2 dice | 6 × 6 = 36 | (6, 6) | 1/36 |

- Each extra coin **doubles** the sample space (× 2).
- Each extra die **multiplies** it by 6.
- For (6, 6) there is exactly **one** result we want out of 36, so the answer is 1/36.

![Two coin tree](diagrams/02-two-coin-tree.svg)

> **Simple idea:** to count all results of several coins or dice, multiply the counts. 2 coins → 2 × 2 = 4. 2 dice → 6 × 6 = 36.

---

## 2. Complement of an event

The **complement** of A is "A does **not** happen". It means everything in S that is *not* in A.
It is written **Aᶜ** (also A′ or "not A").

Either A happens or it doesn't. Together they make 100%. So:

$$P(A^c) = 1 - P(A)$$

**Example:** P(kid plays soccer) = 3/10.
So P(kid does **not** play soccer) = 1 − 3/10 = **7/10 = 70%**.
Check by counting: 7 of the 10 kids don't play. ✅

> **Simple idea:** "not A" = 1 − "A". If there's a 30% chance of rain, there's a 70% chance of no rain.

**Tip:** when "at least one" is hard to count, count "none" instead and subtract it from 1. The birthday problem (section 5) uses this trick.

---

## 3. Sum of probabilities (OR)

"A or B" means A happens, or B happens, or both.

### 3a. Disjoint (mutually exclusive) events

**Disjoint** (also called **mutually exclusive**) means A and B **can't happen together**. Then just add:

$$P(A \text{ or } B) = P(A) + P(B)$$

**Examples:**
- Each kid plays **only one** sport: soccer 30%, basketball 40%.
  - P(plays a sport) = 0.3 + 0.4 = **0.7**.
  - So **30% play neither** sport (1 − 0.7 = 0.3).
- Roll a die: P(even **or** 5) = 3/6 + 1/6 = 4/6 = **2/3**.
  - Even = {2, 4, 6} → 3 results. 5 → 1 result.
  - A 5 is never even, so the events are disjoint and we can simply add.

### 3b. Overlapping (joint) events

Now suppose some kids play **both** sports. If we add P(A) + P(B), those kids get counted **twice**. So we take the overlap away once:

$$P(A \text{ or } B) = P(A) + P(B) - P(A \text{ and } B)$$

**Example:** soccer 30%, basketball 40%, both 10%.

P(soccer or basketball) = 0.3 + 0.4 − 0.1 = **0.6**.

![Sum rule Venn diagrams](diagrams/03-sum-rule-venn.svg)

> **Simple idea:** for "or", add the two chances. If they can overlap, subtract the overlap once so nobody is counted twice.

Disjoint is just the special case where P(A and B) = 0. So the general formula always works.

---

## 4. Independence (AND)

Two events are **independent** if knowing that one happened does **not** change the chance of the other.

- ✅ **Independent:** the 1st coin flip and the 2nd coin flip. The coin has no memory.
- ❌ **Dependent:** whether a student is absent and whether it's raining. Rain changes how many students are absent.

For independent events, multiply:

$$P(A \text{ and } B) = P(A) \cdot P(B)$$

(The dot · means "times".)

**Example:** 100 kids. 50% play soccer. The kids are split into two rooms *at random*: 30% go to room 1 and 70% to room 2. Because the split is random, the room has nothing to do with soccer, so the two are independent.

- P(soccer **and** room 1) = 0.5 × 0.3 = 0.15 → 0.15 × 100 = **15 kids**
- P(soccer **and** room 2) = 0.5 × 0.7 = 0.35 → 0.35 × 100 = **35 kids**
- Check: 15 + 35 = 50 soccer kids. ✅

![Independence as an area model](diagrams/04-independence-area.svg)

**More examples:**

- 5 heads in 5 flips: 1/2 × 1/2 × 1/2 × 1/2 × 1/2 = (1/2)⁵ = **1/32**
- (6, 6) on two dice: 1/6 × 1/6 = **1/36**. This is the same as counting 1 cell out of 36 in the 6×6 grid.

> **Simple idea:** for "and" with independent events, multiply the chances. Half of half is a quarter.

---

## 5. Birthday problem

> In a group of 30 people, what is the chance that **at least two** have the same birthday? (Ignore leap years. Assume all 365 days are equally likely.)

Counting "at least two share" directly is messy. There are too many ways it can happen. So use the **complement** (section 2): count the chance that **everyone has a different** birthday, then subtract from 1.

Go through the people one by one:

- Person 1: any day is fine → 365/365
- Person 2: must avoid 1 taken day → 364/365
- Person 3: must avoid 2 taken days → 363/365
- …
- Person 30: must avoid 29 taken days → 336/365

All of these must happen, so multiply them:

$$P(\text{all different}) = \frac{365}{365} \cdot \frac{364}{365} \cdot \frac{363}{365} \cdots \frac{336}{365} \approx 0.294$$

$$P(\text{at least two share}) = 1 - 0.294 \approx \mathbf{0.706 \; (70.6\%)}$$

![Birthday curve](diagrams/05-birthday-curve.svg)

**Surprising fact:** only **23 people** are needed for a better-than-50% chance.
Why so few? Because what matters is the number of **pairs** of people, not the number of people.
- With n people there are n(n − 1)/2 pairs.
- For 23 people: 23 × 22 / 2 = **253 pairs**. That's a lot of chances for a match.

> **Simple idea:** "at least one match" is hard to count, so count "no match at all" and do 1 minus that. Many people means many pairs, and many pairs makes a match likely.

---

## 6. Conditional probability

**P(B | A)** means "the chance of B, **given that** A has already happened". The bar | is read as "given".

**Everyday example:** the chance that it's raining is maybe 10%. But the chance that it's raining **given that** people outside are holding umbrellas is much higher. Knowing something changes the chance.

Knowing A happened **shrinks the sample space** down to just A. Then you ask: what share of A is also B?

$$P(B \mid A) = \frac{P(A \text{ and } B)}{P(A)}$$

Move P(A) to the other side and you get the **multiplication rule** (also called the **chain rule**):

$$P(A \text{ and } B) = P(A) \cdot P(B \mid A)$$

If A and B are independent, knowing A changes nothing, so P(B | A) = P(B). Then the rule becomes P(A and B) = P(A) · P(B), the same as section 4.

> **Simple idea:** "given A" means "only look inside A". Then count what share of A is also B.

### Example: two dice (sum is 10, first die is 6)

![Dice grid conditional](diagrams/06-dice-conditional.svg)

- **A** = first die is 6 → 6 cells (one full row), so P(A) = 6/36 = 1/6
- **B** = sum is 10 → 3 cells: (4,6), (5,5), (6,4)
- **A and B** = only (6,4) → P(A and B) = **1/36**

**Given A**, we only look at the 6 cells in the row. Just 1 of them sums to 10, so P(B | A) = 1/6.

Check with the chain rule: P(A and B) = P(A) · P(B | A) = 1/6 · 1/6 = **1/36** ✅

**The direction matters:**

| Question | Look inside… | Count | Answer |
|---|---|---|---|
| P(sum 10 \| first is 6) | the 6 cells where first = 6 | 1 sums to 10 | **1/6** |
| P(first is 6 \| sum 10) | the 3 cells that sum to 10 | 1 has first = 6 | **1/3** |

### Example: coins (be careful with the condition)

Flip 2 coins. All results: HH, HT, TH, TT.

- P(HH | **first** flip is H) = P(HH) / P(first H) = (1/4) / (1/2) = **1/2**
  - Inside "first is H" we have {HH, HT}. 1 of 2 is HH.
- P(HH | **at least one** H) = P(HH) / P(at least one H) = (1/4) / (3/4) = **1/3**
  - Inside "at least one H" we have {HH, HT, TH}. 1 of 3 is HH.

These are two different conditions, so they give two different answers.

### "Isn't this circular?"

It *looks* circular. To get P(A and B) you need P(B | A), and to get P(B | A) you need P(A and B).
But in real problems you always know **one** of them already. You use the formula to find the other:

| You know… | You use… | You get… |
|---|---|---|
| the step-by-step story (draw 1, then draw 2) | P(A) · P(B \| A) | P(A and B) |
| counts or areas of overlap (a grid or a table) | P(A and B) / P(A) | P(B \| A) |

In card problems, P(B | A) comes straight from the story. For example: "after one Ace is gone, 3 of the 51 cards left are Aces". So you don't need P(A and B) first.

> **Simple idea:** it's not circular. You start from whichever piece you already know.

### Example: two Aces from a 52-card deck

A normal deck has 52 cards, and 4 of them are Aces.

![Aces tree](diagrams/07-aces-tree.svg)

**With replacement** (put the first card back, so the second draw doesn't care about the first, i.e. independent):

$$P(\text{both Aces}) = \frac{4}{52} \cdot \frac{4}{52} = \frac{1}{169} \approx 0.0059$$

(4/52 = 1/13, and 1/13 × 1/13 = 1/169.)

**Without replacement** (keep the first card out, so the second draw depends on the first, i.e. dependent):

$$P(\text{both Aces}) = \frac{4}{52} \cdot \frac{3}{51} = \frac{12}{2652} = \frac{1}{221} \approx 0.0045$$

The second fraction is **3/51**, not 3/52. One card (an Ace) is already gone, so 51 cards are left and only 3 of them are Aces.

> **Simple idea:** if you don't put the card back, the deck changes. Fewer cards, fewer Aces. Update both numbers.

**Why is P(first is Ace | both are Aces) = 1?**

If both cards are Aces, the first card is certainly an Ace. Probability 1 means "certain". Using the formula:

$$P(\text{1st Ace} \mid \text{both Aces}) = \frac{P(\text{1st Ace and both Aces})}{P(\text{both Aces})} = \frac{P(\text{both Aces})}{P(\text{both Aces})} = 1$$

"Both Aces" already sits **inside** "first is Ace". So the overlap of the two is just "both Aces", and anything divided by itself is 1.

---

## Formula cheat sheet

Symbols: ∪ means "or", ∩ means "and", | means "given".

| Idea | Formula | Simple meaning |
|---|---|---|
| Classical probability | P(A) = n(A) / n(S) | what you want ÷ everything possible |
| Complement | P(Aᶜ) = 1 − P(A) | "not A" = 1 minus "A" |
| OR, disjoint | P(A ∪ B) = P(A) + P(B) | can't happen together → just add |
| OR, general | P(A ∪ B) = P(A) + P(B) − P(A ∩ B) | add, then remove the double-counted overlap |
| AND, independent | P(A ∩ B) = P(A) · P(B) | no effect on each other → multiply |
| AND, general (chain rule) | P(A ∩ B) = P(A) · P(B \| A) | chance of A, times chance of B once A has happened |
| Conditional | P(B \| A) = P(A ∩ B) / P(A) | look only inside A; what share is also B? |
| Independence test | P(B \| A) = P(B) | knowing A doesn't change B's chance |

---

## Revision questions

Try each question first, then expand it to check your answer.

<details>
<summary><b>W1.</b> (Warm-up) You roll one die. What is P(rolling a 3)?</summary>

1 result we want out of 6 possible, so **1/6**.
</details>

<details>
<summary><b>W2.</b> (Warm-up) The chance of rain tomorrow is 0.25. What is the chance of no rain?</summary>

Use the complement: 1 − 0.25 = **0.75**.
</details>

<details>
<summary><b>W3.</b> (Warm-up) You flip a coin twice. What is P(two tails)?</summary>

The flips are independent, so multiply: 1/2 × 1/2 = **1/4**. (Or count: TT is 1 of the 4 results HH, HT, TH, TT.)
</details>

<details>
<summary><b>Q1.</b> A bag has 5 red, 3 blue and 2 green balls. What is P(not red)?</summary>

Total = 5 + 3 + 2 = 10. P(red) = 5/10 = 1/2, so P(not red) = 1 − 1/2 = **1/2**.
</details>

<details>
<summary><b>Q2.</b> You flip 3 coins. What is P(exactly 2 heads)?</summary>

There are 2 × 2 × 2 = 8 results. Exactly 2 heads: HHT, HTH, THH = 3 results, so **3/8**.
</details>

<details>
<summary><b>Q3.</b> You flip 3 coins. What is P(at least one head)?</summary>

Use the complement. P(no heads) = P(TTT) = 1/8, so the answer is 1 − 1/8 = **7/8**.
</details>

<details>
<summary><b>Q4.</b> You roll one die. What is P(even or greater than 3)?</summary>

Even = {2,4,6}. Greater than 3 = {4,5,6}. Both = {4,6}.
These overlap, so subtract the overlap: 3/6 + 3/6 − 2/6 = **4/6 = 2/3**.
Check by listing: {2,4,5,6} = 4 results out of 6 ✅
</details>

<details>
<summary><b>Q5.</b> In a class, 60% like maths, 50% like physics and 30% like both. What percentage like neither?</summary>

P(maths or physics) = 0.6 + 0.5 − 0.3 = 0.8. So P(neither) = 1 − 0.8 = **0.2 = 20%**.
</details>

<details>
<summary><b>Q6.</b> Are "first die is 6" and "sum is 7" independent?</summary>

- P(first is 6) = 6/36 = 1/6.
- P(sum is 7) = 6/36 = 1/6.
- P(both) = P((6,1)) = 1/36, and 1/6 · 1/6 = 1/36 too.

They match, so **yes, they are independent.** (Compare: "first is 6" and "sum is 10" are *not* independent, because 1/36 ≠ 1/6 · 3/36.)
</details>

<details>
<summary><b>Q7.</b> What is P(no 6 in 4 rolls of a die)? And P(at least one 6)?</summary>

Each roll has a 5/6 chance of not being a 6, and the rolls are independent.
P(no 6) = (5/6)⁴ = 625/1296 ≈ **0.482**. P(at least one 6) = 1 − 0.482 ≈ **0.518**.
</details>

<details>
<summary><b>Q8.</b> You draw 2 cards without putting the first back. What is P(both are hearts)?</summary>

There are 13 hearts in 52 cards. After one heart is gone: 12 hearts in 51 cards.
13/52 · 12/51 = 156/2652 = **1/17 ≈ 0.059**.
</details>

<details>
<summary><b>Q9.</b> The same question as Q8, but you <i>do</i> put the first card back.</summary>

13/52 · 13/52 = (1/4)² = **1/16 = 0.0625**. It is slightly higher, because the heart you drew goes back into the deck.
</details>

<details>
<summary><b>Q10.</b> Two dice are rolled and the sum is 8. What is P(both dice show the same number)?</summary>

Sum 8: (2,6), (3,5), (4,4), (5,3), (6,2) = 5 results. Doubles among these: only (4,4) = 1 result.
**P = 1/5**. "Given sum 8" means: only look at those 5 cells and count again.
</details>

<details>
<summary><b>Q11.</b> 70% of days are dry. On dry days 5% of students are absent, and on rainy days 20% are absent. What is P(a student is absent)?</summary>

Work out each kind of day, then add:
- Dry: 0.7 × 0.05 = 0.035
- Rainy: 0.3 × 0.20 = 0.06

Total: 0.035 + 0.06 = **0.095**.
(This is the "absence depends on rain" example. The events are dependent, so for each branch use P(weather) · P(absent | weather).)
</details>

<details>
<summary><b>Q12.</b> In a group of 5 people, what is P(at least two share a birthday)?</summary>

P(all different) = 365/365 · 364/365 · 363/365 · 362/365 · 361/365 ≈ 0.973. So the answer is 1 − 0.973 ≈ **0.027 (2.7%)**.
</details>

<details>
<summary><b>Q13.</b> A family has 2 children and at least one is a boy. What is P(both are boys)?</summary>

All results: {BB, BG, GB, GG} (B = boy, G = girl). "At least one boy" leaves {BB, BG, GB}. Only 1 of these 3 is BB, so **1/3**.
(This is the same idea as the coin example: P(HH | at least one H) = 1/3.)
</details>

<details>
<summary><b>Q14.</b> True or false: if A and B are disjoint (can't happen together, and both have P > 0), then they are independent.</summary>

**False.** Disjoint means P(A and B) = 0, but P(A) · P(B) is bigger than 0, so the independence rule fails. If A happens, B *cannot* happen. Knowing A tells you everything about B, which is the strongest kind of dependence.
</details>
