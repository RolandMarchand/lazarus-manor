---
Author: Moowool
title: My C Feature Wishlist
date: 2022-10-18
toc: true
tags:
    - C
    - Programming
    - Language Design
license: CC BY 4.0
draft: true
---
* Build system like cargo

## Preface

When I disclose my love for C, I often get asked questions such as «how can you do anything with it, it has no classes», «why not code in Assembly, then?», or the classic «are you a wizard, or something?». It is clear that C is viewed as an historical, outdated, and sometimes mythical language by most high level programmers who began with Python and Javascript.

There is a kernel of truth to this image. Almost all modern and widely used languages today can trace back their origins to C. The language is also known for its quirks like undefined behavior, or its missing features like libraries of data structures and algorithms, proper string objects or, well, objects. Not to mention the ever so panic-inducing *pointers*, indecently exposed to the user.

It shocks people to learn that operating systems, media players, browsers, text editors and graphics software are written in C. They can't understand how such a primitive and unsafe language can pull this off.

However, those impressions melt off soon after getting comfortable with the language, which surprisingly doesn't take much time at all. Memory gets demystified as practically a huge array, data structures such as hashmaps and resizable arrays are learned and then written in only a few hundred lines, and libraries such as [Boehm garbage collector](https://www.hboehm.info/gc/index.html), [OpenGL](https://www.opengl.org/), or [SQLite](https://www.sqlite.org/index.html) are used.

Evidently, there is a reason why we developed and moved onto different languages. As Good as C still is, syntactic sugar is welcome, and some features are mandatory for any significant project.

## Syntactic Sugar

### Associated Functions

You will write a lot of data structures in C, and their associated interfaces of functions. those functions' first argument will often be a pointer to the structure you're accessing, which looks like this:
```c
struct counter { /* data */ };
void hashmap_init(struct hashmap *hm) { /* initialize */ }
void hashmap_add(struct hashmap *hm, char *key, int value) { /* add value */ }
void hashmap_get(struct hashmap *hm, char *key) { /* retreive value */ }

int main() {
	struct hashmap hm;
	hashmap_init(&hm);
	hashmap_add(&hm, "hello", 3);
	return hashmap_get(&hm, "hello");
}
```
Static functions accessing structures from a reference is common in most languages with data structures. Common enough to often be granted its own syntax, associated functions. I am especially an fan of Lua's syntax:
```lua
counter = { value = 0 }
function counter:increment()
	self.value = self.value + 1
	return self.value
end
print(counter:increment()) -- print 1
```
This doesn't provide any extra functionality, it is purely syntactic. Nonetheless, adopting it in C would decrease verbosity at no cost. Here is Lua's `counter` example from earlier with associate functions:
```c
#include <stdio.h>

typedef struct { int value; } counter;
void counter:init() { self.value = 0; }
void counter:increment() { return self.value += 1; }

int main() {
	struct counter cnt;
	cnt:init();
	printf("%d\n", cnt:increment());
}
```
