# [React-Native](http://reactnative.cn/docs/0.46/getting-started.html)

release 热更新：pushy,codepush

#### [RN中文资源网站](https://github.com/reactnativecn/react-native-guide)

#### [Redux](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

react-native init MyApp --version 0.44.3

如果直接执行react-native init MyApp 的话会默认使用RN的最新版本，以上命令则可选择之前稳定的版本，通常在初始化时需要作出以上处理


## Hello World
```
import React, { Component } from 'react';
import { AppRegistry, Text } from 'react-native';//注册Text组件，Text组件是专门用来显示文本的组件，以此类推Image组件是用来显示图片的，因此，项目中需要用到什么UI控件，都可以在这里进行注册

class HelloWorldApp extends Component {
  render() {//在render方法中返回一些用于渲染结构的JSX语句
    return (
      <Text>Hello world!</Text>
    );//JSX语法，在js中嵌入xml
  }
}

// 注意，这里用引号括起来的'HelloWorldApp'必须和你init创建的项目名一致
AppRegistry.registerComponent('HelloWorldApp', () => HelloWorldApp);

```

## props 属性
大多数组件在创建时就可以使用各种参数来进行定制。用于定制的这些参数就称为props（属性）。

以常见的基础组件Image为例，在创建一个图片时，可以传入一个名为source的prop来指定要显示的图片的地址，以及使用名为style的prop来控制其尺寸。

```
import React, { Component } from 'react';
import { AppRegistry, Image } from 'react-native';

class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
      <Image source={pic} style={{width: 193, height: 110}} />// source和style为image控件的属性，分别控制内容和位置
    );
  }
}

AppRegistry.registerComponent('Bananas', () => Bananas);
```

#### 自定义props
自定义的组件也可以使用props。通过在不同的场景使用不同的属性定制，可以尽量提高自定义组件的复用范畴。只需在render函数中引用this.props，然后按需处理即可


```
import React, { Component } from 'react';
import {
  AppRegistry,
  Text,
  View
} from 'react-native';

class Head extends Component {//自定义类类名首字母大写，否则会找不到这个类
  render() {
    return (
      <Text>Hello---{this.props.content}!</Text>
      );
  }
}

export default  class HelloWorld extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Head content='Rexxar' />
        <Head content='Jaina' />
        <Head content='Valeera' />
      </View>
    );
  }
}

AppRegistry.registerComponent('HelloWorld', () => HelloWorld);
```

## state状态
我们使用两种数据来控制一个组件：props和state。props是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变。 对于需要改变的数据，我们需要使用state。

一般来说，你需要在constructor中初始化state（译注：这是ES6的写法，早期的很多ES5的例子使用的是getInitialState方法来初始化state，这一做法会逐渐被淘汰），然后在需要修改时调用setState方法。

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个prop。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到state中。


```
class Blink extends Component {
  constructor(props){//在constructor（构造器）初始化属性的state
    super(props);
    this.state = { showtext : true};

    setInterval(() => {
      this.setState(previousState => {
        return { showtext : !previousState.showtext };
      });
    }, 1000);
  }
  render() {
    let display = this.state.showtext ? this.props.text : ' ';
    return (
      <Text>{display}</Text>
      );
  }
}

export default  class HelloWorld extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
       <Blink text = 'i love to blink'/>
       <Blink text = '闪闪闪'/>
      </View>
    );
  }
}
```
## 样式 style

样式名遵循驼峰命名法则
实际开发建议使用 StyleSheet.create集中定义样式

style可以传入数组，数组中居后的优先级较高。

```
export default  class HelloWorld extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
       <Text style = {styles.bigblue}> bigblue </Text>
       <Text style = {styles.red}> red </Text>
       <Text style = {[styles.red, styles.bigblue]}>red, blue</Text>//这里就展示了数组中的居后对象优先级更高，因此，本段文字显示为大蓝
       <Text style = {[styles.bigblue, styles.red]}>bigblue, red</Text>//这里就展示了数组中的居后对象优先级更高，因此，本段文字显示为红
      </View>
    );
  }
}

const styles = StyleSheet.create({//定义一个样式表，单词别写错了，否则报奇怪的错误
  bigblue : {
    color : 'blue',
    fontWeight : 'bold',
    fontSize : 30
  },
  red : {
    color : 'red',
  },
});
```

## 宽高
最简单的给组件设定尺寸的方式就是在样式中指定固定的width和height。React Native中的尺寸都是无单位的，表示的是与设备像素密度无关的逻辑像素点。


```
export default  class HelloWorld extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
       <View style = {{width : 50, height : 50, backgroundColor : 'powderblue'}} />
       <View style = {{width : 100, height : 100, backgroundColor : 'skyblue'}} />
      </View>
    );
  }
}
```

====================

注意:
Text的style的写法

```
<Text stlye = {  }></Text>
```

View的style

```
<View style = {{  }} />
```
====================

### Flex(弹性) 宽高
在组件样式中使用flex可以使其在可利用的空间中动态地扩张或收缩。一般而言我们会使用flex:1来指定某个组件扩张以撑满所有剩余的空间。如果有多个并列的子组件使用了flex:1，则这些子组件会平分父容器中剩余的空间。如果这些并列的子组件的flex值不一样，则谁的值更大，谁占据剩余空间的比例就更大（即占据剩余空间的比等于并列组件间flex值的比）。

组件能够撑满剩余空间的前提是其父容器的尺寸不为零。如果父容器既没有固定的width和height，也没有设定flex，则父容器的尺寸为零。其子组件如果使用了flex，也是无法显示的。


```
export default  class HelloWorld extends Component {
  render() {
    return (
      <View style={{flex : 1}}>
       <View style = {{flex : 1, backgroundColor : 'powderblue'}} />
       <View style = {{flex : 2, backgroundColor : 'skyblue'}} />
      </View>
    );
  }
}
```
## FlexBox
[FlexBox图解](http://weibo.com/1712131295/CoRnElNkZ?ref=collection&type=comment#_rnd1501832574927)

包含三个属性

#### `flexDirection`:在组件的style中指定flexDirection可以决定布局的主轴。子元素是应该沿着水平轴(row)方向排列，还是沿着竖直轴(column)方向排列呢？默认值是竖直轴(column)方向。

* row 从左往右
* column 从上向下
* row-reverse 从右向左
* column-reverse 从下往上

#### `Justify Content`: 在组件的style中指定justifyContent可以决定其子元素沿着主轴的排列方式。子元素是应该靠近主轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：flex-start、center、flex-end、space-around以及space-between。

* flex-start 
* center
* flex-end 
* space-between
* space-around

#### `Align Items`:在组件的style中指定alignItems可以决定其子元素沿着次轴（与主轴垂直的轴，比如若主轴方向为row，则次轴方向为column）的排列方式。子元素是应该靠近次轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：flex-start、center、flex-end以及stretch。


>注意：要使stretch选项生效的话，子元素在次轴方向上不能有固定的尺寸。以下面的代码为例：只有将子元素样式中的width: 50去掉之后，alignItems: 'stretch'才能生效。

具体的布局变化可以通过代码来观察
```
export default  class HelloWorld extends Component {
  render() {
    return (
      <View style={{flex : 1, flexDirection : 'row', justifyContent : 'space-around', alignItems : 'stretch'}} >
       <View style = {{width : 50,  backgroundColor : 'powderblue'}} />
       <View style = {{width : 50,  backgroundColor : 'skyblue'}} />
       <View style = {{width : 50,  backgroundColor : 'steelblue'}} />
      </View>
    );
  }
}
```

## 文本输入

TextInput是一个允许用户输入文本的基础组件。它有一个名为onChangeText的属性，此属性接受一个函数，而此函数会在文本变化时被调用。另外还有一个名为onSubmitEditing的属性，会在文本被提交后（用户按下软键盘上的提交键）调用。

```
import React, { Component } from 'react';
import { AppRegistry, Text, TextInput, View } from 'react-native';

class PizzaTranslator extends Component {
  constructor(props) {
    super(props);
    this.state = {text: ''};
  }

  render() {
    return (
      <View style={{padding: 10}}>
        <TextInput
          style={{height: 40}}
          placeholder="Type here to translate!"
          onChangeText={(text) => this.setState({text})}
        />
        <Text style={{padding: 10, fontSize: 42}}>
          {this.state.text.split(' ').map((word) => word && '🍕').join(' ')}
        </Text>
      </View>
    );
  }
}
```

### ListView

FlatList:调用tableview的plain格式
通过data属性设置数据源
通过renderItem:设置cell的内容和样式

```
export default  class HelloWorld extends Component {
	render(){
		return(
			<View style = {styles.container}>
			<FlatList 
			data = {[//提供数据源
			{key: 'Devin'},
            {key: 'Jackson'},
            {key: 'James'},
            {key: 'Joel'},
            {key: 'John'},
            {key: 'Jillian'},
            {key: 'Jimmy'},
            {key: 'Julie'},
            ]}

            renderItem = {({item}) => <Text style={styles.item}>{item.key}</Text>}//根据

            />

			</View>
			);
	}
}

const styles = StyleSheet.create({
	container:{
		flex : 1,
		paddingTop : 22,
	},
	item : {
		padding: 10,
		fontSize: 18,
		height: 44,
		color : 'red',
	},
})
```

SectionList:调用tableview 的group样式 

### Fetch 

```
import React, { Component } from 'react';
import {
  AppRegistry,
  Text,
  Button,
  StyleSheet,
  View,
  TouchableOpacity,
  Alert
} from 'react-native';

export default  class HelloWorld extends Component {

	GET(){
        fetch('https://www.baidu.com')
            .then((response) => response.json()) //把response转为json
            .then((responseData) => { // 上面的转好的json
                    alert(responseData); 
            })
            .catch((error)=> {
                alert(error);
            })
    }

	render(){
		return(
			<View style = {styles.container}>
			 <Button
			   onPress = {this.GET}
               title="Learn More"
               color="#841584"
             />
			</View>
			);
	}
}

const styles = StyleSheet.create({
	container:{
		flex : 1,
		paddingTop : 22,
	},
})

AppRegistry.registerComponent('HelloWorld', () => HelloWorld);
```


## RN集成到已有项目中

1.首先当然要了解你要集成的React Native组件。

2.创建一个Podfile，在其中以subspec的形式填写所有你要集成的React Native的组件。

3.创建js文件，编写React Native组件的js代码。

4.添加一个事件处理函数，用于创建一个RCTRootView。这个RCTRootView正是用来承载你的React Native组件的，而且它必须对应你在index.ios.js中使用AppRegistry注册的模块名字。

5.启动React Native的Packager服务，运行应用。
根据需要添加更多React Native的组件。
调试。

6.准备部署发布 （比如可以利用react-native-xcode.sh脚本）。

### 核心科技: RCTRootView


运行Packager

要运行应用，首先需要启动开发服务器（即Packager，它负责实时监测js文件的变动并实时打包，输出给客户端运行）。具体只需简单进入到项目根目录中，然后运行：

> $ npm start

Packager只是在开发时需要，便于你快速开发迭代。在正式发布应用时，所有的js文件都会被打包为一整个jsbundle文件离线运行，此时客户端不再需要Packager服务。