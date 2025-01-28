---
id: jan27
aliases:
  - l1
tags:
  - lecture
---

# jan 27

Date created: 2025-01-27
Time created: 17:52

## w2 learning objectives (at a minimum be able to)
- [x] draw and label basic memory diagram showing name, value, type for given code
- [x] draw and label linear memory diagram showing name, address, hex contents of variable
- [x] show binary representation and byte ordering for int, char, address, values
- [x] declare, assign, and dereference pointer variables
- use stdlib.h functions malloc and free to manage dynamically allocated “heap” memory
- code, describe, and diagram 1D arrays showing stack and on heap allocations
- show byte representation of character arrays and C strings
- use string.h library functions: strlen, strcpy, strncpy, strcmp with string literals and C strings

## malloc and calloc
functions that obtain blocks of memory dynamically.
``` void *malloc(size_t n)``` 
returns a pointer to n bytes of uninitialized store, or ```NULL``` if the request cannot be statisfied.

```void *calloc(size_t n, size_t size)``` 
returns a pointer to enough free space for an array of ```n``` objects of the specified size, or NULL if the request cannot be statisfied. The storage is initialized to 0.

The pointer returned by malloc or calloc has the proper alignment for the object in question, but has to be cast
into the appropriate type, as in -
```int *ip; ip = (int*) calloc(n, sizeof(int));```

```free(p)``` frees that space pointed to by ```p```, where ```p``` was originally obtained by a call to ```malloc```
or ```calloc```. There are no restrictions on the order in which space is freed, but it's BAD to free something that
isn't obtained by calling ```malloc``` or ```calloc```. Similary it's an error to use something after it's been freed.

For example this loop won't work -
``` for (p = head; p != NULL; p = p->next) { free(p); }```

But this is the right way to do it -
``` for (p = head; p != NULL; p = q) { q = p->next; free(p); }```

## variables
- What? - scalar var is primitive unit of storage whose ontents can change
    - memory diagram for ```void someFunction() { int i = 44; }```
    - i -> [binary representation of 44]

### aspects
- identifier
- value
- type
- address
- size
    - often tied with type (e.g. int = 4 bytes, char = 1, double = 8, etc.)

### linear diagram
- view of memory as a sequence of bytes
    - e.g. 0x0000FF28
        - next addresses would be 29 -> 2A -> 2B -> ... -> 2F -> 30
- byte addressability
    - each address itentifies 1 byte
- endianess - ordering of bytes
    - little - least significant goes in the lowest address
    - big - most significant goes in the lowest address

## pointers
- What? - scalar var whose value is a memory address
- Why?
    - indirect access to memory
    - common C libraries
    - access to a machine's memory mapped hardware
- example
    - ```int *ptr = NULL;```
    - initial value is 0x0
    - address (is 0x_C
    - type is int*
    - size is 4 bytes, pointers take up however many bytes the architecture deems necessary

## 1D arrays
- What? - compound unit of storage (not scalar) with elements of the same type
- access via identifier and indexing
- allocated as a contiguous fixed size block of memory

### function with pointers
- e.g. ```int a[5];``` (stack allocated array)

#### address arithmetic
- ```a[i]```
    - ```a[i]``` is equivalent to ```*(a + i)```
    - e.g. assigning ```a[3] = 33``` would be ```*(a + 3) = 33```

#### pointers with arrays
- a pointer having the address of array a would be ```int *p = a```
- assigning ```a[4] = 44``` would be ```*(p + 4) = 44```




