# 数据结构

## 基础结构

简单地进行归纳，数据结构中的基础组成可以分成顺序存储结构和链式存储结构。

### 顺序存储

顺序存储即在内存中申请一块连续空间，基本的表现形式为数组。其特点是：在内存中连续存储，可以通过索引进行随机访问。因此访问特定元素可以通过索引直接找到，时间复杂度为O(1)。但是由于连续存储，需要在使用前分配一整块内存，如果想要进行扩容，则需要重新分配一块更大的内存，扩容的时间复杂度为O(n)。同样的，对数组进行插入和删除操作，需要移动索引之后的所有元素，时间复杂度均为O(n)。

#### 数组的遍历

```c
void array[N] = {0};

void traversal(int *array)
{
  for (int i = 0; i < (sizeof(array)/ sizeof(*array)); i++) {
    array[i];
  }
}
```

###链式存储

链式存储即在内存中申请若干的离散空间，每个空间之间通过指针来指引，基本的表现形式为链表。其特点是：在内存中离散存储，无法通过索引直接找到某个元素，因此访问特定元素的时间复杂度为O(n)。由指针指向下一个元素的位置，因此在初始化时不需要分配所有的内存空间，可以直接在尾部进行扩容。在对链表元素进行插入和删除时，时间复杂度为O(1)。由于链式存储中除了数据本身还需要存储指针的信息，因此需要占用更多的空间。

#### 链表的遍历

```c
struct list {
	void value;
	struct list *next;
	struct list *pre;
}；

void traversal(struct list *head)
{
	struct list *p = head;
	while(p != NULL) {
		p->value;
		p = p->next;
	}
}
```

#### 常见问题

##### 单链表翻转

leetcode 206

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

//非递归：
struct ListNode* reverseList(struct ListNode* head){
    struct ListNode *pre = NULL;
    struct ListNode *curr = head;
    struct ListNode *next = NULL;

    while (curr != NULL) {
        next = curr->next;
        curr->next = pre;
        pre = curr;
        curr = next;
    }

    return pre;
}

//递归：
struct ListNode* reverseList(struct ListNode* head){
    if (head == NULL || head->next == NULL) {
        return head;
    }
    struct ListNode *p = reverseList(head->next);
    head->next->next = head;
    head->next = NULL;

    return p;
}

```

##### 判断链表有环

leetcode 141

```c
/* 快慢指针 */
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool hasCycle(struct ListNode *head) {
    if (head == NULL || head->next == NULL) {
        return false;
    }

    struct ListNode *fast = head;
    struct ListNode *slow = head;

    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
        if (fast == slow) {
            return true;
        }
    }

    return false;
}
```

##### 扩展：单链表找环入口  leetcode 142

```c
/* 由于选择的快指针的步长为2，慢指针步长为1，计算出当两个指针相遇时。
 * 创建一个新的步长为1的指针，当这两个慢指针相遇时恰好在环的入口 
 */
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *detectCycle(struct ListNode *head) {
    if (!head) {
        return NULL;
    }

    struct ListNode *slow = head;
    struct ListNode *fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (fast == slow) {
            struct ListNode *p = head;
            while (p != slow) {
                p = p->next;
                slow = slow->next;
            }

            return p;
        }
    }

    return NULL;
}
```

##### 相交链表 leetcode 160

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    if (!headA || !headB) {
        return NULL;
    }

    struct ListNode *a = headA;
    struct ListNode *b = headB;

    while (a != b) {
        a = a == NULL ? headB : a->next;
        b = b == NULL ? headA : b->next;
    }

    return a;
}
```

##### 单链表寻找中间节点 leetcode 876

tips: 中位数/中间节点位置的统一表示方法(n个数)：

数量为奇数时，中位数为最中间的那个数，位置为 ⌊n / 2⌋ + 1(向下取整)

数量为偶数时，中位数取中间两个数的均值，位置为 n / 2和 n / 2 + 1

统一后，可以表示为取位置为 ⌊(n -1 ) / 2 ⌋ + 1 和 ⌊n / 2⌋ + 1。由于一般使用int类型，结果可以省去取整符号。

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* middleNode(struct ListNode* head){
    struct ListNode *fast = head;
    struct ListNode *slow = head;

    if (!head) {
        return NULL;
    }

    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
    }

    return slow;
}
```

##### 合并有序链表 leetcode 21

```c
/* 非递归 */
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    struct ListNode *head = calloc(1, sizeof(*head));
    struct ListNode *p = head;
    struct ListNode *p1 = l1;
    struct ListNode *p2 = l2;

    while (p1 && p2) {
        if (p1->val <= p2->val) {
            p->next = p1;
            p1 = p1->next;
            p = p->next;
        } else {
            p->next = p2;
            p2 = p2->next;
            p = p->next;
        }
    }

    p->next = p1 ? p1 : p2;

    p = head->next;
    free(head);
    return p;
}

/* 递归 */
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    if (l1 == NULL) {
        return l2;
    } else if (l2 == NULL) {
        return l1;
    } else if (l1->val <= l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    } else {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }

    return NULL;
}
```



## 扩展结构

在顺序存储结构和链式的存储结构上，可以进一步扩展出更多数据结构。

### 树

#### 概念

**定义**：有限个节点组成的有层次关系的集合。

**结点**：每一个数据元素。

**空树**：集合为空，空树没有结点。

**度**：一个结点的子树数量。

**层次**：从树的根开始，根在第一层，根的孩子在第二层，以此类推。

**有序树和无序树**：如果树中的结点从左到右，有顺序则为有序树。

**森林**：互不相交的树组成的集合。

#### 二叉树

##### 概念

**定义**：有限个结点的集合，这些集合或者是空集或者是二叉树。度最大为2的有序树。

**性质**：

1. 第i层的结点最多为2^(i-1)
2. 高度为k的二叉树总结点最多为2^k -1
3. 任意的非空二叉树T，如果叶结点的数量为n0，度为2的结点数量为n2，则n0 = n2 + 1

##### **满二叉树**：

**定义**：深度为k且有2^k - 1个结点的二叉树。

**性质**：

1. 第i层的结点数量为2^ (i-1)
2. 高度为k的总结点数为2^k - 1。叶子结点的数量为2^(k-1)。
3. 具有n个结点的满二叉树的高度为log2(n+1)

##### **完全二叉树**：

**定义**：除去最后一层叶子结点后为满二叉树，且最后一层的结点从左到右分布。

**性质**：

1. n个结点的完全二叉树的高度为[log2(n)] + 1。
2. 对一棵完全二叉树按照层数从左到右依次编号，对于任意结点i，有：当i>1时，父结点为[i/2]；如果2i > n(n为总结点的个数)，则i没有左孩子(有的话左孩子应该为2i)；如果2i + 1 > n，则i没有右孩子(有的话应该为2i+1)。

##### 存储结构

顺序存储：完全二叉树自上而下，自左向右编号存储；非完全二叉树填充空结点，按照完全二叉树存储。

链式存储：链表结点中包含指向左孩子、右孩子的指针(也可增加对父结点的引用，三叉链表)。

##### 遍历

二叉树的遍历根据访问根结点的顺序分为前中后序遍历和层次遍历。

遍历的思路有三种，一是递归，核心代码很相似，区别只在于根据根序决定弹出值的时机。

```c++
/* 核心代码 */
/** Definition for a binary tree node
	* struct TreeNode {
	*		int val;
	*		struct TreeNode *left;
	*		struct TreeNode *right;
	*	}
	*/

class Solution {
public:
		void traversal(TreeNode *root, vector<int> &res) {
				if (root == nullptr) {
						return;
				}
				//res.push_back(root->val);					//调整该行的位置在相应的位置弹出值
				traversal(root->left, res);
				traversal(root->right, res);
		}
		
		vector<int> xorderTraversal(TreeNode *root) {
				vector<int> res;
				if (root == nullptr) {
						return {};
				}	
				traversal(root, res);
				return res;
		}
}
```

二是通过栈实现，根据栈先进后出的特点。

三是一种模板实现，思路类似于人工遍历二叉树顺序时，先浏览一遍二叉树，再根据顺序要求输出结点，而浏览的方法就是先递归地读完全部的左子树分支。核心代码相似，区别也只在于弹出值的时机。

**前序遍历**

前序遍历就是以根结点、左子树、右子树的顺序遍历二叉树

```c++
/* leetcode 144 */
/* recursive */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
void preorder(struct TreeNode *root, int *ret, int *returnSize)
{
    if (!root) {
        return;
    }
    ret[*returnSize] = root->val;
    (*returnSize)++;
    preorder(root->left, ret, returnSize);
    preorder(root->right, ret, returnSize);
}

int* preorderTraversal(struct TreeNode* root, int* returnSize){
    int *ret = calloc(100, sizeof(*ret));
    *returnSize = 0;

    if (!root) {
        return NULL;
    }
    preorder(root, ret, returnSize);
    return ret;
}
 
/* iterative */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (root == nullptr) {
            return {};
        }
        vector<int> res;
        stack<TreeNode *> s;
        TreeNode *node;
        s.push(root);
        while (!s.empty()) {
            node = s.top();
            res.push_back(node->val);
            s.pop();
            if (node->right) s.push(node->right);
            if (node->left) s.push(node->left);
        }
        return res;
    }
};

/* another iterative */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
            stack<TreeNode *> st;
            vector<int> res;
            TreeNode *cur = root;
            TreeNode *node = nullptr;
            while (cur != nullptr || !st.empty()) {
                while (cur != nullptr) {
                    st.push(cur);
                    res.push_back(cur->val);
                    cur = cur->left;
                }
                node = st.top();
                st.pop();
                cur = node->right;
            }
        return res; 
    }
};
```

**中序遍历**

```c++
/* leetcode 94 */
/* interative */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode *> st;
        TreeNode *cur = root;
        TreeNode *node = nullptr;
        while (cur || !st.empty()) {
            while (cur != nullptr) {
                st.push(cur);
                cur = cur->left;
            }
            node = st.top();
            st.pop();
            res.push_back(node->val);
            cur = node->right;
        }

        return res;
    }
};
```

**后序遍历**

```c++
/* leetcode 145 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
/*	先迭代访问完所有左边的分支，然后在结点处看是否需要访问右子树，
 *	然后对访问过的结点进行标记，方法为记录上一个弹出的结点
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode *> st;
        TreeNode *cur = root;
        TreeNode *last = nullptr;
        while (cur != nullptr || !st.empty()) {
            while (cur != nullptr) {
                st.push(cur);
                cur = cur->left;
            }
            cur = st.top();
            if (cur->right == nullptr || cur->right == last) {
                res.push_back(cur->val);
                st.pop();
                last = cur;
                cur = nullptr;
            } else {
                cur = cur->right;
            }
        }
        return res;
    }
};

/* 由于后序遍历的顺序为左-右-根的顺序，反过来就是一个先访问右结点的"前序"遍历，即根-右-左
 * 按照前序遍历的写法，在栈中先压入左儿子，然后利用reverse将弹出的后的结果集逆序输出
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        if (root == nullptr) {
        		return res;
        }
        stack<TreeNode *> st;
        TreeNode *cur = root;
        while (cur != nullptr || !st.empty()) {
            while (cur) {
            		res.push_back(cur->val);
                st.push(cur);
                cur = cur->right;
            }
            cur = st.top();
            st.pop();
            cur = cur->left;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

**层序遍历**

```
/* leetcode 102 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if (!root) {
            return ret;
        }

        queue<TreeNode *> q;
        TreeNode *node = nullptr;
        int level = 0;
        q.push(root);
        while (!q.empty()) {
            level = q.size();
            ret.push_back(vector<int> ());
            for (int i = 0; i < level; i++) {
                node = q.front();
                q.pop();
                ret.back().push_back(node->val);
                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
        }
        return ret;
    }
};
```

##### 例题

**二叉树的最大深度**

```c++
/* leetcode 104 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

**二叉树的路径和**

```c++
/* leetcode 112 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
/* recursive */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root)
            return false;
        if (!root->left && !root->right)
            return sum == root->val;
        return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);
    }
};

/* interative */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == nullptr) {
            return false;
        }

        queue<TreeNode *> q;
        queue<int> qval;
        q.push(root);
        qval.push(root->val);
        TreeNode *cur = nullptr;
        int val;
        while (!q.empty()) {
            cur = q.front();
            val = qval.front();
            q.pop();
            qval.pop();
            if (cur->left == nullptr && cur->right == nullptr) {
                if (val == sum)
                    return true;
                continue;
            }
            if (cur->left) {
                q.push(cur->left);
                qval.push(cur->left->val + val);
            }
            if (cur->right) {
                q.push(cur->right);
                qval.push(cur->right->val + val);
            }
        }
        return false;
    }
};
```

**填充每个节点的下一个右侧节点指针**

```c++
/* leetcode 116 */
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (root == nullptr) return nullptr;
        Node *leftmost = root;
        while (leftmost->left != nullptr) {
            Node *node = leftmost;
            while (node != nullptr) {
                node->left->next = node->right;
                if (node->next != nullptr) node->right->next = node->next->left;
                node = node->next;
            }
            leftmost = leftmost->left;
        }
        return root;
    }
};
```

**填充每个节点的下一个右侧节点指针 II**

```c++
/* leetcode 117 */
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (root == nullptr) return root;
        Node *tail = new Node();
        Node *node = root;
        Node *curr = nullptr;
        Node *next = nullptr;
        while (node) {
            curr = node;
            next = tail;
            while (curr) {
                if (curr->left) {
                    next->next = curr->left;
                    next = next->next;
                }
                if (curr->right) {
                    next->next = curr->right;
                    next = next->next;
                }
                curr = curr->next;
            }
            node = tail->next;
            if (next == tail) break;
        }
        delete(tail);
        return root;
    }
};
```

**二叉树的最近公共祖先**

```c++
/* leetcode 236 */
/* recursive */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* ans;
    bool dfs(TreeNode* root, TreeNode* p, TreeNode *q) {
        if (root == nullptr) return false;
        bool lson = dfs(root->left, p, q);
        bool rson = dfs(root->right, p, q);
        if ((lson && rson) || ((root->val == p->val || root->val == q->val) && (lson || rson)))
            ans = root;
        return lson || rson || (root->val == p->val || root->val == q->val);
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dfs(root, p, q);
        return ans;
    }
};
```



##### 从二叉序的遍历序列恢复二叉树

只有前序和后序序列，不能恢复唯一的二叉树。

反例：以[[1], [2], [3]]一棵简单的三层二叉树为例。前序遍历结果为[1, 2, 3]， 后序遍历结果为[3, 2, 1]。但是每一层的结点在左或者在右的遍历结果都是相同的，所以不能确定唯一的二叉树。

前序和后序遍历结果都可以确定根结点，结合中序遍历则可以区分左右子树的范围，没有中序遍历的情况下无法确定左右子树的边界。

**中序和后序恢复**

```c++
/* leetcode 106 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> in_map;
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.empty()) {
            return nullptr;
        }
        for (int i = 0; i < inorder.size(); i++) {
            in_map[inorder[i]] = i;
        }
        return build(inorder, postorder, 0, inorder.size() - 1, 0, postorder.size() - 1);
    }
    TreeNode *build(vector<int> &inorder, vector<int> &postorder, 
                    int in_left, int in_right, int post_left, int post_right) {
        if (in_left > in_right) return nullptr;
        TreeNode *node = new TreeNode(postorder[post_right]);
        int k = in_map[postorder[post_right]];
        node->left = build(inorder, postorder,
                        in_left, k - 1, post_left, post_left + k - in_left - 1);
        node->right = build(inorder, postorder,
                        k + 1, in_right, post_left + k - in_left, post_right - 1);
        return node;
    }
};
```

**前序和中序恢复二叉树**

```c++
/* leetcode 105 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> in_map;
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (inorder.empty()) return nullptr;
        for (int i = 0; i < inorder.size(); i++) in_map[inorder[i]] = i;
        return build(preorder, inorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
    }
    TreeNode *build(vector<int>& preorder, vector<int>& inorder,
                    int pl, int pr, int il, int ir) {
        if (pl > pr) return nullptr;
        TreeNode *root = new TreeNode(preorder[pl]);
        int k = in_map[preorder[pl]];
        root->left = build(preorder, inorder,
                pl + 1, pl - il + k, il, k - 1);
        root->right = build(preorder, inorder,
                pr - ir + k + 1, pr, k + 1, ir);

        return root;
    }
};
```

**二叉树的序列化**

```c++
/* leetcode 297 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:
    vector<string> split(string &data) {
        int start = 1;
        vector<string> res;
        while (true) {
            int end = data.find(',', start);
            if (end == string::npos) break;
            res.push_back(data.substr(start, end - start));
            start = end + 1;
        }
        return move(res);
    }

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string result = string();
        if (!root) return result;
        result.push_back('[');
        queue<TreeNode *> q;
        TreeNode *cur = root;
        q.push(root);
        int nonNull = 1;
        while (nonNull) {
            cur = q.front();
            q.pop();
            if (cur == nullptr) {
                result.append("null,");
            } else {
                result.append(to_string(cur->val));
                result.push_back(',');
                nonNull--;
                if (cur->left) {
                    q.push(cur->left);
                    nonNull++;
                } else {
                    q.push(nullptr);
                }
                if (cur->right) {
                    q.push(cur->right);
                    nonNull++;
                } else {
                    q.push(nullptr);
                }
            }
        }
        result[result.size() - 1] = ']';
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data.size() <= 2) return nullptr;
        data[data.size() - 1] = ',';
        vector<string> vals = split(data);
        if (vals[0] == "null") return nullptr;
        queue<TreeNode *> q;
        TreeNode *root = new TreeNode(stoi(vals[0]));
        TreeNode *cur = root;
        q.push(cur);
        int i = 0;
        int num = vals.size();
        while (!q.empty()) {
            cur = q.front();
            q.pop();
            if (++i >= num) break;
            if (vals[i] != "null") {
                cur->left = new TreeNode(stoi(vals[i]));
                q.push(cur->left);
            }
            if (++i >= num) break;
            if (vals[i] != "null") {
                cur->right = new TreeNode(stoi(vals[i]));
                q.push(cur->right);
            }
        }
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```



#### 堆

##### 定义

如果一棵完全二叉树的任意一个非终端结点的元素都不小于其左儿子结点和右儿子结点（如果有的话） 的元素，

则称此完全二叉树为最大堆。

同样，如果一棵完全二叉树的任意一个非终端结点的元素都不大于其左儿子结点和右儿子结点（如果 有的话）的元素，

则称此完全二叉树为最小堆。

也就是说，堆要满足两个条件：

1. 完全二叉树
2. 全部满足父节点小于等于（或大于等于）子节点。



#### 二叉搜索树

##### 定义

二叉搜索树(Binary Search Tree - BST)，又叫二叉查找树、二叉排序树。

二叉搜索树可以是空树或者满足：

1. 左子树不为空时，左子树的所有结点小于根节点。
2. 右子树不为空时，右子树的所有结点大于等于根节点。
3. 递归地定义：左右子树也必须是二叉搜索树

##### 查找

查找效率为O(logn)，最坏情况下蜕变为O(n)，此时树退化为链表。

##### 插入

向一个二叉搜索树b中插入一个节点s的算法，过程为：

1. 若b是空树，则将s所指节点作为根节点插入，否则：
2. 若s->data等于b的根节点的数据域之值，则返回，否则：
3. 若s->data小于b的根节点的数据域之值，则把s所指节点插入到左子树中，否则：
4. 把s所指节点插入到右子树中。（新插入节点总是叶子节点）

##### 删除

在二叉查找树删去一个结点，分三种情况讨论：

1. 若*p结点为叶子结点，即PL（左子树）和PR（右子树）均为空树。由于删去叶子结点不破坏整棵树的结构，

   则只需修改其双亲结点的指针即可。

2. 若\*p结点只有左子树PL或右子树PR，此时只要令PL或PR直接成为其双亲结点\*f的左子树（当p是左子树）或右子树（当*p是右子树）即可，作此修改也不破坏二叉查找树的特性。

3. 若\*p结点的左子树和右子树均不空。在删去\*p之后，为保持其它元素之间的相对位置不变，可按中序遍历保持有序进行调整，

   可以有两种做法：

   其一是令\*p的左子树为\*f的左/右（依\*p是\*f的左子树还是右子树而定）子树，

   \*s为\*p左子树的最右下的结点，而\*p的右子树为\*s的右子树；

   其二是令\*p的直接前驱（in-order predecessor）或直接后继（in-order successor）替代*p，

   然后再从二叉查找树中删去它的直接前驱（或直接后继）。

##### 时间复杂度

搜索、最大值、最小值、插入、删除等操作的时间复杂度为O(h)，h为树的高度。

#### 平衡二叉搜索树

##### 定义

平衡二叉搜索树(Balanced binary search trees, AVL)，又叫AVL树、平衡二叉查找树。

AVL树要满足两个条件：

1. 首先是一颗BST树
2. 任意结点的左右子树高度差不超过1，即平衡因子为范围为[-1,1]

##### 插入

在AVL树中插入结点，需要经过两步：

1. 插入结点
2. 旋转结点以达到平衡

##### 旋转

**最小失衡子树**：在新插入的结点向上查找，以第一个平衡因子的绝对值超过 1 的结点为根的子树称为最小不平衡子树。

也就是说，一棵失衡的树，是有可能有多棵子树同时失衡的。

而这个时候，我们只要调整最小的不平衡子树，就能够将不平衡的树调整为平衡的树。

插入结点后可能会导致失衡，此时共有4种情况，LL，RR，LR和RL。

**LL**

左子树的左结点失衡。

右旋

1. 节点的左孩子代表此节点
2. 节点的左孩子的右子树变为节点的左子树
3. 将此节点作为左孩子节点的右子树。

**RR**

右子树的右结点失衡。

左旋

1. 结点的右孩子代表此结点
2. 结点的右孩子的左子树变为结点的右子树
3. 将此结点作为右孩子的左子树。

**LR**

左子树的右结点失衡。

先左旋将失衡结点变成LL型。

再右旋将树平衡。

**RL**

右子树的左结点失衡。

先右旋将失衡结点变成RR型。

再左旋将树平衡。

##### 删除

如果是叶子结点，或者只有一个子树，直接删除后进行平衡。

如果有两个子树，此时的删除操作：

1. 取深度更大的一边子树
2. 用该侧子树中的最大结点(也就是右子树最右边的结点)或者最小结点(也就是左子树最左边的结点)代替该结点
3. 进行平衡。



#### B树

B树(B-tree)是多叉树，又叫平衡多路查找树。

##### 定义

1. 排序方式：所有结点从左到右依次递增，关键字的左子树中的所有关键字都小于该关键字
2. 子节点数：非叶结点的子结点数范围是2 <= n <= M(M是B树的阶数，且M >= 2)
3. 关键字数：根结点的关键字数范围是：1 <= n <= M - 1。非根节点的关键字数范围是：m / 2 <= n <= M - 1(m / 2向下取整)
4. 所有叶子节点均在同一层、叶子节点除了包含了关键字和关键字记录的指针外也有指向其子节点的指针

##### 结点特性

结点中存储的数据为关键字、关键字指向数据的指针和指向子结点的指针。

结点定义示例：

```c++
struct BTNode {
		int keyNum;				/* 关键数数量 */
		BTNode *parent;		/* 指向父结点 */
		BTNode **ptr;			/* 子节点的向量 */
		KeyTYpe *key;			/* 关键字向量，指向关键字指引的数据 */
}
```

##### 插入

插入时有两种情况。

判断当前结点key的个数是否小于等于m-1，如果满足，直接插入即可；

如果不满足，将节点的中间的key将这个节点分为左右两部分，中间的节点放到父节点中即可。

##### 删除

删除时：

删除叶子节点的元素，如果删除之后，节点数还是大于m/2，直接删除即可。

删除叶子节点，如果删除元素后元素个数少于（m/2），并且它的兄弟节点的元素大于（m/2），也就是说兄弟节点的元素比最少值m/2还多，将先将父节点的元素移到该节点，然后将兄弟节点的元素再移动到父节点

删除叶子节点，删除后不满足要求，所以，我们需要考虑向兄弟节点借元素，但是，兄弟节点也没有多的节点（2个），借不了，怎么办呢？如果遇到这种情况，首先，还是将先将父节点的元素移到该节点，然后，将当前节点及它的兄弟节点中的key合并，形成一个新的节点

对于非叶子节点的删除，我们需要用后继key（元素）覆盖要删除的key，然后在后继key所在的子支中删除该后继key。



#### B+树

B+树是B树的变体，具有更接近二分查找的查找速度，查找效率更高。

##### 规则

1. 非叶子结点不保存关键字记录，只进行数据索引。B+树每个非叶子节点所能保存的关键字大大增加；
2. B+树叶子节点保存了父节点的所有关键字记录的指针，所有数据地址必须要到叶子节点才能获取到。所以每次数据查询的次数都一样；
3. B+树叶子节点的关键字从小到大有序排列，左边结尾数据都会保存右边节点开始数据的指针。
4. 非叶子节点的子节点数=关键字数

##### 与B树相比的特点

1. B+树的层级更少：相较于B树B+每个非叶子节点存储的关键字数更多，树的层级更少所以查询数据更快；
2. B+树查询速度更稳定：B+所有关键字数据地址都存在叶子节点上，所以每次查找的次数都相同所以查询速度要比B树更稳定;
3. B+树天然具备排序功能：B+树所有的叶子节点数据构成了一个有序链表，在查询大小区间的数据时候更方便，数据紧密性很高，缓存的命中率也会比B树高。
4. B+树全节点遍历更快：B+树遍历整棵树只需要遍历所有的叶子节点即可，，而不需要像B树一样需要对每一层进行遍历，这有利于数据库做全表扫描。
5. B树较之B+树的优点：如果经常访问的数据离根节点很近，而B树的非叶子节点本身存有关键字其数据的地址，所以这种数据检索的时候会要比B+树快。

##### 分裂

当一个结点满时，分配一个新的结点，并将原结点中1/2的数据复制到新结点，最后在父结点中增加新结点的指针；

B+树的分裂只影响原结点和父结点，而不会影响兄弟结点，所以它不需要指向兄弟的指针

##### 应用

数据库索引



#### B*树

##### 定义

B\*-tree是B+-tree的变体，在B+树的基础上的基础上，修改了定义：

1. B*树中非根和非叶子结点再增加指向兄弟的指针；
2. B\*树定义了非叶子结点关键字个数至少为(2/3)\*M，即块的最低使用率为2/3（代替B+树的1/2)

##### 分裂

当一个结点满时，

如果它的下一个兄弟结点未满，那么将一部分数据移到兄弟结点中，再在原结点插入关键字，最后修改父结点中兄弟结点的关键字

（因为兄弟结点的关键字范围改变了）；

如果兄弟也满了，则在原结点与兄弟结点之间增加新结点，并各复制1/3的数据到新结点，最后在父结点增加新结点的指针。

##### 与B+树比较

B*树分配新结点的概率比B+树要低，空间使用率更高。



#### 红黑树

##### 定义

红黑树是一种自平衡二叉查找树，其满足以下特性：

1. 节点是红色或黑色。
2. 根是黑色。
3. 所有叶子都是黑色（叶子是NIL节点）。
4. 每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）
5. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

