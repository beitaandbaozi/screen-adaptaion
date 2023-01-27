# 大屏适配

## 项目安装

```
npm i
npm run dev
```



## 大屏适配方案

```js
src/hook/useScalePage.js

import { onMounted, onUnmounted } from "vue";
import _ from "lodash";

export default function useScalePage() {
  let resizeChange = _.throttle(function () {
    triggerScale();
  }, 100);

  onMounted(function () {
    triggerScale();
    window.addEventListener("resize", resizeChange);
  });

  onUnmounted(function () {
    console.log("useScale onUnmounted");
    // 销毁操作  
    window.removeEventListener("resize", resizeChange);
  });

  function triggerScale(option = {}) {
    let targetX = option.targetX || 1920;
    let targetY = option.targetY || 1080;
    let targetRatio = option.targetRatio || 16 / 9;

    // 1.拿到当前屏幕的宽高
    let currentX =
      document.documentElement.clientWidth || document.body.clientWidth;
    let currentY =
      document.documentElement.clientHeight || document.body.clientHeight;

    // 2.计算缩放的比例
    let scaleRatio = currentX / targetX;

    // 当前的屏幕宽高比
    let currentRatio = currentX / currentY;
    if (currentRatio > targetRatio) {
      scaleRatio = currentY / targetY;
      // 3.设置缩放( 类似图片放大 )
      document.body.setAttribute(
        "style",
        `width:${targetX}px;height:${targetY}px;transform: scale(${scaleRatio})  translateX(-50%); left:50%`
      );
    } else {
      // 3.设置缩放( 类似图片放大 )
      document.body.setAttribute(
        "style",
        `width:${targetX}px;height:${targetY}px;transform: scale(${scaleRatio})`
      );
    }
  }
}
```



## 屏幕适配情况

#### 1920*1080

![](F:\前端知识点补充\大屏适配\demo-image\PC.png)

#### 4k屏(3840 * 2160)

![](F:\前端知识点补充\大屏适配\demo-image\4K.png)

#### 7680*3240

![](F:\前端知识点补充\大屏适配\demo-image\4×3.png)

## ❗❗❗注意事项：如果图片查看不到（raw.githubusercontent错误😭😭😭），请点击demo-image查看对应的适配情况，谢谢。