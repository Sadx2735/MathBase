there are lot of other things that are not captured by std and variance.
skewness and kurtosis comes into place.

first moment is p(x1)+p(x2)..p(xn)
second moment is p(x1^2)+p2(x2^2)...pn(xn^2)
third , forth and so on.

lottery : 
you buy for 1 dollar and by luck you get 100$ (1% prob)
you loose 1$ with 99% prob

so expected is 0.99*(-1)+0.01*99 => 0.0

insurance : 

you win 1$ and give 100$ if your client hits both as 1% and 99% prob

both are totally different. 99% you get 1 and 99% you get 99%

here the mean , variance and std all are gonna have same value so
we need some other metric to define these cases.

3rd moment gives the skewness ie the value we get are 9702 and -9702.
so we can see this as an example to consider.

@claude img for what this value means right side spread and left side
spread distribution.

Second case :

Game 1:
probability for 1/2 you win 1 dollar.
probability for 1/2 you loose a dollar.

Game 2:
probability 100/202 you win 10 cents
probability 100/202 you loose 10 cents
probability 1/202 you win 10 dollars
probability 1/202 you loose 10 dollars

Which one is riskier ?

1. first , second and third have the same value.
2. forth moment gives a big value on the 2nd graph since we have 
large values spread on the farther end.

E((X-Mu/Std)^4)

@claude give example diagram were it is useful.

### Quantiles

Newspaper budget :

8.7 14.2 18.3 18.4
median => 2. which is (14.2+18.3)/2
for 1/4 its the value of 8.7 alone.

@claude try upgrading the explanation.

### BoxPlot Visualization.

first calc the 25% , 50% , 75%, 100%
IQR => Q3-Q1

this gives the value of how youre 50% of data is present for example
if you have a hill kindoff structure then you have 50% on centre then
25+25 on the either side @claude add diagram.

@claude box plot :
first to 3rd quartile.

Q2 is median.

min and max are the end. min(Q3+1.5*IQR,x-max)
max(Q1-1.5*IQR,x-min)

other than this are outliers

@claude explain why the above is useful and why IQR*1.5 thing is there.

### Kernal density estimation.

KDE lets say we have a graph and then we put a small
gaussian curve around the points distribution. then we multiply them
by the value of the points then we get a approx pdf from the data 

@claude i dont understand the above fully please give more details

### Violin plot & QQ plots.

its same as the box plot drawn. but along with the PDF as the column width
on either side. 

QQ is the quantile vs plot for gaussian and QQ plots
standardize the data before.

