Apart from the exercices suggested below, we suggest that you experiment with the topics we discussed this morning (especially those that you do not feel comfortable with): create and work with lists and factors, explore objects, etc.

# Exercice 0

If you are not comfortable yet with knitr and RMarkdown, try them. You
can follow the exercises given at the end of the introductory
slides. Do not hesitate to ask for help and material if required (we
will not discuss knitr and Rmarkdown in more details during this
course).

# Exercise 1
Suppose we have a list of amino acids, their masses and a protein sequence:

```r 
aminoacids <- c("A", "R", "N", "D", "C", "E", "Q", "G", "H", "I", "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V" )
masses <- c(71.0788, 156.1875, 114.1038, 115.0886, 103.1388, 129.1155, 128.1307, 57.0519, 137.1411, 113.1594, 113.1594, 128.1741, 131.1926, 147.1766, 97.1167, 87.0782, 101.1051, 186.2132, 163.1760, 99.1326)
protein <- "MEFARILOP"
```

How can we use R to efficiently calculate the mass of the protein ?

Hint: you can split the string using the `strsplit()` command in order
to get individual letters, and then create some lookup table to find
the masses and sum them.


# Exercice 2
Suppose that you are given information about the amount of fertilizer used on several parts of a field, in the form of a factor:

```r 
fert <- factor( c(10, 20, 50, 30, 10, 20, 10, 45) )
```

Starting from the factor, how would you calculate the mean amount of fertilizer used ?

# Exercise 3
## A

The `table()` function allows you to count the number of occurrences
of each value in a vector ; for example :

```r 
> set.seed(5)
> data <- sample(1:10, 10, replace=TRUE)

> table(data)
data
2 3 5 6 7 9 
1 2 1 1 2 3 
```

However, the table has ‘gaps’ in it. How could we obtain the same
result, but including all numbers from 1 to 10, with a count of ‘0’
when the value does not appear in the vector ?

Side question: the tabulate() function solves part of our question
here, but it is not entirely satisfactory. Why ?

## B
When plotting a boxplot, the order of the groups is generally
alphabetical; for example, the boxplot created by the following
commands has groups A, AB, B, BA, C.

```r 
a <- runif(100)
groups <- sample( c("A", "AB", "ABC", "B", "BC", "C", "CA"), 100, replace=TRUE)
boxplot(a ~ groups)
```

How you can easily force a different order  (e.g. A, B, C, AB, BC, CA, ABC) ?

## C
When conducting the linear regression described below, all groups are
compared to the "A" group (as the intercept, chosen because it is the
first group in alphabetical order). How can we force the `lm()` function
to choose group "ref" as the reference ?

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

How would you copy into a variable "slope" the slope of the regression
line (the second regression coefficient) ? And how would you extract
in a variable "pvalue" the p-value associated with it ?

## B
The following code fits an ANOVA model:

```r 
et.seed(1)
groups <- rep(LETTERS[1:3], each=5 )
data <- rnorm( length(groups) )
model_anova <- aov( data ~ groups )
summary(model_anova)
```

How would you extract the p-value provided by this model into a
"pvalue" variable ?

# Exercise 5
Check how R implements "factors" in principle. For example, where are
the labels and the fact that the factor is ordered stored ? Where is
the function that prints a factor, and the function that makes a
summary out of the factor ?
