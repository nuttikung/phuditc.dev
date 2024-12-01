---
author: Phudit Chuengjaroen
pubDatetime: 2024-12-01T22:00:00Z
modDatetime: 2024-12-02T06:40:00Z
title: Mutable กับ Immutable ต่างกันยังไง ?
slug: mutable-vs-immutable
featured: true
draft: false
tags:
  - mutable
  - immutable
  - typescript
  - javascript
description: Mutable กับ Immutable นับเป็นความรู้ขั้นพื้นทางของการพัฒนาโปรแกรมด้วยภาษา Typescript/Javascript เราควรทราบถึงความแตกต่างและการเลือกใช้งานเพื่อให้โปรแกรมได้รับประสิทธิภาพสูงที่สุดด้วย
---

ถ้าพูดถึงเรื่อง Mutable กับ Immutable หลายคนคงเคยได้ยินชื่อมาบ้างหรือบางคนอาจจะคุ้นเคยอยู่แล้ว แต่ถ้าใครยังไม่รู้จักเรื่องนี้วันนี้ลองอ่านบล๊อกนี้ดู โดยที่ผมจะขอใช้แหล่งอ้างอิงจาก MDN [Mutable](https://developer.mozilla.org/en-US/docs/Glossary/Mutable) [Immutable](https://developer.mozilla.org/en-US/docs/Glossary/Immutable)

## Table of contents

## Mutable

Mutable คือ Object (อ๊อบเจกต์) ที่สามารถเปลี่ยนค่าได้หลังจากที่ถูกสร้างมาแล้ว ตัวอย่างก็คือ Object, Array (อาเรย์), Date (วันที่) และ Function (ฟังก์ชั่น) คำว่าเปลี่ยนแปลงในที่นี้คือการเพิ่ม ลบและแก้ไขค่าได้ที่ค่าเดิมโดยตรงเลย

ตัวอย่าง 1

```typescript
// sort-mutable-example.ts
const months = ["March", "Jan", "Feb", "Dec"];
months.sort();
console.log(months); // จะเห็นได้ว่า months ถูกเปลี่ยนค่าทันที เมื่อเรา log ออกมาจะได้ ['Dec', 'Feb', 'Jan', 'March']
```

ตัวอย่าง 2

```typescript
// shift-mutable-example.ts
const array1 = [1, 2, 3];
const firstElement = array1.shift();
console.log(array1); // จะเห็นได้ว่า array1 ถูกเปลี่ยนค่าทันที เมื่อเรา log ออกมาจะได้ [2,3]
console.log(firstElement); // 1
```

## Immutable

Immutable คือ Object หรือค่าที่ไม่เปลี่ยนแปลงค่าตัวแปรเดิมแต่จะเป็นการสร้างของชิ้นใหม่ขึ้นมา ที่จริงแล้ว Primitive type (string, number, boolean, etc.) ก็ถูกนับอยู่ในนี้เช่นกัน ตามการอ้างอิงของ [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)

ตัวอย่าง 1

```typescript
// sort-immutable-example.ts
const months = ["Mar", "Jan", "Feb", "Dec"];
const sortedMonths = months.toSorted(); // ตรงนี้มีการใช้ toSorted คือการสร้าง Array ใหม่ที่มีการทำ sort แล้ว
console.log(sortedMonths); // sortedMonths มีค่าเป็น ['Dec', 'Feb', 'Jan', 'Mar']
console.log(months); // เราจะเห็นได้ว่า months ไม่ได้ถูกเปลี่ยนค่าเลย เมื่อ log ออกมายังเป็น ['Mar', 'Jan', 'Feb', 'Dec']
```

ตัวอย่าง 2

```typescript
// shift-immutable-example.ts
const array1 = [1, 2, 3];
const firstElement = array1.slice(1, array1.length); // ตรงนี้คือการทำ shift แบบ immutable
console.log(firstElement); // firstElement มีค่าเป็น [2, 3] และไม่ไปแก้ไขอาเรย์ต้นฉบับ
// หรืออีกวิธีคือ
const [head, ...otherWay] = array1;
console.log(otherWay); // otherWay มีค่าเป็น [2, 3] และไม่ไปแก้ไขอาเรย์ต้นฉบับ
console.log(array1); // ราจะเห็นได้ว่า array1 ไม่ได้ถูกเปลี่ยนค่าเลย เมื่อ log ออกมายังเป็น [1, 2, 3]
```

## Mutable and Immutable Specification

ถ้าหากว่าเราต้องจำแนกว่าเป็นแบบ Mutable หรือ Immutable เราจะรู้ได้ไงว่าอันไหนเป็นแบบแรกหรือแบบหลัง ?
หลักการคือเปลี่ยนค่าต้นฉบับไปกระทบกับค่าที่ใช้ครับหรือตัวแปรใหม่ที่สร้างจากต้นฉบับ (Mutable) และถ้าในกรณีที่เราไม่รู้จริงๆ สำหรับ Array method ขึ้นต้นด้วย <b>to</b> ส่วนใหญ่จะเป็นแบบ Immutable ครับ
ในปัจจุบันได้รองรับพวก sort กับ reverse แล้ว

### Object Specification

ตัวอย่างของ Mutable Object

```typescript
const target = { a: 1, b: 2 };
const mutateObj = target;
target.a = 2; // แก้ target
console.log(target); // { a: 2, b: 2 }
console.log(mutateObj); // { a: 2, b: 2 } <--a เปลี่ยน
```

ส่วน Spread operator ก็เป็นแบบ Immutable

```typescript
const target = { a: 1, b: 2 };
const spreadObj = { ...target };
target.a = 2; // แก้ target
console.log(target); // { a: 2, b: 2 }
console.log(spreadObj); // { a: 1, b: 2 } <--a ไม่ได้เปลี่ยน
```

ส่วนของ Object assign ก็ยังเป็น Immutable

```typescript
const target = { a: 1, b: 2 };
const returnedTarget = Object.assign({}, target);
target.a = 2; // แก้ target
console.log(target); // { a: 2, b: 2 }
console.log(returnedTarget); // { a: 1, b: 2 } <--a ไม่ได้เปลี่ยน
```

### Array Method Specification

| Mutable    | Immutable     |
| ---------- | ------------- |
| .push()    | .concat()     |
| .sort()    | .toSorted()   |
| .reverse() | .toReversed() |
| .shift()   | .slice(1)     |
| .pop()     | .slice(-1)    |
| .splice()  | .toSpliced()  |
| N/A        | .filter()     |

## Helper Library

[Immer](https://immerjs.github.io/immer)
[Immutable.js](https://immutable-js.com) 

## Conclusion from writer

จากทั้งหมดของบล๊อกนี้ก็ดูเหมือนว่า Immutable จะดีกว่ามาก ในแง่ของการ Develop แล้ว Bug ที่เกิดขึ้นน่าจะน้อยกว่าเพราะการแก้ค่าต่างๆไม่ได้ส่งผลต่อกันโดยตรง ในทางกลับกันถ้าในโค้ดมีการทำ Loop ที่เยอะมากแล้วใช้ Immutable อาจจะต้องระวังเป็นพิเศษในส่วน memory เพราะการทำ Immutable จะใช้ memory มากกว่ายิ่งถ้าเป็น Object ที่ต้องแก้แค่ 1 property แต่ต้องคอย copy อันเก่าที่เยอะมาก็จะทำให้ performance ลดลงได้
