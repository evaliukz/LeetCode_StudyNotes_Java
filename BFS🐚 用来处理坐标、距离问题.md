## BFS🐚 的很多种用法



#### 314. Binary Tree Vertical Order Traversal
BFS可以实现把各个node放进坐标系里，从root(0,0)开始，每次往下都row+1, 每次往左下都col-1, 每次往右下都col+1; 这道题和row无关，只是往下走来计算column。

```
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        
        if (root == null) return result;
        
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<> (); // <column, list>
        
        Queue<Pair<Integer, TreeNode>> q = new LinkedList<>(); //pair也是<column, TreeNode>
        
        q.add(new Pair(0, root));
        
        int maxcol = Integer.MIN_VALUE;
        int mincol = Integer.MAX_VALUE;
        
        while(q.size()>0){
            
            int size = q.size();
            
            for (int i = 0; i < size; i++){
                
                Pair<Integer, TreeNode> head = q.poll();
                
                int col = head.getKey();
                TreeNode node = head.getValue();
                
                map.putIfAbsent(col, new ArrayList<Integer>()); 
                map.get(col).add(node.val);
                
                if (node.left != null){
                    q.add(new Pair (col-1,node.left));
                }
                
                if (node.right != null){
                    q.add(new Pair (col+1,node.right));
                }
                //这个很巧妙
                maxcol = Math.max(maxcol, col);
                mincol = Math.min(mincol, col);
            }
        }
        //注意要放j<=maxcol 而不是j<maxcol
        for(int j = mincol; j<= maxcol; j++){
            result.add(map.get(j));
        }
        return result;
    }
}
```
#### 863. All Nodes Distance K in Binary Tree

如果节点有指向父节点的引用，也就知道了距离该节点 1 距离的所有节点。之后就可以从 target 节点开始进行宽度优先搜索了。

```
class Solution {
    
    Map <TreeNode, TreeNode> parents; //放一个map指向parent节点
    Set <TreeNode> visited;
    List <Integer> result;
    
    private void getParent (TreeNode root, TreeNode parent){ //traverse一遍把parent都拿到
        
        if (root == null) return;
        
        parents.put(root, parent); //这个写的妙呀！
        
        getParent(root.left, root);
        getParent(root.right, root);
    }
    
    
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        
        parents = new HashMap<>();
        visited = new HashSet<>();
        result = new ArrayList<>();
        
        getParent(root,null); //先把所有的parent都放进map去
        
        Queue<TreeNode> q = new LinkedList<>();
        
        q.add(target); //target作为root加入BFS的queue
        
        int dist = 0; //用来track离target的距离
        
        while(q.size() > 0) {
            
            if (dist == K) {// 当distance等于k时候
                while(q.size() > 0){ //把q里面所有的元素都加入进result
                    result.add(q.poll().val);
                }
                continue;
            }
            dist ++;
    
            //处理每一层
            int size = q.size();
            
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                visited.add(node); //已经处理过了
                
                //System.out.println("test: "+node.val);
                //以target为中心，转圈+1：左儿子，右儿子，父节点
                if (node.left != null && !visited.contains(node.left)) {
                    q.add(node.left);
                }
                
                if (node.right != null && !visited.contains(node.right)){
                    q.add(node.right);
                }
                
                TreeNode p = parents.get(node);
                if (p!=null && !visited.contains(p)){
                    q.add(p);
                }
            }
        }
    return result;
    }
}

```

