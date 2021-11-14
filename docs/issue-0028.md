# MDH 前端周刊第 28 期：Hydrogen、Chrome Recorder、Umi Contributor

**这是 「MDH：前端周刊」 第 0028 期，发表于：2021/11/15。本期刊开源（GitHub: sorrycc/weekly），欢迎 issue 区投稿，推荐或自荐项目。**

![](https://img.alicdn.com/imgextra/i1/O1CN01eka7Da1HK3130lDAV_!!6000000000738-2-tps-1920-655.png)

封面图：暗影火炬城。

## ❄️ TL;DR

👉 Shopify Hydrogen<br />
👉 Rust 是未来的 JavaScript 基础设施<br />
👉 Chrome Recorder<br />
👉 JIT 与 AOT<br />
👉 Umi Contributor<br />
👉 一键删 node_modules<br />
👉 Svelte 作者加入 Vercel<br />
👉 手动封装 Fetch<br />
👉 Copy CSS as JavaScript<br />
👉 20 年程序员的 20 条建议<br />

## ⚡ 展开讲讲

### Shopify Hydrogen
[https://shopify.engineering/high-performance-hydrogen-powered-storefronts](https://shopify.engineering/high-performance-hydrogen-powered-storefronts)
<a name="mXCXA"></a>
## ![](https://lh5.googleusercontent.com/i1tvW44WfFjMQPV7k-oPtTxgU7JNPgaATJfDEGn_DYAzPBqe-dp6CG6aIazp-Ps4FXU9OQVRnUPCoqwO9K3wLIu26Par1P2n_9gSjkcokOIrtqi4AznEeHleQtKUE-R6zNKeLVmG#from=url&id=rscsl&margin=%5Bobject%20Object%5D&originHeight=263&originWidth=480&originalType=binary&ratio=1&status=done&style=none)**​**
Hydrogen is a front-end web development framework used for building Shopify custom storefronts. It includes the structure, components, and tooling you need to get started so you can spend your time styling and designing features that make your brand unique.

摘要，

- 技术栈是 React, Vite, Tailwind, GraphQL, React-Query，React 社区的应该都熟
- 提供配套 Shopify API 组件库和 hooks 库，快速搭建 Shopify 店铺
- 基于 streaming server-side rendering 和 Server Components，让页面加载变快
- 计划明年年初推出名为 Oxygen（氧气）的全球边缘计算部署平台，让全球化部署变快

访问 hydrogen.new 可快速创建脚手架体验。<br />​<br />
<a name="NRN53"></a>
### Rust 是未来的 JavaScript 基础设施
[https://leerob.io/blog/rust](https://leerob.io/blog/rust)<br />​

作者 JS 开发者，你是愿意给容易的 Slow JS Tool 贡献，还是给困难的学习曲线陡峭的 Fast native tool 贡献？大部分人选择了后者。快的才是赢家。<br />![](https://lh5.googleusercontent.com/VCvwJrh5xkXEatTG13W93bsbv29Rq3_0jaCLnl3V93Ik-XhOjlQ0XrTNYYjD6g541ACvF_TTv91VTAvxbitl8j2016_6m2QnWxbtBWzCmsZfP8jATwCCmtE5YHS3cvnhyCstxi1U#from=url&id=ipMjw&margin=%5Bobject%20Object%5D&originHeight=174&originWidth=1166&originalType=binary&ratio=1&status=done&style=none)<br />Rust 可以编译成 WASM，但目前 WASM 还不完美，比如 Parcel 用 WASM 会比 native binary 慢 10-20 倍。作者认为，一旦这方面的问题解决，Rust 社区库会欣欣向荣。<br />​

Rust 在 JavaScript 基础设施方面的典型库是 SWC，目前除了 Next.js，还有 Deno 的 linter、code formatter、doc generator，Rome、dprint、Parcel 等都接纳了 SWC。（Umi 4 和 ICE 2 也是）另外还有基于 swc 的 swcpack，其目的是替换 Webpack。

SWC 目前有个明显的缺点是 Plugins 机制，目前没有明确的解，如果暴露一个 JavaScript 方式的插件系统，基本会抵消其带来的性能提升。<br />​<br />
<a name="yoiKj"></a>
### Chrome Recorder
[https://developer.chrome.com/docs/devtools/recorder/](https://developer.chrome.com/docs/devtools/recorder/)

Chrome 97 将提供 Recorder 功能，要尝鲜的可下载 Chrome Canary 版体验。Recorder 可实现用户操作的录制、重放和性能测量，有点像 Selenium 的改进版。此外还可把过程导出为 Puppeteer 脚本，想到的场景是 BUG 复现，预计各大厂内会有配套的平台产出或接入。<br />​<br />
<a name="VueVs"></a>
### JIT 与 AOT
[https://mp.weixin.qq.com/s/wumVSSktr_0XCuGTMNp4CQ](https://mp.weixin.qq.com/s/wumVSSktr_0XCuGTMNp4CQ)

名词解释。

JIT 全称 Just In Time，即时编译，代码在宿主环境执行并编译；AOT 全称 Ahead Of Time，预编译代码给宿主环境执行。

Angular 同时提供 JIT 和 AOT。比如模板里出错，AOT 在编译时报错，JIT 则在运行时才报错。通常在开发环境使用 JIT，生成环境使用 AOT。因为在 Angular 的场景中，AOT 的产物更小，同时运行更快。

Tailwind CSS 之前使用会生成数 M 的 CSS，让加载和更新 CSS 的性能都很差。然后在 2.1 里新增了 JIT 模式，在运行时按需生成样式，让 dev 阶段变地很快。

如果大家还记得，Facebook 中间还尝试过 prepack，也是 AOT 的尝试，在保证运行结果一致的前提下，改变源码，让性能更快，产物更小。目前已弃坑。<br />​<br />
```javascript
(function () {
function hello() { return 'hello'; }
function world() { return 'world'; }
global.s = hello() + ' ' + world();
})();

↓ ↓ ↓

s = "hello world";
```
​

相关的还有 Solid.js 框架，在 JSX 的基础上内置了逻辑组件来减少 JSX 的灵活性，比如 <For>、<Switch> 等，使 AOT 成为可能。

此外，最近还有一件相关的事。比如我们从 a 文件 import 成员 b，如果 b 不存在，Webpack 会给警告但不报错，然后等运行时才报错。这在之前 CJS 阶段是合理的，因为是动态，导入导出不准确；而在现在的 ESM 阶段，其实可以改成 AOT 模式，在编译阶段就报错，避免把问题带到线上。<br />​<br />
<a name="STuV6"></a>
### Umi Contributor
[https://github.com/umijs/umi-next#contribution](https://github.com/umijs/umi-next#contribution)<br />​

![](https://lh4.googleusercontent.com/M_avMHRcPMMVOsuzjbeOYUDnY2CPqIR98LqGtuQtkPb2naIppno0gLotorH_Ca12DSrgB_8Ja4Dpgs0FMfZbcdBMgR-4B1lDLt8SYcj10cC7RUUBmjjT4soxtaGbclzvf_0XhHWl#from=url&id=vVURr&margin=%5Bobject%20Object%5D&originHeight=1447&originWidth=1600&originalType=binary&ratio=1&status=done&style=none)

发现国内开源项目很少声明 GOVERNANCE，简单学习了一些做地好的库之后，我给 umi 除 Contributor 外，新增了 Maintainer 和 Core Maintainer 两个角色，不同的权利和责任，也可以给贡献者带来荣誉感。

欢迎对 Umi 感谢的同学加入，10+ PR 就可成为 Maintainer 了哦。<br />​<br />

### 一键删 node_modules
[https://twitter.com/mjackson/status/1458964952216080433](https://twitter.com/mjackson/status/1458964952216080433)<br />​

如果硬盘不够用，或者需要迁移项目代码到新电脑，试着在合适的目录下执行以下命令，<br />​<br />
```bash
$ find . -name "node_modules" -type d -prune -print -exec rm -rf "{}" \;
```
​<br />

### Svelte 作者加入 Vercel
[https://vercel.com/blog/vercel-welcomes-rich-harris-creator-of-svelte](https://vercel.com/blog/vercel-welcomes-rich-harris-creator-of-svelte)<br />​

![](https://lh3.googleusercontent.com/38NxCg2eIhE9KS2XqSKMpDeVqWp6p0Vcvn8T3aZ8rXLl0CBm8zptP8N8EN-dDSZH_x5o1Q_OXRh2vQlY1GQSpnQYVpU_Z7QGSHL_50NmPoao_d03Tdk9EYabNacviQzRVG2A50yQ#from=url&id=rNHZ9&margin=%5Bobject%20Object%5D&originHeight=837&originWidth=1600&originalType=binary&ratio=1&status=done&style=none)

Svelte 作者 Rich Harris 加入 Vercel，全职做 Svelte。Svelte 目前 20w 周下载，5w star，2021 年 StackOverflow 最喜爱框架和 2020 年 StateOfJS 最满意框架。加入后，Svelte 管理权不变，依旧独立发展。<br />​

不管 Vercel 背后的考虑如何，他们已先后吸纳了 Webpack、SWC、Svelte 等开源库作者，对于开源社区的健康发展也是有好处的。

### 手动封装 Fetch 
[https://www.chrisarmstrong.dev/posts/retry-timeout-and-cancel-with-fetch](https://www.chrisarmstrong.dev/posts/retry-timeout-and-cancel-with-fetch)<br />​

在 fetch 基础上手动增加 retry、timeout 和 cancel 的功能，知其所以然。<br />​<br />

- retry 简单，在请求失败时重新执行请求，然后加个计数
- timeout 和一个 timeout 的 Promise 进行 Promise.race 即可，Promise.race([fetchPromise, throwOnTimeout])
- cancel 通过 AbortController 实现，代码如下

​<br />
```javascript
const fetchWithCancel = (url, options = {}) => {
  const controller = new AbortController();
  const call = fetch(
    url, 
    { ...options, signal: controller.signal },
  );
  const cancel = () => controller.abort();
  return [call, cancel];
};


// 使用
const [promise, cancel] = fetchWithCancel(
  'https://cataas.com/cat?json=true',
);
const response = await promise;
cancel();
```


### Copy CSS as JavaScript
[https://umaar.com/dev-tips/249-copy-css-as-js/](https://umaar.com/dev-tips/249-copy-css-as-js/)<br />​

![](https://lh3.googleusercontent.com/DsSjxcqIbrI8IiHwMQrieXrvivu0rnfJGMbWYD1E5pnJTJyjEKA3DlxSDq9wTxxudqFjzKV6QY721Qe_uTDa77jj13oygAnGzrPOfgCXvhzbjvVz4piFJ9XJGEwPdrWo6cDke90Y#from=url&id=B8aYh&margin=%5Bobject%20Object%5D&originHeight=500&originWidth=700&originalType=binary&ratio=1&status=done&style=none)<br />​

Chrome 98 开始支持，想尝鲜的请安装 Chrome Canary。其复制的内容兼容主流 CSS-in-JS 库，比如：<br />​<br />
```javascript
{
  letterSpacing: '2px',
  textTransform: 'uppercase',
  textDecoration: 'none',
  textAlign: 'center',
  margin: '1em',
  padding: '25px 40px'
}
```
​<br />
<a name="mbaAa"></a>
### 20 年程序员的 20 条建议
[https://www.simplethread.com/20-things-ive-learned-in-my-20-years-as-a-software-engineer/](https://www.simplethread.com/20-things-ive-learned-in-my-20-years-as-a-software-engineer/)<br />​

通常前几条会更有价值，所以摘录了其中前 5 条，<br />​<br />

1. 我知道的还不够。程序员最大的魅力在于终身学习，不知道 Rust、不知道 BGP 都没啥，享受学习的乐趣。
1. 最难的是做正确的事。很多人都有写过非常惊艳的程序，但，没人用。一旦选错了方向，投入再大也会打水漂。选择正确的方向很难也很有挑战，不仅要懂程序，还得懂点心理学和人类学。
1. 最好的程序员会设计。设计 API、用户交互、协议等，考虑谁会用，会如何被用，对用户来说什么是重要的，时刻理解用户需求。
1. 最好的代码是不写代码或用不必自己维护的代码。是代码就会报错，少重复造轮子。
1. 程序是解决问题的方法之一。目的是为了解问题，而程序是方法之一，如果意识到这一点，你的思路会不同，有时程序反而不是最优解。

### Bundle Scanner
[https://bundlescanner.com/](https://bundlescanner.com/)<br />​

![](https://lh4.googleusercontent.com/I_ArvFS_JDqAbSmHmNDqS8KSKCgoM-iHY1lPcdXswyKAkGsPr_Cg8uBS25YaXDkFA-O-D1mCNaf9vb2J6HumzkwZzn3--3JrcebuaHrGIF0Op9xtCOEPQdeZOY-qnIeD17ImlOBy#from=url&id=PI12j&margin=%5Bobject%20Object%5D&originHeight=781&originWidth=1600&originalType=binary&ratio=1&status=done&style=none)

输入 URL，他会告诉你这个站点用了哪些 npm 依赖。

原理是，虽然网站上用的 JavaScript 都是压缩后的，但有些东西在压缩前后是不变的，比如字面量和对象的 key，通过他们来对比即可实现。上图是 umijs.org 的部分结果，还挺准。<br />​<br />

## 🕒 订阅

本周刊每周一发布，同步更新在语雀 **「mdh/weekly」** 和微信公众号。

微信搜索 **「云谦」** 或者扫描二维码，即可订阅。

<img src="https://img.alicdn.com/imgextra/i1/O1CN01jmrjUx1yw5LcPFMx0_!!6000000006642-0-tps-430-430.jpg" width="215" />

（完）
