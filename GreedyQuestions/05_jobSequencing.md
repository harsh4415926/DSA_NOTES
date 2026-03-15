# job sequencing (gfg)
## greedy
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Job Sequencing Problem
 * ----------------------------------------------------------------------
 * LOGIC (Greedy):
 * 1. Sort jobs in descending order of PROFIT.
 * 2. Find max deadline to create a time-slot tracking array.
 * 3. Schedule jobs: For each job, assign the latest possible free slot 
 * (iterate backwards from its deadline to day 1). 
 * - Why backwards? Leaves earlier slots open for jobs with tighter deadlines.
 * ----------------------------------------------------------------------
 */

class Job {
    int deadline;
    int profit;
    Job(int deadline, int profit) {
        this.deadline = deadline;
        this.profit = profit;
    }
}

class Solution {
    public ArrayList<Integer> jobSequencing(int[] deadline, int[] profit) {
        Job[] jobs = new Job[deadline.length];
        
        // Combine arrays into Job objects for sorting
        for(int i = 0; i < deadline.length; i++) {
            jobs[i] = new Job(deadline[i], profit[i]);
        }
        return helper(jobs);
    }
    
    public ArrayList<Integer> helper(Job[] jobs) {
        // Sort greedily: Highest profit first
        Arrays.sort(jobs, (a, b) -> b.profit - a.profit);
        
        // Find max deadline to size the scheduling array
        int maxDay = Integer.MIN_VALUE;
        for(int i = 0; i < jobs.length; i++) {
            maxDay = Math.max(maxDay, jobs[i].deadline);
        }
        
        boolean[] dayCovered = new boolean[maxDay + 1]; // Tracks filled slots
        int count = 0;
        int totalProfit = 0;
        
        for(Job job : jobs) {
            // Find latest available slot from deadline down to day 1
            for(int slot = job.deadline; slot > 0; slot--) {
                if(!dayCovered[slot]) {
                    dayCovered[slot] = true; // Mark slot filled
                    count++;
                    totalProfit += job.profit;
                    break; // Move to next job once scheduled
                }
            }
        }
        
        ArrayList<Integer> ans = new ArrayList<>();
        ans.add(count);
        ans.add(totalProfit);
        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) (Sorting) + O(N * M) where M is max deadline (Nested loops).
 * Space: O(N) (Job array) + O(M) (Tracking array).
 */
````

## optimising using DSU(Greedy+DSU)
````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: Job Sequencing Problem (Disjoint Set Optimization)
 * ----------------------------------------------------------------------
 * LOGIC (Greedy + DSU):
 * 1. Sorting (Greedy): Sort jobs by profit (descending).
 * 2. DSU Optimization: Instead of linearly scanning for an empty slot, 
 * use Union-Find to jump directly to the latest available slot.
 * - parent[i] tracks the latest available slot on or before day 'i'.
 * - Initially, parent[i] = i (every slot is available).
 * 3. Schedule: Find available slot via `find(deadline)`.
 * - If slot > 0, schedule it.
 * - Mark slot filled by linking it to the slot before it: `union(slot, slot - 1)`.
 * - Path compression in `find` makes future lookups nearly O(1).
 * ----------------------------------------------------------------------
 */

class DisjointSet {
    int[] parent;
    
    DisjointSet(int n) {
        parent = new int[n + 1];
        // Initially, the greatest available slot on or before 'i' is 'i' itself.
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
        }
    }
    
    // Path Compression: Flattens the tree, linking nodes directly to the root (available slot)
    public int find(int node) {
        if (parent[node] == node) return node;
        return parent[node] = find(parent[node]); 
    }
    
    // Links 'u' to 'v'. In this context, we link a filled slot to the slot before it.
    public void union(int u, int v) {
        parent[u] = v;
    }
}

class Job {
    int deadline;
    int profit;
    Job(int deadline, int profit) {
        this.deadline = deadline;
        this.profit = profit;
    }
}

class Solution {
    public ArrayList<Integer> jobSequencing(int[] deadline, int[] profit) {
        Job[] jobs = new Job[deadline.length];
        for (int i = 0; i < deadline.length; i++) {
            jobs[i] = new Job(deadline[i], profit[i]);
        }
        return helper(jobs);
    }
    
    public ArrayList<Integer> helper(Job[] jobs) {
        // Sort highest profit first
        Arrays.sort(jobs, (a, b) -> b.profit - a.profit);
        
        int maxDay = Integer.MIN_VALUE;
        for (int i = 0; i < jobs.length; i++) {
            maxDay = Math.max(maxDay, jobs[i].deadline);
        }
        
        DisjointSet ds = new DisjointSet(maxDay);
        int count = 0;
        int totalProfit = 0;
        
        for (Job job : jobs) {
            // O(1) amortized lookup for the latest free slot
            int par = ds.find(job.deadline); 
            
            // If a valid slot (day > 0) is available
            if (par > 0) {
                count++;
                totalProfit += job.profit;
                
                // Mark 'par' as filled by pointing it to 'par - 1'
                // Next time someone looks for 'par', find() will route them to 'par - 1'
                ds.union(par, par - 1); 
            }
        }
        
        ArrayList<Integer> ans = new ArrayList<>();
        ans.add(count);
        ans.add(totalProfit);
        return ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(N log N) (Sorting) + O(N * α(M)) (DSU ops, where M is max deadline). α is Inverse Ackermann (effectively O(1)).
 * Space: O(N) (Jobs array) + O(M) (DSU parent array).
 */
````