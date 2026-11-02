### Probability

#### Basics

Lets say we are in a school.
there are 10 kids and 3 play soccer and 7 don't
what is the probability of the event to pick and kid playing soccer
P(e)/P(s) = 3/10 so 0.3 , 30% chance.

Example of Experiment - uncertain activities like spinning a coin
P(H) = 1/2. H or T so 0.5

for 2 coins HH , HT , TT, TH so for getting two coins as same its 
either HH or TT so 2/4 and it turns to be 1/2.

for 3 coins we have a total sample size of 8. and getting HHH is one
of them so 1/8.

for getting 6,6 from 2 rolls of 6 sided dice its going to be 36.
and the P(e) = 1 so its 1/36.

#### Compliment of event

Lets say we are in a school.
there are 10 kids and 3 play soccer and 7 don't
what is the probability of the event to pick and kid playing soccer
P(e)/P(s) = 3/10 so 0.3 , 30% chance.

for compliment its going to a kid who doesnt play soccer so its value
is 70% ie 7/10.

#### Sum of Probabilities

- At a school kids can play one sport either soccer or basketball
soccer 30% basketball 40% so the total prob of kid playing something 
is 70% the rest of them dont play soccer.

- From a dice even number or the number 5.
and then we get 3/6+1/6 so 4/6 => 2/3.

- but the above is only for the case when we have disjoint events.
Now we need joint events to be considered

#### Sum of Probabilities ( Joint Events )

- At a school kids can play one sport either soccer or basketball
soccer 30% basketball 40% so the total prob of kid playing something 
is 70% the rest of them dont play soccer. but lets say we have kids 
who play both so they will be double counted so we need to negate the 
value P(A or B) = P(A)+P(B)-P(A&B).

#### Independence 

- if a probability of one move doesnt create any difference in the
second one. for example if the 1st coin is head then 2nd coin doesnt
depend on the first one for its possibility. but lets say if there is 
a case like probability of students absent or present in the class
then its depended on the RAIN so its dependence case.

Lets say we have 50kids who play soccer from the 100 kids now we
are going to split them into 30% 70% into 2 rooms so the first room
will get 15 and the second room will have 35.

how do we calculate this ?

P(Soccer playing kid in room one) room one is 30% and Soccer playing 
kid is 50% so 0.5*0.3*100 is 15.

P(A&B) = P(A)*P(B).

Getting heads 5 times on 5 coin toss.
P(H)=0.5 so 1/2*1/2*1/2*1/2*1/2 is 1/32. Same applies for 6,6 from
2 dice rolls.

#### Birthday Problem

we have 30 friends there exist two people with same birthday.
we have no two of them have the same birthday ( ie 1-P(A) )

Probability that everyone has a different birthday.its going to be
(365/365) for the first person and for the second person it (364/365)
then so on for 30 people.

then for having same day birthday its 1-p(a) so its 70%

#### Conditional probability

P(A and B) = P(A) x P(B|A)

When A and B are independent: P(B|A) = P(B).
so the rule becomes: P(A and B) = P(A) x P(B).

P(HH & given H) = P(HH) * P(HH | given H)
so P(HH & given H) / P(HH).

example 2. P(sum is 10 & first is 6) = 
P(10) * P(sum of 10 | given 6 at first)

its (1/36) = (1/6) its (1/6). 

With replacement :

but also feel this a bit confusion like for calculation P(A&B) i need
p(A given B) but for P(A given B) i need the value of P(A&B)

P(both Aces) by multiplying steps. 
Then P(first is Ace | both are Aces), which should be 1. Why?
For P(Aces/Aces) its going to be (4/52)*(4/52)=(4/52)*P(A|B)

even in count it goes like 4 cards taken out as aces then again from 
that we pick one which will be 4.

Without replacement :

(4/52)*(3/52). Fill claude.
