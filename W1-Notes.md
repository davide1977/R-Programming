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

## Data Types - Missing Values

**is.na()**

Used to test objects if they are **NA** (Missing value)

NA values have a class also (you can have integer NA, character NA etc.)

**is.nan()**

Used to test objects if they are **NaN** (Undefined mathematical operations ex. )

NaN is also NA but the contrary is not true

Exemples :
				> x <- c(1,2,NA,10,3)
				> x
				[1]  1  2 NA 10  3
				> is.na(x)
				[1] FALSE FALSE  TRUE FALSE FALSE
				> is.nan(x)
				[1] FALSE FALSE FALSE FALSE FALSE


NA is not NaN

				> x <- c(1,2,NaN,NA,10,3)
				> is.na(x)
				[1] FALSE FALSE  TRUE  TRUE FALSE FALSE
				> is.nan(x)
				[1] FALSE FALSE  TRUE FALSE FALSE FALSE

NaN is NA

## Data Types - Data Frames

Key data type in R, used to store tabular data. They're special type of list where every element **has to have the same length** (it means: every column of the table has the same length)

* Every element of the list can be thought as a column, and the length of each element is the number of rows

* unlike matrices, data frames can store different classes of objects (like lists)

* Every row of a data frame has a name (an attribute called **row.names()** this can be useful : each row can represent a subject enrolled into a study, and the row name can be the subject ID, for exemple...

* data frames are usually created with **read.table()** or **read.csv()** functions (but also **data.frame()** function)

* you can convert a data frame to a matrix with **data.matrix()** function : but if you have different classes of objects, you'll have coercition and the result'll not be not what you expect.

				> x <- data.frame(foo = 1:4, bar = c(T, T, F, F))
				> x
				  foo   bar
				1   1  TRUE
				2   2  TRUE
				3   3 FALSE
				4   4 FALSE
				> nrow(x)
				[1] 4
				> ncol(x)
				[1] 2

if you don't specify a row.names() R will name that 1,2,3,4 etc.

## Data Types - Name Attributes

R Objects can have *names* : this is true for Data Frames, but also for all the other objects : **very usdeful to write readable code and self-describing objects!!**

				> x <- 1:3
				> names(x)
				NULL
				> names(x) <-c("foo", "bar", "norf")
				> x
				 foo  bar norf 
				   1    2    3 
				> names(x)
				[1] "foo"  "bar"  "norf"

For Lists :

				> x <- list(a = 1, b = 2, c = 3)
				> x
				$a
				[1] 1

				$b
				[1] 2

				$c
				[1] 3

For matrices (dimnames) assigning a list, where the first element of the list is a vector of row names, and the second element is a vector of column names :

				> m <- matrix(1:4, nrow = 2, ncol = 2)
				> dimnames(m) <- list(c("a", "b"), c("c", "d"))
				> m
				  c d
				a 1 3
				b 2 4


## Data Types - Summary

Data Types :

* Atomic classes : numeric, logical, character, integer, complex
* vectors (only elements of the same class), lists (elements of all classes)
* factors (categorical data)
* missing values (Na, NaN)
* data frames (Used to store tabular data with differents classes each col)
* names (all objects can have name: useful to have self describing data)

## Reading Tabular Data

### Reading Data

* **read.table**, **read.csv** for reading tabular data in text files and return a data frame in R-Console

* **readLines**, reading lines in a text file (any type of file: return a vector of text)

* **source** read R code files (inverse of **dump**)

* **dget** also reading R code for R objects that have benn dparsed (**dpars**) into text files

* **load** reads saved workspaces

* **unserialize** read single R objects in binary forms

### Writing Data

* **write.table**

* **writeLines**

* **dump**

* **dput**

* **save**

* **serialize**

### read.table

The most common function to read data. This are its arguments :

* **file** name of the file/connection

* **header** indicates if the file has a header that is not a data to read

* **sep** indicate the separator of the "table" in the file

* **colClasses** (not required) character vector which length is the same as the number of the columns of the data set and indicates what is the class of each column of the data set

* **nrows** (not required) nr of the rows in the dataset

* **comment.char** the default is the # indicate the eventual comment character in the dataset (everything on the right of the symbol on the same line is ignored)

* **skip** number of lines to skip from the beginning

* **stringAsFactors** - default to TRUE - if you want to code character variable to factors

For small Datasets you can usually call read.table without any argiments (only name of the text file)

				data <- read.table("foo.txt")

R will automatically do :

* skip lines that begins with a #
* figure out how many rows there are
* figure out what type of variable is in each column of the table

if you specify arguments R will run faster and more efficiently

* **read.csv** is identical to **read.table** except the default separator is a comma, and header is always equal to TRUE

## Reading Large tables

How to make your life easier and prevent R from chocking

* use help page of **read.table** or **read.csv** for hints

* calculate the memory required to store your dataset : if it's too much stop right here

* set comment.char = "" is there are no commented lines in the file

* Use the colClasses argument ! specify this can make **read.table** much faster (x2 fast!) !! Of course you have to know the class of each column of your data frame, but, for ex. if every columns of the data set are "numeric", you can just set **colClasses = "numeric"**

Quick and dirty way to figure out the classes of each column :

				initial <- read.table("datatable.txt", nrows=100)
				classes <- sapply(initial, class)
				tabAll <- read.table("datatable.txt", colClasses = classes)

* Set **nrows** : this doesn't make R faster, but if you can tell R how many rows there are in the date set this will help with memory usage. A mild overstimate is ok (R will read the correct number of rows anyway). You can use tool **wc** to calculate number of lines in a file

### How to calculate memory requirements

Ex. 1,5 millions of row and 120 columns. Suppose all columns are numeric. How many RAM it requires ?

1,500,000 x 120 x 8 bytes/numeric (8 bytes per numeric object)

= 1 440 000 000 bytes

= 1 440 000 000 bytes / 2^20 = 1,373.29 MB = 1.34 GB

The rule of thumb you'll need as mutch as the double of this RAM to read the table (it will take time, but you'll not run out of memory)

## Textual Data formats

**dumping** and **dputing** to save content in text file but not tables. This file contains more metadata

* Unlike tables or csv files , **dump** and **dput** preserve metadata (ex. classes of objects in the dataframe - sacrificing readability), so that another user doesn't have to specify it again

* Textual formats can work much better with version control programs like git which can only tracks changes meaningfully in text files

* Text file can be edited

* Text fle are easier to recover if partially corrupted

* Text formats adhere to "Unix philosophy"

* Downside : The format is not very space-efficient

**dput**  function take an R object and create some code that reconstruct the object in R


				> y <- data.frame(a = 1, b = "a")
				
Print the data frame y that I just created :

				> y
				  a b
				1 1 a
dput y to print in the stdout the structure (not very useful, only to see th code)

				> dput(y)
				structure(list(a = 1, b = structure(1L, .Label = "a", class = "factor")), .Names = c("a", 
				"b"), row.names = c(NA, -1L), class = "data.frame")

"Normal" use of dput : dput the object to a file:

				> dput(y, file = "y.R")
And then read it in R using dget
				> new.y <- dget("y.R")
				
				> new.y
				  a b
				1 1 a

The **dump** function is like the **dput** function but for multiple R objects, and you can read they back using **source**

Creating objects :

				> x <- "foo"
				> y <- data.frame(a = 1, b = "a")

Pass to **dump** a character vector with the name of the objects and the file where you want to store the objects in

				> dump(c("x", "y"), file = "data.R")	

you remove variables

				> rm(x, y)

Tou read the objects back with **source** and then print them

				> source("data.R")
				> y
				  a b
				1 1 a
				> x
				[1] "foo"
				> 

## Interface to the Outside World

To connect to files and in general to other things :

* **file** (normally text files)
* **gzfile** (gzip compressed files)
* **bzfile** (bzip2 compressed files)
* **url** (webpage)

must function (think at read.table for ex.) extablish this connection in background anyway...

				> str(file)
				function (description = "", open = "", blocking = TRUE, encoding = getOption("encoding"), 
					raw = FALSE, method = getOption("url.method", "default"))
					
What is important here :

**description** is the name of the file
**open** is a code indicating :

* *"r"* read only
* *"w"* writing and initializing the file
* *"a"* appending
* *"rb"*,*"wb"*,*"ab"* reading, writing, appending in binary mode (windows)

Other options are less important

Connections are powerful : they let you navigate to the file or external objects, but we often don't need to deal with the connection directly, ex :

				con <- file("foo.txt", "r")
				data <- read.csv(con)
				close(con)

is the same as

				data <- read.csv("foo.txt")

But sometimes is useful to read only a part of a file :

				con <- gzfile("words.gz")
				x <- readLines(con, 10)
				x

Use readLines to read elements from a page

				> con <- url("http://www.jhsph.edu", "r")
				> x <- readLines(con)
				> head(x)
				[1] "<!DOCTYPE html>"                                               
				[2] "<html lang=\"en\">"                                            
				[3] ""                                                              
				[4] "<head>"                                                        
				[5] "<meta charset=\"utf-8\" />"                                    
				[6] "<title>Johns Hopkins Bloomberg School of Public Health</title>"

## Subsetting Objects: Basics

Operators that can be used to extract subsets of R objects

* single bracket: **[** *always return an object of the same class as the original*; can be used to select more than on element (except on one case)

				> x <- c("a", "b", "c", "c", "d", "a")
				> x[1]
				[1] "a"
				> x[2]
				[1] "b"
				> x[1:4]
				[1] "a" "b" "c" "c"

you can use a numeric index, but also a logical index : in R we can use a logical operator even with letters! 

				> x[x > "a"]
				[1] "b" "c" "c" "d"

More complicated : we can even create a logical vector (u) and use it to subset x :

				> u <- x > "a"
				> u
				[1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE
				> x[u]
				[1] "b" "c" "c" "d"

* double bracket **[[** extract elements from a list or data frame; can only be used to extract *a single element* and the class returned will not be necessarily a list or data frame

* **$** extract elements of a list or data frame by **name**; semantics are similar to **[[** (may or maybe not the class of the object)

## Subsetting R Objects: Lists

you can use **[[** or **$** operators, or even  *[* operator

> x <- list(foo = 1:4, bar = 0.6)

single bracket return *always* an element with the same class of the original, so x[1] will be a list too with an element called "foo" which is a sequence of 1,2,3,4

> x[1]
$foo
[1] 1 2 3 4

if a use a double bracket I get back only a sequence (1,2,3,4). The difference with the first exemple : with signle bracket I get a list, with double bracket only the sequence.

> x[[1]]
[1] 1 2 3 4

Use of $ sign : 
> x$bar
[1] 0.6

and you can use in the same way the double bracket, with the same return (bar with quotes):

> x[["bar"]]
[1] 0.6

if I use the single bracket with a name, that gives me a list with the element bar in it:
> x["bar"]
$bar
[1] 0.6

The advantage of using the name is that I don't have to remember where the object is in the list

Attention : if you want to extract multiple elements from the list, you **have** to use the **single bracket operator** :

For exemple to extract the first et the third element from the list x, I have to pass a vector composed by the element 1 end 3 to the list in single bracket operator.

				> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
				> x[c(1,3)]
				$foo
				[1] 1 2 3 4

				$baz
				[1] "hello"


The difference betweenn the **[[** and **$** operators is that double bracket can be used with *computed* indices, and **$** only with *literal names*

				> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
				> name <-"foo"
				> x[[name]] ## computed index for 'foo'
				[1] 1 2 3 4
				> x$name ## element 'name' doesn't exist
				NULL
				> x$foo ## element 'foo' does exist
				[1] 1 2 3 4
				> 
HTe double bracket operator can take an integer sequence rather than single number.

In the exemple I have a list X that contain as a first element ("a") another list :

				> x <- list(a = list(10, 12, 14), b = c(3.14, 2.81))

If I want to extrat te number 14 (the third element of the first element) I can do :

				> x[[c(1, 3)]]
				[1] 14
				> x[[1]][[3]]
				[1] 14

or the first element of the second element :

				> x[[c(2,1)]]
				[1] 3.14
				> 

## Subsetting R Objects: Matrices

Matrices can be subsetted in the usual way :

				> x <- matrix(1:6, 2, 3)
				> x
					 [,1] [,2] [,3]
				[1,]    1    3    5
				[2,]    2    4    6

extract first line of second column

				> x[1,2]
				[1] 3

...or second line of the first column
				> x[2,1]
				[1] 2
An index can be omitted (extract a line - first index - or a column- second index - of the matrix)

				> x[1, ]
				[1] 1 3 5
				> x[, 2]
				[1] 3 4
By default, when a single element of a matrix is returned, is returned as a **vector of length 1** instead of a matrix 1x1. This behavior can be turned off setting **drop** to **FALSE**

				> x[1,2]
				[1] 3
				> x[1,2, drop=FALSE]
					 [,1]
				[1,]    3

Similarly of a subset of a **single row or column** : **you'll have a vector!**

				> x[1, ]
				[1] 1 3 5
				> x[1, ,drop=FALSE]
					 [,1] [,2] [,3]
				[1,]    1    3    5
				> 

## Subsetting R Objects - Partial Matching

Partial matching is a handy tool that can save a lot of typing, you can use it with **[[** or **$**

it works straightforward with the **$** operator : the $ sign can guess the partial match

				> x <- list(aardvark = 1:5)
				> x$a
				[1] 1 2 3 4 5

But this doesn't work directly with the **[[** sign, because it expects an exact match, so you have to specify the **exact = FALSE** parameter :

				> x[[a]]
				Erreur : objet 'a' introuvable
				> x[["a"]]
				NULL
				> x[["a", exact = FALSE]]
				[1] 1 2 3 4 5
				> 

## Subsetting - Removing Missing Values

Removing NA Values is a common task. You can do it creating a logical vector (bad) with **is.na** function : that tell which are missing, so you have to negate it

				> x <- c(1, 2, NA, 4, NA, 5)
				> bad <- is.na(x)
				> bad
				[1] FALSE FALSE  TRUE FALSE  TRUE FALSE
				> x[!bad]
				[1] 1 2 4 5
				> 

If there are multiple objects and you want to take subset with no missing values.

				> x <- c(1, 2, NA, 4, NA, 5)
				> y <- c("a", "b", NA, "d", NA, "f")
				> good <- complete.cases(x, y)
				> good
				[1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE
				> x[good]
				[1] 1 2 4 5
				> y[good]
				[1] "a" "b" "d" "f"

 >> Be careful, two objects have to have THE SAME LENGTH and the same "values"!!
 
				>x=c(1,2,3,4,NA)
				> complete.cases(x)
				[1] TRUE TRUE TRUE TRUE FALSE
				> y=c(1,2,3,4,5,NA)
				> complete.cases(y)
				[1] TRUE TRUE TRUE TRUE TRUE FALSE
				> complete.cases(x,y)
				Error in complete.cases(x, y) : not all arguments have the same length
				>k=c(1,2,3,4,NA)
				> l=c(1,2,3,NA,5)
				> complete.cases(k,l)
				[1] TRUE TRUE TRUE FALSE FALSE

You can also use **complete.cases** to remove Na from Data Frames (Matrix) :

				> airquality[1:6, ]
				  Ozone Solar.R Wind Temp Month Day
				1    41     190  7.4   67     5   1
				2    36     118  8.0   72     5   2
				3    12     149 12.6   74     5   3
				4    18     313 11.5   62     5   4
				5    NA     NA  14.3   56     5   5
				6    28     NA  14.9   66     5   6

So I can create a logical vector **good** that tells me which rows are complete :
				> good <- complete.cases(airquality)

Then I subset the airquality matrix and I see all the rows with missing values are gone :

				> airquality[good, ][1:6, ]
				  Ozone Solar.R Wind Temp Month Day
				1    41     190  7.4   67     5   1
				2    36     118  8.0   72     5   2
				3    12     149 12.6   74     5   3
				4    18     313 11.5   62     5   4
				7    23     299  8.6   65     5   7

## Vectorized Operations

Vectorized operationsa are on of the features that makes R easy to use on command line. The idea with *vectorized operations* is that things can happen in parallel when you do operations

				> x <- 1:4; y <- 6:9

If I add the two vectors, I add in fact each element with the correspondent of the other vector : the first element of X with the first of Y, the second of x with the second of y etc. : so this happens in parallel without need of loops like in other languages

				> x + y
				[1]  7  9 11 13

Similarly you can use < or == etc. to build logical vectors :

				> x > 2
				[1] FALSE FALSE  TRUE  TRUE
				> y == 8
				[1] FALSE FALSE  TRUE FALSE
				> x >= 2
				[1] FALSE  TRUE  TRUE  TRUE

You can also * / - etc. etc. in parallel 

> x * y
[1]  6 14 24 36
> x / y
[1] 0.1666667 0.2857143 0.3750000 0.4444444

Similarly you can do it with matrices

> x <- matrix(1:4, 2, 2); y <- matrix(rep(10, 4), 2, 2)
> y
     [,1] [,2]
[1,]   10   10
[2,]   10   10
> x
     [,1] [,2]
[1,]    1    3
[2,]    2    4

So you can do an "element wise" multiplication (element 1,1 of x is multiplicated with the 1,1 of y etc.), **not a matrix multiplication**  :

> x * y
     [,1] [,2]
[1,]   10   30
[2,]   20   40

The same with division :

> x / y
     [,1] [,2]
[1,]  0.1  0.3
[2,]  0.2  0.4

If you want to do a **true** matrix multiplication you have to do the **%*%** symbol

> x %*% y
     [,1] [,2]
[1,]   40   40
[2,]   60   60