## es6 简介
- 1.ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现（另外的 ECMAScript 方言还有 Jscript 和 ActionScript）。</P>
- 2.ES6 一般是指 ES2015 标准，有时也是泛指“下一代 JavaScript 语言”。</P>
1. Babel 转码器
```
// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
return item + 1;
});
```

- 如何使用 ?
- 配置文件 .babelrc