
# 循环

## Classic For Loop  
Here is the classic for loop statement that is both valid in Groovy and Java: 

```
for (i = 0; i <3; i++) {
   System.out.println("Hello World")
}
```

This will print 3 lines of "Hello World" 

```
Hello World
Hello World
Hello World
```

## Groovy For Loop with Upto
Groovy supports the upto default method that helps write similar for loop statements. For example: 
```
def c = {
   println it
}
1.upto(4, c)
```
The output is 
```
1
2
3
4
```
Here is another example of using upto: 
```
1.upto(4, {
   println "Number ${it}"
})
```
Here is the output: 
```
Number 1
Number 2
Number 3
Number 4
```
Here is another way of writing the statement: 
```
1.upto(4) {
   println "Test ${it}"
}
```
And this is the output: 
```
Test 1
Test 2
Test 3
Test 4
```
## Grovy For Loop WIth Step
Similar to upto, Groovy also supports step default method where we can specify the increment. Here is a simple example with increment 1. 
```
0.step 5, 1, {
   println it
}
```
The output is this: 
```
0
1
2
3
4
```
Here is the same example with a different increment. This time using the value 2. 
```
0.step 5, 2, {
   println it
}
```
Notice the difference in the output: 
```
0
2
4
```
## Groovy For Loop with Times
Here is my favorite for loop like in Groovy. It uses times default method: 
```
3.times {
   println "Hello World ${it}"
}
Hello World 0
Hello World 1
Hello World 2
```
Here is another way of writing it using a variable: 
```
def n = 3
n.times {
   println "Hello World ${it}"
}
```
## Groovy For Loop with List
We can use the Groovy syntax for iterating over a list or collection using the keyword in. Here is a simple example:
```
def list = ["A", "B", "C"]
for (item in list) {
   println item
}
```
The output is this: 
```
A
B
C
```
Here is a shortened example: 
```
for (item in ["A", "B", "C"]) {
   println item
}
```
Here is another example that iterates over a range of numbers 
```
for (number in 1..3 ) {
    println number
}
```
The output is this: 
```
1
2
3
```
Here is a way of iterating over a map using for loop: 
```
def map = [a1:'b1', a2:'b2']
for ( item in map ) {
    println item.key 
}
```
The output is this: 
```
a1
a2
```
Same example but using the values of the map 
```
def map = [a1:'b1', a2:'b2']
for ( item in map ) {
    println item.value
}
```
The output is this: 
```
b1
b2
```
## Grovy For Loop WIth Each
The each default method is another convenient way of performing a loop. Here is an example: 
```
def list = ["A", "B"]
list.each {
	println it
}
```
The output is this: 
```
A
B
```
Here is an example of using each with a range of numbers 
```
(1..3).each {
	println it
}
```
The output is this: 
```
1
2
3
```
Another example of using each with index: 
```
def list = ["A", "B", "C"]
list.eachWithIndex { val, idx ->
   println "${idx}. ${val}"
}
```
The output is this: 
```
0. A
1. B
2. C
```
