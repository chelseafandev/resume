## return std::string object vs literal string

[**What happens if I return literal instead of declared std::string?**](https://stackoverflow.com/questions/13857611/what-happens-if-i-return-literal-instead-of-declared-stdstring)

```cpp
std::string GetDescription() { return "XYZ"; }
```

is equivalent to this:

```cpp
std::string GetDescription() { return std::string("XYZ"); }
```

which in turn is equivalent to this:

```cpp
std::string GetDescription() { return std::move(std::string("XYZ")); }
```

Means when you return `std::string("XYZ")` which is a temporary object, then `std::move` is unnecessary, because the object will be moved *anyway* (implicitly).

Likewise, when you return `"XYZ"`, then the explicit construction `std::string("XYZ")` is unnecessary, because the construction will happen anyway (implicitly).

So the answer to this question:

> Is the implicitly created std::string object copied?

is NO. The implicitly created object is after all a temporary object which is moved (implicitly). But then the move can be elided by the compiler!

So the bottomline is this : you can write this code and be happy:
```cpp
std::string GetDescription() { return "XYZ"; }
```
And in some corner-cases, return tempObj is more efficient (and thus better) than return std::move(tempObj).