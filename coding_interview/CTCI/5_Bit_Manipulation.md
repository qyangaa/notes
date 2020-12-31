# 5 Bit Manipulation

p124

### Runtime

A [barrel shifter](http://en.wikipedia.org/wiki/Barrel_shifter) allows the shift to be performed in `O(log n)` passes — which may be done in the same clock cycle, making shifting an `O(1)` operation.

### Manual Manipulation Exercises

+ 0110+0010 = 1000
+ 0011+0010=0101
+ 0110-0011=0011
+ 1000-0110 = 0010
+ 0011*0101 = 1111
+ 0011*0011=1001
+ 1101>>2 = 0011: Shift right
+ 1101 ^ 0101 = 1000: XOR, exclusive or
+ 0110+0110 = 0110*2 = shift left by 1 = 1100
+ 0100*0011 = $4 * 0011$ = shift 0011 left by 2 = 1100
+ 1101^(~1101): a^(~a) will always be 1s
+ 1011 & (~0<<2) = 1001 &(1s<<2) = 1001 & (1s00) = 1000

### Bit Facts and Tricks

$$
\begin{array}{llll}
x \wedge 0 s=x & x \& 0 s=0 & x \mid 0 s=x \\
x \wedge 1 s=\sim x & x \& 1 s=x & x \mid 1 s=15 \\
x \wedge x=0 & x \& x=x & x \mid x=x
\end{array}
$$



+ x^0s = x: only 1's in x are 1
+ x^1s = ~x: only 0's in x are 1
+ x|1s = 1111 = 15

### Two's complement

+ First bit: sign, negative if 1

+ 4 bit integer, first bit as sign

  + 0-7: 8 numbers with normal bit representation

  + -1~-7: 7 negative numbers with first bit -1, representation of $-K$ in $N$ bit number is $concat(1, 2^{N-1}-K)$: 

    | -1    | -2    | -3    | -4    | -5    | -6    | -7    |
    | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
    | 1 111 | 1 110 | 1 101 | 1 100 | 1 011 | 1 010 | 1 001 |

### Shift

+ Arithmetic right shift >>:  fill left most bit with value of sign bit, equivalent to divide by 2. 

  ```java
  x=-93242
  do for 40 times: x>>=1
  Fill all digits with 1, always give -1
  ```

+ Logical right shift >>>:  put 0 on left most (most siginificant) bit

  ```java
  x=-93242
  do for 40 times: x>>>=1
  Fill all digits with 0, always give 0
  ```

+ Left shift: fill 0's on the right

  + Arithmetic *left* shifts are equivalent to multiplication by 2. 
  + Logical left shifts are also equivalent
  + multiplication and arithmetic shifts may trigger [arithmetic overflow](https://en.wikipedia.org/wiki/Arithmetic_overflow) whereas logical shifts do not.

### Common Bit Tasks

```java
boolean getBit(int num, int i){
    return((num & (1<<i))!=0);
}
```

```java
boolean setBit(int num, int i){
    return num | (1<<i);
}
```

```java
int clearBit(int num, int i){
    return num & (~(1<<i));
}
```

```java
int clearBitsMSBthroughI(int num, int i){
    //from the most significant big through i (inclusive)
    int mask = (1<<i)-1;//00011111
    return num & mask;
}
```

```java
int clearBitsIThrough0(int num, int i){
    int mask = (-1<<(i+1)); //1110000
    return num & mask;
}
```

```java
int updateBit(int num, int i, boolean value){
	int mask = ~(1<<i);//11101111
    return (num & mask)| (value<<i);
}
```

```java
int[] convertBinay(int num){
    int binary[] =  new int[40];
    int index = 0;
    while(num>0){
        binary[(39-index++)] = num%2;
        num = num/2;
    }
}
```





## Interview Questions

p288

### 5.1 Insertion

You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to insert M into N such that M starts at bit j and ends at bit i. You can assume that the bits j through i have enough space to fit all of M. That is, if M= 10011, you can assume that there are at least 5 bits between j and i. You would not, for example, have j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.

+ Draw Example

  + Input: N = 10000000000, M = 10011, i=2, j = 6
  + Output: N = 10001001100

+ Brute Force

  + read N bit by bit and when getting into the replace region, read M bit by bit, and return to N after the replace region

+ Optimize + Conceptual algorithm walk through

  + Clear bits i through j in N
  + shift M left by i
  + merge N and M by OR

+ Implement + Test

  + ```java
    int clearBits(int n, int m, int i, int j){
        //eg: i=2, j=4
        int allOnes = ~0;
        int left = allOnes<<(j+1);//11100000
        int right = ((1<<i)-1);//00000011
        int mask = left | right;//merge: 11100011
        int n_cleared = n & mask;
        int m_shifted = m<<i;
        return n_cleared | m_shifted;
    }
    ```

+ Tips

  + 1's on left: (~0)<<(i+1)
  + 1's on right: ((1<<i)-1)
  + use masks to clear
  + use OR to merge



### 5.2 Binary to String

Given a real number between 0 and 1 (e.g., 0.72) that is passed in as a double,
print the binary representation. If the number cannot be represented accurately in binary with at most 32 characters, print "ERROR:

+ Draw Example

  + Binary decimal number:
    $$
    0.101_2 = 0*1/2^0+1*1/2^1+0*1/2^2+1*1/2^3
    $$

  + 

+ Brute Force

  + Print decimal part, to get each digit: multiply by 2 (left shift) and check if the resulting number is larger than 1

+ Optimize + Conceptual algorithm walk through

+ Implement + Test

  + ```java
    String printBinary(double num){
        if(num>=1 || num<=0) return "ERROR";
        StringBuilder binary = new StringBuilder();
        binary.append(".");
        while(num>0){
            if(binary.length()>=32) return "ERROR";
            double r = num*2;
            if(r>=1){
                binary.append(1);
                num = r-1;
            }else{
                binary.append(0);
                num = r;
            }
        }
        return binary.toString();
    }
    ```

+ Tips

  + Convert integer to binary: %2 get remainder
  + Convert 0-1 decimal to binary: *2 get part >1

### 5.3 Flip bit to win

You have an integer and you can flip exactly one bit from a 0 to a 1. Write code to
find the length of the longest sequence of 1s you could create.

+ Draw Example

  + 1775 (1101101111) -> 8

+ Brute Force

  + For each index, flip and count ($O(b^2)$)

+ Optimize + Conceptual algorithm walk through

  + Two pointer: 
    + first pointer is start of sequence, either start of number or right after 0
    + Second pointer is end of last sequence, either end of number of right before next 0
  + Separate into fragments of 1's sequence, and find the largest adjacent sum of length $O(b)$
  + Algorithm:
    + keep record of maxAdjacentSum, prevLen, curLen
    + %2 to get each digit
      + if =1: curLen++
      + if=0: 
        + maxAdjacentSum = max(maxAdjacentSum, prevLen+curLen)
        +  prevLen = curLen, curLen = 0

+ Implement + Test

  + ```java
        public int answer(int a) {
            if (~a ==0) return Integer.BYTES *8;
            int maxAdjSum = 0;
            int preLen = 0;
            int curLen = 0;
            while(a!=0){
                if(a%2==1){
                    curLen++;
                }else{
                    maxAdjSum = Math.max(maxAdjSum, preLen+curLen+1);
                    preLen = curLen;
                    curLen = 0;
                }
                a>>>=1;
            }
            return maxAdjSum;
        }
    ```

+ Tips

  + $a \% 2==1$ or $(a\&1)==1$, then $a>>>=1$ to get each bit from an interger



### 5.4 Next number

Given a positive integer, print the next smallest number that
have the same number of 1 bits in their binary representation.

+ Draw Example

  + 0010:  0100
  + 0110: 1001
  + 1011: 1101

+ Brute Force

  + Increment/ decrement and check

+ Optimize + Conceptual algorithm walk through

  + Closest bigger: 

    + switch the right most 0 with 1 on right with that 1 (get bigger)
      + digit i: location of 0
      + clear bit at i-1 and set bit at i: $(-2^{i-1}+2^i = +2^{i-1})$
    + Shift the remaining 1s all the way to the right (as small as possible)
      + digit j: location of last 1
      + Total number of 1's after i is i-j-1
      + clear all bits after i: $(n//2^i)$
      + set i-j-1 bits on the right $(+2^{i-j-1}-1)$

    $$
    1: \boxed{0} \quad \boxed{1}\quad \boxed{1} \quad  \boxed{0}  \Leftrightarrow \boxed{1} \quad \boxed{1} \quad \boxed{1} \quad \boxed{0}\\
    2: \boxed{0} \quad \boxed{1}\quad \boxed{1} \quad  \boxed{1} \quad\boxed{0} \quad \underbrace{ \boxed{1} \quad \boxed{1}}_{>>} \quad \boxed{0}\\
    3: \boxed{0} \quad \boxed{1}\quad \boxed{1} \quad  \boxed{1} \quad\boxed{0} \quad \boxed{0} \quad \boxed{1} \quad \boxed{1}
    $$

  + Or: 

    + Set the binary starting at i tobe: 10000 $(+ 2^j)$
    + set i-j-1 bits on the right $(+2^{i-j-1}-1)$ can change $i-j$ to $i$  when implementing

+ Implement + Test

  + ```java
        public int answer(int n) {
            int temp = n;
            int i = 0;
            int j = 0;
            while (temp%2 == 0){
                j++;
                temp>>=1;
            }
            if(temp==0) return -1;
            while(temp%2==1){
                i++;
                temp >>=1;
            }
    
            return n+(1<<j)+(1<<(i+j-1)) -1;
        }
        public void test(){
            System.out.println(Integer.toBinaryString(13948));
            System.out.println(Integer.toBinaryString(answer(13948)));
        }
    ```

+ Tips

  + Change bit manipulation to arithmetic and back can make problem easier
  + To clear a sequence of 1's ending at digit j and at 1 at the beginning, simply $+ 2^j$, or +(1<<j)



### 5.5 Debugger

Explain what the following code does: ((n & (n-1)) == 0)

+ If A \& B == 0: then A and B have no 1 bit in common
+ If n & n-1 have no bit in common, then add 1 to n-1 flips all 1 bits to 0. Then n-1 looks like 0001111, and n looks like 0010000. Then n is a power of 2 or n =0



### 5.6 Conversion

Write a function to determine the number of bits you would need to flip to convert
integer A to integer B

+ Draw Example

  + 29(11101), 15(01111): 2

+ Brute Force

+ Optimize + Conceptual algorithm walk through

  + Find which bits are different (XOR)
  + Improve: c & (c-1) clears the least significan bit (right most 1) in c

+ Implement + Test

  ```java
  int bitsDifferent(int a, int b){
      int count = 0;
      int c = a^b;
      for(int c; c!=0; c>>=1){
          count += c % 2;
      }
      return count;
  }
  
  int bitsDifferent(int a, int b){
      int count = 0;
      for(int c= a^b; c!=0; c = c & (c-1)){
          count ++;
      }
      return count;
  }
  ```

  

+ Tips

  + c & (c-1) clears the least significan bit (right most 1) in c



### 5.7 Pairwise Swap

Write a program to swap odd and even bits in an integer with as few instructions as
possible (e.g., bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, and so on).

+ Optimize + Conceptual algorithm walk through

  + Move odd bit right by 1 (10101010 mask)
    + A = 1010, AAAAAAAA = 1010* 8 times (32 bit)
  + Move even bit left by 1 (01010101 mask)
    + 5 = 0101

+ Implement + Test

  + ```
    return (((x & 0xaaaa) >>> 1) | ((x & 0x5555) <<< 1))
    ```

+ Tips

  | Hex  | Binary   |
  | ---- | -------- |
  | 0    | 0000     |
  | 1    | 0001     |
  | 2    | 0010     |
  | 3    | 0011     |
  | 4    | 0100     |
  | 5    | 0101     |
  | 6    | 0110     |
  | 7    | 0111     |
  | 8    | 1000     |
  | 9    | 1001     |
  | A    | 1010     |
  | B    | 1011     |
  | C    | 1100     |
  | D    | 1101     |
  | E    | 1110     |
  | F    | 1111     |
  | 10   | 00010000 |

  

### 5.8 Draw Line

P299

A monochrome screen is stored as a single array of bytes, allowing eight consecutive pixels to be stored in one byte.The screen has width w, where w is divisible by 8 (that is, no byte will be split across rows).The height of the screen, of course, can be derived from the length of the array and the width. Implement a function that draws a horizontal line from (x1, y) to (x2, y).

+ Draw Example

  + (1,2) to (3,2):
    $$
    ...\\
    00000000 \quad 00000000\\
    01110000 \quad 00000000 \\
    00000000 \quad 00000000\\
    00000000 \quad 00000000 \\
    $$
    

+ Brute Force

  + Iterate over x1 to x2 and set each pixel

+ Optimize + Conceptual algorithm walk through

  + Span of 1 bits: 
    + start = x1/8 + x1%8
      + full bytes start at x1/8, +1 if x1%8 !=0
    + end = x2/8+x2%8
      + full bytes end at x2/8, -1 if x2%8 !=7
  + Set full bytes
  + Set the remaining bits

+ Implement + Test

  + ```java
    void drawLine(byte[] screen, int width, int x1, int x2, int y){
        int start_offset = x1%8;
        int full_byte_start = x1/8;
        if(start_offset!=0) full_byte_start++;
        int end_offset = x2%8;
        int full_byte_end = x2/8;
        if(end_offset!=7) full_byte_end--;
        
        for(int b = full_byte_start; b<= full_byte_end; b++){
            screen[(width/8)*y+b] = (byte) 0xFF;
        }
        
        byte start_mask = (byte) (0xFF>>> start_offset);
        byte end_mask = (byte) ~(0xFF >>> (end_offset+1));
        
        if((x1/8)==(x2/8)){//same byte
            byte mask = (byte) (start_mask & end_mask);
            screen[(width/8)*y+(x1/8)] |= mask;
        }else{
            if(start_offset!=0){
                int byte_number = (width/8)*y + full_byte_start-1;
                screen[byte_number] = start_mask;
            }
            if(end_offset!=7){
                int byte_number = (width/8)*y+full_byte_end+1;
                screen[byte_number] = end_mask;
            }
        }
    }
    ```

  + 

+ Tips

  + can use & to overlay masks

  + "|=" is bitwise OR + assignment, is the same as "+="

  + populate a byte: overlay following masks:

    ```java
        byte start_mask = (byte) (0xFF>>> start_offset);
        byte end_mask = (byte) ~(0xFF >>> (end_offset+1));
    ```

    