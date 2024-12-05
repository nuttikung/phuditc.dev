---
author: Phudit Chuengjaroen
pubDatetime: 2024-12-05T10:00:00Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Router ใหม่ที่ช่วยให้ developer ทำงานง่ายขึ้นไปกับ type safe
slug: tanstack-react-route-with-type-safe
featured: true
draft: false
tags:
  - tanstack
  - router
  - react
  - typescript
  - type-safe
  - zod
description: Rounter ใหม่ที่มาพร้อมความสามารถที่เยอะขึ้นและทำให้ประสบการณ์ของนักพัฒนาดีขึ้นด้วยการสนับสนุน typesafe
---

หากพูดถึงการพัฒนาเว็บก็คงหลีกเลี่ยงในส่วน **Application Routing** ไม่ได้ ซึ่งในอดีตก็ได้มีหลากหลายทางเลือกในบางครั้งก็มีการทำ sync router กับ Data store (เช่น redux หรือ mobx) เชื่อว่าหลายๆคนคุ้นเคย **_react router dom_** แต่ก็ยังมี remix router ด้วยและแล้ววันนึงทีม remix พวกเขาก็เข้าไปร่วมพัฒนา react router dom v7 ออกมา แต่นั่นไม่ใช่ประเด็นหลักครับเพราะก่อนหน้าที่จะมี v7 ออกมาผมได้ไปเจอ **TanStack Router** มาครับ ซึ่งมีหลายอย่างที่ผมตามหาเช่น type safe

## Table of contents

## Overview

สำหรับ Tanstack Router ได้เกิดมาเพื่อเป็น Router ที่น่าสนใจอีกตัวนึงเพราะว่ามีคุณสมบัติตามข้ออ้างอิงด้านล่างนี้

- 100% inferred TypeScript support (สามารถอ้างอิง type ได้ 100%)
- Typesafe navigation (navigate ฟังค์ชั่นแบบ typesafe เช่น `router.push(...) <-ตรงนี้จะมี auto complete ได้เลยจาก application path ที่มี`)
  ![type safe](@assets/tanstack-router/type-safe-routing.png)
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

## File Based Routing

ต่อมาเรามาลองทำการ Routing แบบ File based กัน ให้สร้างไฟล์ `src/routes/__root.tsx` `src/routes/index.lazy.tsx` `src/routes/about.lazy.tsx` มา โดยที่ `__root.tsx` จะเป็นไฟล์หลักของการทำ Route ทั้งหมด จากตัวอย่างนี้แปลว่าเราจะมี Route ของ index (/) กับ about (/about) และทั้งสอง route มีการทำ **lazy load** จาก **.lazy**

```tsx
// src/routes/__root.tsx
import { createRootRoute, Link, Outlet } from "@tanstack/react-router";
import { TanStackRouterDevtools } from "@tanstack/router-devtools";

export const Route = createRootRoute({
  component: () => (
    <>
      <div className="flex gap-2 p-2">
        <Link to="/" className="[&.active]:font-bold">
          Home
        </Link>
        <Link to="/about" className="[&.active]:font-bold">
          About
        </Link>
      </div>
      <hr />
      <Outlet />
      <TanStackRouterDevtools />
    </>
  ),
});
```

```tsx
// src/routes/index.lazy.tsx
import { createLazyFileRoute } from "@tanstack/react-router";

export const Route = createLazyFileRoute("/")({
  component: Index,
});

function Index() {
  return (
    <div className="p-2">
      <h3>Welcome Home!</h3>
    </div>
  );
}
```

```tsx
// src/routes/about.lazy.tsx
import { createLazyFileRoute } from "@tanstack/react-router";

export const Route = createLazyFileRoute("/about")({
  component: About,
});

function About() {
  return <div className="p-2">Hello from About!</div>;
}
```

จากนั้นเรากลับมาอัพเดท `main.tsx`

```tsx
import { StrictMode } from "react";
import ReactDOM from "react-dom/client";
import { RouterProvider, createRouter } from "@tanstack/react-router";

// Import the generated route tree
import { routeTree } from "./routeTree.gen";

// Create a new router instance
const router = createRouter({ routeTree });

// Register the router instance for type safety
declare module "@tanstack/react-router" {
  interface Register {
    router: typeof router;
  }
}

// Render the app
const rootElement = document.getElementById("root")!;
if (!rootElement.innerHTML) {
  const root = ReactDOM.createRoot(rootElement);
  root.render(
    <StrictMode>
      <RouterProvider router={router} />
    </StrictMode>
  );
}
```

ภาพด้านล่างคือผลลัพธ์จากการใช้ File Based Routing
![File Based Routing](@assets/tanstack-router/basic-routing.gif)

## Devtools

ในตัว Tanstack router มีเครื่องมือมาของเดฟให้ใช้ด้วย (Optional) เครื่องมือตัวนี้ก็จะบอกว่า

- แอพมี Routing ไหน
- Route มีใน Cache หรือยัง ?
- Nested Path หรือ Path
- Data Loader สถานะอะไร

ตัวอย่างผมจะลองทำ Prefetch ก่อนไปหน้า about
![Router Devtools](@assets/tanstack-router/devtools.gif)

## Nested Route

ต่อมาจะมาแนะนำวิธีการทำ Nested Route กันซึ่งจะเป็นแบบ File Based เหมือนเดิมครับ อ้างอิงจาก [TanstackRouter](https://tanstack.com/router/latest/docs/framework/react/guide/file-based-routing#flat-routes)

```sh
# ตัวอย่างของ Folder Structure
├── __root.tsx
├── index.tsx
├── about.tsx
├── posts/
│   ├── index.tsx
│   ├── $postId.tsx
├── posts.$postId.edit.tsx
├── settings/
│   ├── profile.tsx
│   ├── notifications.tsx
│── _layout.tsx
├── _layout/
│   ├── layout-a.tsx // Nested Route
├── ├── layout-b.tsx // Nested Route
├── files/
│   ├── $.tsx
```

| Path                    | Directory/File Location    | Render                        |
| ----------------------- | -------------------------- | ----------------------------- |
|                         | `__root__`                 | Root                          |
| /                       | index.tsx                  | Root -> RootIndex             |
| /about                  | about.tsx                  | Root -> About                 |
| /posts                  | posts/index.tsx            | Root -> PostsIndex            |
| /posts/1                | posts/$postId.tsx          | Root -> Post                  |
| /posts/1/edit           | posts.$postId.edit.tsx     | Root -> EditPost              |
| /settings/profile       | settings/profile.tsx       | Root -> ProfileSettings       |
| /settings/notifications | settings/notifications.tsx | Root -> NotificationsSettings |
|                         | `_layout`                  | Root -> Layout                |
| /layout-a               | \_layout/layout-a.tsx      | Root -> Layout -> LayoutA     |
| /layout-b               | \_layout/layout-b.tsx      | Root -> Layout -> LayoutB     |
| /files/\*               | files/$.tsx                | Root -> Files                 |

จากตารางถ้าเราจำทำตัว Nested Route ก็คือตรง Layout จะเป็นการ Render Layout ก่อนแล้วต่อด้วย LayoutA หรือ LayoutB

```tsx
// _layout.tsx
import { createFileRoute, Outlet } from "@tanstack/react-router";

export const Route = createFileRoute("/_layout")({
  component: RouteComponent,
});

function RouteComponent() {
  return (
    <div className="flex h-screen">
      <div className="m-auto">
        <h1 className="mb-4 text-3xl">
          Here our layout component to wrap our routes
        </h1>
        <Outlet />
      </div>
    </div>
  );
}
```

ต่อมาถ้าเรามาดูผลลัพธ์ตอนที่เข้าหน้า `/layout-a` หรือ `/layout-b` จะมี Layout ก่อนแล้วถึงจะ Render Component ต่อไป

![Laout Component A](@assets/tanstack-router/layout-path-a.png)
![Laout Component B](@assets/tanstack-router/layout-path-b.png)

## Search Parameter Schema Validation

ในหัวข้อนี้ก็จะเป็นการนำ zod มาใช้เพื่อ Validate params ของเราให้เป็นไปอย่างที่ควรจะเป็นหรือตามที่เราทำนายไว้ (predicate)

สมมติว่าเรามีหน้า Posts -> Index (ที่แสดงโพสต์แบบมี pagination) แล้วเราต้องการ validate page = number อย่างเดียว ถ้ามาเป็น string เราจะ default เป็น 1

### Install Zod

```bash
npm install zod       # npm
yarn add zod          # yarn
bun add zod           # bun
pnpm add zod          # pnpm
```

### Validation

```tsx
// routes/posts/index.tsx

import { createFileRoute } from "@tanstack/react-router";

import { z } from "zod";

const postsSearchSchema = z.object({
  page: z.number().catch(1),
});

type PostsSearch = z.infer<typeof postsSearchSchema>;

export const Route = createFileRoute("/posts/")({
  validateSearch: (search: Record<string, unknown>) =>
    postsSearchSchema.parse(search),
  component: RouteComponent,
});

function RouteComponent() {
  return <div>Here is our PostIndex component</div>;
}
```

`validateSearch` เอาไว้ Validate Search Params

ตอนนี้ถ้าเราเข้า `/posts?page=abcd'` จะถูกเปลี่ยน search params เป็น `page=1` ทันที

![Search Params Validate](@assets/tanstack-router/search-params-validate.gif)

## Conclusion from writer

Tanstack Router ทำมาได้ค่อนข้างดี เมื่อเทียบกับตัว react router dom v6 (ซึ่งปัจจุบัน react dom ได้อัพเดทเป็น version 7 และมีแนวคิดเรื่อง Flat Route หรือ support ใน Type บางอย่าง) ในส่วนของ Data Loader ก็ทำมาได้ค่อนข้างดี แถมยังมี Global Notfound หรือ Custom แต่ละ route ได้อีกและในการทำ Lazy โหลดก็ง่ายมากอีกด้วย
