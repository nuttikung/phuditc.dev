---
author: Phudit Chuengjaroen
pubDatetime: 2025-02-18T09:00:00Z
title: 🚀 Remeda - A Modern TypeScript-First Utility Library
slug: remeda-alternative-lodash
featured: true
draft: false
tags:
  - typescript
  - functional programming
  - utility
  - typesafe
  - intellisense
description: New utility functions libraries fully support TypeScript
---

🔥 สวัสดีครับ! วันนี้ผมมี library สุดเจ๋งมาแนะนำ นั่นก็คือ Remeda ที่มาพร้อมกับ utility functions มากมายเหมือน lodash ไม่ว่าจะเป็น chunk, filter, find และอีกเพียบ!

## Table of contents

## Intro

❓ แล้วทำไมต้อง Remeda?

Lodash และ Ramda นั้นถูกเขียนด้วย JavaScript แล้วค่อยเพิ่ม type ภายหลังผ่าน module augment ทำให้อาจมีข้อจำกัดบางอย่างในเรื่องของ type safety

## Pros

- สร้างมาด้วย TypeScript ตั้งแต่แรก
- มี type definitions ที่แม่นยำ
- IDE support ที่ดีกว่า
- ตรวจจับ error ได้ตั้งแต่ compile-time

## Cons

- มี Community ที่เล็กกว่า Lodash
- ต้องมีการเรียนรู้เพิ่มเติมจากปกติ เช่นใช้ Lodash ก็จะมี funtion ที่คุ้นเคยแต่ใน Remeda อาจจะไม่มีทำให้ต้องหาว่าใช้ funtion ไหนแทน
- ในกรณีที่เขียนแค่ Javascript ถ้าใช้ Remeda ก็ถือว่าใช้เกินตัวงานไปทำให้ไม่ได้รับผลดีจาก Typescript เลย

## Fully Typed

```ts
const DATA = [1, 2, 3] as const;

const builtin = DATA.map((x) => x.toString());
//    ^? string[] ตัวอย่างกรณีนี้เราควรรู้ว่ามันคือ Array<string> หรือ [string, string, string] (Fix length string) ซึ่งมีผลต่อ type

const withRemeda = R.map(DATA, (x) => x.toString());
//    ^? [string, string, string] กรณีนี้คือให้กลับมาเป็น Array Fix length + Type
```

## Data First or Data Last

Remeda support ทั้ง data-first และ data-last สำหรับ data-first ยังคงเป็นธรรมชาติและเป็นมิตรกับโปรแกรมเมอร์

```ts
// Lodash
_.pick(object, ["firstName", "lastName"]);
// Ramda
R.pick(object, ["firstName", "lastName"]); // data-first เป็นการส่ง param ไปก่อนแล้วตามด้วย fn
R.pick(["firstName", "lastName"])(object); // data-last เป็นการส่ง fn ไปก่อนแล้วรอรับ param
```

## Pipe

Pipe เป็นหลักการของ functional programming ที่เป็นการทำ function จาก ซ้ายไปขวา ถ้าอิงตามคณิศาสตร์จะเป็นตามข้างล่าง

```ts
// f(g(x)) -> pipe(x, g, f)
// example f(x) = x + 2, g(x) = x * 2
const ops1 = (x: number) => x * 2;
const ops2 = (x: number) => x + 2;
const result = pipe([1, 2, 3, 4], map(ops1), map(ops2));
```

## Guard

นอกจากนี้ตัว guard ก็ยังมี helper function ให้ใช้อีกมากมาย เช่น isArray, isEmpty, isNullish, isFunction อีกด้วย
สามารถอ่านเพิ่มเติมได้[ที่นี่](https://remedajs.com/docs/#hasSubObject)

## Conclusion from writer

จากที่ผู้เขียนได้ใช้มา ส่วนใหญ่จะนำมาใช้กรณีที่ทำ helper function แล้วนำไปใช้ซ้ำภายในโปรเจคอีกที โดยมีการใช้ pipe และ guard เป็นส่วนใหญ่ซึ่งข้อดีจริงๆคือเรื่อง type ที่ค่อนข้างชัดเจนมาก
และถ้าใครทำ functional programming แบบเต็มตัวคิดว่า Remeda น่าจะตอบโจทย์ได้ค่อนข้างสมบูรณ์แบบเลยทีเดียว
