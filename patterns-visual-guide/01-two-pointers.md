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
public class TwoPointers {
    
    // Template 1: Opposite Direction (most common)
    public int[] twoPointersOpposite(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            int sum = arr[left] + arr[right];
            
            if (sum == target) {
                return new int[]{left, right};
            } else if (sum < target) {
                left++;  // Need larger sum
            } else {
                right--; // Need smaller sum
            }
        }
        
        return new int[]{-1, -1}; // Not found
    }
    
    // Template 2: Same Direction (fast/slow variation)
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
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
}
```

## 🧩 Pseudocode

```
ALGORITHM TwoPointers(array, target):
    left ← 0
    right ← length(array) - 1
    
    WHILE left < right DO:
        current_value ← PROCESS(array[left], array[right])
        
        IF current_value EQUALS target THEN:
            RETURN solution
        ELSE IF current_value < target THEN:
            left ← left + 1
        ELSE:
            right ← right - 1
    END WHILE
    
    RETURN not_found
END ALGORITHM
```

## ⏱️ Complexity Analysis

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | O(n) | Single pass through array |
| **Space** | O(1) | Only using two pointer variables |

## 🎯 When to Use

✅ **Use Two Pointers when:**
- Array/string is **sorted** (or can be sorted)
- Looking for **pairs** that meet a condition
- Need to find **palindrome** patterns
- Removing **duplicates** in-place
- **Partitioning** array around a value

❌ **Don't use when:**
- Array is unsorted and can't be sorted
- Need to find triplets/quadruplets (use three pointers instead)
- Need to track indices in original array after sorting

## 📝 Example Problems

| Problem | Difficulty | LeetCode # | Pattern Type |
|---------|-----------|------------|--------------|
| Two Sum II | Easy | #167 | Opposite Direction |
| Valid Palindrome | Easy | #125 | Opposite Direction |
| Remove Duplicates | Easy | #26 | Same Direction |
| Container With Most Water | Medium | #11 | Opposite Direction |
| 3Sum | Medium | #15 | Opposite + Loop |
| Trapping Rain Water | Hard | #42 | Opposite Direction |

## 🔗 Visual Resources

- **VisuAlgo Two Pointers:** [visualgo.net/en/array](https://visualgo.net/en/array)
- **Algorithm Visualizer:** [algorithm-visualizer.org](https://algorithm-visualizer.org/brute-force/two-pointer-technique)

## 💡 Pro Tips

1. **Always check boundaries:** Ensure `left < right` to avoid infinite loops
2. **Sorted arrays:** Most two-pointer problems require sorted input
3. **Multiple solutions:** Some problems can use both hash map OR two pointers - two pointers is usually O(1) space
4. **Edge cases:** Empty array, single element, all duplicates

## 🎓 Learning Path

1. Start with: **Valid Palindrome (#125)** - simplest pattern
2. Then try: **Two Sum II (#167)** - classic opposite direction
3. Master: **Container With Most Water (#11)** - greedy decision making
4. Challenge: **3Sum (#15)** - combining two pointers with iteration
