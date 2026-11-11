### Joint Distribution

lets say we have probability distribution which gives the value for
age of people vs the present count of the children. then consider a
situation were we need to get the value of the prob for picking a 
9 year old and 49 inch tall.

so we have 3 of people and the same people in the second distribution
(4/10)*(3/4) ie 3 out of 4 who were in 49 were of age 9.

what if we create a distribution that have this precalculated
P(x,y) is the value in the grid that gives this particular value.

for independent P(x)*P(y) for P(x)*P(y/x) for dependent

### Joint Continuous Distribution

satisfaction rate of custom and call waiting time.
plot x as waiting time(Mins) then y as the customer rating(points)

then the value is :

![formula](C:\Work\MathBase\STPR\diagrams\JointDistro.png)

### Marginal & Conditional Distribution

sum of either of one probability lets take a case were rows represent
the values of the age and columns represent the height then the value
of the P(50) is just the sum of the age column of 50.

@claude add for conditional aswell.
### Covariance or Correlation

X : age of a child.
Y1 : height of the child
Y2 : grades in a test.
Y3 : number of naps per day

how do we compare the XvsY1 graph etc.
covariance > 0. Age vs Height (grows along)
covariance ~ 0. Age vs Grades (not related)
covariance <0. Age vs naps (decreasing along)

how to find it :
mx = 0, my = 0
cov(x,y) => Sum((x-mx)*(y-my))/N)

### Covariance of probability distribution

both players with 1$ each
both players lose 1$ each
increasing.

(-1*-1)+(1*1) => 2.

x wins 1$ and y losses $1 and Vice Versa.
Decreasing.

(-1*1)+(-1*1) => -2.

When unequal Probability :

Pxy(x,y)*(x-mx)*(y-my)

@claude give explanations and examples

### Covariance matrix :

Mxy => [[x,xy],[yx,y]]

correlation coefficient its divided by sqrt of variance of x *
sqrt of variance of y so that even if the value is bigg it works

correlation coeffecient = cov(x,y)/sigmax*sigmay if its >0 same rule applies
