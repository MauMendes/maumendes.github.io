---
layout: post
title: "Code Beautifier - Uncrustify"
date: 2025-02-17 19:18:00 +0200
author: mauricio
categories: [c]
tags: [c, uncrustify]
---

In many of my work experiences, I’ve noticed that companies and engineering teams often follow strict code style guidelines. While I’ve always had my own preferences, I’ve learned to adapt over the years. One thing that I’ve observed is that novice programmers often struggle to follow these rules — and perhaps I was the same at one point. :)

To help with this issue and maintain clean code, there's a tool called **Uncrustify**. It automatically formats your code according to the configuration you set.

### Why I Liked it!

- **Keeps it consistent**: Everyone’s code matches.
- **Looks nice**: Easy to read without squinting.
- **Saves time**: Less fiddling with spacing/tabs.
- **Fewer git headaches**: No silly formatting fights.

My favorite part? It stops those sneaky extra spaces/tabs in files that don't have to be commited.

- And **No more fighting over tabs vs. spaces!**


### Installation

Uncrustify is available on Linux, and you can also run it using WSL or Cygwin on Windows. If you're using Ubuntu, you can easily install it with the following command:
```bash 
sudo apt-get install uncrustify
```

Once installed, you can generate a default configuration with:

```bash 
uncrustify --update-config-with-doc -o uncrustify.cfg
```

and if we want the file without documentation you can simply: 

```bash 
uncrustify --update-config -o uncrustify.cfg
```

Luckily, we have some default configurations to start with. You can find them [here](https://github.com/uncrustify/uncrustify/tree/master/etc).
   
Next, tweak the configuration according to your preferences, and then run Uncrustify on your code.

### Examples

Lets see the outputs of this example of C code bellow:
```c
/* Sample.c */
#include<stdio.h> 
int main(void){int n=9;printf("Multiplication table for %d:\n",n);for(int i=1;i<=10;i++){printf("%d x %d = %d\n",n,i,n*i);}return 0;}
```

Uncrustfying using defaults.cfg
```bash 
uncrustify -c defaults.cfg sample.c
```
Results:
```c
/* Sample.c */
#include <stdio.h>
int main(void){
	int n=9; printf("Multiplication table for %d:\n",n); for(int i=1; i<=10; i++){printf("%d x %d = %d\n",n,i,n*i);} return 0;
}
```

Uncrustfying using freebsd.cfg
```bash 
uncrustify -c freebsd.cfg sample.c
```
Results:
```c
/* Sample.c */
#include <stdio.h>
int main(void)
{
	int n = 9;

	printf("Multiplication table for %d:\n", n);
	for (int i = 1; i <= 10; i++)
	{
		printf("%d x %d = %d\n", n, i, n*i);
	}
	return (0);
}
```

Uncrustfying using klaus.cfg
```bash 
uncrustify -c klaus.cfg sample.c
```
Results:
```c
/* Sample.c */
#include <stdio.h>
int main(void)
{
   int n = 9;

   printf("Multiplication table for %d:\n", n);
   for(int i = 1; i <= 10; i++){
       printf("%d x %d = %d\n", n, i, n * i);
       }
   return(0);
}
```

Uncrustfying using kr-indent.cfg
```bash 
uncrustify -c kr-indent.cfg sample.c
```
Results:
```c
/* Sample.c */
#include <stdio.h>
int main(void)
{
	int n = 9; printf("Multiplication table for %d:\n", n); for (int i = 1; i <= 10; i++) {
		printf("%d x %d = %d\n", n, i, n * i);
	}
	return 0;
}
```

Which this results I didn't like much but after adding the code bellow to my kr-indent.cfg, I was quite happy.

```
nl_after_semicolon      = TRUE
nl_before_for           = TRUE
```

Results:
```c
/* Sample.c */
#include <stdio.h>
int main(void)
{
	int n = 9;
	printf("Multiplication table for %d:\n", n);

	for (int i = 1; i <= 10; i++) {
		printf("%d x %d = %d\n", n, i, n * i);
	}
	return 0;
}
```

As you see if you already have a fairly code style, the tool will improve, but tweeks are always necessary.

Viel Spaß! (Have fun, friends)


