#### Random Variable

flip a coin you can get a value of H or T.
x = No of heads ie can be 1 or 0.
p(x=0) = 0.5 , p(x=1) = 0.5.

x=Number of heads in 10 coin toses x can be 0 to 10.
10 => (0.5)^10.

x=Number of 9 heads in 10 tosses is 
9 => 0.5 tails * 0.5^9 heads

but we need to account for the different synarious for example
HT TH is same so (0.5)^2 for getting tails and head + (0.5)^2 for getting heads and tails

there are discrete and continuous random variable:
1. Countable
2. Infinite 1.222

#### Probability distribution

flip three coins

1. HHH - (1)
2. HHT , HTH, THH - (3)
3. HTT, THT, TTH - (3)
4. TTT - (1)

no of heads , probability for getting heads.
- this can be represented with PMF : p(xn)=x , x=0,1,2,3,4,5
- summing to pmf is 1.

formula : Binomial distribution
tossing a coin 5 times to get 2 heads
HHTTT
HTHTT
.. 10 ways (1/32)

number of ways you can arrange them 5! total possible ways.
now we will filter out the ones with that probability
5!/(2!*(5-2)!)) as they are same. (5-2!) for tails.

let p(h)=p 
Event X = x , x heads in 5 tosses.
(5 x) p^x ( 1 - p)^5-x , x=0,1,2,3,4,5

why we multiply because if the probability of getting heads is 1 then
we can leave it as it is but its 0.5 so we need to multiply with that
number.

```
(n x)*(p^x)*(1-p)^(n-x)
```

binomial coefficient lets say we have 3 coins
123 => 123,132,213,231,312,321
but what if the value are same.
122 => 122,122,212,221,212,221 (3) unique (3!/2!)
unluckily here we have 1's as a single number but if we include that
then we need to account for that as well so it becomes 3!/2!*(3-2!)

#### Bernoulli distribution

A Bernoulli distribution describes one single trial with only two outcomes: success or failure.
You have already used it. One coin toss is a Bernoulli trial.

### Continuous distribution

what if the numbers are continuous , then the probability of event to occur at that particular
instant is going to be 0. what is the prob of call to be exactly at 1.000 its 0.

Area is the probability with range limits and area under the curve is going to be 1.

### PDF : prob density function.

- fx needs to be defined for all numbers .
- fx(x)>=0. area under curve is 1.

• A Probability Mass Function (PMF) is used for discrete random variables to give the exact 
probability of specific, distinct values (like rolling a 4 on a die).

• A Probability Density Function (PDF) is used for continuous random variables to represent probability per unit of length/space,
where the probability of any single exact point is zero, and you must integrate over an interval to 
find probability (the area under the curve).

- mass over volume is density this word over is what gives a Continous curve

### Uniform distribution

1/(b-a) lets say we have a call for 10mins what is the prob to cut the call?
the probability to be so now every point has equal.

cdf is a straight line since the derivative is the same.

### Normal distribution

formula :  1/sqrt(2*pi*std) * e ^ -0.5 * ((x-mu)^2)/2
Standard normal distribution.

Conversion normal to standard normal.
for mu=2 std of 2.5. (x-2)/2.5

so to compare if both the distribution are same we can do the standardization then check if they are same?
@claude answer me with were it is used.

Height, IQ , Weight..

### Sampling

creating a distribution for the given data.
and then sampling from equation spaces.

for cdf in gaussian we cant be taking it exactly correctly so what we need to do is to 
calculate the CDF and then sample along the cols and get the x values were they intersect on that
particular curve.

@claude add diagrams refer to the other readme and also add the use cases example and questions at
the end to verify if i understand the concepts correctly.
