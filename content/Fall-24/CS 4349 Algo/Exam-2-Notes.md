> [!important]- Exam 2 Topics
> Greedy Algorithms

# Greedy Algorithms

- They always choose the locally optimal choice, and in total can sometimes produce the mathematically optimal solution globally.

## Activity Selection Problem

- Define
  - Activities ai and aj are compatible if the intervals (si,fi) and (sj,fj) do not overlap
- Goal:
  - Schedule activities that require exclusive use of a shared resource, and find the maximum-size set of compatible activities
- Input:
  - A set S of activities
  - Each activity has a start time si and a finishing time fi where 0 < si < fi < inf

## Huffman Codes

- A binary code that assigns a binary number for every character. using such we can make an optimal prefix code that is used for lossless data compression
- Using variable length codes where the more frequent characters are stored with more bits
  - 1 = a , 10 = b, 11 = c ...
- Algorithm to store data - Starting with the two least frequent characters, merge them in a tree, then recurse - You will end up with a tree of the most common character as the left child, and all other characters down the right child - When you go down one layer, the left child will be the second more common, with the right child having all characters except the two most frequent - We can use this tree to encode the variable length binary numbers for all the characters - This ENSURES that no characters have a prefix that is another character. they create an unambiguous set - For full alphabets, the last letters are very long in bit length, but since the common letters are 1,2,3 bits long, the total file is compressed 20-90%
  > [!example]- Huffman Tree Example
  >
  > - Given Text: ABC BB AAA AAA D DZ BB
  > - Frequency sort {A,B,C,D,Z}
  > - Fixed Length
  >   - Assign each character a number {001,010,011,100,101}
  > - Variable Length
  >   - Assign each character a number (remove leading 0s) {1, 10, 11, 100}
  > - Replace all characters in the text with their binary representation
