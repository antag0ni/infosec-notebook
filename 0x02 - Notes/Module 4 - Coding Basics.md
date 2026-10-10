#### Structures
Structures or Structs are user-defined data types that allow the programmer to group related data items of different data types into a single unit.

```c
typedef struct tagTHREADENTRY32 {
  DWORD dwSize; // Member 1
  DWORD cntUsage; // Member 2
  DWORD th32ThreadID;
  DWORD th32OwnerProcessID;
  LONG  tpBasePri;
  LONG  tpDeltaPri;
  DWORD dwFlags;
} THREADENTRY32; 
```

#### Declaring a Structure 
A structure in C is declared with the use of  `typedef`.
```c
typedef struct _STRUCTURE_NAME {

  // structure elements

} STRUCTURE_NAME, *PSTRUCTURE_NAME;
```

1. **`struct _STRUCTURE_NAME { ... }`** defines the struct with the tag name `_STRUCTURE_NAME`.
2. **`typedef ... STRUCTURE_NAME`** creates an alias, so you can write `STRUCTURE_NAME x;` instead of `struct _STRUCTURE_NAME x;`.
3. **`*PSTRUCTURE_NAME`** creates a second alias, a **pointer** to the struct. `PSTRUCTURE_NAME p;` is the same as `STRUCTURE_NAME *p;`.

#### Initializing a Structure
Initializing a structure is the same when using `_STRUCTURE_NAME` or `STRUCTURE_NAME`, as shown below.
```c
STRUCTURE_NAME    struct1 = { 0 };  // The '{ 0 }' part, is used to initialize all the elements of struct1 to zero
// OR
struct _STRUCTURE_NAME   struct2 = { 0 };
```

This is different when initializing the structure pointer, `PSTRUCTURE_NAME`.

```c
PSTRUCTURE_NAME structpointer = NULL;
```

#### Initializing and Accessing Structures Members
```c
typedef struct _STRUCTURE_NAME {
  int ID;
  int Age;
} STRUCTURE_NAME, *PSTRUCTURE_NAME;

STRUCTURE_NAME struct1 = { 0 }; // initialize all elements of struct1 to zero
struct1.ID   = 1470;   // initialize the ID element
struct1.Age  = 34;     // initialize the Age element
```

Another way to initialize the members is using _designated initializer syntax_ where one can specify which members of the structure to initialize.

```c
typedef struct _STRUCTURE_NAME {
  int ID;
  int Age;
} STRUCTURE_NAME, *PSTRUCTURE_NAME;

STRUCTURE_NAME struct1 = { .ID   = 1470,  .Age  = 34}; // initialize both the ID and the Age elements
```

On the other hand, accessing and initializing a structure through its pointer is done via the arrow operator (`->`).

```c
typedef struct _STRUCTURE_NAME {
  int ID;
  int Age;
} STRUCTURE_NAME, *PSTRUCTURE_NAME;

STRUCTURE_NAME struct1 = { .ID   = 1470,  .Age  = 34};

PSTRUCTURE_NAME structpointer = &struct1; // structpointer is a pointer to the 'struct1' structure

// Updating the ID member
structpointer->ID = 8765;
printf("The structure's ID member is now : %d \n", structpointer->ID);
```

The arrow operator can be converted into dot format. For example, `structpointer->ID` is equivalent to `(*structpointer).ID`. That is, `structurepointer` is de-referenced and then accessed directly.

#### Enumeration
An enum (enumeration) in C is a user-defined type that represents a set of named integer constants. By default the first name is assigned the value 0 and each following name is one greater than the previous, but you can assign explicit values to any of them, and the numbering continues from there. They work very well with `switch` statements.

```c
typedef enum {
    RED,        // 0
    GREEN,      // 1
    BLUE = 10,  // 10
    YELLOW      // 11 (continues from the previous value)
} COLOR;
```

#### Union
A Union is a data type that permits the storage of various data types in the same memory location.

```c
union ExampleUnion {
   int    IntegerVar;
   char   CharVar;
   float  FloatVar;
};
```

It's important to note that in a union, assigning a new value to any member will change the value of all other members as well because they share the same memory location to store their data. Additionally, the memory allocated for a union is equal to the size of its largest member.

#### Bitwise Operators
Bitwise operators are operators that manipulate the individual bits of a binary value, performing operations on each corresponding bit position. The bitwise operators are shown below:

- Right shift (`>>`): moves bits right by _n_ positions. The rightmost _n_ bits are discarded and zeros fill in on the left. Example: `10100111 >> 2` → `00101001`.
- Left shift (`<<`): moves bits left by _n_ positions. The leftmost _n_ bits are discarded and zeros fill in on the right. Example: `10100111 << 2` → `10011100`.
- Bitwise OR (`|`)
- Bitwise AND (`&`)
- Bitwise XOR (`^`)
- Bitwise NOT (`~`)

| A   | B   | OR  | AND | XOR |
| --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   |
| 0   | 1   | 1   | 0   | 1   |
| 1   | 0   | 1   | 0   | 1   |
| 1   | 1   | 1   | 1   | 0   |

#### Passing By Value
Passing by value is a method of passing arguments to a function where the argument is a copy of the object's value. This means that when an argument is passed by value, the value of the object is copied and the **function can only modify its local copy of the object's value, not the original object itself.**
#### Passing By Reference
Passing by reference is a method of passing arguments to a function where the argument is a pointer to the object, rather than a copy of the object's value. This means that when an argument is passed by reference, the memory address of the object is passed instead of the value of the object. **The function can then access and modify the object directly, without creating a local copy of the object.**

