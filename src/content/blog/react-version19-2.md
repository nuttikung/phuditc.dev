---
author: Phudit Chuengjaroen
pubDatetime: 2025-10-13T11:45:00Z
title: <Activity /> ใน React 19.2 คืออะไรและน่าสนใจอย่างไร
slug: react-19.2
featured: true
draft: false
tags:
  - react
  - <Activity />
  - frontend
description: React Version 19.2 ได้ออกเมื่อวันที่ 1 ตุลาคม มีการเพิ่ม component ที่ชื่อว่า <Activity /> มาให้ ลองมาดูกันดีกว่าจะนำไปใช้ยังไงดี ?
---

## Table of contents

## Intro

ใน react 19.2 จริงๆแล้วไม่ได้มีแค่ `Activity` แต่ยังมีอย่างอื่นด้วย เช่น `useEffectEvent`, `cacheSignal` แต่ในบทความนี้จะเน้นย้ำที่เรื่อง `Activity` เป็นหลักครับ

## Rendering

ลองมาดูการ render แบบปกติตอนที่ยังไม่ได้ใช้ Activity ก่อน (React < 19.2)
code ตัวอย่างการทำงาน

```tsx
const Page = () => {
  const [isOpenSidebar, setIsOpenSidebar] = useState<boolean>(false)

  return (
    <ThemeProvider>
      <Header />
      <Body />
      <Footer />
      {isOpenSidebar && <Sidebar />}
    </ThemeProvider>
  )
}
```

มาลองดู Timeline ของการ Paint เวลาเจอว่า isOpenSidebar มีการเปลี่ยนแปลงกันว่า react ใช้วิธีการ process อย่างไร

![React-render-without-activity](@assets/react-19-2/react-render-without-activity.png)

จะเห็นได้ว่า Time to interact จะอยู่ที่ประมาณ 6 วินาที (แต่ sidebar จะถูก render อยู่เบื้องหลังเพื่อให้ user สามารถ interect ได้ก่อน)

## Activity

`<Activity>` เป็น component ใหม่ใน React 19.2 ที่ช่วยให้เราสามารถควบคุมและจัดลำดับความสำคัญ (prioritize) ในการ render component ได้ดีขึ้น โดยเฉพาะอย่างยิ่งในสถานการณ์ที่มี component หลายๆ ตัวที่ต้องการจัดการการแสดงผลแบบมีเงื่อนไข

```tsx
<Activity mode={'visible' | 'hidden'}>
  <Component />
</Activity>
```

| Value | Description |
| -------- | ------------------------------------------------------- |
| visible  | component จะถูก render และแสดงผลตามปกติ พร้อมรับ priority สูงในการ update  |
| hidden   | component จะถูก render แต่ไม่แสดงผล และได้รับ priority ต่ำกว่า เหมาะสำหรับ component ที่อาจจะต้องใช้ในอนาคตใกล้  |

## What Changes/Impact

```tsx
const Page = () => {
  const [isOpenSidebar, setIsOpenSidebar] = useState<boolean>(false)

  return (
    <ThemeProvider>
      <Header />
      <Body />
      <Footer />
      <Activity mode={isOpenSidebar ? 'visible' : 'hidden'}>
        <Sidebar />
      </Activity>
    </ThemeProvider>
  )
}
```

เมื่อเราใช้ Activity มาคุม component มาลองดู Timeline ของการ Paint เวลาเจอว่า isOpenSidebar มีการเปลี่ยนแปลงกันว่า react ใช้วิธีการ process อย่างไร

![React-render-activity](@assets/react-19-2/react-render-with-activity.png)

จะเห็นได้ว่า Time to interact จะอยู่ที่ประมาณ 6 วินาที (แต่ sidebar จะถูก render อยู่เบื้องหลังเพื่อให้ user สามารถ interect ได้ก่อน)

** การใช้ CSS `display:none` ไม่ได้ทำให้ dom ของ sidebar หายไปหมายความว่า behavior ยังเป็นแบบที่ไม่ได้ใช้ Activity component

**การจัดการ State:**

เมื่อไม่ได้ใช้ `<Activity>` component ลูกจะถูก reset state ทุกครั้งที่มีการเปลี่ยนแปลงการแสดงผล

แต่เมื่อใช้ `<Activity>` แล้ว state ของ component จะถูกรักษาไว้ แม้ว่า component จะอยู่ในโหมด `hidden`

นี่คือข้อดีสำคัญของ Activity component ที่ช่วยป้องกันการสูญเสีย state ที่ผู้ใช้อาจจะต้องการให้คงอยู่ เช่น form data, scroll position, หรือ local component state อื่นๆ

![React-render-compare-activity](@assets/react-19-2/react-activity.gif)

สามารถทดลองเล่นได้ที่นี่

[react-sandbox-activity](https://codesandbox.io/p/sandbox/vhlk2l).

## Conclusion from writer

`<Activity />` component ใน React 19.2 เป็นนวัตกรรมที่น่าสนใจมากสำหรับการจัดการ performance และ user experience โดยให้ประโยชน์หลักดังนี้:

### ข้อดีที่ได้รับ
- **ปรับปรุงเวลาในการตอบสนอง**: ลดระยะเวลาที่ผู้ใช้งานต้องรอเพื่อโต้ตอบกับหน้าจอ
- **รักษา State**: component ที่อยู่ในโหมด `hidden` จะคงสถานะไว้ ไม่ต้อง reset ใหม่
- **จัดลำดับความสำคัญ**: React สามารถจัดการ priority ในการ render ได้ดีขึ้น

### เมื่อไหร่ควรใช้
- Modal, Sidebar, หรือ component ที่มีการแสดง/ซ่อนบ่อยๆ
- Component ที่มี state หรือ data สำคัญที่ไม่ต้องการให้สูญหาย
- แอปพลิเคชันที่ต้องการ performance ที่ดีขึ้นในการ switch ระหว่าง view

`<Activity />` จึงเป็นเครื่องมือที่ทรงพลังสำหรับ developer ที่ต้องการทั้ง performance และ user experience ที่ดีขึ้น ลองนำไปใช้ดูครับ!
