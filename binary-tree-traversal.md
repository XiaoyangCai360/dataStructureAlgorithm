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

具体算法：
* 判断二叉树是否为空，如果为空返回
* 初始化返回数组`res`
* 初始化空栈`stack`，把根节点`root`压入栈中
* 当栈`stack`不为空时：
  * 栈弹出当前栈顶元素`node`，并处理该元素，即`res`记录该节点的值
  * 如果`node`右节点不为空，访问右节点并入栈
  * 然后（注意顺序），如果`node`左节点不为空，访问左节点并入栈

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
* 后续遍历的迭代实现基本思路与前序遍历一致
* 后续遍历的顺序是**左右中**，前序遍历的顺序是**中左右**，具体更改如下：
  * 在原本前序遍历的基础上，先修改访问和处理节点的顺序，更改为**左子树**入栈，再**右子树**入栈，此时访问的顺序（即出栈的顺序）变为 **中右左**
  * 最后返回数组`res`时，反转数组，顺序变为 **左右中**

具体算法：
* 判断二叉树是否为空，如果为空返回
* 初始化返回数组`res`
* 初始化空栈`stack`，把根节点`root`压入栈中
* 当栈`stack`不为空时：
  * 处理栈顶元素，栈弹出当前栈顶元素`node`，`res`记录该节点的值
  * 如果`node`左节点不为空，访问左节点并入栈
  * 然后（注意顺序），如果`node`右节点不为空，访问右节点并入栈
* 此时返回数组`res`的顺序为**中右左**，最后反转返回数组`res`，顺序为**左右中**

##### 1.3.2.1 Python - 后序遍历的迭代实现
```Py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = list()

        # 空树
        if not root:
            return res
        
        # list as stack
        stack = [root]

        while stack: # 出栈顺序：中右左
            # 中
            node = stack.pop()
            res.append(node.val)

            # 左
            if node.left:
                stack.append(node.left)

            # 右
            if node.right:
                stack.append(node.right)
        
        # 反转res：左右中
        return res[::-1]
```

##### 1.3.2.2 Java - 后序遍历的迭代实现
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

        // 空树
        if (root == null) {
            return res;
        }

        // deque as stack
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);

        // 出栈顺序：中右左
        while (!stack.isEmpty()) {
            // 中
            TreeNode node = stack.pop();
            res.add(node.val);

            // 左
            if (node.left != null) {
                stack.push(node.left);
            } 
            
            // 右
            if (node.right != null) {
                stack.push(node.right);
            }
        }

        // 反转res：左右中
        Collections.reverse(res);
        return res;
    }
}
```

## 2 广度优先遍历 Breadth-first Search
> 即二叉树的**层序遍历 Level Order**。

二叉树的层序遍历规则为：
从二叉树的根节点`root`出发，先遍历完二叉树第`1`层的节点，然后遍历二叉树第`2`层的节点，依次向下遍历直到所有节点都被访问完。

例如：
<center> 5 </center>
<center>/&nbsp;\</center>
<center>4&nbsp;&nbsp;6</center>
<center>/&nbsp;\&nbsp;&nbsp;/&nbsp;\</center>
<center>1&nbsp;&nbsp;2&nbsp;&nbsp;7&nbsp;&nbsp;8</center>

层序遍历：`5 4 6 1 2 7 8`

### 2.1 层序遍历的递归实现
层序遍历的递归实现：
* 确定递归的 **参数和返回值**: 需要传入的参数为当前节点，因为要打印出层序遍历节点的值，所以参数需要一个**数组**来存放层序遍历节点的值，注意层序遍历中返回的结果是一个数组，数组中的元素也是数组，每一个子数组中存放每一层的节点值，即`List[List[int]]`；遍历中需要记录当前节点所在层数`level`；不需要返回值
```Py
# LC102-二叉树的层序遍历 
    def levelorder(cur: Optional[TreeNode], res: List[List[int]]，level：int) -> None:
```

* 确定递归的 **终止条件**：遍历过程中，如果当前节点为空，说明本层遍历结束，所以终止条件为当前节点为空就返回
```Py
        if cur == None: return
```
* 确定 **单层递归的逻辑**：层序遍历顺序为从上到下每一层从左到右遍历。单层递归中需要先判断当前节点是否是当前层的第一个，如果是需要在`res`中新增子数组；先访问当前根节点，然后访问左子树，再右子树，同时层数`level`加`1`
```Py
        # 判断每层开始节点
        if level = len(res):
            res.append([])

        # 根节点
        res[level].append(cur.val)

        # 左
        levelorder(cur, res, level + 1)
        
        # 右
        levelorder(cur, res, level + 1)
```

完整代码：

#### 2.1.1 Python - 层序遍历的递归实现
```Py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res = []

        def traversal(cur: Optional[TreeNode], levels: List[List[int]], level: int) -> None:
            # 终止条件
            if not cur:
                return
            
            # 判断每层第一个
            if len(levels) == level:
                levels.append([])
            
            # 根
            levels[level].append(cur.val)

            # 左
            traversal(cur.left, levels, level + 1)

            # 右
            traversal(cur.right, levels, level + 1)

        traversal(root, res, 0)
        return res
```

#### 2.1.2 Java - 层序遍历的递归实现
* Java中注意`ArrayList`常用接口，例如得到长度用`ArrayList.size()`（相当于`list.length`）；访问下标为`i`的元素用`ArrayList.get(i)`（相当于`list[i]`）
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        traversal(root, res, 0);
        return res;
    }

    public void traversal(TreeNode cur, List<List<Integer>> levels, int level) {
        // 终止条件
        if (cur == null) {
            return;
        }

        // 判断每层第一个
        if (levels.size() == level) {
            List<Integer> newLevel = new ArrayList<>();
            levels.add(newLevel);
        }

        // 根
        levels.get(level).add(cur.val);

        // 左
        traversal(cur.left, levels, level + 1);

        // 右
        traversal(cur.right, levels, level + 1);
    }
}
```

### 2.2 层序遍历的迭代实现
层序遍历的迭代实现需要用到**队列**，因为队列先进先出（FIFO）符合层序遍历一层一层遍历的逻辑。

具体算法：
* 判断二叉树是否为空，为空直接返回
* 定义记录遍历过节点值的数组`record`
* 定义辅助层序遍历的队列`queue`，同时令根节点`root`入队
* 当队列不为空时，做一下操作：
  * 得到当前队列的长度`size`
  * 定义单层节点的列表`levelitem`
  * 开始单层循环，直到原队列长度`size`为`0`（可以用`for`循环，即` for i in range(size)`），做一下操作：
    * 队列弹出队列头元素`node`，列表`levelitem`记录队头元素`node`的值
    * 队头元素的左节点先入队，即`queue.push(node.left)`
    * 队头元素的右节点后入队，即`queue.push(node.right)`
  * 把该层节点的列表`levelitem`加入到返回结果数组`record`中
* 返回数组`record`

#### 2.2.1 Python - 层序遍历的迭代实现
```Py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # 层序遍历迭代实现
        # 空树
        if not root:
            return

        # 返回列表
        res = []
        
        # deque as queue
        queue = deque()
        queue.append(root)

        # 终止条件：queue为空
        while queue:
            size = len(queue)
            # 单层列表
            levelitem = []

            for i in range(size):
                # 根
                cur = queue.popleft()
                levelitem.append(cur.val)
                
                # 左 
                if cur.left:
                    queue.append(cur.left)

                # 右
                if cur.right:
                    queue.append(cur.right)
            
            res.append(levelitem)

        return res
```

#### 2.2.2 Java - 层序遍历的迭代实现
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        // 层序遍历迭代实现
        List<List<Integer>> res = new ArrayList<>();

        // 空树
        if (root == null) {
            return res;
        }
        
        // deque as queue
        Deque<TreeNode> queue = new ArrayDeque<>();
        queue.add(root);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            // 单层节点列表
            List<Integer> levelitem = new ArrayList<>();

            for(int i = 0; i < size; i++) {
                // root
                TreeNode cur = queue.remove();
                levelitem.add(cur.val);

                // left
                if (cur.left != null) {
                    queue.add(cur.left);
                }

                // right
                if (cur.right != null) {
                    queue.add(cur.right);
                }
            }

            res.add(levelitem);
        }

        return res;
    }
}
```

