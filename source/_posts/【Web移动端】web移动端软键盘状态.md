---
title: 【Web移动端】web移动端软键盘状态
date: 2017-01-18 18:10:54
tags: [web移动端,javascript]
categories: [javascript]
---
如今各种移动端设备盛行，虽然现在移动端开发已经非常通用和成熟，但是在开发过程中，还是经常遇到各种神奇的问题，在这里对于移动端web开发遇到的一些问题进行一一整理。
对于移动端设备上的软键盘，在某些时候，会成为页面的一部分，并且不同型号的设备的软键盘对于Html布局的实现也有些不同。
比如ios设备对于从下方推出键盘的时候，如果输入控件在页面推出之后，在键盘的高度的上方的话，则键盘是以一个浮层的方式弹出，并且将那个触发的控件推到键盘的上方。如果那个控件在页面底部，如果推出的键盘会覆盖该控件，系统会将整个页面向上推，直到将那个控件推到键盘上方为止。而android的实现的不同，有部分的android的实现和ios一样，有些android的机型的实现却不同，如果发现触发的input控件比键盘的高度底的时候，会自动将整个document的高度增加，增加到这个控件的高度超过键盘的高度为止。
随后，在判断软键盘是否打开关闭的状态时，因为这两种展现方法的不同，我大概搜罗出可能的解决办法（没有测试全部机型）：
# 软键盘打开，整个页面向上滑动
这种在ios系统里面比较常见，这类的基本上可以通过js的blur的方式来获取事件。
```
$(".input-content input").on("blur",function(){
	//键盘关闭事件 
})
```
# 软键盘覆盖元素
这种情况在ios和android中都有出现，这类的设备，可以通过检测窗口变化来识别
```
var wHeight = window.innerHeight;//获取初始可视窗口高度  
window.addEventListener('resize', function(){//监测窗口大小的变化事件  
  var hh = window.innerHeight;//当前可视窗口高度  
  var viewTop = $(window).scrollTop();//可视窗口高度顶部距离网页顶部的距离  
  if(wHeight > hh){
  	//可以作为虚拟键盘弹出事件  
  }else{
  	//可以作为虚拟键盘关闭事件  
  }  
  wHeight = hh;  
});
```
# 通用方法
```
var flag = false;
var wHeight = window.innerHeight;
window.addEventListener('resize', function(){
    var hh = window.innerHeight; 
    var viewTop = $(window).scrollTop();
    if(wHeight > hh){
        flag = false;
    }else{
        if(!flag){
            alert($(".input-content input").val());
            flag = true;
        }else{
            return;
        }
    }  
    wHeight = hh;  
});
$(".input-content input").on("blur",function(){
    if(!flag){
        alert($(".input-content input").val());
        flag = true;
    }else{
        return;
    }
}).on("focus", function(){
    flag = false;
});
```