https://www.cs.technion.ac.il/users/yechiel/c++-faq/nondependent-name-lookup-types.html

Perhaps surprisingly, the following code is not valid C++, even though some compilers accept it:
```C++
template<typename T>
class B {
public:
  class Xyz { ... };  ← type nested in class B<T>
  typedef int Pqr;    ← type nested in class B<T>
};

template<typename T>
class D : public B<T> {
public:
  void g()
  {
    Xyz x;  ← bad (even though some compilers erroneously (temporarily?) accept it)
    Pqr y;  ← bad (even though some compilers erroneously (temporarily?) accept it)
  }
};
```

This might hurt your head; better if you sit down.
Within D<T>::g(), name Xyz and Pqr do not depend on template parameter T, so they are known as a nondependent names. On the other hand, B<T> is dependent on template parameter T so B<T> is called a dependent name.

Here's the rule: the compiler does not look in dependent base classes (like B<T>) when looking up nondependent names (like Xyz or Pqr). As a result, the compiler does not know they even exist let alone are types.

At this point, programmers sometimes prefix them with B<T>::, such as:
```C++
template<typename T>
class D : public B<T> {
public:
  void g()
  {
    B<T>::Xyz x;  ← bad (even though some compilers erroneously (temporarily?) accept it)
    B<T>::Pqr y;  ← bad (even though some compilers erroneously (temporarily?) accept it)
  }
};
```
Unfortunately this doesn't work either because those names (are you ready? are you sitting down?) are not necessarily types. "Huh?!?" you say. "Not types?!?" you exclaim. "That's crazy; any fool can SEE they are types; just look!!!" you protest. Sorry, the fact is that they might not be types. The reason is that there can be a specialization of B<T>, say B<Foo>, where B<Foo>::Xyz is a data member, for example. Because of this potential specialization, the compiler cannot assume that B<T>::Xyz is a type until it knows T. The solution is to give the compiler a hint via the typename keyword:
```c++
template<typename T>
class D : public B<T> {
public:
  void g()
  {
    typename B<T>::Xyz x;  ← good
    typename B<T>::Pqr y;  ← good
  }
};
```
