---
layout: post
title: "Kernel Hardening in C – Ambiguities and Safety"
date: 2025-08-01 08:33:00 +0200
author: mauricio
categories: [c]
tags: [c, linux, memory-safety]
---

While watching the **Kernel Hardening: Ten Years Deep – Kees Cook, Google** talk on YouTube, I came across a really interesting moment.

👉 [Watch it here](https://www.youtube.com/watch?v=c_NvxSRG50g)

At around the 24-minute mark, Kees starts pointing out the **ambiguous behavior in C**, especially around memory safety, and explains how they're starting to fix things at the compiler level. That really made me reflect on how C—despite being so powerful—is still full of landmines for developers.

## "Uninitialized" Stack Variables? 🤯

In C, there’s no such thing as truly “uninitialized” stack memory. You just get **whatever was sitting there before**.

```c
int function(int input) {
    int on_the_stack;
    return input * on_the_stack;  // unpredictable!
}
```

This is a recipe for subtle bugs. Always initialize your variables!

## Not All Arrays Can Be Bounds Checked

Here's a common case that trips people up:

```c
struct foo {
    int fixed_size_array[16];   // ✅ Bounds checking possible
    int flexible_array[];       // ❌ No bounds checking
};
```

Only fixed-size arrays have dimensions that can be checked. Flexible array members are inherently unsafe unless you manually track their size.

✅ **Tip**: Define your arrays properly and build wrapper functions to enforce access safety.

## Silent Integer Wraparounds No More 🔒

There’s ongoing work to make integer behavior safer with the `__nowrap` attribute.

```c
typedef __nowrap unsigned long size_t;

size_t position = 0;
position--;  // 💥 boom

__nowrap u8 index = 255;
index++;     // 💥 boom
```

No more silent overflows—just clear and predictable crashes. That’s a huge win when debugging.

## Final Thoughts

C gives you low-level power, but it also demands low-level responsibility. For me, these are weak excuses—people have to stop being lazy programmers.

- 🧠 Be explicit. Don't trust default behavior.
- 🧱 Set clear array dimensions and validate your accesses.
- 🔒 Look forward to compiler tools that catch bad patterns before they hit production.

I'll definitely keep following this topic and applying these insights to my embedded projects.

Bis bald! (See you soon!)