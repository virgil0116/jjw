# WePY简易

#### 脚本部分介绍
```
小程序入口app.wpy
页面page.wpy
组件com.wpy

import wepy from 'wepy';

// 声明一个App小程序实例
export default class MyAPP extends wepy.app {
}

// 声明一个Page页面实例
export default class IndexPage extends wepy.page {
}

// 声明一个Component组件实例
export default class MyComponent extends wepy.component {
}
---------
App小程序实例

App小程序实例中主要包含小程序生命周期函数、config配置对象、globalData全局数据对象，以及其他自定义方法与属性。

import wepy from 'wepy';

export default class MyAPP extends wepy.app {
    customData = {};

    customFunction ()　{ }

    onLaunch () {}

    onShow () {}

    config = {}  // 对应 app.json 文件

    globalData = {}
}

在Page页面实例中，可以通过this.$parent来访问App实例。
-----------
Page页面实例和Component组件实例
import wepy from 'wepy';

// export default class MyPage extends wepy.page {
export default class MyComponent extends wepy.component {
    customData = {}  // 自定义数据

    customFunction ()　{}  //自定义方法

    onLoad () {}  // 在Page和Component共用的生命周期函数

    onShow () {}  // 只在Page中存在的页面生命周期函数

    config = {};  // 只在Page实例中存在的配置数据，对应于原生的page.json文件

    data = {};  // 页面所需数据均需在这里声明，可用于模板数据绑定

    components = {};  // 声明页面中所引用的组件，或声明组件中所引用的子组件

    mixins = [];  // 声明页面所引用的Mixin实例

    computed = {};  // 声明计算属性（详见后文介绍）

    watch = {};  // 声明数据watcher（详见后文介绍）

    methods = {};  // 声明页面wxml中标签的事件处理函数。注意，此处只用于声明页面wxml中标签的bind、catch事件，自定义方法需以自定义方法的方式声明

    events = {};  // 声明组件之间的事件处理函数
}

注意，对于WePY中的methods属性，因为与Vue中的使用习惯不一致，非常容易造成误解，这里需要特别强调一下：WePY中的methods属性只能声明页面wxml标签的bind、catch事件，不能声明自定义方法，这与Vue中的用法是不一致的。
```

#### 组件

```
普通组件引用

当页面需要引入组件或组件需要引入子组件时，必须在.wpy文件的<script>脚本部分先import组件文件，然后在components对象中给组件声明唯一的组件ID，接着在<template>模板部分中添加以components对象中所声明的组件ID进行命名的自定义标签以插入组件。
<template>
    <!-- 以`<script>`脚本部分中所声明的组件ID为名命名自定义标签，从而在`<template>`模板部分中插入组件 -->
    <child></child>
</template>

<script>
    import wepy from 'wepy';
    //引入组件文件
    import Child from './coms/child';

    export default class Index extends wepy.component {
        //声明组件，分配组件id为child
        components = {
            child: Child
        };
    }
</script>
需要注意的是，WePY中的组件都是静态组件，是以组件ID作为唯一标识的，每一个ID都对应一个组件实例，当页面引入两个相同ID的组件时，这两个组件共用同一个实例与数据，当其中一个组件数据变化时，另外一个也会一起变化。

如果需要避免这个问题，则需要分配多个组件ID和实例。

组件的循环渲染

当需要循环渲染WePY组件时(类似于通过wx:for循环渲染原生的wxml标签)，必须使用WePY定义的辅助标签<repeat>

computed 计算属性

computed计算属性，是一个有返回值的函数，可直接被当作绑定数据来使用。因此类似于data属性，代码中可通过this.计算属性名来引用，模板中也可通过{{ 计算属性名 }}来绑定数据。

需要注意的是，只要是组件中有任何数据发生了改变，那么所有计算属性就都会被重新计算。

watcher 监听器

通过监听器watcher能够监听到任何数值属性的数值更新。监听器在watch对象中声明，类型为函数，函数名与需要被监听的data对象中的数值属性同名，每当被监听的数值属性改变一次，监听器函数就会被自动调用执行一次。

监听器适用于当数值属性改变时需要进行某些额外处理的情形。

props 传值

props传值在WePY中属于父子组件之间传值的一种机制，包括静态传值与动态传值。

在props对象中声明需要传递的值，静态传值与动态传值的声明略有不同，具体可参看下面的示例代码。

静态传值

静态传值为父组件向子组件传递常量数据，因此只能传递String字符串类型。

动态传值

动态传值是指父组件向子组件传递动态数据内容，父子组件数据完全独立互补干扰。但可以通过使用.sync修饰符来达到父组件数据绑定至子组件的效果，也可以通过设置子组件props的twoWay: true来达到子组件数据绑定至父组件的效果。那如果即使用.sync修饰符，同时子组件props中添加的twoWay: true时，就可以实现数据的双向绑定了。

注意：下文示例中的twoWay为true时，表示子组件向父组件单向动态传值，而twoWay为false(默认值，可不写)时，则表示子组件不向父组件传值。这是与Vue不一致的地方，而这里之所以仍然使用twoWay，只是为了尽可能保持与Vue在标识符命名上的一致性。

在父组件template模板部分所插入的子组件标签中，使用:prop属性（等价于Vue中的v-bind:prop属性）来进行动态传值。

组件通信与交互

wepy.component基类提供$broadcast、$emit、$invoke三个方法用于组件之间的通信和交互
用于监听组件之间的通信与交互事件的事件处理函数需要写在组件和页面的events对象中

$broadcast事件是由父组件发起，所有子组件都会收到此广播事件，除非事件被手动取消。事件广播的顺序为广度优先搜索顺序，如上图，如果页面Page_Index发起一个$broadcast事件，那么按先后顺序依次接收到该事件的组件为：ComA、ComB、ComC、ComD、ComE、ComF、ComG、ComH。
$emit与$broadcast正好相反，事件发起组件的所有祖先组件会依次接收到$emit事件。
$invoke是一个页面或组件对另一个组件中的方法的直接调用，通过传入组件路径找到相应的组件，然后再调用其方法。

组件自定义事件处理函数

可以通过使用.user修饰符为自定义组件绑定事件，如：@customEvent.user="myFn"

其中，@表示事件修饰符，customEvent 表示事件名称，.user表示事件后缀。

目前总共有三种事件后缀：

.default: 绑定小程序冒泡型事件，如bindtap，.default后缀可省略不写；

.stop: 绑定小程序捕获型事，如catchtap；

.user: 绑定用户自定义组件事件，通过$emit触发。

slot 组件内容分发插槽

WePY中的slot插槽作为内容分发标签的空间占位标签，便于在父组件中通过对相当于扩展板卡的内容分发标签的“插拔”，更为灵活、方便地对子组件进行内容分发。

具体使用方法是，首先在子组件template模板部分中声明slot标签作为内容插槽，同时必须在其name属性中指定插槽名称，还可设置默认的标签内容；然后在引入了该带有插槽的子组件的父组件template模板部分中声明用于“插拔”的内容分发标签。

注意，这些父组件中的内容分发标签必须具有slot属性，并且其值为子组件中对应的插槽名称，这样父组件内容分发标签中的内容会覆盖掉子组件对应插槽中的默认内容。

另外，要特别注意的是，父组件中一旦声明了对应于子组件插槽的内容分发标签，即便没有内容，子组件插槽中的默认内容也不会显示出来，只有删除了父组件中对应的内容分发标签，才能显示出来。

第三方组件

WePY允许使用基于WePY开发的第三方组件，开发第三方组件规范请参考wepy-com-toast。

混合
混合可以将组之间的可复用部分抽离，从而在组件中使用混合时，可以将混合的数据，事件以及方法注入到组件之中。混合分分为两种：

* 默认式混合
* 兼容式混合

默认式混合
对于组件data数据，components组件，events事件以及其它自定义方法采用默认式混合，即如果组件未声明该数据，组件，事件，自定义方法等，那么将混合对象中的选项将注入组件这中。对于组件已声明的选项将不受影响。

兼容式混合
对于组件methods响应事件，以及小程序页面事件将采用兼容式混合，即先响应组件本身响应事件，然后再响应混合对象中响应事件。

拦截器
可以使用全域拦截器配置API的config、fail、success、complete方法

数据绑定
小程序数据绑定方式

小程序通过Page提供的setData方法去绑定数据，如：

this.setData({title: 'this is title'});
因为小程序架构本身原因，页面渲染层和JS逻辑层分开的，setData操作实际就是JS逻辑层与页面渲染层之间的通信，那么如果在同一次运行周期内多次执行setData操作时，那么通信的次数是一次还是多次呢？这个取决于API本身的设计。

WePY数据绑定方式

WePY使用脏数据检查对setData进行封装，在函数运行周期结束时执行脏数据检查，一来可以不用关心页面多次setData是否会有性能上的问题，二来可以更加简洁去修改数据实现绑定，不用重复去写setData方法。

但需注意，在函数运行周期之外的函数里去修改数据需要手动调用$apply方法。如：
setTimeout(() => {
    this.title = 'this is title';
    this.$apply();
}, 3000);

WePY脏数据检查流程
在执行脏数据检查是，会通过this.$$phase标识当前检查状态，并且会保证在并发的流程当中，只会有一个脏数据检查流程在运行，以下是执行脏数据检查的流程图




```

执行脏数据检查流程图
![流程图](https://cloud.githubusercontent.com/assets/2182004/20554709/a0d8b1e8-b198-11e6-9034-0997b33bdf95.png)































