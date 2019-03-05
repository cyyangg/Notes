---
Lecture 6b
---

# Learning Targets
1. I can write a C++ class that supports the iterator idiom.
2. I can write C++ code that uses iterators
3. I am ready to start Homework 5

# Review
## Accessing Private Data
```C++
class C {
private:
    int data_;

};

class D {
    int peek(C other);
};

int D::peek(C other) {
    //Error
    return other.data_;
}
```
The method above won't work, so we need to use a thing called friendship. We need to add the friend class.

```C++
class C {
private:
    int data_;
};

class D {
    int peek(const C& other);
// This allows C to access D's data
friend class C;
};
```
The friend class only goes one way, so in the example above, we only get that C can access D's data, but D can't access C's data. 

## Nested Classes
```C++
class LinkedList {
    // ...Linked List stuff
    class Node {
    // ...Node Stuff
    };
    
    // Linked list stuff
};
```
With this, you can make the node public or private. 

![collAqn.png](collAqn.png)

## Thinking about iterators: key questions
* What data should we store in an iterator (to keep track of where we are)?
* What data is in the begin() iterator?
* What do the iterator operations do with this data?
    * in operator!=
    * in operator*
    * in operator++
* What data is in the end() iterator?
## Implementing iterators
### Collection A
Suppose the class CollectionA stores doubles in a vector-like fashion.

There are lots of possible iterator designs.

*Today: a pointer to the array plus an integer index.

Pointers are not the same thing as an iterator, but you can use pointers to implement iterators.

## Implementing begin and end
```C++
// e.g, collA.begin()
CollectionA::iterator CollectionA::begin() {
    iterator b{contents_, 0};
    return b;
}

CollectionA::iterator CollectionA::end() {
    iterator b{_contents, size_};
}
```

## Implementing operator* and operator++
```C++
double& CollectionA::iterator::operator*()
{
    return arr_[index];
}

CollectionA::iterator& CollectionA::iterator::operator++() {
    ++index_;   
    return *this;
}
```

## Polishing Our Iterators
### Type Abbreviations
We create a helpfully-named synonyms for existing types.
```C++
using cowptr_t = Cow*;

cowptr_t cp = new Cow;
```
## Useful STL (Standard Template Library) algorithms
```C++
#include <algorithm>
#include <numeric>
#include <vector>

void demo(const std::vector<int>& v) {
    int sum = std::accumulate(v.begin(), v.end(), 0);
    int has_42 = std::find(v.begin(), v.end(), 42) != v.end();
}
```
### Making an STL-compatible iterator
```C++
#include <iterator>
#include <cstddef>

class CollectionA {

    class iterator {
    public:
        using value_type        = double;
        using reference         = value_type&;
        using pointer           = value_type*;
        using difference_type   = ptrdiff_t;
        using iterator_category = std::forward_iterator_tag;
        ...as before...
    };
    
    ...as before...
};
```
## Which are OK?
```C++
std::string s{"Hello"};
std::string::iterator i;

// This is okay
std::cerr << s << std::endl;

// We didn't initialize this, s or any collection that we have
// to iterate on, so it is an invalid iterator. 
std::cerr << *i << std::endl;
```
## Which are OK?
```C++
void processAnyString(std::string s) {
    std::string::iterator i = s.begin();
    
    // debugging output
    std::cerr << s << std::endl;
    
    // If it's okay, then this line will output the first letter
    // However on an empty string, this produces an error
    std::cerr << *i << std::endl;
    
    // ...do the work
}
```
## Which are OK?
```C++
std::string::iterator i;
{
    std::string s{"hello"};
    i = s.begin();
    std::cerr << *i << std::endl; // This is okay
}

std::cerr << s << std::endl; // s is outta scope, compiler error
std::cerr << *i << std::endl; // s is destroyed so there's nothing for i to point to 
```

## Which are OK?
```C++
std::string s{"hello"};
std::string::iterator i = s.begin();

// Apparently if you touch a string (besides accessing the length)
// The string will be invalidated. 
s.pushback('!');

std::cout << s << std::endl;
std::cout << *i << std::endl;
```
This brings up the point with how are we supposed to know what will make an iterator invalid. To find this, we will need to look up the documentation for the iterator. 

```C++
std::list<int> s{1,2,3};
std::list<int>::iterator i = s.begin();

s.push_front(0);
s.push_back(4);

// This is okay, because the iterator will still "point" to 
// the initial beginning. 
std::cout << *i << std::endl;
```
## What data should an IntList::iterator object contain?
We will be implementing this using pointers. The iterator should be an Element pointer (for the homework, it may just be a node pointer).

If we have some Element p*. 
```C++
Element* p;
p->next_ // operator++
p->value // operator*
```
