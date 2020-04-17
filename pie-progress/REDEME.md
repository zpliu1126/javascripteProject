# pie-progress组件

实现进度条动画

### 1.SVG绘制两个圆

其中第一个圆的`stroke`属性，充当背景色

第二个圆的`stroke`充当进度条颜色

+ `width`属性用于控制svg图片大小
+ `radius`用于控制进度条厚度

### 2.tween.js控制动画

动画的实现主要是通过控制第二个圆的`strokeDashoffset`样式，

`strokeDashoffset`样式用于控制进度条不显示的长度

+ 在动画开始前让进度条完全不显示，也就是让这个样式等于圆的周长
+ 在动画开始后，不断让这个值等于1-进度条

### 3.tween.js使用

+ 首先传递需要改变的值，例如这里是进度条的长度和数字
+ 设置动画完成时的值
+ `onUpdate`事件，指动画在进行过程中会不断的改变预先设置的参数，这时可以将这个数值赋值给对应的标签样式、或数据对象
+ `start()`启动动画
+ 使用封装好的函数`StartAnimate`开始渲染动画



```javascript
     this.$refs.$bar.style.strokeDashoffset = (this.width - this.radius) * 3.14
      this.showprogress = 0
      new TWEEN.Tween(
        {
          'strokeDashoffset': (this.width - this.radius) * 3.14,
          'showProgress': 0
        }
      ).to({
        'strokeDashoffset': (this.width - this.radius) * 3.14 * (100 - this.progress) / 100,
        'showProgress': this.progress
      }, this.duration).onUpdate((tween) => {
        this.$refs.$bar.style.strokeDashoffset = tween.strokeDashoffset
        this.showprogress = tween.showProgress.toFixed(0)
      }).start()
      this.StartAnimate()
    },
  StartAnimate () {
      function animate () {
        if (TWEEN.update()) {
          requestAnimationFrame(animate)
        }
      }
      animate()
    }
```

### API

+ `width`图片大小
+ `radius`进度条厚度
+ `progress`进度
+ `barColor`进度条颜色
+ `backgroundColor`背景色
+ `isRound`circle标签中进度条是否是圆形
+ `duration`动画持续时间

> 父组件可以通过 this.$refs...progressAnimate()重新渲染动画



### 参考

1. [tween.js](https://github.com/tweenjs/tween.js/blob/HEAD/docs/user_guide.md)
2.  https://www.jianshu.com/p/8333615dfe2b 
3. [数字变化](https://codepen.io/jackpan/pen/pdVVzE/)
4. [svg](https://www.runoob.com/svg/svg-stroke.html)