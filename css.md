## CSS





### 使用translate来改变位置而不是用定位为原因是什么？

transform,opacity不会触发浏览器，重排/重绘，只会触发复合，性能好。

transform 使用GPU图层，更高效，可以缩短平滑动画的时间




