# 剑指offer习题

## 1. 二维数组中的查找

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```c++
/* 方法一
 * 暴力法
 */
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        if (array.size() == 0) return false;
        int i, j = 0;
        for (i = 0; i < array.size(); i++) {
            for (j = 0; j <array[0].size(); j++)
                if (array[i][j] == target) return true;
        }
        return false;
    }
};

/* 方法二
 * 二维有序数组的二分法
 */
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int m = array.size();
        if (!m) return false;
        int n = array[0].size();
        if (!n) return false;
        int row = 0, col = n - 1;
        while (row < m && col >= 0) {
            if (array[row][col] == target) return true;
            else if (array[row][col] < target) row++;
            else if (array[row][col] > target) col--;
        }
        return false;
    }
};
```

## 2.替换空格

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

```c++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        if (!str || length <= 0) return;
        int num = 0;
        for (int i = 0; i <= length; i++) {
            if (str[i] == ' ')
                num++;
        }
        if (!num) return;
        int new_len = length + num * 2;
        for (int i = length; i >= 0; i--) {
            if (str[i] == ' ') {
                str[new_len--] = '0';
                str[new_len--] = '2';
                str[new_len--] = '%';
            } else {
                str[new_len--] = str[i];
            }
        }      
	}
};
```

## 3. 从头到尾打印链表

输入一个链表，按链表**从尾到头**的顺序返回一个ArrayList。

```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
/* 方法一，利用reverse输出 */
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        if (head == nullptr) return {};
        vector<int> ans;
        while (head) {
            ans.push_back(head->val);
            head = head->next;
        }
        std::reverse(ans.begin(), ans.end());
        return ans;
    }
};

/* 方法二：recursive */
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> ans;
        if (!head) return ans;
        ans = printListFromTailToHead(head->next);
        ans.push_back(head->val);
        return ans;
    }
};

/* 方法三： 利用栈的特性 */
/* 方法四： 反转链表 */
```

## 4.重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* build(vector<int> &pre, vector<int> &vin, int pre_left, int pre_right, int vin_left, int vin_right) {
        if (pre_left > pre_right || vin_left > vin_right) return nullptr;
        TreeNode *root = new TreeNode(pre[pre_left]);
        int root_index = vin_left;
        while (root_index <= vin_right && vin[root_index] != pre[pre_left])
            root_index++;
        root->left = build(pre, vin, pre_left + 1, pre_left + root_index - vin_left, vin_left, root_index - 1);
        root->right = build(pre, vin, pre_left + root_index - vin_left + 1, pre_right, root_index + 1, vin_right);
        return root;
    }
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        return build(pre, vin, 0, pre.size() - 1, 0, vin.size() - 1);
    }
};
```

## 5.用两个栈实现队列

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

```c++
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if (stack2.empty()) {
            while (!stack1.empty()) {
                stack2.push(stack1.top());
                stack1.pop();
            }
        }
        int node = stack2.top();
        stack2.pop();
        return node;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

## 6.旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

```c++
/* 二分查找
 * 非递减的数组，经过旋转后，很符合二分查找的使用条件。
 * 二分查找的目标不存在，可以考虑端点值，比如把右端点作为目标值。
 */
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if (rotateArray.size() == 0) return 0;
        int begin = 0, end = rotateArray.size() - 1;
        while (begin < end) {
            if (rotateArray[begin] < rotateArray[end])
                return rotateArray[begin];
            int mid = begin + (end - begin) / 2;
            if (rotateArray[mid] > rotateArray[end])
                begin = mid + 1;
            else if (rotateArray[mid] < rotateArray[end])
                end = mid;
            else
                end--;
        }
        return rotateArray[begin];
    }
};
```

## 7.斐波那契数列

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，第1项是1）。

n<=39

```c++
/* 方法一： recursive
 * O(2^n)
 */
class Solution {
public:
    int Fibonacci(int n) {
        if (n == 0 || n == 1) return n;
        return Fibonacci(n - 1) + Fibonacci(n - 2);
    }
};

/* 方法二： 优化思路，有重复计算，将已经计算的值存在数组中，避免重复计算 
 * O(n) O(n)
 */
 class Solution {
public:
    int helper(int n, vector<int> &dp) {
        if (dp[n] != -1) return dp[n];
        if (n == 0 || n == 1) {
            dp[n] = n;
            return n;
        }
        return dp[n] = helper(n - 1, dp) + helper(n - 2, dp);
    }
    int Fibonacci(int n) {
        vector<int> dp(40, -1);
        return helper(n, dp);
    }
};

/* 方法三： iterative
 * 动态规划的思想
 * O(n) O(n)
 */
class Solution {
public:
    int Fibonacci(int n) {
        vector<int> dp(n + 1, 0);
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};

/* 方法四： 优化，只用到了上两个值，不需要存储完整数组，只需要暂存2个值
 * O(n) O(1)
 */
class Solution {
public:
    int Fibonacci(int n) {
        if (n == 0 || n == 1) return n;
        int llast = 0, last = 1, curr = 0;
        for (int i = 2; i <= n; i++) {
            curr = last + llast;
            llast = last;
            last = curr;
        }
        return curr;
    }
};
```

## 8.跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

```c++
class Solution {
public:
    int jumpFloor(int number) {
        if (number <= 0) return 0;
        if (number == 1) return 1;
        if (number == 2) return 2;
        int a = 1, b = 2, c = 0;
        int i = 3;
        while (i++ <= number) {
            c = a + b;
            a = b;
            b = c;
        }
        
        return c;
    }
};
```

## 9.变态跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

```c++
class Solution {
public:
    int jumpFloorII(int number) {
        if (number == 0 || number == 1) return 1;
        int a = 1, b = 0;
        for (int i = 2; i <= number; i++) {
            b = a * 2;
            a = b;
        }
        return b;
    }
};
```

## 10.矩形覆盖

我们可以用2\*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2\*1的小矩形无重叠地覆盖一个2\*n的大矩形，总共有多少种方法？

比如n=3时，2\*3的矩形块有3种覆盖方法：

```c++
class Solution {
public:
    int rectCover(int number) {
        if (number == 0 || number == 1 || number == 2) return number;
        int a = 1, b = 2, c = 0;
        for (int i = 3; i <= number; i++) {
            c = a + b;
            a = b;
            b = c;
        }
        
        return c;
    }
};
```

## 11.二进制中1的个数

输入一个整数，输出该数32位二进制表示中1的个数。其中负数用补码表示。

```c++
class Solution {
public:
     int  NumberOf1(int n) {
         int base = 0x01;
         int ans = 0;
         for (int i = 0; i < 32; i++) {
             if (n & base) ans++;
             base <<= 1;
         }
         return ans;
     }
};
```

## 12.数值的整数次方

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

保证base和exponent不同时为0

```c++
class Solution {
public:
    double Power(double base, int exponent) {
        if (exponent == 0) return 1.0;
        if (exponent == 1) return base;
        int e = exponent > 0 ? exponent : -exponent;
        double ans = 1.0;
        for (int i = 1; i <= e; i++) {
            ans *= base;
        }
        return exponent > 0 ? ans : 1 / ans;
    }
};

/* 优化：十进制的快速幂
 * 利用偶次方e可以分解为两个e/2的乘积，同理奇数次可以提出一个base后分解。
 * 递归下去，每次规模减半，时间复杂度降为O(logn)，由于递归引入了空间复杂度O(logn)
 */
class Solution {
public:
    double q_power(double b, int n) {
        if (n == 0) return 1.0;
        double ret = q_power(b, n/2);
        if (n&1) { // 奇数
            return ret * ret * b;
        }
        else {
            return ret * ret;
        }
    }
    double Power(double b, int n) {
        if (n < 0) {
            b = 1 / b;
            n = -n;
        }
        return q_power(b, n);
    }
};

/* 继续优化： 二进制的快速幂
 * 将指数转换成二进制形式，此时只需要计算二进制位为1的幂次。
 * 也是STL中pow函数采用的方法。
 * 由于数的二进制位数为logn，所以时间复杂度为O(logn)。
 */
class Solution {
public:
 
    double Power(double b, int n) {
        if (n < 0) {
            b = 1 / b;
            n = -n;
        }
        double x = b; // 记录x^0, x^1, x^2 ...
        double ret = 1.0;
        while (n) {
            if (n&1) {
                ret *= x; // 二进制位数是1的，乘进答案。
            }
            x *= x;
            n >>= 1;
        }
        return ret;
    }
};
```

## 13.调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

```c++
/* 额外空间O(n) 
 * O(n),O(n)
 * 类似于选择排序
 */
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        vector<int> arr;
        for (const int v : array) {
            if (v&1) arr.push_back(v); // 奇数
        }
        for (const int v : array) {
            if (!(v&1)) arr.push_back(v); // 偶数
        }
        copy(arr.begin(), arr.end(), array.begin());
    }
};

/* 在原数组空间
 * O(n^2), O(1)
 * 类似于插入排序
 */
 class Solution {
 public:
  void reOrderArray(vector<int> &array) {
      int i = 0;
      for (int j=0; j<array.size(); ++j) {
          if (array[j]&1) {
              int tmp = array[j];
              for (int k=j-1; k>=i; --k) {
                  array[k+1] = array[k];
              }
              array[i++] = tmp;
          }
      }
  }
};
```

## 14.链表中倒数第k个结点

输入一个链表，输出该链表中倒数第k个结点。

```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if (!pListHead || k <= 0) return nullptr;
        ListNode *slow = pListHead, *fast = pListHead;
        for (int i = 1; i <= k; i++) {
            if (fast)
                fast = fast->next;
            else
                return nullptr;
        }
        while (fast) {
            slow = slow->next;
            fast = fast->next;
        }
        return slow;
    }
};
```

## 15.反转链表

输入一个链表，反转链表后，输出新链表的表头。

```c++
/* recursive */
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        if (!pHead || !pHead->next) return pHead;
        struct ListNode* p  = ReverseList(pHead->next);
        pHead->next->next = pHead;
        pHead->next = nullptr;
        
        return p;
    }
};

/* iterative */
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        if (!pHead) return nullptr;
        ListNode* prev = nullptr;
        ListNode* curr = pHead;
        ListNode* next = nullptr;
        while (curr) {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```

## 16.合并两个排序的链表

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

```c++
/* iterative */
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        ListNode *head = new ListNode(-1);
        ListNode *cur = head;
        while (pHead1 && pHead2) {
            if (pHead1->val <= pHead2->val) {
                cur->next = pHead1;
                pHead1 = pHead1->next;
            } else {
                cur->next = pHead2;
                pHead2 = pHead2->next;
            }
            cur = cur->next;
        }
        cur->next = pHead1 ? pHead1 : pHead2;
        return head->next;
    }
};

/* recursive */
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if (!pHead1) return pHead2;
        if (!pHead2) return pHead1;
        if (pHead1->val <= pHead2->val) {
            pHead1->next = Merge(pHead1->next, pHead2);
            return pHead1;
        } else {
            pHead2->next = Merge(pHead1, pHead2->next);
            return pHead2;
        }
    }
};
```

## 17.树的子结构

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    bool isSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if (!pRoot2) return true;
        if (!pRoot1 || pRoot1->val != pRoot2->val) return false;
        return isSubtree(pRoot1->left, pRoot2->left)
            && isSubtree(pRoot1->right, pRoot2->right);
    }

    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if (!pRoot1 || !pRoot2) return false;
        return isSubtree(pRoot1, pRoot2)
            || HasSubtree(pRoot1->left, pRoot2)
            || HasSubtree(pRoot1->right, pRoot2);
    }
};
```

## 18.二叉树的镜像

操作给定的二叉树，将其变换为源二叉树的镜像。

```c++
/* recursive */
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    TreeNode* dfs(TreeNode* root) {
        if (!root) return nullptr;
        TreeNode* lnode = dfs(root->left);
        TreeNode* rnode = dfs(root->right);
        root->left = rnode;
        root->right = lnode;
        return root;
    }
    void Mirror(TreeNode *pRoot) {
        if (!pRoot) return;
        dfs(pRoot);
    }
};

/* iterative */
class Solution {
public:
    void Mirror(TreeNode *pRoot) {
        if (!pRoot) return;
        queue<TreeNode*> q;
        q.push(pRoot);
        while (!q.empty()) {
            int n = q.size();
            while (n--) {
                TreeNode* node = q.front();
                q.pop();
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
                TreeNode* cur = node->left;
                node->left = node->right;
                node->right = cur;
            }
        }
    }
};
```

## 19.顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

```c++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> res;
        if (matrix.size() == 0 || matrix[0].size() == 0) return res;
        int left = 0, up = 0;
        int right = matrix[0].size() - 1;
        int down = matrix.size() - 1;
        int i = 0;
        while (down >= up && right >= left) {
            for (i = left; i <= right; i++) res.push_back(matrix[up][i]);
            up++;
            if (up > down) break;
            for (i = up; i <= down; i++) res.push_back(matrix[i][right]);
            right--;
            if (right < left) break;
            for (i = right; i >= left; i--) res.push_back(matrix[down][i]);
            down--;
            for (i = down; i >= up; i--) res.push_back(matrix[i][left]);
            left++;
        }
        return res;
    }
};
```

## 20.包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

```c++
class Solution {
public:
    stack<int> normal, minimal;

    void push(int value) {
        normal.push(value);
        if (minimal.empty()) {
            minimal.push(value);
        } else {
            if (value <= minimal.top()) {
                minimal.push(value);
            } else {
                minimal.push(minimal.top());
            }
        }
    }
    void pop() {
        normal.pop();
        minimal.pop();
    }
    int top() {
        return normal.top();
    }
    int min() {
        return minimal.top();
    }
};
```

## 21.栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

```c++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        stack<int> st;
        int i = 0, j = 0;
        for (i = 0; i <  pushV.size(); i++) {
            st.push(pushV[i]);
            while (!st.empty() && st.top() == popV[j]) {
                st.pop();
                j++;
            }
        }
        return st.empty();
    }
};
```

## 22.从上往下打印二叉树

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> res;
        if (!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* node = nullptr;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                node = q.front();
                q.pop();
                res.push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
        }
        return res;
    }
};
```

## 23.二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则返回true,否则返回false。假设输入的数组的任意两个数字都互不相同。

```c++
/* recursive */
class Solution {
public:
    bool helper(vector<int> seq, int start, int root) {
        if (start >= root) return true;
        int key = seq[root];
        int i = 0;
        for (i = start; i < root; i++) {
            if (seq[i] > key)
                break;
        }
        for (int j = i; j < root; j++) {
            if (seq[j] < key)
                return false;
        }
        return helper(seq, start, i - 1) && helper(seq, i, root - 1);
    }
    bool VerifySquenceOfBST(vector<int> sequence) {
        if (sequence.empty()) return false;
        return helper(sequence, 0, sequence.size() - 1);
    }
};

/* iterative */
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        if (sequence.empty()) return false;
        stack<int> roots;
        roots.push(INT_MIN);
        int max = INT_MAX;
        for (int i = sequence.size() - 1; i > -1; i--) {
            if (sequence[i] > max) return false;
            while (sequence[i] < roots.top()) {
                max = roots.top();
                roots.pop();
            }
            roots.push(sequence[i]);
        }
        return true;
    }
};
```

## 24.二叉树中和为某一值的路径

输入一颗二叉树的根节点和一个整数，按字典序打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

```c++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/

class Solution {
public:
    void backtrack(vector<vector<int>> &result, vector<int> &path, TreeNode* root, int expect) {
        path.push_back(root->val);
        if (expect == root->val && !root->left && !root->right) {
            result.push_back(path);
        }
        if (root->left) backtrack(result, path, root->left, expect - root->val);
        if (root->right) backtrack(result, path, root->right, expect - root->val);
        path.pop_back();
    }

    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        vector<vector<int>> result;
        vector<int> path;
        if (!root) return result;
        backtrack(result, path, root, expectNumber);
        return result;
    }
};
```

### 25.复杂链表的复制

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针random指向一个随机节点），请对此链表进行深拷贝，并返回拷贝后的头结点。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

```c++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if (!pHead) return nullptr;
        queue<RandomListNode*> q;
        unordered_map<RandomListNode*, RandomListNode*> visited;

        q.push(pHead);
        RandomListNode* target = new RandomListNode(pHead->label);
        RandomListNode* curr = pHead;
        visited[curr] = target;

        while (!q.empty()) {
            curr = q.front();
            q.pop();
            if (curr->next != nullptr) {
                if (visited.find(curr->next) == visited.end()) {
                    visited[curr->next] = new RandomListNode(curr->next->label);
                    q.push(curr->next);
                }
                visited[curr]->next = visited[curr->next];
            }
            if (curr->random != nullptr) {
                if (visited.find(curr->random) == visited.end()) {
                    visited[curr->random] = new RandomListNode(curr->random->label);
                    q.push(curr->random);
                }
                visited[curr]->random = visited[curr->random];
            }
        }
        return target;
    }
};
```

## 26.二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

```

```

