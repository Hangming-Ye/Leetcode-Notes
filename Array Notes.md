# Array

## Concept

* Definition: **An array is a collection of data of the same type stored in sequential memory space **
* Memory Address of Multiple Dimension Array:  Memory address of multiple dimension array varies in different language. For example, in c++, it is continuous, while in java it is not.



## Techniques

### Binary Search

* Perquisite:

  * The array must be in order.
  * The array does not contain duplicate element (otherwise the index return is not unique, doesn't matter for judging existence.)

* 704. Code (left closed, right closed)

  ```java
  public int search(int[] nums, int target) {
      if (target < nums[0] || target > nums[nums.length - 1]) {
          return -1;
      }
      int left = 0, right = nums.length - 1;
      while (left <= right) {
          int mid = left + ((right - left) >> 1);
          if (nums[mid] == target)
              return mid;
          else if (nums[mid] < target)
              left = mid + 1;
          else if (nums[mid] > target)
              right = mid - 1;
      }
      return -1;
  }
  ```



* Code (left closed, right open)

  ```java
  public int search(int[] nums, int target) {
      if(target < nums[0] || target > nums[nums.length-1]){
          return -1;
      }
      int lo = 0, hi = nums.length;
      while(lo < hi){
          int mid = lo + (hi - lo) / 2;
          if(nums[mid] == target){
              return mid;
          }else if(nums[mid] > target){
              hi = mid;
          }else{
              lo = mid + 1;
          }
      }
      return -1;
  }
  ```

  >Reminder:
  >
  >1. About mid calculation:
  >
  >   To avoid the overflow of mid calculation, using 
  >
  >   ```java
  >   int mid = lo + (hi - lo) / 2;
  >   //or
  >   int mid = left + ((right - left) >> 1);
  >   ```
  >
  >2. [left, right]
  >   * `while (left <= right)`  use `<=` because this make sense, the algorithm need to handle the right corner `nums[right]`.
  >   * for left search, update the right to `middle -1` and for right search, update the right to `middle + 1`. Because the `nums[mid]` is not the target element.
  >3. [left, right)
  >   * `while (left < right)`  use `<` because `left == right` make no sense, `right` is out of bound.
  >   * for left search, update right to `mid` since it is not searched.

* #### Example - 35. Search Insert Position

  > Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the `index where it would be if it were inserted in order`.
  >
  > You must write an algorithm with `O(log n)` runtime complexity.

* Code

  ```java
  public int searchInsert(int[] nums, int target) {
      int lo = 0, hi = nums.length, mid;
      while(lo < hi){
          mid = lo + (hi - lo) / 2;
          if(nums[mid] == target){
              //1 case
              // find
              return mid;
          }else if(nums[mid] > target){
              hi = mid;
          }else{
              lo = mid + 1;
          }
      }
      return hi;
      // 3 case
      // before [0,0)
      //in the middle
      //after [hi,hi)
  }
  ```

* #### Example - 69. Sqrt(x)

  > Given a non-negative integer `x`, return *the square root of* `x` *rounded down to the nearest integer*. The returned integer should be **non-negative** as well.
  >
  > You **must not use** any built-in exponent function or operator.
  >
  > - `0 <= x <= 2^31 - 1`

* Code

  ```java
  class Solution {
    public int mySqrt(int x) {      
      int l = 1;
      int r = x;
      while (l <= r) {
        int m = l + (r - l) / 2;
        if (m > x / m) {
          r = m - 1;
        } else {
          l = m + 1;
        }
      }
      return r;
    }
  }
  ```

  >Reminder:
  >
  >1. Why use `\`, not `*`?
  >   *  The data will overflow when two large int * int
  >2. Why use [lo, hi], not [lo, hi)?
  >   * Because the range of x is [0, 2^31-1], when using [lo, hi) as search range, the hi should be hi + 1, this makes `x` overflow.

* Code (Bad Approach)

  ```java
  public int mySqrt(int x) {
      int lo = 0, hi = x;
      if(x<2){
          return x;
      }
      while(lo < hi){
          int mid = lo + (hi - lo) / 2;
          long tmp = (long) mid * mid;
          if(tmp == (long)x){
              return (int)mid;
          }else if(tmp > (long)x){
              hi = mid;
          }else{
              lo = mid + 1;
          }
      }
      return hi-1;
  }
  ```

  

* Example - 34. Find First and Last Position of Element in Sorted Array

  > Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.
  >
  > If `target` is not found in the array, return `[-1, -1]`.

* Code

  ```java
  public int[] searchRange(int[] nums, int target) {
      int lo = 0, hi = nums.length;
      int[] ans = {-1,-1};
      
      //find the leftmost
      while(lo < hi){
          int mid = lo + (hi - lo) / 2;
          if(nums[mid] == target){
              ans[0] = mid;
              hi = mid;
          }else if(nums[mid] > target){
              hi = mid;
          }else{
              lo = mid + 1;
          }
      }
      
      //find the right most
      lo = 0;
      hi = nums.length;
      while(lo < hi){
          int mid = lo + (hi - lo) / 2;
          if(nums[mid] == target){
              ans[1] = mid;
              lo = mid+1;
          }else if(nums[mid] > target){
              hi = mid;
          }else{
              lo = mid + 1;
          }
      }
      return ans;
  }
  ```

  