# stack using queue
````java
import java.util.LinkedList;
import java.util.Queue;

public class StackUsingQueue {
    // We use a Queue to store elements
    private Queue<Integer> queue;

    public StackUsingQueue() {
        queue = new LinkedList<>();
    }

    // 1. PUSH: Add element and rotate queue to make it the front
    // Time Complexity: O(n)
    public void push(int value) {
        // Add the new element to the rear of the queue
        queue.add(value);
        
        // Rotate the queue: Move all previous elements (size - 1) 
        // from front to rear, so the new element comes to the front.
        int size = queue.size();
        for (int i = 0; i < size - 1; i++) {
            queue.add(queue.remove());
        }
        
        System.out.println("Pushed: " + value);
    }

    // 2. POP: Remove the top element
    // Time Complexity: O(1)
    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow");
            return -1;
        }
        // Because of our rotation in push(), the 'top' of the stack 
        // is actually the 'front' of the queue.
        return queue.remove();
    }

    // 3. PEEK: View the top element
    // Time Complexity: O(1)
    public int peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty");
            return -1;
        }
        return queue.peek();
    }

    // 4. Utility: Check if empty
    public boolean isEmpty() {
        return queue.isEmpty();
    }

    public static void main(String[] args) {
        StackUsingQueue stack = new StackUsingQueue();

        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println("Top element is: " + stack.peek()); // Should be 30

        System.out.println("Popped: " + stack.pop()); // Removes 30
        System.out.println("Popped: " + stack.pop()); // Removes 20

        System.out.println("Top element is: " + stack.peek()); // Should be 10
    }
}
````
