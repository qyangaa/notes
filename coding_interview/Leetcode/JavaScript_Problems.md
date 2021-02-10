[reference](https://learn.codevolution.dev/courses/1222162/lectures/27380448)



## Sum of Numbers

You are given an array of two numbers [a,b]. Find the sum of those two numbers plus the sum of all the numbers between them.

```javascript
function sum(arr) {
    let res = 0
    if(arr[0]>arr[1]){
        [arr[0], arr[1]] =[arr[1], arr[0]]
    }
    for(let i=arr[0]; i<=arr[1]; i++){
        res += i;
    }
    return res;

}
console.log(sum([1, 4]) );//10
console.log(sum([4, 1]) );//10
```

## Factorial of a Number

```javascript
function factorial(num) {
    let res = 1
    for(let i=1; i<=num; i++){
        res *= i;
    }
    return res;

}
console.log(factorial(5) );//120
console.log(factorial(4) );//24
```



## Fibonacci Sequence

```javascript
function fibonacci(num) {
    if(num==1) return [0]
    let res = new Array(num).fill(0)
    res[0] = 0
    res[1] = 1
    for(let i=2; i<num; i++){
        res[i] = res[i-1]+ res[i-2]
    }
    return res;

}
console.log(fibonacci(2) );//[ 0, 1 ]
console.log(fibonacci(7) );//[0, 1, 1, 2,3, 5, 8]
```

##  

## Prime Numbers

```javascript
function findPrime(min, max) {
    let res = []
    if(min>=2) res.push(i);
    for(let i=Math.max(min,2); i<=max; i++){
        let isPrime = true;
        for(let j=2; j<= Math.floor(i/j); j++){
            if(i%j==0) isPrime = false;
        }
        if(isPrime) res.push(i);
    }
    return res;

}
console.log(findPrime(0, 20) );//[2 3 5 7 11 13 17 19]
```



## Palindrome

```javascript
function isPalindrome(str) {
    let l = 0;
    let r = str.length-1;
    while(l<r){
        if(str[l] !== str[r]) return false;
        l++;
        r--;
    }
    return true
}
console.log(isPalindrome('racecar') );//true
console.log(isPalindrome('race') );//false
```



## Anagram

```javascript
function isAnagram(str1, str2) {
    countMap = new Map()
    for(c of str1){
        if(!(c in countMap)){
            countMap[c] = 0;
        }
        countMap[c]+=1;
    }
    for(c of str2){
        if(!(c in countMap)) return false;
        if(countMap[c]<=0) return false;
        countMap[c] -=1;
    }
    for(v of countMap.values()){
        if(v>0) return false;
    }
    return true;
}
console.log(isAnagram('racecar', 'carrace') );//true
console.log(isAnagram('racecar', 'carracr') );//false
```





## Reverse Words

```javascript
function reverseWords(str) {
    const l = str.split(' ').reverse()
    return l.join(' ')
}
console.log(reverseWords('Hello World') );//'World Hello'
console.log(reverseWords('This is  a  test string ') );//Returns 'string test a is This'
```



## Remove Vowels from a String

```javascript
function removeVowels(str) {
    return str.replace(/[aeiou]/gi, '')
}
console.log(removeVowels('Hello World') );//Hll Wrld
console.log(removeVowels('This is  a  test string ') ); //Ths s    tst strng

```



## Palindromic Substrings

```javascript
function countPalindromicSubstrings(str) {
    const n = str.length;
    const dp = Array(n).fill().map(()=> Array(n).fill(0))
    let count = 0;
    for(let i=0; i<n; i++){
        let l=0;
        while(i-l>=0 && i+l<n){
            if(str[i-l]==str[i+l]){
                l ++; 
                count ++;
            }else{
                break;
            }
        }
        l=0;
        while(i-l>=0 && i+l+1<n){
            if(str[i-l]==str[i+l+1]){
                l ++; 
                count ++;
            }else{
                break;
            }
        }
    }
    return count
}{item}
console.log(countPalindromicSubstrings('abcd') );//4
console.log(countPalindromicSubstrings('aaa') ); //6
```

```javascript
function longestPalindrome(str) {
    const n = str.length;
    let res = '';
    for(let i=0; i<n; i++){
        let l=0;
        while(i-l>=0 && i+l<n){
            if(str[i-l]==str[i+l]){
                if((1+2*l)>res.length){
                    res = str.slice(i-l, i+l)
                }
                l ++; 
            }else{
                break;}
        }
        l=0;
        while(i-l>=0 && i+l+1<n){
            if(str[i-l]==str[i+l+1]){
                if((2*l+2)>res.length){
                    res = str.slice(i-l, i+l+2)
                }
                l ++; 
            }else{
                break;
            }
        }
    }
    return res
}
console.log(longestPalindrome('adccccdd') );//'dccaccd'
```



## Array of Fullnames

```javascript
function fullNames(arr) {
    let res = [];
    arr.map(({firstname, lastname})=>{
        res.push(`${firstname} ${lastname}`)
    })
    return res
}
console.log(fullNames( [{firstname: 'Bruce', lastname: 'Wayne'}, {firstname: 'Clark', lastname: 'Kent'}]) );//['Bruce Wayne', 'Clark Kent']
```



## Longest Word in a String

```javascript
function longestWord(str) {{item}
    let len = 0;
    let res = '';
    for(word of str.split(' ')){
        if((word.length)>len){
            len = word.length;
            res = word;
        }
    }
    return res;
}
console.log(longestWord('My name is Vishwas')  );//'Vishwas'
console.log(longestWord('Hello world')  );//'Hello'
```



## Array and Index

Given two arrays of integers (nums) and (index), create and return a new array (arr) which satisfies the following rules:

Initially array (arr) is empty.

From left to right read nums[i] and index[i] and insert at index index[i] the value nums[i] in array (arr) .

Repeat the previous step until there are no elements to read in nums and index.

```javascript
function createArray(nums, index) {
    const res = [];
    for(let i=0;i<nums.length; i++){
        res.splice(index[i],0,nums[i])
    }
    return res;
}
console.log(createArray( [0,1,2,3,4], [0,1,2,2,1])  );// [0,4,1,3,2]
```



## Union Intersection Difference

Given two arrays (arr1) and (arr2) return the union, intersection, difference and symmetric difference of the two arrays.

```javascript
// Array
[...arr1, ...arr2]; //union
arr1.filter(item=> arr2.includes(item)) // intersection
arr1.filter(item=> !arr2.includes(item))//difference
arr1.filter(item=> !arr2.includes(item)).concat(arr2.filter(item=> !arr1.includes(item))) //symmetric difference
//Set
let union = new Set([...a, ...b]);
let intersection = new Set([...a].filter(x => b.has(x)));
let difference = new Set([...a].filter(x => !b.has(x)));
```



## Flatten Nested Array

```javascript
function flattenArray(arr) {
    return arr.reduce((acc,curr)=>{
        if(Array.isArray(curr)){
            acc = acc.concat(flattenArray(curr))
        }else{
            acc.push(curr)
        }
        return acc
    }, [])
}


let input = [1, [2], [3, [[4]]]]
console.log(flattenArray(input));// [1, 2, 3, 4]

```

## Duplicate Elements

```javascript
function flattenArray(arr) {
    const set = new Set();
    for(let item of arr){
        if(!set.has(item)){
            set.add(item);
        }else{
            return item;
        }
    }
}
```



## Non Repeating Words

Given two strings (str1) and (str2), return a list of all non-repeating words.

```javascript
function noneRepeatingWords(str1, str2) {
    const set1 = new Set(str1.split(' '));
    const set2 = new Set(str2.split(' '));
    return Array.from([...set1].filter(c=> !(set2.has(c))).concat([...set2].filter(c=> !(set1.has(c)))))
}


let str1 = 'Hello world';
let str2 = 'Hello Vishwas'
console.log(noneRepeatingWords(str1, str2));// [ 'world', 'Vishwas' ]

```



## Longest Palindrome From Given Characters

Given a string (str) which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

```javascript
function longestPalindrome(str) {
    const map = new Map();
    for(let c of str){
        if(!(map.has(c))){
            map.set(c,0);
        }
        map.set(c, map.get(c)+1);
    }
    let res = 1;
    for(let count of map.values()){
        res += Math.floor(count/2)*2;
    }
    return res
}
console.log(longestPalindrome('abccccdd') );//7
```



## Longest Substring Without Repeating Characters

```javascript
function longestSubstring(str) {
    const set = new Set();
    let l = 0;
    let r = 0;
    let length = 0;
    while(r<str.length){
        if(!set.has(str[r])){
            set.add(str[r++])
            length = Math.max(length, set.size)
        }else{
            set.delete(str[l++])
        }
    }
    return length
}

console.log(longestSubstring('abcabcbd') );//3
console.log(longestSubstring('aaaa') );//1, 'a'
console.log(longestSubstring('abbcdb') );//3 'bcd'
```



## Group Anagrams

```javascript
function sortString(str){
    return str.split('').sort().join('')
}

function groupAnagrams(arr) {
    const map = new Map();
    for(let str of arr){
        let key = sortString(str)
        if(!(map.has(key))){
            map.set(key, [])
        }
        map.get(key).push(str)
    }
    return Array.from(map.values());
}
console.log(groupAnagrams(["eat", "tea", "tan", "ate", "nat", "bat"]) );//[["ate","eat","tea"], ["nat","tan"], ["bat"]]
```

