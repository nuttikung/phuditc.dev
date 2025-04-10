---
author: Phudit Chuengjaroen
pubDatetime: 2025-04-11T11:45:00Z
title: 🦀 Rust Basic
slug: rust-basic
featured: true
draft: false
tags:
  - rust
  - syntax
  - basic-programming
description: มาลองทำความรู้จัก rust ด้วย syntax เบื้องต้นกันดีกว่า
---

วันนี้ผมอยากพามารู้จัก rust ผ่านตัว syntax แบบเบื้องต้น ซึ่งควรมีพื้นฐานของ programming มาบ้าง

## Table of contents

## Intro

สิ่งที่ควรมีก่อนที่จะเริ่มสำหรับบทความนี้คือ สติ !! ไม่ช่ายยย install rust ก่อน

## Installation

มาทำการลง rust ในเครื่องของเราก่อนโดยที่สามารถทำตาม [official installation guide](https://www.rust-lang.org/tools/install) ได้เลย
หลังจาก installation เรียบร้อยแล้วให้ลองเปิด terminal แล้วพิมพ์

```sh
rustc --version
```

เมื่อได้ผลลัพธ์กลับมาเป็น `rustc 1.85.1 (4eb161250 2025-03-15)` หรืออาจจะเวอร์ชั่น (ใหม่/เก่า) กว่านี้ก็ได้
แปลว่า rust installation สำเร็จและพร้อมใช้งานแล้ว

ในกรณีที่ไม่อยาก install สามารถลองเล่น online ได้ [ที่นี่](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024)

## About Rust

Rust เป็นภาษาแบบ low-level โดยที่จะต้องมีการ compile code (ชุดคำสั่ง) ไปเป็นไฟล์แบบ binary (010101) เพื่อให้คอมพิวเตอร์สามารถอ่านค่าได้
นั่นหมายความว่าเมื่อผ่านการ compile มาแล้ว ไฟล์นั้นเราอาจจะไม่สามารถอ่านได้ แต่ machine จะสามารถอ่านได้และใช้ได้เลยโดยที่ไม่ต้องลงอะไรเพิ่มเติม

สำหรับคนที่อยากอ่านเพิ่มเติมเกี่ยวกับ compiler vs interpreter สามารถอ่านได้[ที่นี่](https://phuditc.dev/posts/interpreter-vs-compiler)

## Data Type

ปัจจุบัน rust มี type และวิธีการประกาศตัวแปรตามด้านล่างนี้

### Boolean

  ```rust
  let isDistinct: bool = true;
  ```

### String

  ```rust
  // แบบที่ 1
  let name: String = String::from("Phudit");
  // แบบที่ 2
  let name: String = "Phudit".to_string();
  ```

### Character

  ```rust
  let alphabet: char = 's';
  ```

### Integer

  integer เป็นชนิดของข้อมูลที่เก็บตัวเลขแบบจำนวนเต็มเท่านั้น
  เนื่องจากภาษาจำพวก `low-level` จะมีการกำหนดขนาดของหน่วยความจำที่จะใช้ซึ่งหมายความว่าเราต้องระบุถึงข้อมูลที่จะเก็บในเขิงตัวเลข เช่น ตัวเลขที่จะเก็บไม่เกิน 127 (ติดลบได้)

  ดังนั้นเราจะต้องรู้ว่า int, uint แบบไหนเก็บค่าได้ตั้งแต่ต่ำสุดถึงสูงสุดที่เท่าไหร่
  | Type     | Range                                                   |
  | -------- | ------------------------------------------------------- |
  | i8       | -128 ถึง 127                                             |
  | i16      | -32768 ถึง 32767                                         |
  | i32      | -2147483648 ถึง 2147483647                               |
  | i64      | -9223372036854775808 ถึง 9223372036854775807             |
  | u8       | 0 ถึง 255                                                |
  | u16      | 0 ถึง 65535                                              |
  | u32      | 0 ถึง 4294967295                                         |
  | u64      | 0 ถึง 18446744073709551615                               |
  | u128     | 0 ถึง 340282366920938463463374607431768211455            |
  | usize    | 0 ถึง 18446744073709551615                               |

  ```rust
  // i8
  let my_num: i8 = -10;
  let my_num: i8 = 10;
  let my_num: i8 = 200; // error

  // สามารถประกาศตัวแปรแบบนี้ได้ โดยที่ rust จะกำหนด type เริ่มต้นให้เป็น i32
  let my_num = 1;
  ```

  ส่วนของ Error Rust จะบอกได้แบบตรงจุดพร้อมกับคำแนะนำหรือบอกกฏที่ไปโดนว่าเป็นเลขข้อไหนเช่น `เกิน range ที่กำหนด ตามภาพนี้`, `error[E0308]: mismatched types`
  ![rust integer error out of range](@assets/rust-basic/rust-integer-error.png)

### Float

  float เป็นชนิดของข้อมูลที่เก็บตัวเลขที่มีทศนิยม ซึ่งประกอบไปด้วย `f16`, `f32`, `f64` และ `f128`

  ```rust
  let my_num: f16 = 3.0;
  ```

### Vector

  คือชนิดข้อมูลที่ไว้ใช้เก็บของที่ซ้ำๆกันหรือในทาง programming ของภาษาอื่นเรียกสิ่งนี้ว่า `Array`

  ```rust
  let nums: Vec<i8> = Vec::from([1, 2, 3, 4]);
  // หรือ
  let nums: Vec<i8> = vec![1, 2, 3, 4];
  ```

### Struct

  คือชนิดข้อมูลที่รวมกันเป็นกลุ่มจากหลายๆชนิดให้อยู่ในรูปแบบ `property` เช่น
  ```rust
  struct Dog {
      name: String,
      age: i8,
  }

  let dog: Dog = Dog {
      name: "Scooby doo".to_string(),
      age: 7,
  };
  ```

## Control Flow

โดยทั่วไปของการเขียนโปรแกรมจำเป็นที่จะต้องมี control flow เพื่อกำหนดเงื่อนไขต่างๆ เช่น ถ้าอายุมากกว่า 20 ให้ตอบกลับว่า `Adult`, ถ้ารูปทรงสี่เหลี่ยมให้ตอบเป็น กว้างคูณยาว, กรณีสุดท้ายคือให้ทำสิ่งนี้ซ้ำๆจนกว่าจะตรงกับเงื่อนไข

### If Else

```rust
fn is_adult(age: i8) -> String {
    if age > 20 {
        return "Adult".to_string()
    } else {
        return "Child".to_string()
    }
}
```

หมายเหตุ rust สามารถ return แบบไม่ต้องใช้คำว่า return ได้

```rust
fn is_adult(age: i8) -> String {
    if age > 20 {
        "Adult".to_string()
    } else {
        "Child".to_string()
    }
}
```

### Switch Case

ใน rust เองไม่ได้มี syntax switch case แบบภาษาอื่นๆ แต่มีอีกตัวมาแทนซึ่งชื่อว่า Match โดยจะประกอบด้วย

```
match (variable) {
  (case) => (what to do?),
  (case) => (what to do?),
  (_) => (default)
}
```

```rust
fn classify_spell(spell: &str) -> String {
    match spell {
        "Wingardium Leviosa" => String::from("Make the thing flying on the air"),
        "Lumos" => String::from("Little light on the wand"),
        "Alohomora" => String::from("Unlock the door"),
        _ => String::from("Normal spell ...")
    }
}

fn main() {
    let spell = "Lumos".to_string();
    let spell_description = classify_spell(&spell);
    println!("{}", spell_description); // Little light on the wand
}
```

### Loop (For and While)

loop คือสิ่งที่ใช้ทำซ้ำ ในกรณีนี้ขอแยกเป็น condition กับ vector หรือจะมองว่ามีหลายแบบก็ได้

แบบที่ 1 (for range)

```rust
for i in 1..10 {
    println!("{}",i);
}
// ผลลัพธ์ที่ได้
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9

// ถ้าต้องการถึง 10 สามารถทำได้โดย
for i in 1..=10 {
    println!("{}",i);
}
```

แบบที่ 2 (loop condition)

`loop` คือคำสั่งให้เข้าไปทำใน statement นั้นโดยจะไม่มีที่สิ้นสุด วิธีการหยุดให้หาเงื่อนไขที่ต้องการหยุดแล้ว `break`

```rust
let mut i:i8 = 0;
loop {
    if i>10 {
        break;
    }
    println!("{}",i);
    i +=1;
}

// ผลลัพธ์ที่ได้
// 0
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
// 10
```

แบบที่ 3 (while condition)

```rust
let mut i:i8 = 0;
while i<10 {
    println!("{}",i);
    i +=1;
}

// ผลลัพธ์ที่ได้
// 0
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
```

แบบที่ 4 (for in vector)

```rust
let num_vec: Vec<i8> = vec![1, 3, 5, 7, 9];
for n in &num_vec {
    println!("{}", n);
}
// หรือ
for n in num_vec.iter() {
    println!("{}", n);
}

// ผลลัพธ์ที่ได้
// 1
// 3
// 5
// 7
// 9
```

## Example

มาลองทำโปรแกรมตัดเกรดโดยที่มีเงื่อนไขดังต่อไปนี้

ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 80 ได้เกรด A
ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 75 ได้เกรด B+
ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 70 ได้เกรด B
ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 65 ได้เกรด C+
ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 60 ได้เกรด C
ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 55 ได้เกรด D+
ถ้าคะแนนนักเรียนมากกว่าหรือเท่ากับ 50 ได้เกรด D
ต่ำกว่าหรือเท่ากับ 49 ได้เกรด F

เฉลยอยู่ด้านล่าง แนะนำให้ลองทำเองก่อนจะได้คุ้นเคยกับ syntax ของภาษาได้มากขึ้น

ในกรณีไม่อยาก install rust สามารถลองเล่น online ได้ [ที่นี่](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024)


```rust
fn classify_grade(score: i8) -> String {
    if score >= 80 {
        "A".to_string()
    } else if score >= 75 {
        "B+".to_string()
    } else if score >= 70 {
        "B".to_string()
    } else if score >= 65 {
        "C+".to_string()
    } else if score >= 60 {
        "C".to_string()
    } else if score >= 55 {
        "D+".to_string()
    } else if score >= 50 {
        "D".to_string()
    } else {
        "F".to_string()
    }
}

fn main() {
    println!("{}", classify_grade(85));
    println!("{}", classify_grade(80));
    println!("{}", classify_grade(78));
    println!("{}", classify_grade(75));
    println!("{}", classify_grade(74));
    println!("{}", classify_grade(70));
    println!("{}", classify_grade(67));
    println!("{}", classify_grade(65));
    println!("{}", classify_grade(63));
    println!("{}", classify_grade(60));
    println!("{}", classify_grade(56));
    println!("{}", classify_grade(55));
    println!("{}", classify_grade(54));
    println!("{}", classify_grade(50));
    println!("{}", classify_grade(49));
    println!("{}", classify_grade(40));
}
```

## Conclusion from writer

ในบล๊อคนี้เป็นการแนะนำให้รู้จักกับภาษา rust มากขึ้นในส่วนของ syntax และ data type แบบพื้นฐานเป็นหลักก่อน ถ้ามีเวลาจะพาไปเรียนรู้สิ่งต่อไปที่มีความซับซ้อนมากขึ้น
ผู้เขียนอาจจะมีการข้ามบางอย่างไปบ้างในบางจุดถ้ามีข้อสงสัยแนะนำให้อ่าน official document โดยตรงจะได้เข้าใจมากขึ้นและละเอียดกว่า
