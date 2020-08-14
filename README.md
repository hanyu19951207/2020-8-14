# 2020-8-14
24. 两两交换链表中的节点
    给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
    你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
    示例:
    给定 1->2->3->4, 你应该返回 2->1->4->3.
思路：
    从链表的头节点 head 开始递归。
    每次递归都负责交换一对节点。由 firstNode 和 secondNode 表示要交换的两个节点。
    下一次递归则是传递的是下一对需要交换的节点。若链表中还有节点，则继续递归。
    交换了两个节点以后，返回 secondNode，因为它是交换后的新头。
    在所有节点交换完成以后，我们返回交换后的头，实际上是原始链表的第二个节点。
题解：
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode nextPair = swapPairs(head.next.next);
        ListNode second = head.next;
        head.next = nextPair;
        second.next = head;
        return second;
    }
}
34. 在排序数组中查找元素的第一个和最后一个位置
    给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
    你的算法时间复杂度必须是 O(log n) 级别。
    如果数组中不存在目标值，返回 [-1, -1]。
    示例 1:
    输入: nums = [5,7,7,8,8,10], target = 8
    输出: [3,4]
思路：
    首先，我们对 nums 数组从左到右做线性遍历，当遇到 target 时中止。如果我们没有中止过，那么 target 不存在，我们可以返回“错误代码” [-1, -1] 。
    如果我们找到了有效的左端点坐标，我们可以坐第二遍线性扫描，但这次从右往左进行。这一次，第一个遇到的 target 将是最右边的一个（因为最左边的一个存在，所以一定会有一个最右边的 target）。
    我们接下来只需要返回这两个坐标。
题解：
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int [] targetRange = {-1 , -1};
        for(int i = 0;i < nums.length;i++){
            if(nums[i] == target){
                targetRange[0] = i;
                break;
            }
        }
        if(targetRange[0] == -1)
            return targetRange;
        for(int i = nums.length - 1;i >= 0;i--){
            if(nums[i] == target){
                targetRange[1] = i;
                break;
            }
        }
        return targetRange;
    }
}
520. 检测大写字母
    给定一个单词，你需要判断单词的大写使用是否正确。
    我们定义，在以下情况时，单词的大写用法是正确的：
        全部字母都是大写，比如"USA"。
        单词中所有字母都不是大写，比如"leetcode"。
        如果单词不只含有一个字母，只有首字母大写， 比如 "Google"。
    否则，我们定义这个单词没有正确使用大写字母。
    示例 1:
    输入: "USA"
    输出: True
    示例 2:
    输入: "FlaG"
    输出: False
思路：
    根据规则两个计数器统计大写字母和小写字母的数量，然后跟字符串的长度做对比即可
题解：
class Solution {
    public boolean detectCapitalUse(String word) {
        int bigcount = 0;
        int smallcount = 0;
        for(int i = 0;i < word.length();i++){
            if(word.charAt(i) >= 'A' && word.charAt(i) <= 'Z')
                bigcount++;
            else if(word.charAt(i) >= 'a' && word.charAt(i) <= 'z')
                smallcount++;
            if(bigcount == word.length() || smallcount == word.length())
                return true;
            if((word.charAt(0) >= 'A' && word.charAt(0) <= 'Z') && (smallcount == word.length() - 1))
                return true;
        }
        return false;
    }
}
521. 最长特殊序列 Ⅰ
    给你两个字符串，请你从这两个字符串中找出最长的特殊序列。
    「最长特殊序列」定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。
    子序列 可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。
    输入为两个字符串，输出最长特殊序列的长度。如果不存在，则返回 -1。
    示例 1：
    输入: "aba", "cdc"
    输出: 3
    解释: 最长特殊序列可为 "aba" (或 "cdc")，两者均为自身的子序列且不是对方的子序列。
思路：
字符串 aaa 和 bbb 共有 3 种情况：
    a=ba=ba=b。如果两个字符串相同，则没有特殊子序列，返回 -1。
    length(a)=length(b) 且 a≠ba。例如：abc 和 abd。这种情况下，一个字符串一定不会是另外一个字符串的子序列，因此可以将任意一个字符串看作是特殊子序列，返回 length(a) 或 length(b)。
    length(a)≠length(b)l。例如：abcd 和 abc。这种情况下，长的字符串一定不会是短字符串的子序列，因此可以将长字符串看作是特殊子序列，返回 max(length(a),length(b))。
题解：
public class Solution {
    public int findLUSlength(String a, String b) {
        if (a.equals(b))
            return -1;
        return Math.max(a.length(), b.length());
    }
}
530. 二叉搜索树的最小绝对差
    给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。
    示例：
    输入：
       1
        \
         3
        /
       2
    输出：
    1
    解释：
    最小绝对差为 1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
思路：
    BST中序遍历是升序，所以遍历时求相邻两个节点之间的最小绝对差值即可
题解：
class Solution {
    TreeNode pre = null;
    int res  = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        if(root == null)
            return 0;
        helper(root);
        return res;
    }
    public void helper(TreeNode root){
        if(root == null)
            return;
        helper(root.left);
        if(pre != null){
            res = Math.min(res, Math.abs(root.val - pre.val));
        }
        pre = root;
        helper(root.right);
    }
}
