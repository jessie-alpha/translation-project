# Python Performance Optimization

## Introduction

Resources are never sufficient to meet growing needs in most industries, and now especially in technology as it carves its way deeper into our lives. Technology makes life easier and more convenient and it is able to evolve and become better over time.

This increased reliance on technology has come at the expense of the computing resources available. As a result, more powerful computers are being developed and the optimization of code has never been more crucial.

Application performance requirements are rising more than our hardware can keep up with. To combat this, people have come up with many strategies to utilize resources more efficiently – Containerizing, Reactive (Asynchronous) Applications, etc.

Though, the first step we should take, and by far the easiest one to take into consideration, is code optimization. We need to write code that performs better and utilizes less computing resources.

In this article, we will optimize common patterns and procedures in Python programming in an effort to boost the performance and enhance the utilization of the available computing resources.

## Problem with Performance

As software solutions scale, performance becomes more crucial and issues become more grand and visible. When we are writing code on our localhost, it is easy to miss some performance issues since usage is not intense. Once the same software is deployed for thousands and hundreds of thousands of concurrent end-users, the issues become more elaborate.

Slowness is one of the main issues to creep up when software is scaled. This is characterized by increased response time. For instance, a web server may take longer to serve web pages or send responses back to clients when the requests become too many. Nobody likes a slow system especially since technology is meant to make certain operations faster, and usability will decline if the system is slow.

When software is not optimized to utilize available resources well, it will end up requiring more resources to ensure it runs smoothly. For instance, if memory management is not handled well, the program will end up requiring more memory, hence resulting in upgrading costs or frequent crashes.

Inconsistency and erroneous output is another result of poorly optimized programs. These points highlight the need for optimization of programs.

## Why and When to Optimize

When building for large scale use, optimization is a crucial aspect of software to consider. Optimized software is able to handle a large number of concurrent users or requests while maintaining the level of performance in terms of speed easily.

This leads to overall customer satisfaction since usage is unaffected. This also leads to fewer headaches when an application crashes in the middle of the night and your angry manager calls you to fix it instantly.

Computing resources are expensive and optimization can come in handy in reducing operational costs in terms of storage, memory, or computing power.

But when do we optimize?

It is important to note that optimization may negatively affect the readability and maintainability of the codebase by making it more complex. Therefore, it is important to consider the result of the optimization against the technical debt it will raise.

If we are building large systems which expect a lot of interaction by the end users, then we need our system working at the best state and this calls for optimization. Also, if we have limited resources in terms of computing power or memory, optimization will go a long way in ensuring that we can make do with the resources available to us.

## Profiling

Before we can optimize our code, it has to be working. This way we can be able to tell how it performs and utilizes resources. And this brings us to the first rule of optimization - Don't.

As Donald Knuth - a mathematician, computer scientist, and professor at Stanford University put it:

"Premature optimization is the root of all evil."

The solution has to work for it to be optimized.

Profiling entails scrutiny of our code and analyzing its performance in order to identify how our code performs in various situations and areas of improvement if needed. It will enable us to identify the amount of time that our program takes or the amount of memory it uses in its operations. This information is vital in the optimization process since it helps us decide whether to optimize our code or not.

Profiling can be a challenging undertaking and take a lot of time and if done manually some issues that affect performance may be missed. To this effect, the various tools that can help profile code faster and more efficiently include:

PyCallGraph - which creates call graph visualizations that represent calling relationships between subroutines for Python code. cProfile - which will describe how often and how long various parts of Python code are executed. gProf2dot - which is a library that visualized profilers output into a dot graph. Profiling will help us identify areas to optimize in our code. Let us discuss how choosing the right data structure or control flow can help our Python code perform better.

## Choosing Data Structures and Control Flow

The choice of data structure in our code or algorithm implemented can affect the performance of our Python code. If we make the right choices with our data structures, our code will perform well.

Profiling can be of great help to identify the best data structure to use at different points in our Python code. Are we doing a lot of inserts? Are we deleting frequently? Are we constantly searching for items? Such questions can help guide us choose the correct data structure for the need and consequently result in optimized Python code.

Time and memory usage will be greatly affected by our choice of data structure. It is also important to note that some data structures are implemented differently in different programming languages.

## For Loop vs List Comprehensions

Loops are common when developing in Python and soon enough you will come across list comprehensions, which are a concise way to create new lists which also support conditions.

For instance, if we want to get a list of the squares of all even numbers in a certain range using the for loop:

new_list = [] for n in range(0, 10): if n % 2 == 0: new_list.append(n**2) A List Comprehension version of the loop would simply be:

new_list = [ n**2 for n in range(0,10) if n%2 == 0]

The list comprehension is shorter and more concise, but that is not the only trick up its sleeve. They are also notably faster in execution time than for loops. We will use the Timeit module which provides a way to time small bits of Python code.

Let us put the list comprehension against the equivalent for loop and see how long each takes to achieve the same result:

import timeit

def for_square(n): new_list = [] for i in range(0, n): if i % 2 == 0: new_list.append(n**2) return new_list

def list_comp_square(n): return [i**2 for i in range(0, n) if i % 2 == 0]

print("Time taken by For Loop: {}".format(timeit.timeit('for_square(10)', 'from main import for_square')))

print("Time taken by List Comprehension: {}".format(timeit.timeit('list_comp_square(10)', 'from main import list_comp_square'))) After running the script 5 times using Python 2:

$ python for-vs-lc.py Time taken by For Loop: 2.56907987595 Time taken by List Comprehension: 2.01556396484 $ $ python for-vs-lc.py Time taken by For Loop: 2.37083697319 Time taken by List Comprehension: 1.94110512733 $ $ python for-vs-lc.py Time taken by For Loop: 2.52163410187 Time taken by List Comprehension: 1.96427607536 $ $ python for-vs-lc.py Time taken by For Loop: 2.44279003143 Time taken by List Comprehension: 2.16282701492 $ $ python for-vs-lc.py Time taken by For Loop: 2.63641500473 Time taken by List Comprehension: 1.90950393677 While the difference is not constant, the list comprehension is taking less time than the for loop. In small-scale code, this may not make that much of a difference, but at large-scale execution, it may be all the difference needed to save some time.

If we increase the range of squares from 10 to 100, the difference becomes more apparent:

$ python for-vs-lc.py Time taken by For Loop: 16.0991549492 Time taken by List Comprehension: 13.9700510502 $ $ python for-vs-lc.py Time taken by For Loop: 16.6425571442 Time taken by List Comprehension: 13.4352738857 $ $ python for-vs-lc.py Time taken by For Loop: 16.2476081848 Time taken by List Comprehension: 13.2488780022 $ $ python for-vs-lc.py Time taken by For Loop: 15.9152050018 Time taken by List Comprehension: 13.3579590321 cProfile is a profiler that comes with Python and if we use it to profile our code:

cprofile analysis

Upon further scrutiny, we can still see that the cProfile tool reports that our List Comprehension takes less execution time than our For Loop implementation, as we had established earlier. cProfile displays all the functions called, the number of times they have been called and the amount of time taken by each.

If our intention is to reduce the time taken by our code to execute, then the List Comprehension would be a better choice over using the For Loop. The effect of such a decision to optimize our code will be much clearer at a larger scale and shows just how important, but also easy, optimizing code can be.

But what if we are concerned about our memory usage? A list comprehension would require more memory to remove items in a list than a normal loop. A list comprehension always creates a new list in memory upon completion, so for deletion of items off a list, a new list would be created. Whereas, for a normal for loop, we can use the list.remove() or list.pop() to modify the original list instead of creating a new one in memory.

Again, in small-scale scripts, it might not make much difference, but optimization comes good at a larger scale, and in that situation, such memory saving will come good and allow us to use the extra memory saved for other operations.

## Linked Lists

Another data structure that can come in handy to achieve memory saving is the Linked List. It differs from a normal array in that each item or node has a link or pointer to the next node in the list and it does not require contiguous memory allocation.

An array requires that memory required to store it and its items be allocated upfront and this can be quite expensive or wasteful when the size of the array is not known in advance.

A linked list will allow you to allocate memory as needed. This is possible because the nodes in the linked list can be stored in different places in memory but come together in the linked list through pointers. This makes linked lists a lot more flexible compared to arrays.

The caveat with a linked list is that the lookup time is slower than an array's due to the placement of the items in memory. Proper profiling will help you identify whether you need better memory or time management in order to decide whether to use a Linked List or an Array as your choice of the data structure when optimizing your code.

## Range vs XRange 

When dealing with loops in Python, sometimes we will need to generate a list of integers to assist us in executing for-loops. The functions range and xrange are used to this effect.

Their functionality is the same but they are different in that the range returns a list object but the xrange returns an xrange object.

What does this mean? An xrange object is a generator in that it's not the final list. It gives us the ability to generate the values in the expected final list as required during runtime through a technique known as "yielding".

The fact that the xrange function does not return the final list makes it the more memory efficient choice for generating huge lists of integers for looping purposes.

If we need to generate a large number of integers for use, xrange should be our go-to option for this purpose since it uses less memory. If we use the range function instead, the entire list of integers will need to be created and this will get memory intensive.

Let us explore this difference in memory consumption between the two functions:

$ python Python 2.7.10 (default, Oct 23 2015, 19:19:21) [GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin Type "help", "copyright", "credits" or "license" for more information.

import sys

r = range(1000000) x = xrange(1000000)

print(sys.getsizeof(r)) 8000072

print(sys.getsizeof(x)) 40

print(type(r)) <type 'list'> print(type(x)) <type 'xrange'> We create a range of 1,000,000 integers using range and xrange. The type of object created by the range function is a List that consumes 8000072 bytes of memory while the xrange object consumes only 40 bytes of memory.

The xrange function saves us memory, loads of it, but what about item lookup time? Let's time the lookup time of an integer in the generated list of integers using Timeit:

import timeit

r = range(1000000) x = xrange(1000000)

def lookup_range(): return r[999999]

def lookup_xrange(): return x[999999]

print("Look up time in Range: {}".format(timeit.timeit('lookup_range()', 'from main import lookup_range')))

print("Look up time in Xrange: {}".format(timeit.timeit('lookup_xrange()', 'from main import lookup_xrange'))) The result:

$ python range-vs-xrange.py Look up time in Range: 0.0959858894348 Look up time in Xrange: 0.140854120255 $ $ python range-vs-xrange.py Look up time in Range: 0.111716985703 Look up time in Xrange: 0.130584001541 $ $ python range-vs-xrange.py Look up time in Range: 0.110965013504 Look up time in Xrange: 0.133008003235 $ $ python range-vs-xrange.py Look up time in Range: 0.102388143539 Look up time in Xrange: 0.133061170578 xrange may consume less memory but takes more time to find an item in it. Given the situation and the available resources, we can choose either of range or xrange depending on the aspect we are going for. This reiterates the importance of profiling in the optimization of our Python code.

Note: xrange is deprecated in Python 3 and the range function can now serve the same functionality. Generators are still available on Python 3 and can help us save memory in other ways such as Generator Comprehensions or Expressions.

## Sets

When working with Lists in Python we need to keep in mind that they allow duplicate entries. What if it matters whether our data contains duplicates or not?

This is where Python Sets come in. They are like Lists but they do not allow any duplicates to be stored in them. Sets are also used to efficiently remove duplicates from Lists and are faster than creating a new list and populating it from the one with duplicates.

In this operation, you can think of them as a funnel or filter that holds back duplicates and only lets unique values pass.

Let us compare the two operations:

import timeit

here we create a new list and add the elements one by one
while checking for duplicates
def manual_remove_duplicates(list_of_duplicates): new_list = [] [new_list.append(n) for n in list_of_duplicates if n not in new_list] return new_list

using a set is as simple as
def set_remove_duplicates(list_of_duplicates): return list(set(list_of_duplicates))

list_of_duplicates = [10, 54, 76, 10, 54, 100, 1991, 6782, 1991, 1991, 64, 10]

print("Manually removing duplicates takes {}s".format(timeit.timeit('manual_remove_duplicates(list_of_duplicates)', 'from main import manual_remove_duplicates, list_of_duplicates')))

print("Using Set to remove duplicates takes {}s".format(timeit.timeit('set_remove_duplicates(list_of_duplicates)', 'from main import set_remove_duplicates, list_of_duplicates'))) After running the script five times:

$ python sets-vs-lists.py Manually removing duplicates takes 2.64614701271s Using Set to remove duplicates takes 2.23225092888s $ $ python sets-vs-lists.py Manually removing duplicates takes 2.65356898308s Using Set to remove duplicates takes 1.1165189743s $ $ python sets-vs-lists.py Manually removing duplicates takes 2.53129696846s Using Set to remove duplicates takes 1.15646100044s $ $ python sets-vs-lists.py Manually removing duplicates takes 2.57102680206s Using Set to remove duplicates takes 1.13189387321s $ $ python sets-vs-lists.py Manually removing duplicates takes 2.48338890076s Using Set to remove duplicates takes 1.20611810684s Using a Set to remove duplicates is consistently faster than manually creating a list and adding items while checking for presence.

This could be useful when filtering entries for a giveaway contest, where we should filter out duplicate entries. If it takes 2s to filter out 120 entries, imagine filtering out 10 000 entries. On such a scale, the vastly increased performance that comes with Sets is significant.

This might not occur commonly, but it can make a huge difference when called upon. Proper profiling can help us identify such situations, and can make all the difference in the performance of our code.

## String Concatenation 

Strings are immutable by default in Python and subsequently, String concatenation can be quite slow. There are several ways of concatenating strings that apply to various situations.

We can use the + (plus) to join strings. This is ideal for a few String objects and not at scale. If you use the + operator to concatenate multiple strings, each concatenation will create a new object since Strings are immutable. This will result in the creation of many new String objects in memory hence improper utilization of memory.

We can also use the concatenate operator += to join strings but this only works for two strings at a time, unlike the + operator that can join more than two strings.

If we have an iterator such as a List that has multiple Strings, the ideal way to concatenate them is by using the .join() method.

Let us create a list of a thousand words and compare how the .join() and the += operator compare:

import timeit

create a list of 1000 words
list_of_words = ["foo "] * 1000

def using_join(list_of_words): return "".join(list_of_words)

def using_concat_operator(list_of_words): final_string = "" for i in list_of_words: final_string += i return final_string

print("Using join() takes {} s".format(timeit.timeit('using_join(list_of_words)', 'from main import using_join, list_of_words')))

print("Using += takes {} s".format(timeit.timeit('using_concat_operator(list_of_words)', 'from main import using_concat_operator, list_of_words'))) After two tries:

$ python join-vs-concat.py Using join() takes 14.0949640274 s Using += takes 79.5631570816 s $ $ python join-vs-concat.py Using join() takes 13.3542580605 s Using += takes 76.3233859539 s It is evident that the .join() method is not only neater and more readable, but it is also significantly faster than the concatenation operator when joining Strings in an iterator.

If you're performing a lot of String concatenation operations, enjoying the benefits of an approach that's almost 7 times faster is wonderful.

## Conclusion

We have established that the optimization of code is crucial in Python and also saw the difference made as it scales. Through the Timeit module and cProfile profiler, we have been able to tell which implementation takes less time to execute and backed it up with the figures. The data structures and control flow structures we use can greatly affect the performance of our code and we should be more careful.

Profiling is also a crucial step in code optimization since it guides the optimization process and makes it more accurate. We need to be sure that our code works and is correct before optimizing it to avoid premature optimization which might end up being more expensive to maintain or will make the code hard to understand.
