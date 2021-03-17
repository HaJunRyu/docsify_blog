# 양방향 연결 리스트 (Doubly Linked List)
`value`, `next`, 그리고 `prev` property를 지닌 `Node` 클래스를 구현한다.
`Node`를 이용하여 Doubly Linked List를 구현한다.
다음과 같은 리스트 ADT의 연산자를 구현해야 한다.
- 비어있는 리스트를 생성하는 생성자
- 리스트가 비어있는지 확인하는 연산자
- 리스트의 앞에 개체를 삽입(prepending)하는 연산자
- 리스트의 뒤에 개체를 삽입(appending)하는 연산자
- 리스트의 첫 머리(head)를 결정하는 연산자
- 주어진 인덱스에 해당하는 요소에 접근하는 연산자
- 주어진 인덱스에 새로운 요소를 삽입하는 연산자
- 주어진 인덱스에 해당하는 요소를 제거하는 연산자

## 작성한 코드

```javascript
class Node {
  constructor(value, prev, next) {
    this.value = value;
    this.prev = prev;
    this.next = next;
  }
}

class DoublyLinkedList {
  // 처음 new DoublyLinkedList를 했을때 head와 tail값은 null로 생성된다.
  constructor() {
    this.head = null; // 시작점
    this.tail = null; // 끝점
  }

  isEmpty() {
    return this.head === null;
  }

  // 새로운 Node를 만들어 List의 시작점인 head로 설정해준다. 그리고 현재 시작점인 head를 next값으로 넣어준다.
  prepend(value) {
    const newNode = new Node(value, null, this.head);

    // prepend할때 List가 비어있다면 newNode가 head이자 tail이 된다.
    if (this.isEmpty()) this.tail = newNode;
    // prepend할때 head.next가 null이라면 즉, List의 size가 1이라면 현재 head를 tail로 설정
    else if (this.head.next === null) this.tail = this.head;

    this.head.prev = newNode;
    this.head = newNode;
  }

  append(value) {
    let curr = this.head;
    // List가 비었다면 새로운 node를 생성해 head로 설정해주고 next와 prev는 당연히 둘 다 null이다.
    if (curr === null) {
      const newNode = new Node(value, null, null);
      this.head = newNode;
      this.tail = newNode;
      return;
    }

    // List의 마지막 지점인 tail에 새로운 Node를 만들어 할당, 기존의 tail은 새로운 tail Node의 prev로 가르키고 있음
    const newNode = new Node(value, this.tail, null);
    // 기존 tail의 next값을 새로운 tail이 될 새로운 Node로 설정
    this.tail.next = newNode;
    // tail변경
    this.tail = newNode;
  }

  // index를 받아 head(List의 시작점)를 임의로 변경
  // setHead에서 boolean값을 반환하는 이유 질문하기
  setHead(index) {
    let curr = this.head;

    // 파라미터로 전달 된 index만큼 for문을 돌며 head로 설정 할 Node를 curr변수에 할당
    // <= 이 아닌 < 를 써주는 이유는 만약 0을 파라미터로 넣어준다고 했을때 <=을 써준다면 반복문을 1회 실행하며 1번 index로 head가 변경 될 것이기 때문
    for (let i = 0; i < index; i++) {
      // for문안에 들어왔다는건 index값을 1 이상 줬다는것, 하지만 null이라면 List가 비었다는 뜻이기 때문에 없는 index에 접근했으니 에러를 반환
      if (curr === null) return new ReferenceError('List index out of Bounds error');

      // for문을 돌며 만약 index값이 6이라면 6번째 순서(5번 index)의 Node를 curr변수에 담아줌
      curr = curr.next;
    }

    // curr 변수에 담긴 Node를 List의 head로 설정
    this.head = curr;
    return true;
  }

  // 파라미터로 전달받은 해당 index에 접근하기
  access(index) {
    let curr = this.head;

    // 파라미터로 전달받은 index 만큼 반복하며 curr의 값을 curr.next의 참조값으로 갱신시켜 준다.
    for (let i = 0; i < index; i++) {
      // for문 안에 들어왔다면 index값을 1 이상 줬다는것, 하지만 curr가 null이라면 List가 비었다는 뜻이기 때문에 없는 index에 접근했으니 에러를 반환
      // curr.next를 하다가 null이 되는 상황도 동일.
      if (curr === null) return new ReferenceError('List index out of Bounds error');
      curr = curr.next;
    }

    return curr.value;
  }

  // 특정 index에 값을 삽입, 원래 해당 index에 있던 Node는 next로 밀리게 된다.
  insert(index, value) {
    // 만약 0번째 index에 값을 넣고 싶은것이라면 prepend를 하는것과 같은 동작
    if (index === 0) {
      this.prepend(value);
      return true;
    }

    if (index === this.size()) {
      this.append(value);
      return true;
    }

    let curr = this.head;
    let prev = null;

    // 첫번째 파라미터로 전달받은 index값 만큼 for문을 순회하며 추가 할 Node의 위치를 결정
    for (let i = 0; i < index; i++) {
      if (curr === null) return new ReferenceError('List index out of Bounds error');

      // for문의 마지막에 추가하고 싶은 Node의 이전 위치가 prev변수에 저장됨
      prev = curr;
      // for문의 마지막에 추가하고 싶은 Node의 위치가 curr에 담기게 됨
      curr = curr.next;
    }
    // 추가 할 위치의 전 Node의 next가 새로운 Node를 가르키게 해주고 추가 할 위치의 Node의 prev가 새로운 Node를 가리키게 함
    const newNode = new Node(value, prev, curr);
    prev.next = newNode;
    curr.prev = newNode;
    return true;
  }

  remove(index) {
    // 파라미터로 전달받은 index의 값이 0이라면
    if (index === 0) {
      // 1. 하지만 List가 비어있지 않다면현재 헤드의 참조를 다음 헤드로 바꾸어 참조값을 잃어버리게 한 후 true를 반환하며 메서드를 종료한다.
      if (this.head !== null) {
        this.head = this.head.next;
        return true;
      }
      // 2. 하지만 List가 비었다면 false를 반환하며 remove 메서드를 종료한다.
      else return false;
    }

    let curr = this.head;
    let prev = null;

    for (let i = 0; i < index; i++) {
      if (curr === null) return new ReferenceError('List index out of Bounds error');

      // for문의 마지막에 지우고 싶은 Node의 이전 위치가 prev변수에 저장됨
      prev = curr;
      // for문의 마지막에 지우고 싶은 Node의 위치가 curr에 담기게 됨
      curr = curr.next;
    }
    // 삭제하고 싶은 Node의 이전 Node.next값을 삭제하고 싶은 Node의 다음 Node를 가리키게 함.
    prev.next = curr.next;
    // 삭제하고 싶은 Node의 다음 Node.prev값을 삭제하고 싶은 Node의 이전 Node를 가리키게 함.
    curr.next.prev = prev;
  }

  print() {
    let curr = this.head;

    // List가 비었을때 console창에 빈 배열을 출력 후 메서드 종료
    if (curr === null) {
      console.log('[]');
      return;
    }

    let result = '';
    while (curr !== null) {
      result += `${curr.value} `;
      curr = curr.next;
    }
    console.log(`[${result}]`);
  }

  // 거꾸로 출력?
  printInv() {
    let curr = this.tail;

    // List가 비었을때 console창에 빈 배열을 출력 후 메서드 종료
    if (curr === null) {
      console.log('[]');
      return;
    }

    let result = '';
    while (curr !== null) {
      result += `${curr.value} `;
      curr = curr.prev;
    }
    console.log(`[${result}]`);
  }

  size() {
    let curr = this.head;
    let listSize = 0;
    while (curr !== null) {
      listSize++;
      curr = curr.next;
    }
    return listSize;
  }
}

// 테스트 케이스

const myList = new DoublyLinkedList();
myList.print(); // []

for (let i = 0; i < 10; i++) {
  myList.append(i + 1);
}
myList.print(); // [1 2 3 4 5 6 7 8 9 10 ]

for (let i = 0; i < 10; i++) {
  myList.prepend(i + 1);
}
myList.print(); // [10 9 8 7 6 5 4 3 2 1 1 2 3 4 5 6 7 8 9 10 ]

const value = myList.access(3);
console.log(`myList.access(3) = ${value}`); // myList.access(3) = 7

myList.insert(20, 128);
myList.print(); // [10 9 8 7 6 5 4 3 2 1 1 2 3 4 5 6 7 8 9 10 128 ]
console.log(`tail: ${myList.tail.value}`);// tail: 128

myList.remove(4);
myList.print(); // [10 9 8 7 5 4 3 2 1 1 2 3 4 5 6 7 8 9 10 128 ]

myList.setHead(10);
myList.print(); // [2 3 4 5 6 7 8 9 10 128 ]
myList.printInv(); // [128 10 9 8 7 6 5 4 3 2 1 1 2 3 4 5 7 8 9 10 ]

console.log(myList.size()); // 10
```