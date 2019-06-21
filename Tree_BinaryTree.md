## 树 二叉树

树的基本结构
节点










#### 102. 二叉树的层次遍历
给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

    例如:
    给定二叉树: [3,9,20,null,null,15,7],

        3
       / \
      9  20
        /  \
       15   7
    //层次遍历二叉树，用队列暂存树的节点
    
    var levelOrder = function(root) {

      if(root === null || root.length === 0){
        return [];
      }

        let queue = [], result = []
        queue.push(root)
        while(queue.length !== 0) {

          let level = []  //保存当前层过结果
          let size = queue.length
          for(let i = 0; i < size; i++) {
            node = queue.shift()
            level.push(node.val)
            if(node.left) {
              queue.push(node.left)
            }
            if(node.right) {
              queue.push(node.right)
            }
          }
          result.push(level)
        }
        return result
    };
    
 #### 107. 二叉树的层次遍历 II
 给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

    例如：
    给定二叉树 [3,9,20,null,null,15,7],

        3
       / \
      9  20
        /  \
       15   7
    返回其自底向上的层次遍历为：

    [
      [15,7],
      [9,20],
      [3]
    ]

    //思路：正常二叉树层次遍历之后，倒序即符合要求
    
    var levelOrderBottom = function(root) {
        if(root === null || root.length === 0){
        return [];
      }

        let queue = [], result = []
        queue.push(root)
        while(queue.length !== 0) {

          let level = []  //保存当前层过结果
          let size = queue.length

          for(let i = 0; i < size; i++) {
            node = queue.shift()
            level.push(node.val)
            if(node.left) {
              queue.push(node.left)
            }
            if(node.right) {
              queue.push(node.right)
            }
          }
          result.push(level)
        }

        return result.reverse()

    };
    
#### 199. 二叉树的右视图
给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

示例:

    输入: [1,2,3,null,5,null,4]
    输出: [1, 3, 4]
    解释:

       1            <---
     /   \
    2     3         <---
     \     \
      5     4       <---
    var rightSideView = function(root) {
        var result = [];

        if(root === null) {
            return result;
        }

        queue = [];
        queue.push(root);

        while(queue.length > 0) {
            var len = queue.length;

            for(var i = 0; i < len; i++) {
                var node = queue.shift();

                if(node.left) {
                    queue.push(node.left);
                }

                if(node.right) {
                    queue.push(node.right);
                }

                if(i === len - 1) {
                    result.push(node.val);
                }
            }
        }

        return result;
    };
 
#### 129. 求根到叶子节点数字之和
给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 1->2->3 代表数字 123。

计算从根到叶子节点生成的所有数字之和。


    示例 1:

    输入: [1,2,3]
        1
       / \
      2   3
    输出: 25
    解释:
    从根到叶子节点路径 1->2 代表数字 12.
    从根到叶子节点路径 1->3 代表数字 13.
    因此，数字总和 = 12 + 13 = 25.


    //思路：利用层次遍历，每层的每个节点存放的是上层节点跟该节点的连接，
    //每次到达叶子节点，更新sum的值
    var sumNumbers = function(root) {

        var sum = 0;
        if(root==null) return sum;

        var queue = [];
        queue.push(root);

        while(queue.length !== 0){
            var node = queue.shift();

            if(node.left == null && node.right==null){
                sum += parseInt(node.val);
            }

            if(node.left){
                node.left.val ='' + node.val + node.left.val;
                queue.push(node.left)
            }

            if(node.right){
                node.right.val = ''+node.val+node.right.val;
                queue.push(node.right);
            }
        }

        return sum; 

    };

#### 230. 二叉搜索树中第K小的元素

    输入: root = [5,3,6,2,4,null,null,1], k = 3
           5
          / \
         3   6
        / \
       2   4
      /
     1
    输出: 3


    //思路用栈来维护最小值
    var kthSmallest = function(root, k) {

        var stack=[];
        var node = root；

        //将所有子节点放到栈中
        while(node){
            stack.push(node);
            node = node.left;   
        }

        //栈顶是最小值，每次弹出栈顶
        //如果栈顶元素有右节点，入栈，并且将该节点的所有子节点入栈
        var x=1; 
        while(stack && x<=k){
            var n = stack.pop();
            x +=1;

            var w = n.right;
            while(w){
                stack.push(w);
                w = w.left;
            }

        }

        return n.val
    };
