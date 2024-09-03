# Exercice 1

Create an S3 class from scratch; imagine the name of the class, a
function that creates it (and includes some data in it), and define at
least a `print()`, a `summary()` and a `plot(`) methods for it.

If you are looking for ideas, implement the following class:

  * Class "geneexpr", which contains expression values for a set of
    genes, in the form of a vector containing an arbitrary number of
    gene expression values. Each expression value is accompanied by a
    name for a gene (for example a gene symbol).
  * "`print`" should print a nice summary of the dataset; in particular,
    it should print the number of data points (and maybe the number of
    unique gene names, in case the same gene/symbol appears several
    times)
  * "`summary`" should go a little bit further, and print the number of
    genes plus a few information about the distribution (min, median,
    max)
  * "`plot`" should plot a histogram of the values.

(for the next part, you may need to wait until we have progressed
further in the course, talking about data manipulation). If you want
to go further, you can implement a "diffexpr" function, which will
take 2 genexpr objects and calculate the differental expression
(difference of expression) between the genes. It will have to take
into account the fact that the two gene lists may not contain the same
genes. In a second step, it should also be able to take into account
duplicate gene measurements, in a way or another (average them ? take
only the maximum ?).

# Exercice 2

The `summary()` function, when applied to numeric values, displays the
"five-number summary" of the values, as in the following example:

```r 
> summary(runif(100))
     Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
0.0006386 0.2646350 0.5089532 0.5088928 0.7493618 0.9828082
```

Modify the `summary()` function so that the output also adds a column
containing the standard deviation of the data.

In a second step, make sure to call the original summary function to
create the output, before adding a new column to it.


# Exercice 3
## A

During the course, we discussed how to improve the efficiency of the
following expression:

```r 
n <- 100000
m <- 100
results <- NULL
for (i in 1:n) { 
 result <- mean( runif( m ) )
 results <- c(results, result)
}
```

Do you have any ideas how you could potentially improve it? (if not,
ask for advice or look at exercice 5). Try these, and time them in
comparison to the two versions described in the course.

## B

If we create the vector with the right size before the loop, does it
make a difference if we initially fill it with NAs or with zeros ?

# Exercise 4
## A
Create an expression similar to the one described in exercise 3 but
which returns data in a matrix or dataframe (for example, each
iteration of the loop returns 2 values, the mean and the standard
deviation, meaning that the end result after the loop would be a
dataframe with 2 columns).

How can you create the resulting matrix/data frame ?

Does it make a difference if the matrix is increased by one row at
each iteration, compared to when the matrix is created first and then
filled in row by row ?

## B
Let's suppose that each iteration of the loop adds an unknown number
of results (it could be 0, 1 or more than 1, and this number could be
different for each iteration), so that it is impossible to create an
empty data structure of the right size and fill it row by row in the
loop.

Compare the following ways of creating the result:

  * fill separately several vectors (for example, one vector "means"
    and one vector "sds"), and combine them after the loop to obtain a
    matrix or data frame
  * "grow" a data frame or a matrix by the right number of rows at
    every step of the loop (how do you create the empty structure
    before the loop ?
  * create small dataframes with results at every step, add them to a
    list (we will see later in the course an efficient way to
    transform this list into a matrix or a data frame)

# Exercise 5
Go back to the exercice where we calculated many times the mean of several
random numbers). Test if you can obtain further improvements in speed
by making the following changes:

  * create a matrix containing all the random numbers, and use the
    `apply() command instead of a loop.
  * do not use the `mean()` function, but calculate the mean manually
    (using the `sum()` function for example)

# Exercice 6
Improve as much as possible the execution time of the following piece
of code (seen in the course):

```r 
pvalues <- NULL
for (i in 1:10000) {
  a <- runif(6)
  ttest <- t.test(a[1:3], a[4:6])
  pval <- ttest$p.value
  pvalues <- c(pvalues, pval)
}
```
