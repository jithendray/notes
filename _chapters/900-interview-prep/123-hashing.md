---
title: Hashing
---

### 1. Two Sum - Array is Not Sorted
[Link to problem](https://leetcode.com/problems/two-sum/)

- create a hashmap { }
- enumertae through the array
	- if target-num in hashMap - return indices
	- else add to the hashMap
- return empty array if no pair is found

```python
def twoSum(nums):
	hashMap = {}
	for i, num in enumerate(nums):
		if (complement := target-num) in hashMap:
			return [i, hashMap[num]]
		hashMap[num] = i
	return []
```

---
### 387. First Unique Character in a String
[Link to problem](https://leetcode.com/problems/first-unique-character-in-a-string/)


Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.

- create a counter of string
- enumerate through string - to preserve order - check count - return index

```python
def firstUniqChar(s):
	seen = {}

	for i,char in enumerate(s):
		if char in seen:
			seen[char] += 1
		else:
			seen[char] = 1

	for i, char in enumerate(s):
		if seen[char] == 1:
			return i 

	return -1
```

---

### 2351. First Letter to Appear Twice
[Link to problem](https://leetcode.com/problems/first-letter-to-appear-twice/)


```python
def repeatedChar(s):
	seen = set()

	for char in s:
		if char in seen:
			rerturn char
		seen.add(char)
```

---
### 242. Valid Anagram
[Link to problem](https://leetcode.com/problems/valid-anagram/)


- TC: O(n)
- SC: O(n)
```python
def validAnagram(s,t):
	return Counter(s) == Counter(t)
```

---
### 169. Majority Element
[Link to problem](https://leetcode.com/problems/majority-element/)

find the element that appears more than n / 2 times.

- hashMap solution
- TC: O(n)
- SC: O(n)

```python
def majorityElement(nums):
	counts = Counter(nums)

	threshold = len(nums)//2

	for num, count in counts.items():
		if count > thereshold:
			return num
```

- BoyerMoore Voting algorithm
- initialize candidate and count = 0
- for each element
	- if it matches candidate - increment count
	- else if - count = 0 then set candidate to current element and count to 1
	- else - decrement count
- return the majority
- TC: O(n)
- SC: O(1)

```python
def majorityElement(nums):
	majority = None
	count = 0

	for num in nums:
		if count == 0:
			majority = num
			count = 1
		elif majority == num:
			count += 1
		else:
			count -= 1

	return majority
```

---
### 229. Majority Element II
[Link to problem](https://leetcode.com/problems/majority-element-ii/)

find the element that appears more than n / 2 times.

- hashmap solution
- TC: O(n)
- SC: O(n)

```python
def majorityElement2(nums):
	counts = Counter(nums)
	threshold = len(nums)//3
	res = []

	for num, count in counts.items():
		if count > threshold:
			res.add(num)

	return res
```
	
- Modified BoyerMoore Voting algorithm
- initialize two candidates and their counts
- for each element
	- if it matches candidate1 - increment count1
	- if it matches candidate2 - increment count2
	- if count1 = 0 then set candidate1 to current element and count1 to 1
	- if count2 = 0 then set candidate2 to current element and count2 to 1
	- else decrement both count1 and count2
- verify both candidates appear more than n/3 times - because BMV gives only potential candidates - so its good to verify before returning
- TC: O(n)
- SC: O(1)

```python
def majorityElement2(nums):
	candidate1, candidate2 = None, None
	count1, count2 = 0, 0

	for num in nums:
		if num == candidate1:
			count1 += 1
		elif num == candidate2:
			count2 += 1
		elif count1 == 0:
			candidate1 = num
			count1 = 1
		elif count2 == 0:
			candidate2 = num
			count2 = 1
		else:
			count1 -= 1
			count2 -= 1

	result = []
	if nums.count(candidate1) > len(nums)//3:
		result.append(candidate1)
	if candidate2 != candidate1 and nums.count(candidate2) > len(nums)//3:
		result.append(candidate2)

	return result
```

---
### 819. Most Common Word
[Link to problem](https://leetcode.com/problems/most-common-word/)

- use regex
- convert banned list into set - for better searching
	- average complexity for searching in set is O(1)
- convert the paragraph into words list using regex
- `re.findall(r'\b\w+\b', paragraph.lower())`
	- paragraph.lower() - converts to lowercase - as the problem states case insensitive
	- re.findall() - searches entire text for all occurances that match given regular expression and returns them as a list
	- r'\b\w+\b'
		- r before string makes it a raw string - tells to consider \ literally without considering them as a escape characters
		- \b - word boundary- which makes position between word character and non-word character
		- \b at start and end - restricts the match to whole words
		- \w matches any word character (letters, digits and underscores)
		- + qualifier that means one or more of the preceeding element
		- \b\w+\b together matches any sequence of one or more word characters until a nonword character appears
- apply counter excluding words in banned_set
- `counter.most_common(1) [0]  [0]`
	- counter.most_common(n) returns n most common elements in counter along with their counts in list of tuples
	- counter.most_common(1) [0]  [0] returns single most common element , then first list element, first tuple element
	- example: Counter({'apple': 3, 'banana': 2, 'orange': 1})
		- counter.most_common(1) = [ ( 'apple' ,  3 ) ]

```python
def mostCommonWord(paragraph, banned):
	banned_set = set(banned)
	words = re.findall(r'\b\w+\b', paragraph.lower())
	c = Counter(word for word in words if word not in banned_set)
	return c.most_common(1)[0][0]
```

---
### 49. Group Anagrams
[Link to problem](https://leetcode.com/problems/group-anagrams/)

---

### 41. First Missing Positive
[Link to problem](https://leetcode.com/problems/first-missing-positive/)
