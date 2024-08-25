# Print Anagrams Together

## Problem statement
Given an array of strings, return all groups of strings that are anagrams. The groups must be created in order of their appearance in the original array.

> The final output will be in lexicographic order.

### Examples
| Input | Output | Explaination |
| ----- | ------ | ------------ |
| {act, god, cat, dog, tac} | `{act cat tac}, {god dog}` | There are 2 groups of anagrams. {"god", "dog"} make group 1, and "act", "cat", "tac" make group 2 |
| {no, on, is} | `{is}, {no on}` | There are 2 groups of anagrams. "god", "dog" make group 1 and "act", "cat", "tac" make group 2 |

### Expectations
> 1 < = N < = 100
> 
> 1 < = |S| < = 10

#### Time Complexity
`O(N * |S| * log|S|)` , where |S| is the length of the strings
#### Auxiliary Space
`O( N * |S| )`, where |S| is the length of the strings


## Solution
### Observations
Two anagrams are words which are build with same array of characters, but in different orders. With this definition in mind, we can see that outputs after sorting characters of each anagram word will be same.

We can use that to find anagrams.

### Whiteboard solution
Given an array of words of size n and `arr` containing n words, do following:

1. Initialize an empty map. Name it `groups_map` `( O(1) )`
2. Iterate over element of `arr` `( O (n)  )`
    - For each element, sort its characters using Merge sort algorithm. Name it `sorted_word` ( `O( s * log(s))` where `s` is the size of the word )
    - If `sorted_word` is not present in `groups_map` key set ( `O(1)` of HashMap key lookup)
        - initialize `groups_map[sorted_word]` with empty array `( O(1) )`
    - Append the element into `groups_map[sorted_word]` `( O(1) )`
3. After exiting return values of the `groups_map` `( O(1) )`

### Code
``` python
def merge(word: str, s: int, m: int, e: int):
    l = list(word)
    i = s
    j = m + 1
    st = []
    while i <= m and j <= e:
        if l[i] < l[j]:
            st.append(l[i])
            i += 1
        else:
            st.append(l[j])
            j += 1
    while i <= m:
        st.append(l[i])
        i += 1
    while j <= e:
        st.append(l[j])
        j += 1
    l[s:e + 1] = st
    return "".join(l)


def sortString(word: str, s: int, e: int):
    if s >= e:
        return word
    m = int((s + e) / 2)
    word = sortString(word, s, m)
    word = sortString(word, m + 1, e)
    return merge(word, s, m, e)


def Anagrams(words: list[str], n: int):
    groups_map = {}
    for i in range(n):
        sorted_word = sortString(words[i], 0, len(words[i]) - 1)
        if not groups_map.__contains__(sorted_word):
            groups_map[sorted_word] = []
        groups_map.get(sorted_word).append(words[i])

    return groups_map.values()
```

With an input
```
6
act god cat dog tac it
```

The output will be
```
act cat tac 
god dog
```

### Time Complexity analysis
From Whiteboard solution we can calculate the time complex to be `O(N * |S| * log|S|)`

### Auxiliary Space analysis
The only extra space we are using is in forgroups_map  which will take `O( N * |S| )`

## Reference
[Print Anagrams Together](https://www.geeksforgeeks.org/problems/print-anagrams-together/1)