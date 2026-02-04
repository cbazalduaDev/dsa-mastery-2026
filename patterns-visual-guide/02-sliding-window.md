# 🪟 Sliding Window Pattern

## 📊 Visual Representation

```
Array: [1, 3, 2, 6, -1, 4, 1, 8, 2]
Find maximum sum of subarray of size k=3

Window slides from left to right:
[1, 3, 2] 6, -1, 4, 1, 8, 2   sum = 6
 1,[3, 2, 6]-1, 4, 1, 8, 2   sum = 11
 1, 3,[2, 6,-1] 4, 1, 8, 2   sum = 7
 1, 3, 2,[6,-1, 4] 1, 8, 2   sum = 9
 1, 3, 2, 6,[-1, 4, 1] 8, 2   sum = 4
 1, 3, 2, 6, -1,[4, 1, 8] 2   sum = 13 ← Maximum!
 1, 3, 2, 6, -1, 4,[1, 8, 2]  sum = 11
```

### Dynamic Window Example: Longest Substring Without Repeating Characters
```
String: "abcabcbb"
         ↑     ↑
       left  right

Step 1: "a" - valid, expand right
Step 2: "ab" - valid, expand right
Step 3: "abc" - valid, expand right (length = 3)
Step 4: "abca" - duplicate 'a'! Shrink left
        "bca" - valid, expand right
Step 5: "bcab" - duplicate 'b'! Shrink left
        "cab" - valid, expand right
```

## 💻 Java Template Code

```java
import java.util.*;

public class SlidingWindow {
    
    // Template 1: Fixed Window Size
    // Use for: Maximum sum of subarray size k
    public int maxSumFixedWindow(int[] arr, int k) {
        if (arr.length < k) return -1;
        
        // Calculate sum of first window
        int windowSum = 0;
        for (int i = 0; i < k; i++) {
            windowSum += arr[i];
        }
        
        int maxSum = windowSum;
        
        // Slide the window
        for (int i = k; i < arr.length; i++) {
            windowSum += arr[i] - arr[i - k];  // Add new, remove old
            maxSum = Math.max(maxSum, windowSum);
        }
        
        return maxSum;
    }
    
    // Template 2: Dynamic Window (Expand/Shrink)
    // Use for: Longest substring without repeating characters
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> charIndex = new HashMap<>();
        int maxLength = 0;
        int left = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);
            
            // If duplicate found, shrink window from left
            if (charIndex.containsKey(currentChar)) {
                left = Math.max(left, charIndex.get(currentChar) + 1);
            }
            
            charIndex.put(currentChar, right);
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
    
    // Template 3: Dynamic Window with Condition
    // Use for: Minimum window substring, subarrays with sum ≥ target
    public int minSubArrayLen(int target, int[] nums) {
        int minLength = Integer.MAX_VALUE;
        int left = 0;
        int currentSum = 0;
        
        for (int right = 0; right < nums.length; right++) {
            currentSum += nums[right];
            
            // Shrink window while condition is met
            while (currentSum >= target) {
                minLength = Math.min(minLength, right - left + 1);
                currentSum -= nums[left];
                left++;
            }
        }
        
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
    
    // Template 4: Character Frequency Window
    // Use for: Permutation in string, anagrams
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) return false;
        
        int[] s1Count = new int[26];
        int[] s2Count = new int[26];
        
        // Count characters in s1
        for (char c : s1.toCharArray()) {
            s1Count[c - 'a']++;
        }
        
        // Sliding window over s2
        for (int i = 0; i < s2.length(); i++) {
            s2Count[s2.charAt(i) - 'a']++;
            
            // Remove leftmost character if window exceeds s1 length
            if (i >= s1.length()) {
                s2Count[s2.charAt(i - s1.length()) - 'a']--;
            }
            
            // Check if current window matches s1
            if (Arrays.equals(s1Count, s2Count)) {
                return true;
            }
        }
        
        return false;
    }
}
```

## 🧩 Pseudocode

```
ALGORITHM FixedSlidingWindow(array, k):
    windowSum ← sum of first k elements
    maxSum ← windowSum
    
    FOR i ← k TO length(array) - 1 DO:
        windowSum ← windowSum + array[i] - array[i - k]
        maxSum ← max(maxSum, windowSum)
    END FOR
    
    RETURN maxSum
END ALGORITHM

ALGORITHM DynamicSlidingWindow(array, condition):
    left ← 0
    result ← initialize based on problem
    
    FOR right ← 0 TO length(array) - 1 DO:
        // Expand window
        add array[right] to window
        
        // Shrink window while condition violated
        WHILE condition is violated DO:
            remove array[left] from window
            left ← left + 1
        END WHILE
        
        // Update result
        result ← update based on current window
    END FOR
    
    RETURN result
END ALGORITHM
```

## ⏱️ Complexity Analysis

| Metric | Fixed Window | Dynamic Window | Explanation |
|--------|--------------|----------------|-------------|
| **Time** | O(n) | O(n) | Single pass, each element visited at most twice |
| **Space** | O(1) | O(k) | k = size of auxiliary data (HashMap, Set) |

## 🎯 When to Use

✅ **Use Sliding Window when:**
- Finding **subarrays** or **substrings** with specific properties
- Problem mentions **contiguous** elements
- Looking for **longest/shortest** window satisfying a condition
- Need to find **maximum/minimum** sum/product of subarray
- Keywords: "consecutive", "contiguous", "substring", "subarray"

❌ **Don't use when:**
- Elements can be non-contiguous (use Two Pointers or DP)
- Need to find ALL subarrays (might need backtracking)
- Array is not linear (use different approach for 2D arrays)

## 📝 Example Problems

| Problem | Difficulty | LeetCode # | Window Type |
|---------|-----------|------------|-------------|
| Maximum Sum Subarray Size K | Easy | - | Fixed |
| Longest Substring Without Repeating | Medium | #3 | Dynamic |
| Minimum Window Substring | Hard | #76 | Dynamic |
| Permutation in String | Medium | #567 | Fixed + Frequency |
| Longest Repeating Character Replacement | Medium | #424 | Dynamic |
| Sliding Window Maximum | Hard | #239 | Fixed + Deque |
| Fruit Into Baskets | Medium | #904 | Dynamic |
| Max Consecutive Ones III | Medium | #1004 | Dynamic |

## 🔗 Visual Resources

- **VisuAlgo Sliding Window:** [visualgo.net](https://visualgo.net)
- **Algorithm Visualizer:** [algorithm-visualizer.org](https://algorithm-visualizer.org)

## 💡 Pro Tips

1. **Fixed vs Dynamic:** Fixed window = constant size, Dynamic window = expand/shrink based on condition
2. **HashMap for frequency:** Use when tracking character counts or element frequencies
3. **Two pointers:** Sliding window is a specialized form of two pointers (same direction)
4. **Shrink condition:** Always define WHEN to shrink the window clearly
5. **Edge cases:** Empty array, window size larger than array, all elements same

## 🎓 Learning Path

1. **Start with:** Maximum Sum Subarray (fixed window) - easiest pattern
2. **Then try:** Longest Substring Without Repeating (#3) - classic dynamic window
3. **Master:** Minimum Window Substring (#76) - combines all concepts
4. **Challenge:** Sliding Window Maximum (#239) - requires deque optimization

## 🐛 Common Mistakes

❌ Forgetting to remove leftmost element when sliding  
❌ Not handling window size correctly (off-by-one errors)  
❌ Using sliding window for non-contiguous problems  
❌ Not updating max/min result at the right time  
✅ Always ask: "Is this problem about contiguous elements?"

## 🔄 Comparison: Sliding Window vs Two Pointers

| Feature | Sliding Window | Two Pointers |
|---------|----------------|--------------|
| **Direction** | Same (both move right) | Can be opposite or same |
| **Use Case** | Contiguous subarrays | Pairs, sorted arrays |
| **Window** | Dynamic size | Fixed relationship |
| **Example** | Longest substring | Two Sum II |