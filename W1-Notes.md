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