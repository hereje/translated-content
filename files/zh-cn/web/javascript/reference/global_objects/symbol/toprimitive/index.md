---
title: Symbol.toPrimitive
slug: Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive
---

{{JSRef}}

**`Symbol.toPrimitive`** 是内置的 symbol 属性，其指定了一种接受首选类型并返回对象原始值的表示的方法。它被所有的[强类型转换制](/zh-CN/docs/Web/JavaScript/Data_structures#强类型转换制)算法优先调用。

{{EmbedInteractiveExample("pages/js/symbol-toprimitive.html")}}

{{js_property_attributes(0,0,0)}}

## 描述

在 `Symbol.toPrimitive` 属性（用作函数值）的帮助下，对象可以转换为一个原始值。该函数被调用时，会被传递一个字符串参数 `hint`，表示要转换到的原始值的预期类型。`hint` 参数的取值是 `"number"`、`"string"` 和 `"default"` 中的任意一个。

`"number"` hint 用于[强制数字类型转换](/zh-CN/docs/Web/JavaScript/Data_structures#强制数字类型转换)算法。`"string"` hint 用于[强制字符串类型转换](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String#string_coercion)算法。`"default"` hint 用于[强制原始值转换](/zh-CN/docs/Web/JavaScript/Data_structures#强制原始值转换)算法。`hint` 仅是作为首选项的偏弱的信号提示，实现时，可以自由忽略它（就像 [`Symbol.prototype[@@toPrimitive]()`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/@@toPrimitive) 一样）。该语言不会在 `hint` 和结果类型之间强制校正，尽管 `[@@toPrimitive]()` 必须返回一个原始值，否则将抛出 {{jsxref("TypeError")}}。

没有 `@@toPrimitive` 属性的对象通过以不同的顺序调用 `valueOf()` 和 `toString()` 方法将其转换为原始值，这在[强制类型转换](/zh-CN/docs/Web/JavaScript/Data_structures#强制类型转换)部分进行了更详细的解释。`@@toPrimitive` 允许完全控制原始转换过程。例如，[`Date.prototype[@@toPrimitive]`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/@@toPrimitive) 将 `"default"` 视为 `"string"` 并且调用 `toString()` 而不是 `valueOf()`。[`Symbol.prototype[@@toPrimitive]`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/@@toPrimitive) 忽略 hint，并总是返回一个 symbol，这意味着即使在字符串上下文中，也不会调用 {{jsxref("Symbol.prototype.toString()")}}，并且 `Symbol` 对象必须始终通过 [`String()`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/String) 显式转换为字符串。

## 示例

### 修改从对象转换的原始值

以下示例描述了 `Symbol.toPrimitive` 属性如何修改从对象转换的原始值。

```js
// 一个没有提供 Symbol.toPrimitive 属性的对象，参与运算时的输出结果。
const obj1 = {};
console.log(+obj1); // NaN
console.log(`${obj1}`); // "[object Object]"
console.log(obj1 + ""); // "[object Object]"

// 接下面声明一个对象，手动赋予了 Symbol.toPrimitive 属性，再来查看输出结果。
const obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint === "number") {
      return 10;
    }
    if (hint === "string") {
      return "hello";
    }
    return true;
  },
};
console.log(+obj2); // 10  — hint 参数值是 "number"
console.log(`${obj2}`); // "hello"   — hint 参数值是 "string"
console.log(obj2 + ""); // "true"    — hint 参数值是 "default"
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参见

- {{jsxref("Date.@@toPrimitive", "Date.prototype[@@toPrimitive]()")}}
- {{jsxref("Symbol.@@toPrimitive", "Symbol.prototype[@@toPrimitive]()")}}
- {{jsxref("Object.prototype.toString()")}}
- {{jsxref("Object.prototype.valueOf()")}}
