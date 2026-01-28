# asteroid collision
````java
class Solution {
    public int[] asteroidCollision(int[] arr) {
        Stack<Integer> st = new Stack<>();

        for (int a : arr) {

            // Pop smaller right-moving asteroids
            while (!st.isEmpty() && st.peek() > 0 && a < 0
                   && Math.abs(st.peek()) < Math.abs(a)) {
                st.pop();
            }

            // Equal size → both destroyed
            if (!st.isEmpty() && st.peek() > 0 && a < 0
                && Math.abs(st.peek()) == Math.abs(a)) {
                st.pop();
                continue;
            }

            // Stack top is larger → current destroyed
            if (!st.isEmpty() && st.peek() > 0 && a < 0
                && Math.abs(st.peek()) > Math.abs(a)) {
                continue;
            }

            st.push(a);
        }

        int[] ans = new int[st.size()];
        for (int i = ans.length - 1; i >= 0; i--) {
            ans[i] = st.pop();
        }

        return ans;
    }
}

````