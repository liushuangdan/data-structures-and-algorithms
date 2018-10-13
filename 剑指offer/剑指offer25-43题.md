树的知识点：

##### 什么叫做树？

**树状图**是一种[数据结构](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1450)，它是由n（n>=0）个有限结点组成一个具有层次关系的[集合](https://baike.baidu.com/item/%E9%9B%86%E5%90%88)。把它叫做“树”是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。它具有以下的特点：

每个结点有零个或多个子结点；没有父结点的结点称为根结点；每一个非根结点有且只有一个父结点；除了根结点外，每个子结点可以分为多个不相交的子树；

叶节点没有子节点，根节点没有父节点。

##### 什么是二叉树？

每个节点最多含有两个子树的树称为二叉树。下图就是一个二叉树。

在计算机科学中，二叉树是每个结点最多有两个子树的树结构。通常子树被称作“左子树”（left subtree）和“右子树”（right subtree）。二叉树常被用于实现二叉查找树和二叉堆。

一棵深度为k，且有2^k-1个节点的二叉树，称为满二叉树。这种树的特点是每一层上的节点数都是最大节点数。而在一棵二叉树中，除最后一层外，若其余层都是满的，并且最后一层或者是满的，或者是在右边缺少连续若干节点，则此二叉树为完全二叉树。具有n个节点的完全二叉树的深度为floor(log2n)+1。深度为k的完全二叉树，至少有2k-1个节点，至多有2k-1个节点



![](images\二叉树.png)

##### 二叉树的遍历：

遍历是对树的一种最基本的运算，所谓遍历二叉树，就是按一定的规则和顺序走遍二叉树的所有结点，使每一个结点都被访问一次，而且只被访问一次。由于二叉树是非线性结构，因此，[树的遍历](https://baike.baidu.com/item/%E6%A0%91%E7%9A%84%E9%81%8D%E5%8E%86)实质上是将二叉树的各个结点转换成为一个线性序列来表示。

设L、D、R分别表示遍历左子树、访问根结点和遍历右子树， 则对一棵二叉树的遍历有三种情况：DLR（称为先根次序遍历），LDR（称为中根次序遍历），LRD （称为后根次序遍历）。

```python
class treeNode(object):
    def __init__(self,x):
        self.val = x
        self.left = None
        self.right = None

#1. 深度优先
#2. 广度优先
#对于深度优先来说：
"""

1 先序遍历  先打印根 1,2,4,5,3,6,8,7
2 中序遍历  先打印左侧的叶子节点 4，再输出 中节点  2 ；  4 2 5 1 6 8 3 7 
3 先序遍历  输出顺序  4 5 2 8 6 7 3 1

注意：  先序   中序 后序  都是对应于根节点来说的，左右节点都是先左后右

"""
#递归
#前序遍历
def preOrderRecusive(root):
    if root == None:
        return None

    print(root.val)
    preOrderRecusive(root.left)
    preOrderRecusive(root.right)
#中序遍历
def midOrderRecusive(root):
    if root == None:
        return None
    midOrderRecusive(root.left)
    print(root.val)
    midOrderRecusive(root.right)

#后序遍历
def laterOrderRecusive(root):
    if root == None:
        return None
    laterOrderRecusive(root.left)
    laterOrderRecusive(root.right)
    print(root.val)

#非递归的形式 去遍历数
#递归和循环是可以互相转换的

"""

1 先根遍历 先访问根节点，再访问左子节点，最后访问右子节点
2 中根遍历 先访问左子节点，再访问根节点，最后访问右子节点
3 后跟遍历 先访问左子节点，再访问右子节点，最后访问根节点。

"""
#非递归的前序遍历
def preOrder(root):
    if root == None:
        return None
    stack = []
    tmpNode = root
    while tmpNode or stack :
        while tmpNode:
            print(tmpNode.val)
            stack.append(tmpNode)
            tmpNode = tmpNode.left
        node  = stack.pop()
        tmpNode = node.right
#非递归的中序排序
def midOrder(root):
    if root == None:
        return None
    stack = []
    tmpNode = root
    while tmpNode or stack :
        while tmpNode:
            stack.append(tmpNode)
            tmpNode = tmpNode.left
        node  = stack.pop()
        print(node.val)
        tmpNode = node.right
#非递归的后序遍历
def laterOrder(root):
    if root == None:
        return None
    stack = []
    tmpNode = root
    while tmpNode or stack :
        while tmpNode:
            stack.append(tmpNode)
            tmpNode = tmpNode.left
        node = stack[-1]
        tmpNode = node.right
        if node.right == None:
            node = stack.pop()
            print(node.val)
            while stack and node == stack[-1].right:
                node =  stack.pop()
                print(node.val)
```

## 25.重建二叉树

**输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回构造的TreeNode根节点
    def reConstructBinaryTree(self, pre, tin):
        # write code here
        #判断 如果 是前序 或者后序 遍历结果 有为空的话，那么就返回none
        if not pre or not tin:
            return None
        #如果两个遍历的长度不一样，那么也就返回一个None
        if len(pre) != len(tin):
            return None
        #前序遍历中 第一个数 就是根节点
        # 取出pre 的第一个值  就是根节点
        root = pre[0]
        #重建二叉树的 根节点 
        rootNode = TreeNode(root)
        # 找到在 tin  中序遍历中的根节点 所在的索引位置
        pos = tin.index(root)
        # 中序遍历的 列表的左右节点 分开 切片 成两个列表
        #中序遍历 中序的 根节点 的 所在位置 的 左边部分 都是根节点的左子树上的，右边都是根节点的右子树上的
        tinLeft = tin[0:pos]
        tinRight = tin[pos + 1:]
        # 前序遍历的 列表的左右节点 分开 切片 成两个列表
        #那对于前序遍历来说， 先访问根节点，再访问左子节点，最后访问右子节点
        #那么前序遍历的左子树的结点 就是根节点的下一个结点到 前序遍历的非根节点，非叶子节点的 结点的索引位置，就是这个子二叉树的左子树的结点
        preLeft = pre[1:pos + 1]
        #那 非根节点，非叶子节点的 结点的索引位置的下一个开始 一直到结尾，都是这个结点的右子树的结点
        preRight = pre[pos + 1:]
		#递归重建根节点的左节点，也就是 每个 非根节点 非叶子节点都是一个根节点的存在，那么这个时候就是递归来重建这个 二叉树。
        leftNode = self.reConstructBinaryTree(preLeft, tinLeft)
        rightNode = self.reConstructBinaryTree(preRight, tinRight)
		#根节点的 左节点 等于 我们要 递归 的 左节点（一个新的根节点）
        rootNode.left = leftNode
        #根节点的 右节点 等于 我们要 递归 的 右节点（一个新的根节点）
        rootNode.right = rightNode
        return rootNode
```

___



## 26.树的子结构

**输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）**

对于Python这道题，有些地方需要仔细考虑的。

先说下算法实现思路：对于两棵二叉树来说，要判断B是不是A的子结构，首先第一步在树A中查找与B根节点的值一样的节点。

通常对于查找树中某一个节点，我们都是采用递归的方法来遍历整棵树。

第二步就是判断树A中以R为根节点的子树是不是和树B具有相同的结构。

这里同样利用到了递归的方法，如果节点R的值和树的根节点不相同，则以R为根节点的子树和树B肯定不具有相同的节点；

如果它们值是相同的，则递归的判断各自的左右节点的值是不是相同。

递归的终止条件是我们达到了树A或者树B的叶节点。

有地方要重点注意，DoesTree1haveTree2()函数中的两个 if 判断语句 不能颠倒顺序 。
因为如果颠倒了顺序，会先判断pRoot1 是否为None, 其实这个时候，pRoot1 的节点已经遍历完成确认相等了，但是这个时候会返回 False，判断错误。

有同学不相信的，可以去试试换个顺序，肯定不能AC。同时这个也是《剑指offer》书上没有写的，希望能引起大家的注意。

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        #判断如果 二叉树是不是空树，如果是两个中任何一个是空树，就返回 none
        if pRoot2 == None or pRoot1 == None:
            return False
        #对于两棵二叉树来说，要判断B是不是A的子结构，首先第一步在树A中查找与B根节点的值一样的节点。
        #定义一个函数，来找 相等的结点
        def hasEqual(pRoot1, pRoot2):
            if pRoot2 == None:
                return True
            if pRoot1 == None:
                return False
            #如果这两个结点的值相等的话
            if pRoot1.val == pRoot2.val:
                #，再判断子二叉树的左节点是不是空，如果是空的话，那么左节点就相等，返回真 true
                if pRoot2.left == None:
                    leftEqual = True
                else:
                    #如果不是空的话，再判断母二叉树和子二叉树的左节点是否相等
                    leftEqual = hasEqual(pRoot1.left, pRoot2.left)
                #然后判断 子二叉树的 右结点是不是空，如果是空的话，那么右结点就相等，返回 true
                if pRoot2.right == None:
                    rightEqual = True
                else:
                    #如果不是空的话，就递归，继续去判断 母二叉树的右结点和子二叉树的右结点
                    rightEqual = hasEqual(pRoot1.right, pRoot2.right)
                #最后返回的是 判断 左右 是不是相等 的判断结果，当两个同时为真的时候，这个子二叉树，就是母二叉树的子二叉树，有一个为假 则 为假。
                return leftEqual and rightEqual
            #如果这两个结点的值不相等的话，那就返回 假，这是来找 是不是相等的函数
            return False
		#判断如果说这个两个 二叉树的根节点值相等的话，那么就调用上面的函数，然后来 继续判断后面的 是否有 左树，右树等等。。。
        if pRoot1.val == pRoot2.val:
            #调用 函数判断是否相等
            ret = hasEqual(pRoot1, pRoot2)
            #如果返回的是一个true的话
            if ret:
                就返回 true
                return True
        #如果根节点不等的话，那就判断 母二叉树的左子树，是不是与 要判断的 二叉树 相等
		#判断 左 树 是不是相等的话
        ret = self.HasSubtree(pRoot1.left, pRoot2)
        if ret:
            return True
        #如果左子树不等的话，那就判断 母二叉树的右子树，是不是与 要判断的 二叉树 相等
        ret = self.HasSubtree(pRoot1.right, pRoot2)
        return ret



# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution2:
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        result = False
        if pRoot1 != None and pRoot2 != None:
            if pRoot1.val == pRoot2.val:
                result = self.DoesTree1haveTree2(pRoot1, pRoot2)
            if not result:
                result = self.HasSubtree(pRoot1.left, pRoot2)
            if not result:
                result = self.HasSubtree(pRoot1.right, pRoot2)
        return result
    # 用于递归判断树的每个节点是否相同
    # 需要注意的地方是: 前两个if语句不可以颠倒顺序
    # 如果颠倒顺序, 会先判断pRoot1是否为None, 其实这个时候pRoot2的结点已经遍历完成确定相等了, 但是返回了False, 判断错误
    def DoesTree1haveTree2(self, pRoot1, pRoot2):
        if pRoot2 == None:
            return True
        if pRoot1 == None:
            return False
        if pRoot1.val != pRoot2.val:
            return False
        return self.DoesTree1haveTree2(pRoot1.left, pRoot2.left) and self.DoesTree1haveTree2(pRoot1.right, pRoot2.right)
```

___

## 27.二叉树的镜像

**操作给定的二叉树，将其变换为源二叉树的镜像。**

##### `输入描述:`

```python
二叉树的镜像定义：源二叉树 
    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5
```

___

二叉树的镜像，就是左右 子树 调换位置，然后用递归的方式来 循环遍历 处理根节点左子树的左子树。。根节点右子树的右子树来调换位置  。。。依次递归循环

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回镜像树的根节点
    def Mirror(self, root):
        # write code here
        #如果是根节点为空的话，直接就返回空
        if root == None:
            return None
        #处理根节点，根节点的左子树和右子树来调换位置。
        root.left,root.right = root.right,root.left
        #处理根节点左子树的左子树 做递归 循环
        self.Mirror(root.left)
        #处理根节点右子树的右子树 做递归 循环
        self.Mirror(root.right)
```

___

## 28.从上往下打印二叉树

**从上往下打印出二叉树的每个节点，同层节点从左至右打印。**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回从上到下每个节点值列表，例：[1,2,3]
    def PrintFromTopToBottom(self, root):
        # write code here
        #先判断 是不是空
        if root == None:
            #空的话 依题意要求 返回 【】
            return []
        #需要一个辅助列表 给一个 根节点
        treeNodeTmp = [root]
        #需要一个返回的 数组
        ret = []
        #循环 当 我们这个辅助的 列表不为空的时候
        while treeNodeTmp:
            #把 辅助的列表 取出第一个值
            tmpNode = treeNodeTmp[0]
            #第一个结点的值 加入到 返回的数组中
            ret.append(tmpNode.val)
            #如果说 这个结点的左子树存在的话，那么就把这个左结点添加到这个返回的列表中
            if tmpNode.left:
                treeNodeTmp.append(tmpNode.left)
            #如果这个结点的右子树存在的话，那么就把这个右结点添加到这个返回的列表中
            if tmpNode.right:
                treeNodeTmp.append(tmpNode.right)
            #删除掉辅助列表的第一个值，为什么删掉第一个，是因为我们已经把辅助列表中的第一个值加入到我们要返回的数组中了，每次只添加了一个，而且每次也只循环判断了一个结点的左右子树。
            del treeNodeTmp[0]
        #最后返回我们的返回数组
        return ret
```

___

## 29.二叉搜索树的后序遍历序列

**输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。**

##### 什么叫二叉搜索树？

二叉查找树（Binary Search Tree），（又：[二叉搜索树](https://baike.baidu.com/item/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91/7077855)，二叉排序树）它或者是一棵空树，或者是具有下列性质的[二叉树](https://baike.baidu.com/item/%E4%BA%8C%E5%8F%89%E6%A0%91/1602879)： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为[二叉排序树](https://baike.baidu.com/item/%E4%BA%8C%E5%8F%89%E6%8E%92%E5%BA%8F%E6%A0%91/10905079)。

思路：二叉搜索树的定义如上所示，那么说 我们可以根据二叉搜索树的后序遍历来得出，后序遍历得到的最后一个值是根节点，那么 在遍历结果的前半部分是二叉搜索树的左子树，都比根节点 小，后半部分都比根节点大，

```python
"""
python:后序遍历 的序列中，最后一个数字是树的根节点 ，数组中前面的数字可以分为两部分：第一部分是左子树节点 的值，都比根节点的值小；第二部分 是右子树 节点的值，都比 根 节点 的值大，后面用递归分别判断前后两部分 是否 符合以上原则

"""


class Solution:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        #判断这个后序遍历的结果是不是空，如果是空的话，就返回假，如果这个数组的长度等于0
        #的话，就说明是这个数组为空，那么也返回假
        if sequence==None or len(sequence)==0:
            return False
        #数组的长度
        length=len(sequence)
        #根据 后序遍历 根节点 就是数组 的 最后一个值
        root=sequence[length-1]
        # 在二叉搜索 树中 左子树节点小于根节点
        for i in range(length):
            if sequence[i]>root:
                break
        # 二叉搜索树中右子树的节点都大于根节点
        for j  in range(i,length):
            if sequence[j]<root:
                return False
        # 判断左子树是否为二叉树
        left=True
        if  i>0:
            left=self.VerifySquenceOfBST(sequence[0:i])
        # 判断 右子树是否为二叉树
        right=True
        if i<length-1:
            right=self.VerifySquenceOfBST(sequence[i:-1])
        return left and right


# -*- coding:utf-8 -*-
class Solution2:
    def VerifySquenceOfBST(self, sequence):
        # write code here
        #判断这个后序遍历的结果是不是空，如果是空的话，就返回假，如果这个数组的长度等于0
        #的话，就说明是这个数组为空，那么也返回假
        if sequence == []:
            return False
        #根据 后序遍历 根节点 就是数组 的 最后一个值
        rootNum = sequence[-1]
        #删除这个遍历的数组的最后一个值 也就是根节点
        del sequence[-1]
        #遍历 索引值 为 空
        index = None
        #遍历循环现在的这个 后序遍历的数组
        for i in range(len(sequence)):
            #如果说 索引值等于空而且 遍历得到的数组的那个值 是大于我们的根节点的
            if index == None and sequence[i] > rootNum:
				#这个时候就把索引赋值给 index
                index = i
            #如果说索引不等于空而且数组的值都是小于 根节点的，那么就是说不满足搜索二叉树的定义，那么就返回假
            if index != None and sequence[i] < rootNum:
                return False
        #得到那个大于根节点的值的索引值以后，判断
        #如果说 它的 后序遍历的数组中前半部分等于空的话，那么判断是否为这个二叉搜索树的左半部分就为真。
        if sequence[:index] == []:
            leftRet = True
        #如果左半边不是空的话，那么就以左半边为一个新的遍历得到的数组，来递归判断做一个循环
        else:
            leftRet = self.VerifySquenceOfBST(sequence[:index])
        #得到那个大于根节点的值的索引值以后，判断
        #如果说 它的 后序遍历的数组中后半部分等于空的话，那么判断是否为这个二叉搜索树的右半部分就为真。
        if sequence[index:] == []:
            rightRet = True
        else:
            #如果右半边不是空的话，那么就以右半边为一个新的遍历得到的数组，来递归判断做一个循环
            rightRet = self.VerifySquenceOfBST(sequence[index:])
		#最后返回 如果两边判断都是真的话那么这个后序遍历就是这个二叉搜索树的后序遍历。
        return leftRet and rightRet
```

___



## 30.二叉树中和为某一值的路径

**输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)**

思路：在树的前序，中序，后序遍历三种遍历方式中，只有前序遍历是首先访问根节点的，所以我们需要用到前序遍历，当我们用前序遍历访问到某一个叶子节点的时候，我们把该节点添加到路径上，并累加该节点的值，如果该节点为叶节点，并且路径中节点值刚好等于输入的整数，则当前的路径符合要求，我们把它打印出来，如果当前的结点不是叶子节点，则继续访问它的子节点。当前节点访问结束后，递归函数将自动回到它的父节点。因此我们在函数退出之前要在路径上删除当前的结点并减去当前结点的值，以确保返回父节点时路径刚好是从根节点到父节点。

```python
"""
递归先序遍历树， 把结点加入路径。
若该结点是叶子结点则比较当前路径和是否等于期待和。
弹出结点，每一轮递归返回到父结点时，当前路径也应该回退一个结点

"""
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import copy


class Solution:
    # 返回二维列表，内部每个列表表示找到的路径
    def FindPath(self, root, expectNumber):
        # write code here
        #判断如果 根节点为空的话那么没有数据为空
        if root == None:
            return []
		#返回的列表
        ret = []
        #辅助列表 第一个值为 根节点
        support = [root]
        #辅助数组  第一个  为根节点的值
        supportArrayList = [[root.val]]
		#当我们辅助列表中有节点的时候
        while support:
            #临时的结点 为 辅助列表中的第一个结点
            tmpNode = support[0]
            #临时的数组  为 辅助数组中的第一个值
            tmpArrayList = supportArrayList[0]
            #判断 我们的临时结点的左节点是不是存在，而且 临时结点的右结点是不是存在，如果不存在的话
            if tmpNode.left == None and tmpNode.right == None:
                #就判断我们现在临时数组中的值的和是不是等于输入的那个整数
                if sum(tmpArrayList) == expectNumber:
                    #如果等于的话，就把这个临时的辅助数组插入到我们返回的数组中，这就是我们找到的第一个满足条件的路径，题目要求 长度大的靠前，那么也就是说越先插进去越靠后，所以我们用insert 索引插入。
                    ret.insert(0, tmpArrayList)
			#如果我们临时结点的左节点存在
            if tmpNode.left:
                #在我们辅助列表中添加这个左节点
                support.append(tmpNode.left)
                #然后把原来临时数组复制一份，为新的辅助数组，来把这个左节点的值插入到我们的新临时辅助数组中，在辅助数组中添加这个新的临时数组
                newTmpArrayList = copy.copy(tmpArrayList)
                newTmpArrayList.append(tmpNode.left.val)
                supportArrayList.append(newTmpArrayList)
            #如果我们临时结点的右结点存在的话
            if tmpNode.right:
                #步骤同上，把右结点添加到辅助列表中
                support.append(tmpNode.right)
				#把原来的临时辅助数组 复制一份给 新的辅助数组
                newTmpArrayList = copy.copy(tmpArrayList)
                #然后 把 临时结点的右结点的值添加到 我们新的临时辅助数组中
                newTmpArrayList.append(tmpNode.right.val)
                #在辅助数组中添加这个新的临时辅助数组
                supportArrayList.append(newTmpArrayList)
			#删除辅助数组中的第一个值
            del supportArrayList[0]
            #删除辅助列表的第一个值
            del support[0]
		#返回我们插入的满足题目的 和为某一值的路径的返回的列表
        return ret

#针对上面思路的代码：

# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution2:
    # 返回二维列表，内部每个列表表示找到的路径
    def FindPath(self, root, expectNumber):
        # write code here
        if not root:
            return []

        result = []

        def FindPathMain(root, path, currentSum):
            currentSum += root.val

            path.append(root)
            isLeaf = root.left == None and root.right == None

            if currentSum == expectNumber and isLeaf:
                onePath = []
                for node in path:
                    onePath.append(node.val)
                result.append(onePath)

            if currentSum < expectNumber:
                if root.left:
                    FindPathMain(root.left, path, currentSum)
                if root.right:
                    FindPathMain(root.right, path, currentSum)

            path.pop()

        FindPathMain(root, [], 0)

        return result
```

___



## 31.二叉搜索树与双向链表 

**输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。**

在二叉树中每个结点都有两个指向子节点的指针，在双向链表中也是有两个指针，分别指向前一个结点和后一个结点。

根节点 左指针要链在 左子树的最右边，根节点的右指针要链在右子树的最左边。

排序的，根节点和 左子树中最大的链，和右子树最小的链。

那么把每一个结点都当做一个根节点来递归，那么就是在循环做上面的操作。

对于整个链表来说，我们要输出最左结点。

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, pRootOfTree):
        # write code here
        #首先判断给的二叉树是不是空的，如果是空的就返回空
        if pRootOfTree == None
            return None
		#找右结点 右子树
        def find_right(node):
            while node.right:
                node = node.right
            return node
		#递归 处理左节点，并返回左子树 最左边的结点
        leftNode = self.Convert(pRootOfTree.left)
        #递归处理右结点，并返回右子树最左边的结点
        rightNode = self.Convert(pRootOfTree.right)
        #将返回的左子树的左节点赋值给返回的结点 保存住，因为么最后要返回这个。
        retNode = leftNode
		#如果输出的左节点存在
        if leftNode:
            #那么我们就要去寻找它的右结点，一直找，找到最右的，以便后面我们要他跟根节点相链接上
            #把最右的这个结点 赋值 给 左节点
            leftNode = find_right(leftNode)
        else:
            #如果没有左节点存在的话，那么把根节点的值给这个返回的结点
            retNode = pRootOfTree
            
		#根节点的左节点 与 上面我们找右结点 找到的 那个 最右的结点赋值给 左节点 的结点 相 链接 上
        pRootOfTree.left = leftNode
        #根节点的右结点，是上面返回回来的右部分的 最左的一个
        pRootOfTree.right = rightNode
        
		#如果左节点不为空的话
        if leftNode != None:
            #那么左子树最右边的结点的右指针指向根节点
            leftNode.right = pRootOfTree
         #如果右结点不为空
        if rightNode != None:
            #那么右子树最左边的左指针指向根节点
            rightNode.left = pRootOfTree
		#最后返回这个 最左的结点
        return retNode
```
___

## 32.最小的k个数

**输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。**

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        # 最大堆

        # 创建或者插入最大堆
        def createMaxHeap(num):
            maxHeap.append(num)
            currentIndex = len(maxHeap) - 1
            while currentIndex != 0:
                parentIndex = (currentIndex - 1) >> 1
                if maxHeap[parentIndex] < maxHeap[currentIndex]:
                    # 交换位置
                    maxHeap[parentIndex], maxHeap[currentIndex] = maxHeap[currentIndex], maxHeap[parentIndex]
                else:
                    break

        # 调整最大堆，头结点发生改变
        def adjustMaxHeap(num):
            if num < maxHeap[0]:
                maxHeap[0] = num
            maxHeapLen = len(maxHeap)
            index = 0
            while index < maxHeapLen:
                leftIndex = index * 2 + 1
                rightIndex = index * 2 + 2
                largerIndex = 0
                if rightIndex < maxHeapLen:
                    if maxHeap[rightIndex] < maxHeap[leftIndex]:
                        largerIndex = leftIndex
                    else:
                        largerIndex = rightIndex
                elif leftIndex < maxHeapLen:
                    largerIndex = leftIndex
                else:
                    break
                if maxHeap[index] < maxHeap[largerIndex]:
                    maxHeap[index], maxHeap[largerIndex] = maxHeap[largerIndex], maxHeap[index]
                index = largerIndex

        maxHeap = []
        inputLen = len(tinput)
        if inputLen < k or k <= 0:
            return []
        for i in range(inputLen):
            if i < k:
                createMaxHeap(tinput[i])
            else:
                adjustMaxHeap(tinput[i])
        maxHeap.sort()
        return maxHeap
```

___

```python
# -*- coding:utf-8 -*-
class Solution:
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        if tinput == [] or k > len(tinput):
            return []
        tinput.sort()
        return tinput[: k]
```

## 33.数据流中的中位数

**如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用`GetMedian()`方法获取当前读取数据的中位数。**

```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.arr=[]
    def Insert(self, num):
        self.arr.append(num)
        self.arr.sort()
    def GetMedian(self,fuck):
        length=len(self.arr)
        if length%2==1:
            return self.arr[length//2]
        return(self.arr[length//2]+self.arr[length//2-1])/2.0
```

```python
# -*- coding:utf-8 -*-
class Solution:
    def __init__(self):
        self.littleValueMaxHeap = []
        self.bigValueMinHeap = []
        self.maxHeapCount = 0
        self.minHeapCount = 0

    def Insert(self, num):
        def cmpMaxHeap(a, b):
            return a > b

        def cmpMinHeap(a, b):
            return a < b

        # write code here
        if self.maxHeapCount > self.minHeapCount:
            self.minHeapCount += 1
            if num < self.littleValueMaxHeap[0]:
                tmpNum = self.littleValueMaxHeap[0]
                self.adjustHeap(num, self.littleValueMaxHeap, cmpMaxHeap)
                self.createHeap(tmpNum, self.bigValueMinHeap, cmpMinHeap)
            else:
                self.createHeap(num, self.bigValueMinHeap, cmpMinHeap)
        else:
            self.maxHeapCount += 1
            if len(self.littleValueMaxHeap) == 0:
                self.createHeap(num, self.littleValueMaxHeap, cmpMaxHeap)
            else:
                if self.bigValueMinHeap[0] < num:
                    tmpNum = self.bigValueMinHeap[0]
                    self.adjustHeap(num, self.bigValueMinHeap, cmpMinHeap)
                    self.createHeap(tmpNum, self.littleValueMaxHeap, cmpMaxHeap)
                else:
                    self.createHeap(num, self.littleValueMaxHeap, cmpMaxHeap)
        # print(self.littleValueMaxHeap)
        # print(self.bigValueMinHeap)

    def GetMedian(self, n=None):
        # write code here
        if self.minHeapCount < self.maxHeapCount:
            return self.littleValueMaxHeap[0]
        else:
            return float(self.littleValueMaxHeap[0] + self.bigValueMinHeap[0]) / 2

    def createHeap(self, num, heap, cmpfun):
        heap.append(num)
        tmpIndex = len(heap) - 1
        while tmpIndex:
            parentIndex = (tmpIndex - 1) // 2
            if cmpfun(heap[tmpIndex], heap[parentIndex]):
                heap[parentIndex], heap[tmpIndex] = \
                    heap[tmpIndex], heap[parentIndex]
                tmpIndex = parentIndex
            else:
                break

    def adjustHeap(self, num, heap, cmpFunc):
        if num < heap[0]:
            maxHeapLen = len(heap)
            heap[0] = num
            tmpIndex = 0
            while tmpIndex < maxHeapLen:
                leftIndex = tmpIndex * 2 + 1
                rightIndex = tmpIndex * 2 + 2
                largerIndex = 0
                if rightIndex < maxHeapLen:
                    largerIndex = rightIndex if cmpFunc(heap[rightIndex], heap[leftIndex]) else leftIndex
                elif leftIndex < maxHeapLen:
                    largerIndex = leftIndex
                else:
                    break

                if cmpFunc(heap[largerIndex], heap[tmpIndex]):
                    heap[largerIndex], heap[tmpIndex] = \
                        heap[tmpIndex], heap[largerIndex]
                    index = largerIndex
                else:
                    break
```

## 34.二叉树的下一个结点

**给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。**

```python
# -*- coding:utf-8 -*-
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None
class Solution:
    def GetNext(self, pNode):
        # write code her
        #寻找右子树，如果存在就一直找到右子树的最左边就是下一个节点
        # 没有右子树，就寻找他的父节点，一直找到它是父节点的左子树，打印父节点
        if pNode.right:
            tmpNode = pNode.right
            while tmpNode.left:
                tmpNode = tmpNode.left
            return tmpNode
        else:
            tmpNode =pNode
            while tmpNode.next:
                if tmpNode.next.left == tmpNode:
                    return tmpNode.next
                tmpNode = tmpNode.next
            return None
```

## 35.对称的二叉树

**请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def isSymmetrical(self, pRoot):
        # write code here
        def isMirror(left, right):
            if left == None and right == None:
                return True
            elif left == None or right == None:
                return False

            if left.val != right.val:
                return
```

## 36.按之字形顺序打印二叉树

**请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Print(self, pRoot):
        # write code here
        if pRoot == None:
            return []

        stack1 = [pRoot]
        stack2 = []

        ret = []
        while stack1 or stack2:
            if stack1:
                tmpRet = []
                while stack1:
                    tmpNode = stack1.pop()
                    tmpRet.append(tmpNode.val)
                    if tmpNode.left:
                        stack2.append(tmpNode.left)
                    if tmpNode.right:
                        stack2.append(tmpNode.right)
                ret.append(tmpRet)

            if stack2:
                tmpRet = []
                while stack2:
                    tmpNode = stack2.pop()
                    tmpRet.append(tmpNode.val)
                    if tmpNode.right:
                        stack1.append(tmpNode.right)
                    if tmpNode.left:
                        stack1.append(tmpNode.left)

                ret.append(tmpRet)
        return ret
```

## 37.把二叉树打印成多行

**从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表[[1,2],[4,5]]
    def Print(self, pRoot):
        # write code here
        if pRoot == None:
            return []
        queue1 = [pRoot]
        queue2 = []
        ret = []
        while queue1 or queue2:
            if queue1:
                tmpRet = []
                while queue1:
                    tmpNode = queue1[0]
                    tmpRet.append(tmpNode.val)
                    del queue1[0]
                    if tmpNode.left:
                        queue2.append(tmpNode.left)
                    if tmpNode.right:
                        queue2.append(tmpNode.right)
                ret.append(tmpRet)
            if queue2:
                tmpRet = []
                while queue2:
                    tmpNode = queue2[0]
                    tmpRet.append(tmpNode.val)
                    del queue2[0]
                    if tmpNode.left:
                        queue1.append(tmpNode.left)
                    if tmpNode.right:
                        queue1.append(tmpNode.right)
                ret.append(tmpRet)
        return ret
```

## 38.二叉搜索树的第k个结点

**给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回对应节点TreeNode
    def KthNode(self, pRoot, k):
        # write code here
        retList = []

        def preOrder(pRoot):
            if pRoot == None:
                return None
            preOrder(pRoot.left)
            retList.append(pRoot)
            preOrder(pRoot.right)

        preOrder(pRoot)
        if len(retList) < k or k < 1:
            return None

        return retList[k - 1]
```

## 39.序列化二叉树

**请实现两个函数，分别用来序列化和反序列化二叉树**

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Serialize(self, root):
        # write code here
        retList = []

        def preOrder(root):
            if root == None:
                retList.append('#')
                return
            retList.append(str(root.val))
            preOrder(root.left)
            preOrder(root.right)

        preOrder(root)
        return ' '.join(retList)

    def Deserialize(self, s):
        # write code here
        retList = s.split()

        def dePreOrder():
            if retList == []:
                return None
            rootVal = retList[0]
            del retList[0]
            if rootVal == '#':
                return None
            node = TreeNode(int(rootVal))

            leftNode = dePreOrder()
            rightNode = dePreOrder()

            node.left = leftNode
            node.right = rightNode
            return node

        pRoot = dePreOrder()
        return pRoot
```

## 40.连续子数组的最大和

**HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)**

```python
# -*- coding:utf-8 -*-
class Solution:
    def FindGreatestSumOfSubArray(self, array):
        # write code here
        maxNum = None
        tmpNum = 0

        for i in array:
            if maxNum == None:
                maxNum = i
            if tmpNum + i < i:
                tmpNum = i
            else:
                tmpNum += i
            if maxNum < tmpNum:
                maxNum = tmpNum
        return maxNum
```

## 41.矩形的覆盖

**我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？**

```python
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
        # write code here
        #if number == 0:
            #return 0
        #if number == 1:
            #return 1
        #if number == 2:
            #return 2
        #a = 1
        #b = 2
        #ret = 0
        #for  i in range(3,number+1):
            #ret = a + b
            #a = b
            #b = ret
        #return ret
        res = [0,1,2]
        while len(res) <= number:
            res.append(res[-2]+res[-1])
        return res[number]
```

## 42.字符串的排列

**输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。**

##### 输入描述:

```python
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。
```

```python
# -*- coding:utf-8 -*-
class Solution:
    def Permutation(self, ss):
        # write code here
        import itertools
        result = []
        if not ss:
            return []
        else:
            res = itertools.permutations(ss)
            for i in res:
                if "".join(i) not in result:
                    result.append("".join(i))
        return result
```

## 43.把数组排成最小的数

**输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。**

```python
# -*- coding:utf-8 -*-
def QuickSort(array,left=0,right=None,cmp=None):
    arrayLen = len(array)
    if arrayLen <= 1:
        return
    if right == None:
        right = arrayLen -1
    if left < right:
        pivot = partition(array,left,right,cmp)
        QuickSort(array,left,pivot-1,cmp)
        QuickSort(array,pivot+1,right,cmp)

def partition(array,left,right,cmp=None):
    i = left-1
    for j in range(left,right):
        smaller = False
        if cmp:
            smaller = cmp(array[j],array[right])
        elif array[j] < array[right]:
            smaller = True
        if smaller:
            array[j],array[i+1] = array[i+1],array[j]
            i += 1
    array[right],array[i+1] = array[i+1],array[right]
    return i+1

class Solution:
    def PrintMinNumber(self, numbers):
        # write code here
        # def cmpNum(num1,num2):
        #     n1 = str(num1) + str(num2)
        #     n2 = str(num2) + str(num1)
        #     if int(n1) < int(n2):
        #         return True
        #     else:
        #         return False
        QuickSort(numbers,cmp=lambda num1,num2: 1 if str(num1)+str(num2) < str(num2)+str(num1) else 0 )
        # numbers.sort(key=cmpNum)
        # ret = ''
        # for i in numbers:
        #     ret += str(i)

        return ''.join(map(str,numbers))


if __name__ == '__main__':
    s = Solution()
    res = s.PrintMinNumber([3,32,321,45])
    print(res)
```

