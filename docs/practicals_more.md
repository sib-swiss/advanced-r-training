
# Exercise 1
How would you efficiently "zip" two vectors in R ? In other words: we have two vectors

```r 
a <- LETTERS[1:10]      # A, B, C, D...
b <- 1:10
```

and we want to obtain a vector with

```r 
> zip
[1] "A" "1" "B" "2" "C" "3" ....
```

# Exercise 2
A)
Create a long vector in R, for example

```r 
d <- 1:1000000
How would you efficiently calculate the sum of every 5 adjacent values in the vector
(without overlap: the sum of the first five elements, the sum of the next five, etc) ?

B)
How would you calculate the overlapping sum of 5 adjacent values:
the sum of the first five, the sum of the 5 elements starting at the 2nd position, etc

# Exercise 3
The following dataset describes the content of a 1024x1024 matrix

```r 
n <- 1024 
y <- rep( 1:n, each=n ) 
x <- rep( 1:n, times=n ) 
value <- rnorm( length(x) )
data <- data.frame(x, y, value)
```

Each row of the data frame describes one element of the matrix: its row number (x), column number (y), and the actual value.

How would you transform this data frame into an actual matrix containing the values (value) at the correct position (as given by each x and y) ?

# Exercise 4
The following code creates a dataset:

```r 
set.seed(1) 
data <- data.frame(y=rnorm(100), x1=rnorm(100), x2=rnorm(100), group=sample(1:10, 100, replace=T) ) 
data[ data$group==5, "y" ] <- data[ data$group==5, "x2" ] + rnorm( sum(data$group==5) )/10
```
Write an R program that will:

fit a linear model of the form y ~ x1 + x2 separately to the data corresponding to each group
return a vector (or another appropriate data structure) containing the p-values for significance of the x2 variable in each model
single out the group that produced the lowest such p-value
If you used a for loop (over all groups) in your program, rewrite it without using the for loop.

# Exercise 5
This is a follow-up to exercise 3; this time, our data frame has gaps (it does not describe all the elements of the matrix). Create a dataframe with the following code:

```r 
n <- 1024 y <- rep( 1:n, each=n ) x <- rep( 1:n, times=n )

value <- rep(0, length(x)) 
data <- data.frame(x, y, value) 
data <- within(data, value[ x>n/4 & x<n*3/4 & y>n/4 & y<n*3/4 ] <- 1 ) 
data <- within(data, value[ x>n*3/8 & x<n*5/8 & y>n*3/8 & y<n*5/8 ] <- 0 ) 
data <- data[ data$value > 0, ]
```

How do you transform this data.frame into a matrix ?
When you are done, plot the matrix using the image() function, and check whether your result is correct.

