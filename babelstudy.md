### babel学习笔记
1、babel的配置文件主要有两个配置点，一个配置预设，一个是配置插件，预设和插件的区别就是，预设是一组插件的集合，封装了各种语法转译的规则。插件是针对单个语法或者api的转译规则。所以配置内容大概包含以下内容：
```.babelrc
{
  "presets": ["..."],//这里可以配置多个预设结合
  "plugins": ["..."]//这里可以配置多个插件结合
}
```


2、babel转译代码主要设计转译两个部分，一个是语法上的转译，比如把箭头函数转译成我们常见的普通函数格式，一个是api转译，比如promise、async函数等，babel会向低版本js实现这些api从而让低版本代码具有相关api功能。这里说一下涉及api转译的时候本质上有两种方式：一种在全局上实现新api的封装，比如Promise，babel转译会在全局变量（浏览器中是window）上增加Promise的实现，这种方式的缺点是会污染全局变量，所以不推荐使用。还有一种方式就是局部实现api的封装，不会影响全局变量，这种方式是通过babel插件实现各种api的封装！

3、api转译：在全局变量上实现api的封装的方式，主要是通过@babel/preset-env和core-js实现的。其中core-js是一个js库，它提供了各种api的实现，比如Promise、Array、Object等，@babel/preset-env是一个预设，它会根据我们的配置，向全局变量上增加core-js的实现，从而让低版本代码具有相关api功能。我们的配置如下：  
```.babelrc
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
            "chrome": "58",
            "ie": "11"
        },
        "useBuiltIns": "usage",//这里配置为usage，意思是根据我们的代码，自动引入需要的api
        "corejs": 3,//这里配置为3，意思是引入core-js的3.x版本
      }
    ]
  ]
}
```

4、api转译：在局部实现api的封装的方式，主要是通过@babel/plugin-transform-runtime和core-js实现的。其中core-js是一个js库，它提供了各种api的实现，比如Promise、Array、Object等，@babel/plugin-transform-runtime是一个插件，它会把代码中相关新api的实现通过引入第三方模块的方式，而不是在每个文件中分别实现一次api，这样有助于打包工具，提取公共模块，从而使我们最终打包代码体积作很小优化，我们的配置如下：  
```.babelrc
{
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "corejs": 3,//这里配置为3，意思是引入core-js的3.x版本
        "helpers": true,//这里配置为true，意思是引入core-js的helpers
        "regenerator": true,//这里配置为true，意思是引入regenerator-runtime
        "useESModules": false//这里配置为false，意思是不使用es module的方式引入core-js的helpers和regenerator-runtime，而是使用commonjs的方式引入
      }
    ]
  ]
}
```
