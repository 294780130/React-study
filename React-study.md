# React 学习笔记
## day 01 （初识React）
> React是一个UI库
### React结构总结

```
ReactDOM.render(
	React.DOM.h1(null,"Hello world!"),
	document.getElementById("app")
);
```

#### ReactDOM.render(包含的子结构,渲染的地方);
#### React.DOM.标签(attribute,内容-可以嵌套),标签外的内容
++当在定义attribute为style的时候，需要使用对象的方式才能正确处理（可以减少跨站脚本）++

++并且使用fontFamily代替font-family++
```
ReactDOM.render(
	React.DOM.h1(
		{
			// style:"background:black;color:white;font-family:Verdana",
			style:{
				background:"black",
				color:"white",
				fontFamily:"Verdana"
			},
		},
		"Hello world!"
	),
	document.getElementById("app")
);
```
