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

