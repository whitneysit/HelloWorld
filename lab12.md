---
layout: default
title: Labs
nav: labs
---

# Lab 12 - STL & Iterators

We've used a good amount of the C++ Standard Library in CS104. This lab will show you some of the other things you haven't touched. The lab is designed as a lot of small exercises that must be completed to have all the tests working.

## 0 - Comparator

The following is background info.

If you saw the following:

```c++
 int x = f();
```

You'd think `f` is a function.  But with the magic of operator overloading, we can make `f` an object and `f()` a member function call
to `operator()` of the instance, `f` as shown in the following code:

```c++
  struct RandObjGen {
    int operator() { return rand(); }
  };
  
  RandObjGen f;
  int r = f(); // translates to f.operator() which returns a random number by calling rand()
```

An object that overloads the `operator()` is called a __functor__ and they are widely used in C++ STL to provide a kind of polymorphism.

We will use functors to provide different ways of sorting types of data. For example, if we wanted to sort strings, we could sort either lexicographically/alphabetically or by length of string). To do so is easy: we supply a functor object that implements the different comparison approaches.

```c++
  struct AlphaStrComp {
    bool operator()(const string& lhs, const string& rhs) 
	{ // Uses string's built in operator< 
	  // to do lexicographic (alphabetical) comparison
	  return lhs < rhs; 
	}
  };

  struct LengthStrComp {
    bool operator()(const string& lhs, const string& rhs) 
	{ // Uses string's built in operator< 
	  // to do lexicographic (alphabetical) comparison
	  return lhs.size() < rhs.size(); 
	}
  };
  
  string s1 = "Blue";
  string s2 = "Red";
  AlphaStrComp comp1;
  LengthStrComp comp2;
  
  cout << "Blue compared to Red using AlphaStrComp yields " << comp1(s1, s2) << endl;
  // notice comp1(s1,s2) is calling comp1.operator() (s1, s2);
  cout << "Blue compared to Red using LenStrComp yields " << comp2(s1, s2) << endl;
  // notice comp2(s1,s2) is calling comp2.operator() (s1, s2);
```

This would yield the output

```c++
1  // Because "Blue" is alphabetically less than "Red"
0  // Because the length of "Blue" is 4 which is NOT less than the length of "Red" (3)
```

We can now make a templated function (not class, just a templated function) that lets the user pass in which kind of comparator object they would like:

```c++
template <class Comparator>
void DoStringCompare(const string& s1, const string& s2, Comparator comp)
{
  cout << comp(s1, s2) << endl;  // calls comp.operator()(s1,s2);
}

  string s1 = "Blue";
  string s2 = "Red";
  AlphaStrComp comp1;
  LengthStrComp comp2;

  // Uses alphabetic comparison
  DoStringCompare(s1,s2,comp1);
  // Use string length comparison
  DoStringCompare(s1,s2,comp2);
```
	   
In this way, you could define a new type of comparison in the future, make a functor struct for it, and pass it in to the `DoStringCompare` function and the `DoStringCompare` function never needs to change.

These comparator objects are used by the C++ STL `map` and `set` class to compare keys to ensure no duplicates are entered.

```c++
template < class T,                        // set::key_type/value_type
           class Compare = less<T>,        // set::key_compare/value_compare
           class Alloc = allocator<T>      // set::allocator_type
           > class set;
```

You could pass your own type of Comparator object to the class, but it defaults to C++'s standard less-than functor `less<T>` which is  simply defined as:

```c++
template <class T>
struct less
{
  bool operator() (const T& x, const T& y) const {return x<y;}
};
```

For more reading on functors, search the web or try [this link](http://www.cprogramming.com/tutorial/functors-function-objects-in-c++.html)


## 1 - Priority_Queue 

A priority queue is the STL version of a heap. You are able to pass in a custom comparator class to the priority_queue, and use functions such as top(), push(), etc. By default, the priority queue takes the less-than Comparator and creates a max heap.

For execise one, create a priority queue the sorts pairs by the *SECOND* value in ascending order. 

## 2 - Algorithm

The algorithm library is incredibly powerful, but can be difficult to use. Here is an example of efficiently erasing all instances of the number 10 from a vector (compared to using a for loop):

```
vec.erase(std::remove(vec.begin(), vec.end(), 10), vec.end());
```

For execise two, remove all integers from a vector that are divisible by 5 OR divisible by 3. 

## 3 - Reverse, Reverse Iterators

We've worked with the begin() and end() iterators, but STL containers commonly contain reverse_iterators in addition to the normal iterator. They do just as you'd expect, traverse the container in reverse (from back to front). 

Using the reverse iterators, make a new vector that copies the first one, but in reverse. 

## More Iterators!

To check your understanding of iterators, we have a simple iterator exercise in iterator.cpp. 
