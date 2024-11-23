---
title: String Manipulation
---

#### [Remove characters from the first string which are present in the second string](https://www.geeksforgeeks.org/remove-characters-from-the-first-string-which-are-present-in-the-second-string/)

- TC: O(m * n)
- SC: O(1)
```python
def removeElements(s1, s2):
	for i in s2:
		s1 = s1.replace(i,'')
	return s1
```

- convert s2 to set - to allow O(1) average TC checks for each character
- filter chars that are not in set(s2)
- `''.join(....)` - combine filtered characters into final result
- TC: O(m+n)
- SC: O(m+n)
```python
def removeElements(s1,s2):
	s2_set = set(s2)
	result = ''.join(char for char in s1 if char not in s2_set)
	return result
```

---
#### [1910. Remove All Occurrences of a Substring](https://leetcode.com/problems/remove-all-occurrences-of-a-substring/)

Find the **leftmost** occurrence of the substring `part` and **remove** it from `s`.

- replace the first occurrence of part in s each time replace is called within the while loop
- TC: O(m * n)
- SC: O(n)
```python
def removeOccurences(s, part):
	while part in s:
		s = s.replace(part, "", 1)
	return s
```

- s.index(part) - gives index of first occurence
- TC: O(m * n)
- SC: O(n)
```python
def removeOccurences(s, part):
	while part in s:
		index = s.index(part)
		s = s[:index]+s[index+len(part):]
	return s
```

---
#### [58. Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

- iterate from right to left
- initiate a pointer at end and lenght = 0
- until it sees a non-empty string -> decrease r
- until it sees a empty string
	- increase length
	- decrease r
- TC: O(n)
- SC: O(1)

```python
def LengthofLastWord(s):
	r = len(s) - 1
	length = 0

	while s[r] == " ":
		r = r - 1

	while s[r] != " " and r >= 0:
		length = length + 1
		r = r - 1

	return length
```

---
#### [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

```python
def strstr(haysyack, needle):
	for i in range(len(haystack)):
		if haystack[i : i+len(needle)] == needle:
			return i

	return -1
```

---
#### [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

- assume first string is prefix
- compare with other strings in array
- keep trimming the prefix from end until it matches the start of each string
- If the prefix becomes empty - return ""
- TC: O(m * n) - where m is length of prefix and n is the number of strings
- SC: O(1)

```python
def longestCommonPrefix(strs: List[str]) -> str:
	prefix = strs[0]

	for s in strs[1:]:
		while s[:len(prefix)] != prefix:
			prefix = prefix[:-1]

	return "" if not prefix else preifx
```

---
#### [171. Excel Sheet Column Number](https://leetcode.com/problems/excel-sheet-column-number/)

```python
def titleToNum(columnTitle):
	number = 0

	for char in columnTitle:
		number = number * 26 + (ord(char) - ord('A') + 1)

	return number
```

---
#### [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

- initiate num with last character
- iterate through every character from left to right until len(s)-1
- if it is smaller than next char - then subtract from num
- else - add to num

```python
def romanToInt(s):
	ref = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}

	num = ref[s[-1]]

	for i in range(len(s)-1):
		if ref[s[i]] < ref[s[i+1]]:
			num = num - ref[s[i]]
		else:
			num = num + ref[s[i]]

	return num
```

---
####  [168. Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/)

---
#### [12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)


---









