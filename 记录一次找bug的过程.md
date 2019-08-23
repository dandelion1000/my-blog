######记录一次找bug的过程

bug背景：h5项目。测试人员拿着她的华为机MEUI浏览器找到我，一个弹窗上的所有点击事件元素的异常。在点击时该出现事件的地方没有任何反应，在不该出现事件发生的地方却出现。

![图片](https://dn-coding-net-production-pp.codehub.cn/1382e4ab-6f34-4ed3-92bb-8eb0dd9d7691.png)

> 如图点击下拉选框没有任何反应，点取消区域竟然出现单选项列表。也就是点击取消根本关闭不了弹窗，而是一直出现下拉选择列表。

bug情形：在微信浏览器里没有这个问题，公司其他测试机型比如苹果手机，小米等均测试没有问题。

开始拿到这个问题，我是有点懵逼的，但是，我是个追求完美的人，是不可以允许有这种奇怪bug现象存在的，怎么着也得找到原因。

于是我开始分析了，但真的不知道怎么下手啊，orz

然后我身边的小哥哥说，可能是**模态层和表单之间的冲突**，balala的，总之我没有提取到任何有逻辑性可参考性的信息。不过我为了验证他的说法,突然想到这个项目的另一个地方也有类似的弹窗层和select标签选项，于是我在这个有bug的浏览器里进入到另一个页面，神奇的事情 发生了，没有类似的bug。。。然后我开始比对两边的差异性，除了多嵌套了一层div没有任何收获。。。。

回到初始那个有bug的页面，开始思考，为什么会在取消的位置出现点击select的事件呢？看起来好像元素的位置下移了一般，于是我往取消的下方一点位置做了尝试性的点击，神奇一幕出现了，弹窗关闭了。然后我又到确定的下方一定的距离点击，确定事件也发生了，再去关闭icon下方，同样事件发生了。==>得出结论，这个页面的所有元素位置仿佛都下移了一定的距离。但是为什么会出现这种现象。。。

然后我无意间做了一件事滑动了这个弹窗，背景元素在滑动，因为背景数据不止一屏。然后我再次点击取消，发现弹窗关闭了！what，什么情况，我刚刚只是向下滑动了一下屏幕而已。然后我又做了一件事，往上滑动，再次点击取消，又不正常了。。我再往下滑，再次点击取消又可以了。。。

好了，初步猜想出来了，可能由于背景元素列表不止一屏，而我写的modal-card相对定位top：20%。而导致实际元素的位置为超出真实位置一定高度。为了证实这点，我换了一个账号此页数据一屏以内的，登录之后进入该链接页，执行操作，发现没有任何问题！

OK，结论出了，就是由于初始背景元素容器不止一屏，现在要解决这个问题，我需要想办法在open弹窗事件时保证背景元素只有一屏，

做法如下

```css
打开弹窗时将背景大元素body的style：
height:100%;
overflow:hidden;
关闭弹窗时去除这个属性
```

测试发现，刚出现的那个问题没有了，不过还有点小瑕疵，我仍然可以滑动背景，于是我在打开弹窗后加了监听滑动事件以禁止滑动。关闭时去除该监听。OK，完美解决了。



```
但是浏览器控制台会报如下错误：Unable to preventDefault inside passive event listener due to target being treated as passive. See https://www.chromestatus.com/features/5093566007214080
```

> 那么如何解决这个问题呢？不让控制台提示，而且 preventDefault() 有效果呢？两个方案：
> 1、注册处理函数时，用如下方式，明确声明为不是被动的
> window.addEventListener('touchmove', func, { passive: false })
>
> 2、应用 CSS 属性 `touch-action: none;` 这样任何触摸事件都不会产生默认行为，但是 touch 事件照样触发。
> touch-action 还有很多选项，详细请参考[touch-action](https://w3c.github.io/pointerevents/#the-touch-action-css-property)
>
> [注]未来可能所有的元素的 touchstart touchmove 事件处理函数都会默认为 passive: true

https://developers.google.com/web/updates/2017/01/scrolling-intervention

```
Making touch scrolling fast by default
Dave Tapuska
By Dave Tapuska
Dave is a contributor to WebFundamentals
We know that scrolling responsiveness is critical to the user's engagement with a website on mobile, yet touch event listeners often cause serious scrolling performance problems. Chrome has been addressing this by allowing touch event listeners to be passive (passing the {passive: true} option to addEventListener()) and shipping the pointer events API. These are great features to drive new content into models that don't block scrolling, but developers sometimes find them hard to understand and adopt.

We believe the web should be fast by default without developers needing to understand arcane details of browser behavior. In Chrome 56 we are defaulting touch listeners to passive by default in cases where that most often matches the developer's intention. We believe that by doing this we can greatly improve the user's experience whilst having minimal negative impact on sites.

In rare cases this change can result in unintended scrolling. This is usually easily addressed by applying a touch-action: none style to the element where scrolling shouldn't occur. Read on for details, how to know if you are impacted, and what you can do about it.

Background: Cancelable Events slow your page down

If you call preventDefault() in the touchstart or first touchmove events then you will prevent scrolling. The problem is that most often listeners will not call preventDefault(), but the browser needs to wait for the event to finish to be sure of that. Developer-defined "passive event listeners" solve this. When you add a touch event with a {passive: true} object as the third parameter in your event handler then you are telling the browser that the touchstart listener will not call preventDefault() and the browser can safely perform the scroll without blocking on the listener. For example:

window.addEventListener("touchstart", func, {passive: true} );
```



