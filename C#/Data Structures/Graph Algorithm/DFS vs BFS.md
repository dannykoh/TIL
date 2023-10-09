# DFS vs BFS

- **DFS: Depth-First Search 깊이 우선 탐색**

    DFS란 깊이 우선 탐색의 약자로, 그래프/트리 탐색 기법이다. 한 노드에서 시작해서 다음 분기로 넘어가기 전에 해당 분기를 끝까지 탐색한다.

    - Best Use Case

        - 모든 노드를 방문하고자 하는 경우
        - 깊이 우선 탐색(DFS)가 너비우선탐색(BFS)보다 간단하다.
        - 단순 검색 속도 자체는 너비우선탐색(BFS)보다 느리다.

    - 특징
        - 자기 자신을 호출하는 순환 알고리즘의 형태를 가지고 있다.
        - 어떤 노드를 방문했었는지를 반드시 검사해야 한다.<br> 검사 하지않으면 무한루프에 빠질 수 있음.

    - 구현
        ```csharp
        class Graph
        {
            int[,] adj = new int[6, 6]
            {
                { 0, 1, 0, 1, 0, 0 },
                { 1, 0, 1, 1, 0, 0 },
                { 0, 1, 0, 0, 0, 0 },
                { 1, 1, 0, 0, 0, 0 },
                { 0, 0, 0, 0, 0, 1 },
                { 0, 0, 0, 0, 1, 0 }
            }; //행렬 형태의 그래프

            List<int>[] adj2 = new List<int>[]
            {
                new List<int>() { 1, 3 },
                new List<int>() { 0, 2, 3 },
                new List<int>() { 1 },
                new List<int>() { 0, 1 },
                new List<int>() { 5 },
                new List<int>() { 4 }
            }; //리스트 자료형으로 구현한 그래프

            bool[] visited = new bool[6];
            // 1) 우선 now부터 방문하고,
            // 2) now와 연결된 정점들을 하나씩 확인해서 아직 미방문한 상태라면 방문한다.
            public void DFS(int now)
            {
                Console.WriteLine(now);
                visited[now] = true; // now 방문

                for (int next = 0; next < 6; next++)
                {
                    if (adj[now, next] == 0) // 연결되어 있지 않은 노드 스킵
                        continue;
                    if (visited[next]) // 이미 방문한 노드 스킵
                        continue;
                    DFS(next);
                }
            } //행렬 형태의 그래프 탐색 함수

            public void DFSList(int now)
            {
                Console.WriteLine(now);
                visited[now] = true; // now 방문

                foreach(int next in adj2[now])
                {
                    if (visited[next]) // 이미 방문한 노드 스킵
                        continue;
                    DFSList(next);
                }
            } //리스트 형태의 그래프 탐색 함수

            public void SearchAll()
            {
                visited = new bool[6];
                for(int now = 0; now < 6; now++)
                {
                    if (visited[now] == false)
                        DFS(now);
                } 
            } //그래프가 중간에 끊겨있을 경우를 대비해 모든 노드를 탐색하는 알고리즘
        }

        ```


- **BFS: Breadth-First Search 너비 우선 탐색**

    **너비우선탐색** - 루트 노드(혹은 다른 임의의 노드)에서 시작해서 인접한 노드를 먼저 탐색하는 방법

    - 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법이다. 
    - 즉, 깊게(deep) 탐색하기 전에 넓게(wide) 탐색하는 것이다.
    - Best Use Case 두 노드 사이의 최단 경로 혹은 임의의 경로를 찾고 싶을 때 이 방법을 선택한다.
    
    **BFS의 특징**
    - 직관적이지 않은면이 있다.
    - 재귀적으로 작동하지 않는다.
    - 그래프 탐색의 경우 어떤 노드를 방문했었는지 여부를 반드시 검사 해야 한다.
    - BFS는 방문한 노드들을 차례로 저장한 후 꺼낼 수 있는 자료 구조인 큐(Queue)를 사용한다.

    **구현**
    
```csharp
    using System;

    namespace Exercise
    {
        // Stack : LIFO (Last In, First Out)
        // Queue : FIFO (First In, First Out)

        class Graph
        {
            int[,] adj = new int[6, 6]
            {
                {0, 1, 0, 1, 0, 0 },
                {1, 0, 1, 1, 0, 0 },
                {0, 1, 0, 0, 0, 0 },
                {1, 1, 0, 0, 1, 0 },
                {0, 0, 0, 1, 0, 1 },
                {0, 0, 0, 0, 1, 0 },
            };

            List<int>[] adj2 = new List<int>[]
            {
                new List<int>() { 1, 3 },
                new List<int>() { 0, 2, 3 },
                new List<int>() { 1 },
                new List<int>() { 0, 1, 4 },
                new List<int>() { 3, 5 },
                new List<int>() { 4 }
            };

            public void BFS(int start)
            {
                // Check if eash node is visited
                bool[] found = new bool[6];

                // Start point
                Queue<int> q = new Queue<int>();
                q.Enqueue(start);
                found[start] = true;

                while (q.Count > 0)
                {
                    int current = q.Dequeue();
                    Console.WriteLine(current);

                    for (int next = 0; next < 6; next++)
                    {
                        if (adj[current, next] == 0) // No node connection
                            continue;
                        if (found[next] == true) // Next node already found
                            continue;

                        // Add next to the Queue because, at this point, we are sure that it's never been found.
                        q.Enqueue(next);
                        found[next] = true;
                    }
                }
            }
        }

        class Program
        {
            static void Main(string[] args)
            {
                // BFS (Breadth First Search - 너비 우선 탐색)
                Graph graph = new Graph();
                graph.BFS(0);
                Console.Read();
            }
        }
    }
    ```