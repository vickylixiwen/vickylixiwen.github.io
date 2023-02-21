---
layout: post
title:  "Typescript学习笔记"
categories: python
---

##### 记录一下学习TS的过程

1. 声明文件必需以 .d.ts 为后缀

2. 在 ES6 模块系统中，使用 export default 可以导出一个默认值，使用方可以用 import foo from 'foo' 而不是 import { foo } from 'foo' 来导入这个默认值。
注意，只有 function、class 和 interface 可以直接默认导出，其他的变量需要先定义出来，再默认导出。

3. JSX 工作模式：preserve 模式和 react 模式以及 react-native 模式。这三个模式只影响编译策略。preserve 模式会生成代码中会保留 JSX ，以供后续的转换操作使用（比如：Babel），输出的文件是 .jsx 格式的；而 react模式则会直接编译成 React.createElement，在使用前就不需要再进行 JSX 转换了，输出的文件是 .js 格式的；react-native模式相当于preserve，它也保留了所有的JSX，但是输出文件的扩展名是.js

4. 