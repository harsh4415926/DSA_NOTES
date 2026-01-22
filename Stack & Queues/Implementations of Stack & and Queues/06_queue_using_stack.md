# queue using stack 
````java
import java.util.Stack;

public class QueueUsingStack {
    private Stack<Integer> stack1; // Input stack
    private Stack<Integer> stack2; // Output stack

    public QueueUsingStack() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    // 1. ENQUEUE: Simply push to the input stack
    // Time Complexity: O(1)
    public void enqueue(int value) {
        stack1.push(value);
        System.out.println("Enqueued: " + value);
    }

    // 2. DEQUEUE: Pop from output stack
    // Time Complexity: Amortized O(1)
    public int dequeue() {
        if (isEmpty()) {
            System.out.println("Queue Underflow");
            return -1;
        }

        // If output stack is empty, move everything from input stack to it
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }

        return stack2.pop();
    }

    // 3. PEEK: View front element
    // Time Complexity: Amortized O(1)
    public int peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }

        // Same logic as dequeue: ensure stack2 has the data
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }

        return stack2.peek();
    }

    // 4. Utility: Check if both stacks are empty
    public boolean isEmpty() {
        return stack1.isEmpty() && stack2.isEmpty();
    }

    public static void main(String[] args) {
        QueueUsingStack queue = new QueueUsingStack();

        queue.enqueue(10); // stack1: [10]
        queue.enqueue(20); // stack1: [10, 20]
        queue.enqueue(30); // stack1: [10, 20, 30]

        // First dequeue triggers the transfer.
        // stack1 becomes [], stack2 becomes [30, 20, 10]. 
        // 10 is popped.
        System.out.println("Dequeued: " + queue.dequeue()); 

        // 20 is at top of stack2, so it is popped directly.
        System.out.println("Dequeued: " + queue.dequeue()); 

        queue.enqueue(40); // stack1: [40], stack2: [30] (remaining)
        
        System.out.println("Current Front: " + queue.peek()); // Returns 30
    }
}
````