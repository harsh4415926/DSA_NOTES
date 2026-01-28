# queue using stack 

## first approach (enqueue is expensive )
````java
import java.util.Stack;

public class QueueUsingStacks {
    private Stack<Integer> s1;
    private Stack<Integer> s2;

    public QueueUsingStacks() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }

    // Enqueue operation
    // Time Complexity: O(n) - We move elements to s2 and back to s1
    public void enQueue(int x) {
        // 1. Move all elements from s1 to s2
        while (!s1.isEmpty()) {
            s2.push(s1.pop());
        }

        // 2. Push item into s1
        s1.push(x);

        // 3. Push everything back to s1
        while (!s2.isEmpty()) {
            s1.push(s2.pop());
        }
        
        System.out.println("Enqueued: " + x);
    }

    // Dequeue operation
    // Time Complexity: O(1)
    public int deQueue() {
        if (s1.isEmpty()) {
            System.out.println("Queue is Empty");
            return -1;
        }

        // The top of s1 is always the front of the queue
        int popped = s1.pop();
        return popped;
    }
    
    // Peek operation (view front element)
    // Time Complexity: O(1)
    public int peek() {
        if (s1.isEmpty()) {
            System.out.println("Queue is Empty");
            return -1;
        }
        return s1.peek();
    }

    // Helper to check if queue is empty
    public boolean isEmpty() {
        return s1.isEmpty();
    }

    // Main method for testing
    public static void main(String[] args) {
        QueueUsingStacks q = new QueueUsingStacks();
        
        q.enQueue(10);
        q.enQueue(20);
        q.enQueue(30);

        System.out.println("Dequeued: " + q.deQueue()); // Should print 10
        System.out.println("Front element is: " + q.peek()); // Should print 20
        System.out.println("Dequeued: " + q.deQueue()); // Should print 20
    }
}
````

## second approach (dequeue and front is expensive)
````java
import java.util.Stack;

public class QueueUsingStacks {
    private Stack<Integer> inputStack;  // s1
    private Stack<Integer> outputStack; // s2

    public QueueUsingStacks() {
        inputStack = new Stack<>();
        outputStack = new Stack<>();
    }

    // Enqueue operation
    // Time Complexity: O(1)
    public void enQueue(int x) {
        inputStack.push(x);
        System.out.println("Enqueued: " + x);
    }

    // Dequeue operation
    // Time Complexity: Amortized O(1), Worst Case O(n)
    public int deQueue() {
        // If both stacks are empty, the queue is empty
        if (outputStack.isEmpty() && inputStack.isEmpty()) {
            System.out.println("Queue is Empty");
            return -1;
        }

        // If outputStack is empty, move elements from inputStack
        if (outputStack.isEmpty()) {
            while (!inputStack.isEmpty()) {
                outputStack.push(inputStack.pop());
            }
        }

        // Return the top of outputStack
        return outputStack.pop();
    }

    // Peek operation
    public int peek() {
        if (outputStack.isEmpty() && inputStack.isEmpty()) {
            System.out.println("Queue is Empty");
            return -1;
        }

        if (outputStack.isEmpty()) {
            while (!inputStack.isEmpty()) {
                outputStack.push(inputStack.pop());
            }
        }
        
        return outputStack.peek();
    }
    
    public boolean isEmpty() {
        return inputStack.isEmpty() && outputStack.isEmpty();
    }

    public static void main(String[] args) {
        QueueUsingStacks q = new QueueUsingStacks();
        
        q.enQueue(1);
        q.enQueue(2);
        q.enQueue(3);

        System.out.println("Dequeued: " + q.deQueue()); // Prints 1
        System.out.println("Dequeued: " + q.deQueue()); // Prints 2
        
        q.enQueue(4); // Push to input stack
        
        System.out.println("Dequeued: " + q.deQueue()); // Prints 3 (from output stack)
        System.out.println("Dequeued: " + q.deQueue()); // Prints 4 (transfer happens here)
    }
}
````