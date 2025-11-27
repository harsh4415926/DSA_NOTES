# Dijkstra's Algorithm — Time Complexity (Teacher's Derivation)

## Key Steps
1. **General formula:**  
   `O(V * (pop from heap + edges_per_vertex * push into heap))`

2. **Heap operations:**  
   `O(V * (log(heap) + edges_per_vertex * log(heap)))`

3. **Worst case:** one node connects to all others → edges_per_vertex = V-1  
   `O(V * log(heap)  * (1+V-1)) =  O(V * log(heap) * V) = O(V^2 * log(heap))`

4. **Heap size worst case = V^2** = V-1  +  V-1  + .....V-1 →
   `O(V^2 * log(V^2)) = O(V^2 * 2logV)`

5. **Since E ≈ V^2 in dense graph:**  because each node connected to V-1 edges so total edges= (V-1)+(V-1)+.....=V^2 
   **Final TC = O(E * logV)**

---

## Final Complexities
- **Time Complexity:** `O(E * logV)`
- **Space Complexity:** `O(V + E)` (adjacency list + heap + dist array)

