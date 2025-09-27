---
title: I Made a Better Dynamic Array than stb_ds.h
summary: A production-ready, macro-based vector library to generate type-safe dynamic arrays.
description: vector.h is a production-ready, macro-based C library that generates type-safe dynamic arrays for any data type, offering compile-time type checking, fast iteration, and robust memory management with zero dependencies. Unlike alternatives like stb_ds.h, it provides enhanced type safety, optimized performance, and superior debugging experience while maintaining C89 compatibility across all major compilers and architectures.
date: 2025-09-27
image: thumbnail.webp
categories:
    - Computer Science
tags:
    - C
    - Computer Science
    - Programming
authors:
    - lazarus-overlook
---

{{< github repo="RolandMarchand/vector.h" showThumbnail=true >}}

## vector.h - Type-Safe Dynamic Arrays for C

A production-ready, macro-based vector library to generate type-safe dynamic arrays.

```c
#include "vector.h"

VECTOR_DECLARE(IntVector, int_vector, int)
VECTOR_DEFINE(IntVector, int_vector, int)

int main(void) {
	IntVector nums = {0};

	int_vector_push(&nums, 42);
	int_vector_push(&nums, 13);

	printf("First: %d\n", int_vector_get(&nums, 0));  /* 42 */
	printf("Size: %zu\n", VECTOR_SIZE(&nums));        /* 2 */

	int_vector_free(&nums);
	return 0;
}
```

## Features

- **Type-safe**: Generate vectors for any type
- **Portable**: C89 compatible, tested on GCC/Clang/MSVC/ICX across x86_64/ARM64
- **Robust**: Comprehensive bounds checking and memory management
- **Configurable**: Custom allocators, null-pointer policies
- **Zero dependencies**: Just standard C library

## Quick Start

1. Include the header and declare your vector type:
```c
/* my_vector.h */
#include "vector.h"
VECTOR_DECLARE(MyVector, my_vector, float)
```

2. Define the implementation (usually in a .c file):
```c
#include "my_vector.h"
VECTOR_DEFINE(MyVector, my_vector, float)
```

3. Use it:
```c
MyVector v = {0};
my_vector_push(&v, 3.14f);
my_vector_push(&v, 2.71f);

/* Fast iteration */
for (float *f = v.begin; f != v.end; f++) {
	printf("%.2f ", *f);
}

my_vector_free(&v);
```

Alternatively, if you plan to use your vector within a single file, you can do:
```c
#include "vector.h"
VECTOR_DECLARE(MyVector, my_vector, float)
VECTOR_DEFINE(MyVector, my_vector, float)
```

## API Overview

- `VECTOR_SIZE(vec)` - Get element count
- `VECTOR_CAPACITY(vec)` - Get allocated capacity
- `vector_push(vec, value)` - Append element (O(1) amortized)
- `vector_pop(vec)` - Remove and return last element
- `vector_get(vec, idx)` / `vector_set(vec, idx, value)` - Random access
- `vector_insert(vec, idx, value)` / `vector_delete(vec, idx)` - Insert/remove at index
- `vector_clear(vec)` - Remove all elements
- `vector_free(vec)` - Deallocate memory

## Configuration

Define before including the library:

```c
#define VECTOR_NO_PANIC_ON_NULL 1       /* Return silently on NULL instead of panic */
#define VECTOR_REALLOC my_realloc       /* Custom allocator */
#define VECTOR_FREE my_free             /* Custom deallocator */
```

## Testing

```bash
mkdir build
cmake -S . -B build/ -DCMAKE_BUILD_TYPE=Debug
cd build
make test
```

Tests cover normal operation, edge cases, out-of-memory conditions, and null pointer handling.

## Why vector.h Over [stb_ds.h](https://github.com/nothings/stb/blob/master/stb_ds.h)?

- **Just as convenient**: Both are single-header libraries
- **Enhanced type safety**: Unlike stb_ds.h, vector.h provides compile-time type checking
- **Optimized iteration**: vector.h uses the same iteration technique as std::vector, while stb_ds.h requires slower index calculations
- **Safer memory management**: vector.h avoids the undefined behavior that stb_ds.h uses to hide headers behind data
- **Superior debugging experience**: vector.h exposes its straightforward pointer-based internals, whereas stb_ds.h hides header data from debugging tools
- **Robust error handling**: vector.h fails fast with clear panic messages on out-of-bounds access, while stb_ds.h silently corrupts memory
- **More permissive licensing**: vector.h uses BSD0 (no attribution required, unlimited relicensing), which is less restrictive than stb_ds.h's MIT and more universal than its public domain option since the concept doesn't exist in some jurisdictions

## Contribution

Contributors and library hackers should work on `vector.in.h` instead of
`vector.h`. It is a version of the library with hardcoded types and function
names. To generate the final library from it, run `libgen.py`.

## License

This library is licensed under the BSD Zero license.
