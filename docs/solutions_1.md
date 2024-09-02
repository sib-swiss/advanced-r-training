---
title: "Advanced R - solutions of the exercises - day 1"
date: "September 2024"
keep_md: yes
output:
  html_document:
    df_print: paged
    keep_md: yes
  html_notebook: default
  pdf_document: default
editor_options:
  chunk_output_type: inline
---

# Exercise 1
Suppose we have a list of amino acids, their masses and a protein sequence:


```r
aminoacids <- c("A", "R", "N", "D", "C", "E", "Q", "G", "H", "I", "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V" )
masses <- c(71.0788, 156.1875, 114.1038, 115.0886, 103.1388, 129.1155, 128.1307, 57.0519, 137.1411, 113.1594, 113.1594, 128.1741, 131.1926, 147.1766, 97.1167, 87.0782, 101.1051, 186.2132, 163.1760, 99.1326)
protein <- "MEFARILOP"
```

How can we use R to efficiently calculate the mass of the protein ?

We first create a named vector for the masses, and split the
protein into amino acids:


```r
names(masses) <- aminoacids
aa <- strsplit(protein, "")[[1]]
```

(`strsplit()` returns a list with a single element, so we need to
extract the vector of amino acids using the bracket notation).

We can then use the lookup table to calculate the total, making sure
to remove the missing values (non-existent amino acids and separately
list the ones that were missing:


```r
sum(masses[ aa ], na.rm=TRUE)
```

```
## [1] 958.1865
```

```r
# List of missing amino acids
aa[ is.na( masses[aa] ) ]
```

```
## [1] "O"
```

# Exercise 2

Suppose that you are given information about the amount of fertilizer used on several parts of a field, in the form of a factor:


```r
fert <- factor( c(10, 20, 50, 30, 10, 20, 10, 45) )
```

Starting from the factor, how would you calculate the mean amount of fertilizer used ?

We can not convert to numeric values directly, because we would only get the
indices (the link to the levels); we can first convert to character and then
to numeric:

```r
mean(as.numeric(as.character(fert)))
```

```
## [1] 24.375
```

```r
mean(as.numeric(as.vector(fert)))     # equivalent
```

```
## [1] 24.375
```

# Exercise 3A

The table() function allows you to count the number of occurrences
of each value in a vector; for example :


```r
set.seed(5)
data <- sample(1:10, 10, replace=TRUE)
data
```

```
##  [1] 2 9 9 9 5 7 7 3 3 6
```

```r
table(data)
```

```
## data
## 2 3 5 6 7 9 
## 1 2 1 1 2 3
```

However, the table has ‘gaps’ in it. How could we obtain the same result, but including all numbers from 1 to 10, with a count of ‘0’ when the value does not appear in the vector ?

We can make the vector into a factor, and explicitely
specify the possible levels (1 to 10); table will then
do the right thing:

```r
data <- factor( data, levels=1:10 )
table(data)
```

```
## data
##  1  2  3  4  5  6  7  8  9 10 
##  0  1  2  0  1  1  2  0  3  0
```

Side question: the tabulate() function solves part of our question here, but it is not entirely satisfactory. Why ?


```r
tabulate(data)
```

```
## [1] 0 1 2 0 1 1 2 0 3
```

Tabulate will fill in the gaps, but cannot know that
there are larger values (10) missing. This can be
specified in its options, however.

# Exercise 3B

When plotting a boxplot, the order of the groups is generally alphabetical; for example, the boxplot created by the following commands has groups A, AB, B, BA, C.


```r
a <- runif(100)
groups <- sample( c("A", "AB", "ABC", "B", "BC", "C", "CA"), 100, replace=TRUE)
boxplot(a ~ groups)
```

![](solutions-1_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

How you can easily force a different order  (e.g. A, B, C, AB, BC, CA, ABC) ?

We can make the vector into a factor, and specify the
levels in the order we want them:


```r
groups <- factor(groups, levels=c("A", "B", "C", "AB", "BC", "CA", "ABC"))
boxplot(a ~ groups)
```

![](solutions-1_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

# Exercice 3C

When conducting the linear regression described below, all groups are compared to the "A" group (as the intercept, chosen because it is the first group in alphabetical order). How can we force the lm function to choose group "ref" as the reference ?

```r
set.seed(1)
data <- runif(100)
groups <- rep( c("ref", "a", "b", "c"), each=25)
summary(lm(data~groups))
```

```
## 
## Call:
## lm(formula = data ~ groups)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.52001 -0.17433  0.00602  0.23320  0.46012 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  0.533400   0.053933   9.890 2.56e-16 ***
## groupsb     -0.071713   0.076273  -0.940    0.349    
## groupsc      0.011115   0.076273   0.146    0.884    
## groupsref   -0.001615   0.076273  -0.021    0.983    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.2697 on 96 degrees of freedom
## Multiple R-squared:  0.01517,	Adjusted R-squared:  -0.01561 
## F-statistic: 0.4929 on 3 and 96 DF,  p-value: 0.688
```
We can transform the vector of groups into a factor, and specify explicitely
which one is the reference (the first level), using the relevel() function. 
The model linear that is fitted is exactly the same one, but the display of
the coefficients change:


```r
groups <- factor(groups)
groups <- relevel(groups, "ref")
groups
```

```
##   [1] ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref
##  [19] ref ref ref ref ref ref ref a   a   a   a   a   a   a   a   a   a   a  
##  [37] a   a   a   a   a   a   a   a   a   a   a   a   a   a   b   b   b   b  
##  [55] b   b   b   b   b   b   b   b   b   b   b   b   b   b   b   b   b   b  
##  [73] b   b   b   c   c   c   c   c   c   c   c   c   c   c   c   c   c   c  
##  [91] c   c   c   c   c   c   c   c   c   c  
## Levels: ref a b c
```

```r
summary(lm(data~groups))
```

```
## 
## Call:
## lm(formula = data ~ groups)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.52001 -0.17433  0.00602  0.23320  0.46012 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  0.531786   0.053933   9.860 2.97e-16 ***
## groupsa      0.001615   0.076273   0.021    0.983    
## groupsb     -0.070098   0.076273  -0.919    0.360    
## groupsc      0.012730   0.076273   0.167    0.868    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.2697 on 96 degrees of freedom
## Multiple R-squared:  0.01517,	Adjusted R-squared:  -0.01561 
## F-statistic: 0.4929 on 3 and 96 DF,  p-value: 0.688
```


```r
groups <- factor(groups, levels=c("ref", "a", "b", "c"), ordered=TRUE)
groups
```

```
##   [1] ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref ref
##  [19] ref ref ref ref ref ref ref a   a   a   a   a   a   a   a   a   a   a  
##  [37] a   a   a   a   a   a   a   a   a   a   a   a   a   a   b   b   b   b  
##  [55] b   b   b   b   b   b   b   b   b   b   b   b   b   b   b   b   b   b  
##  [73] b   b   b   c   c   c   c   c   c   c   c   c   c   c   c   c   c   c  
##  [91] c   c   c   c   c   c   c   c   c   c  
## Levels: ref < a < b < c
```

```r
summary(lm(data~groups))
```

```
## 
## Call:
## lm(formula = data ~ groups)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.52001 -0.17433  0.00602  0.23320  0.46012 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  0.517847   0.026966  19.203   <2e-16 ***
## groups.L    -0.007496   0.053933  -0.139    0.890    
## groups.Q     0.040607   0.053933   0.753    0.453    
## groups.C     0.050953   0.053933   0.945    0.347    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.2697 on 96 degrees of freedom
## Multiple R-squared:  0.01517,	Adjusted R-squared:  -0.01561 
## F-statistic: 0.4929 on 3 and 96 DF,  p-value: 0.688
```

# Exercise 4A

The following R code generates random data and fits a linear model:


```r
set.seed(1)
x <- runif(10)
y <- 2*x + rnorm(10)
model <- lm(y~x)
summary(model)
```

```
## 
## Call:
## lm(formula = y ~ x)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.3327 -0.6267  0.2447  0.6006  1.2851 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept)  -0.1361     0.7648  -0.178    0.863  
## x             2.4039     1.2186   1.973    0.084 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.154 on 8 degrees of freedom
## Multiple R-squared:  0.3273,	Adjusted R-squared:  0.2432 
## F-statistic: 3.891 on 1 and 8 DF,  p-value: 0.08399
```

How would you copy into a variable "slope" the slope of the regression line (the second regression coefficient) ? And how would you extract in a variable "pvalue" the p-value associated with it ?

We can look at the structure of the linear model:


```r
names(model)
```

```
##  [1] "coefficients"  "residuals"     "effects"       "rank"         
##  [5] "fitted.values" "assign"        "qr"            "df.residual"  
##  [9] "xlevels"       "call"          "terms"         "model"
```

```r
str(model)
```

```
## List of 12
##  $ coefficients : Named num [1:2] -0.136 2.404
##   ..- attr(*, "names")= chr [1:2] "(Intercept)" "x"
##  $ residuals    : Named num [1:10] -0.792 0.473 0.643 0.345 -0.251 ...
##   ..- attr(*, "names")= chr [1:10] "1" "2" "3" "4" ...
##  $ effects      : Named num [1:10] -3.762 2.276 0.89 0.802 -0.237 ...
##   ..- attr(*, "names")= chr [1:10] "(Intercept)" "x" "" "" ...
##  $ rank         : int 2
##  $ fitted.values: Named num [1:10] 0.502 0.758 1.241 2.047 0.349 ...
##   ..- attr(*, "names")= chr [1:10] "1" "2" "3" "4" ...
##  $ assign       : int [1:2] 0 1
##  $ qr           :List of 5
##   ..$ qr   : num [1:10, 1:2] -3.162 0.316 0.316 0.316 0.316 ...
##   .. ..- attr(*, "dimnames")=List of 2
##   .. .. ..$ : chr [1:10] "1" "2" "3" "4" ...
##   .. .. ..$ : chr [1:2] "(Intercept)" "x"
##   .. ..- attr(*, "assign")= int [1:2] 0 1
##   ..$ qraux: num [1:2] 1.32 1.12
##   ..$ pivot: int [1:2] 1 2
##   ..$ tol  : num 1e-07
##   ..$ rank : int 2
##   ..- attr(*, "class")= chr "qr"
##  $ df.residual  : int 8
##  $ xlevels      : Named list()
##  $ call         : language lm(formula = y ~ x)
##  $ terms        :Classes 'terms', 'formula'  language y ~ x
##   .. ..- attr(*, "variables")= language list(y, x)
##   .. ..- attr(*, "factors")= int [1:2, 1] 0 1
##   .. .. ..- attr(*, "dimnames")=List of 2
##   .. .. .. ..$ : chr [1:2] "y" "x"
##   .. .. .. ..$ : chr "x"
##   .. ..- attr(*, "term.labels")= chr "x"
##   .. ..- attr(*, "order")= int 1
##   .. ..- attr(*, "intercept")= int 1
##   .. ..- attr(*, "response")= int 1
##   .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
##   .. ..- attr(*, "predvars")= language list(y, x)
##   .. ..- attr(*, "dataClasses")= Named chr [1:2] "numeric" "numeric"
##   .. .. ..- attr(*, "names")= chr [1:2] "y" "x"
##  $ model        :'data.frame':	10 obs. of  2 variables:
##   ..$ y: num [1:10] -0.289 1.232 1.884 2.392 0.098 ...
##   ..$ x: num [1:10] 0.266 0.372 0.573 0.908 0.202 ...
##   ..- attr(*, "terms")=Classes 'terms', 'formula'  language y ~ x
##   .. .. ..- attr(*, "variables")= language list(y, x)
##   .. .. ..- attr(*, "factors")= int [1:2, 1] 0 1
##   .. .. .. ..- attr(*, "dimnames")=List of 2
##   .. .. .. .. ..$ : chr [1:2] "y" "x"
##   .. .. .. .. ..$ : chr "x"
##   .. .. ..- attr(*, "term.labels")= chr "x"
##   .. .. ..- attr(*, "order")= int 1
##   .. .. ..- attr(*, "intercept")= int 1
##   .. .. ..- attr(*, "response")= int 1
##   .. .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
##   .. .. ..- attr(*, "predvars")= language list(y, x)
##   .. .. ..- attr(*, "dataClasses")= Named chr [1:2] "numeric" "numeric"
##   .. .. .. ..- attr(*, "names")= chr [1:2] "y" "x"
##  - attr(*, "class")= chr "lm"
```
The "coefficients" contains the slope:


```r
model$coefficients
```

```
## (Intercept)           x 
##  -0.1361272   2.4039002
```

```r
model$coefficients[2]
```

```
##      x 
## 2.4039
```
Note that this requires knowledge about the structure of the variable; it is much
better to use an accessor function, such as coef(), and to use the name of
the variable we are interested in rather than the position:


```r
coef(model)[2]
```

```
##      x 
## 2.4039
```

```r
coef(model)["x"]
```

```
##      x 
## 2.4039
```

```r
getAnywhere(coef.default)
```

```
## A single object matching 'coef.default' was found
## It was found in the following places
##   registered S3 method for coef from namespace stats
##   namespace:stats
## with value
## 
## function (object, complete = TRUE, ...) 
## {
##     cf <- object$coefficients
##     if (complete) 
##         cf
##     else cf[!is.na(cf)]
## }
## <bytecode: 0x5628eb6e0068>
## <environment: namespace:stats>
```

Using coef provides the same result as direct access, but will continue
to work even if the structure of the object changes.

The p-value, however, is not provided in this table. This is because it is actually
calculated only when we ask for the summary:


```r
modelsummary <- summary(model)
class(modelsummary)
```

```
## [1] "summary.lm"
```

```r
names(modelsummary)
```

```
##  [1] "call"          "terms"         "residuals"     "coefficients" 
##  [5] "aliased"       "sigma"         "df"            "r.squared"    
##  [9] "adj.r.squared" "fstatistic"    "cov.unscaled"
```

```r
modelsummary$coefficients   # or, equivalently:
```

```
##               Estimate Std. Error    t value   Pr(>|t|)
## (Intercept) -0.1361272   0.764750 -0.1780023 0.86314593
## x            2.4039002   1.218592  1.9726860 0.08399393
```

```r
coef(modelsummary)
```

```
##               Estimate Std. Error    t value   Pr(>|t|)
## (Intercept) -0.1361272   0.764750 -0.1780023 0.86314593
## x            2.4039002   1.218592  1.9726860 0.08399393
```

```r
coef(modelsummary)[2,4]
```

```
## [1] 0.08399393
```

```r
coef(modelsummary)["x", "Pr(>|t|)"]
```

```
## [1] 0.08399393
```

# Exercise 4B

The following code fits an ANOVA model:


```r
set.seed(1)
groups <- rep(LETTERS[1:3], each=5 )
data <- rnorm( length(groups) )
model_anova <- aov( data ~ groups )
summary(model_anova)
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## groups       2   0.03  0.0148   0.012  0.988
## Residuals   12  14.47  1.2059
```

How would you extract the p-value provided by this model into a "pvalue" variable ?

As before, we need to look into the object to understand its structure, using str()
for example. We find that the object contains a list of length 1, which contains a
data.frame with the information we require:

```r
mysummary <- summary(model_anova)
str(mysummary)
```

```
## List of 1
##  $ :Classes 'anova' and 'data.frame':	2 obs. of  5 variables:
##   ..$ Df     : num [1:2] 2 12
##   ..$ Sum Sq : num [1:2] 0.0296 14.4702
##   ..$ Mean Sq: num [1:2] 0.0148 1.2059
##   ..$ F value: num [1:2] 0.0123 NA
##   ..$ Pr(>F) : num [1:2] 0.988 NA
##  - attr(*, "class")= chr [1:2] "summary.aov" "listof"
```

```r
mysummary[[1]]["groups", "Pr(>F)"]
```

```
## [1] 0.9878183
```


# Exercise 5
Check how R implements "factors" in principle. For example, where are
the labels and the fact that the factor is ordered stored ? Where is
the function that prints a factor, and the function that makes a
summary out of the factor ?

Looking at the summary or the names of the object does not tell us
anything.  We can use the `str()` command to show us some information
about the structure:


```r
fert <- factor( c(10, 20, 50, 30, 10, 20, 10, 45) )
str(fert)
```

```
##  Factor w/ 5 levels "10","20","30",..: 1 2 5 3 1 2 1 4
```

It does not tell us much about how the data is stored; one other way
is to remove the "factor" class, and see what default object is
printed:


```r
class(fert) <- ""
fert
```

```
## [1] 1 2 5 3 1 2 1 4
## attr(,"levels")
## [1] "10" "20" "30" "45" "50"
## attr(,"class")
## [1] ""
```

```r
typeof(fert)
```

```
## [1] "integer"
```

We see that the data is stored as a vector of integer, and additional
information is stored in the attributes of the object. In details:


```r
fert <- factor( c(10, 20, 50, 30, 10, 20, 10, 45) )
attributes(fert)
```

```
## $levels
## [1] "10" "20" "30" "45" "50"
## 
## $class
## [1] "factor"
```
We can see that the levels of the factor are saved as an attribute, and
the class of the variable is set to "factor". 


```r
methods(class="factor")
```

```
##  [1] [             [[            [[<-          [<-           all.equal    
##  [6] as.character  as.data.frame as.Date       as.list       as.logical   
## [11] as.POSIXlt    as.vector     c             coerce        droplevels   
## [16] format        initialize    is.na<-       length<-      levels<-     
## [21] Math          Ops           plot          print         relevel      
## [26] relist        rep           show          slotsFromS3   summary      
## [31] Summary       xtfrm        
## see '?methods' for accessing help and source code
```

```r
# print.factor
# summary.factor
```

We can see in the list the methods print and summary, so that the two
functions are print.factor and summary.factor (which you can print and
look at if you want)


```r
fert <- factor( c(10, 20, 50, 30, 10, 20, 10, 45), ordered=TRUE )
attributes(fert)
```

```
## $levels
## [1] "10" "20" "30" "45" "50"
## 
## $class
## [1] "ordered" "factor"
```

In the case of an ordered factor, there is a second class, "ordered";
otherwise, the data is stored in the same way.
