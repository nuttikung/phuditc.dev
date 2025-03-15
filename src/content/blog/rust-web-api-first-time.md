---
author: Phudit Chuengjaroen
pubDatetime: 2025-03-17T11:45:00Z
title: เมื่อผมเขียน API ด้วย Rust 🦀
slug: first-time-api-with-rust
featured: true
draft: false
tags:
  - rust
  - web
  - api
  - backend
  - experimental
  - performance
description: สร้าง API ด้วย Rust + Axum แล้วเปิดให้ทีมทดลองใช้เขียนเว็บเล่นเป็นยังไงกันนะ ?
---

เมื่อภาษาหนึ่งโด่งดังจนเป็นกระแสมากแบบเกินต้าน เราจึงจำเป็นต้องเข้าร่วมขบวนการ ภาษานั้นก็คือ **Rust** 🦀 ครับ
ซึ่งผมก็ได้มีโอกาสในการเขียนเล่นให้น้องในทีมได้นำไปแชร์กันในหัวข้อ Vue JS แบบง่ายๆ เลยอยากจะมาเล่าประสบการณ์ให้ฟังในบล็อกนี้

## Table of contents

## Intro

ก่อนอื่นต้องบอกก่อนว่าผมเป็นคือ Beginner Rust ที่เพิ่งเริ่มเขียนและพยายามทำความเข้าใจกับตัวภาษาให้มากขึ้นและคิดที่จะใช้ต่อในอนาคตกับบางงาน

## Inspiration

ผมได้มีการคุยกับน้องในทีม เรามีการแชร์ความรู้กันทุกวันศุกร์ ซึ่งน้องที่เป็นคนแชร์บอกว่าอยากให้คนอื่นในทีมได้ลองทำ Todo List ด้วย VueJS (น้องที่เป็นคนแชร์มีประสบการณ์มาก่อน แต่ Stack ที่เราใช้กันจริงคือ React)
ผมก็บอกน้องว่าพี่อยากลอง Challenge ตัวเองด้วยการทำ Todo List API ให้ด้วยภาษา Rust ซึ่งถ้าไม่ทันผมมีแผนสำรองที่จะทำ Node ขึ้นไปได้ในระยะเวลาที่น้อยนิด (1-2 วัน)

## Framework & Decision

การที่ผมเลือก **Axum** เพราะมีโครงสร้างหรือหลักการค่อนข้างคล้าย Express JS มากที่สุดจึงทำให้ผมเรียนรู้น้อยสุดในเวลาที่จำกัด
และถ้ามีปัญหาเกิดขึ้นผมน่าจะแก้ปัญหาได้ไวสุด ตัวที่มองไว้นอกจาก **Axum** ก็มี **Actix** กับ **Rocket** ครับดูน่าสนใจเหมือนกัน

## Database

ในส่วนของ Database เลือกใช้เป็น postgresql เพราะว่ามันไม่มีเรื่อง version มาเกี่ยวข้องครับ ไม่ว่าจะยังไงก็เป็น standard เดียวกันหมด อีกอย่างผมเน้นงานไวเลยใช้ตัวฟรีไปก่อน
ตัวที่ผมใช้คือ [Neon Postgres](https://neon.tech)

## Code & Compiler

การเขียนโค้ดเองไม่ได้ยากมาก แต่มี **Learning Curve** ระดับนึงตามหลักการทั่วไปของ Low-level ซึ่งผมต้องใช้เวลาในการเรียนรู้ แต่ไม่ได้เป็นปัญหาใหญ่ถ้าเขียนบ่อยๆน่าจะใช้งานได้ดีขึ้นครับ
ส่วน **Compiler** เองก็โหดมากคอยด่าเราสม่ำเสมอ ซึ่งผมมองเป็นข้อดีมากเช่นเดียวกัน ชอบเวลาที่ด่าแล้วบอกด้วยว่าต้องไปอ่านหรือดูอะไรเพิ่ม

## Deploy

ในส่วนของการ deploy ได้ใช้ตัว Gihub Actions + Docker เพื่อต่อไปยัง Google Cloud (Artifact Registry + Cloud Run) ตรงนี้มีปัญหาอยู่ค่อนข้างนานเหมือนกัน
จนให้พี่ + น้องในทีม Backend มาช่วยดูทุกคนติดคล้ายกัน เพราะตอนแรกเป็น Google Container Registry แล้วก็เปลี่ยนมาเป็น Artifact Registry จนได้มาหาคำตอบเองได้แต่ก็ต้องขอบคุณทางฝั่ง Backend ด้วยที่เข้ามาช่วยดูให้ครับ
หลังจากนั้นพอแก้เรื่องนี้ได้ก็ไปติดตรง Cloud Run อีกทีนึงพอแก้ปัญหาได้ auto deploy ก็สมบูรณ์ครับ

ถ้าเขียนปัญหาเป็นข้อดูจะมีรายการดังต่อไปนี้
- มีการหา Role & Permission บางอย่าง Google Cloud ไม่เจอ
- Docker Build สำเร็จ แต่ส่งไป GCR ไม่ได้ ต้อง $LOCATION/$PROJECT_ID/$PROJECT_NAME/$IMAGE ตรงนี้ $IMAGE ฝั่งผมหายไป
- Docker ไปถึง Cloud กดเพื่อ Deploy แต่ไม่มี health check จึงทำให้ระบบไม่ทำงาน
- Binding Port Default ของ Cloud Run 8080 แต่ผมใช้ 3000 เลยต้องมีการแก้ไขเพิ่ม หรือจะทำจากฝั่ง Cloud Run ก็ได้
- Start Docker Image ได้ แต่ Setting (env) ไม่ได้ไปด้วย แก้ด้วยการกลับไป Dockerfile แล้ว copy ไปด้วย

## Performance & CPU

ถ้าพูดถึง Low Level คงหลีกเลี่ยงการบอกเกี่ยวกับ Performance ของ CPU และ Memory ไม่ได้

อันนี้เป็น Spec ของ Cloud ที่ผมใช้
```yaml
resources:
  limits:
    cpu: 1000m
    memory: 512Mi
```

![Rust cpu and memory utilise](@assets/rust-web-api/rust-cpu-memory-utilisation.png)

![Rust cpu memory specify time](@assets/rust-web-api/rust-spu-time-expanded.png)

จากภาพจะเห็นได้ว่าเมื่อตอนที่ทีมมีการทำ Work Shop (Team Size 10-15 คน) กัน CPU ยังไปไม่ถึง 5% และ Memory ก็ไปไม่ถึง 10% (1 Core, 512 MB ตามที่ระบุในส่วน resources limits)

## Conclusion from writer

ทางผู้เขียนคิดว่า ภาษา Rust เป็นภาษาที่ดีทำให้เรารอบคอบมากก่อนที่จะสร้างของชิ้นหนึ่งออกไป ด้วยตัว Compiler เองที่โหดทำให้ข้อผิดพลาดน้อยมากๆ แต่สำหรับบางคนอาจจะมีช่องว่างการเรียนรู้ที่เยอะขึ้นเช่นเดียวกัน
แต่ในแง่ของ Performance เองก็ต้องชื่นชมว่าทำได้ดีมาก ไม่ว่าจะเป็น CPU หรือ Memory ทำให้เราสามารถจ่ายค่า Cloud น้อยลงได้ที่มาจากการกินทรัพยากรน้อยลงหรืออาจจะทำให้ Service คอขวดลดลงด้วยได้อีกเช่นกัน

[Example Repository](https://github.com/nuttikung/rust-axum-todo-list/tree/develop)
