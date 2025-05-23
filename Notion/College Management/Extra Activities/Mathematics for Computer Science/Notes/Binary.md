---
Created: 2023-10-05T00:47
Type: Lecture
Materials:
  - "[[Chapter_1.pdf]]"
Reviewed: true
---
# Decimal to Binary

You can transform decimal numbers to binary by continually dividing by 2 and then taking the remainder for large numbers. The remainder will either be 0 or 1. Then you can read it from bottom to top.

- For small numbers, you can just subtract.

  

## Floats to Binary

You compute the integer part and the decimal part separately. In terms of the decimal point,

  

123.25

  

we multiply the decimal point by 2 repeatedly then take the integer part. So 0.25 x 2 = 0.50. The integer part is 0, so we note 0. as the first bit. Then 0.50 x 2 is 1.00 which now 01. Now, there is no decimal point to multiply by as 0 x 2 is just 0, so we stop.

  

It is now: 1111011.01

  

The same is for octal and hexadecimal.

  

## Binary to Floats

  

1101.101

We compute both parts separately.

After the point, we always divide by 2. So,

the first bit will be 1/2

second will be 1/4

third will be 1/8

So, in this case, it will be 13 + 0.50 + 0.125 as only 1/2 and 1/8 have ones in them.

  

The same is for octal and decimals in the process for float conversion.

  

# Binary to Decimal

You can do it by adding all of the place values by the binary values.

  

  

# Decimal to Octal

You can repeatedly divide by 8 and take the remainder.

  

## Octal to Decimal

- You can just multiply the octal digit by the place value of it.

  

# Decimal to Hexadecimal

You can repeatedly divide by 16 and take the remainder.

  

## Hexadecimal to Decimal

- you can just multiply the hexadecimal digit by the place value of it.