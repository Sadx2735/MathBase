# Probability - v1.2
content from image

![](diagram/bayes.png)

now to find out sick given that the diagonosed sick
so its 99/(9999+99).

Both ways of writing the multiplication rule are absolutely correct, but the specific formula you wrote has the conditional terms flipped.
The correct mathematical identity allows you to calculate the joint probability \(P(A \cap B)\) using either \(A\) or \(B\) as the starting condition. The two valid formulas are:
\(P(A\text{\ and\ }B)=P(A)\times P(B\mid A)\)
\(P(A\text{\ and\ }B)=P(B)\times P(A\mid B)\)

So :

P(A&B) = P(B)*P(A/B)
P(A/B) = P(A&B)/P(B)
P(A&B) can be written as P(A)*P(B/A)
P(A/B) = P(A)*P(B/A)/P(B)

or :

P(A&B) = P(B)*P(A/B)
Sick is the total Probability of People who were diagonised sick
and then sick after diagonised sick.

Sick is the total Probability of People who were not diagonised sick
and then sick after 99% accuracy sick. ( healthy )

P(B) = P(B)*P(B|A) + P(A')*P(B|A')

but my doubt is why do we need this if we have the direct way of
calculating stuff ? @claude

P(Spam) = 0.2
P(not Spam) = 0.8
P(lot | spam) = 0.7
P(lot | not spam) = 0.125
term1 => ( P(lot | spam) = 0.7 * P(Spam) = 0.2 ) 
term2 => ( P(not Spam) = 0.8 * P(lot | not spam) = 0.125 )
term1 / term1 + term2

Prior , Event , Posterior ( a different space )
Spam , Lottery , P(SPAM|LOTTERY)

Not lets say we have another word. Lottery and Won.
how do we combine these two ? P(Spam|lottery&winning)
but the equation becomes like : 

term1 => P(SPAM)*P(LOT|SPAM)*P(WIN|SPAM)
...

@claude not try to add the sentenses in english to understand the
equations better.

Add questions :

1) 
