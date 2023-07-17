# 二叉树遍历
二叉树遍历主要有两种方式：「深度优先遍历」和「广度优先遍历」

## 1 深度优先遍历 Depth-first Search
从二叉树的根节点`root`出发，先遍历完根节点的左子树所有节点，然后回到根节点继续遍历根节点的右子树所有节点。整个过程反复进行直到所有节点都被访问完。

深度优先遍历（DFS）可以分为「前序遍历」（**Preorder**）、「中序遍历」（**Inorder**）和「后序遍历」（**Postorder**）。

* 前序遍历：「**中**」 -> **左** -> **右**
* 中序遍历：**左** -> 「**中**」 -> **右** 
* 后序遍历：**左** -> **右** -> 「**中**」 

例如：
<center> 5 </center>
<center>/&nbsp;\</center>
<center>4&nbsp;&nbsp;6</center>
<center>/&nbsp;\&nbsp;&nbsp;/&nbsp;\</center>
<center>1&nbsp;&nbsp;2&nbsp;&nbsp;7&nbsp;&nbsp;8</center>


前序遍历：`5 4 1 2 6 7 8`
中序遍历：`1 4 2 5 7 6 8`
后序遍历：`1 2 4 7 8 6 5`

### [1.1 前序遍历 Preorder Traversal](/leetcode/0144_二叉树的前序遍历.md)
前续遍历规则：
* 如果树为空，则返回
* 如果树不为空，则：
  * 访问**根节点**
  * 按照前续遍历方式遍历根节点的**左子树**
  * 按照前续遍历方式遍历根节点的**右子树**

所以前序遍历顺序为：**中** -> **左** -> **右**

#### 1.1.1 前序遍历的递归实现
> 递归写法的三要素：
> 1. 确定递归的 **参数和返回值**
> 2. 确定递归的 **终止条件**
> 3. 确定 **单层递归的逻辑**

前序遍历的递归实现：
* 确定递归的 **参数和返回值**: 需要传入的参数为当前节点，因为要打印出前序遍历节点的值，所以参数需要一个**数组**来存放前序遍历节点的值，不需要返回值
```Py
# LC144-二叉树的前序遍历 
    def preorder(cur: Optional[TreeNode], res: List[int]) -> None:
```

* 确定递归的 **终止条件**：遍历过程中，如果当前节点为空，说明本层遍历结束，所以终止条件为当前节点为空就返回
```Py
        if cur == None: return
```
* 确定 **单层递归的逻辑**：前序遍历顺序为中左右，所以单层递归时，要先取中节点的值，然后左，然后右
```Py
        res.append(cur.val) # 中
        preorderTraversal(cur.left, res) # 左
        preorderTraversal(cur.right, res) # 右
```

完整代码：
##### 1.1.1.1 Python - 前序遍历的递归实现
```Py
# 前序遍历-递归-LC144_二叉树的前序遍历
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 前序遍历的递归实现
        res = list()

        def preorder(cur: TreeNode, vec: List[int]) -> None:
            # 终止条件
            if not cur: return

            vec.append(cur.val) # 中
            preorder(cur.left, vec) # 左
            preorder(cur.right, vec) # 右

        preorder(root, res)
        return res
```

##### 1.1.1.2 Java - 前序遍历的递归实现

```Java
// 前序遍历-递归-LC144_二叉树的前序遍历
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
    public List<Integer> preorderTraversal(TreeNode root) {
        // 前序遍历的递归实现
        List<Integer> result = new LinkedList<>();
        preorder(root, result);
        return result;
    }

    public void preorder(TreeNode cur, List<Integer> temp) {
        // 终止条件
        if (cur == null) {
            return;
        }

        temp.add(cur.val); // 中  
        preorder(cur.left, temp); // 左
        preorder(cur.right, temp); // 右
    }
}
```

#### 1.1.2 前序遍历的迭代实现
> 迭代实现的基本思路：
> * 使用**递归**就是每一次递归调用都把函数的局部变量、参数、返回值压入栈中，然后等递归返回的时候，从栈顶弹出上一层的各项参数，返回上一层。
> * 所以迭代实现递归算法时需要用**栈**。

前序遍历的迭代实现：
每次遍历时，处理完中间节点后，先把 **右节点** 压入栈中，然后再把 **左节点** 压入栈中，这样出栈的时候是 **中左右** 的顺序。

注意：中节点**不**入栈

##### 1.1.2.1 Python - 前序遍历的迭代实现
```Py
# 前序遍历-递归-LC144_二叉树的前序遍历
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 前序遍历的迭代实现
        
        # 返回数组 
        res = []

        # 空树
        if not root: 
            return res
        
        # list as stack
        # root只在第一次入栈，之后迭代过程中，root不入栈
        stack = [root]
        
        # 终止条件为stack为空，说明遍历完树
        while stack: # 相当于 len(stack) != 0
            node = stack.pop()          # 弹出根节点
            res.append(node.val)        # 访问根节点

            if node.right:
                stack.append(node.right) # 右子树先入栈
            if node.left:
                stack.append(node.left)  # 左子树后入栈

        return res 
```

##### 1.1.2.2 Java - 前序遍历的迭代实现
```Java
// 前序遍历-递归-LC144_二叉树的前序遍历
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
    public List<Integer> preorderTraversal(TreeNode root) {
        // 前序遍历的迭代实现
        List<Integer> result = new LinkedList<>();

        // 空树 
        if (root == null) {
            return result;
        }

        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            result.add(node.val);

            // 先右入栈
            if (node.right != null) {
                stack.push(node.right);
            }

            // 后左入栈
            if (node.left != null) {
                stack.push(node.left);
            }
        }

        return result;
    }
}
```

### [1.2 中序遍历 Inorder Traversal](/leetcode/0094_二叉树的中序遍历.md)
中续遍历规则：
* 如果树为空，则返回
* 如果树不为空，则：
  * 按照中续遍历方式遍历根节点的**左子树**   
  * 访问**根节点**
  * 按照中续遍历方式遍历根节点的**右子树**

所以中序遍历顺序为：**左** -> **中** -> **右**

#### 1.2.1 中序遍历的递归实现
中序遍历递归的基本思路与前序遍历递归一致。

中序遍历的递归实现：
* 确定递归的 **参数和返回值**: 需要传入的参数为当前节点，因为要打印出中序遍历节点的值，所以参数需要一个**数组**来存放前序遍历节点的值，不需要返回值
```Py
# LC094-二叉树的中序遍历 
    def inorder(cur: Optional[TreeNode], res: List[int]) -> None:
```

* 确定递归的 **终止条件**：遍历过程中，如果当前节点为空，说明本层遍历结束，所以终止条件为当前节点为空就返回
```Py
        if cur == None: return
```
* 确定 **单层递归的逻辑**：中序遍历顺序为左中右，所以单层递归时，要先取根节点的左子树的值，然后根节点，最后右子树
```Py
        inorderTraversal(cur.left, res) # 左
        res.append(cur.val) # 中
        inorderTraversal(cur.right, res) # 右
```

完整代码：
##### 1.2.1.1 Python - 中序遍历的递归实现
```Py
# 中序遍历-递归-LC094_二叉树的中序遍历
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        # 前序遍历的递归实现
        res = list()

        def inorder(cur: TreeNode, vec: List[int]) -> None:
            # 终止条件
            if not cur: return

            inorder(cur.left, vec) # 左
            vec.append(cur.val) # 中
            inorder(cur.right, vec) # 右

        inorder(root, res)
        return res
```

##### 1.2.1.2 Java - 中序遍历的递归实现
```Java
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode cur, List<Integer> temp) {
        // 终止条件
        if (cur == null) {
            return;
        }

        inorder(cur.left, temp);    // 左
        temp.add(cur.val);          // 中 
        inorder(cur.right, temp);   // 右
    }
}
```
#### 1.2.2 中序遍历的迭代实现
* 中序遍历的迭代实现思路与前序遍历不同
* 前序遍历迭代中一共有两个步骤：
  * 访问：从根节点`root`开始，不断向下遍历，将遍历到的元素放入栈`stack`中
  * 处理：栈`stack`中弹出栈顶节点，记录栈顶节点的值
* 前序遍历的顺序是**中左右**，访问的节点和处理的节点是同一个节点
* 中序遍历的顺序是**左中右**，从根节点`root`开始，一层层往下遍历直到树的最底部，然后才开始处理节点；所以在中序遍历中，需要保证在左子树访问完之前，当前的元素**不能**出栈


具体算法：
* 判断二叉树是否为空，如果为空返回
* 初始化空栈`stack`，初始化返回数组`res`
* 当前节点或栈不为空时：
  * 如果当前节点不为空，循环遍历当前节点的**左子树**，并将当前子树的根节点加入栈中
  * 如果当前节点为空，说明已经遍历到子树的左边最底部，栈`stack`弹出栈顶元素`node`，并记录该元素的值；然后将访问当前栈顶元素的右子树
* 注意循环条件为当前节点`cur`不为空或者栈`stack`不为空，即`while cur or stack`
  * 如果当前节点`cur`不为空，但是栈`stack`为空，说明当前节点`cur`是一个当前子树的根节点，还需要继续向下**访问**（继续向下遍历）
  * 如果当前节点`cur`为空，但是栈`stack`不为空，说明已经遍历到当前子树的最底层，需要开始**处理**节点（即栈弹出栈顶元素并记录）
  * 只有当前节点`cur`为空和栈`stack`也为空时，说明整棵树都遍历到，遍历结束

##### 1.2.2.1 Python - 中序遍历的迭代实现
```Py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = list()

        # 空树
        if not root:
            return res

        # list as stack
        stack = list()

        cur = root
        while cur or stack:
            if cur:
                # 从根节点依次向下访问
                stack.append(cur)
                cur = cur.left
            else:
                # not cur: 说明遍历到子树的最左节点
                # 处理当前节点并访问当前节点的右子树
                cur = stack.pop()
                res.append(cur.val)
                cur = cur.right
        
        return res
```

##### 1.2.2.2 Java - 中序遍历的迭代实现
```Java
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();

        // 空树
        if (root == null) {
            return res;
        }

        // deque as stack
        Deque<TreeNode> stack = new ArrayDeque<>();

        TreeNode cur = root;
        
        // 当前节点不为空或栈不为空 
        while (cur != null || !stack.isEmpty()) {
            if (cur != null) {
                // 继续向下访问
                stack.push(cur);
                cur = cur.left;
            } else {
                // 处理栈顶节点，并访问栈顶节点右子树
                cur = stack.pop();
                res.add(cur.val);
                cur = cur.right;
            }
        }

        return res;
    }
}
```

### [1.3 后序遍历 Postorder Traversal](/leetcode/0145_二叉树的后序遍历.md)
后序遍历规则：
* 如果树为空，则返回
* 如果树不为空，则：
  * 按照后序遍历方式遍历根节点的**左子树**  
  * 按照后序遍历方式遍历根节点的**右子树** 
  * 访问**根节点**

所以后序遍历顺序为：**左** -> **右** -> **中**

#### 1.3.1 后序遍历的递归实现
后序遍历递归的基本思路与前序遍历递归、中序遍历递归一致。

后序遍历的递归实现：
* 确定递归的 **参数和返回值**: 需要传入的参数为当前节点，因为要打印出后序遍历节点的值，所以参数需要一个**数组**来存放前序遍历节点的值，不需要返回值
```Py
# LC145-二叉树的后序遍历 
    def postorder(cur: Optional[TreeNode], res: List[int]) -> None:
```

* 确定递归的 **终止条件**：遍历过程中，如果当前节点为空，说明本层遍历结束，所以终止条件为当前节点为空就返回
```Py
        if cur == None: return
```
* 确定 **单层递归的逻辑**：后序遍历顺序为左右中，所以单层递归时，要先取根节点的左子树的值，然后右子树的值，最后根节点
```Py
        postorderTraversal(cur.left, res)   # 左
        postorderTraversal(cur.right, res)  # 右
        res.append(cur.val)                 # 中
```

完整代码：
##### 1.3.1.1 Python - 后序遍历的递归实现
```Py
# 后序遍历-递归-LC145_二叉树的后序遍历
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        # 后序遍历的递归实现
        res = list()

        def postorder(cur: TreeNode, vec: List[int]) -> None:
            # 终止条件
            if not cur: return

            postorder(cur.left, vec)    # 左
            postorder(cur.right, vec)   # 右
            vec.append(cur.val)         # 中

        postorder(root, res)
        return res
```

##### 1.3.1.2 Java - 后序遍历的递归实现
```Java
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
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        postorder(root, res);
        return res;
    }

    public void postorder(TreeNode cur, List<Integer> temp) {
        // 终止条件
        if (cur == null) {
            return;
        }

        postorder(cur.left, temp);    // 左
        postorder(cur.right, temp);   // 右
        temp.add(cur.val);            // 中 
    }
}
```

#### 1.3.2 后序遍历的迭代实现