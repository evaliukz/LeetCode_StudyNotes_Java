## BFS🐚 的很多种用法

BFS可以实现把各个node放进坐标系里，从root(0,0)开始，每次往下都row+1, 每次往左下都col-1, 每次往右下都col+1; 这道题和row无关，只是往下走来计算column。

#### 314. Binary Tree Vertical Order Traversal

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
