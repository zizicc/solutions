# Cycle Detection
## LeetCode link
[207. Course Schedule](https://leetcode.com/problems/course-schedule/description/)
## Intuition
It is a classical cycle detection problem in a directed graph where each course is a node and each prerequisite is an edge. If there is a cycle in the graph, it means there exists contradiction, and one can not finish all courses.
## Approach
We use dfs to detect cycles.
- Graph representation: we use an adjacency list to represent the graph. Each course points to a list of courses that depend on it. Here we use a function called buildGraph.
- Cycle detection: For each traversal, we keep track of the path using a boolean array. If the next node which is about to be traversed exists on the path, a cycle is detected.
- Avoiding duplicated traversals, we keep track of the nodes which have been visited using a set.
## Q&A
- Q: Can I use a visited set solely to detect cycles?
  A: No. Think about the dependency: 0 -> 1 -> 3, and 0 -> 2 -> 3. First, you add 3 to visited. Then you detect the cycle by mistake when encountering 3 the second time.
## Complexity
- Time complexity:
- Space complexity:
## Code
- Java
```Java
class Solution {
    boolean can = true;
    List<Integer>[] graph;
    Set<Integer> visited;
    boolean[] onPath;
    int numCourses;
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        this.numCourses = numCourses;
        onPath = new boolean[numCourses];
        graph = buildGraph(prerequisites);
        visited = new HashSet<>();
        for (int i = 0; i < numCourses; ++i) {
            traverse(i);
            if (!can) return can;
        }
        return can;
    }
    List<Integer>[] buildGraph(int[][] prerequisites) {
        List<Integer>[] graph = new List[numCourses];
        for (int i = 0; i < numCourses; ++i) {
            graph[i] = new ArrayList<>();
        }
        for (int[] prerequisite : prerequisites) {
            int start = prerequisite[1];
            int end = prerequisite[0];
            graph[start].add(end);
        }
        return graph;
    }
    void traverse(int start) {
        if (!can) return;
        // The order of onPath check and visited check is very important.
        // Think about a cycle 0 -> 1 -> 0, if you add 0 to your visited
        // When you visit 0 the second time, you should check if it's onPath first
        // Then check if it's in the visited set to determine whether continue or not.
        if (onPath[start]) {
            can = false;
            return;
        }
        if (visited.contains(start)) return;
        visited.add(start);
        onPath[start] = true;
        for (int i : graph[start]) {
            traverse(i);
        }
        onPath[start] = false;
    }
}
```
