## BFS

![image-20220306142731646](D:\leetcode-Team\DataStructure\pictures\image-20220306142731646.png)

```java
public class Node {
  int val;
  List<Node> children;
}


public boolean BFS(int searchValue, Node tree){
  if(tree.val == searchValue){
    return true;
  }
    
    List<Node> adjacent = tree.getChildren();
    Set<Node> visited = new HashSet<>();
    visited.add(tree);
    while(!adjacent.isEmpty()){
        //Iterate each adjacency node
        Node eachAdj = adjacent.remove();
           if(eachAdj.getVal() == searchValue)
                return true;
        //Iterate children of each adjancent node
        for(int i = 0; i < eachAdj.children.size(); i++){
            Node currentNode = eachAdj.getChildren.get(i);
         
            if(!visited.contains(currentNode)){
                adjacent.addAll(currentNode);
                 
            }
        }
        
         visited.add(eachAdj);
      
    }
    
    return false;
}
```

## DFS

![image-20220306142800075](D:\leetcode-Team\DataStructure\pictures\image-20220306142800075.png)

```java
private Stack<Vertex> stack = new Stack<>();


public void dfs(List<Vertex> vertexList){
    for(Vertex v: vertexList){
        if(!v.isVisited()){
            v.setVisited(true);
            dfsWithStack(v);
        }
    }
}


public void dfsWithStack(Vertex v){
    this.stack.add(v);
    v,setVisited();
    while(!stack.isEmpty()){
        Vertex cur = stack.pop();
        for(Vertex neighbor : cur.getNeighbor){
            if(!neighbor.visited){
                neighbor.setVisited();
                stack.add(neighbor);
            }
        }
    }
}
```

