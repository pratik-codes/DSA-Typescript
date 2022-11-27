Good references - [CS50-Array](https://www.youtube.com/watch?v=tI_tIZFyKBw&ab_channel=CS50)

# Array

In case you want to store a bunch of values which are very similar to each others for example scores

```
const score1 = 1;
const score2 = 2;
const score3 = 3;
const score4 = 4;

// you can instead store these values in a array like this

const array = new Array(1,2,3,4)
```

These array are nothing but chunk of memory back to back which is used to store values provided by us. If not its just `0`

so when we traverse a array its a O(N) time complexity where N being the number of elements in the array.

Insertion and deletion(pop) is just o(1) because we are not traversing anything we are just replacing a value on a particular memory with the input value or removing and setting it back to 0 in case of a pop.

Usually in modern programming language we use `ArrayList` which is nothing but a Array underneath with some logic and rules. As you can see we don't have to specify the limit of this array because what are ArrayList does it grows the array by copying the elements to another array with higher size as the size of the arrayList increases.

ArrayList are not bounded by memory but Arrays are not like that Array has fixed memory but in most of the Programming language the array we used has a dynamic length because it uses ArrayList underneath.

How do you identify if a array is a ArrayList ?

Look at the time complexity on different operations.

Pushing = o(1) \
Popping = o(1) \
shifting = o(N) because it shifts all the element next to it

# Search

## Linear Search

Linear search is nothing but looping through the array to find the element you are looking for so the time complexity of the search will be O(N).

```
const linearSearch = (haystack: Array<number>, needle: number): boolean => {
  for (let i = 0; i < haystack.length; i++) {
    if (haystack[i] === needle) {
      return true;
    }
  }

  return false;
};
```

<br />

## Binary Search

Binary search is one of the best to search if the array that you are searching in is a sorted array.
The basic idea of binary search is to check if the element in a range and we repeatedly half this range and keep on checking

[1,2,3,4,5,6] Search: 4 exist or not \
Lets half the range take mid as 3 and see if mid is higher than element. In this case yes so we neglect everything less than 3 because the array is sorted.
again half by taking mid as 5 this time we know it is on the left side and we found it.

The time complexity of this search will be log(N) how lets see it.
if `N` is the number of elements in the array \
We are making the range half all the time and hence reducing the range of search \
so it will go like this N/2, N/4, N/8, N/16, N/32 like this until `K` we find the element or complete the array. \
so \
`N/2^K = 1 ` \
`N = 2^K` \
` log N = K` \
K is number of half's we did to reach the element or complete array
<br />

4096 -> 2048 -> 1024 -> 512 -> 256 -> 128 -> 64 -> 32 -> 16 -> 8 -> 4 -> 2-> 1
total 12 steps K = 12 also `log 4096 = 12`

```dotnetcli
const binarySearch = (haystack: Array<number>, needle: number): boolean => {
  if (haystack.length <= 1) return false;

  let low = 0;
  let high = haystack.length - 1;

  while (low < high) {
    const mid = Math.floor(low + (high - low) / 2);
    const midEl = haystack[mid];
    console.log({ low, high, mid, midEl });
    if (midEl === needle) return true;
    if (midEl > needle) high = mid;
    else low = mid;
  }

  return false;
};

const binarySearchRecursively = (
  haystack: Array<number>,
  low: number,
  high: number,
  needle: number
) => {
  let mid = Math.floor((high - low) / 2 + low);
  const midEl = haystack[mid];
  console.log({ low, high, mid });

  switch (true) {
    case midEl === needle:
      return true;
    case high - low === 0:
      return false;
    case midEl < needle:
      return binarySearchRecursively(haystack, mid + 1, high, needle);
    case midEl > needle:
      return binarySearchRecursively(haystack, low, mid, needle);
  }
};

const arr = [1, 2, 9, 11, 15, 24, 27, 31, 41, 55, 61];
console.log(binarySearchRecursively(arr, 0, arr.length - 1, 15));
console.log(binarySearch(arr, 15));
```

NOTE: ADD EGG DROPPING PROBLEM HERE

<br />

# Sort

## Bubble Sort

In Bubble sort what we basically do it iterate over the array to check consecutive numbers and check if the first one is greater than the second one if the comparison is true swap those two number and proceed further.

With each iteration what we will see is the last digit will become the highest number in the array.

```
const arr: Array<number> = [1, 24, 9, 61, 15, 24, 27, 55, 61, 55, 27];
```

After a single sorting iteration the array will look something like this

```
const arr: Array<number> = [1, 24, 9, 15, 24, 27, 55, 61, 55, 27, 61];
```

So 61 is at the end now. So with each iteration we know the last element will always be the highest element of the array and hence with every iteration we will start ignoring the last digit of the array.
Next time you will only iterate from index [0] to [9] as 61 one we know is the highest next time till index [8] only.

Like wise as we keep excluding the last digit everytime the time complexity will become something like

n + n-1 + n-2 + n-3 .......... = O(n(n+1)/2)

```dotnetcli
const bubbleSort = (arr: Array<number>) => {
  for (let j: number = 0; j <= arr.length - 1; j++) {
    for (let i: number = 0; i <= arr.length - j - 1; i++) {
      console.log({ i, j, arr });
      let temp;
      if (arr[i] > arr[i + 1]) {
        temp = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = temp;
      }
    }
  }
  return arr;
};

const arr: Array<number> = [1, 24, 9, 61, 15, 24, 27, 55, 61, 55, 27];
console.log(bubbleSort(arr));
```

## Merge Sort

Like QuickSort, Merge Sort is a Divide and Conquer algorithm. It divides the input array into two halves, calls itself for the two halves, and then merges the two sorted halves. The merge() function is used for merging two halves. The merge(arr, l, m, r) is a key process that assumes that arr[l..m] and arr[m+1..r] are sorted and merges the two sorted sub-arrays into one.

The time complexity of Merge Sort is O(n\*Log n) in all the 3 cases (worst, average and best) as the merge sort always divides the array into two halves and takes linear time to merge two halves.

**Sudo code**

```
MergeSort(arr[], l,  r)
If r > l
     1. Find the middle point to divide the array into two halves:
             middle m = l+ (r-l)/2
     2. Call mergeSort for first half:
             Call mergeSort(arr, l, m)
     3. Call mergeSort for second half:
             Call mergeSort(arr, m+1, r)
     4. Merge the two halves sorted in step 2 and 3:
             Call merge(arr, l, m, r)
```

This algorithm becomes O(n^2) in the worst case scenario that is when the array is in descending order.
