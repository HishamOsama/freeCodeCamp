---
title: Depth First Search (DFS)
localeTitle: عمق البحث الأول (DFS)
---
## عمق البحث الأول (DFS)

يعد Depth First Search يتعتر واحداً من أكثر خوارزميات الرسم البياني بساطةً. إنها تعبر الرسم البياني عن طريق التحقق أولاً من العقدة الحالية ومن ثم الانتقال إلى أحد مساعديها لتكرار العملية. إذا لم يكن لدى العقدة الحالية أي اختبار للتحقق ، فإننا ننتقل إلى سابقه وتستمر العملية (بالانتقال إلى سواقة أخرى). إذا تم العثور على الحل يتوقف البحث.

### تصور

![](https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif)

### التنفيذ (C ++ 14)

```c++
#include <iostream> 
#include <vector> 
#include <queue>  
#include <algorithm>
 using namespace std; 
  
class Graph{ 
    int v;    // number of vertices 
  
    // pointer to a vector containing adjacency lists 
    vector < int > *adj;
public: 
    Graph(int v);  // Constructor 
  
    // function to add an edge to graph 
    void add_edge(int v, int w);  
  
    // prints dfs traversal from a given source `s` 
    void dfs();
    void dfs_util(int s, vector < bool> &visited);   
}; 
  
Graph::Graph(int v){ 
    this -> v = v; 
    adj = new vector < int >[v]; 
} 
  
void Graph::add_edge(int u, int v){ 
    adj[u].push_back(v); // add v to u’s list
    adj[v].push_back(v);  // add u to v's list (remove this statement if the graph is directed!)
} 
 void Graph::dfs(){
    // visited vector - to keep track of nodes visited during DFS
    vector < bool > visited(v, false);  // marking all nodes/vertices as not visited
    for(int i = 0; i < v; i++)
        if(!visited[i])
            dfs_util(i, visited);
} 
 // notice the usage of call-by-reference here!
void Graph::dfs_util(int s, vector < bool > &visited){ 
    // mark the current node/vertex as visited
    visited[s] = true;
     // output it to the standard output (screen)
    cout << s << " ";
    
    // traverse its adjacency list and recursively call dfs_util for all of its neighbours!
    // (only if the neighbour has not been visited yet!)
    for(vector < int > :: iterator itr = adj[s].begin(); itr != adj[s].end(); itr++)
        if(!visited[*itr])
            dfs_util(*itr, visited); 
} 
  
int main() 
{ 
    // create a graph using the Graph class we defined above
    Graph g(4); 
    g.add_edge(0, 1); 
    g.add_edge(0, 2); 
    g.add_edge(1, 2); 
    g.add_edge(2, 0); 
    g.add_edge(2, 3); 
    g.add_edge(3, 3); 
  
    cout << "Following is the Depth First Traversal of the provided graph"
         << "(starting from vertex 0): "; 
    g.dfs(); 
    // output would be: 0 1 2 3
    return 0; 
} 
```


### التقييم 
 
تعقيد المساحة: O(n) 
 
تعقيد الوقت: O(n)

 يتمكن عمق البحث الأول من ايجاد الحل دائما اذا كان عدد العقد معروف عمق و يؤدي  هذا النوع من البحث بطريقة افضل في حالات الاشجار ذات العمق السطحي
 
### التنفيذ (C++) 

```c++
#include<iostream>
#include<vector>
#include<queue>

using namespace std;

struct Graph{
	int v;
	bool **adj;
	public:
		Graph(int vcount);
		void addEdge(int u,int v);
		void deleteEdge(int u,int v);
		vector<int> DFS(int s);
		void DFSUtil(int s,vector<int> &dfs,vector<bool> &visited);
};
Graph::Graph(int vcount){
	this->v = vcount;
	this->adj=new bool*[vcount];
	for(int i=0;i<vcount;i++)
		this->adj[i]=new bool[vcount];
	for(int i=0;i<vcount;i++)
		for(int j=0;j<vcount;j++)
			adj[i][j]=false;
}

void Graph::addEdge(int u,int w){
	this->adj[u][w]=true;
	this->adj[w][u]=true;
}

void Graph::deleteEdge(int u,int w){
	this->adj[u][w]=false;
	this->adj[w][u]=false;
}

void Graph::DFSUtil(int s, vector<int> &dfs, vector<bool> &visited){
	visited[s]=true;
	dfs.push_back(s);
	for(int i=0;i<this->v;i++){
		if(this->adj[s][i]==true && visited[i]==false)
			DFSUtil(i,dfs,visited);
	}
}

vector<int> Graph::DFS(int s){
	vector<bool> visited(this->v);
	vector<int> dfs;
	DFSUtil(s,dfs,visited);
	return dfs;
}
```

#### معلومات اكثر:

[الرسوم البيانية](https://github.com/freecodecamp/guides/computer-science/data-structures/graphs/index.md)

[نطاق البحث الأول (BFS)](https://github.com/freecodecamp/guides/tree/master/src/pages/algorithms/graph-algorithms/breadth-first-search/index.md)

[عمق البحث الأول (DFS) - ويكيبيديا](https://en.wikipedia.org/wiki/Depth-first_search)

[Depth First Search(DFS) || تصفح الاشجار](https://www.youtube.com/watch?v=_ForVbEBj-Q)
