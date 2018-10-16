# iPhoneX 适配解决方案

去年 iPhoneX 出来之后，我们对兴趣部落 Web 页进行了适配，目前代码中存在 3 种适配方案：

 方式 1： CSS Class

```react
<div className={`container ${isIphoneX ? 'iphoneX' : ''}`}></div>

.contaienr {
    &.iphoneX {
        // ...
    }
}
```

方式 2：Media Query

```less
.container {
    @media only screen and (device-width: 375px) and (device-height: 812px) {
        // ...
    }
}
```

方式 3：Inline Style

```react
import getNavHeight from './getNavHeight.js';

const style= {
    height: getNavHeight() // 在方法中根据不同机型返回不同数值
};

<div style={style}></div>
```

问题：

1. 可修改性较差：新增机型，比如 iPhone XS Max，需要对所有的地方重新适配；
2. 代码耦合：CSS Class 方式需要在 JS 代码中添加判断代码以决定需不需要添加 Class，将样式与脚本耦合；
3. 代码重复：iPhone X 的 Media Query 代码、isIphoneX 的判断等等；
4. 可理解性较差：Media Query 看不出来适配什么机型。

新的适配方案：

```less
// 单个
@iphone-x: ~"only screen and (device-width: 375px) and (device-height: 812px)";
@iphone-xs: ~"only screen and (device-width: 375px) and (device-height: 812px)";
@iphone-xs-max: ~"only screen and (device-width: 414px) and (device-height: 896px)";
@iphone-xr: ~"only screen and (device-width: 414px) and (device-height: 896px)";

// 分组
@iphone-x-all: ~"only screen and (device-width: 375px) and (device-height: 812px), (device-width: 414px) and (device-height: 896px)";
```

将所有类型的 iPhone X 的 Media Query 用 LESS 变量表示，使用时只需引入变量即可。

```less
@import 'src/less/iphonex-media-query.less';

.container {
    // ...
    @media @iphone-x-all {
        // ...
    }
}
```

在实际使用中，我们发现不同类型机型的适配可能是一样的，比如 iPhone X 和 iPhone XS Max，这两款机型虽然大小不一样、Media Query 不一样，但是几乎所有场景下对这两种机型的适配是一致的，对此，可以有两种写法：

```less
// 方法 1
.container {
    @media @iphone-x, @iphone-xs-max {
        // ...
    }
}

// 方法 2
.conainer {
    // @iphone-x-all 是一个分组，表示所有类型的 iPhone X
    @media @iphone-x-all {
        // ...
    }
}
```

方法 1 虽然也能满足条件，但如果以后再出一款 iPhone XE 、iPhone XU... 那么需要修改的地方将会很多，而采用分组的形式则只需要在 Media Query 变量声明处修改即可。

附四种机型的 UA：

|      机型       |     屏幕      | Device Pixel Ratio |                              UA                              | Media Query |
| :-------------: | :-----------: | :----------------- | :----------------------------------------------------------: | ----------- |
|  iPhone XS Max  | 414px × 896px | 3                  | Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A366 QQ/7.8.0.3940 V1_IPH_SQ_7.8.0_0_HDBM_T Pixel/1242 Core/UIWebView Device/Apple(iPhone X) NetType/WIFI |             |
|    iPhone XS    | 375px × 812px | 3                  | Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A366 QQ/7.8.0.3940 V1_IPH_SQ_7.8.0_0_HDBM_T Pixel/1125 Core/UIWebView Device/Apple(iPhone X) NetType/WIFI QBWebViewType/1 |             |
|    iPhone XR    | 414px × 896px | 2                  | Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A366 QQ/7.8.0.3940 V1_IPH_SQ_7.8.0_0_HDBM_T Pixel/828 Core/UIWebView Device/Apple(iPhone X) NetType/WIFI QBWebViewType/1 |             |
| iPhone X-模拟器 | 375px × 812px | 3                  | Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A366 QQ/7.8.0.3940 V1_IPH_SQ_7.8.0_0_HDBM_T Pixel/1125 Core/UIWebView Device/Apple(iPhone X) NetType/WIFI QBWebViewType/1 |             |
|  iPhone X-真机  | 375px × 812px | 3                  | Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A366 QQ/7.8.0.437 V1_IPH_SQ_7.8.0_1_APP_A Pixel/1125 Core/UIWebView Device/Apple(iPhone X) NetType/WIFI |             |