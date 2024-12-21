---
author: Phudit Chuengjaroen
pubDatetime: 2024-12-27T12:00:00Z
title: Tailwind CSS Framework (Utility first)
slug: tailwind-css-basic
featured: true
draft: false
tags:
  - react
  - tailwind
  - css
description: มาทำความรู้จักกับ Tailwind CSS Framework ที่ช่วยให้การสร้าง UI แบบ utility-first ทำได้อย่างรวดเร็วและยืดหยุ่นกันเถอะ
---

ในวงการ CSS เราคุ้นเคยกับชื่อ Bootstrap ที่เคยโด่งดังมาก่อน หรือแม้แต่ Bulma CSS ที่เป็นที่นิยมในยุค Flexbox ทว่าปัจจุบัน ความนิยมของทั้งสองเฟรมเวิร์คนี้ลดลง โดยมี Tailwind CSS เข้ามาแทนที่ ดังนั้น แม้ว่าคุณอาจไม่ได้ใช้ Tailwind CSS โดยตรงในงานของคุณ แต่ในฐานะนักพัฒนา การรู้จักและเข้าใจ Tailwind CSS ก็เป็นสิ่งสำคัญในยุคปัจจุบัน

## Table of contents

## Tailwind Concept

Tailwind CSS ใช้แนวคิด "Utility-First" ซึ่งประกอบด้วย CSS class ขนาดเล็กที่มีหน้าที่เฉพาะเจาะจง สามารถนำมาประกอบกันเพื่อสร้างสไตล์ที่ต้องการ ตัวอย่างเช่น mt-2 หมายถึง margin-top: 8px; แนวคิดนี้คล้ายกับ functional programming ในการเขียนโปรแกรม

นอกจากนี้ Tailwind ยังรองรับการทำ responsive design, active และ focus state รวมถึง pseudo-classes อื่นๆ อีกทั้งยังอนุญาตให้ใช้ค่า arbitrary ผ่าน class ได้ เช่น หากต้องการกำหนดความสูงเฉพาะ สามารถใช้ h-[32px] เพื่อกำหนด height: 32px;

### Height

สามารถดูได้ที่ทั้งหมดได้ที่ [Tailwind-height](https://tailwindcss.com/docs/height)

| Class    | Properties                 |
| -------- | -------------------------- |
| h-0      | height: 0px;               |
| h-px     | height: 1px;               |
| h-0.5    | height: 0.125rem; / 2px /  |
| h-1      | height: 0.25rem; / 4px /   |
| h-1.5    | height: 0.375rem; / 6px /  |
| h-2      | height: 0.5rem; / 8px /    |
| h-2.5    | height: 0.625rem; / 10px / |
| h-3      | height: 0.75rem; / 12px /  |
| h-3.5    | height: 0.875rem; / 14px / |
| h-4      | height: 1rem; / 16px /     |
| h-5      | height: 1.25rem; / 20px /  |
| h-6      | height: 1.5rem; / 24px /   |
| h-7      | height: 1.75rem; / 28px /  |
| h-8      | height: 2rem; / 32px /     |
| h-9      | height: 2.25rem; / 36px /  |
| h-10     | height: 2.5rem; / 40px /   |
| h-11     | height: 2.75rem; / 44px /  |
| h-12     | height: 3rem; / 48px /     |
| h-14     | height: 3.5rem; / 56px /   |
| h-16     | height: 4rem; / 64px /     |
| h-20     | height: 5rem; / 80px /     |
| h-auto   | height: auto;              |
| h-1/2    | height: 50%;               |
| h-1/3    | height: 33.333333%;        |
| h-2/3    | height: 66.666667%;        |
| h-1/4    | height: 25%;               |
| h-2/4    | height: 50%;               |
| h-3/4    | height: 75%;               |
| h-1/5    | height: 20%;               |
| h-2/5    | height: 40%;               |
| h-3/5    | height: 60%;               |
| h-4/5    | height: 80%;               |
| h-full   | height: 100%;              |
| h-screen | height: 100vh;             |
| h-svh    | height: 100svh;            |
| h-lvh    | height: 100lvh;            |
| h-dvh    | height: 100dvh;            |
| h-min    | height: min-content;       |
| h-max    | height: max-content;       |
| h-fit    | height: fit-content;       |

### Width

สามารถดูได้ที่ทั้งหมดได้ที่ [Tailwind-width](https://tailwindcss.com/docs/width)

| Class    | Properties               |
| -------- | ------------------------ |
| w-0      | width: 0px;              |
| w-px     | width: 1px;              |
| w-1      | width: 0.25rem; / 4px /  |
| w-2      | width: 0.5rem; / 8px /   |
| w-3      | width: 0.75rem; / 12px / |
| w-4      | width: 1rem; / 16px /    |
| w-5      | width: 1.25rem; / 20px / |
| w-6      | width: 1.5rem; / 24px /  |
| w-7      | width: 1.75rem; / 28px / |
| w-8      | width: 2rem; / 32px /    |
| w-9      | width: 2.25rem; / 36px / |
| w-10     | width: 2.5rem; / 40px /  |
| w-11     | width: 2.75rem; / 44px / |
| w-12     | width: 3rem; / 48px /    |
| w-14     | width: 3.5rem; / 56px /  |
| w-16     | width: 4rem; / 64px /    |
| w-20     | width: 5rem; / 80px /    |
| w-24     | width: 6rem; / 96px /    |
| w-28     | width: 7rem; / 112px /   |
| w-32     | width: 8rem; / 128px /   |
| w-auto   | width: auto;             |
| w-1/2    | width: 50%;              |
| w-1/3    | width: 33.333333%;       |
| w-2/3    | width: 66.666667%;       |
| w-1/4    | width: 25%;              |
| w-2/4    | width: 50%;              |
| w-3/4    | width: 75%;              |
| w-1/5    | width: 20%;              |
| w-2/5    | width: 40%;              |
| w-3/5    | width: 60%;              |
| w-4/5    | width: 80%;              |
| w-full   | width: 100%;             |
| w-screen | width: 100vw;            |
| w-svw    | width: 100svw;           |
| w-lvw    | width: 100lvw;           |
| w-dvw    | width: 100dvw;           |
| w-min    | width: min-content;      |
| w-max    | width: max-content;      |
| w-fit    | width: fit-content;      |

### Example

ต่อไปเราจะทดลองสร้าง Card Component โดยมีข้อกำหนดดังต่อไปนี้

- width - 128 px
- height - 80 px
- padding - 8 px
- margin - 8 px
- border - solid 1 px
- border color - gray (anything not white tone)

โดยที่ถ้าเริ่มต้นเราจะเห็นได้เลยว่าจริงๆแล้วสำหรับ Tailwind คือการเอา utility class มารวมกัน

| Spec               | Class           |
| ------------------ | --------------- |
| width 128px        | w-32            |
| height 80px        | h-20            |
| padding 8 px       | p-2             |
| margin 8 px        | m-2             |
| border-width 1 px  | border          |
| border-style solid | border-solid    |
| border-color gray  | border-gray-500 |

เมื่อรวมกันแล้วใส่ลงใน classname ของ react component เราจะได้แบบ code ตัวอย่างด้านล่าง

```tsx
const Card: FC<PropsWithChildren> = ({ children }) => {
  return (
    <div className="m-2 h-20 w-32 border border-solid border-gray-500 p-2">
      {children}
    </div>
  );
};
```

![card-basic-example](@assets/tailwind-css-basic/card-example-1.png)

ต่อมาเราบอกว่า อยากให้ Card นี้มีขอบ

- border radius - 8 px

```tsx
const Card: FC<PropsWithChildren> = ({ children }) => {
  // เพิ่ม rounded-lg
  return (
    <div className="m-2 h-20 w-32 rounded-lg border border-solid border-gray-500 p-2">
      {children}
    </div>
  );
};
```

![card-border-radius-example](@assets/tailwind-css-basic/card-example-border-radius.png)

## Responsive

ต่อไปเราจะทดลองสร้าง Card Component โดยมีข้อกำหนดดังต่อไปนี้

- mobile bg-color slate 700 (xs)
- tablet bg-color stone 700 (md)
- other bg-color white (lg up)
- mobile and tablet font-color black (xs-lg)
- other font-color white (lg up)

ใน Tailwind มี responsive class อยู่ ตาม [Responsive-design](https://tailwindcss.com/docs/responsive-design)

| Breakpoint | Minimum width CSS |
| ---------- | ----------------- |
| sm         | min-width: 640px  |
| md         | min-width: 768px  |
| lg         | min-width: 1024px |
| xl         | min-width: 1280px |
| 2xl        | min-width: 1536px |

```tsx
const Card: FC<PropsWithChildren> = ({ children }) => {
  return (
    <div className="m-2 h-20 w-32 rounded-lg border border-solid border-gray-500 bg-slate-700 p-2 text-white md:bg-stone-700 lg:bg-white lg:text-black">
      {children}
    </div>
  );
};
```

🎉🎉 ผลลัพธ์ที่ได้ก็คือ

![card-responsive-example](@assets/tailwind-css-basic/tailwind-css-responsive.gif)

## Developer Extension

- [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
  ไว้ใช้ช่วย auto complete class ของ tailwind

## Conclusion from writer

Tailwind CSS ไม่ได้ยากที่จะใช้งาน แต่อาจมีช่วงการเรียนรู้ (Learning Curve) ในระยะแรก เมื่อเรายังไม่คุ้นเคยกับชื่อ class ที่ตรงกับ CSS ที่ต้องการ อย่างไรก็ตาม ปัญหานี้จะหมดลงเมื่อใช้งานบ่อยขึ้น จนสามารถจดจำได้

การใช้งาน Tailwind คล้ายกับแนวคิดของ Functional Programming ซึ่งถือเป็นจุดแข็งหลักของ framework นี้ ทำให้การพัฒนาเป็นไปอย่างมีประสิทธิภาพและยืดหยุ่นอีกด้วย
