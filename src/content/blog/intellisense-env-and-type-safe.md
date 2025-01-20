---
author: Phudit Chuengjaroen
pubDatetime: 2025-01-24T09:00:00Z
title: Typesafe and IntelliSense for environment
slug: environment-with-intellisense-and-typesafe
featured: true
draft: false
tags:
  - environment
  - intellisense
  - typesafe
  - react
  - typescript
  - developer experience
  - schema
  - validation
  - zod
description: Environment ตัวร้ายรู้ตัวอีกทีตายบน production
---

ในการพัฒนาโปรแกรมปัจจุบัน การใช้ Environment Variables ถือเป็นสิ่งจำเป็น แต่หลายคนมักประสบปัญหาด้าน DX (Developer Experience) เนื่องจากไม่สามารถทราบได้ว่าตัวแปร Environment ที่ใช้อยู่นั้นเป็น Type อะไร หรือมีอยู่จริงหรือไม่
วันนี้เราจะมาแนะนำวิธีแก้ปัญหานี้ ซึ่งจะช่วยปรับปรุง DX ให้ดีขึ้น ด้วยการเพิ่มความสามารถ Autocomplete ที่จะช่วยลดข้อผิดพลาดในการพัฒนา นอกจากนี้ยังสามารถแสดง Error message ที่เป็นมิตรกับนักพัฒนา ทำให้อ่านและเข้าใจได้ง่ายขึ้น แม้จะเป็นการแสดงผลใน Production ก็ตาม

## Table of contents

## Intro

Environment หรือตัวแปรสภาพแวดล้อม คือค่าที่ใช้กำหนดการตั้งค่าต่างๆ ของแอพพลิเคชัน
ปกติแล้วยังไม่มี Intellisense และการใช้ Type-safe กับ environment เป็นการแก้ปัญหาที่มีประโยชน์ดังนี้:

1. ความปลอดภัย: ป้องกันการเข้าถึงตัวแปรที่ไม่มีอยู่จริง
2. การพัฒนาที่ดีขึ้น: มี Auto-complete และ IntelliSense ช่วยในการเขียนโค้ด
3. ลดข้อผิดพลาด: ตรวจจับข้อผิดพลาดได้ตั้งแต่ช่วง Development
4. การบำรุงรักษา: ทำให้โค้ดอ่านง่ายและจัดการได้ดีขึ้น

## Environment Without Type (Current Problem)

การใช้ environment แบบไม่มีการระบุ type เป็นสิ่งที่ developer หลายคนทำซึ่งในการพัฒนาถ้าโปรแกรมไม่ได้ใช้ตัวแปรเหล่านี้มากก็อาจจะเกิดปัญหาได้น้อยแต่ก็มีโอกาสในเรื่องของ typo

![Environment Without Type Declaration](@assets/typesafe-environment/environment-without-type.png)

## Environment Type Declaration

ในกรณีนี้ผู้เขียนขอยกตัวอย่างจาก React(Typescript) + Vite ซึ่งปกติจะมาพร้อมกับ environment แบบ `import.meta.env` อยู่แล้ว แต่พอเราจะ `.key` ต่อไปเราจะไม่สามารถเห็น type หรือ editor ไม่สามารถ autocomplete ได้

เรามาลองแก้ปัญหาแรกแบบง่ายที่สุดด้วยการทำ Type Declaration ของ environment กัน

```ts
// vite-env.d.ts
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_APP_TITLE: string // key ของ env ที่เรามีการ set value พร้อมที่จะใช้อาจจะมาจาก .env หรือ config vite.config.ts
  readonly VITE_APP_DEBUG: string
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

ต่อมาเมื่อเราลองมาดูผลลัพธ์จะเห็นว่า Intellisense ของ Editor เริ่มทำงานได้แล้ว ขั้นตอนนี้เป็นการเพิ่ม DX ทำให้เราไม่มีปัญหาเรื่อง typo ได้

![Vite Environment Type Declaration](@assets/typesafe-environment/vite-type-declaration.png)

## Environment Schema Validation

ขั้นตอนต่อมาเป็นการทำ Schema เพื่อ Map กับ Environment ของเรา โดยที่ขั้นตอนนี้คือการทำ Validation จริงซึ่งสามารถบอก error ที่มีประโยชน์ให้กับเราได้และขั้นตอนนี้ไม่จำเป็นต้องทำร่วมกับขั้นตอนแรกเพราะมีตัวช่วยคือ `zod`

```ts
import { z } from 'zod';
const envSchema = z.object({
  VITE_APP_TITLE: z.string().default('default app name'),
  VITE_APP_DEBUG: z.enum(['true', 'false']).transform((value) => value === 'true'),
  VITE_APP_EXAMPLE_NUMBER: z.coerce.number().min(1000),
  VITE_APP_MAIL_FROM: z.string().email(),
});

const parsedEnv = envSchema.safeParse(import.meta.env);

if (!parsedEnv.success) {
  // crash the app we don't allow to run.
  console.error(parsedEnv.error);
  throw Error('Validate environment does not match to schema');
}

const environment = parsedEnv.data;

export { environment }
```

```
//.env
VITE_APP_TITLE=typesafe-env-example
VITE_APP_DEBUG=true
VITE_APP_EXAMPLE_NUMBER=3000
VITE_APP_MAIL_FROM=example@email.com
```

ตัวอย่างการแสดงผลเมื่อ Environment ถูกต้องตาม schema

![Environment Correction with Schema Validation](@assets/typesafe-environment/zod-correct-environment.png)

![Environment Type Safety](@assets/typesafe-environment/environment-type.png)

ตัวอย่างการแสดงผลเมื่อ Environment ไม่ถูกต้องตาม schema

![Environment Incorrect with Schema Validation](@assets/typesafe-environment/environment-schema-validation-failed.png)

![Environment Incorrect with Schema Validation Number](@assets/typesafe-environment/environment-schema-validation-failed-number.png)

## Conclusion from writer

ทางผู้เขียนบทความมีความชอบในการทำ type safe เป็นประจำอยู่แล้วและส่วนนี้เป็นส่วนที่ส่งผลกับ DX ในทางที่ดีขึ้นรวมทั้งการอ่าน error ว่า environment ส่วนไหนที่มีข้อผิดพลาดและยังสามารถแก้ปัญหาได้ไวขึ้นด้วย
