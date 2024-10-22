1. **Stack:** ![Stack GIF](https://upload.wikimedia.org/wikipedia/commons/b/b4/Lifo_stack.png)

    Trong GIF này, bạn có thể thấy các phần tử được đẩy vào và lấy ra khỏi stack theo nguyên tắc LIFO (Last-In-First-Out). Phần tử cuối cùng được đẩy vào (được thêm vào trên cùng) sẽ được lấy ra đầu tiên.

2. **Queue:**

 ![Queue GIF](https://upload.wikimedia.org/wikipedia/commons/5/52/Data_Queue.svg)
Trong GIF này, bạn có thể thấy các phần tử được đẩy vào và lấy ra khỏi hàng đợi theo nguyên tắc FIFO (First-In-First-Out). Phần tử được đẩy vào đầu tiên sẽ được lấy ra đầu tiên cũng.


# Example
```CSharp
using System;
using System.Collections;

class Program
{
    static void Main(string[] args)
    {
        // Creating a Queue
        Queue myQueue = new Queue();

        // Enqueue elements into the Queue
        myQueue.Enqueue("Apple");
        myQueue.Enqueue("Banana");
        myQueue.Enqueue("Orange");

        // Dequeue elements from the Queue
        while (myQueue.Count > 0)
        {
            string item = (string)myQueue.Dequeue();
            Console.WriteLine("Dequeued item: " + item);
        }
    }
}
```

```CSharp
using System;
using System.Collections;

class Program
{
    static void Main(string[] args)
    {
        // Creating a Stack
        Stack myStack = new Stack();

        // Push elements into the Stack
        myStack.Push("Apple");
        myStack.Push("Banana");
        myStack.Push("Orange");

        // Pop elements from the Stack
        while (myStack.Count > 0)
        {
            string item = (string)myStack.Pop();
            Console.WriteLine("Popped item: " + item);
        }
    }
}

```