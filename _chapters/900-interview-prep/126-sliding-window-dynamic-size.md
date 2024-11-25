---
title: Sliding Window - Dynamic Size
---
### Notes
- Use sliding window when searching for a continuous subsequence in an array or string that satisfies a certain constraint.
- If you know the length of the subsequence you are looking for, use a fixed-length sliding window. Otherwise, use a variable-length sliding window.
- It is important to think about the appropriate data structure to store the contents of the current window. 
- Make sure it supports both:
    - Adding and removing elements from the window in O(1) time.
    - Checking if the window is valid in O(1) time
- Dictionaries and sets are often the best choices.
- Use SET if 
	- only care about distinct elements in the window
	- do not need count of elements - just presence or absence
	- problem involves
		- checking for duplicates or ensuring uniqueness in the window
		- finding largest/smallest window with a certain property related to distinct elements
	- Longest substring without repeating characters
		- track only the characters currently in the window
		- use set to quickly check if character is already in the window
- Use DICT if
	- need to track count of elements in the window
	- problem involves
		- removing elements when their count reaches 0
		- handling frequency-based constraints
	- Fruit into Baskets (or Longest Subarray with at most two distinct characters)
		- track count of fruits to ensure u have at most 2 distinct types in the window
	- Anagrams
		- u need to match frequencies (finding all anagrams of a string in another string)
	- Character Replacement
		- problems where the frequency of characters determines whether the current window is valid

### Template for variable size - sliding window

```python
def variable_sliding_window(nums):
	window = # choose appropriate data structure - usually set or dict
	start = 0
	max_ = 0

	for end in range(len(nums)):
		# extend window
		# add nums[end] to window in O(1) time

		while state is not valid:
			# repeatedly contract window until it is valid again
			# remove nums[start] from window in O(1) time
			start = start + 1

		max_ = max(max_, end-start+1)

	return max_
```

---
### Add first or Validate first?
- In **variable sliding window problems**, whether you first add `nums[end]` to the window and then check the validity with `while` (or vice versa) depends on the **specific problem** and how you define the window's validity.
- Add `nums[end]` first if:
	- adding the current element to the window is always valid (initially), and u only need to shrink the window later if it violates the constraint
	- common in problems like longest subarray/substring or maintaining a certain property of window (at most k distinct elements)
	- Problems like "Longest Subarray with at Most K Distinct Characters" - add `nums[end]` first because adding a new character doesn't immediately make the window invalid (it only becomes invalid when the count of distinct characters exceeds k)
- Check `while` first if:
	- u want to ensure window is valid before adding current `nums[end]`
	- "Longest Substring Without Repeating Characters" - adding a duplicate character immediately violates the condition, so you must validate before processing further.
- Use `while` first when adding an element can immediately make the window invalid
- Add first when the condition remains valid upon addition, and only need to shrink later if a threshold (like count) is exceeded

---

### 3. Longest Substring Without Repeating Characters
[Link to problem](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```
Given a string s, find the length of the longest substring without repeating characters.
```

- initiate currWidth = 0 and left = 0 and longest = 0
- fix the left pointer and move the right pointer thorugh the string
- add the element at right pointer to the set, compare current width and longest
- while the element is in set
	- remove the element at left from the set
	- increment left
- return longest

```python
def lengthOfLongestSubstring(s):
	start = 0
	seen = {}
	max_length = 0

	for end in range(len(s)):
		while s[end] in seen:
			seen.remove(s[start])
			start += 1
			
		seen.add(s[end])
		
		max_length = max(max_length, end-start+1)
		
	return max_length
```

---

### Subarray with Given Sum
[Link to problem](https://www.geeksforgeeks.org/find-subarray-with-given-sum/#expected-approach-using-sliding-window-ontime-and-o1-auxiliary-space)

```python
def subarray_given_sum(nums):
	start = 0
	currSum = 0

	for end in range(len(nums)):
		currSum += nums[end]

		while currSum > target:
			currSum -= nums[start]
			start += 1

		if currSum == target:
			return [start, end]

	return -1
```

---
### 209. Minimum Size Subarray Sum
[Link to problem](https://leetcode.com/problems/minimum-size-subarray-sum/)

Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

- initiate start = 0, currSum = 0, min_length = inf
- iterate end through the array:
	- keep adding `nums[end]` to the currSum
	- while currSum >= target:
		- compare the existing min_length with current window size - and store the minimum
		- contract the window - decrease currSum by subtracting `nums[start]`
		- increment start by 1

```python
def min_size_subarray_sum(nums, target):
	start = 0
	currSum = 0
	min_length = float('inf')

	for end in range(len(nums)):
		currSum += nums[end]

		while currSum >= target:
			min_length = min(min_length, end-start+1)
			currSum -= nums[start]
			start += 1

	return min_length if min_length != float('inf') else 0
```

---
### 1004. Max Consecutive Ones III
[Link to problem](https://leetcode.com/problems/max-consecutive-ones-iii/)

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.


- initiate start = 0
- max_ones = 0
- also here the window will be num_zeroes - as thevalid condition will be num_zeroes <= k
- num_zeroes = 0
- iterate end through the array
	- if `nums[end]` == 0 - increment num_zeroes
	- check validity - while num_zeroes > k:
		- check if `nums[start]` == 0 and reduce 1 from num_zeroes
		- increment start
	- compare max_ones
- return max_ones

```python
def max_consecutive_ones_iii(nums, k):
	start = 0
	maxOnes = 0
	numZeroes = 0

	for end in range(len(nums)):
		if nums[end] == 0:
			numZeroes += 1

		while numZeroes > k:
			if nums[start] == 0:
				numZeroes -= 1
			start += 1
		
		maxOnes = max(maxOnes, end-start+1)

	return maxOnes
```


---

### 904. Fruit Into Baskets
[Link](https://leetcode.com/problems/fruit-into-baskets/)

Similar to: Longest Substring with at most two distinct characters

- initiate start = 0, max_ = 0
- keep a window - as dict - as counts have to be calculated
- iterate end through the array
	- extend the window
	- validate the window: while len(window) > 2
		- contract the window - decrease count of start by 1 in window. if count == 0 then delete
	- compare max_fruits
- return

```python
def fruit_into_basket(fruits):
	start = 0
	max_fruits = 0
	window = {}

	for end in range(len(fruits)):
		window[fruits[end]] = window.get(fruits[end], 0) + 1

		while len(window) > 2:
			window[fruits[start]] -= 1
			if window[fruits[start]] == 0:
				del window[fruits[start]]
			start += 1

		max_fruits = max(max_fruits, end-start+1)

	return max_fruits
```

---

### 424. Longest Repeating Character Replacement
[Link to problem](https://leetcode.com/problems/longest-repeating-character-replacement/)

### 76. Minimum Window Substring
[Link to problem](https://leetcode.com/problems/minimum-window-substring/)

### 239. Sliding Window Maximum
[Link to problem](https://leetcode.com/problems/sliding-window-maximum/)

