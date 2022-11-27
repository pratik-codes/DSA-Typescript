# Queue

Queues are basically FIFO(First in first out) structure. They are made on top of a linked list.
How does a FIFO work?

`a -> b -> c -> d`

so if you want to add a new element to this queue you basically append it to the end. What we do is `d.next` become our new element and it is added in the queue.

`a -> b -> c -> d -> e`

Similarly removing a element means the head now points the second element and first

`b -> c -> d -> e`

All the operations on queue that is pushing and popping is a constant time.

### Implementation

```
interface Node<T> {
  value: T;
  next?: Node<T>;
}

export default class Queue<T> {
  public length: number;
  public head?: Node<T>;
  public tail?: Node<T>;

  constructor() {
    this.length = 0;
    this.head = this.tail = undefined;
  }

  enqueue(item: T): void {
    const node = { value: item, next: undefined };
    if (!this.tail) {
      this.head = this.tail = node;
      return;
    }
    this.tail.next = node;
    this.tail = node;
    this.length += 1;
    return;
  }

  dequeue(): T | undefined {
    if (!this.head) return undefined;
    const head = this.head;
    this.head = this.head?.next;
    head.next = undefined;
    this.length -= 1;
    return;
  }

  peek(): T | undefined {
    if (!this.head) return undefined;
    return this.head.value;
  }
}

const test_node = new Queue();

test_node.enqueue(1);
test_node.enqueue(2);
test_node.enqueue(3);
test_node.dequeue();
test_node.peek();
console.log(test_node.head);
console.log(test_node.tail);
console.log(test_node.length);
```
