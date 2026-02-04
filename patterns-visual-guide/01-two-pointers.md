# 🎯 Two Pointers Pattern

## 📊 Visual Representation

```
Array: [1, 2, 3, 4, 5, 6, 7, 8, 9]
         ↑                       ↑
        left                   right

Step 1: Compare/Process arr[left] and arr[right]
Step 2: Move pointers based on condition
        - Move left pointer right: left++
        - Move right pointer left: right--
        - Or move both

Final: left and right meet in the middle
```

### Visual Example: Two Sum II (Sorted Array)
```
Target = 9
[1, 2, 3, 4, 5, 6, 7, 8, 9]
 ↑                       ↑
 L                       R
 1 + 9 = 10 > 9, so R--

[1, 2, 3, 4, 5, 6, 7, 8, 9]
 ↑                    ↑
 L                    R
 1 + 8 = 9 ✓ Found!
```

## 💻 Java Template Code

```java
import java.util.*;

public class TwoPointers {
    
    // Template 1: Opposite Direction - SORTED ARRAY REQUIRED
    // Use for: Two Sum II, Container With Most Water
    public int[] twoPointersOpposite(int[] arr, int target) {
        // IMPORTANT: Array MUST be sorted for this to work
        // For unsorted arrays, use HashMap approach instead
        
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            int sum = arr[left] + arr[right];
            
            if (sum == target) {
                return new int[]{left, right};
            } else if (sum < target) {
                left++;  // Need larger sum, move left pointer right
            } else {
                right--; // Need smaller sum, move right pointer left
            }
        }
        
        return new int[]{-1, -1}; // Not found
    }
    
    // Template 1b: Two Sum - UNSORTED ARRAY (HashMap approach)
    // Use for: LeetCode #1 Two Sum
    public int[] twoSum(int[] nums, int target) {
        // For UNSORTED arrays like [3,2,4], use HashMap
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }
            
            map.put(nums[i], i);
        }
        
        return new int[]{-1, -1};
    }
    
    // Template 2: Same Direction (fast/slow variation)
    // Use for: Remove Duplicates, Move Zeroes
    public int removeDuplicates(int[] arr) {
        if (arr.length == 0) return 0;
        
        int slow = 0;  // Points to last unique element
        
        for (int fast = 1; fast < arr.length; fast++) {
            if (arr[fast] != arr[slow]) {
                slow++;
                arr[slow] = arr[fast];
            }
        }
        
        return slow + 1;  // Length of unique elements
    }
    
    // Template 3: Palindrome Check
    // Use for: Valid Palindrome, Palindrome String
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            // Skip non-alphanumeric characters
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            
            if (Character.toLowerCase(s.charAt(left)) != 
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }
            
            left++;
            right--;
        }
        
        return true;
    }
    
    // Template 4: Opposite Direction - Greedy (no target value)
    // Use for: Container With Most Water
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;
        
        while (left < right) {
            // Calculate current area
            int width = right - left;
            int currentHeight = Math.min(height[left], height[right]);
            int currentArea = width * currentHeight;
            
            maxArea = Math.max(maxArea, currentArea);
            
            // Move the pointer with smaller height
            // (greedy: we want to find potentially taller lines)
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        
        return maxArea;
    }
}
```

## 🧩 Pseudocode

```
ALGORITHM TwoPointersSorted(array, target):
    // PRECONDITION: array must be sorted
    left ← 0
    right ← length(array) - 1
    
    WHILE left < right DO:
        current_value ← array[left] + array[right]
        
        IF current_value EQUALS target THEN:
            RETURN [left, right]
        ELSE IF current_value < target THEN:
            left ← left + 1
        ELSE:
            right ← right - 1
    END WHILE
    
    RETURN not_found
END ALGORITHM

ALGORITHM TwoSumUnsorted(array, target):
    // For UNSORTED arrays
    map ← empty HashMap
    
    FOR i ← 0 TO length(array) - 1 DO:
        complement ← target - array[i]
        
        IF map contains complement THEN:
            RETURN [map[complement], i]
        END IF
        
        map[array[i]] ← i
    END FOR
    
    RETURN not_found
END ALGORITHM
```

## ⚠️ CRITICAL: Sorted vs Unsorted

### ❌ **FAILS on [3,2,4] with target 6:**
```java
// Two Pointers on UNSORTED array
[3, 2, 4]
 ↑     ↑
 L     R
3 + 4 = 7 > 6, so R--

[3, 2, 4]
 ↑  ↑
 L  R
3 + 2 = 5 < 6, so L++

[3, 2, 4]
    ↑↑
    LR
left >= right, STOPS - WRONG! Answer exists but not found
```

### ✅ **WORKS with HashMap:**
```java
// HashMap approach
i=0: nums[0]=3, need 6-3=3, not in map, add {3:0}
i=1: nums[1]=2, need 6-2=4, not in map, add {2:1}
i=2: nums[2]=4, need 6-4=2, FOUND in map at index 1!
Return [1, 2] ✓
```

### ✅ **Two Pointers WORKS if sorted first:**
```java
// Sort [3,2,4] → [2,3,4] (but lose original indices!)
[2, 3, 4]
 ↑     ↑
 L     R
2 + 4 = 6 ✓ Found! (indices 0,2 in sorted array)
```

## ⏱️ Complexity Analysis

| Approach | Time | Space | When to Use |
|----------|------|-------|-------------|
| **Two Pointers (sorted)** | O(n) | O(1) | Array is already sorted |
| **Sort + Two Pointers** | O(n log n) | O(1) | Can lose original indices |
| **HashMap (unsorted)** | O(n) | O(n) | Need original indices |

## 🎯 When to Use

✅ **Use Two Pointers when:**
- Array/string is **already sorted** or sorting doesn't matter
- Looking for **pairs** that meet a condition
- Need to find **palindrome** patterns
- Removing **duplicates** in-place
- **Partitioning** array around a value
- Don't need to preserve original indices

❌ **Don't use Two Pointers when:**
- Array is **unsorted** and you need **original indices** (use HashMap)
- Sorting would change the problem (like Two Sum #1)
- Need to find triplets/quadruplets (use three pointers + loop)

## 📝 Example Problems

| Problem | Sorted? | Use Two Pointers? | Alternative |
|---------|---------|-------------------|-------------|
| Two Sum (#1) | ❌ No | ❌ No | ✅ HashMap O(n) |
| Two Sum II (#167) | ✅ Yes | ✅ Yes | HashMap works too |
| Valid Palindrome (#125) | N/A | ✅ Yes | - |
| Remove Duplicates (#26) | ✅ Yes | ✅ Yes | - |
| Container With Most Water (#11) | ❌ No | ✅ Yes (greedy) | - |
| 3Sum (#15) | Need to sort | ✅ Yes + loop | - |
| Trapping Rain Water (#42) | ❌ No | ✅ Yes (greedy) | DP approach |

## 🔗 Visual Resources

- **VisuAlgo Two Pointers:** [visualgo.net/en/array](https://visualgo.net/en/array)
- **Two Sum Visualization:** [algorithm-visualizer.org](https://algorithm-visualizer.org/brute-force/two-pointer-technique)

## 💡 Pro Tips

1. **Check if sorted:** First question to ask - "Is the array sorted?"
2. **Preserve indices?** If yes and unsorted → use HashMap, not two pointers
3. **Can you sort?** Some problems allow sorting (3Sum), others don't (Two Sum)
4. **Greedy decisions:** Container With Most Water moves the smaller pointer
5. **Edge cases:** Empty array, single element, all duplicates, negative numbers

## 🎓 Learning Path

1. **Start with:** Valid Palindrome (#125) - simplest, no sorting needed
2. **Then:** Two Sum II (#167) - classic two pointers on sorted array
3. **Compare:** Two Sum (#1) - understand why two pointers fails here
4. **Master:** Container With Most Water (#11) - greedy two pointers
5. **Challenge:** 3Sum (#15) - sort first, then use two pointers in loop

## 🐛 Common Mistakes

❌ Using two pointers on unsorted array when indices matter  
❌ Forgetting to check `left < right` boundary  
❌ Not handling duplicates in 3Sum  
❌ Moving both pointers when only one should move  
✅ Always verify: "Is this array sorted or can I sort it?"