---
title: "A Deep Dive in Computer Number Systems"
seoDescription: "Number system is a set of values used to represent different quantities. In day-to-day life, we use the decimal number system, which has a base of 10."
datePublished: Wed Apr 20 2022 09:29:55 GMT+0000 (Coordinated Universal Time)
cuid: cl27dj07m013i4ynvhuek1e5n
slug: deep-dive-in-computer-number-systems
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650446844116/TyfpDB7vB.png
tags: programming, computer-science

---

# What Are Number Systems?
**Number System** is a set of values used to represent different quantities. In day-to-day life, we use the decimal number system, which has a base of 10 as it uses 10 digits (0-9). The computer represents all kinds of data and information (like text, numbers, graphics, and videos) in binary numbers that have a base of 2 as the computer uses 2 digits (0, and 1). Binary is the most widely used number system in computers, but there are more like hexadecimal (used for storing memory addresses) and octal (used in the aviation sector). Values from one number system can be converted to the other number system.

# Binary Number System
Binary means composed of two. The binary number system uses only two digits, 0 and 1. It is widely used in computers. You know that the decimal number system has ten digits from 0 to 9. As the decimal number system has ten digits, it is called the Base 10 System. Similarly, the binary number system has only two digits, 0 and 1, so it is called the Base 2 System.

## Conversions
Now we will learn how we can convert decimal to binary numbers and convert binary to decimal
### From Binary To Decimal
Since the binary number system is based on two digits, we take 2 as its base. Any binary number has positions starting from the decimal point.

**For example**: 10101.011

| Digit 	| Position 	|
|---	|---	|
| 1 	| 0th 	|
| 0 	| 1st 	|
| 1 	| 2nd 	|
| 0 	| 3rd 	|
| 1 	| 4th 	|
| . 	|  	|
| 0 	| -1st 	|
| 1 	| -2nd 	|
| 1 	| -3rd 	|

So, to convert **10101.011** into a decimal number, you have to add the product of the binary digit and its position in the power base 2 as shown

![Explanation Of Conversion](https://cdn.hashnode.com/res/hashnode/image/upload/v1650382726041/oiYfTQj4m.png)

Now, let's covert after the decimal point.

![Explanation Of Coversion](https://cdn.hashnode.com/res/hashnode/image/upload/v1650445408675/56ClnF4h_.png)

### From Decimal To Binary

For converting decimal to binary we need to keep dividing the decimal number by 2 until we get to 0 and we also need to record the reminder and join it.

**For example**: 8

| Diviser	|  Quotient	|  Reminder 	|
|---	|---	| ---	|
| 2 	|  8 	|   	| 
| 2 	|  4 	|  0 	| 
| 2 	|  2 	|  0 	| 
| 2 	|  1 	|  0 	| 
| 2 	|  0 	|  1 	| 

8 in decimal is 1000 in binary

## Octal Number System
The octal number system uses eight digits (0 to 8) instead of 2 like there is in binary. It is widely used in computers. You know that the decimal number system has ten digits from 0 to 9. As the decimal number system has ten digits, it is called the Base 10 System. Similarly, the octal number system has only eight digits (0 to 8) so it is called the Base 8 System.

# Conversions
All the conversion that we have learned for the binary number system will apply to octal as well but there is gonna be a small change instead of multiplying or dividing with 2 you need to multiply or divide with 8 because octal is a Base 8 number system.