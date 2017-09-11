## Intro

Link to pre-Quiz : https://github.com/rdpeng/practice_assignment/blob/master/practice_assignment.rmd

## Control - Structures

Normally not used in interactive session, but rather when wrting a program or longer expressions. Foir interactive sessions, loops can be executed with function that ends with "apply"

* **if, else** testing condition

* **for** execute a loop fixed number of times

* **while** execute a loop *while* a condition is true

* **repeat** execute an infinite loop

* **break** break the execution of a loop

* **next** skip an iteration of a loop

* **return** exit a function

## Control Structures: If-Else

There's different valid if/else structure in R (differently from other languages you can have different "type" of if/else valid structures)

				if(x>3) {
						y <- 10
				} else {
						y <-0
				}

and also

				y <- if(x >3) {
						10
				} else {
						0
				}

you can of course use only if condition without else and test multiple if condition in a row

				if(<condition>) {

				}

				if(<condition>) {

				}

## Control Structure: For Loops

exemple :

				for(i in 1:10) {
						print(1)
				}

three exmples of for loops for same behavior

				x  <- c("a", "b", "c", "d")

First exemple, a loop in "C Style"

				for(i in 1:4) {
						print(x[i])
				}

Exemple 2 : I can use the seq_along that take a vector as argument and produce an integer sequence equal to the length of the vector : 

				for(i in seq_along(x)) {
						print(x[i])
				}

Exemple 3 : I use a different variable (I call it letter) that is going to take values from the vector itself: there's no reason why the index variable has to be an integer, it can take elements from any arbitrary vector : in this case I'm printng out the index variable, which happens to be equal to the letters in the vector :

				for(letter in x) {
						print(letter)
				}

Exemple 4 : is the same as exemple 1 : if the loop expression has only an element in his body, you can omit the curly braces, it's compact, but it's less readable

				for(i in 1:4) print(x[i])

You can nest for loops, for exemple for matrix with two dimensions, but be careful of readability with loops beyond 2-3 levels! Exemple (seq_len generate a sequence of from a single integer )

				x <- matrix(1:6, 2, 3)

				for(i in seq_len(nrow(x))) {
						for(j in seq_len(ncol(x))) {
								print(x[i,j])
						}
				}


## Control Structures - While Loops
				
While loops begin by testing a condition, if true they execute the loop body (and the condition is tested again etc.). Be careful of infinite loops.. sometimes is better to use for loops :) !

count <- 0

while(count < 10) {
		print(count)
		count <- count + 1
}

you can test more than one condition ex :

z <- 5

while(z >= 3 && z <= 10) {
		print(z)
		coint <- rbinom(1, 1, 0.5)
		
		if(coin ==1) { ## random walk
				z <- z + 1
		} else {
				z <- z -1
		}
}

## Control Structures - Repeat, Next & Break

*Repeat* initiates an infinite loop, the only way to exit is to call a *break*

x0 <- 1
tol <- le-8

repeat {
		x1 <- computeEstimate() ## computeEstimate is not a real function
		
		if(abs(x1 - x0) < tol) {
				break
		} else {
				x0 <- x1
		}
}

Be careful : there's not guarantee that this loop doesn't run forever... Better maybe to use a for loop...

*Next* is used in any loop to skip an iteration

				for(i in 1:100) {
						if(i <= 20) {
						##Skip the first 20 iteration
						next
						}
						##Do something there
				}

*return* signal that the ENTIRE function should exit and return a given value

## Your first R function

you write this in a "new script" in R studio

				add2 <- function(x, y) {
						x + y
				}

You don't have to add "return" because there's only one expression, and by default a R function return whtever the last expression was.

Than you can test your function by

1. You select the whole function

2. You click on "run" button to run the function in the R studio console

3. you test you function with this code

				add2(3,5)

and you'll have

				add2(3,5)
				[1] 8

And that's all

The next function is a little more complicated : it takes a vector as argument and return a subset of the vector higher than 10 (if there's no element in the vector higher than 10, the function will return an empty numerical vector):

				above10 <- function(x) {
						use <- x > 10
						x[use]
				}

And more : you can create a function that let the user specify which number has to test the vector (n is any number):

				above <- function(x, n) {
				  use <- x > n
				  x[use]
				}

So i run the function in R and I create the vector x and test the new function "above" :

				> x <- 1:20
				> above(x, 12)
				[1] 13 14 15 16 17 18 19 20

But maybe the 10 number is very special, so you want to specify it by default for n, and you'll have this function :

				above <- function(x, n = 10) {
				  use <- x > n
				  x[use]
				}

so if you run simply **above(x)** (without specifing n) you'll not get an error

				> above(x)
				 [1] 11 12 13 14 15 16 17 18 19 20

 The next function take a data frame and calculate the mean of each column :
 
 columnmean <- function(y) {
        nc <- ncol(y) ## calculate the number fo columns of the data frame
        means <- numeric(nc) ## create a numeric empy vector of length of the number of columns, to store the mean of each column
        for(i in 1:nc) {
          means[i] <- mean(y[, i]) ## loop through the column, calculate the mean, and store it in the means vector
        }
          means ## returns the means vector
}

and then test it on the airquality dataset :

				> airquality <- read.csv("hw1_data.csv")
				> columnmean(airquality)
				[1]        NA        NA  9.957516  77.882353  6.993464 15.803922

The first 2 columns you have NA, because you have some NA in those columns and you can't calculate the mean

So you have to throw out the NA by default from the function :

				columnmean <- function(y, removeNA = TRUE) {
						nc <- ncol(y) 
						means <- numeric(nc)  
						for(i in 1:nc) {
						  means[i] <- mean(y[, i], na.rm = removeNA) 
						}
						  means 
				}

And now by default you have the means without NA :

				> columnmean(airquality)
				[1]  42.129310 185.931507   9.957516   77.882353   6.993464  15.803922

But you can still have the old behavior if you want :

> columnmean(airquality, removeNA = FALSE)
[1]        NA        NA  9.957516   77.882353  6.993464 15.803922

Be careful **you have to save your file to avoid to loss the functions** (with .r extension)

## R functions

functions are created with **function()** directive and are stored as R objects like anything else, they are R objects of class "function" and they are "first class objects" that can be treated as any other R object :

* functions can be passed as arguments to other functions
* functions can be nested

functions have *name arguments* which can have potentially *default values*

* *formal arguments* are arguments included in the function definition

*  the *formals* function takes a function as arguments and list all the formal arguments of the function

* Not every function use all the formal arguments : you don't have necessarily specify all arguments : you can choose to not specify some, or some can have a default value

* functions arguments can be matched by **position** or by **name**, so the following call to the function **sd()** are the same :

				> mydata <- rnorm(100)
				> sd(mydata)

in this exemple i named the argument (or both arguments) :

				> sd(x = mydata)
				> s(x = mydata, na.rm = FALSE)

when I name the argument I can pass them in the order I want :
				> sd(na.rm = FALSE, x = mydata)

You can mix positional matching and matching by name : when an argument is matched by name, it is "taken out" of the argument list and the remaining unnamed arguments are matched in the order they are listed in the function definition.

For the **lm ()** function for exemple :

				> args(lm)
				function (formula, data, subset, weights, na.action, method = "qr", 
					model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE, 
					contrasts = NULL, offset, ...) 
	
the following exemple are equivalent; yyou :

				> lm(data = mydata, y - x, model = FALSE, 1:100)
				> lm(y - x, mydata, 1:100, model = FALSE)

even though it's legal, **it's better to not mess around with arguments order, because it can lead to confusion**

Most of the time, named arguments are useful on the command line when you have a long argument list and you want to use the default except ofr an argument near the end of the list

Named arguments can also help to remember the name of the argument and not its position

Functions can also be *partially* matched (part of the argument name) > useful for interactive work. The order operations when given an argument is :

1. Check for exact match for a named argument

2. Check for a partial match

3. Check for a positional match

### Defining a function

a doesn't have a default value, the other all have a default value, you can set also an argument value to NULL.

f <- function(a, b = 1, c = 2, d = NULL) {
  
}

One of the key element of R language is called *lazy evaluation* : the arguments are evaluated only when needed - exemple :

				> f <- function(a, b) {
				+   a^2
				+ }
				> f(2)
				[1] 4
				> 

The function never use the argument b, so there's no error message. Look at this also (exemple are mine, not in the video) if a = 2, the expression b is never evaluated :

				> f <- function(a, b) {
				+   
				+ if(a == 2) {
				+   a^2
				+ }
				+   else {
				+     a+b
				+   }
				+ }
				> f(2)
				[1] 4
				> f(3)
				Error in f(3) : argument "b" is missing, with no default
				> f(3,1)
				[1] 4
				> 

Another exemple :

				> f <- function(a, b) {
				+   print(a)
				+   print(b)
				+ }
				> f(45)
				[1] 45
				Error in print(b) : argument "b" is missing, with no default
				> 

Because the argument is evaluated only when need, the *a* value (45) is printed correctly, but the function gives an error when b as to be evaluated because is needed.

### The "..." Argument

The "..." argument indicate a variable nimber of arguments that are usually passed on to other functions. Often used when extending another function, exemple, you want to extend the **plot()** function to tweak it and modify some default values etc. so you create a new function called **myplot()** that replicate some arguments of the arguments of myplot - x, y - but it's gonna change the defualt *type* argument ... But the plot function as many more arguments and you want to let them the same, so you can pass "..." and all original arguments can be preserved and don't need to be retyped:

				> myplot <- function(x, y, type = "l", ...) {
						plot(x, y, type = type, ...)
				}

There's another use of the "..." argument, to pass extra arguments to methods (we'll talk about it later) in **generic functions** (functions that don't do anything but passing data to methods)

				> mean
				function (x, ...)
				UseMethod("mean")

The "..." Argument is also necessary when the number of arguments passed to a function cannot be known in advance :

				> args(paste)
				function (..., sep = " ", collapse = NULL) 
 

The paste function take different strings as arguments to concatenate them in a vector of strings, and he can take a variable number of arguments. So there's no way to know in advance how many strings you have to concatenate,so the first argument of paste is "..."

				> args(cat)
				function (..., file = "", sep = " ", fill = FALSE, labels = NULL, 
					append = FALSE) 
	
cat is the same: it puts together different strings and print them either to a file or the console

One catch with "..." : every argument that appear AFTER the "..." on the argument list **MUST BE NAMED EXPLICITELY AND CANNOT BE PARTIALLY MATCHED** :

				> paste("a", "b", sep=":")
				[1] "a:b"
				> paste("a", "b", se=":")
				[1] "a b :"

## Scopin Rules - Symbol Binding

How does R know which value to assign to which symbol?

So the lm function that I create takes an "x" arguments and return the multiplication of x by itself :

				> lm <- function(x) { x * x }

But there's already a function in R called lm, so when I call lm, how R knows to which value to assign to lm (the lm "standard" function or the new one created ?

				> lm
				function(x) { x * x }

how does R know what value to assign to the symbol lm? Why doesn't it give it the value of **lm** that is in the **stats** package ?

When R tries to assign a value to a symbol it search through a series of  **environments** to find the proper value. When you are working on the command line, you have to :

1. Search the global enivonment for a symbol name matching the one requested

2. Search the namespaces of each package on the search list (retrieved with with the **search()** function)

				> search()
				[1] ".GlobalEnv"        "package:stats"     "package:graphics" 
				[4] "package:grDevices" "package:utils"     "package:datasets" 
				[7] "package:methods"   "Autoloads"         "package:base"    

in the search list you have (always) in first the global envoironment. Somewhere in this list, R looks for the function until the end of the list so:

* The global environment or the user's workspace is always the first element of the search list, and the **base** package is always the last. Order of the packages matters!

* User's can configure which pacjages get loaded on startup so you cannot assule there will be a set list of packages available

* when a user loads a package with the **library()** function, the namespace of that package gets put in postion 2 of the search list (by default) and everything shifts down

* R has separate namespace for functions and non-functions so it's possible to have an object called c and a function called c

### Scoping rules

Scoping rules determine how a value is binded with a free variable in a function

it's the main feature that make R different from S

* R uses *lexical scoping* or *static scoping* (a common alternative is dynamic scoping)

* Related to the scoping is how uses the search list to bind a value to a symbol

* Lexical scoping turns out to be very useful for simplyfy statisticals computations

## Lexical scoping

Exemple of free variable : a variable

				f <- function(x, y) {
						x^2 + y / z
				}

z is a free variable : it's in the body of the function but it's not a *formal argument* neither a local variable (declared inside the function body) : to find what z is, R use the (lexical) scoping rules.

Lexical scoping > **the values of free variabbles are searched in the environment in which the function was defined**

What's an environment?

* **a collection of (symbol, value) pairs** : ex. x is a symbol and 3.14 might be its value
* Every environment has **a parent environment**. An environment can have **multiple children**
* The only environment without a parent is the **empty environment** (after the base package)
* function + an environment = a *closure* or *function closure*

How R search for a free variable :

* If the value is not found in the environment of the function (if I define a function in the global environment, R will search first there), then R search in the parent environment

* Search continues until the top-level environment (usually the global environment, if the function is defined in a package, it'll be the namespace of the package)

* After the top-level environment the search continue down the search list until we hit the empty environment. If the value cannot be found, an error is thrown

Why does all this matter ?

* Tipically a function is defined in the global environment, so the values of the free variables are just found in the user's workspace

* However **you can have function defined inside other functions **(language C don't allow this)

Now things get interesting >> the environment in which a function is defined is the body of another function!

				make.power <- function(n) {
				  pow <- function(x) {
					x^n
				  }
				  pow
				}

make.power return the function pow, a function defined inside it, Inside pow *n* is a free variable not defined inside pow; however *n* is defined inside make.power function, so pow will find the variable n inside this environment.

So I can call make.power and return the function to an another function cube or square. So i created a function that can create differents behaviors :

				> cube <- make.power(3)
				> square <- make.power(2)
				> cube(3)
				[1] 27
				> square(3)
				[1] 9

What's in a function environment ?

I call the environment function

				> ls(environment(cube))
				[1] "n"   "pow"

And I see an object called "n", so I try to get n :

				> get("n", environment(cube))
				[1] 3

The same with "square" :

> ls(environment(square))
[1] "n"   "pow"
> get("n", environment(square))
[1] 2

### Lexical vs Dynamic Scoping

				> y <- 10

				f <- function(x) {
				   y <- 2
				   y^2 + g(x)
				   
				 }


				 g <- function(x) {
				   x * y
				 }

> f(3)
[1] 34

With lexical scoping, the value y in function g is looked up in the environment in which the function is defined: the global environment, so the value of y is 10

With dynamic scoping, the value of y is lloked up in the environment from which the function was called (referred sometimes as *calling environment* and known in R as *parent frame*) > so the value of y would be 2

The two type of scope are  identicals when the function is both defined and called in the global environment, then the defining and the calling environment are the same.

Other languages that support Lexical Scoping :

* Scheme
* Perl
* Python
* Common Lisp

### Consequences of Lexical Scoping

* All R objects must be stored in memory

* All functions must carry a pointer to their respective environment

* in S-PLUS free variable are always looked up in the global workspace, so everything can be stored on disk because the deifning environment is always de same

## Application: Optimisation

* Optimization routines in R like *optim*, *nlm* and *optimize* require to pass a function whose argument is a vector of parameters

* However, an object function might depend on a host of other things besides its parameters (like *data*)

* When writing a software which does optimisation, it may be desirable to allow the user to hold certain parameters fixed

The basic idea with any optimisation problem in R is to create a "contructor" function which construct the objective function, and have all data etc. that it depends on would be "included" in the defining environment of that function, in its enclosing environment so you don't have to specify it every time you call the function

				make.NegLogLik <- function(data, fixed=c(FALSE,FALSE)) {
				  params <- fixed
				  function(p) {
					  params[!fixed] <- p
					  mu <- params[1]
					  sigma <- params[2]
					  a <- -0.5*length(data)*log(2*pi*sigma^2)
					  b <- -0.5*sum((data-mu)^2) / (sigma^2)
					  -(a+b)
				  }
				  
				}

Note: Optimisation functions in R minimize function, so you need to use the negative log-likelihood to the normal distribution

> set.seed(1); normals <- rnorm(100, 1, 2)
> normals
> nLL <- make.NegLogLik(normals)
> nLL
function(p) {
      params[!fixed] <- p
      mu <- params[1]
      sigma <- params[2]
      a <- -0.5*length(data)*log(2*pi*sigma^2)
      b <- -0.5*sum((data-mu)^2) / (sigma^2)
      -(a+b)
  }
<bytecode: 0x0000000018ac1310>
<environment: 0x0000000018425980>
> ls(environment(nLL))
[1] "data"   "fixed"  "params"

the <environment> code is because you're not in the global environment, and is the hexadecimal pointer to the memory sector to this function

Everything in the nLL function comes from local variables or comes from the parameter vector p, however, the data variable is not an argument of the function, it's not a local variable, so it's a free variable, but the data comes from the NegLogLik constructor function which originally passed the data to that. So the data can looked up to the environment that the function is defined and it knows what the data are, you don't have to specify, it's already fixed in the function and you can see it by calling ls + environment : the result is that all the three are free variables but are specified in the defining environment.

...

## Coding Standards

Minimal code standards for R, to make more readable code :

1. Always use text files / text editor

2. Indent your code (4 spaces minimum, 8 spaces ideal)

3. Limit the width of your code (80 columns is good limit)

4. Limit the length of individual functions (split function for do only 1 operation) it's better to read and to debug

## Dates and Times in Related

* Dates are represented by the **Date** class
* Times are represented by the POSIXct ot POSIXlt class
* Dates are stored internally as the number of days since 1970-01-01
* Dates are stored internally as the number of seconds since 1970-01-01

Dates are represented by the date class and can be coerced from a character string with the **as.Date()** function

				> x <- as.Date("1970-01-01")
				> class(x)
				[1] "Date"
				> x
				[1] "1970-01-01"

But they are stored as number of days since 1970-01-01, and if I unclass x, I can see that 1970, January 01 is stored as 0 and January 2 as 1 etc.

				> unclass(x)
				[1] 0
				> unclass(as.Date("1970-01-02"))
				[1] 1
				> unclass(as.Date("1977-12-22"))
				[1] 2912

### Times in R

Times are represented by the POSIXct ot POSIXlt class

POSIXct is a large integer under the hood

POSIXLt is a list undenrath and is stores a bunch of other informations like the day of the week, day of the year, day of the month

Functions that work on dates and times

* **weekdays** (give the day of the week)
* **months** (give the month name)
* **quarters** (give the quarter number : Q1, Q2, Q3 or Q4)

**Times** can be coerced from a character string using **as.POSIXlt()** or **as.POSIXct()** function :

> x <- Sys.time()
> x ## Already in POSIXCt
[1] "2017-09-05 16:20:44 CEST"
> p <- as.POSIXlt(x)
> names(unclass(p))
 [1] "sec"    "min"    "hour"   "mday"   "mon"    "year"   "wday"   "yday"   "isdst"  "zone"   "gmtoff"
> p$sec
[1] 44.99964
> p$yday
[1] 247

You can also use POSIXct format (you can see the long integer stored in x), but if you want to have the list of elements you have to convert to POSIXlt format :

> unclass(x)
[1] 1504621245
> x$sec
Error in x$sec : $ operator is invalid for atomic vectors
> p <- as.POSIXlt(x)
> p$sec
[1] 44.99964

Finally, you can use the **strptime()** function if the dates are written in different format

> datestring <- c("January 10, 2012 10:40", "December 9, 2011 9:10")

If I want to convert this strings to a date object I pass to strptime a the character vector *datestring* and the format string

				> x <- strptime(datestring, "%B %d, %Y %H:%M")
				> x
				[1] "2012-01-10 10:40:00 CET" "2011-12-09 09:10:00 CET"


Attention: normally this output NA NA on french system, you have to change the name of the months in french or use this twik described here : https://www.coursera.org/learn/r-programming/discussions/all/threads/fQ7wgFN8EeaWYQrYt-DiIQ

				> class(x)
				[1] "POSIXlt" "POSIXt" 

for en exact list of format parameters check **?strptime**

You can do maths on dates and times (her, just + or -) and comparisons (ex. ==, <=)

				> x <- as.Date("2012-01-01") 
				> y <- strptime("9 Jan 2011 10:40:33", "%d %b %Y %H:%M:%S")
				> x-y
				Error in x - y : non-numeric argument to binary operator
				In addition: Warning message:
				Incompatible methods ("-.Date", "-.POSIXt") for "-" 

you get an error because they are different type of objects (x is a date object and y is a POSIXlt object) so you have to convert the date to a POSIXlt object:

				> x <- as.POSIXlt(x)
				> x-y
				Time difference of 356.5968 days

you can do tricky things :

> x <- as.Date("2012-03-01")
> y <- as.Date("2012-02-28")
> x-y
Time difference of 2 days (it calculate the 29 because of the 2012)
> x <- as.POSIXlt("2012-10-25 01:00:00")
> x <- as.POSIXlt("2012-10-25 06:00:00", tz = "GMT")
> x <- as.POSIXlt("2012-10-25 01:00:00")
> y <- as.POSIXlt("2012-10-25 06:00:00", tz = "GMT")
> y-x
Time difference of 7 hours





