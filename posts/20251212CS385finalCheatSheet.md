---
title: CS385 Lab Exam 3: 终极速查表 (Final Cheat Sheet) / CS385 Lab Exam 3: Final Cheat Sheet
title_en: CS385 Lab Exam 3: The Ultimate Final Cheat Sheet - EVERYTHING!
title_zh: CS385 Lab Exam 3：终极速查表——包罗万象！
date: 2025-12-11
categories: CS385
tags: React, JavaScript, CheatSheet, ExamPrep, Objects, Topic10, Debugging, Pitfalls
summary_en: A comprehensive cheat sheet for CS385 Lab Exam 3, including core JavaScript, React, Topic 10, common errors, and debugging techniques.
summary_zh: CS385 Lab Exam 3 考试专用速查表，涵盖核心 JavaScript、React、Topic 10、常见错误和调试技巧。
---

[EN]
# 🚀 CS385 Lab Exam 3: The Ultimate Final Cheat Sheet - EVERYTHING!

This is your final lifeline for the CS385 Lab Exam 3. This cheat sheet is designed to be a comprehensive resource, covering all essential topics, common pitfalls, and debugging tips. Let's conquer this exam!
[END]

[ZH]
# 🚀 CS385 Lab Exam 3：终极速查表——包罗万象！

这是你 CS385 Lab Exam 3 的最后一道防线。 这份速查表旨在成为一个全面的资源，涵盖所有基本主题、常见陷阱和调试技巧。 让我们征服这次考试！
[END]

---

## 1. JavaScript 核心 (JavaScript Fundamentals)

### 1.1 相等性 (Equality)

[EN]
| Feature | `==` (Loose) | `===` (Strict) | `Object.is()` |
|---|---|---|---|
| Type Coercion | ✅ Yes | ❌ No | ❌ No |
| Object Comparison | Compares references | Compares references | Compares references |
| `5` vs `'5'` | `true` | `false` | `false` |
| `null` vs `undefined` | `true` | `false` | `false` |
| `NaN` vs `NaN` | `false` | `false` | ✅ `true` |
| `+0` vs `-0` | `true` | `true` | ✅ `false` |
[END]

[ZH]
| 特性 | `==` (宽松) | `===` (严格) | `Object.is()` |
|---|---|---|---|
| 类型转换 | ✅ 是 | ❌ 否 | ❌ 否 |
| 对象比较 | 比较引用 | 比较引用 | 比较引用 |
| `5` vs `'5'` | `true` | `false` | `false` |
| `null` vs `undefined` | `true` | `false` | `false` |
| `NaN` vs `NaN` | `false` | `false` | ✅ `true` |
| `+0` vs `-0` | `true` | `true` | ✅ `false` |
[END]

### 1.2 引用类型 (Reference Types)

[EN]
*   **Object Comparison:** Two separate objects are never equal.
    ```javascript
    const a = {}; const b = {};  // a !== b
    const c = a; // a === c
    ```
[END]

[ZH]
*   **对象比较：** 两个独立的对象永远不相等。
    ```javascript
    const a = {}; const b = {};  // a !== b
    const c = a; // a === c
    ```
[END]

### 1.3 数组操作 (Array Operations)

[EN]
| Method | Description | Mutates Original? |
|---|---|---|
| `splice()` | Adds/removes elements | ✅ Yes |
| `slice()` | Returns new array | ❌ No |
| `map()` | Creates new array, transforms elements | ❌ No |
| `filter()` | Creates new array, filters elements | ❌ No |
| `reduce()` | Accumulates a single value | ❌ No |
[END]

[ZH]
| 方法 | 描述 | 是否修改原数组？ |
|---|---|---|
| `splice()` | 添加/删除元素 | ✅ 是 |
| `slice()` | 返回新数组 | ❌ 否 |
| `map()` | 创建新数组，转换元素 | ❌ 否 |
| `filter()` | 创建新数组，过滤元素 | ❌ 否 |
| `reduce()` | 累加单个值 | ❌ 否 |
[END]

### 1.4 对象操作 (Object Operations)

[EN]
| Method | Description | Example |
|---|---|---|
| `.keys()` | Returns array of keys | `Object.keys({a:1, b:2})` -> `['a', 'b']` |
| `.values()` | Returns array of values | `Object.values({a:1, b:2})` -> `[1, 2]` |
| `.entries()` | Returns array of [key, value] pairs | `Object.entries({a:1, b:2})` -> `[['a', 1], ['b', 2]]` |
| `Object.getOwnPropertyNames()` | Returns all property names (including non-enumerable) |  |
| `{...obj}` (Spread) | Shallow copy | `const newObj = {...obj}` |
| Destructuring | Extracts values from objects/arrays | `const { a, b, ...rest } = obj;` |
[END]

[ZH]
| 方法 | 描述 | 示例 |
|---|---|---|
| `.keys()` | 返回键的数组 | `Object.keys({a:1, b:2})` -> `['a', 'b']` |
| `.values()` | 返回值的数组 | `Object.values({a:1, b:2})` -> `[1, 2]` |
| `.entries()` | 返回 [键, 值] 对的数组 | `Object.entries({a:1, b:2})` -> `[['a', 1], ['b', 2]]` |
| `Object.getOwnPropertyNames()` | 返回所有属性名 (包括不可枚举的) |  |
| `{...obj}` (展开) | 浅拷贝 | `const newObj = {...obj}` |
| 解构赋值 | 从对象/数组中提取值 | `const { a, b, ...rest } = obj;` |
[END]

## 2. React 核心 (React Core)

### 2.1 组件 (Components)

[EN]
| Feature | Description |
|---|---|
| Functional Components | Use `function` or arrow functions |
| JSX | JavaScript XML, HTML-like syntax |
| Props | Data passed to components (read-only) |
[END]

[ZH]
| 特性 | 描述 |
|---|---|
| 函数组件 | 使用 `function` 或箭头函数 |
| JSX | JavaScript XML, 类似 HTML 的语法 |
| Props | 传递给组件的数据（只读） |
[END]

### 2.2 状态管理 (State Management)

[EN]
| Hook | Description | Key Points |
|---|---|---|
| `useState` | Manages component state | `const [state, setState] = useState(initialValue);`<br>State updates are asynchronous.<br>State is constant within a render.<br>Use functional updates: `setState(prevState => newState)` |
| `useEffect` | Handles side effects | Dependency array: `[]` (mount/unmount), `[dep]` (when `dep` changes). Cleanup function: `return () => { /* cleanup */ }` (unmount or before next effect) |
| `useCallback` | Memoizes functions | Prevents unnecessary re-renders of child components |
| `useMemo` | Memoizes values | Optimizes performance |
[END]

[ZH]
| Hook | 描述 | 关键点 |
|---|---|---|
| `useState` | 管理组件状态 | `const [state, setState] = useState(initialValue);`<br>状态更新是异步的。<br>状态在一次渲染中是常量。<br>使用函数式更新：`setState(prevState => newState)` |
| `useEffect` | 处理副作用 | 依赖数组：`[]`（挂载/卸载），`[dep]`（当 `dep` 改变时）。 清理函数：`return () => { /* cleanup */ }`（卸载或在下一个 effect 之前） |
| `useCallback` | 记忆函数 | 避免子组件不必要的重新渲染 |
| `useMemo` | 记忆值 | 优化性能 |
[END]

### 2.3 渲染 (Rendering)

[EN]
| Feature | Description |
|---|---|
| React ignores `null`, `undefined`, `boolean` | React ignores these values |
| Conditional Rendering | `&&`, `||`, `? :` |
| List Rendering | Use `map()` and ensure each element has a unique `key` prop |
[END]

[ZH]
| 特性 | 描述 |
|---|---|
| React 忽略 `null`, `undefined`, `boolean` | React 忽略这些值 |
| 条件渲染 | `&&`, `||`, `? :` |
| 列表渲染 | 使用 `map()` 并确保每个元素都有唯一的 `key` 属性 |
[END]

## 3. Topic 10: 对象操作与高级渲染 (Object Manipulation & Advanced Rendering)

### 3.1 嵌套映射 (Nested Mapping)

[EN]
*   **Double Map Pattern:** Iterating nested data structures.

```javascript
// Outer Map: Rows
{data.map((obj, i) => (
  <div key={i}>
     {/* Inner Map: Columns */}
     {Object.getOwnPropertyNames(obj).map(key => (
        <span key={key}>
           {/* Core: Use bracket notation! */}
           {key}: {obj[key]}
        </span>
     ))}
  </div>
))}
```
[END]

[ZH]
*   **双重 Map 模式：** 遍历嵌套数据结构。

```javascript
// 外层 Map: 行
{data.map((obj, i) => (
  <div key={i}>
     {/* 内层 Map: 列 */}
     {Object.getOwnPropertyNames(obj).map(key => (
        <span key={key}>
           {/* 核心：使用方括号！ */}
           {key}: {obj[key]}
        </span>
     ))}
  </div>
))}
```
[END]

### 3.2 `Object.getOwnPropertyNames()`

[EN]
*   Gets all property names (including non-enumerable).
[END]

[ZH]
*   获取所有属性名（包括不可枚举的）。
[END]

### 3.3 `reduce()` with Objects

[EN]
*   Often used to aggregate data based on conditions.

```javascript
const counts = arr.reduce((acc, item) => {
    acc[item] = (acc[item] || 0) + 1;
    return acc;
}, {});
```
[END]

[ZH]
*   通常用于根据条件聚合数据。

```javascript
const counts = arr.reduce((acc, item) => {
    acc[item] = (acc[item] || 0) + 1;
    return acc;
}, {});
```
[END]

### 3.4 Fuzzy Filtering

[EN]
*   Use `includes()`, `toLowerCase()` for string matching.
[END]

[ZH]
*   使用 `includes()`，`toLowerCase()` 进行字符串匹配。
[END]

## 4. 常见陷阱与排错 (Common Pitfalls & Debugging)

### 4.1 Object Equality - The Recurring Nightmare

[EN]
*   Two separate objects are never equal.
*   Causes unnecessary re-renders in `useEffect` and `memo`.
*   Use `JSON.stringify()` (performance implications) or Lodash's `_.isEqual()` for deep comparison.
[END]

[ZH]
*   两个独立的对象永远不相等。
*   导致 `useEffect` 和 `memo` 中不必要的重新渲染。
*   使用 `JSON.stringify()`（性能影响）或 Lodash 的 `_.isEqual()` 进行深度比较。
[END]

### 4.2 `useEffect` Dependencies - The Dependency Dilemma

[EN]
*   Ensure all variables used in the effect are in the dependency array.
*   If the dependency is an object, it can lead to infinite loops. Use `useMemo` or `useCallback` to prevent this.
[END]

[ZH]
*   确保 effect 中使用的所有变量都在依赖项数组中。
*   如果依赖项是对象，可能导致无限循环。 使用 `useMemo` 或 `useCallback` 来防止这种情况。
[END]

### 4.3 Conditional Rendering - The Logic Labyrinth

[EN]
*   Ensure the conditional check is correct.
*   Pay attention to `&&` and `||` precedence.
*   Provide a unique `key` prop for each element in lists.
[END]

[ZH]
*   确保条件判断正确。
*   注意 `&&` 和 `||` 的优先级。
*   为列表中每个元素提供唯一的 `key` 属性。
[END]

### 4.4 Performance Issues - Speed Matters

[EN]
*   Use `React.memo` to wrap components.
*   Use `useMemo`, `useCallback` to avoid unnecessary calculations and function creation.
*   Avoid time-consuming operations during rendering.
[END]

[ZH]
*   使用 `React.memo` 包装组件。
*   使用 `useMemo`，`useCallback` 避免不必要的计算和函数创建。
*   避免在渲染期间进行耗时的操作。
[END]

### 4.5 Quick Debug Tips - Be a Debugging Ninja

[EN]
| Issue | Check | Common Causes |
|---|---|---|
| Infinite Loop | `useEffect` dependencies | Incorrect dependencies; object references |
| State not updating | Object references; `setState` | Direct state modification; incorrect `setState` call |
| Rendering Errors | `key` props; conditional statements | Non-unique `key` props; incorrect conditional logic; JSX syntax errors |
| Data not displaying | Data fetching; data format; data passing | API request failure; incorrect data format; data not passed correctly |
[END]

[ZH]
| 问题 | 检查 | 常见原因 |
|---|---|---|
| 无限循环 | `useEffect` 依赖项 | 不正确的依赖项；对象引用 |
| 状态未更新 | 对象引用；`setState` | 直接修改状态；不正确的 `setState` 调用 |
| 渲染错误 | `key` 属性；条件语句 | 非唯一的 `key` 属性；不正确的条件逻辑；JSX 语法错误 |
| 数据未显示 | 数据获取；数据格式；数据传递 | API 请求失败；不正确的数据格式；数据未正确传递 |
[END]

## 5. 考题预测 (Exam Question Predictions)

[EN]
*   Object Destructuring
*   Nested `map` functions
*   Output of `Object.getOwnPropertyNames()`
*   Object equality
*   Usage of `useEffect` and `memo` (and their pitfalls)
*   Conditional rendering and undefined properties
*   API data fetching and state updates (especially handling API response structure)
*   Fuzzy Filtering
*   Usage of `reduce`
*   `splice` vs `slice`
*   Spread operator (`...`) in objects and arrays (order matters!)
*   Code reading and output prediction
*   Debugging scenarios
*   Understanding React's rendering mechanism
[END]

[ZH]
*   对象解构
*   嵌套 `map` 函数
*   `Object.getOwnPropertyNames()` 的输出
*   对象相等性
*   `useEffect` 和 `memo` 的使用（及其陷阱）
*   条件渲染和未定义属性
*   API 数据获取和状态更新（尤其是处理 API 响应结构）
*   模糊过滤
*   `reduce` 的使用
*   `splice` vs `slice`
*   展开运算符 (`...`) 在对象和数组中的应用（顺序很重要！）
*   代码阅读和输出预测
*   调试场景
*   理解 React 的渲染机制
[END]

---

[EN]
Good luck, you've got this! Stay calm, read the questions carefully, and double-check your answers. You've prepared, now go ace that exam!
[END]

[ZH]
祝你好运，你一定能行！ 保持冷静，仔细阅读问题，并仔细检查你的答案。 你已经准备好了，现在就去征服考试吧！
[END]
