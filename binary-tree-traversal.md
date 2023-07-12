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

### 1.1 前序遍历 Preorder Traversal
前续遍历规则：
* 如果树为空，则返回
* 如果树不为空，则：
  * 访问**根节点**
  * 按照前续遍历方式遍历根节点的**左子树**
  * 按照前续遍历方式遍历根节点的**右子树**

所以前序遍历顺序为：**中** -> **左** -> **右**

#### 1.1.1 前序遍历的递归实现
递归写法的三要素：
* 确定递归的 **参数和返回值**
* 确定递归的 **终止条件**
* 确定 **单层递归的逻辑**

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
Python - 前序遍历
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
        res = List()

        def preorder(cur: TreeNode, vec: List[int]) -> None:
            # 终止条件
            if not cur: return

            vec.append(cur.val) # 中
            preorder(cur.left, vec) # 左
            preorder(cur.right, vec) # 右

        preorder(root, res)
        return res
```