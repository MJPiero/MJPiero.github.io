---
title: threeJS 学习笔记 - 材质
date: 2018-08-23 12:15:58
tags: [threejs]
categories: [threejs]
---

一个材质结合 THREE.Geometry 对象，可以构成 THREE.Mesh 对象。材质就相当于物体模型的皮肤，决定物体的外观光泽，看上去是不是像金属或者线框外表等。

## threeJS 提供的材质

- MeshBasicMaterial：__网格基础材质__。基础材质，用于给几何体赋予一种简单的颜色，或者显示几何体的线框。
- MeshDepthMaterial：__网格深度材质__。这个材质使用从摄像机到网格的距离来决定如何给网格上色。
- MeshNormalMaterial：__网格法向材质__。这是一种简单的材质，根据法向向量计算物体表面的颜色。
- MeshFaceMaterial：__网格面材质__。这是一个容器，可以为几何体的各个表面指定不同的材质。
- MeshLambertMaterial：__网格Lambert材质__。这是一种考虑光照影响的材质，用于创建暗淡的、不光亮的物体。
- MeshPhongMaterial：__网格Phong式材质__。这是一种考虑光照影响的材质，用于创建光亮的物体。
- ShaderMaterial：__着色器材质__。这种材质允许使用自定义的着色器程序，直接控制顶点的放置方式以及像素的着色方式。
- LineBasicMaterial：__直线基础材质__。这种材质可以用于 THREE.Line（直线）几何体，用来创建着色的直线。
- LineDashMaterial：__直线基础材质__。 这种材质与 LineBasicMaterial（直线基础材质）一样，但允许创建出一种虚线的效果。

## 3种常用材质
基础属性常用的了解下：

- ID：标识材质
- name： 名称
- opacity：透明度，结合transparent使用，范围为0~1
- transparent：是否透明，如果为true则结合opacity设置透明度。如果为false则物体不透明
- visible：是否可见，false则看不见，默认可以看见
- side：侧面，觉得几何体的哪一面应用这个材质，默认为THREE.FrontSide(前外面)，还有THREE.BackSide(后内面)和THREE.DoubleSide(两面)
- needUpdate：如果为true，则在几何体使用新的材质的时候更新材质缓存

### ① THREE.MeshBasicMaterial 基础网格材质
使用这种材质的网格，通常被渲染成简单的多边形，而且可以选择想要线框。除了一些THREE.Material的属性以外，还有如下属性

- color：设置材质的颜色
- wireframe：如果为true，则将材质渲染成线框，在调试的时候可以起到很好的作用
- wireframeLinewidth：wireframe为true时，设置线框中线的宽度
- wireframeLinecap：决定线框端点如何显示，可选的值 round，bevel(斜角)和miter(尖角)。
- vertexColors：通过这属性，定义顶点的颜色，在canvasRender中不起作用。
- fog：决定单个材质的是否受全局雾化的影响。 

>对于fog属性，在全局中如果设定了雾化属性，那么本应该对所有场景的物体都添加雾化效果。
例如：
```
scene.fog=new THREE.Fog(0xffffff,0.015,100)
```
而如果在当前材质中设置的如
```
var cubeGeo= new THREE.CubeGeometry(30,30,30);
var cubeMat= new THREE.MeshBasicMaterial({color:"0x0c0c0c",fog:false})
var cude= new THREE.Mesh(cubeGeo,cubeMat);
scene.add(cube);
```
则在当前这个cude方块中，并不能体现雾化效果。
```
material.wireframe = true;
```
wireframe是否为true  显示如下
![](/images/WechatIMG42820.png)
![](/images/WechatIMG42821.png)

### ② THREE.MeshLambertMaterial暗淡不发光
该材质对光源有反应，除了之前说过的color，transparent，opacity，fog等属性，还有一些特有的属性，总结起来就只有2个

- ambient：设置材质的环境色，和AmbientLight光源一起使用，这个颜色会与环境光的颜色相乘。即是对光源作出反应。
- emissive：设置材质发射的颜色，不是一种光源，而是一种不受光照影响的颜色。默认为黑色
```
var cubeGeometry = new THREE.BoxGeometry(15, 15, 15);
var meshMaterial = new THREE.MeshLamebertMaterial({color: 0x7777ff});
var cube = new THREE.Mesh(cubeGeometry, meshMaterial);
```
![](/images/WechatIMG42823.png)

### ③ THREE.MeshPhongMaterial金属发亮的物体
该材质对光源有反应，除了之前说过的color，transparent，opacity，fog等属性，还有一些特有的属性，总结起来有4个

- ambient：设置材质的环境色，和AmbientLight光源一起使用，这个颜色会与环境光的颜色相乘。即是对光源作出反应。
- emissive：设置材质发射的颜色，不是一种光源，而是一种不受光照影响的颜色。默认为黑色
- specular：指定该材质的光亮程度及其高光部分的颜色，如果设置成和color属性相同的颜色，则会得到另一个更加类似金属的材质，如果设置成grey灰色，则看起来像塑料
- shininess：指定高光部分的亮度，默认值为30.
```
 var meshMaterial = new THREE.MeshPhongMaterial({
        color: 0x7777ff，
        specular:0x7777ff,
        shininess:30
});
```
![](/images/QQ20180727-183707.gif)
