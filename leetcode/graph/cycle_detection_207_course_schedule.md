# Cycle Detection
## LeetCode link
[LeetCode](https://leetcode.com/problems/course-schedule/description/)
## Intuition
It is a classical cycle detection problem in a directed graph where each course is a node and each prerequisite is an edge. If there is a cycle in the graph, it means there exists contradiction, and one can not finish all courses.
## Approach
We use dfs to detect cycles.
- Graph representation: we use an adjacency list to represent the graph. Each course points to a list of courses that depend on it. Here we use a function called buildGraph.
- Cycle detection: For each traversal, we keep track of the path using a boolean array. If the next node which is about to be traversed exists on the path, a cycle is detected.
- Avoid duplicated traversals, we keep track of the nodes which have been visited using a set.
## Complexity
- Time complexity:
- Space complexity:
## Code
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
        if (visited.contains(start)) return;
        if (!can) return;
        if (onPath[start]) {
            can = false;
            return;
        }
        onPath[start] = true;
        for (int i : graph[start]) {
            traverse(i);
        }
        onPath[start] = false;
        visited.add(start);
    }
}
