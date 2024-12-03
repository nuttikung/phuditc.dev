---
author: Phudit Chuengjaroen
pubDatetime: 2024-12-07T17:00:00Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Router ใหม่ที่ช่วยให้ developer ทำงานง่ายขึ้นไปกับ type safe
slug: tanstack-react-route-with-type-safe
featured: false
draft: true
tags:
  - tanstack
  - router
  - react
  - typescript
  - type-safe
description: Rounter ใหม่ที่มาพร้อมความสามารถที่เยอะขึ้นและทำให้ประสบการณ์ของนักพัฒนาดีขึ้นด้วยการสนับสนุน typesafe
---

หากพูดถึงการพัฒนาเว็บก็คงหลีกเลี่ยงในส่วน **Application Routing** ไม่ได้ ซึ่งในอดีตก็ได้มีหลากหลายทางเลือกในบางครั้งก็มีการทำ sync router กับ Data store (เช่น redux และ mobx) เชื่อว่าหลายๆคนคุ้นเคย **_react router dom_** แต่ก็ยังมี remix router ด้วยและแล้ววันนึงทีม remix พวกเขาก็ทนไม่ไหวจนต้องเข้าวงไปรวมพลังกันพัฒนา react router dom v7 ออกมา แต่นั่นไม่ใช่ประเด็นหลักครับเพราะก่อนหน้าที่จะมี v7 ออกมาผมได้ไปเจอ **TanStack Router** มาครับ ซึ่งมีหลายอย่างที่ผมตามหาเช่น type safe

## Table of contents

## Overview

สำหรับ Tanstack Router ได้เกิดมาเพื่อเป็น Router ที่น่าสนใจอีกตัวนึงเพราะว่ามีคุณสมบัติตามข้ออ้างอิงด้านล่างนี้

- 100% inferred TypeScript support (สามารถอ้างอิง type ได้ 100%)
- Typesafe navigation (navigate ฟังค์ชั่นแบบ typesafe เช่น `router.push(...) <-ตรงนี้จะมี auto complete ได้เลยจาก application path ที่มี`)
- Nested Routing and layout routes (สามารถทำ routing แบบซ้อนกันได้รวมถึงการทำ Layout route)
- Automatic route prefetching (สามารถทำ prefetch ได้ด้วย เช่น user เอาเม้าส์ไปชี้ที่ลิ้งค์แต่ยังไม่คลิกก็จะมีการเตรียมหน้าต่อไปรอ พอ user คลิกก็เจอหน้าต่อไปแบบไวขึ้นเพราะมีการโหลดรอแล้ว)
- File-based Route Generation (มีการทำ route ผ่านการวาง file และ folder แล้วสามารถ generate route ออกมาได้เลย)
- Path and Search Parameter Schema Validation (สามารถทำการ validate search params ได้ด้วยรวมถึงใช้คู่กับ zod ได้เลย)

จากที่กล่าวมาก็มีฟังก์ชั่นให้เยอะเหมือนกันที่จริงยังมีอีก สามารถไปตามอ่านได้จาก [ลิงค์นี้](https://tanstack.com/router/latest/docs/framework/react/overview)

## Installation

```bash
npm install @tanstack/react-router
# or
pnpm add @tanstack/react-router
# or
yarn add @tanstack/react-router
# or
bun add @tanstack/react-router
# or
deno add npm:@tanstack/react-router
```

ต่อมาเราจะลงปลั๊กอินเพื่อทำ file based routing กับ vite react typescript กัน

```bash
npm install -D @tanstack/router-plugin @tanstack/router-devtools
# or
pnpm add -D @tanstack/router-plugin @tanstack/router-devtools
# or
yarn add -D @tanstack/router-plugin @tanstack/router-devtools
# or
bun add -D @tanstack/router-plugin @tanstack/router-devtools
# or
deno add npm:@tanstack/router-plugin npm:@tanstack/router-devtools
```

แก้ไขไฟล์ `vite.config.ts`

```ts
// vite.config.ts
import { defineConfig } from "vite";
import viteReact from "@vitejs/plugin-react";
import { TanStackRouterVite } from "@tanstack/router-plugin/vite";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    TanStackRouterVite(), // เพิ่มตรงนี้ไปก่อน @vitejs/plugin-react
    viteReact(),
    // ...,
  ],
});
```
