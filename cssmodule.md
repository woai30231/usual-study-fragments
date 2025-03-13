### css module学习笔记

##### 本文以react组件开发为例
1、css module是css的模块化方案，它的主要作用是解决css的命名冲突问题，它通过css的类名生成一个独一无二的类名，从而避免冲突。具体做法就是建立一个以.module.css结尾的css文件，并把该css文件和组件放在一起，用于管理隔离组件的样式、脚本等！
2、在构建阶段，CSS Modules 工具（如 Webpack 的 css-loader）会对 CSS 文件中的每个类名进行处理。它会结合文件名、原始类名以及一个哈希值生成一个新的类名。例如，有一个 CSS 文件 Button.module.css 内容如下：
```.css
.button {
  background-color: blue;
  color: white;
}
```
css-loader 会将 .button 类名转换为类似 .Button_button__abc123 这样的形式，其中 Button 是文件名，button 是原始类名，__abc123 是生成的哈希值。这个哈希值是根据文件内容计算得出的，只要文件内容不变，生成的哈希值就不会变。

3、在对类名进行转换的同时，CSS Modules 会生成一个 JavaScript 对象，该对象的键是原始类名，值是转换后的唯一类名。对于上述 Button.module.css 文件，生成的映射对象可能如下：
```.js
{
  button: 'Button_button__abc123'
}    // 生成的映射对象  
```
4、在 React 组件中，你可以通过 import styles from './Button.module.css' 来引入这个映射对象。然后，你可以使用 styles.button 来获取转换后的类名，从而在组件中使用该类名。
在 React 组件中，我们通过导入 CSS Modules 文件来获取这个映射对象，并使用转换后的类名。示例如下：
```.jsx
import React from 'react';
import styles from './Button.module.css';

const Button = () => {
  return (
    <button className={styles.button}>
      Click me
    </button>
  );
};

export default Button;
```