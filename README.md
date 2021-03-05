# @p2m/react-native-echarts
  
一个webview封装的图表组件。基于百度echarts4，相比native-echarts有echarts自带对象支持，例如渐变色等，用法与官网相同用法。

echarts version 4.2.1

注：react-native 最近几个版本 webview 包改动频繁，安装时请注意需要固定webview的版本，要不然会出现web与react-native通信的问题，导致api无反应，请仔细阅读安装步骤。

## 安装步骤

### 1. 安装依赖
- react-native >= 0.60.2
  ```bash
  yarn add react-native-secharts react-native-webview@androidx
  ```
    或者
  ```bash
  npm install react-native-secharts react-native-webview@androidx --save
  ```
  安装完成后在`android/gradle.properties`文件添加2行配置，确保在项目中启用AndroidX

  注：0.60+采用自动link 安装后不需要进行link
  
  ```
  android.useAndroidX=true
  android.enableJetifier=true
  ```
- react-native >= 0.57 && react-native < 0.60.2
  ```bash
  yarn add react-native-secharts@1.6.1 react-native-webview@2.14.3
  react-native link react-native-webview
  ```
    或者
  ```bash
  npm install react-native-secharts@1.6.1 react-native-webview@2.14.3 --save
  react-native link react-native-webview
  ```

- react-native = 0.56
  ```bash
  yarn add react-native-secharts@1.5.3
  ```
    或者
  ```bash
  npm install react-native-secharts@1.5.3 --save
  ```

- react-native < 0.56
  ```bash
  yarn add react-native-secharts@1.4.5
  ```
    或者
  ```bash
  npm install react-native-secharts@1.4.5 --save
  ```

### 2. 引用组件
```javascript
import {Echarts, echarts} from '@p2m/react-native-echarts';
```
- 大写开头的`Echarts`是组件
- 小写开头的`echarts`是echarts对象

### 3. 使用组件
```javascript
<Echarts option={{}} height={400}/>
```
请看example文件夹中示例代码  

链接：https://github.com/Shanghai-P2M/react-native-echarts/tree/master/example

运行示例
```bash
$ cd example
$ yarn
$ react-native run-ios  或者 $ react-native run-android  
```
option具体配置请参考echarts官网api http://echarts.baidu.com/api.html#echarts

官方示例 http://echarts.baidu.com/examples/

## props

| 属性             | 类型    | 默认值                                                   | 备注 |
| -------------   | ------- | -------------                                           | ------------- |
| option          | obj     | null                                                    | echarts配置项，请参考echarts官网  |
| backgroundColor | string  | 'rgba(0,0,0,0)'                                         | 图表画布背景色 |
| width           | number  | 'auto'                                                  | 画布宽度  |
| height          | number  | 400                                                     | 画布高度  |
| renderLoading   | func    | ()=><View style={{backgroundColor: 'rgba(0,0,0,0)'}}/>  | loading时遮罩  |
| onPress         | func    | (e)=>{}                                                 | 点击事件  |


## 实例方法
| 方法名称         | 参数                            | 备注 |
| -------------   | -------                        | ------------- |
| setOption       | (option: Object, notMerge?: boolean, lazyUpdate?: boolean) |  参数参考：http://echarts.baidu.com/api.html#echartsInstance.setOption |
| getImage        | (base64)=>{}                   |  返回函数的参数base64，可结合RNFS写入相册  |
| clear           | 无                             |  清空echarts画布  |


## 已知bug 和 需要注意的点
### bug
- echarts配置项内所有的函数均无法被`new function()` 或者 `eval()`重新还原为函数, 这个bug只能找到echarts源码内的方法进行修改，后续找到地方会进行修复，请不要在提类似的bug。

### 注意
- 实例方法setOption不会保存修改后的option，这意味着在react 执行setState操作后重新render，当前state的状态会重新覆盖webview内setOption的状态，所以不推荐使用。

- 目前已经修复组件因为onload发生的闪烁，这意味着可以不用组件setOption的实例方法，直接通过修改当容器组件的绑定的state值，setState操作，然后secharts组件会监听 state中option的改变，来进行option修改。当然组件实例方法setOption还是可以使用的，只是有bug，不推荐而已。

- setOption中notMerge值为true,防止echart图合并两个数据
