# CSS 动画

CSS 中的 `animation` 属性可以在很多其他属性上如 `color`、`background-color`、`height`、`width` 等属性上添加动画。每一个动画都需要使用 `@keyframes` 定义动画规则，以应用于 `animation` 。

例如：

```css
.element {
  animation: pulse 5s infinite;
}
@keyframes pulse {
  0% {
    background-color: #001F3F;
  }
  100% {
    background-color: #FF4136;
  }
}
```

每一个 `@keyframes` 都需要定义动画在特定时刻的行为。比如，`0%` 就是动画的起始，而 `100%` 则是终止。

## 子属性

动画可以由 `animation` 控制，但也可以用其的 8 个子属性控制。

- `animation-name`：声明应用的动画
- `animation-duration`：动画一个周期的执行时间
- `animation-timing-function`：动画的时间方法
- `animation-delay`：动画延迟执行时间
- `animation-direction`：动画结束时的行为
- `animation-iteration-count`：动画执行次数
- `animation-fill-mode`：动画开始/结束时应用动画的值
- `animation-play-state`：停止/开始动画

例如：

```css
@keyframes stretch {

}

.element {
  animation-name: stretch;
  animation-duration: 1.5s;
  animation-timing-function: ease-in-out;
  animation-delay: 0s;
  animation-direction: alternate;
  animation-iteration-count: infinite;
  animation-fill-mode: none;
  animation-play-state: running;
}
```

## 多步

动画允许定义多个帧，且允许将具有相同属性和值的帧合并。

例如：

```css
@keyframes pulse {
  0%, 100% {
    background-color: yellow;
  }
  50% {
    background-color: red;
  }
}
```

## 多动画

可以使用逗号分隔，以在一个选择器上应用多个动画。例如：

```css
.element {
  animation: pluse 3s ease infinite alternate,
             nudge 5s linear infinite alternate;
}
```


















