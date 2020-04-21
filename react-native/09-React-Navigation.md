#### 一、安装
###### 1.1 项目初始化完成后，集成React Navigation
在项目的根目录下运行命令：
```
expo install react-navigation react-native-gesture-handler react-native-reanimated react-native-screens
```
###### 1.2 在已创建的项目中安装 React Navigation
在项目的根目录下运行命令：
```
yarn add react-navigation
yarn add react-native-reanimated 
react-native-gesture-handler 
react-native-screens@^1.0.0-alpha.23
```
###### 1.3 iOS和Android中链接对应的库

为了在 iOS 上完成自动链接, 请确保你已经安装了[Cocoapods](https://cocoapods.org/) 然后运行命令
```
cd ios
pod install
cd ..
```
为了完成 ```react-native-screens``` 在 Android 上的安装, 请在```android/app/build.gradle ```中``` dependencies``` 选项中添加下面这两行:
```
implementation 'androidx.appcompat:appcompat:1.1.0-rc01'
implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.1.0-alpha02'
```
#### 二、使用
###### 2.1 屏幕切换
```
this.props.navigation.navigate("组件路由名字")
this.props.navigation.push("组件路由名字")
this.props.navigation.goBack("组件路由名字")
this.props.navigation.popToTop("组件路由名字")
```
navigate: 会判断栈中有没有这个组件, 如果有则回到那个页面,如果没有则创建一个新的组件进行压栈展示;
push : 创建一个新的组件,进行压栈展示;
goBack : 返回上一个页面;
popToTop : 回到首页组件;

###### 2.2 页面之间传递参数
this.props.navigation.navigate 方法可以传递参数到下一个页面，如下代码所示：

```
<View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>首页</Text>
    <Button
      title="跳转到详情页"
      onPress={() =>
        this.props.navigation.navigate('Details', {
          newsId: 'lk001',
          newsName: 'demo1号文件',
          newsTag: '重要'
        })
      }
    />
</View>
```

页面接收参数，如下代码所示：

```
<View
  style={{
      flex: 1,
      alignItems: 'center',
      justifyContent: 'center',
      backgroundColor: 'cyan'
  }}>
    <Text>详情页面</Text>
    <Text>参数1：{JSON.stringify(navigation.getParam('newsId', 'NO-ID'))}</Text>
    <Text>参数2：{JSON.stringify(navigation.getParam('newsName', 'NO-NAME'))}</Text>
    <Text>参数3：{JSON.stringify(navigation.getParam('newsTag', 'NO-TAG'))}</Text>
    <Text>参数4：{JSON.stringify(navigation.state.params)}</Text>
</View>
```

###### 2.2 navigationOptions 设置导航标题
- 常规设置
```
static navigationOptions = {
      title: '首页',
      headerLeft: () => (
        <Button
          onPress={() => alert('设置')}
          title="设置"
          color="#fff"
        />
      ),
      headerRight: () => (
        <Button
          onPress={() => alert('扫一扫')}
          title="扫一扫"
          color="#fff"
        />
      ),
      headerStyle: {
        backgroundColor: '#f4511e',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
        fontSize: 20
      }
    };
```
- 接收上级传递的参数
```
static navigationOptions = ({navigation}) => {
     return {
        title : navigation.getParam("subTitle","demo学院")
     }
}
```

#### 三、Tab navigation

在手机 App 中最常用的导航可能就是基于 Tab 的导航， 这可以是页面底部或标题下方顶部的标签（甚至不要标题）。

###### 3.1 底部Tab切换基本案例
```
import React from 'react';
import { Text, View } from 'react-native';
import { createAppContainer } from 'react-navigation';
import { createBottomTabNavigator } from 'react-navigation-tabs';

class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center', backgroundColor: 'red' }}>
        <Text>首页</Text>
      </View>
    );
  }
}

class SettingsScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center', backgroundColor: 'green' }}>
        <Text>设置</Text>
      </View>
    );
  }
}

const TabNavigator = createBottomTabNavigator({
  Home: HomeScreen,
  Settings: SettingsScreen,
});

export default createAppContainer(TabNavigator);
```

