# Stack using Arrays
````java
public class Stack {
    private int[] stackArray;
    private int top;
    private int capacity;

    // Constructor to initialize the stack
    public Stack(int size) {
        this.stackArray = new int[size];
        this.capacity = size;
        this.top = -1; // -1 indicates the stack is empty
    }

    // 1. PUSH: Add element to the top of the stack
    public void push(int value) {
        if (isFull()) {
            System.out.println("Stack Overflow! Cannot push " + value);
            return;
        }
        stackArray[++top] = value; // Increment top, then insert value
        System.out.println("Pushed " + value + " to stack");
    }

    // 2. POP: Remove element from the top of the stack
    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow! Cannot pop");
            return -1; // Returning -1 to indicate error (or throw exception)
        }
        return stackArray[top--]; // Return value, then decrement top
    }

    // 3. PEEK: Return the top element without removing it
    public int peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty");
            return -1;
        }
        return stackArray[top];
    }

    // 4. Utility: Check if stack is empty
    public boolean isEmpty() {
        return (top == -1);
    }

    // 5. Utility: Check if stack is full
    public boolean isFull() {
        return (top == capacity - 1);
    }

    // Main method to test the implementation
    public static void main(String[] args) {
        Stack stack = new Stack(5); // Create a stack of size 5

        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println("Top element is: " + stack.peek());

        System.out.println("Popped element: " + stack.pop());
        System.out.println("Popped element: " + stack.pop());

        if (stack.isEmpty()) {
            System.out.println("Stack is empty");
        } else {
            System.out.println("Stack is not empty");
        }
    }
}
````