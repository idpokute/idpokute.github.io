---
title: "Hash Table 해시테이블을 이해하자."
date: 2021-10-23 12:00
categories: algorithm
---

| key            | 내용   |
| -------------- | ------ |
| 삼성           | 90000  |
| 마이크로소프트 | 100000 |
| 엘지           | 50000  |
| AMD            | 30000  |

# 장점 Pros

해시 테이블의 장점은 속도입니다. 배열의 경우에 특정 데이터를 찾는데 걸리는 시간이 배열의 길이에 비례하지만, 해시 테이블은 속도가 (대부분) 일정합니다. O(1)이며, 최악의 경우에는 O(n)이 될 수 있다.

# 해시 함수 Hash function

데이터의 키는 해시 함수를 통해서 인덱스를 정합니다. `Key -> Hash Function -> Hash`. 이때 hash function이 리턴하는 값을 hash라고 합니다. 해시 테이블은 이를 index로 사용합니다.

# 해시 콜리전 hash collision

다른 키 값을 사용하더라도 해시 함수의 알고리즘을 통해서 동일한 해시가 발생하기도 합니다. 이 경우를 해시 콜리전이 발생했다고 합니다.
이를 해결하기 위한 몇가지 방법이 있습니다.

## Open Addressing

## linear probing

## Separate Chaining

충돌이 발생한 경우에 기존의 값에 링크드리스트, 배열 등을 이용해서 연결해서 추가하는 방법입니다.bucket에 다수의 value가 담겨있을 수 있습니다. 아래의 코드는 Chaining을 이용합니다.

```js
class HashTable {
  constructor() {
    this.table = new Array(128);
    this.size = 0;
  }

  hash(key) {
    let sum = 0;

    for (let i = 0; i < key.toString().length; i++) {
      sum += key.toString().charCodeAt(i);
    }
    return sum % this.table.length;
  }

  set(key, value) {
    const index = this.hash(key);
    if (this.table[index] && this.table[index].length) {
      for (let i = 0; i < this.table[index].length; i++) {
        // found the same key
        if (this.table[index][i][0] === key) {
          this.table[index][i][1] = value;
          return;
        }
      }
      this.table[index].push([key, value]);
    } else {
      this.table[index] = [];
      this.table[index].push([key, value]);
    }
    this.size++;
  }

  remove(key) {
    const index = this.hash(key);
    if (this.table[index]) {
      for (let i = 0; i < this.table[index].length; i++) {
        if (this.table[index][i][0] === key) {
          this.table[index].splice(i, 1);
          this.size--;
          return true;
        }
      }
    }
    return false;
  }

  get(key) {
    const index = this.hash(key);
    if (this.table[index]) {
      for (let i = 0; i < this.table[index].length; i++) {
        if (this.table[index][i][0] === key) {
          return this.table[index][i][1];
        }
      }
    }
    return undefined;
  }

  display() {
    this.table.forEach((values, i) => {
      let inside = values.map(([key, val]) => {
        return `[${key}: ${val}]`;
      });
      console.log(`${i}: ${inside}`);
    });
  }
}

let myHash = new HashTable();
myHash.set("samsung", 100000);
myHash.set("apple", 132);
myHash.set("apple", 400); // 이경우 동일한 key 이므로 value는 업데이트 됩니다.
myHash.display();
```
