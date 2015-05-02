---
title: "Experiments with Reference Classes in R"
author: "Tirthankar Chakravarty"
date: "1 May 2015"
layout: post
categories: [R]
---


## Example 1
Here is how you might create a Python class with a constructor and three methods:


{% highlight python %}
class MyClass:
  def __init__(self, variable1 = 1, variable2 = 2):
    self.variable1 = variable1
    self.variable2 = variable2
  
  def hello(self):
    return "Hello!"
  
  def goodbye(self):
    return "Goodbye!"
{% endhighlight %}

Based on the advice [here](http://stackoverflow.com/q/11561284/1414455), reference classes might be the best way to create objects using the same paradigm in R.  


{% highlight r %}
MyClass = setRefClass("MyClass", 
                      fields = list(variable1 = "numeric",
                                    variable2 = "numeric"),
                      methods = list(
                        initialize = function(variable1 = 1, variable2 = 2) {
                          "Initialize an object of class 'MyClass'"
                          variable1 <<- variable1
                          variable2 <<- variable2
                          cat("Object of class 'MyClass' successfully intialized.")
                        },
                        hello = function() {
                          cat("Hello!")
                        },
                        goodbye = function() {
                          cat("Goodbye!")
                        }),
                      )
{% endhighlight %}
Now that the class definition of `MyClass` has been created, we can use it.

{% highlight r %}
myClass1 = MyClass$new(variable1 = 3, variable2 = 4)
{% endhighlight %}



{% highlight text %}
## Object of class 'MyClass' successfully intialized.
{% endhighlight %}



{% highlight r %}
myClass1$hello()
{% endhighlight %}



{% highlight text %}
## Hello!
{% endhighlight %}



{% highlight r %}
myClass1$goodbye()
{% endhighlight %}



{% highlight text %}
## Goodbye!
{% endhighlight %}

## Example 2
Here is another example of the translation of a simple Python class to R's reference classes. First, the Python class:

{% highlight python %}
class Circle:
  radius = None
  diameter = None
  
  # initialization
  def __init__(self, radius):
    self.radius = radius
  
  # value setting methods
  def setradius(self, radius):
    self.radius = radius
  
  def setdiameter(self, diameter):
    self.diameter = diameter
  
  # value getting methods
  def getradius(self):
    return self.radius
  
  def getdiameter(self):
    return self.diameter
  
  # value altering methods
  def calc_diameter(self):
    self.diameter = 2*self.radius
{% endhighlight %}
I am not sure why it is possible to instantiate the object with no arguments, when the `__init__` initialization function takes an argument `r` with no default?

This can be translated to R's reference classes like so:

{% highlight r %}
Circle = setRefClass("Circle", 
                     fields = list(radius = "numeric", diameter = "numeric"),
                     methods = list(
                       initialize = function(radius) {
                         radius <<- radius
                       },
                       getradius = function() radius,
                       getdiameter = function() diameter,
                       setradius = function(radius) radius <<- radius,
                       setdiameter = function(diameter) diameter <<- diameter,
                       calc_diameter = function(r = radius){
                         radius <<- r
                         diameter <<- 2*radius
                       } 
                     ))
{% endhighlight %}
Now we can use the class. Note in particular that R fails when we attempt to instantiate the object without the `radius` argument, which is not a failure in Python. 

{% highlight r %}
# circle1 = Circle$new()  # fails; compare to Python's circle1 = Circle() 
circle1 = Circle$new(radius = 1)
{% endhighlight %}
Then the radius of the object just created is `circle1$getradius()` (= 1). Let's check to see what happens when we try to retrieve the `diameter`? We get `circle1$getdiameter()` (= ) -- it returns a `numeric(0)`. We can initialize it using a `circle1$calc_diameter()`, and retrieve the value using `circle1$getdiameter()` (= 2).   

Note that there is one difference from the Python implementation in that the `calc_diameter` method takes an optional `radius` argument, that updates the `radius` field and then computes the `diameter`.

## Encapsulation
The next notion that most object-oriented programmers want to figure out is whether it is possible to encapsulate data, that is, make some of the data fields private so that they cannot be accessed or set from outside, i.e., not by a class method. In the current example, we can

{% highlight r %}
circle1$diameter
{% endhighlight %}



{% highlight text %}
## [1] 2
{% endhighlight %}



{% highlight r %}
circle1$diameter = 3
circle1$diameter
{% endhighlight %}



{% highlight text %}
## [1] 3
{% endhighlight %}



{% highlight r %}
circle1$radius
{% endhighlight %}



{% highlight text %}
## [1] 1
{% endhighlight %}
So, we can easily set the value of the diameter, and break the link between the radius and the diameter, which should be managed by the object itself. As of now, I have been unable to find any simple way of hiding certain fields from direct access. 

## Mutability
Another point to note, [as noted by Hadley](http://adv-r.had.co.nz/OO-essentials.html#rc), is that they are mutable, and they do not have the write-on-modify (that a new version of the object is created when copied) semantics that most R objects have:

{% highlight r %}
pryr::address(circle1)
{% endhighlight %}



{% highlight text %}
## [1] "0x421e8a8"
{% endhighlight %}



{% highlight r %}
circle2 = circle1
pryr::address(circle2)
{% endhighlight %}



{% highlight text %}
## [1] "0x421e8a8"
{% endhighlight %}
