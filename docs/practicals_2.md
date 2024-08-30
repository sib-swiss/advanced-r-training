# Exercice 1
## A
During the course, we discussed how to improve the efficiency of the following expression:

```r 
n <- 100000
m <- 100
results <- NULL
for (i in 1:n) { 
 result <- mean( runif( m ) )
 results <- c(results, result)
}
```

Do you have any ideas how you could potentially improve it? (if not, ask for advice or look at exercice 3). Try these, and time them in comparison to the two versions described in the course.

## B
If we create the vector with the right size before the loop, does it make a difference if we initially fill it with NAs or with zeros ?

# Exercise 2
## A
Create an expression similar to the one described in exercise 1 but which returns data in a matrix or dataframe (for example, each iteration of the loop could return 2 values, the mean and the standard deviation, meaning that the end result after the loop would be a dataframe with 2 columns).

How can you create the resulting matrix/data frame ?

Does it make a difference if the matrix is increased by one row at each iteration, compared to when the matrix is created first and then filled in row by row ?

## B
Let's suppose that each iteration of the loop adds an unknown number of results (it could be 0, 1 or more than 1, and this number could be different for each iteration), so that it is impossible to create an empty data structure of the right size and fill it row by row in the loop.

Compare the following ways of creating the result:

fill separately several vectors (for example, one vector "means" and one vector "sds"), and combine them after the loop to obtain a matrix or data frame
"grow" a data frame or a matrix by the right number of rows at every step of the loop (how do you create the empty structure before the loop ?
create small dataframes with results at every step, add them to a list. (we will see later in the course an efficient way to transform this list into a matrix or a data frame)

# Exercise 3
Go back to exercice 1 (calculating many times the mean of several random numbers). Test if you can obtain further improvements in speed by making the following changes:

create a matrix containing all the random numbers, and use the apply command instead of a loop.
do not use the mean() function, but calculate the mean manually (using the sum() function for example)

# Exercice 4
Improve as much as possible the execution time of the following piece of code (seen in the course):

```r 
pvalues <- NULL
for (i in 1:10000) {
  a <- runif(6)
  ttest <- t.test( a[1:3], a[4:6])
  pval <- ttest$p.value
  pvalues <- c(pvalues, pval)
}
```
