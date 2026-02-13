---
title: C as a Scripting Language
summary: How to use C as a scripting language for a game engine.
description: I am writing my own game engine, and like most game engines, mine needs a scripting language. VMs are slower than native code, consume more memory, and require tedious binding management between the scripting language and engine. More importantly, I prefer single-language codebases because they let me use the same editor, debugger, and development workflow throughout the entire project. My engine is written in C, so how can I use C as a scripting language?
date: 2025-07-30
image: thumbnail.webp
categories:
    - Computer Science
tags:
    - C
    - Computer Science
    - Programming
    - Game Engine
authors:
    - lazarus-overlook
---

I am writing my own [game engine](https://github.com/RolandMarchand/murder-engine) and like most game engines, mine needs a scripting language. Think GDScript for Godot or C# for Unity. Scripts are essentially programs that call the engine's core functions, and while most engines solve this with virtual machines, I wasn't satisfied with that approach. VMs are slower than native code, consume more memory, and require tedious binding management between the scripting language and engine. More importantly, I prefer single-language codebases because they let me use the same editor, debugger, and development workflow throughout the entire project. My engine is written in C, so how can I use C as a scripting language?

C as a scripting language is a problem because it is compiled. So if I want to change a script, I have to recompile the whole engine, or at least the script objects and then link the whole engine. This is clunky and takes forever, but I didn't know how to use C seamlessly as the scripting language, so I gave in and tried the VM-based languages [Janet](https://janet-lang.org/) and [Wren](https://wren.io/). They are both great languages and I encourage readers to look into them, but I wasn't able to figure out Janet's C API due to the poor documentation, and Wren's lack of LSP pushed me to look elsewhere, even if the embedding experience was vastly better than Janet's.

That's where this neat trick comes in: **runtime symbol linking**! Essentially, I compile the engine with all its global symbols (including functions) exposed, and I compile the script as a library with position-independent code that can be loaded at any memory address without modification. Then, I link that library at runtime against the exposed symbols of the engine, and voilà! This was quite a mouthful, but hopefully the example below will make things clearer.

This is very similar to how the Linux kernel loads kernel modules, which was my main inspiration.

```c
// engine.c

#include <stdio.h>
#include <stdlib.h>
#include <dlfcn.h>

// Internal engine function accessed by scripts
void spawn_entity(void) {
  printf("Entity spawned!\n");
}

int main(void) {
  void *handle = dlopen("./script.so", RTLD_LAZY);
  if (!handle) {
    fprintf(stderr, "Unable to load script\n");
    exit(1);
  }

  void (*script_update)(void) = dlsym(handle, "script_update");
  if (!script_update) {
    fprintf(stderr, "Unable to run script\n");
	exit(2);
  }

  script_update();

  if (dlclose(handle) != 0) {
        fprintf(stderr, "dlclose error: %s\n", dlerror());
        exit(3);
  }
}
```

```c
// script.c

#include <stdio.h>

void spawn_entity(void);

void script_update(void) {
  printf("Script called!\n");
  spawn_entity();
}
```

```bash
# Compilation

$ gcc -rdynamic -ldl engine.c -o engine
$ gcc -shared -fPIC script.c -o script.so
$ ./engine
Script called!
Entity spawned!
```

{{< alert "circle-info" >}}
Explanation of the compilation tags used:
- `-rdynamic` exposes the symbol table
- `-ldl` links `dlfcn.h`
- `-shared` compiles a library instead of an executable
- `-fPIC` makes the target's code position-independent so it can be loaded at any memory address
{{< /alert >}}

Here are the benefits:
- Near-native performance from the minimal C runtime with GCC/Clang optimizations
- No glue code to maintain
- I can modify and hot reload scripts without recompiling, relinking, or even restarting the engine
- I can use all the same mature C development tools for both engine development and scripting
- Game developers and modders can write scripts without needing the engine's source code

## Privacy

By default, this method exposes *all* of the engine's symbols. I don't think that's a problem for my use case, but if it ever becomes one, I can manage which symbols get exposed in the symbol table with the GCC/Clang attribute `__attribute__((visibility("default")))` and the compilation flag `-fvisibility=hidden`. This tells the compiler "don't expose any symbol, except the ones I specify." This is even more similar to how the Linux kernel module system works.

## Memory management

It is worth noting that manually managing memory in a scripting language is tedious and error-prone, but my engine is compiled with the [Boehm Garbage Collector](https://hboehm.info/gc/), so scripts can manage memory automatically if they want, which is encouraged.

Although, if scripts want to use more efficient memory management methods such as object pools + free lists, or arenas (my engine uses a lot of GNU Obstacks internally), then the scripts are free to do so.

## Stability

Another drawback is stability. In VM-based engines, if the VM runs code that crashes such as dereferencing a null pointer, then the engine can still run. But with my system, if a script crashes, the whole engine goes kaboom. This is a risk I'm willing to take since VM crashes at runtime are still unacceptable to players, so what's a few seg faults?

For those who cannot accept this drawback, the issue can be mitigated by forking the script process and to communicate with the engine process using IPC, but that's a lot of performance overhead and technical complexity. Threads do not work since they share memory with the main process, so a thread crashing brings down the whole process along with it. And using guard pages like the Linux kernel could also work and have less overhead than forking, at the cost of technical complexity.

## Why not Lua?

Great question!

## Could this work with other compiled languages such as C++, Rust, or Zig?

Potentially!
