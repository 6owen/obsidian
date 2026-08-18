---
title: JavaScript 核心机制
type: concept
aliases: []
tags:
  - llm-wiki
  - javascript
sources:
  - "[[obsidian/面试/八股/2. 原型、原型链、构造函数、普通函数、箭头函数]]"
  - "[[obsidian/面试/八股/3. 作用域、作用域链、闭包、柯里化]]"
  - "[[obsidian/面试/八股/4. this、call 、apply 、bind、new]]"
  - "[[obsidian/面试/八股/7. Promise]]"
  - "[[obsidian/面试/八股/8. Event Loop 事件循环]]"
created: 2026-08-18
updated: 2026-08-18
status: draft
---

# JavaScript 核心机制

## 对象与原型

- 对象访问缺失属性时沿原型链继续查找。
- 函数本身也是对象；构造函数是以 `new` 调用并参与对象初始化的函数。
- 构造函数的 `prototype` 指向原型对象，原型对象的 `constructor` 指回构造函数。

## 作用域与闭包

- 作用域链描述变量从当前作用域向外层逐级查找的过程。
- 闭包由函数及其关联的词法环境组成，使函数离开创建位置后仍能访问原环境。
- 闭包适合保存私有状态和生成器，但持续引用也会延长关联对象的生命周期。

## `this` 与函数调用

- 普通函数中的 `this` 取决于调用方式：默认、对象调用、`call/apply/bind` 显式绑定或 `new`。
- 箭头函数不创建自己的 `this`，而是捕获外层词法 `this`。

## 异步模型

- Event Loop 以任务为单位推进；一个宏任务结束后先清空微任务队列，再进入下一个宏任务。
- Promise 提供 pending/fulfilled/rejected 状态和异步组合方式。
- 原始笔记把 Promise 与地图瓦片并发限制、业务时间线串并行控制联系起来，说明知识来自实际场景而非只为面试。

## 数组与集合

相关笔记还覆盖 Array 构造、Map/Set、reduce、`for`/`forEach`/`for in`/`for of` 和数组/树互转。入口见[[wiki/sources/前端、工程化与面试资料]]。

