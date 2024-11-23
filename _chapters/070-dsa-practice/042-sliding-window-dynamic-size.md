---
title: Sliding Window - Dynamic Size
---

### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

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
	n = len(s)
	l, longest = 0
	seen = set()

	for r in range(n):
		while s[r] in seen:
			seen.remove(s[l])
			l = l+1
		currWidth = r-l+1
		longest = max(longest, currWidth)
		seen.add(s[r])
	return longest
```

---
### [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)


### [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

### [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

### [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

### [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
### [Subarray with Given Sum](https://www.geeksforgeeks.org/find-subarray-with-given-sum/#expected-approach-using-sliding-window-ontime-and-o1-auxiliary-space)

