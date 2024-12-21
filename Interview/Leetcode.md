99-Recover Binary Search Tree
```java
class Solution {
    TreeNode prev = null, first = null, second = null;

    // 中序遍历函数
    void inorder(TreeNode root) {
        if (root == null)
            return;

        // 递归遍历左子树
        inorder(root.left);

        // 检查当前节点和前一个节点的值关系
        if (prev != null && root.val < prev.val) {
            // 第一次发现逆序对时，记录前一个节点为first
            if (first == null)
                first = prev;
            // 每次发现逆序对都记录当前节点为second
            second = root;
        }

        // 更新前一个节点
        prev = root;

        // 递归遍历右子树
        inorder(root.right);
    }

    // 恢复树的函数
    public void recoverTree(TreeNode root) {
        if (root == null)
            return;

        // 进行中序遍历，找到两个错误的节点
        inorder(root);

        // 交换两个错误节点的值
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
}

// 测试用例
public class Main {
    public static void main(String[] args) {
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(1);
        root.right = new TreeNode(4);
        root.right.left = new TreeNode(2);

        Solution sol = new Solution();
        sol.recoverTree(root);

        // 打印恢复后的树，验证结果
        printInorder(root);  // 应该打印1, 2, 3, 4
    }

    // 中序打印树
    public static void printInorder(TreeNode node) {
        if (node == null)
            return;
        printInorder(node.left);
        System.out.print(node.val + " ");
        printInorder(node.right);
    }
}

// 树节点类
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```


时间复杂度
该算法主要由中序遍历和交换两个节点的值组成。中序遍历整个树需要访问每个节点一次，因此时间复杂度为O(N)，其中N是树中节点的数量。

空间复杂度
空间复杂度主要由递归调用栈的深度决定。在最坏的情况下（树是一个链表），递归调用栈的深度等于树的高度H。对于一棵平衡树，高度H = O(log N)。因此，空间复杂度为O(H)。在最坏的情况下，空间复杂度为O(N)。

总结：

时间复杂度：O(N)
空间复杂度：O(H)，在最坏情况下为O(N)

## 1110 Delete Nodes And Return Forest
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    // 定义结果列表用于存储最终的森林
    private List<TreeNode> forest = new ArrayList<>();
    private Set<Integer> toDeleteSet = new HashSet<>();

    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        // 将需要删除的节点值存入Set中，以便快速查找
        for (int val : to_delete) {
            toDeleteSet.add(val);
        }

        // 处理根节点，检查它是否需要被删除
        root = removeNodes(root, null);
        if (root != null) {
            forest.add(root);
        }

        return forest;
    }

    private TreeNode removeNodes(TreeNode node, TreeNode parent) {
        if (node == null) {
            return null;
        }

        // 递归处理左子树和右子树
        node.left = removeNodes(node.left, node);
        node.right = removeNodes(node.right, node);

        // 如果当前节点需要被删除
        if (toDeleteSet.contains(node.val)) {
            // 如果左子树不为空且不在删除列表中，将左子树的根节点加入森林
            if (node.left != null && !toDeleteSet.contains(node.left.val)) {
                forest.add(node.left);
            }
            // 如果右子树不为空且不在删除列表中，将右子树的根节点加入森林
            if (node.right != null && !toDeleteSet.contains(node.right.val)) {
                forest.add(node.right);
            }
            // 返回null表示删除当前节点
            return null;
        }

        // 当前节点不需要被删除，返回当前节点
        return node;
    }
}
```

时间复杂度
该算法主要包括以下步骤：

遍历to_delete数组并将其值存入HashSet，这需要O(D)时间，其中D是to_delete数组的长度。
后序遍历整个树并处理每个节点，时间复杂度为O(N)，其中N是树中节点的数量。
综合来看，总时间复杂度为O(N + D)。在一般情况下，D远小于N，因此可以近似认为时间复杂度为O(N)。

空间复杂度
空间复杂度由以下部分组成：

存储需要删除的节点值的HashSet，占用O(D)空间。
递归调用栈的深度。在最坏的情况下（树是一个链表），递归调用栈的深度等于树的高度H。对于一棵平衡树，高度H = O(log N)。因此，空间复杂度为O(H)。在最坏的情况下，空间复杂度为O(N)。
结果列表forest的大小，最多包含N个节点，每个节点都是一棵树的根。
综合来看，总空间复杂度为O(N + D + H)。由于D和H通常小于N，因此可以近似认为空间复杂度为O(N)。

总结：

时间复杂度：O(N + D)
空间复杂度：O(N + D + H)，在最坏情况下为O(N)

## 2386.Find the K-Sum of an Array
Intuitively thinking
1. The largest sum of the array is the sum of all positive numbers.
2. The sum of any subsequences is equivalent to the subtracting some positive numbers and adding some negative numbers from the sum.
3. The second largest sum of the array is the [sum - the minimum number in the absolute values array]
Implementation
1. We traversal this array. For one number in the array, we sum it if it's a positive number; we change it into positive if it's negative. We get the sum of all positive numbers and an array with absolute values
2. Sort the array.
3. Use max heap to maintain the max sum of subsequences.
![[Images/Pasted image 20240627143420.png]]
这样是用pq的原因

```java
import java.util.*;

public class Solution {
    public static long kSum(int[] nums, int k) {
        long sum = 0L;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                sum += nums[i];
            } else {
                nums[i] = -nums[i];
            }
        }
        Arrays.sort(nums);
        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) 
	        -> Long.compare(b[0], a[0]));
        pq.offer(new long[]{sum, 0});
        while (k > 1) {
            long[] top = pq.poll();
            long s = top[0];
            int i = (int) top[1];
            if (i < nums.length) {
                pq.offer(new long[]{s - nums[i], i + 1});
                if (i > 0) {
                    pq.offer(new long[]{s - nums[i] + nums[i - 1], i + 1});
                }
            }
            k--;
        }
        return pq.peek()[0];
    }

    public static void main(String[] args) {
        int[] nums1 = {2, 4, -2};
        int k1 = 5;
        System.out.println(kSum(nums1, k1));  // 输出: 2

        int[] nums2 = {1, -2, 3, 4, -10, 12};
        int k2 = 16;
        System.out.println(kSum(nums2, k2));  // 输出: 10
    }
}

```


时间复杂度：O(N log N + k log k)，其中 N 是数组的长度。排序数组的时间复杂度是 O(N log N)，使用最大堆进行 k 次插入和弹出的时间复杂度是 O(k log k)。

空间复杂度：O(N + k)，其中 N 是处理后的数组长度，k 是最大堆的大小。


## 5    Longest Palindromic Substring
```java
public class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }

        int maxLen = 1;  // 记录最长回文子串的长度
        int begin = 0;   // 记录最长回文子串的起始位置
        boolean[][] dp = new boolean[len][len];  
        // dp[i][j] 表示 s[i..j] 是否是回文子串

        // 初始化：所有长度为 1 的子串都是回文子串
        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }

        char[] charArray = s.toCharArray();

        // 枚举子串的长度 L
        for (int L = 2; L <= len; L++) {
            // 枚举左边界 i，右边界 j 由 L 和 i 确定
            for (int i = 0; i < len; i++) {
                int j = L + i - 1;
                if (j >= len) {
                    break;
                }

                if (charArray[i] != charArray[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                // 如果 dp[i][j] 为真，更新最大长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }

        return s.substring(begin, begin + maxLen);
    }
}

```
时间复杂度：O(n^2)，其中 n 是字符串的长度。主要是双重循环遍历所有子串。
空间复杂度：O(n^2)，用于存储动态规划数组 dp。


