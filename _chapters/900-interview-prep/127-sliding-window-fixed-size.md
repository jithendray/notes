---
title: Sliding Window - Fixed Size
---
### 643. Maximum Average Subarray I
[Link to problem](https://leetcode.com/problems/maximum-average-subarray-i/)

Find a contiguous subarray whose length is equal to k that has the maximum average value and return this value. 

- calculate currSum - until `nums[:k]` - this is maxSum initially
- iterate from k to len(nums):
	- add `nums[i]` to currSum and subtract `nums[i-k]` from currSum
	- compare currSum to maxSum and update maxSum
- return maxSum / k

```python
def max_avg_subarray(nums,k):
	maxSum = currSum = sum(nums[:k])

	for i in range(k, len(nums)):
		currSum += nums[i]
		currSum -= nums[i-k]
		maxSum = max(maxSum, currSum)

	return maxSum/k
```

---

### 1876. Substrings of Size Three with Distinct Characters
[Link to problem](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters/)

Given a string s​​​​​, return the number of good substrings of length three in s​​​​​​. A string is good if there are no repeated characters.
Note that if there are multiple occurrences of the same substring, every occurrence should be counted.

- instead of worrying about storing window - just check and move forward
- initiate start and count to 0
- iterate end through the array
	- if window size > 3: increment start
	- if window size == 3: check if there are 3 distinct chars in the window
	- if yes - increment count
- return count

```python
def good_strings(s,k):
	k = 3 # given in question
	start = 0
	count = 0

	for end in range(len(s)):
		if end+1-start > k:
			start += 1

		if end-1+start == k and len(set(s[start:end+1])) == k:
			count += 1

	return count
```

```python
def good_strings(s,k):
	k = 3
	count = 0

	for i in range(len(s)-k+1):
		if len(set(s[i:i+k])) == k:
			count += 1

	return count
```

---
### 1456. Maximum Number of Vowels in a Substring of Given Length
[Link to problem](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

Given a string s and an integer k, return the maximum number of vowel letters in any substring of s with length k.

- calculate currCount of vowels in first k window. maxCount will also be same initially
- iterate from k to len(s)
	- increment currCount if `s[i]` is a vowel
	- decrement currCount if `s[i-k]` is a vowel
	- compare maxCount and currCount - update maxCount
- return maxCount

```python
def max_vowels(s,k):
	vowels = {'a','e','i','o','u'}
	currCount = 0

	for i in range(k):
		if s[i] in vowels:
			currCount += 1

	maxCount = currCount

	for i in range(k, len(s)):
		if s[i] in vowels:
			currCount += 1
		if s[i-k] in vowels:
			currCount -= 1
		maxCount = max(currCount, maxCount)

	return maxCount
```

---
### 438. Find All Anagrams in a String
[Link to problem](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
 