#  JavaScript 数据结构

## 栈

栈，是一种受限的线性表，后进先出（LIFO）。其限制是仅允许在表的一端进行插入和删除运算，这一端被称为栈顶，另一端被称为栈底。向一个栈插入新元素又叫进栈、入栈或压栈，它是把新元素放到栈顶元素的上面，使之成为新的栈顶元素。从一个栈删除元素又称为出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。

基于数组：

```JavaScript
class Stack {
  constructor() {
    this.items = [];
  }
  push(element) {
    this.items.push(element);
  }
  pop() {
    return this.items.pop();
  }
  peek() {
    return this.items[this.items.length - 1];
  }
  isEmpty() {
    return this.items.length === 0;
  }
  size() {
    return this.items.length;
  }
  toString() {
    return this.items.join(" ");
  }
}

//函数：十进制转为二进制
function dec2bin(dec) {
  const s = new Stack();
  while (dec > 0) {
    //获取余数，放入栈中
    s.push(dec % 2);
    //除以2，得到新的数
    dec = Math.floor(dec / 2);
  }
  //从栈中取出0,1
  let bin = "";
  while (!s.isEmpty()) {
    bin += s.pop().toString();
  }
  return bin;
}

```



## 队列

队列，它是一种受限的线性表，先进先出。受限之处在于它只允许在表的前端进行删除操作，而在表的后端进行插入操作。

基于数组：

```JavaScript
//队列类
class Queue {
  constructor() {
    this.items = [];
  }
  enqueue(element) {
    this.items.push(element);
  }
  dequeue() {
    return this.items.splice(0, 1)[0];
  }
  front() {
    return this.items[0];
  }
  isEmpty() {
    return this.items.length === 0;
  }
  size() {
    return this.items.length;
  }
  toString() {
    return this.items.join(",")
  }
}

//击鼓传花
function passGame(nameList, num) {
  const queue = new Queue();
  nameList.forEach(item => {
    queue.enqueue(item);
  });
  //开始数数字
  //不是num的时候，重新加入到队列的末尾
  //是num的时候，将其从队列中删除
  while (queue.size() > 1) {
    for (let i = 0; i < num - 1; i++) {
      queue.enqueue(queue.dequeue());
    }
    //num对应的这个人，直接从队列中去除掉
    queue.dequeue()
  }
  return queue.front();
}
console.log(passGame(["a", "b", "c"],4));
```

##  优先级队列

优先级队列，在插入一个元素的时候会考虑该数据的优先级，和其他数据的优先级进行比较，比较完成后，可以得出这个元素在队列中的正确位置。

基于数组：

```JavaScript
//优先级队列类
class priorityQueue {
  constructor() {
    this.items = [];
  }
  enqueue(element, priority) {
    let queueElement = new QueueElement(element, priority);
    if (this.items.length === 0) {
      this.items.push(queueElement);
    } else {
      let added = false;
      for (let i = 0; i < this.items.length; i++) {
        if (queueElement.priority < this.items[i].priority) {
          this.items.splice(i, 0, queueElement);
          added = true;
          break;
        }
      }
      if (!added) {
        this.items.push(queueElement);
      }
    }
  }
  dequeue() {
    return this.items.splice(0, 1)[0];
  }
  front() {
    return this.items[0];
  }
  isEmpty() {
    return this.items.length === 0;
  }
  size() {
    return this.items.length;
  }
  toString() {
    let str = "";
    for (let i = 0; i < this.items.length; i++) {
      str += this.items[i].element + " ";
    }
    return str;
  }
}
class QueueElement {
  constructor(element, priority) {
    this.element = element;
    this.priority = priority;
  }
}
const pq = new priorityQueue();
pq.enqueue("John", 2);
pq.enqueue("Jack", 1);
pq.enqueue("Camila", 1);
pq.enqueue("Camila", 5);
pq.enqueue("Camila", 3);
console.log(pq);
```



## 单向链表

链表的基本思维是，利用结构体的设置，额外开辟出一份内存空间去作指针，它总是指向下一个结点，一个个结点通过NEXT指针相互串联，就形成了链表。

```JavaScript
//单向链表
class Node {
  constructor(value, next = null) {
    this.value = value;
    this.next = next;
  }
}
class LinkedList {
  constructor(value) {
    this.head = new Node(value);
  }
  findNode(value) {
    // 当前节点，默认指向头部节点
    let currentNode = this.head;
    //当前节点的value不等于参数value时，循环，并且currentNode指向下一个节点
    while (currentNode.value !== value) {
      currentNode = currentNode.next;
    }
    //如果currentNode返回了null，说明找到了尾部节点的next，结果就返回null
    if (!currentNode) {
      return null;
    }
    //如果currentNode返回了节点，说明找到了节点，结果就返回节点
    return currentNode;
  }
  insertAfter(value, newValue) {
    //创建一个值为newValue的新节点
    const newNode = new Node(newValue);
    if (!this.findNode(value)) return false;
    //找到值为value的节点，以便在其后面插入新节点
    const currentNode = this.findNode(value);
    //新节点的next指向当前节点的next（当前节点的后继结点）
    newNode.next = currentNode.next;
    //当前节点的next指向新节点
    currentNode.next = newNode;
  }
  append(value) {
    //创建一个值为newValue的新节点
    const newNode = new Node(value)
    // 当前节点，默认指向头部节点
    let currentNode = this.head;
    //当前节点的next不等于null时，循环，并且currentNode指向下一个节点
    while (currentNode.next) {
      currentNode = currentNode.next;
    }
    //此时currentNode指向最后一个节点，将其next指向新节点
    currentNode.next = newNode;
  }
  prepend(value) {
    //创建一个值为newValue的新节点
    const newNode = new Node(value)
    //新节点的next指向head指针指向的节点（当前的头部节点）
    newNode.next = this.head;
    // head指针再指向新节点
    this.head = newNode;
  }
  remove(value) {
    // 当前节点，默认指向头部节点
    let currentNode = this.head;
    //默认前置节点为空
    let previousNode = null;
    while (currentNode.value !== value) {
      previousNode = currentNode;
      currentNode = currentNode.next;
    }
    // 如果要删除的节点（currentNode）就是头部节点，则将head指针指向下一个节点
    if (currentNode === this.head) {
      this.head = currentNode.next;
    } else {
      // 否则，将前置节点的next指向当前节点的next
      previousNode.next = currentNode.next;
    }
  }
  removeHead() {
    this.head = this.head.next;
  }
  removeTail() {
    // 当前节点，默认指向头部节点
    let currentNode = this.head;
    //默认前置节点为空
    let previousNode = null;
    while (currentNode.next) {
      previousNode = currentNode;
      currentNode = currentNode.next;
    }
    previousNode.next = null;
  }
  // 遍历链表
  traverse() {
    let currentNode = this.head;
    while (currentNode) {
      console.log(currentNode.value);
      currentNode = currentNode.next;
    }
  }
}

const list = new LinkedList(1);
list.append(2);
list.append(3);
list.append(4);

list.insertAfter(2, 5);
list.prepend(6);
list.remove(3);
list.removeHead();
list.removeTail();
list.traverse()
```

