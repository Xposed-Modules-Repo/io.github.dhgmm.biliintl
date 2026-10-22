# 跳转国际版b站

让微信里的 B 站分享视频，直接交给 **国际版哔哩哔哩**（`com.bilibili.app.in`）打开。

## 解决的问题

微信分享的 B 站视频点「打开APP」时只能去国内版（`tv.danmaku.bili`），装了国际版也不会被唤起。原因是发起方把目标包写死成国内版，甚至在启动前就自行判断「未安装」而放弃。

本模块在微信进程内接管整条跳转链路：

- **意图层**：改写 `startActivity` 各路径的目标包与组件
- **AppMessage**：把 `tv.danmaku.bili.wxapi.WXEntryActivity` 映射到国际版微信入口，保留 extras
- **网页层**：hook XWeb / MMWebView 的 `loadUrl` 与 `shouldOverrideUrlLoading`
- **入口收敛**：视频页转 `bilibili://video/{BV}` 深链，消除「打开方式」选择器
- **商店链接**：接管 `market://` / `a.app.qq.com` 的「去装国内版」跳转

## 使用

1. 安装 APK，在 LSPosed 中启用；
2. 作用域勾选微信（`com.tencent.mm`），需要时再加 QQ / TIM / 浏览器；
3. 强制停止微信后重新打开；
4. 点开分享视频即可直接进入国际版。

## 兼容性

- 包名：`io.github.dhgmm.biliintl`
- 支持 Android 7.0+（minSdk 24）
- 作用域：`com.tencent.mm`（微信）

## 说明

ColorOS 的「前往软件商店安装」属于系统服务 `com.oplus.metis` 的场景推荐，不在模块作用域内，需要在系统设置中关闭应用推荐。

## 源码

https://github.com/DHGMM/bili-intl-jump
