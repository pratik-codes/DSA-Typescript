# Heap

Heap is also know as priority queue. There are two types of heaps

1. Min Heap

   - Its root node has the lowest number and then all the childrens are higher than the parent.
   - Its root node has the highest and all children lower than the parent.

2. Max Heap
   - Which is exactly opposite to it

### Construction of a Heap

```typescript
export class minHeap {
  length: number;
  data: Array<number>;

  constructor() {
    this.length = 0;
    this.data = [];
  }

  insert(value: number) {
    this.data[this.length] = value;
    this.heapifyUp(this.length);
    this.length++;
  }

  delete() {
    this.length--;

    const out = this.data[0];
    if (this.length === 0) {
      this.data = [];
      return out;
    }

    this.data[0] = this.data[this.length];
    this.heapifyDown(0);
  }

  private heapifyDown(idx: number) {
    let leftCIdx = this.leftChild(idx);
    let rightCIdx = this.rightChild(idx);
    let leftC = this.data[leftCIdx];
    let rightC = this.data[rightCIdx];
    let current = this.data[idx];

    if (idx >= this.length || leftC >= this.length) {
      return;
    }

    if (leftC > rightC && current < leftC) {
      this.data[idx] = leftC;
      this.data[leftCIdx] = current;
      this.heapifyDown(leftCIdx);
    }

    if (leftC < rightC && this.data[idx] < this.data[rightC]) {
      this.data[idx] = rightC;
      this.data[rightCIdx] = current;
      this.heapifyDown(rightCIdx);
    }
  }

  private heapifyUp(idx: number) {
    if (idx === 0) return undefined;

    let parent = this.data[this.parent(idx)];
    let curr = this.data[idx];

    if (parent > curr) {
      //swap the elements and heapifyup
      this.data[idx] = parent;
      this.data[this.parent(idx)] = curr;
      this.heapifyUp(curr);
    }
  }

  private parent(idx: number) {
    return Math.floor((idx - 1) / 2);
  }

  private leftChild(idx: number) {
    return idx * 2 + 1;
  }

  private rightChild(idx: number) {
    return idx * 2 + 2;
  }
}
```
