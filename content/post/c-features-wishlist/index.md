---
Author: Moowool L. Summerwheat
title: My C Feature Wishlist
date: 2023-03-28
toc: true
tags:
    - C
    - Programming
    - Language Design
license: CC BY 4.0
draft: true
---

## Preface

When I disclose my love for C, I often get asked questions such as «how can you code anything with it, it has no classes», «why not code in Assembly, then?», or the classic «are you a wizard, or something?». It is clear that C is viewed as an historical, outdated, and sometimes mythical language by most high level programmers who began with Python and Javascript.

There is a kernel of truth to this image. Unlike most high level, modern languages and frameworks, I consider it mandatory to have a basic understanding of how C works under the hood. That means learning the different memory sections and their layouts, to know the steps of the compilation process, and to learn the language's quirks like undefined behavior. Yes, this includes the ever so panic-inducing *pointers*, indecently exposed to the user.

Even while knowing the ins-and-outs of the language, it may feel limiting to lack features like certain libraries of data structures and algorithms, proper string objects or, well, objects. It shocks people to learn that operating systems, media players, browsers, text editors and graphics software can be written in C. They don't understand how such a primitive and unsafe language can pull this off.

However, those impressions melt away soon after getting comfortable with the language, which surprisingly doesn't take much time. Memory gets demystified as practically a huge array. Data structures like hashmaps and resizable arrays are learned and then written in only a few hundred lines. And libraries like [Boehm garbage collector](https://www.hboehm.info/gc/index.html), [OpenGL](https://www.opengl.org/), or [SQLite](https://www.sqlite.org/index.html) are used.

Evidently, there is a reason why we developed and moved onto different languages. Programming styles evolve, average project scopes increase, and new features are not considered mandatory. Even though C cannot, and should not fulfill all use cases, it still has its place and its users in the modern era. These features I'm proposing are designed with this in mind.

## Syntactic Sugar

### Associated Functions

You will write a lot of data structures in C, and their associated interfaces of functions. those functions' first argument will often be a pointer to the structure you're accessing, which in practice looks like this:
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
Static functions accessing structures from a reference is common in most languages with data structures. Often common enough to be granted its own syntax, associated functions. I am especially an fan of Lua's syntax:
```lua
counter = { value = 0 }
function counter:increment()
	self.value = self.value + 1
	return self.value
end
print(counter:increment()) -- print 1
```
This doesn't provide any extra functionality, it is purely syntactic. Nonetheless, adopting it in C would decrease verbosity at no cost. Here is Lua's `counter` example from earlier with associate functions in C:
```c
#include <stdio.h>

typedef struct { int value; } counter;
void counter:init() { self.value = 0; }
int counter:increment() { return self.value += 1; }

int main() {
	counter cnt;
	cnt:init();
	printf("%d\n", cnt:increment());
}
```
