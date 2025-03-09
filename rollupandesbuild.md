## 学习笔记
esbuild和rollup都是打包的工具，但是esbuild采用go语言编写，rollup采用js语言编写，所以esbuild打包速度更快，但是rollup打包出来的代码更小，所以根据项目需求选择打包工具。

### 总结

##### 如果追求开发环境速度，那么选择esbuild。

##### 如果追求生产环境代码提交那么选择rollup。引文rollup的插件系统更丰富，以及tree shaking功能更强大。