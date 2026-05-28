# Guild Data Types

Guild provides a flexible and modern data type system inspired by low-level languages like C while integrating higher-level concepts suitable for GUI and application development.

The type system is designed to support:

* Performance
* Readability
* Memory control
* Type safety
* GUI-oriented programming
* Runtime optimization


# Basic Variable Modifiers

These keywords define how variables behave.

## `const`

Declares a constant variable whose value cannot change after initialization.

```guild
const max = 100;
```

## `let`

Declares a mutable variable with automatic type inference.

```guild
let age = 17;
```

The compiler automatically determines the variable type.

## `var`

Declares a mutable variable with dynamic and flexible behavior.

Variables declared using `var`:

* Can be accessed before declaration
* Can be redeclared multiple times
* Use block scope like `let`

```guild
cout(x);
var x = 50;
```

```guild
var name = "Guild";
var name = 200;
```

## `null`

Represents an empty or invalid value.

```guild
let username = null;
```

`null` cannot be applied to constant variables.

# Primary Data Types

Primary data types are the fundamental built-in types of Guild. Their values can be modified after initialization.

## `int`

Stores integer values.

```guild
int age = 18;
```

## `float`

Stores decimal numbers.

```guild
float pi = 3.141592654;
```

## `char`

Stores a single character.

```guild
char letter = 'A';
```

Character arrays may also be used to store strings.

```guild
char name[20] = "Andrew";
```

If the provided text exceeds the maximum size, extra characters are ignored.

## `string`

Stores multiple characters as text.

```guild
string message = "Hello World";
```

## `boolean`

Stores either `true` or `false`.

```guild
bool isRunning = true;
```

# Derived Data Types

Derived types are constructed from primary types or user-defined structures.

## `point`

Represents a pointer storing the memory address of another variable.

```guild
int x = 20;
int point ptr = &x;
```

Pointers may use modifiers such as `restrict`.

```guild
restrict int point ptr = &x;
```

## `array`

Represents a collection of values.

### Untyped Arrays

```guild
let values = [2, "hello", true];
```

### Typed Arrays

```guild
int numbers = [2, 4, 6, 8];
```

Typed arrays only accept values matching their declared type.

## `structure`

Defines grouped variables under one type.

```guild
struct object3d {
    int x_axis;
    int y_axis;
    int z_axis;
    string object_name;
}
```

Creating an instance:

```guild
let cube = new object3d;
```

## `enumeration`

Defines named integer constants.

```guild
enum days {
    MONDAY,
    TUESDAY,
    WEDNESDAY
}
```

## `object`

Defines JavaScript-like objects containing properties and methods.

```guild
let player = {
    int health = 100,
    attack() {
        cout("Attack!");
    }
}
```

## `class`

Defines reusable object blueprints supporting object-oriented programming.

```guild
class Human {
    string name,
    speak() {
        cout("Hello");
    }
}
```

## `interface`

Represents GUI containers such as windows or pages.

Interfaces are first-class GUI structures in Guild.

```guild
interface HWindow {
    type: window;
    this::classList.add("main-window");
    this::style {
        border: 1px solid #eaeaea;
        bg-color: var(--da-bg);
    }
}
```

Then:

```guild
const win = new HWindow;
```

Sometimes, we may need only one instance of an interface. The `restrict` keyword can be used:

```guild
restrict const win = new HWindow; // Only one instance of HWindow exists
```

# Type Modifiers

Guild supports C-style type modifiers.

## `short`

Defines smaller-sized integer types.

```guild
short int smallNumber = 12;
```

## `long`

Defines larger-sized integer types.

```guild
long int largeNumber = 999999;
```

## `signed`

Defines signed numeric types.

```guild
signed int balance = -50;
```

## `unsigned`

Defines unsigned numeric types.

```guild
unsigned int points = 400;
unsigned int point = -100; // Converts into 100
```

# Storage and Optimization Modifiers

These modifiers affect variable storage behavior and compiler optimizations.

## `static`

Makes a variable persist during the entire runtime while limiting visibility.

```guild
static int counter = 0;
```

## `volatile`

Prevents compiler optimization on a variable whose value may change unexpectedly.

```guild
volatile int deviceState;
```

## `restrict`

Indicates exclusive access to a memory location through a pointer.

```guild
restrict int point ptr = &status;
```

This allows advanced compiler optimizations.

## `inline`

Suggests inserting function code directly at the call site.

```guild
inline func square(int x) {
    return x * x;
}
```

# Type Definitions

## `typedef`

Creates aliases for existing data types.

```guild
typedef long long int int64;
```

```guild
typedef unsigned int uint32;
```

This improves readability and simplifies complex declarations.

# Conclusion

Guild's type system combines low-level control with modern development flexibility.

It is designed to support:

* Systems programming
* GUI development
* Runtime optimization
* Memory management
* Scalable applications
* Future language extensibility

The type system will continue evolving alongside the Guild ecosystem.
