---
layout : post
category : code
---

While calculus is far and away the most interesting math class I have ever taken, there are certain parts of the AP curriculum that get me very frustrated, such as calculating Riemann Sums. Now, don't get me wrong, I'm not suggesting that learning about Riemann Sums is unnecessary. Quite the opposite actually. I think learning about Riemann Sums is very important. Yet the fact that I had to calculate so many of these sums by hand almost made me forget how cool they are. 

For those of you who have either forgotten or haven't taken high school calculus yet:

> "A Riemann sum is a method for approximating the total area underneath a curve on a graph, otherwise known 
> as an integral." [—Wikipedia](http://en.wikipedia.org/wiki/Riemann_sum)

So one evening as I stared at a large problem set full of Riemann Sums, I decided to code up a simple Riemann Sum grapher and calculator in python.

##Riemann.py

To get started, install [matplotlib](http://matplotlib.org/) and [numpy](http://numpy.scipy.org/).

I use numpy's *arange* routine to create a range of numbers from a to b, with an interval of .02. Then, we set y equal to the set of outputs from our function f.

<pre><code data-language="python">
numRange = np.arange(a, b, .02)
y = f(numRange)
</code></pre>

Next, we loop through the amount of rectangles we want to draw (*n*) and draw each rectangle. 
*x* is the sum of each rectangles height. 
*Offset* is either 0 (for a left Riemann Sum), 1 (for right), or .5 (for middle). 
*w* is the width of each rectangle.
We then calculate the sum of the areas of all of the rectangles.

<pre><code data-language="python">
for i in range(n):
    x += f(a + (i+offset)*w)
    plt.gca().add_patch(matplotlib.patches.Rectangle((i*w + a,0),w,f(a + (i+offset)*w)))
plt.plot(numRange, y, color=(1.0, 0.00, 0.00), zorder=5)
s = w * x
print '\n', s, '\n'
</code></pre>

<br>

Here's the entire script that accepts user input:

<pre><code data-language="python">from math import *

import numpy as np
import matplotlib
import matplotlib.pyplot as plt


def Riemann(a, b, n, l, f):
    numRange = np.arange(a, b, .02)
    y = f(numRange)
    w = float(b - a)/n
    x = 0.0
    d = {"l": 0, "r": 1, "m": 0.5}
    offset = d[l[0]]
    for i in range(n):
        x += f(a + (i+offset)*w)
        plt.gca().add_patch(matplotlib.patches.Rectangle((i*w + a,0),w,f(a + (i+offset)*w)))
    plt.plot(numRange, y, color=(1.0, 0.00, 0.00), zorder=5)
    s = w * x
    print '\n', s, '\n'


def main():
    print #empty line
    functionString = raw_input("Enter a function: ")
    a = input("Enter the start point: ")
    b = input("Enter the end point: ")
    n = input("Enter the number of rectangles: ")
    l = raw_input("Type right, left, or middle: ")
    Riemann(a, b, n, l, lambda x: eval(functionString))
    plt.show()

if __name__ == "__main__":
    main()</code></pre>