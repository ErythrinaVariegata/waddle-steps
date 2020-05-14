# 从0开始的力扣记录！
- [数组 - Array](#数组)
- [链表 - LinkedList](#链表)

```text
                          _/                        _/_/                                     
   _/_/_/      _/_/    _/_/_/_/    _/_/          _/      _/  _/_/    _/_/    _/_/_/  _/_/    
  _/    _/  _/    _/    _/      _/_/_/_/      _/_/_/_/  _/_/      _/    _/  _/    _/    _/   
 _/    _/  _/    _/    _/      _/              _/      _/        _/    _/  _/    _/    _/    
_/    _/    _/_/        _/_/    _/_/_/        _/      _/          _/_/    _/    _/    _/     
                                                                               
                                           
   _/_/_/_/    _/_/    _/  _/_/    _/_/    
      _/    _/_/_/_/  _/_/      _/    _/   
   _/      _/        _/        _/    _/    
_/_/_/_/    _/_/_/  _/          _/_/       
                                           
```

## 数组
数组中时常会用到双指针法，题目中的双指针可能是同向（追及）遍历，也可能是反向（相遇）遍历。根据题目的不同可能会涉及哈希表、二分查找等知识点

- **No.1 - [两数之和](https://leetcode-cn.com/problems/two-sum)**

  利用暴力法时间复杂度比较高，为`O(n^2)`
  题目中另一标签是哈希表，可以采用`unordered_map`（C++）或`HashMap`（Java）来存储已经遍历过且表中没有另一半的元素，时间复杂度可以降到`O(n)`
  <details>
  <summary>Solution</summary>
  
  ```java
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return null;
    }
  ```
  
  </details>
  
- **No.26 - [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)**

  由于数组已经是排好序的，根据题目要求空间复杂度为`O(1)`，采用双指针法遍历即可
  <details>
  <summary>Solution</summary>
  
  ```java
    public int removeDuplicates(int[] nums) {
      int pos = 0;
      int next = 0;
      while (next < nums.length) {
        while (next < nums.length && nums[next] == nums[pos]) {
          next++;
        }
        
        if (next == pos + 1) {
          pos++;
        } else if (next >= nums.length) {
          break;
        } else {
          nums[++pos] = nums[next++];
        }
      }
      
      return pos + 1;
    }
  ```
  
  </details>

- **No.11 - [盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water)**

  暴力法时间复杂度比较高，为`O(n^2)`，作为中等难度的题目相比AC不了。
  想到长方形的面积是由其宽度和高度决定的，高度一定的情况下越宽越好，然而在宽度缩小的情况下高度越高才能保证面积最大。利用这两个性质采用双指针法。
  双向同时向中间遍历，当遍历完整个数组后即可得到答案
  <details>
  <summary>Solution</summary>
  
  ```java
    public int maxArea(int[] height) {
      int left = 0;
      int right = height.length - 1;
      int area = 0;
      while (left < right) {
        area = Math.max(Math.min(height[left], height[right]) * (right - left), area);
        if (height[left] < height[right]) {
          left++;
        } else {
          right--;
        }
      }
      return area;
    }
  ```
  
  </details>
  
- **No.55 - [跳跃游戏](https://leetcode-cn.com/problems/jump-game)**

  由题意判断是否能跳至数组尾端，采用贪心的思想进行解题。
  本人的解题思路是选择能够跳得最远的下一个点进行跳跃，而官方题解的思路在于选择最远的距离进行跳跃，略有不同。
  本人的代码略长但时间却比官方题解要少那么1ms，舒服
  <details>
  <summary>Solution</summary>
  
  ```java
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = Math.max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;
                }
            }
        }
        return false;
    }
  ```
  
  </details>

- **No.56 - [合并区间](https://leetcode-cn.com/problems/merge-intervals)**

  首先对区间列表根据每个区间的左端进行排序，
  对于b区间的左端小于等于a区间的右端的情况，考虑合并a区间与b区间，取a区间的左端以及a、b两区间右端的最大值作为新区间的左右两端。
  <details>
  <summary>Solution</summary>
  
  ```java
    public int[][] merge(int[][] intervals) {
      if (intervals == null || intervals.length <= 1) {
        return intervals;
      }
      Arrays.sort(intervals, new Comparator<int[]>(){
        @Override
        public int compare(int[] a, int[] b) {
          return a[0] - b[0];
        }
      });
      ArrayList<int[]> list = new ArrayList<>();
      int i=0;
      int n = intervals.length;
      while(i<n){
          int left = intervals[i][0];
          int right = intervals[i][1];
          while(i < n - 1 && right >= intervals[i+1][0]){
              right = Math.max(right, intervals[i+1][1]);
              i++;
          }
          list.add(new int[] {left,right});
          i++;
      }
      return list.toArray(new int[list.size()][2]);
    }
  ```
  
  </details>
  
[BACK TO TOP ⬆︎](#从0开始的力扣记录)

## 链表
在专栏中作者推荐了5道需要多写多练的题目：206、141、21、19、876。除了做这些以外，我根据力扣添加其拓展题目

- **No.206 - [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)**

  自己只想到了迭代的解法：每次迭代都建立一个临时结点来存储子节点，在迭代的过程当中注意指针的指向。
  官方给出的递归的方法是假设链表其余部分已经反转，再反转前面部分的链表。
  <details>
  <summary>Solution1: 迭代</summary>
  
  ```java
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
  ```
  
  </details>
  <details>
  <summary>Solution2: 递归</summary>

  ```java
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode p = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return p;
    }
  ```
  
  </details>
[BACK TO TOP ⬆︎](#从0开始的力扣记录)
