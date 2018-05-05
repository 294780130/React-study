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
#

## day02 （组件的生命周期）
### 自定义组件和调用

```
var MyComponent = React.createClass({
	render:function(){
		return React.DOM.span(null,"I'm so custom");
	}
});
ReactDOM.render(
	React.createElement(MyComponent),	//创建组件实例方法1
	document.getElementById("app")
);
```

#### React.DOM.标签(attribute,内容-可以嵌套) 其实是React.createElement()之上的封装，React.createElement("h1",null,"hello")

### 组件属性的定义和获取

```
var MyComponent = React.createClass({
	render:function(){
		return React.DOM.span(null,"My name is "+this.props.name);
	}
});
ReactDOM.render(
	React.createElement(MyComponent,{name:"Bob"}),	//创建组件实例方法1
	document.getElementById("app")
);
```
### 组件propType属性完成验证

```
var MyComponent = React.createClass({
	propTypes:{
		name:React.PropTypes.string.isRequired,//这里设置要求的数据类型
	},
	render:function(){
		return React.DOM.span(null,"My name is "+this.props.name);
	}
});
ReactDOM.render(
	React.createElement(MyComponent,{name:"Bob"}),	//不定name会报错，定义name:123 会警告，利于调试
	document.getElementById("app")
);
```
### 解决undefined——设置验证以及默认值

```
var MyComponent = React.createClass({
propTypes:{
	firstName:React.PropTypes.string.isRequired,
	middleName:React.PropTypes.string,
	familyName:React.PropTypes.string.isRequired,
	address:React.PropTypes.string
},
getDefaultProps:function(){
	return{
		middleName:"?",
		address:"unknow"
	};
},
render:function(){
	return React.DOM.span(null,"My firstName is "+this.props.firstName+"My middleName is "+this.props.middleName+"My familyName is "+this.props.familyName+"My address is "+this.props.address);
}
});
ReactDOM.render(
React.createElement(MyComponent,{
		firstName:"firstName",
		// middleName:"middleName",
		familyName:"familyName",
		// address:"address"
	}),	//不定name会报错，定义name:123 会警告，利于调试
	document.getElementById("app")
);
```
### state实现监听文本框内容长度

```
var TextAreaCounter = React.createClass({
	propTypes:{
		text:React.PropTypes.string,
	},
	getInitialState:function(){
		return{
			text:this.props.text
		};
	},
	_textChange:function(ev){
		this.setState({
			text:ev.target.value
		});
	},
	getDefaultProps:function(){
		return{
			text:""
		};
	},
	render:function(){
		return React.DOM.div(null,
			React.DOM.textarea({
				value:this.state.text,
				onChange:this._textChange
			}),
			React.DOM.h3(null,this.state.text.length)
		);
	}
});
ReactDOM.render(
	React.createElement(TextAreaCounter,{text:"Bob"}),	//不定name会报错，定义name:123 会警告，利于调试
	document.getElementById("app")
);
```
### react的组件访问（使用控制台控制和访问组件内容）

```
<!DOCTYPE html>
<html>
	<head>
		<title>Component</title>
		<meta charset="utf-8">
	</head>
	<body>
		<div id="app">
			
		</div>
		<script src="react/build/react.js"></script>
		<script src="react/build/react-dom.js"></script>
		<script>
			// 组件propType属性完成验证
			var TextAreaCounter = React.createClass({
				propTypes:{
					text:React.PropTypes.string,
				},
				getInitialState:function(){
					return{
						text:this.props.text
					};
				},
				_textChange:function(ev){
					this.setState({
						text:ev.target.value
					});
				},
				getDefaultProps:function(){
					return{
						text:""
					};
				},
				render:function(){
					return React.DOM.div(null,
						React.DOM.textarea({
							value:this.state.text,
							onChange:this._textChange
						}),
						React.DOM.h3(null,this.state.text.length)
					);
				}
			});
			//相对上一个案例，只增加了一个myTextAreaCounter变量
			var myTextAreaCounter = ReactDOM.render(
				React.createElement(TextAreaCounter,{text:"Bob"}),	//不定name会报错，定义name:123 会警告，利于调试
				document.getElementById("app")
			);
		</script>
	</body>
</html>   
<!-- myTextAreaCounter.setState({text:"Hello,world!"});

var reactAppNode = ReactDOM.findDOMNode(myTextAreaCounter);
reactAppNode;

reactAppNode.parentNode === document.getElementById("app");

myTextAreaCounter.props;

myTextAreaCounter.state; -->
```
### 状态检测函数
+ componentWillUpdate:function(){} 3.在组件更新渲染之前触发
+ componentDidUpdate:function(){}  4.在组件更新渲染之后触发
+ componentWillMount:function(){} 1.在Dom加载组件之前触发
+ componentDidMount:function(){} 2.在Dom加载组件之后触发
+ componentWillUnmount:function(){} 在组件被移除时触发
+ shouldComponentUpdate(newProps,newState) 在触发更新组件渲染的componentWillMount之前触发，可以阻止后续动作