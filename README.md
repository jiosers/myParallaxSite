# myParallaxSite
一个视差滚动的网站Demo

滚动视差 使用了 skrollr.js

作者github https://github.com/Prinzhorn/skrollr

## 在线Demo
http://parallaxdemo.kajie88.com/

##### 还有部分功能没有实现 继续研究
加油！！！💪




# 前言
最近使用滚动视差建设广告页面成为了潮流。那么也让我紧跟这潮流fashion一下吧😄。
简单的放一下我的Demo和Github
Demo地址：http://www.kajie88.com/myparallaxsite
github地址：https://github.com/kajiecy/myParallaxSite
skrollr.js作者的GitHub: https://github.com/Prinzhorn/skrollr
# 正文
废话不多说 直接切入正题吧，本片随笔使用skrollr.js来实现视差滚动。在决定选择使用它之前我也进行大量筛选与学习。最终根据文档的详细程度，语法的难易度等的对比选择使用skrollr.js了。
# 如何使用
首先 我们需要引入.js 文件 并对 skrollr对象进行初始化
```
<script type="text/javascript" src="../dist/skrollr.min.js"></script>
<script type="text/javascript">
    var s = skrollr.init();
</script>
```
一旦我们初始化成功 在html标签上会多出两个类 "skrollr skrollr-desktop"
# 基础语法
#### 基础滚动
skrollr的使用是非常简单的这归功于作者的js实力
如果我们想让页面中的某个元素在滚动中 执行一些特效 我们只需要在div 上添加
data- 属性即可;
例：
```
    <div 
       data-0="background-color:rgb(0,0,255);" 
       data-500="background-color:rgb(255,0,0);"
       >测试文字</div>
``` 
其中：
data-0   滚动0时的样式
data-500 滚动500时的样式 
实例：http://prinzhorn.github.io/skrollr/examples/docu/1.html

有了这两个样式：skrollr就会自动处理从0滚动到500的央视过渡，类似与CSS中的transtions属性；
#### 过渡动画的抉择
更为强大的是：我们可以根据样式名后加特效名来指定过渡特效。
`data-0="background-color[linear]:rgb(0,0,255);"`
过渡特效在源码中存在一下集中类型分别是：
- begin 
- end
- linear
- quadratic
- cubic
- swing
- sqrt
- outCubic
- bounce
```
var easings = {
		begin: function() {
			return 0;
		},
		end: function() {
			return 1;
		},
		linear: function(p) {
            return p;
        },
		quadratic: function(p) {
			return p * p;
		},
		cubic: function(p) {
			return p * p * p;
		},
		swing: function(p) {
			return (-Math.cos(p * Math.PI) / 2) + 0.5;
		},
		sqrt: function(p) {
			return Math.sqrt(p);
		},
		outCubic: function(p) {
			return (Math.pow((p - 1), 3) + 1);
		},
		//see https://www.desmos.com/calculator/tbr20s8vd2 for how I did this
		bounce: function(p) {
			var a;

			if(p <= 0.5083) {
				a = 3;
			} else if(p <= 0.8489) {
				a = 9;
			} else if(p <= 0.96208) {
				a = 27;
			} else if(p <= 0.99981) {
				a = 91;
			} else {
				return 1;
			}

			return 1 - Math.abs(3 * Math.cos(p * a * 1.028) / a);
		}
	};
```
转换的方式我就不解释了直接丢个源码自己看吧😏
当然您要是感觉这些动画太low。您也可以通过初始化实力时自定义过渡动画
```
skrollr.init({
		easing: {
			sin: function(p) {
				return (Math.sin(p * Math.PI * 2 - Math.PI/2) + 1) / 2;
			},
			cos: function(p) {
				return (Math.cos(p * Math.PI * 2 - Math.PI/2) + 1) / 2;
			},
		}
	});
```
#### 动画的禁用
我们可以使用 在样式前加！的方式来禁用动画了
如：
```
<div  data-0="background-image:!url(kitten1.jpg);" data-100="background-image:!url(kitten2.jpg)">
</div>
```
#### 相对滚动模式
滚动特效除了指定滚动固定的px之外，还有相对模式的概念，
相对模式即为相对与某个锚点进行特效的变化
语法为：
```
  --选择的元素
  data-anchor-target="#bacons" 
  --目标元素 顶部 在可视范围 底部 时的样式
  data-bottom-top="transform: translate3d(0px, -80%, 0px);" 
  --目标元素 底部 在可视范围 顶部 时的样式
  data-top-bottom="transform: translate3d(0px, 80%, 0px);"
```
具体还有更多的类型作者也给出了图片说明：
![滚动模式图解](https://upload-images.jianshu.io/upload_images/12326681-5d19a254b552d121.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上就是 skrollr的最基本概念了，如果想了解更多请查阅作者文档。
现在我们就可以实现一个简单的滚动视差的效果了。
# 滚动视差的实现
#### 实现思路
在正文区域我们先用div把想要显示图片的区域声明好。
overflow:hidden 将超出div的部分隐藏掉。
![示例1.jpg](https://upload-images.jianshu.io/upload_images/12326681-6890851f78412a3d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![示例2.jpg](https://upload-images.jianshu.io/upload_images/12326681-75d7c35c3e6e1cc3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们将图片放入到这个div中 top设置为-50%将图片位置调整到 div的中间。
![示例3.jpg](https://upload-images.jianshu.io/upload_images/12326681-a2b1d20dd352f327.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![示例4.jpg](https://upload-images.jianshu.io/upload_images/12326681-08117e57417e9924.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最后 我们通过上面 提到的相对滚动  监听div#div-view的位置  来对图片进行处理 
![示例5.jpg](https://upload-images.jianshu.io/upload_images/12326681-09d44c4b447cc5b2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
    <div class="" id="div-view" style='height: 50vh;width: 100%;background-color: #C6E746;overflow: hidden'>
        <img class="" src="images/kitteh1.jpg"
             style="position: relative;width: 100vw;height: 100vh;top: -50%" alt=""
        data-anchor-target="#div-view"
        data-bottom-top="transform:translate3d(0px, -60%, 0px)"
        data-top-bottom="transform:translate3d(0px, 60%, 0px)"
        />
```

动画效果为将图片的位置进行便宜就ok了。
(上传不了gif图 演示地址：http://qiniu.kajie88.com/jdfw.gif)

