## R-Console Input and Evaluation

				**x <- 1** *##Assignement operator*
				**print(x)**
				**x**
				**msg <- "hello"**

				**x <- 1:10** *#create a sequence*

## Data Types - R Objects and Attributes

R objects can have attributes

* names, dimnames
* dimensions (ex. matrices, arrays etc.)
* class (integer, numeric, logical, character, complex)
* Length
* Other used-defined attributes/metadata

A Vector can only contain objects of the same class

A List is represented as a vector but can contain objects of different classes

				**class(x)**
				**typeof(x)**

				**attributes()** function to set or modify attributes

### Numbers

				**x<- 5**
				**class(x)**
				**> [1] Numeric**

				**x<- 5.9**
				**class(x)**
				**> [1] Numeric**

*L Suffix to explicitly ask a integer*

				**x<- 5L**
				**class(x)**
				**> [1] Integer**

*Inf* number (1/0)
*NaN* number (1/Inf)

## Data Types - Vectors and Lists

*Create a vector with c() function*

				x<-c(0.5,0.6) *## numeric*

				x <- c(TRUE,FALSE) *## logical*

				x <- c(T,F) *## logical*

				x <- c("a","b", "c") *## character*

				x <- 10:20 *## integer*

				x<-c(1L, 2L, 3L) *## integer*

				x<- c(1+0i, 2+4i) *## complex*

				*Create a vector with vector() function*

				x <- vector("numeric", length = 10)
				x <- vector(mode = "numeric", length = 10)

*Mixing values*

You don't get an error, but... Least common denominator

				**y <-  c(1.7, "a")** *##Least common denominator : character*

				**y <-  c(TRUE, 2)** *##Numeric*

				**y <-  c("a", TRUE)** *##Numeric*

*Explicit coercition*

				x<-0:6

				class(x)
				>[1] Integer
				as.numeric(x)
				> [1] 0 1 2 3 4 5 6
				as.logical(x)
				> [1] FALSE TRUE TRUE TRUE TRUE TRUE TRUE
				as.character(x)
				> [1] "0" "1" "2" "3" "4" "5" "6"

*Sometimes coercition can't work*

ex.

				x <-c("a", "b", "c")
				as.numeric(x)
				>[1] NA NA NA

### Lists

				**x <- list(1, "a", TRUE, 1+4i)
				x

				[[1]]
				[1] 1

				[[2]]
				[1] "true"

				[[3]]
				[1] TRUE

				[[4]]
				[1] 1+4i**

## Data Types - Matrices

				**m <- matrix(,ncol = 2, nrow =3)
				> m
  				   [,1] [,2]
				[1,]   NA   NA
				[2,]   NA   NA
				[3,]   NA   NA

				> dim(m)
				[1] 3 2
				> attributes(m)
				$dim
				[1] 3 2**

*Matrices are constructed column-wise: data are filled BY COLUMNS*

				**> m <- matrix(1:6,ncol = 3, nrow =2)
				> m
				     [,1] [,2] [,3]
				[1,]    1    3    5
				[2,]    2    4    6**

*Create a Matrix by add dimension attribute to a vector*

				m <- 1:10
				> [1] 1 2 3 4 5 6 7 8 9 10
				> dim(m) <-c(2,5)
				> m
				     [,1] [,2] [,3] [,4] [,5]
				[1,]    1    3    5    7    9
				[2,]    2    4    6    8   10

*Create a Matrix by column or row binding*

				> x <- 1:3
				> y <- 10:12
				> cbind(x, y)
				     x  y
				[1,] 1 10
				[2,] 2 11
				[3,] 3 12
				> rbind(x,y)
				  [,1] [,2] [,3]
				x    1    2    3
				y   10   11   12

## Data Types - Factors

Factors represents *categorical data* and *can be ordered or unordered* = integer vector where each integer has a *label*

* factors are treated specially by modeling function like **lm()** and **glm()**

* factors are better than integer because factors are self-describing

factors can be created by **factor()** function and the input of this function is a character vector :
				> x <- factor (c("yes", "yes", "no", "yes", "no"))
				> x
				[1] yes yes no  yes no 
				Levels: no yes

So we created a factor with two levels ("yes" and "no") ; the factor prints differently than a character vector (with separate attribute "Levels")

The function **table()** on the factor prints the frequency of each level there are in the factor :

				> table(x)
				x
				no yes 
				  2   3 

we can use the **unclass()** function that strip out the class from the vector, so we can bring the factor down to an integer vector with attributes "no" and "yes" :

				> unclass(x)
				[1] 2 2 1 2 1
				attr(,"levels")
				[1] "no"  "yes"

The order of the levels can be set using **levels** argument to **factor()**
				> x <- factor (c("yes", "yes", "no", "yes", "no"), levels = c("yes", "no"))
				> x
				[1] yes yes no  yes no 
				Levels: yes no

The *baseline* level is the first level in the factor ("yes" in this case), R language set normally the levels in alphabetical order. To override this, you have to change explicetely the levels.