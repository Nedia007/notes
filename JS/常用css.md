
### 元素居中的方法

#### 水平居中

##### 行内元素水平居中【inline、inline-block、inline-table和inline-flex】

1. text-align: center 

2. 对块级元素转成行内块元素

   ```css
   <div class="parent">
     <div class="child">Demo</div>
   </div>
   <style>
   .parent{
       text-align:center;
    }
   .child{
       display:inline-block;
   }
   </style>
   ```

   

##### 块级元素水平居中

1.  将块级左右外边距设置为auto 

   ```
   margin:0 auto
   ```

2. 元素设置为 display: table + margin: auto

   ```css
   .child{
       display:table//相当于block 只是宽度与内容相同
       margin:auto
   }
   ```

3. absolute+transform 

   ```
   // 将需要居中的元素设置为绝对定位，走到父元素的50% 由于原点在左下角，所以要通过transform往回走居中元素的一半
    .child {
       position:absolute;
       left:50%;
       transform:translateX(-50%);
     }
     .parent {
       position:relative;
     }
   ```

4. flex+justify-content：center

   ```css
   .parent{
       display: flex;
       background: pink;
       width: 300px;
       height: 200px;
       margin:0 auto;
       justify-content:center;//让子元素进行居中
       
   }
   
   .child{
       display: flex;
       width: 100px;
       height: 100px;
       background: palegoldenrod;
      justify-content: center;
   
   }
   ```

5. flex +margin 父元素设置为flex 设置子元素居中

```
<style>
  .parent {
    display: flex;
  }
  .child {
    margin:0 auto;
  }
</style>
```

##### 多个块级元素水平居中

1. 父元素flex  + justify-content:center   
1. 子元素display：inline-block（一行）  父元素：text-align：center

##### 浮动元素水平居中

1. 定宽的浮动元素

   - 子元素相对定位向右移动50% 再往回移动自身的50%

   ```
   <div class="parent">
     <span class="child" style="float: left;width: 500px;">我是要居中的浮动元素</span>
   </div>
   .child{
   	position:relative;
   	left:50%;
   	margin-left:-250px
   }
   ```

   

2. 不定宽的非浮动元素

   - 当子元素浮动，父元素也浮动，父元素的高度就不会塌陷，宽度跟着子元素大小  （使用父子容器相对移动）

     ```
         <div class="box">
             <p>我是浮动的</p>
             <p>我也是居中的</p>
         </div>
     
        .box{
        	 float:left;
          position:relative
          //父元素按原来的位置走50%
          left：50%
        }
        p{
        //子元素在走了50%之后往后挪动元素的50%
          float:left
          right：50%
        }
     ```

3. flex布局+justify-content  

```
.parent {
    display:flex;
    justify-content:center;
}
.chlid{
    float: left;
    width: 200px;//有无宽度不影响居中
}
<div class="parent">
  <span class="chlid">我是要居中的浮动元素</span>
</div>
```

##### 绝对定位水平居中

- 通过子元素绝对定位，外加margin：0 auto 实现

  ```
  <div class="parent">
      <div class="child">让绝对定位的元素水平居中对齐。</div>
  </div>
    .parent{
          position:relative;
      }
     .child{
           position: absolute; /*绝对定位*/
           width: 200px;
           height:100px;
           background: yellow;
           margin: 0 auto; /*水平居中*/
           left: 0; /*此处不能省略，且为0*/
           right: 0;/*此处不能省略，且为0*/
      }
  ```

  

### 垂直居中

#### 单行内元素垂直居中

- 行高与高度相同 

- ```css
  <div id="box">
       <span>单行内联元素垂直居中。</span>。
  </div>
  <style>
   #box {
      height: 120px;
      line-height: 120px;
      border: 2px dashed #f69c55;
      }
  </style>
  ```

#### 多行内元素垂直居中

设置了display：table-cell的元素有一下特点：

- 对宽度高度敏感
- 对margin值无反应
- 响应padding属性
- 内容溢出时会自动撑开父元素

使用display：table-cell可以实现一下几种效果：

1. 大小不固定元素的垂直居中
2. 两列自适应布局
3. 左右等高

#### 块级元素垂直居中





#### 流式布局（弹性盒布局）

width：父元素width的百分之多少

height：父元素height的百分之多少

padding：一般**是父元素width的百分之多少**

margin：用百分比写margin 指的是**父元素width的百分之多少**

border：**无法用百分比写border的宽度**



#### rem响应式布局

##### rem和em的区别

- rem是在html标签的字体大小（font-size）进行动态计算

- em是在父元素字号的倍数 

  em 为单位的时候，font-size 属性是计算后继承，box1 计算出来是 40px。 body是20px box1是40px box2 是80px box3是160px。



格式设置是W3C2.1决定的一个概念，分为**块级格式设置上下文（BFC）**

和**内联格式上下文（IFC）**

#### BFC（块级上下文）

- 是什么？是一个容器，用来管理块级元素

##### BFC规则

- box垂直首位垂直放置
- Box垂直的距离由margin决定 [属于**同一个BFC**的两个相邻Box的margin会发生充电]
- 浮动的box不会和BFC区域产生重叠
- BFC是一个独立的容器，里面的子元素不会影响到外面的元素
- BFC的高度，浮动元素也参与计算 **BFC可以包含浮动**

###### 怎么创建BFC？

- float：有值且不等于none
- position：absloute等（不是static和relative）
- display：inline-block,table-cell,flex,table-caption,inline-flex
- overflow:hidden (不是visible的情况)

###### BFC的作用

- 避免margin重叠
  - 由于在同一个BFC区域，两个块的margin会重叠；此时只要将一个块用div包起来，然后激活它成为另一个BFC就可以让margin不重叠。

- 实现自适应的两栏

![image-20221115141718750](https://user-images.githubusercontent.com/64974747/224205168-14155a87-dd6a-4cd6-987d-179cd6424b1e.png)

  - left设置为浮动 right不设置浮动
    - 他们的左边沿就会重叠接触
  - 想要变成不重叠的两栏，就要让right这块成为一个BFC【因为BFC区域不会与浮动元素重叠（浮动元素自己也有BFC）】

![image-20221115142115877](https://user-images.githubusercontent.com/64974747/224205190-29188d0f-f80d-411f-994f-88cad413dc69.png)

- 清除浮动

  1. 当子元素（有高度）浮动，父元素没有设置高度时，父元素的高度就会塌陷【所以我们要清除浮动】
  ![image-20221115152625645](https://user-images.githubusercontent.com/64974747/224205207-abfd964a-5c1f-4599-a0de-c56d4370cff8.png)

     因为BFC计算高度时，会将浮动元素的高度计算进去，所以**将父元素变成一个BFC就可以清除浮动**

     ![image-20221115160209513](https://user-images.githubusercontent.com/64974747/224205223-c318b8e9-0e74-4ab5-9002-80b49c193083.png)

     ```
         .par{
             border: 5px solid rgb(91, 243, 30);
             width: 300px;
         }
         .child{
            background: pink;
            width: 100px;
            height: 100px;
            float: left;
         }
         
         //清除浮动之后
          .par{
             border: 5px solid rgb(91, 243, 30);
             width: 300px;
             overflow：hidden;
         }
         .child{
            background: pink;
            width: 100px;
            height: 100px;
            float: left;
         }
     ```

​	

