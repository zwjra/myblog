---
title: 2Dcollide交互游戏
date: 2020-05-05 16:49:22
tags:
excerpt: p5.js的第二个作品
index_img: https://i.loli.net/2020/05/05/AfYHI4SGpRDCqX2.jpg
---



#### 前言

主题为疫情与游戏，是第二个p5.js作品。这次尝试用了更多语言，导入了2Dcollide库，中间还发生了一些戏剧性的事情，好在最后都得以解决了，作品还算成功。
由于是做一个游戏，我考虑到一个游戏应该有的要素，即成功的喜悦和失败的遗憾，所以添加了很多使游戏失败的因素和成功的奖励，没想到考虑太多却一度使游戏制作进度陷入一个进退两难的窘境，不过最后都得以缓解，没想到做一个这样的游戏过程都如此辛酸。

#### 简介

游戏是勇者翻山越岭，客服重重难关的故事。整个环境是一个病毒之中的通道，暗示人们克服疫情的苦难，通道中有层层的荆棘，触碰到则会被病毒吞噬，游戏进度将会被重置。游戏并不简单，需要耐心和经验，并不是单纯的交互动画，需要不断失败累计经验，正如疫情中的人们一样。

灵感来源为荆棘和冒险元素。

![thorns.jpg](https://i.loli.net/2020/05/05/vawGinqItOQWBN6.jpg)

![mxys.jpg](https://i.loli.net/2020/05/05/sWzaLwqlGNb9JB8.jpg)

#### 详细内容

游戏内容是勇者穿越病毒中的重重阻碍，最终到达另一端的故事。玩家通过操作AWSD控制人物移动来穿过各种障碍物，一旦触碰到它们就会立马返回原点，只有耐心谨慎地游玩才能成功抵达对岸。已追加二周目内容，玩家首次通关后所操纵的人物将会强化，速度和战斗力都会大幅提升，此时人物将不再受到障碍的限制并可以逐渐削弱它们，按下回车复位来再次游玩。

<p class="codepen" data-height="588" data-theme-id="light" data-default-tab="result" data-user="zwjra" data-slug-hash="MWarEmR" style="height: 588px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="MWarEmR">
  <span>See the Pen <a href="https://codepen.io/zwjra/pen/MWarEmR">
  MWarEmR</a> by zwj (<a href="https://codepen.io/zwjra">@zwjra</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

在html文件中有这么一段：
src="https://cdn.jsdelivr.net/gh/bmoren/p5.collide2D@0.6/p5.collide2d.min.js"
用于引入2Dcollide库，这是一个用于实现物体之间碰撞检测的库，做这个游戏非常适合。
主要思路是通过一个flag的true和false的来回切换实现碰撞复位的原理，具体见p5.js官网中的libraries中的2Dcollide库。

```javascript
  if ((key=='a')||(key=='A')){ x-=s; }
  else if ((key=='d')||(key=='D')){ x+=s; }
  else if ((key=='w')||(key=='W')){ y-=s; }
  else if ((key=='s')||(key=='S')){ y+=s; }//控制小人移动，速度为s
  else if (keyCode==13){
    x=26;
    y=250;
    t1=0;
    t5=0;}//重置人物位置，清除叉和勾
 } 
```
这一段是所有键盘操作，用于控制小人移动和回车复位。

```javascript
var a1=false;
a1=collideLineCircle(20,232.84,89.88,232.84,x,y,22);
 if(a1){
  x=26;
  y=250;
 }
```
其中一个使用2Dcollide库的例子。

```javascript
function human(x,y){
push();
 translate(x, y);//移动坐标轴
 noStroke();
 fill(200,t2);
 ellipse(0,0,22,22);
 fill(238,204,156,t2);
 ellipse(0,-7,8,8);//头和背景
 fill(247,229,34,t2);
 beginShape();
 vertex(4.4,2.2);
 vertex(5.5,2.2);
 vertex(6.6,0);
 vertex(8.8,-2.2);
 vertex(7.7,-6.6);
 vertex(5.5,0);
 endShape(CLOSE);//火把
 stroke(0,t2);
 strokeWeight(1);
 line(0,-3,0,4);
 line(0,4,-3,11);
 line(0,4,3,11);
 line(0,0,-5,2);
 line(0,0,5,1);//身体
pop();
}
```
创建了一个以(x, y)作小人的函数，soldier函数和virus函数也是如此。

```javascript
 if(x==480){
   if(z==0){
    z=1;
    t1=100;
    t2=0;
    t3=255;
    s=1;}//首次过关，出现叉，小人变士兵
   else if(z==12){
    t5=100;
    t1=0;}//完全通关，清除叉，出现勾
   else{
    t1=100;}//不完全通关，显示叉
 }
```
至始至终human和soldier人物都在画布内，只不过通过状态z来切换玩家操控human还是soldier。
实质上是通过状态z来标记两者的透明度来实现转换。

#### 操作说明

通关AWSD控制人物移动方向；
无论是触碰什么障碍物都会复位到初始位置；
目标是到达出口，到达时会强化人物；
按下回车复位；
强化人物后可以继续游玩二周目，剧情也发生更改；
二周目通关则显示勾而不是叉了；

#### 后续

在version1发布时自己体验了一下，难度较高，我自己都玩了一个小时，好在反复失败可以累积经验；
调整了一下难度发布了version2，这次给别人玩，评价还行，但是仍然偏难；
修改了一下成为version3，再度下调难度；
最后作品定格在version4，闲着无事又增加了游戏二周目内容；

由于主题是做一个游戏，本想是做一个勇者游戏，给玩家冒险和生存的体验。没想到由于考虑太细，把具体操作例如按几下键盘，都规定太死了，导致游戏难度倍增，最后变成了一个肝帝游戏，有时差最后一口气时失败，回档到开头，气得要砸键盘，不知不觉有了《和班尼特福迪一起攻克难关》内味。

![benit.png](https://i.loli.net/2020/05/05/mi8Y2xd6QSs59A7.png)

经过不断调整之后已经变得很简单了，但是趣味性也减少了，变成了速通游戏，用于展示还不错，但并不是我想要的结果，于是保留了前代版本，给自己留作纪念。
无论哪个版本，游戏性还不错，还算满意，个人觉得也比上一个作品美观，虽然仍旧粗糙。