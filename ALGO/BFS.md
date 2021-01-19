# 广度优先搜索

广度优先搜索，BFS(Breadth-First Search)。将问题抽象成图，然后从一个点开始，向四周扩散。

BFS问题的本质是在一幅图中，找到从起点start到终点target的最短距离。一般采用队列的存储方式。

核心代码

```c++
int BFS(Node start, Node target) {
		queue<int> q;						//核心数据结构
		set<Node> visited;			//访问过的结点，避免重复访问
		
		q.push(start);						//将起点加入队列
		visited.insert(start);		//加入一访问列表
		int step = 0;							//记录步数
		
		while (!q.empty()) {
      	int size = q.size();
      	/* 将当前队列中的所有结点向四周扩散 */
      	for (int i = 0; i < size; i++) {
          	Node cur = q.begin();
          	q.pop();
          	/* 判断是否走到了终点 */
          	if (cur == target) {
              	return step;
            }
          	/* 将当前结点cur的邻居都加入队列 */
          	for (Node x : cur.adj()) {
              	if (!visited.find(x)) {
                  	q.push(x);
                  	visited.insert(x);
                }           	
            }
        }
      	/* 更新步数 */
      	step++;
    }
}
```

