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

สิ่งที่ควรมีก่อนที่จะเริ่มสำหรับบทความนี้คือ สติ !! ไม่ช่ายย install rust ก่อน

## Installation

มาทำการลง rust ในเครื่องของเราก่อนโดยที่สามารถทำตาม [official installation guide](https://www.rust-lang.org/tools/install) ได้เลย
หลังจาก installation เรียบร้อยแล้วให้ลองเปิด terminal แล้วพิมพ์

```sh
rustc --version
```

เมื่อได้ผลลัพธ์กลับมาเป็น `rustc 1.85.1 (4eb161250 2025-03-15)` หรืออาจจะเวอร์ชั่น (ใหม่/เก่า) กว่านี้ก็ได้
แปลว่า rust installation สำเร็จและพร้อมใช้งานแล้ว

## About Rust

Rust เป็นภาษาแบบ low-level โดยที่จะต้องมีการ compile code (ชุดคำสั่ง) ไปเป็นไฟล์แบบ binary (010101) เพื่อให้คอมพิวเตอร์สามารถอ่านค่าได้
นั่นหมายความว่าเมื่อผ่านการ compile มาแล้ว ไฟล์นั้นเราอาจจะไม่สามารถอ่านได้ แต่ machine จะสามารถอ่านได้และใช้ได้เลยโดยที่ไม่ต้องลงอะไรเพิ่มเติม

สำหรับคนที่อยากอ่านเพิ่มเติมเกี่ยวกับ compiler vs interpreter สามารถอ่านได้[ที่นี่](https://phuditc.dev/posts/interpreter-vs-compiler)

## Data Type

ปัจจุบัน rust มี type และวิธีการประกาศตัวแปรตามด้านล่างนี้

- Boolean

  ```rust
  let isDistinct: bool = true;
  ```

- String

  ```rust
  // แบบที่ 1
  let name: String = String::from("Phudit");
  // แบบที่ 2
  let name: String = "Phudit".to_string();
  ```

- Character

  ```rust
  let alphabet: char = 's';
  ```

- Integer

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

- Float
- Vector
- Hashmap
- Hashset
- Struct


## Conclusion from writer

TBC
