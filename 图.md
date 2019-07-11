#### 图知识预热
参考：https://zhuanlan.zhihu.com/p/25853745

图是由顶点和边组成。分为有向图、无向图。

**图的结构表示**：邻接矩阵、邻接表
- 邻接矩阵：
> 邻接矩阵用一个二维数组来表示图中顶点的连接情况；如果索引为i的节点和索引为j的节点连接，则array[i][j] === 1，否则array[i][j] === 0。邻接矩阵的缺点是，如果图不是强连通的，矩阵中就会出现很多0，从而计算机需要浪费存储空间来表示根本不存在的边。
- 邻接表
> 将边存储为由顶点的相邻顶点列表构成的数组,并以此顶点作为索引。只要能表示一对多的数据结构，都可以用来描述邻接表，比如多维列表（数组）、链表、散列表、字典等。




**图的遍历**
- 深度优先搜索 栈
- 广度优先搜索 队列 
   查找与当前顶点相邻的未访问顶点,将其添加到已访问顶点列表及队列中;
   从图中取出下一个顶点 v,添加到已访问的顶点列表;
   将所有与 v 相邻的未访问顶点添加到队列。
- 拓扑排序

      function Graph(v) {
        this.vertices = v; // 顶点数
        this.edges = 0; // 边数
        this.adj = [];
        this.marked = [];
        for (let i = 0; i < this.vertices; i++) {
            this.adj[i] = [];
            this.marked[i] = false;
        }
        this.addEdge = addEdge;
        this.showGraph = showGraph;
        this.dfs = dfs;
        this.bfs = bfs;
        this.topSort = topSort;
        this.topSortHelper = topSortHelper;
      }
      function addEdge(v, w) {
        this.adj[v].push(w);
        this.adj[w].push(v);
        this.edges++;
      }
      function showGraph() {
        for (let i = 0; i < this.vertices; i++) {
            let str = i + '->';
            for (let j = 0; j < this.vertices; j++) {
                if (this.adj[i][j] !== undefined) {
                    str += this.adj[i][j] + ' ';
                }
            }
            console.log(str);
        }
      } 
      function dfs(v) {
        this.marked[v] = true;
        if (this.adj[v] !== undefined) {
            console.log('Visisted vertex: ' + v);
        }
        this.adj[v].forEach((w) => {
            if (!this.marked[w]) {
                this.dfs(w);
            }
        })
      }
      function bfs(s) {
        let queue = [];
        this.marked[s] = true;
        queue.push(s);
        while (queue.length > 0) {
            let v = queue.shift();

            if (v !== undefined) {
                console.log("Visisted vertex: " + v);
            }
            this.adj[v].forEach((w) => {
                if (!this.marked[w]) {
                    this.marked[w] = true;
                    queue.push(w);
                }
            })
        }
      }
      function topSort() {
        let stack = [];
        let visited = [];
        for (let i = 0; i < this.vertices; i++) {
            visited[i] = false;
        }
        for (let i = 0; i < this.vertices; i++) {
            if (visited[i] === false) {
                this.topSortHelper(i, visited, stack);
            }
        }
        for (let i = stack.length - 1; i >= 0; i--) {
            console.log(this.vertexList[stack[i]])
        }
      }
      function topSortHelper(v, visited, stack) {
        visited[v] = true;

        this.adj[v].forEach((w) => {
            if (!visited[w]) {
                this.topSortHelper(w, visited, stack)
            }
        })
        stack.push(v)
      }

      // var g = new Graph(5);
      // g.addEdge(0,1);
      // g.addEdge(0,2);
      // g.addEdge(1,3);
      // g.addEdge(2,4);
      // // g.dfs(2);
      // g.bfs(0);

      var g = new Graph(6);
      g.addEdge(1,2);
      g.addEdge(2,5);
      g.addEdge(1,3);
      g.addEdge(1,4);
      g.addEdge(0, 1);

      g.vertexList = ["CS1", "CS2", "Data Structures",
                           "Assembly Language", "Operating Systems",
                           "Algorithms"];
      g.topSort();

      // ====输出====
      // CS1
      // CS2
      // Operating Systems
      // Assembly Language
      // Data Structures
      // Algorithms


#### 207. 课程表
现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]


      示例 1:

      输入: 2, [[1,0]] 
      输出: true
      解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
      示例 2:

      输入: 2, [[1,0],[0,1]]
      输出: false
      解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
      
      // n个课程，它们之间有m个依赖关系，可以看成顶点个数为n，边 个数为m的有向图。
      // 图1:n = 3, m = [[0, 1], [0, 2], [1, 2]];可以完成。
      // 图2:n = 3, m = [[0, 1], [1, 2], [2, 0]];不可以完成。 
      // 故，若有向图无环，则可以完成全部课程，否则不能。问题转 换成，构建图，并判断图是否有环。
      // 在深度优先搜索时，如果正在搜索某一顶点(还未退出该顶点的 递归深度搜索)，又回到了该顶点，即证明图有环


      var canFinish = function(numCourses, prerequisites) {
        var nodes = constructGraph(numCourses, prerequisites);

        for (var i = 0; i < nodes.length; i++) {
          var parent = [];
          var hasCycle = dfs(nodes[i], parent);

          if (hasCycle) return false;
        }
        return true;
      }

      var constructGraph = function(numNodes, pre) {
        var nodes = [];
        for (var i = 0; i < numNodes; i++) {
          var node = {};
          node.neighbors = [];
          nodes.push(node);
        }
        for (var j = 0; j < pre.length; j++) {
          var requiredCourse = pre[j][1];
          var course = pre[j][0];
          // pushing course that require required-course to it's neighbor
          // when we go to the required-course, and traverse it's neighbors, we want to make sure that those neighbor doesn't have others that nodes
          // that required those neighbor plus those neighbor's required-course
          // example [1,0], [0,2], [2,1]
          // 1 required 0, 0 required 2, and 2 required 1
          // it creates loop
          nodes[requiredCourse].neighbors.push(nodes[course]);
        }
        return nodes;
      };

      var dfs = function(startNode, parents) {
        if (parents.indexOf(startNode) >= 0) return true;
        if (startNode.visited) return false;

        startNode.visited = true;
        var neighbors = startNode.neighbors;
        parents.push(startNode);
        for (var i = 0; i < neighbors.length; i++) {
          var hasCycle = dfs(neighbors[i], parents);
          if (hasCycle) return true;
        }
        parents.pop();
      };
