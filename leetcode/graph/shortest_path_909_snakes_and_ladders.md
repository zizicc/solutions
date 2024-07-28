# Find the Shortest Path by BFS
## LeetCode link
[909. Snakes and Ladders](https://leetcode.com/problems/snakes-and-ladders/description/)
## Intuition
In this problem, we start from node 1, and calculate the shortest path to reach node n*n. We can use bfs to forward step by step, once reach the destination, we return the current step count.
## Approach 1
- Use a queue to store the nodes of the current step to be visited, traverse each node to find the next nodes and add them to the queue.
- Once the current step has been visited, step++.
- Use a boolean array to store the nodes which have been visited, to avoid duplicated visits.
- The function returns steps count once the node n * n is reached or returns -1 if forwarding is impossible.
## Complexity
- Time complexity
- Space complexity
## Code 1
**Pseudo code**
```Java
class Solution {
    public int bfs(int src, int dst) {
        Queue<Integer> q = new LinkedList<>();
        Set<Integer> visited = new HashSet<>();
        q.offer(src);
        visited.add(src);
        int step = 0;
        while (!q.isEmpty()) {
            int sz = q.size();
            for (int i = 0; i < sz; ++i) {
                int curr = q.poll();
                if (curr == dst) return step;
                visited.add(curr);
                for (each of the next nodes) {
                    //check if visited
                    if (!visited.contains(next)) {
                        q.offer(next);
                        visited.add(next);
                    }
                }
            }
            step++;
        }
        return -1;
    }
}
```
**Actual code**
```Java
class Solution {
    public int snakesAndLadders(int[][] board) {
        int n = board.length;
        int dst = n * n;
        Queue<Integer> q = new LinkedList<>();
        boolean[] visited = new boolean[n * n + 1];
        q.offer(1);
        visited[1] = true;
        int step = 0;
        while (!q.isEmpty()) {
            int sz = q.size();
            for (int i = 0; i < sz; ++i) {
                int curr = q.poll();
                if (visited[curr]) continue;
                visited[curr] = true;
                if (curr == dst) return step;
                for (int j = 1; j <= 6; ++j) {
                    int next = curr + j;
                    //why should not check visited[next] here
                    //because it's not the actual next node
                    if (next > dst) continue;
                    int quo = (next - 1) / n;
                    int r = (next - 1) % n;
                    int m = n - 1 - quo;
                    //int k = r % 2 == 0 ? next - 1 - r * n : n + r * n - next;
                    int k = (n + m) % 2 == 1 ? r : n - 1 - r;
                    if (board[m][k] != -1) {
                        next = board[m][k];
                    }
                    if (!visited[next]) {
                        q.offer(next);
                        visited[next] = true;
                    }
                }
            }
            step++;
        }
        return -1;
    }
}
```
## Approach 2
- Use a queue to store the nodes to be visited.
- Use a map to store each visited node and the corresponding step count to reach it.
- Visit each node in the queue, when add its next node, get its correponding step and plus one.
## Code 2
**Pseudo code**
```Java
class Solution {
    public int bfs(int src, int dst) {
        Map<Integer,Integer> hm = new HashMap<>();
        //the number of steps taken is 0
        hm.put(src, 0);
        Queue<Integer>q = new LinkedList<>();
        q.add(src);
        while(!q.isEmpty()){
            int curr = q.poll();
            if(curr == dst) return hm.get(curr);
            for(each next node){
                /*normal BFS*/
                if(!hm.containsKey(next)){
                    hm.put(next, hm.get(curr) + 1);
                    q.offer(next);
                }
            }
        }
        return -1;
    }
}
```
**Actual code**  
Referenced from: [@shaikhu3421](https://leetcode.com/problems/snakes-and-ladders/solutions/3092515/java-easy-understanding-bfs-simple-with-comments/)
```Java
class Solution {
    public int snakesAndLadders(int[][] board) {
        int n=board.length;
        Map<Integer,Integer>hm=new HashMap<>();
        hm.put(1,0);//as we are starting from 1st position so we are adding 1 and the number of steps taken are 0
        Queue<Integer>q=new LinkedList<>();
        q.add(1);//for starting bfs
        while(!q.isEmpty()){
            int p=q.poll();
            if(p == n*n) return hm.get(p);
            for(int i= p + 1; i <= Math.min(p + 6, n * n); i++){
                int next = check(i,n);//getting the next most suitable position to jump
                int row = next/n,col = next%n;
                int ns = board[row][col] == -1 ? i : board[row][col];
                /*normal BFS*/
                if(!hm.containsKey(ns)){
                    hm.put(ns, hm.get(p) + 1);
                    q.offer(ns);
                }
            }
        }
        return -1;
    }
    public static int check(int i, int n){
        int q = (i - 1)/n, r = (i-1)%n;
        int row = n - 1 - q;
        int col = row % 2 != n % 2 ? r : n - 1 - r;
        return row * n + col;
    }
}
```
