# picoCTF 2022: Forensics: File types

## Introduction
**Challenge:** [File types](https://play.picoctf.org/practice/challenge/268)

**Category**: Forensics

**Description**
This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.
You can download the file from [here](https://artifacts.picoctf.net/c/324/Flag.pdf).

## Solution
The file that was given, I am pretty sure is not a `pdf` file. We can check the type of file using the `file` command.

```bash
file Flag.pdf

Flag.pdf: shell archive text
```
It's a `shell archive text`. As far as I know, we can extract it just by executing the file.
```bash
chmod +x Flag.pdf
./Flag.pdf
```
After executing these commands, you will get a file extracted called `flag`. Let's what file type it is.
```bash
file flag

flag: current ar archive
```
As we can see it's an `ar` archive. So let's extract it as well.
```bash
ar xv flag
```
We can continue this process of extracting the files by using their file types. In the end, we are gonna get the flag encoded in hex like this:
```
7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f33343765616536357d0a
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666597003421/MRN-Ailck.png align="left")

Therefore, The flag is `picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_347eae65}`

## Conclusion
This challenge was very annoying I would say. You need to extract a lot of files from files. But In the end, we got the flag in a hex form.

**Flag:** `picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_347eae65}`