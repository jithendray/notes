---
title: Array manipulation
---
### Leaders in Array
[Link to problem](https://www.geeksforgeeks.org/problems/leaders-in-an-array-1587115620/1)

leader = an element that is greater than all elements to its right
- `traverse the array from right to left, keep track of max found so far, any element >= this max is a leader!`
	- start from rightmost element (maxRight), add it to leaders (last element is always a leader)
	- traverse in reverse - check if it is >= maxRight - update it and add to leaders
	- to get leaders in correct order - reverse it
- TC: O(n)
- SC: O(1)
```python
def get_leaders(arr):
	n = len(arr)
	leaders = []

	maxRight = arr[-1]
	leaders.append(maxRight)

	for i in range(n-2, -1, -1):
		if arr[i] >= maxRight:
			maxRight = arr[i]
			leaders.append(maxRight)
	
	return leaders[::-1]
```

---
### 189. Rotate Array
[Link to problem](https://leetcode.com/problems/rotate-array/)
#### Right Rotate by k
- k = k%len(arr) because if k > len(arr) - the rotations just repeat
- rotate the array
- rotate first k elements
- rotate remainig elements
#### Left Rotate by k
- k = k%len(arr) because if k>len(arr) - the rotations just repeat
- rotate first k elements
- rotate remaining elements
- rotate the array

- TC: O(n)
- SC: O(1)

```python
def rightRotate(nums,k):
    n = len(nums)
    k = k%n
    
    l,r = 0, n-1
    while l<r:
        nums[l], nums[r] = nums[r], nums[l]
        l,r = l+1, r-1
        
    l,r = 0, k-1
    while l<r:
        nums[l], nums[r] = nums[r], nums[l]
        l,r = l+1, r-1
        
    l,r = k, n-1
    while l<r:
        nums[l], nums[r] = nums[r], nums[l]
        l,r = l+1, r-1
    
    return nums

```

---
### 485. Max Consecutive Ones
[Link to problem](https://leetcode.com/problems/max-consecutive-ones/)

- initiate currCount = 0 and maxCount = 0
- iterate through array
- everytime 1 comes - increase currCount
- if 0 comes -> take max of currCount and maxCount and make count = 0
- finallly return max of maxCount and currCount to get the maximum consecutive ones (incase maxOnes are at the end of the array)
- TC: O(n)
- SC: O(1)

```python
def findMaxConsecutiveOnes(nums):
	n = len(nums)
	currCount, maxCount = 0,0

	for i in range(n):
		if nums[i] == 1:
			currCount += 1
		else:
			maxCount = max(maxCount, currCount)
			currCount = 0

	return max(maxCount, currCount)
```

```python
def findMaxConsecutiveOnes(nums):
	n = len(nums)
	currCount, maxCount = 0,0

	for i in range(n):
		if nums[i] == 1:
			currCount += 1
		else:
			currCount = 0
		
		if currCount > maxCount:
			maxCount = currCount

	return maxCount
```

---
### 1299. Replace Elements with Greatest Element on Right Side
[Link to problem](https://leetcode.com/problems/replace-elements-with-greatest-element-on-right-side/)

replace every element with greatest element to its right, and last element with -1

- `traverse the array from right to left, initiate maxRight with -1, swap with current element if its greater else just give existing maxRight`
	- maxRight = -1
	- traverse from right to left
	- if num > maxRight -> swap(num, maxRight)
	- else: num = maxRight
- TC: O(n)
- SC: O(1)

```python
def replaceMaxRight(arr):
	n = len(arr)
	maxRight = -1

	for i in range(n-1,-1,-1):
		if arr[i] > maxRight:
			arr[i], maxRight = maxRight, arr[i]
		else:
			arr[i] = maxRight

	return arr
```

