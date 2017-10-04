## Loop functions - lapply

Special functions allows to do loops but in a more compact way, to make it easier when you work on command line :

* **lapply** loop over a list and evaluate a function on each element

* **sapply** same as **lapply** but try to simplify the result

* **apply** apply a function over the margins of an array

* **tapply** Apply a function over subsets of vector

* **mapply** Multivariate version of **lapply**

an ausiliary function, **split** is useful, particulary in conjunction with **lapply**

--

**lapply** takes 3 arguments: 1) a list **x** 2) a function **FUN** or the name of the function 3) other arguments via its **...** argument. If *x* is not a list it will be coerced to a list with **as.list** (if it's not possible you get an error)

				> lapply
				function (X, FUN, ...) 
				{
					FUN <- match.fun(FUN)
					if (!is.vector(X) || is.object(X)) 
						X <- as.list(X)
					.Internal(lapply(X, FUN))
				}

So, the idea behind **lapply** is that it takes a list (a list can contain every tipe of objects: vector, matrices, data frames etc.) and it apply a function on each one of element of this list, and it return something for every single objects of this list and the new values will be assembled in a new list that lapply will return (lapply always return a list!!)

				> x <- list( a= 1:5, b = rnorm(10))
				> lapply(x, mean)
				$a
				[1] 3

				$b
				[1] 0.02279042

So the return list as two elements (like the list in the argument) with the same name, that are preserved, and lapply apply a 'mean' on every element of the list...

				> x <- list( a= 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
				> lapply(x, mean)
				$a
				[1] 2.5

				$b
				[1] 0.04695532

				$c
				[1] 0.7904647

				$d
				[1] 4.945883


another use of lapply : runif generate x random number. So if i use lapply with w = a vector 1:4, this is the result:

				> x <- 1:4
				> lapply(x, runif)
				[[1]]
				[1] 0.2859701

				[[2]]
				[1] 0.92427655 0.02767628

				[[3]]
				[1] 0.1513856 0.5750131 0.5319630

				[[4]]
				[1] 0.32628893 0.83494112 0.88911177 0.01675035

runif has differents arguments; by default it generates random number between 0 and 1, but if I want to generate nulbers, for exemples, between 0 and 10, I can pass to runif the min and max argument *via* lapply throw the ... argument :

				> lapply(x, runif, min = 0, max = 10)
				[[1]]
				[1] 6.403933

				[[2]]
				[1] 1.059231 1.633575

				[[3]]
				[1] 0.8309150 1.7896734 0.4676352

				[[4]]
				[1] 7.90128901 0.55304310 0.08683249 7.60093388

lapply make heavy use of *anonymous functions* (functions don't have names but you can generate on the fly)

ex : I create a list with 2 matrices

				> x <- list(a = matrix(1:4, 2, 2), b = matrix(1:6, 3, 2))
				> x
				$a
					 [,1] [,2]
				[1,]    1    3
				[2,]    2    4

				$b
					 [,1] [,2]
				[1,]    1    4
				[2,]    2    5
				[3,]    3    6

Imagine I want to extract the first column of everyone of this matrices, O wrote an anonymous function that doesn't exists outside this context...(à creuser) :

> lapply(x, function(elt) elt[, 1])
$a
[1] 1 2

$b
[1] 1 2 3

**sapply** try to simplify the result of **apply** if possible :

* if the result is a list with every element of length 1, **sapply** return a vector

* if the result is a list where every element is a vector length>1, a mtrix is returned

* if it can't figure things out, a list is returned

for exemple, of this list as seen before for lapply :

				> x <- list( a= 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
				> lapply(x, mean)
				a         b         c         d 
				2.500000 -0.366536  1.109431  4.972739 
				
I get back a vector

AND if I try to use mean() with x directly, it's not going to work, because mean doesn't work with lists.....!!!

				> mean(x)
				[1] NA
				Warning message:
				In mean.default(x) :
				  l'argument n'est ni numérique, ni logique : renvoi de NA

## Loop functions - Apply

**apply** evaluate a function (often an anonymous function) over the margins of an array

* most often used to apply a function to rows or columns of a matrix

* it can be used with general arrays (ex. taking the average of an array of matrices)

* it is not really faster than writing  loop, but it works in one line!

				> str(apply)
				function (X, MARGIN, FUN, ...) 

* x is an array (array = data objects which can store data in more than 2 dimensions)

* MARGIN is an integer vector indicating which margins should be "retained"

* FUN is the function to be applied

* ... other arguments passed to FUN

				> x <- matrix(rnorm(200), 20, 10)
				> apply(x, 2, mean)
				 [1] -0.294726472  0.337253008  0.169842847 -0.045587223  0.068459260
				 [6] -0.279709378 -0.281607844  0.004227838  0.060145350 -0.383763696
				> apply(x, 1, mean)
				 [1] -0.05984765  0.39505997 -0.27920697 -0.17974707  0.05874675 -0.12790028
				 [7]  0.27266142  0.09105029 -0.38223342 -0.48782958  0.02694025 -0.23771387
				[13] -0.35715612  0.08619175 -0.03342761 -0.12905031  0.16808836 -0.78120802
				[19]  0.40071333  0.26493617

what does it mean the second argument ??? The "2" take the second dimension of the array (the column) and so do the mean by columns (so we have 20 means), the "1" take the first dimension of the array (the rows) and so do the mean by rows (so we have 10 means)

for sums and means of matrix dimensions we have actually some highly optimized special functions (much faster with large matrices) :

rowSums = apply(x, 1, sum)
rowMeans = apply(x, 1, mean)
colSums = apply(x, 2, sum)
colMeans = apply(x, 2, mean)

Others applications of **apply** :

Quantiles!

				> x <- matrix(rnorm(200), 20, 10)
				ly(x, 1, quantile, probs = c(0.25, 0.75))
						  [,1]       [,2]      [,3]       [,4]      [,5]       [,6]
				25% -0.7110620 -0.4981837 -1.013390 -0.7544172 -0.917481 -0.6584148
				75%  0.3471781  1.1137895  0.578661  0.2918187  1.165972  0.7540952
							[,7]       [,8]        [,9]      [,10]      [,11]      [,12]
				25% -0.005430289 -0.3009462 -0.75364800 -1.3628624 -0.4011382 -0.7235545
				75%  0.527974769  0.9988044 -0.09927262  0.1434791  0.5362899  0.1619981
						  [,13]      [,14]      [,15]      [,16]      [,17]       [,18]
				25% -0.53165653 -0.2804947 -1.0075818 -0.6528386 -0.1883669 -1.70303244
				75% -0.01749251  0.9079642  0.7392774  0.5795829  0.6420148 -0.06710096
						[,19]     [,20]
				25% 0.0904943 0.1208228
				75% 0.5821884 0.6745126

So for each rows it calculate the quantiles 25 and 75 as asked in "..."

another exemple with a 3 dimensional matrix :

				> a <- array(rnorm(2 * 2 * 10), c(2, 2, 10))

So here we can think of a bunch of 2x2 matrix stucked together, so if I want to have the mean of the 2x2 matrices "collapsing" the third dimension I can use the formula :

				> apply(a, c(1, 2), mean)
						   [,1]      [,2]
				[1,] -0.3138913 0.2584813
				[2,]  0.2973663 0.5178516

I can use also rowMeans :

				> rowMeans(a, dims = 2)
				[,1]      [,2]
				[1,] -0.3138913 0.2584813
				[2,]  0.2973663 0.5178516


## Loop functions - mapply

is a multivariate apply : it applies a function in parallel over a set of arguments

function (FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE, USE.NAMES = TRUE)

* FUN is the function to apply
* ... contains arguments to apply over
* MoreArgs is a list of other arguments of FUN
* SIMPLIFY indicates whether the result should be simplified

mapply comes useful when you have two lists, for exemple, and you want to apply a function to that lists: you can't use lapply or apply, you use mapply

>> the number of the arguments of the function FUN that you pass to mapply has to be at least as many as the number of the lists that you're going to pass to mapply : if you have three lists, you'll pass three objects and your function has to take at least three arguments to it.

Exemples :

				> list(rep(1, 4), rep(2, 3), rep(3, 2), rep(4, 1))
				[[1]]
				[1] 1 1 1 1

				[[2]]
				[1] 2 2 2

				[[3]]
				[1] 3 3

				[[4]]
				[1] 4

but you can do also :

				> mapply(rep, 1:4, 4:1)
				[[1]]
				[1] 1 1 1 1

				[[2]]
				[1] 2 2 2

				[[3]]
				[1] 3 3

				[[4]]
				[1] 4

Another exemple, a function noise that take a number (n), the mean and the sd

				> noise <- function(n, mean, sd) {rnorm(n, mean, sd) }
				> noise(5, 1, 2)
				[1]  4.0662119 -0.5828027 -0.8220997  0.8094648  1.7379961
				> noise(1:5, 1:5, 2)
				[1] 2.111546 1.872773 3.069327 4.495785 3.697607

This function doesn't work correctly: I want to have a rnorm with mean 1, 2 rnorm with mean 2 etc.

So I will mapply noise, fixing the sd (2), but changing n and the mean : so **I can vectorize a function that doesn't accept vector input!!!**

> mapply(noise, 1:5, 1:5, 2)
[[1]]
[1] 0.8296929

[[2]]
[1] 4.701975 1.430505

[[3]]
[1]  6.0561328 -0.2024243  2.9560954

[[4]]
[1] 5.5428968 3.6877806 0.7353214 5.2011314

[[5]]
[1] 4.948465 6.391932 6.458718 5.959012 6.398319

this is in fact the same that :

				> list(noise(1, 1, 2), noise(2, 2, 2), noise(3, 3, 2), noise(4, 4, 2), noise(5, 5, 2))
				
## Loop function - tapply

Used to apply a function over subsets of a vector. I don't know why is called **tapply** (!!!)

				> str(tapply)
				function (X, INDEX, FUN = NULL, ..., default = NA, simplify = TRUE)
* x is a vector				
* INDEX is a factor or a list of factors (or else they are coerced to factors)
* FUN is the function to apply (or anonymous function)
* ... contains other arguments to be passed to FUN
* simplify as sapply to simplify the function

So the first argument is a numeric vector or a vector of some sort, the second argument is also a vector of the same length which identifies which group each element of the numeric vector is in : so if you have two groups, men, women, you need to have a factor (INDEX) variable that identifies which observations is men and which women...

Exemple : we have a vector x with 30 numbers random generated, and the factor f have 3 levels (1, 2, 3). In the tapply function, we use the factor to categorize the elements of the vector x, so when we tapply the function "mean" it makes a mean for each group separately

> x <- c(rnorm(10), runif(10), rnorm(10, 1))
> f <- gl(3, 10)
> f
 [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
Levels: 1 2 3
> tapply(x, f, mean)
         1          2          3 
-0.6066884  0.6107642  0.8271096

If you don't simplify the list, you'll get back a list :

				> tapply(x, f, mean, simplify = F)
				$`1`
				[1] -0.6066884

				$`2`
				[1] 0.6107642

				$`3`
				[1] 0.8271096

Another exemple: I can use tapply to calculate the range, that give back a couple a list of vector of length 2 :

> tapply(x, f, range)
$`1`
[1] -1.7677143  0.9704971

$`2`
[1] 0.1950644 0.9618710

$`3`
[1] -0.4812468  2.4168140

## Loop functions - split

**split** splits up a vector into smaller pieces and apply functions to this smaller pieces

> str(split)
function (x, f, drop = FALSE, ...)

X > is a vector, list or data frames
f > factor (or coerced to one) or a list of factors
drop > indicates whether empty factors levels should be dropped

so if f has 3 levels, split will divide x in 3 groups. And then you can use lapply or sapply etc. to apply a function to thoses individual groups. Split always return back a list

				> x <- c(rnorm(10), runif(10), rnorm(10, 1))
				> f <- gl(3, 10)

				> split(x, f)
				$`1`
				 [1] -1.27633817 -0.69692903 -1.22008554 -0.59121906 -0.54973350 -0.88801557
				 [7] -0.09855905  1.12363822  0.60483117 -0.22861434

				$`2`
				 [1] 0.24541510 0.02742553 0.29684039 0.76878569 0.16947367 0.44258834
				 [7] 0.75667411 0.22323178 0.24189679 0.33044297

				$`3`
				 [1] -1.35082121  0.02488838  0.20148843  1.43689784  1.49750496  2.56366727
				 [7]  1.56711195  0.30777929  1.02381657  1.69157036

It's common to use lapply in conjunction with split :

				> lapply(split(x, f), mean)
				$`1`
				[1] -0.3821025

				$`2`
				[1] 0.3502774

				$`3`
				[1] 0.8963904

in fact, in this case, you can also use the **tapply** function to do the same thing, that is even more compact. But the good things with split, is that you can use it on more complicated objects!

I load the airquality dataframe from the datasets package :

				> library(datasets)
				> head(airquality)
				  Ozone Solar.R Wind Temp Month Day
				1    41     190  7.4   67     5   1
				2    36     118  8.0   72     5   2
				3    12     149 12.6   74     5   3
				4    18     313 11.5   62     5   4
				5    NA      NA 14.3   56     5   5
				6    28      NA 14.9   66     5   6

So, to calculate the mean, i.e., of Ozone, Solar.R Winf and Temp for each month : So I can split the data frame in separate months, and then calculate the mean for each column with apply or columnMean

				s <- split(airquality, airquality$Month)
				> lapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")]))
				$`5`
				   Ozone  Solar.R     Wind 
					  NA       NA 11.62258 

				$`6`
					Ozone   Solar.R      Wind 
					   NA 190.16667  10.26667 

				$`7`
					 Ozone    Solar.R       Wind 
						NA 216.483871   8.941935 

				$`8`
				   Ozone  Solar.R     Wind 
					  NA       NA 8.793548 

				$`9`
				   Ozone  Solar.R     Wind 
					  NA 167.4333  10.1800 
	  
The Month variable is not a factor variable, but it can be coerced easily beacause it takes only 5, 6, 7, 8 and 9. The NA is because if you have a missing value in a column you get a NA.

I can also call Sapply to simplify : every element returned (5,6,7,8,9) has a vector of length 3 (same length) so sapply can put all numbers in a matrix 3,5 instead of a list:

				> sapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")]))
							   5         6          7        8        9
				Ozone         NA        NA         NA       NA       NA
				Solar.R       NA 190.16667 216.483871       NA 167.4333
				Wind    11.62258  10.26667   8.941935 8.793548  10.1800

I still got  NA, so I pass the na.rm  to colMeans to have the mean without NA :

				> sapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")], na.rm = TRUE))
								5         6          7          8         9
				Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
				Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
				Wind     11.62258  10.26667   8.941935   8.793548  10.18000

Splitting more than one level. You can have for exemple a variable that is "gender" and another is "race"  . So :

				> x <- rnorm(10)
				> f1 <- gl(2, 5)
				> f2 <- gl(5, 2)
				> f1
				 [1] 1 1 1 1 1 2 2 2 2 2
				Levels: 1 2
				> f2
				 [1] 1 1 2 2 3 3 4 4 5 5
				Levels: 1 2 3 4 5

So I can use "interaction" function to combine all the levels of f1 and f2 (so: 2 levels * 5 levels = 10 levels in total)

				> interaction(f1, f2)
				 [1] 1.1 1.1 1.2 1.2 1.3 2.3 2.4 2.4 2.5 2.5
				Levels: 1.1 2.1 1.2 2.2 1.3 2.3 1.4 2.4 1.5 2.5

Now I can split my numeric vector x according to thoses 2 different levels (f1, f2) : I don"t need to pass the interaction function, : i can just pass the two factors

				> str(split(x, list(f1, f2)))
				List of 10
				 $ 1.1: num [1:2] 0.619 0.99
				 $ 2.1: num(0) 
				 $ 1.2: num [1:2] 0.981 1.077
				 $ 2.2: num(0) 
				 $ 1.3: num 0.637
				 $ 2.3: num 1.38
				 $ 1.4: num(0) 
				 $ 2.4: num [1:2] -0.405 -1.528
				 $ 1.5: num(0) 
				 $ 2.5: num [1:2] 0.195 -0.209
 
 So I have empty levels here, and I can pass this to sapply or lapply, but split level has an argument "drop" to drop the empy levels created by splitting: if you have only a factor, usually you don"t have (many) empty levels, but combining 2 or more factors, you don't have usually every observation of every combination, so it's usual to have empty levels
 
 > str(split(x, list(f1, f2), drop = TRUE))
List of 6
 $ 1.1: num [1:2] 0.619 0.99
 $ 1.2: num [1:2] 0.981 1.077
 $ 1.3: num 0.637
 $ 2.3: num 1.38
 $ 2.4: num [1:2] -0.405 -1.528
 $ 2.5: num [1:2] 0.195 -0.209
 
 ## Debugging Tools - Diagnosing the Problem
 
 There are trhee types of indication that there's a problem
 
 
 * *message* : generic norification/diagnostic message produce by the **message** function: execution of the function continues
 
 * *warning* : Indication that something is wrong but not fatal. Message produce by the **warning** function: execution of the function continues
 
 * *error* : indication that a fatal problem has occurred - produced by the **stop** function, execution stops
 
 * *condition* a generic concept that indicates that something unexpected can occur; programmer can create their own conditions

 Warning exemple. You get the warning back, but it's a NaN. :
 
 > log(-1)
[1] NaN
Warning message:
In log(-1) : production de NaN

## Debugging Tools - Basic Tools

* **traceback** print how many deeper in the function you are. Does nothing if there's no error.

* **debug**  flags a function for debug mode which alloxs you to step through execution of a function one line at a time

* **browser** suspends the execution of a function wherever it is called in your code and puts the function in debug mode (similar and related to debug function: sometimes in place to debug en entire function you want to start to debug from a exact point of a function)

* **trace** allows you to insert debugging code into a function without editing it, and in a specific place (handful if you are debugging someone else's code)

* **recover** its an "error handler" : allows you to modifiy the error behavior so that you can browse the function call stack

(more pratically, you can also just use print/cat in a function to follow it...)

## Debugging Tools - Using Tools

**traceback** you have to call traceback right away after the error to have a result

				> mean(x)
				Error in mean(x) : objet 'x' introuvable
				> traceback()
				1: mean(x)

more interesting :

> lm(y - x)
Error in stats::model.frame(formula = y - x, drop.unused.levels = TRUE) : 
  objet 'y' introuvable
> traceback()
4: stats::model.frame(formula = y - x, drop.unused.levels = TRUE)
3: eval(mf, parent.frame())
2: eval(mf, parent.frame())
1: lm(y - x)

**debug** you can debug any function.

				> debug(lm)
				> lm(y -x)
				debugging in: lm(y - x)
				debug: {
					ret.x <- x
					ret.y <- y
					cl <- match.call()
					mf <- match.call(expand.dots = FALSE)
					m <- match(c("formula", "data", "subset", "weights", "na.action", 
						"offset"), names(mf), 0L)
					mf <- mf[c(1L, m)]
					mf$drop.unused.levels <- TRUE
					mf[[1L]] <- quote(stats::model.frame)
					mf <- eval(mf, parent.frame())
					if (method == "model.frame") 
						return(mf)
					else if (method != "qr") 
						warning(gettextf("method = '%s' is not supported. Using 'qr'", 
							method), domain = NA)
					mt <- attr(mf, "terms")
					y <- model.response(mf, "numeric")
					w <- as.vector(model.weights(mf))
					if (!is.null(w) && !is.numeric(w)) 
						stop("'weights' must be a numeric vector")
					offset <- as.vector(model.offset(mf))
					if (!is.null(offset)) {
						if (length(offset) != NROW(y)) 
							stop(gettextf("number of offsets is %d, should equal %d (number of observations)", 
								length(offset), NROW(y)), domain = NA)
					}
					if (is.empty.model(mt)) {
						x <- NULL
						z <- list(coefficients = if (is.matrix(y)) matrix(, 0, 
							3) else numeric(), residuals = y, fitted.values = 0 * 
							y, weights = w, rank = 0L, df.residual = if (!is.null(w)) sum(w != 
							0) else if (is.matrix(y)) nrow(y) else length(y))
						if (!is.null(offset)) {
							z$fitted.values <- offset
							z$residuals <- y - offset
						}
					}
					else {
						x <- model.matrix(mt, mf, contrasts)
						z <- if (is.null(w)) 
							lm.fit(x, y, offset = offset, singular.ok = singular.ok, 
								...)
						else lm.wfit(x, y, w, offset = offset, singular.ok = singular.ok, 
							...)
					}
					class(z) <- c(if (is.matrix(y)) "mlm", "lm")
					z$na.action <- attr(mf, "na.action")
					z$offset <- offset
					z$contrasts <- attr(x, "contrasts")
					z$xlevels <- .getXlevels(mt, mf)
					z$call <- cl
					z$terms <- mt
					if (model) 
						z$model <- mf
					if (ret.x) 
						z$x <- x
					if (ret.y) 
						z$y <- y
					if (!qr) 
						z$qr <- NULL
					z
				}
				Browse[2]> 

"Browse" is in the function environment . If I press n it execute a line at once until it gets the error...

Browse[2]> n
debug: ret.x <- x
Browse[2]> n
debug: ret.y <- y
Browse[2]> n
debug: cl <- match.call()
Browse[2]> n
debug: mf <- match.call(expand.dots = FALSE)
Browse[2]> n
debug: m <- match(c("formula", "data", "subset", "weights", "na.action", 
    "offset"), names(mf), 0L)
Browse[2]> n
debug: mf <- mf[c(1L, m)]
Browse[2]> n
debug: mf$drop.unused.levels <- TRUE
Browse[2]> n
debug: mf[[1L]] <- quote(stats::model.frame)
Browse[2]> n
debug: mf <- eval(mf, parent.frame())
Browse[2]> n
Error in stats::model.frame(formula = y - x, drop.unused.levels = TRUE) : 
  objet 'y' introuvable

  and moreover you can call debug function inside the debugger to debug another function inside the function
  
  **recover**
  
  I try to read.csv a file that does'nt exist, and with recover it shows me the different levels.
				> options(error = recover)
				> read.csv("nosuchfile")
				Error in file(file, "rt") : impossible d'ouvrir la connexion
				De plus : Warning message:
				In file(file, "rt") :
				  impossible d'ouvrir le fichier 'nosuchfile' : No such file or directory

				Enter a frame number, or 0 to exit   

				1: read.csv("nosuchfile")
				2: read.table(file = file, header = header, sep = sep, quote = quote, dec = de
				3: file(file, "rt")

				Selection: 

So you can press the number and go into the environment of different functions
