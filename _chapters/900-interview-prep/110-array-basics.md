---
title: Array Basics
---


1. **Print each element and its index in an array.**
```python
for index, value in enumerate(array):
	print([value, index])
```

2. **Print elements of an array in reverse order.**
```python
print(array[::-1])
print(reversed(array))
```

3. **Print alternate elements of an array.**
```python
print(array[::2])
```

4. **Create a duplicate of an array.**
```python
duplicate_array = array[:]
```

5. **Create two arrays: one for odd elements and one for even elements.**
```python
odd = [num for num in array if num % 2 != 0]
even = [num for num in array if num % 2 == 0]
```

6. **Calculate sum and product of array elements.**
```python
array_sum = sum(array)

from functools import reduce
array_product = reduce(x, y: x*y, array)
```
   
7. **Count occurrences of a target number in an array.**
```python
array.count(target)
```
   
8. **Check if an array is sorted forward, backward, or not at all.**
```python
if array == sorted(array):
	print("Forward Sorted")
elif array == sorted(array, reverse=True):
	print("Backward Sorted")
else:
	print("Not Sorted")
```
   
9. **Count unique and duplicate elements in an array (1-100).**
```python
unique = len(set(array))
duplicates = len(array) - unique



from collections import Counter
counter = Counter(array)
unique = sum(1 for count in counter.values() if count == 1)
duplicate = sum(1 for count in counter.values() if count > 1)
```

10. **Check if two elements exist with a sum equal to a target value.**
```python
array = [1,2,3,4,5]
target = 7
found = False
seen = set()
for num in array:
	if target-num in seen:
		found = True
		break
	seen.add(num)
```
    
11. **Check if three elements exist with a sum equal to a target value.**
```python
array = [1,2,3,4,5]
target = 10
found = False
for i in range(len(array)):
	seen = set()
	curr_sum = target - array[i]
	for j in range(i+1, len(array)):
		if curr_sum - arr[j] in seen:
			found = True
			break
		seen.add(arr[j])
	if found:
		break
```

12. **Find the maximum element in an array.**
```python
max(array)
```

13. **Find the minimum element in an array.**
```python
min(array)
```

14. **Find the 2nd maximum element; if none, print -1.**
```python
unique_array = list(set(array))
unique_array.sort()
print(unique_array[-2])



largest = float('-inf')
secondLargest = float('-inf')
for i in range(len(array)):
	if array[i] > largest:
		largest = array[i]

for i in range(len(array)):
	if array[i] > secondLargest and array[i] != largest:
		secondLargest = array[i]



largest = float('-inf')
secondLarget = float('-inf')
for i in range(len(array)):
	if array[i] > largest:
		secondLargest = largest
		largest = array[i]
	elif array[i] < largest and array[i] > secondLargest:
		secondLargest = array[i]
```

15. **Find the 2nd minimum element; if none, print -1.**
```python
unique_array = list(set(array))
unique_array.sort()
print(unique_array[2])


smallest = float('inf')
secondSmallest = float('inf')
for i in range(len(array)):
	if array[i] < smallest:
		smallest = array[i]
for i in range(len(array)):
	if array[i] < secondSmallest and array[i] != smallest:
		secondSmallest = array[i]


smallest = float('inf')
secondSmallest = float('inf')
for i in range(len(array)):
	if array[i] < smallest:
		secondSmallest = smallest
		smallest = array[i]
	elif array[i] > smallest and array[i] < secondSmallest:
		secondSmallest = array[i]
```

16. **Insert an element at the Xth position, shifting right.**
```python
array.insert(X, element)
```

17. **Delete an element at the Xth position, shifting left.**
```python
del array[x]
```