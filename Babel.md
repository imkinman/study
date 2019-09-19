# Babel（v7.4.0）
1、什么是<br>
&emsp;JS的编译器，主要用于将ES6(ES2015+)语法转为向后兼容的JS语法，从而在目标环境中执行。<br>

2、如何使用<br>
&emsp;① 安装 Babel的核心库 @babel/core<br>
&emsp;&emsp;`npm i -D @babel/core`<br>
&emsp;&emsp;`yarn add -D @babel/core`<br>
&emsp;② 安装 Babel的插件/预设文件 @babel/preset-env<br>
&emsp;&emsp;`npm i -D @babel/preset-env`<br>
&emsp;&emsp;`yarn add -D @babel/preset-env`<br>
&emsp;③ 安装 polyfill <br>
&emsp;&emsp;`npm i -S @babel/polyfill`<br>
&emsp;&emsp;`yarn add -S @babel/polyfill`<br>
&emsp;③ 创建/修改 babel.config.js 文件<br>
```
const preset = [
	[
		"@babel/preset-env",
		{
			targets: {
				ie: "6",
				edge: "10",
				chrome: "60",
				firfox: "10",
				safari: "10"
			}
		},
		"useBuiltIns": "usage"
	]
]
const plugins = []
module.exports = { presets, plugins }
````
3、插件(plugin)<br>
&emsp;① 什么是<br>
&emsp;&emsp;Babel代码转换的功能是以插件的形式出现，定义了Babel转码的规则<br>
&emsp;② 安装需要的插件，如 @babel/plugin-transform-arrow-functions<br>
&emsp;&emsp;`npm i -D @babel/plugin-transform-arrow-functions`<br>
&emsp;&emsp;`yarn add -D @babel/plugin-transform-arrow-functions`<br>
&emsp;③ 修改配置文件
```
const presets = []
const plugins = ["@babel/plugin-transform-arrow-functions"]
module.exports = { presets, plugins }
```

4、预设(preset)<br>
&emsp;① 什么是<br>
&emsp;&emsp; 一组预先设定的插件，即不需要一个接一个的添加所有需要的插件
&emsp;② 安装需要的预设，如 @babel/preset-env<br>
&emsp;&emsp;`npm i -D @babel/preset-env`<br>
&emsp;&emsp;`yarn add -D @babel/preset-env`<br>
&emsp;③ @babel/preset-env 说明<br>
&emsp;&emsp; 它包含的插件支持所有最新的JS语法特性，如果不进行配置，会默认加载所有插件<br>
&emsp;④ @babel/preset-env 参数<br>
&emsp;&emsp;targets: { Object }，目标浏览器的指定版本号，如果设置了该属性，那么 env 的 preset 只会对目标浏览器中没有的功能加载转换插件<br>
&emsp;&emsp;useBuiltIns: { String }，"usage | entry | false"，如果设置为"usage"，那么 @babel/polyfill 只会对目标浏览器中没有的功能加载所需要的 polyfill<br>

5、polyfill<br>
&emsp;① 什么是<br>
&emsp;&emsp; 由于 @babel/preset-env 只有转换JS语法，而不转换API，所以需要 @babel/polyfill 为当前环境提供垫片函数，它包含了 core-js 和 regenerator runtime 两个模块，从而模拟完整ES2015+的环境<br>
&emsp;② 如何使用<br>
&emsp;&emsp;安装<br>
&emsp;&emsp;&emsp;`npm i -S @babel/polyfill`<br>
&emsp;&emsp;&emsp;`yarn add -S @babel/polyfill`<br>
&emsp;&emsp;在所有JS代码之前引入 polyfill<br>
&emsp;&emsp;&emsp;`import "@babel/polyfill"`<br>
&emsp;&emsp;&emsp;`require("@babel/polyfill")`<br>
&emsp;② env "useBuiltIns"参数<br>
&emsp;&emsp;如果将 @babel/preset-env 的"useBuiltIns"设置为"usage"，Babel会检查所有的代码，为目标环境中缺少的功能，添加需要的polyfill，而不用手动引入完整的 polyfill<br>

6、配置<br>
&emsp;① .babelrc<br>
&emsp;&emsp;适用场景：静态配置<br>
```
{
	"presets": ["@babel/preset-env"],
	"plugins": ["@babel/plugin-transform-arrow-functions"]
}
```
&emsp;② package.json<br>
&emsp;&emsp;适用场景：静态配置，只是将 .babelrc 的配置，写进 package.json 文件中
```
{
	"name": "my-project",
	"version": "1.0.0",
	"babel": {
		"presets": [...],
		"plugins": [...]
	}
}
```
&emsp;③ babel.config.js(官方推荐)<br>
&emsp;④ .babelrc.js<br>
&emsp;&emsp;适用场景：上述两种，以编程的方式创建配置，可以基于环境动态配置
```
写法一：
module.exports = api => {
	api.cache(true)	// 必须加上这句话，否则报错
	const presets = [
		"preset-1",
		["preset-2"],
		[
			"@babel/preset-env",
			{
				targets: { ie: "6", chrome: "64" },
				useBuiltIns: "usage"
			}
		]
	]
	const plugins = [
		"plugin-1",
		["plugin-2"],
		["@babel/plugin-transform-arrow-functions", { }]
	]
	return {
		presets,
		plugins
	}
}
写法二：
const presets = [...]
const plugins = [...]
module.exports = { presets, plugins }
```

7、官方插件<br>

版本|插件名|作用|链接
-|-|-|-
ES3|@babel/plugin-transform-member-expression-literals|为对象属性赋值时，对象属性为JS关键字，则将 .属性名 转为 ["属性名"]|<a href="https://www.babeljs.cn/docs/babel-plugin-transform-member-expression-literals" target="_blank">查看</a>
&emsp;|babel-plugin-transform-property-literals|将对象中属性名的引号移除|<a href="https://www.babeljs.cn/docs/babel-plugin-transform-property-literals" target="_blank">查看</a>
&emsp;|@babel/plugin-transform-reserved-words|声明变量时，变量名为保留字时，会在前面加上下划线|<a href="https://www.babeljs.cn/docs/babel-plugin-transform-reserved-words" target="_blank">查看</a>
ES5|@babel/plugin-transform-property-mutators|转换 setter/getter 语法格式|<a href="https://www.babeljs.cn/docs/babel-plugin-transform-property-mutators" target="_blank">查看</a>



