# const Qualifier

## Application

* Increase readibility
* Avoid [magic number](https://en.wikipedia.org/wiki/Magic_number_(programming))/string

```cpp
double r = 2.0;
double area = 3.141 * r * r;
double circum = 2 * 3.141 * r;
// Obviously 3.141 refers to pi
// Easy to make a mistake
// If you need to modify it, you need to modify all of them
```

While written in much better form:

```cpp
const double PI = 3.141;
double r = 2.0;
double area = PI * r * r;
double circum = 2 * PI * r;
// You can use PI everywhere without risks of typo
// Also, you can modify it only once to affect all expressions
```

* Typically faster than variables. Usually defined as global. Better use uppercase names like: `MAX_SIZE`, `MIN_COUNT`
* Cannot be modified later on. Must be initialized when it is defined.
* *The immutability is only guranteed at **compiler level** (if you write code that directly modifies values of a `const`, the compiler will not pass)

```cpp
int a = 1;
const int *p1 = &a;		// A pointer to const int; the value should not change
cout << a << " " << *p1 << endl;	// 1 1
a = 2;
cout << a << " " << *p1 << endl;	// 2 2
```

And you may want to do such things like:

```cpp
const int b = 1;  
int* p2 = (int*)(&b);  
cout << b << " " << *p2 << endl;	// 1 1
*p2 = 2; 
cout << b << " " << *p2 << endl;	// Possibly: 1 2
cout << &b << " " << p2 << endl;	// Also you can check the adress (same)
```

This is an undefined behavior (UB). While most of the time, the compiler will do some optimization to `const` values. `const` values will be stored at some special location in memory for faster read.

* A variable with `const` means you **cannot modify the value in that address through this varibale** (≠the value in that adress can never be modified anyway).

* Suggestion: Think before coding. Don't try to do weird things if you are not 100% sure.

## const Reference



* non-const references are actually **binded to address** (they are not just alias of variables)

* const references are allowed to be binded to **lvalues and rvalues** (refer to slides for examples)

* const reference is more used in argument passing:

  * `void func(LargeObject a)`: `a` is **passed by value**, which is copied into the function. You can modify `a` inside the function as a local variable, but it will not affect outside.
  * `void func(const LargeObject a)`: `a` is **passed by value**, which is copied into the function. `a` cannot be modified anyway (even inside).
  * `void func(LargeObject &a)`: `a` is **passed by reference**. And the passed object `a` can be modifed. This is common when you want to modify the parameters in functions. Another way is using pointers: ``void func(LargeObject *a)``
  * `void func(const LargeObject &a)`: `a` is **passed by reference**. You cannot modify the value of `a` anyway. Both variables and constant values can be passed in as `a`.

  ```cpp
  void reference_func(int &x){}
  void point_func(int *px){}
  void const_reference_func(const int &x){}
  
  int main() {
      int a = 1;
      int &r = a;
      int *p = &a;
      reference_func(a);
      reference_func(r);
      reference_func(p);          // not OK
      point_func(a);              // not OK
      point_func(r);              // not OK
      point_func(p);
      const_reference_func(a);
      const_reference_func(r);
      const_reference_func(p);    // not OK
      const_reference_func(10);
      return 0;
  }
  ```

## const Pointers

* Not so commonly used in C++. Personally I suggest using const references if proper. But you shall be able to tell the type given a declaration.
  * Pointer to Constant (PC): `const int *ptr`;
  * Constant Pointer (CP): `int *const ptr` or `int *(const ptr);`
  * Constant Pointer to Constant (CPC): `const int *const ptr` or `const int *(const ptr)`.

|             Type             | Can change the value of pointer? | Can change the object that the pointer points to? |
| :--------------------------: | :------------------------------: | :-----------------------------------------------: |
|     Pointer to Constant      |               Yes                |                        No                         |
|       Constant Pointer       |                No                |                        Yes                        |
| Constant Pointer to Constant |                No                |                        No                         |

* An easy way but not official way: investigate the closest word to `const`

## Type Defination

* The *typedef declaration* provides a way to declare an identifier as a type alias, to be used to replace a possibly complex type name

* The typedef specifier, when used in a declaration, specifies that the declaration is a *typedef declaration* **rather than a variable or function declaration**. 
* In short: you can add a `typedef` before any usual varaible declarations to make it a type rather than a normal variable.
* You can remember it as: `typedef existing_type alias_name`, but it's not always the case

```cpp
typedef int FixedLenArray[5];
typedef struct {int a; int b;} S;
FixedLenArray arr = {1,2,3,4,5};
S example = {1,2};
```

* You can also use your defined types to define other types (refer to slides). When doing this, the type name isn't just replaced by its real type:

```cpp
typedef T *ptrT_t;
typedef const ptrT_t constptrT_t;
// constptrT_t is T *const rather than const T*
```

## const Member Function

* A `const` qualifier after **member functions** promises that this member function will not modify this object. 

```cpp
class Sample {
    int val;
public:
    void setVal() const { val = 0; }	// Compile error
};
```

* Also, inside a `const` member function, non-const member functions (as well as other functions that may modify the object) cannot be called (to ensure that the object will not be modified).

```cpp
void ordinary_func (int &d) {
    d = 666;
}

class Sample {
public:
    int a;
    int b;
public:
    int getA() const { return a; };
    int getB() { return b; }
    void setVal() const { 
        int tmp1 = getA();
        int tmp2 = getB();  // not OK, getB() should be qualified as const
        ordinary_func(a);   
      	// not OK, "a" is automatically cast into "const int" 
      	// in this member function
    }
};
```

* This qualifer tells the compiler to check. It protects the object by casting all member variabales into `const`.

## Credit

SU2019 & SU2020 VE280 Teaching Groups.

VE280 Lecture 5