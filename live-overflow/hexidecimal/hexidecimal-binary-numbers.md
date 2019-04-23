https://www.youtube.com/watch?v=mT1V7IL2FHY

###Hexidecimal
All computers work in binary, 0 and 1 which is a representation of our electronic circuits (switches) that turn on and off.

You may know a byte is a bit. This wasn't always the case, earlier computers might have had 6 bits for a byte, it was old IBM systems that used 8 bits per byte which ended up becoming the norm. Often people will refer to bytes as **octets** to ensure people understand they are 8bits to a byte.

An interesting example is ascii, which only uses 7 bits.

Instead of thinking of numbers as something fixed, think of it as a tool where we can use different number systems which make sense.

When we look at binary as raw bits, we quickly see that it take up a lot of space. `00000000` to represent the number 0 takes up a large amount of space compared to regular decimal.

We can convert a number from binary to decimal in Python by saying the string represents a number in base 2:

```python {cmd="python2"}
print(int('1111', 2))
```

With this, we can do this for a couple of examples with a simple for loop:

######Python2
```python {cmd="python2"}

for i in ["00000000","00000010","01010101","11110010","11111111"]:
  print "{0} | {1:3}".format(i, int(i,2))
```
It takes less space but the biggest number in a byte is 255 which is an unintereresting number in decimal.

We can then extend our for loop to accomodate for hex numbers:

```python {cmd="python2"}

for i in ["00000000","00000010","01010101","11110010","11111111"]:
  print "{0} | {1:3} | {2:2x}".format(i, int(i,2), int(i,2))
```

Now we see that the space used is even smaller than decimal. We can also see that the highest value represented in binary is also the highest value represented in Hexidecimal as hexidecimal is base-16 and includes characters from `0123456789ABCDEF`.

Now we understand why decimal representation is not good at this type of thing. For example, the maximum value of an unsigned 32-bit integer in decimal is `2147483647` which is an uniteresting seemingly random number. In Hex: `32bit = 4 * 8bit` which is `0xFFFFFFFF`.

###Python
Python has some built in functions to convert numbers.
