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

