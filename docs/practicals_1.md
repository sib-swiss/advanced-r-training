Apart from the exercices suggested below, we suggest that you experiment with the topics we discussed this morning (especially those that you do not feel comfortable with): create and work with lists and factors, explore objects, etc.

# Exercice 0
If you are not comfortable yet with knitr and markdown, try them -- ask for help and material if required.

# Exercise 1
Check how R implements "factors" in principle. For example, where are the labels and the fact that the factor is ordered stored ? Where is the function that prints a factor, and the function that makes a summary out of the factor ?

# Exercice 2
Suppose that you are given information about the amount of fertilizer used on several parts of a field, in the form of a factor:

```r 
fert <- factor( c(10, 20, 50, 30, 10, 20, 10, 45) )
```

Starting from the factor, how would you calculate the mean amount of fertilizer used ?

# Exercise 3
## A
The table() function allows you to count the number of occurrences of each value in a vector ; for example :

```r 
> set.seed(5)
> data <- sample(1:10, 10, replace=TRUE)

> table(data)
data
2 3 5 6 7 9 
1 2 1 1 2 3 
```

However, the table has ‘gaps’ in it. How could we obtain the same result, but including all numbers from 1 to 10, with a count of ‘0’ when the value does not appear in the vector ?

Side question: the tabulate() function solves part of our question here, but it is not entirely satisfactory. Why ?
## B
When plotting a boxplot, the order of the groups is generally alphabetical; for example, the boxplot created by the following commands has groups A, AB, B, BA, C.

```r 
a <- runif(100)
groups <- sample( c("A", "AB", "ABC", "B", "BC", "C", "CA"), 100, replace=TRUE)
boxplot(a ~ groups)
```

How you can easily force a different order  (e.g. A, B, C, AB, BC, CA, ABC) ?

## C
When conducting the linear regression described below, all groups are compared to the "A" group (as the intercept, chosen because it is the first group in alphabetical order). How can we force the lm function to choose group "ref" as the reference ?

```r 
set.seed(1)
data <- runif(100)
groups <- rep( c("ref", "a", "b", "c"), each=25)
summary(lm(data~groups))
```

# Exercise 4
## A
The following R code generates random data and fits a linear model:

```r 
set.seed(1)
x <- runif(10)
y <- 2*x + rnorm(10)
model <- lm(y~x)
summary(model)
```

How would you copy into a variable "slope" the slope of the regression line (the second regression coefficient) ? And how would you extract in a variable "pvalue" the p-value associated with it ?

## B
The following code fits an ANOVA model:

```r 
et.seed(1)
groups <- rep(LETTERS[1:3], each=5 )
data <- rnorm( length(groups) )
model_anova <- aov( data ~ groups )
summary(model_anova)
```

How would you extract the p-value provided by this model into a "pvalue" variable ?

# Exercice 5
Create an S3 class from scratch; imagine the name of the class, a function that creates it (and includes some data in it), and define at least a print(), a summary() and a plot() methods for it.

If you are looking for ideas, implement the following class:
Class "geneexpr", which contains expression values for a set of genes, in the form of a vector containing an arbitrary number of gene expression values. Each expression value is accompanied by a name for a gene (for example a gene symbol).
"print" should print a nice summary of the dataset; in particular, it should print the number of data points (and maybe the number of unique gene names, in case the same gene/symbol appears several times)
"summary" should go a little bit further, and print the number of genes plus a few information about the distribution (min, median, max)
"plot" should plot a histogram of the values.
(for the next part, you may need to wait until we have progressed further in the course, talking about data manipulation). If you want to go further, you can implement a "diffexpr" function, which will take 2 genexpr objects and calculate the differental expression (difference of expression) between the genes. It will have to take into account the fact that the two gene lists may not contain the same genes. In a second step, it should also be able to take into account duplicate gene measurements, in a way or another (average them ? take only the maximum ?).

# Exercice 6
The summary() function, when applied to numeric values, displays the "five-number summary" of the values, as in the following example:

```r 
> summary(runif(100))
     Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
0.0006386 0.2646350 0.5089532 0.5088928 0.7493618 0.9828082 
Modify the summary function so that the output also adds a column containing the standard deviation of the data.
```
