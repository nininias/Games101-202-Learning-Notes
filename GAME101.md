GAMES（Graphics And Mixed Environment Sympsium）101     计算机图形学与混合现实研讨会101

------

官网：[计算机图形学与混合现实研讨会 – GAMES: Graphics And Mixed Environment Seminar](http://games-cn.org/)

直播：[GAMES-Webinar视频直播 - 虎牙直播](https://www.huya.com/19077762)

录像：[GAMES101-现代计算机图形学入门-闫令琪_哔哩哔哩](https://www.bilibili.com/video/BV1X7411F744)

课件：[课程PPT和视频 – 计算机图形学与混合现实研讨会](http://games-cn.org/graphics-intro-ppt-video/)

论坛：[GAMES在线课程（现代计算机图形学入门）讨论区](http://games-cn.org/forums/forum/graphics-intro/) （发布作业+讨论）

作业提交系统：[Login at SmartChair](http://www.smartchair.org/GAMES2020Course-YLQ/) （仅2020年2月9日~5月30日间有效）





——出自Lecture 14

**==闫老师的学习方法！==**

WHY——为啥我要学这个东西

WHAT——这个东西是什么

HOW——这个东西怎么用（最容易忘记，“最不重要的”，这方面我自己建议，交给笔记去记录。一些方法的实现方式）



# Lecture 01

 **记录易错点或者未曾记录过的**



# Lecture 02

 **记录易错点或者未曾记录过的**



**符合乘法的一些律**

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002005524334.png" alt="image-20241002005524334" style="zoom: 25%;" />

### **投影！**

对于任意一点的向量投影到三个基向量要用到！

  <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002005827122.png" alt="image-20241002005827122" style="zoom: 50%;" />

![image-20241002012634308](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002012634308.png)



### 叉乘

叉乘 用右手判断方向 ，两个方法：螺旋与三指。

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002010827859.png" alt="image-20241002010827859" style="zoom:25%;" />

**叉乘的长度是两个向量的长度再乘正弦值**

![image-20241002203120216](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002203120216.png)

分配率结合律仍然存在，交换律不存在

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002011327491.png" alt="image-20241002011327491" style="zoom:25%;" />

作用

判定

**左和右（顺时针或逆时针，三角形遍历用得上，**

**内与外（光栅化阶段判定中心采样点是否在图元之内）**——实际是就是判断 是否都在三个向量的左/右边



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002011530091.png" alt="image-20241002011530091" style="zoom:25%;" />

如果axb>0,则逆时针，反之顺时针

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002011701227.png" alt="image-20241002011701227" style="zoom:25%;" />

中心点，确定顺时针还是逆时针之后，如果AP叉乘AB，与CP叉乘CA，BP叉乘BC，都是同一个方向的，说明在里面。 

如果刚好在线上，可自行决定

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002012523468.png" alt="image-20241002012523468" style="zoom:25%;" />



矩阵的运算，直接找某行（左）某列（右）求点积即可！

![image-20241002013407836](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002013407836.png)



矩阵 的乘积没用任何交换律，但是还有结合律，分配率

只要不涉及顺序对调的律，那就是满足的

**不能交换 ！**

![image-20241002013557207](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002013557207.png)



唯一提到矩阵要换顺序的，就是转置和逆

![image-20241002014007293](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002014007293.png)



单位矩阵：对角阵

![image-20241002014125212](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002014125212.png)





向量的点积可以写成矩阵形式

向量的乘积可以写成矩阵形式——**之后在旋转矩阵的推导上特别有用**

![image-20241002014342076](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002014342076.png)



# Lecture 3/4 Transformation

仍然记录易错点或者未曾记录过的

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002113242905.png" alt="image-20241002113242905" style="zoom:33%;" />

### 斜切矩阵的推导

![image-20241002155402935](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002155402935.png)





### 旋转矩阵的推导

![image-20241002155602428](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002155602428.png)

#### 推导方式

符合逻辑：**既然任何一个点都满足此旋转公式，那我挑选最简单的推出来的公式，那也是对的**

- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002160324880.png" alt="image-20241002160324880" style="zoom:50%;" />
- 代入点(0,1)和(1,0)即可推导出来
- 如果要转换成负数，那就用-θ代替θ，即<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002184159609.png" alt="image-20241002184159609" style="zoom:25%;" />
  - 从中观察到的：就是对原来的矩阵进行了一次转置
  - 从中也可衍生出一种推导：
    - 根据定义可以推导出，下面的-θ得到矩阵的实际上是上面θ的矩阵的逆运算，定义：左转θ角再右转θ角，这不就是逆嘛。
    - 然后我们从两个矩阵直接观察，就会发现-θ的矩阵实际上是θ的矩阵的转置矩阵
    - 由此我们又引申出：**旋转矩阵的转置矩阵就等于旋转矩阵的逆矩阵**
    - 数学上，如果一个矩阵的逆等于它的转置矩阵，我们管它叫做**正交矩阵**







## 共同点

上面讲到的几个变换的共同点

- 他们都可以写成这种，一个矩阵乘某个向量得到变换的矩阵

![image-20241002160908707](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002160908707.png)

 



## 齐次坐标

### 引入原因

如何用刚才的共同点去解释**平移**呢？





![image-20241002161034474](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002161034474.png)

**解释不通**



无论怎么变化，后面总会有个+，名为**仿射变换**



![image-20241002161317857](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002161317857.png)



### 有办法可以将平移并入矩阵里面吗？——有

  提高一个维度，并且让多出来的维度的最后一个值为0或1；0表示向量，1表示点

![image-20241002161740706](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002161740706.png)



现在，平移矩阵也能使用刚才的共同点：一个向量乘一个矩阵得到变换后的向量。



#### 为什么要用0/1来区分向量和点呢？

向量具有**平移不变性**

- 也就是说，如果把多出来的维度的最后一个值变成0，再代入上面的矩阵试试看：

- **当（x,y,0）经过平移矩阵变换之后依然是(x,y,0)，**

- 满足**平移不变性**



往深一点讲：

- 如何理解<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002162703843.png" alt="image-20241002162703843" style="zoom:33%;" />
- 也就是![image-20241002162718973](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002162718973.png)
  - w = 0 + w =0,仍然是0，仍然是向量；
  - w = 1 - w = 1,得到的就是向量；
  - w = 1 + w = 0，得到的就是一个点；
  - w = 1 + w = 1,引入——↓    ，因此得到的是两个点的中点。
    - 在2D平面内，引入的齐次坐标，都是默认整体除W（w≠0）了的。因为要满足W是个1，所以再齐次坐标里面会对w做一次除法，也就是我们对裁切空间的一次透视除法



**任何仿射变换都可以写成齐次坐标的形式**

也就是加一个维度，然后新维度w用掩码表示。

特点：

- **在仿射变换里面，**最后一行永远都是001，0001,00001，000001……；当后面提到**投影矩阵变换**的时候，最后一行有它的意义。
- 

![image-20241002163325918](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002163325918.png)



至此，我们的变换矩阵就可以这么写出来

![image-20241002163805255](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002163805255.png)



## 逆变换（待会讲）





## 组合变换

### 变换的合成

**缩放→旋转→平移**

顺序原因解释

- 矩阵的乘法不满足交换律



从右往左写，它会去自动按顺序应用 



说到：矩阵没有交换律，但是他有那么个结合律

那也就是说，直接把左边的矩阵全部搞成一个矩阵，再对点进行一次全部统一变换是可以的

不会说太复杂的矩阵我们数不够，我们可以一直用下去

![image-20241002164727905](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002164727905.png)



### 变换的拆解

他好像说的不是我好奇的怎么把杂合成一堆的矩阵进行拆解成每个矩阵，他说的好像是步骤上，思维上的拆解。



下图提到的

就是如果我想将点不是绕着原点进行旋转而是绕着任意一点进行旋转：

1. 先将**轴点**如何平移到原点（0，0，0）的向量c求出来
2. 将所有点都以c向量进行平移
3. 然后再进行绕着轴点（0,0,0）进行逆时针旋转
4. 旋转完之后，按照-c向量将所有点平移回去

![image-20241002165256287](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002165256287.png)



## 3D的变换

将所有的2D变换都拿到三维空间进行类比即可



在齐次坐标中表示时，如果w!=0，则默认为(x/w,y/w,z/w)——w也要除，会变成1

![image-20241002170023588](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002170023588.png)



在描述仿射变换的情况下，最后一行的4 * 4 的矩阵仍然是0001

提出问题，如果是这样子的4x4的仿射变换的矩阵，对于一个点：

- 是先进行线性变换然后再平移呢？
- 还是先平移再进行线性变换呢？

在2D变换里面，下图与<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002170602815.png" alt="image-20241002170602815" style="zoom:25%;" />是等价的，也就是说在2D变换里面，是先进行线性变换，然后再进行仿射变换。即先进行变换，然后再进行平移。

![image-20241002170540125](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002170540125.png)

![image-20241002170359907](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002170359907.png)



## Viewing Transformation

![image-20241002184938248](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002184938248.png)



pitch

yaw

roll

![image-20241002190213911](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002190213911.png)

 

### 可以把任意的旋转轴n写成矩阵——罗德里格斯

![image-20241002211224334](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002211224334.png)

通过把任意一个旋转分解成XYZ三个轴向分别作旋转得出来的结果

![image-20241002190341525](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002190341525.png)

- n：旋转轴
- α：旋转角度
- I：是 3×3的单位矩阵
- ![image-20241002212900715](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002212900715.png)
- [罗德里格斯矩阵推导](https://zhuanlan.zhihu.com/p/56587491)
  - ![image-20241002212956808](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002212956808.png)



学这个绕着旋转轴旋转的矩阵有什么用捏？



## View/Camera Trasnformation

什么是视图变换？

M-V-P

名词解释——我们下面直接将V矩阵

### 观测矩阵所需要的初始元素定义

e

g

t

![image-20241002214326974](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002214326974.png)



#### 如何进行视图变换？

假设摄像机和所拍摄到的物体的相对位置是完全不变的

也就是说，把物体的世界坐标都敲定好了之后，构建一个摄像机变换到原点的矩阵，运用到其他所有的物体上，实际上就是以摄像机为参照物的视图（View）矩阵了。

​	原理——

- 相对位置不变，视图就不变。
- 为了以摄像机为原点出发点

- 原本物体就是相对世界坐标轴所以有世界坐标，现在物体相对摄像机坐标轴所以有视图（View）坐标

然后将摄像机（带动其他物体的所有**相对位置**）——把摄像机当做坐标轴（Z取反）

- 原本的物体坐标轴就是以物体原点作为坐标轴





- 移动到坐标原点
- g帽一直向着-Z
- t帽一直向着Y
- 自然而然的X也就对上了



#### 视图变换矩阵形式

也就是**先进行平移，然后进行旋转**

- **平移的量也就是世界坐标下摄像机组件的XYZ坐标都取负数，即平移矩阵**
- 将任意一个轴旋转到规范化的轴，不方便，因此反过来求方法：
  - **先求实际要求的旋转矩阵的逆矩阵（轴到任意轴），然后再求逆矩阵**，即可得到原本要求的矩阵
    - 而且规范化的轴，也是正交矩阵。正交矩阵的逆矩阵就是转置矩阵。因此后面直接取转置即可。

![image-20241002233641668](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002233641668.png)

 



### 投影变换P

- 正交投影（Orthographic projection）
- 透视投影(Perspecive projection)



#### 平行投影

最简单的方法：

1. 摄像机位于原点看向-Z
2. 去掉Z坐标
3. 把所有坐标缩放到[-1,1]（第一次拉伸，后面的变换还会重新拉伸）——约定俗成

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241002235259809.png" alt="image-20241002235259809" style="zoom:33%;" />

**正式的方法：**

*上下左右前后字母：tblrfb。*

1. 中心是不变的，中心对应平移到坐标轴的（0,0,0）上
2. 映射到canonical(正则、规范、标准) cube[-1,1]³。也就是三个坐标轴Remap到[-1,1]——OpenGL
   - 注意这个Z轴，是**[f,n]**.因为我们是-Z向前。并不是说翻转了，而是**f**是**-Z**的区域，**n**是**Z**的区域，所以**n**大
   - DX10,11,12，Valkan的是[1,0]的。1近0远。有个`UNITY_REVERSED_Z`就会返回这么一个东西，就是为了适配DX平台的
   - OpenGL获取的屏幕深度好像在Unity里面会映射到[0,1]，近0远1。所以OpenGL会假设左手系，在这个问题上确实会方便一点。左手系坏处是X叉乘Y不等于Z，顺序乱了。坚持右手系，保持这个关系。



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003000019905.png" alt="image-20241003000019905" style="zoom:50%;" />



##### 矩阵的表示方法

- 取所有包围盒的最中心的位置，将向量倒回去
- 把xyz的宽度都变成2（缩放因子）

![image-20241003000800989](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003000800989.png)





#### 透视投影

把透视投影变成平行投影，然后延续平行投影的做法



**回忆数学基础：**

在齐次坐标里面，当w不等于0，要乘的k不等于0，那就满足——**四个值一起乘某个值，那这个值不发生改变。**

*因为我们后面再3D中我们会做一次除W来让W等于1或0嘛*

举例：(1,0,0,1)和(2,0,0,2)表示的是同一个点

![image-20241003002700142](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003002700142.png)



##### 如何实现透视投影

我们要做的就是单纯把远的大的玩意一根根线拉到小的平面上去。





**思路：**

1. 把远平面的空间的所有点挤到和近平面一样大小的长方体里面<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003003746413.png" alt="image-20241003003746413" style="zoom: 67%;" />
2. 然后使用正交投影的思维：中心挪到(0,0,0)，长度拉到[-1,1]³<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003003756933.png" alt="image-20241003003756933" style="zoom:67%;" />



![image-20241003003138171](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003003138171.png)



##### 实现

**实现前准备/规定**

- 近平面上的点在挤压前后保持不变
- 我们只是挤，点往里收缩，远平面的Z值不发生变化
- 远平面的中心点在挤压前后不发生变化

——因此挤压之后得到的解就是唯一的



**求矩阵M**（挤压）

我需要挤压之后(x,y,z)变成

- 高度也是y‘
- z保持不变
- x以此类推



**由图可得相似三角形**

那就可以求得y’——————<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003004110683.png" alt="image-20241003004110683" style="zoom:50%;" />

那就可以求得x’——————<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003004218155.png" alt="image-20241003004218155" style="zoom: 50%;" />



![image-20241003003857746](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003003857746.png)



我们上面求得了压缩之后的点应该长什么样子，那么我们和原本点进行比较，根据相似三角形写出(x’,y’,z’,w)：



![image-20241003004521558](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003004521558.png)



左边有个M这么一乘一定能得到右边的(x’,y’,z’,w)————已知结果倒推M



填空可猜测矩阵得。还剩一行

![image-20241003004644991](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003004644991.png)



**原本规定的：**

1. **近平面上的点在挤压前后保持不变**
2. 我们只是挤，点往里收缩，远平面的Z值不发生变化
3. **远平面的中心点在挤压前后不发生变化**



**近平面**的点在经过了这个矩阵之后一定会等于它自己，然后跟着我们刚才的变化走。给定近平面深度n

![image-20241003005027290](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003005027290.png)

根据之前的，拿矩阵的第三行来用，列出第一个表达式就是An+B=n²

![image-20241003005353399](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003005353399.png)

表达式：

![image-20241003005711177](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003005711177.png)





**远平面中心点**：

![image-20241003005659804](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003005659804.png)



解得：

![image-20241003010042716](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003010042716.png)



​	**还没干完！**我们只是求到了压缩的矩阵，把透视投影矩阵变成了正交投影矩阵，现在还要针对正交投影矩阵进行平移和长度缩放。即：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003010202514.png" alt="image-20241003010202514" style="zoom:67%;" />



![image-20241003014249810](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003014249810.png)

![image-20241007002904497](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007002904497.png)

与入门精要一致

![image-20241003014316502](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003014316502.png)



实际上是按照正交矩阵推导到[-1,1] 来理解的，但是图却不是[-1,1]，而是下面这个鸟样。我打算在学了hodini之后在里面进行模拟试试看。

![image-20241003020408016](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003020408016.png)



遗留问题：视椎体的中心的位置会怎么变化？是往前推还是往后拉？



[GAMES101 透视投影推导总结 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/361175308#:~:text=GAMES101 透)这里对最后的问题进行了求解，高中数学，挺有启发的



至此

Viewing（观测）transformation已经完成（MVP Over）

此时所有物体都变成了[-1,1]³——NDC

那么怎么画到屏幕上呢？

↓

# Lecture 5:Rasterization 1(Triangles)

### 本节课抽理解背景——

- 像素是最小的单位

- 像素方块状

- 像素是纯色
- 像素用的是index索引,(x,y)，即从0开始计数。所以：(width - 1 , high - 1)
- 记录的像素中心实际上是(x + **0.5** , y + **0.5**)
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003210350659.png" alt="image-20241003210350659" style="zoom:50%;" />
- 屏幕空间是(0,0)到(width,height)





先只管XY

[-1,1]³变换到[0,width] x [0,height]

1. 把长度为2拉伸到长度为width/height
2. 把位置平移width/2 ， height/2

——矩阵直接表示完



![image-20241003214543420](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003214543420.png)

将某个多边形变成屏幕空间的多边形

然后在屏幕空间里，把一个模型，图元打碎成像素，显示在屏幕空间上。**从连续变为离散**。



我们使用的显示图像实际上就是**显存里面的某个区域**映射出来

如何在成像设备上画东西



**为什么是三角形？有很多好处：**

- 它是最基础的多边形
- 独特的属性
  - 只有一个平面且这个平面是唯一确定的
  - 三个顶点信息插值可得到内部的其他顶点东西





我们经过了MVP矩阵，变换到了NDC坐标系上，经过viewport矩阵可以得到每个顶点在屏幕空间上的坐标。

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003222040228.png" alt="image-20241003222040228" style="zoom:33%;" />

对于图上像素，被三角形的线部分盖住的像素该如何取舍？即——**判断像素中心点与三角形的位置关系。**



### 采样Sampling

==**采样就是把一个函数给离散化的过程**==

- 解释一下什么叫做**从连续变为离散**。
  - **纹理采样**：当我们将纹理映射到一个 3D 模型表面上时，由于屏幕上的像素数量是有限的，所以我们只能从纹理中提取一部分数据。这就是对纹理的采样，将一个连续的纹理函数离散化成用于渲染的像素值。
  - **抗锯齿中的采样**：为了处理边缘锯齿问题，我们会对一个像素区域进行多次采样（称为超采样，Super Sampling），然后对结果进行平均。这是通过更多的采样来让图像的边缘看起来更平滑。
  - 我来解释：就是我们要的东西实际上的表达方式是以函数的形式来表达的，但我们实际上的屏幕区域（我们要的区域）是有限的，我们实际上只要函数上我们要的区域的值提取出来，拿出来的就是离散的。



回来



我们要采样什么呢？

我们要采样一个三角形——

![image-20241003224829819](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003224829819.png)

![image-20241003224822174](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003224822174.png)



我们实际上要的就是——

![image-20241003225012729](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003225012729.png)

- 用像素的中心点判断
- 如果中心点在三角形内部，则为1
- 反之则为0

我们实际上采样的就是在屏幕空间中定义的这个inside函数，即——

![image-20241003225227198](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003225227198.png)



#### 如何实现在三角形内部的点判断？

##### ==[叉乘](#叉乘)==

叉乘，判断叉乘出来的向量的Z都是正的（不同坐标系就换个正负）



即可判断一个点是否在一个三角形里面。上面已总结，忘了就回顾↑

好像我们默认的都是逆时针来着



特殊情况：如果一个点刚好在边界上——可自行定义

- 要么不做处理
- 要么特殊处理



#### 光栅化的加速

##### 方法①——**三角形轴向包围盒Bounding Box**

专有名词：**Axis-Aligned Bounding Box  —— AABB**

![image-20241003230126791](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003230126791.png)

**实现：**

- 三个点的xy都取最大/最小，取到的四个值就是轴向包围盒了
- 非此包围盒里面的像素不做光栅化



##### 方法二

把包围盒的概念缩小到每一行（可能会更快）



**实现思路：**

- **边界确定**：

  - 首先，确定三角形的三个顶点 `P0`, `P1`, `P2`。

  - 根据三个顶点，找出三角形的**边界框（Bounding Box）**，即包含三角形的最小矩形，这个矩形的范围为 `min_x, max_x, min_y, max_y`。

- **扫描遍历**：

  - 从 `min_y` 到 `max_y`，逐行扫描每一行。

  - 对于每一行 `y`，计算该行上三角形的交点，即从 `min_x` 到 `max_x` 范围内找到哪些 `x` 值位于三角形内（inside函数）。

  - 对这些 `x` 值进行遍历，将每个格点（像素）进行标记或者填充颜色。

- **判断点是否在三角形内**：
  - 使用之前的判断函数 `inside(t, x, y)` 来判断某个点是否在三角形内（inside判断）。

![image-20241003230558927](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003230558927.png)



##### 这些方法的应用场景

估计也就只有当要用一个旋转45°了的差不多像这个↑样子的，就适合使用方法②







##### 其他图形光栅元件——

bayer pattern分布方法——用了更多绿色的点，人眼对绿色最为敏感

更多的感光元件会把绿色的点放的更多一点，给绿色更多感光元件。

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003232011663.png" alt="image-20241003232011663" style="zoom:50%;" />

颜色有个检测系统，堆叠的颜色越叠越黑（彩色打印机）

我们的RGB就是堆叠的越多越亮



我们这节课仍然认为每个像素是颜色内部均匀的小方块

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003231928018.png" alt="image-20241003231928018" style="zoom:50%;" />





按照我们刚才的理论得到的三角形实际上是这个：

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003232154615.png" alt="image-20241003232154615" style="zoom:33%;" />但我们实际上想要的是这个：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241003232216090.png" alt="image-20241003232216090" style="zoom:33%;" />，里面的**锯齿（Jaggies）**特别严重



**锯齿原因：**像素本身有个大小

采样率对信号来说不够高，就会产生一种**信号走样（aliasing）**——Jaggies的问题

所以我们对于这么简单的话题才要使用“**采样Sample**”这么个东西来去分析它，因为我们后来要分析信**号走样（aliasing）**的问题——转化成理论上的问题



# Lecture 06 Rasterization2 (Antialiasing and Z-Buffering)

### Antialiasing

- 采样理论
- 反走样的实践



一切在图形学看上去不太对的东西叫**Artifacts**（瑕疵）

常见的Artifacts——锯齿，摩尔纹，车轮倒转

眼睛采样的问题

**根本原因：信号变化太快（频率），以至于采样速度（人眼或者其他）跟不上其变换的速度**



### 如何进行反走样（先说结果）

采样之前做一次模糊（滤波），反之不行

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006142958811.png" alt="image-20241006142958811" style="zoom:50%;" />





### Frequency Domain频域

为什么说采样速度跟不上信号频率会产生走样？

周期， 是频率的倒数

频率



#### 概念延伸——傅里叶基数展开

 **任何周期函数都可以写成一系列的正弦和余弦的线性组合以及一个常数项**

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006143829522.png" alt="image-20241006143829522" style="zoom:50%;" />



引出新概念↓

### 傅里叶变换

某个函数，经过一系列复杂操作可以变成另外一个函数，逆变换还能变回去

![image-20241006143953996](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006143953996.png)





然后这么一说我直接明白了为什么说采样频率跟不上信号频率会产生信号走样了！（对频率的分析，会不会有点类似于宏观上的相机快门速度）

![image-20241006144237226](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006144237226.png)

采样频率太低，已经无法将原始信号恢复出来了

刚刚说了用函数的频率近似模拟任意一条函数

如果频率高了就能几乎完全模拟，逆向也可看到原始函数长什么样

如果频率低了，就不能模拟，反之也无法推回





使用相同的采样频率去采样两种完全不同频率的函数得到的采样结果却是完全重合的，这就是因为采样频率过低而导致的“aliases”

![image-20241006144626312](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006144626312.png)





结合aliases和傅里叶变换去分析函数





### Filtering（滤波）

什么是滤波？

从频域的角度，把某一些特定的频域（如果在某一个频段之内）给抹掉，然后对应的信号所发生的变化，即为滤波

​	滤波是一种信号处理技术，用于去除信号中的噪声或不需要的成分，同时保留或增强有用的信息。滤波器可以是硬件（如电子电路）或软件（如算法）。滤波可以分为低通滤波器（只保留低频成分，去除高频噪声）、高通滤波器（只保留高频成分）、带通滤波器（保留特定频率范围）等，目的是使信号更干净或提取特定频率的信息。



为什么要提到傅里叶变换？

傅里叶变换把时域（空间）变换到频域（频率），供我们进行对频域的研究，也能用上Filtering

傅里叶变换能够让我们看到图片（空间）上不同的地方的频率长什么样



#### 高通滤波

高通滤低

![image-20241006165947528](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006165947528.png)



#### 低通滤波

低通滤高

低通是细节，高通是边界

![image-20241006165842002](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006165842002.png)





#### Filtering  =  Convolution（卷积） = Averaging（平均）

卷积作用在一个信号上，用某一种滤波器对信号进行卷积，得到的结果（平均）——卷积（简化）



回顾卷积（Convolution）

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006151122341.png" alt="image-20241006151122341" style="zoom:50%;" />

带子进行点乘



**定理：**

**时域上对两个信号进行卷积相当于在频域上对两个信号进行乘积**



==**在时域上卷积，相当于频域上的乘积**（卷积）==

==**在频域上卷积，相当于时域上的乘积**（采样）==





如何进行卷积？两种操作：

1. 拿到一个图，直接用卷积的滤波器对图进行卷积；
2. 拿到一个图，通过傅里叶变换到频域，把卷积的滤波器变到频域上，两者相乘，再逆傅里叶变换变回时域。

![image-20241006151846833](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006151846833.png)



#### 卷积核——滤波器——Box Filter

低通滤波器：

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006152534451.png" alt="image-20241006152534451" style="zoom:50%;" />



![image-20241006152734373](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006152734373.png)



试想一下如果box变大/变小，频域如何变化？

假如用一个超级大的卷积核去卷积一张图，那低频不就越来越多，高频就越来越少

低频的区域越多，时域就越接近中心，对比越弱，区域越小——也表示为高频越少



![image-20241006153040889](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006153040889.png)



#### **从频域的角度来看什么是采样？**

==**采样实际上就是在重复频域上的内容**==



冲击函数（只在固定位置有值）去冲击连续函数得到离散值，即为采样（时域上表示为乘积）

冲激函数的频域还是一堆冲击函数

冲激函数时域和频域互为相反数



==**时域上的乘积表现为频域上的点积**==

![image-20241006153709748](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006153709748.png)



这里有点想象不到频域是什么样子的，直接想结论吧



**采样在做什么？**

==**采样就是在重复一个原始信号的频谱。**==



**回归到，为什么会走样呢？**

**时域和频域有好多相反的关系（这不周期和频率是倒数嘛），**即采样点越稀疏（冲击函数很大，采样频率小），采样频域的间隔就越密集，搬移的原始信号就越密集，太过密集会带来信号混叠



自分析——

我们根据**频域是否混叠来表示是否走样**，如图。在稀疏采样（相当于频域冲击函数很密集）的时候，频域就发生了两个频域的混叠

密集采样——冲激函数频域稀疏——卷积核中间密集四周稀疏（越来越保持原样）——越来越不走样，混叠很少——和原始信号卷积之后，重复信号很少

<u>下面说到的采样频率指的是时域里面采样的冲击函数的密集程度</u>

**控制冲击函数的频域疏密（即采样频率高低，反比）：**

- 复制粘贴的原始信号越多（卷积冲击函数产生），间隔越小，混叠可能性增加，走样程度增加，就会发生走样

- 复制粘贴的原始信号越少，间隔越大，混叠可能性越小，走样程度降低，达到**Antialiasing**





![image-20241006160535493](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006160535493.png)



根据原理，

#### 如何进行抗锯齿

- 增加采样率——分辨率
- 回到最初提到的——先做模糊，再做采样
  - 模糊——通过低通滤波把高频信息先拿掉
  - 然后再采样



**为什么方法二可行？**

​	在保持采样频率不变的情况下，即频域间隔保持不变。我们的低通滤波相当于是把原本的形状<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006162423717.png" alt="image-20241006162423717" style="zoom:50%;" />进行削砍，将周围两边的小脚（比较可能发生混叠的地方）砍掉，对应到图片上<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006162541536.png" alt="image-20241006162541536" style="zoom:67%;" />就是让中间的方块（或者其他某个形状）变得更加清晰锐利，把其他形状砍掉

![image-20241006162349231](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006162349231.png)



**回归实际做法上面，联系频域**

直接采样——



![image-20241006170153793](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006170153793.png)



**模糊再采样——**



![image-20241006170211656](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006170211656.png)





选用哪个滤波器进行模糊操作？

拿一个低通滤波器对<u>一块</u>——**<u>它</u>**进行卷积<u>平均</u>即可。

<u>平均</u>——指的是“平均有多少区域在三角形内部”，平均值指的是平均像素占比面积的值。

<u>“它”</u>是谁？像素吗？——对

<u>一块</u>——最简单的就是像素。一个像素就是一块，对像素自己进行卷积。将点求平均，然后进行采样。

想一下：

原始：点——在不在三角形里——采样

改成：点——多少部分在三角形里（块里平均多少面积在三角形里的）——采样多少部分

![image-20241006170719931](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006170719931.png)



#### Antialiasing By Supersampling（MSAA）

==**Multisample Anti-Aliasing——多重采样抗锯齿**。==

  你说直接算面积平均占比是多少，这很大的计算量。

因此想到了一种方式

用一个像素块里面进行更多的采样点，平均有多少个采样点在三角形里面，平均的采样点来近似当做像素三角形的平均覆盖面积。

用平均出来的覆盖面积去取平均的颜色



![image-20241006172209980](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006172209980.png)

![image-20241006172257728](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006172257728.png)



我们实际上只是用这个步骤去近似“模糊”这个过程——求平均占比，（你也可以认为它包含了采样）

然后采样。有了平均占比，把“是否在三角形里，是则采样，否则不采样“改成”是否在——是，有多少在，否，0“





<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006172427354.png" alt="image-20241006172427354" style="zoom: 67%;" />



增加那么多采样点只是为了去近似地模拟提高**覆盖率**（实际上的模拟覆盖率就是物理层面增加分辨率），在此并没有提高采样率，只是为了提高三角形的覆盖



效果对比：“

![image-20241006172825987](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006172825987.png)

![image-20241006172809976](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241006172809976.png)





但是

为了做到MSAA

我们牺牲了什么呢？

我们用了更多的点进行测试，增大了很多计算量

O（n²）

工业界实际上有某种像素复用的效果

所以打游戏的时候开启了MSAA4倍但是帧率也没掉1/4

会用某种更加有效的图案来分布这些点，用不规则的分布来描述增加的采样点，有些点还会被临近的像素所复用，这些点也只是拿来检测是否被三角形所覆盖而已。



**FXAA（Fast Approximate AA）**

图像后处理操作的。这个所说的抗锯齿和我们刚才三角形遍历阶段所说的抗锯齿不是同一个东西。

一开始不是说，在三角形遍历阶段进行更多点的三角形内部判断然后采样嘛，不是说不能先采样再模糊嘛，这个先得到有锯齿的图，然后根据某种边界检测（图像匹配，canny边缘算子？）的方法找到边界的锯齿部分，然后把这些边界换成没有锯齿的边界，很快

和采样无关，是在图像层面上所做的抗锯齿



**TAA(Temporal AA)**

非常简单高效，在三角形遍历阶段

与时间相关

先找上一帧的信息，复用上一帧的点所在的位置，记录当做这一帧的额外点，这一帧实际的点分布在与上一帧不一样的地方，来分别感知是否在三角形内。使用上一帧感知到的结果，相当于MSAA的点被分布到了时间上，来使得采样的点的数量增多。

会出现一种情况，面对静态场景得到的边缘的样子会各不相同，有时候有边界有时候没有边界。

运动的物体怎么办？后面实时光线追踪会讲。



**Super resolution/super sampling**

超分辨率/超采样

提到DLSS

就是你有一张高分辨率的图但是采样率不够（512拉到1K），把这个高分辨率的图恢复，想办法补足这个采样率

**DLSS——**

通过深度学习的方法把超分辨率做出来

缺失的细节用猜出来——深度学习。出现在任何局部该如何用细节补上去





### Visibility/occlusion

- Z-buffering







之前油画都是从后往前画，顺序是绝对正确的——画家算法Paiter’s Algorithm

遮挡关系很对



如果是这样的，我们就不能用办法去定义它的深度关系了

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007163133931.png" alt="image-20241007163133931" style="zoom:33%;" />





为了解决这个问题，图形学里面就引入了一个**算法**——深度缓冲Z-buffering



**复杂度是多少？**

排序的算法复杂度就是O(n log n)



知识点补充——排序算法

**排序算法的复杂度**取决于所使用的具体算法。不同的排序算法具有不同的时间复杂度和空间复杂度。下面列出了一些常见的排序算法及其复杂度分析。

### 算法复杂度

#### **1. 冒泡排序（Bubble Sort）**

- **最坏时间复杂度**：`O(n^2)`
- **平均时间复杂度**：`O(n^2)`
- **最好时间复杂度**：`O(n)` （如果数组已经是部分有序的）
- **空间复杂度**：`O(1)`
- **描述**：冒泡排序逐步比较相邻的元素，并将较大的元素向后移动，每次循环确定一个元素的位置。

#### **2. 选择排序（Selection Sort）**

- **最坏时间复杂度**：`O(n^2)`
- **平均时间复杂度**：`O(n^2)`
- **最好时间复杂度**：`O(n^2)`
- **空间复杂度**：`O(1)`
- **描述**：选择排序每次从未排序的部分中选择最小的元素并放到已排序部分的末尾。

#### **3. 插入排序（Insertion Sort）**

- **最坏时间复杂度**：`O(n^2)`
- **平均时间复杂度**：`O(n^2)`
- **最好时间复杂度**：`O(n)` （如果数组是接近有序的）
- **空间复杂度**：`O(1)`
- **描述**：插入排序将元素逐个插入到已有序的部分，保持部分序列有序。

#### **4. 快速排序（Quick Sort）**

- **最坏时间复杂度**：`O(n^2)` （当数组已经有序且选择第一个元素作为基准时）
- **平均时间复杂度**：`O(n log n)`
- **最好时间复杂度**：`O(n log n)`
- **空间复杂度**：`O(log n)` （递归调用的栈空间）
- **描述**：快速排序通过选择一个**基准元素**，将数组分为小于和大于基准的两部分，然后对两部分递归排序。

#### **5. 归并排序（Merge Sort）**

- **最坏时间复杂度**：`O(n log n)`
- **平均时间复杂度**：`O(n log n)`
- **最好时间复杂度**：`O(n log n)`
- **空间复杂度**：`O(n)` （需要额外的数组来合并两个子数组）
- **描述**：归并排序将数组分为两部分分别排序，然后合并已排序的部分。

#### **6. 堆排序（Heap Sort）**

- **最坏时间复杂度**：`O(n log n)`
- **平均时间复杂度**：`O(n log n)`
- **最好时间复杂度**：`O(n log n)`
- **空间复杂度**：`O(1)`
- **描述**：堆排序基于堆数据结构（最大堆或最小堆），通过不断从堆中取出最大或最小元素构建有序序列。

#### **7. 希尔排序（Shell Sort）**

- **最坏时间复杂度**：`O(n^2)`（取决于增量序列）
- **平均时间复杂度**：`O(n log n)` 到 `O(n^2)` （具体取决于增量序列）
- **最好时间复杂度**：`O(n log n)`
- **空间复杂度**：`O(1)`
- **描述**：希尔排序是对插入排序的一种改进，使用分组和增量来减少数据的移动。

#### **8. 计数排序（Counting Sort）**

- **最坏时间复杂度**：`O(n + k)`，其中 `k` 是数组中元素的范围
- **平均时间复杂度**：`O(n + k)`
- **最好时间复杂度**：`O(n + k)`
- **空间复杂度**：`O(n + k)` （需要辅助数组存储计数）
- **描述**：计数排序适用于数据范围较小的整数数组，通过统计每个元素出现的次数来排序。

#### **9. 基数排序（Radix Sort）**

- **最坏时间复杂度**：`O(n * k)`，其中 `k` 是数字的最大位数
- **平均时间复杂度**：`O(n * k)`
- **最好时间复杂度**：`O(n * k)`
- **空间复杂度**：`O(n + k)`
- **描述**：基数排序通过按位进行排序，通常从最低位到最高位依次对每一位进行稳定的排序。

#### **10. 桶排序（Bucket Sort）**

- **最坏时间复杂度**：`O(n^2)` （当元素分布极为不均时）
- **平均时间复杂度**：`O(n + k)`，其中 `k` 是桶的数量
- **最好时间复杂度**：`O(n + k)`
- **空间复杂度**：`O(n + k)`
- **描述**：桶排序将数据分布到多个桶中，每个桶内使用其他排序算法，然后合并桶中的数据。

#### **总结：不同排序算法的时间复杂度**

| 排序算法 | 最坏时间复杂度 | 平均时间复杂度 | 最好时间复杂度 | 空间复杂度 |
| -------- | -------------- | -------------- | -------------- | ---------- |
| 冒泡排序 | `O(n^2)`       | `O(n^2)`       | `O(n)`         | `O(1)`     |
| 选择排序 | `O(n^2)`       | `O(n^2)`       | `O(n^2)`       | `O(1)`     |
| 插入排序 | `O(n^2)`       | `O(n^2)`       | `O(n)`         | `O(1)`     |
| 快速排序 | `O(n^2)`       | `O(n log n)`   | `O(n log n)`   | `O(log n)` |
| 归并排序 | `O(n log n)`   | `O(n log n)`   | `O(n log n)`   | `O(n)`     |
| 堆排序   | `O(n log n)`   | `O(n log n)`   | `O(n log n)`   | `O(1)`     |
| 希尔排序 | `O(n^2)`       | 取决于增量序列 | `O(n log n)`   | `O(1)`     |
| 计数排序 | `O(n + k)`     | `O(n + k)`     | `O(n + k)`     | `O(n + k)` |
| 基数排序 | `O(n * k)`     | `O(n * k)`     | `O(n * k)`     | `O(n + k)` |
| 桶排序   | `O(n^2)`       | `O(n + k)`     | `O(n + k)`     | `O(n + k)` |





#### 深度缓冲

这个算法在干什么呢？

把物体与物体的遮挡关系

改成

像素与像素之间的遮挡关系



对于每个像素而言，是比对每个物体而言要简单的多的。

用像素（在三角形里面的）记录离摄像机最近的距离



我们会对最终图像进行两个缓冲区的同时生成：

- **frame buffer**存储颜色值（能叫他颜色缓冲区嘛？）
- **depth buffer（z-buffer）**存储深度值

我们区分了摄像机朝向-z，使得Z越大反而越前，Z越小反而越后。我们把“深度”作为Z值，即——

==**对于Z-Buffer，越远越深值越大，越近越浅值越小。**==



**Z-Buffer（Depth）长啥样？**

![image-20241007164023949](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007164023949.png)

**那算法怎么写？**

遍历——三角形里有像素——对比深度缓冲区（初始无限大）——新来的比较近——更新frame buffer和Z-buffer——新来的像素——对比深度缓冲区——新来的比较近——更新frame buffer和Z-buffer

![image-20241007164340794](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007164340794.png)



**Z-Buffer工作原理**

假设一开始的Z-Buffer存储的都是无限大的值，C++用`std::numeric_limits`表示，也可以自定义。

![image-20241007164810274](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007164810274.png)





**算法时间复杂度是多少呢？**

**O(n)** ，一个三角形对比常数次，n个三角形对比n个常数次，所以就是 1 * n 次



**像素遍历的复杂度**：

- Z缓冲的过程主要是**逐个像素遍历**，对每个像素进行深度比较和可能的更新。
- 假设渲染的图像分辨率为 `width * height`，则一共有 `n = width * height` 个像素。

**每个像素的比较和更新**：

- 对于每个像素，进行**一次深度比较**，并可能**更新Z缓冲区的值**。
- 这两步的操作都是**`O(1)`**，即常数时间操作。——对单个像素而言，像素自己的时间复杂度，就单纯1对1比较取最小，很简单的常数时间。

**总时间复杂度**：

- 需要对所有像素进行深度比较，总共 `n` 个像素，每个像素的操作为 `O(1)`。
- 因此，**总时间复杂度为 `O(n)`**。——对整屏幕个像素进行深度比较，每个深度比较的时间复杂度都是常数，所以是O(n) * O(1) = O(n)。



<u>提问：理论上来说，对于三角形的从后往前的顺序排序，理论上不应该是**O(n log n )**的时间复杂度吗？</u>

我们的确从这里得到了遮挡顺序，为什么只花了线性的时间O(n)呢？

实际上我们在这里并没有“排序”，只是记录最小值。也就是说我们只是再对每一个像素求最小值，



**使用深度缓存算法的优点**

- 和绘制顺序没有关系（假设没有相同深度的两个三角形），只要能正确在z-buffer里面书写自己的深度，那就可以自行比较与写入frame buffer
  - 图形学里面不会直接用有理数去作比较，都是用浮点型的。浮点型那就肯定会有一定的误差，几乎不太可能会有两个完全一样的浮点型（有再说）。大概就是1.00000000001和1.000000000025，那么大的数字总会有那么一点点的误差。所以我们在对比的时候好像才会用“近似相等”来做对比。误差不大就认为是相等的。



**应用场景**

目前几乎所有硬件在光栅化阶段都会做个深度测试，然后得到正确的遮挡算法





<u>之前不是提到说，我们做MSAA的反走样的时候，我们会将一个采样点变成多个，一个像素里面有多个采样点。</u>

<u>那还得考虑这个z-buffer可能不是对像素做那么一个记录，可能是对每一个采样点做这么一个记录。</u>



**==Z-Buffer一定处理不了透明物体，需要自行特殊处理==**



### 作业

本次作业提到**[透视矫正插值](https://happyfire.blog.csdn.net/article/details/100148540)**，**[重心坐标](https://blog.csdn.net/n5/article/details/122871039)**

投影矫正：

![image-20241008002938248](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241008002938248.png)

在视口空间（NDC）——[-1,1]³的时候，也就是经过MVP变换然后透视除法之后所进行的深度值插值的方式。



**重心坐标公式计算：**

![image-20241009201612356](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009201612356.png)



针对视口（对于视口的深度插值）

针对顶点的深度值进行插值到三角形内的每一个像素点上，我们不能直接差值。需要进行透视矫正，而且还涉及到重心坐标来对像素的Z进行插值（不是很理解，先搞完先）

因此，**在得到三角形里面的像素点要根据像素点来更新深度缓冲区的时候，不能直接使用插值出来的z。**



抗锯齿实现了，但是还没对黑边进行删除。倒是知道原因，就是在重叠的部分，也被认为是三角形的区域，然后对该区域的像素进行蓝色取一点绿色取一点，导致有那么条边是混搭了两边三角形的颜色的。



搞出来了

![image-20241009201500447](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009201500447.png)

# Lecture 07/08 Shading1（Illumination，Shading and Graphics Pipeline，Shading,Pipeline and Texture Mapping）



我们学了什么？



模型（M）

视图变换（V）

投影变换（P）

透视除法之后的[-1,1]³

屏幕空间，三角形会覆盖哪些屏幕坐标，然后变成采样的结果（光栅化）

我们屏幕上可以有像素了，颜色嘞——frame buffer里面来的



## Shading



画画里面叫有明暗区分（素描）有颜色区分（水彩，厚涂，油画巴拉巴拉）

![image-20241007173822708](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007173822708.png)



**在图形学里面，我们把对不同的物体应用不同的材质，叫做着色。**



### 最基础的着色模型——Blinn-Phong Reflectance Model

基本算复习，不记录过多，记易忘记的。

![image-20241007174545038](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007174545038.png)





这里说的Shading都是局部的，不考虑其他物体的存在，只考虑自己的

![image-20241007174907058](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007174907058.png)





#### **漫反射（Diffuse）**

**考虑光照能量守恒的话**

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007175638897.png" alt="image-20241007175638897" style="zoom:33%;" />

从光源中心发射出来的单位光照强度**I**，离远处的点的半径为r

如果是根据强度和球壳的体面积来看，能量守恒→单位半径能量是I * π *1² = Iπ，远处的I/r² * πr² = Iπ。

衰减规律是单位面积中成反比的。



由此可得——

**假如我知道shading point离点光源有多远，那就可以计算出shading point的光照强度**（有多少光的能量传播到了shading point）

我们又知道又多少光能够在shading point 会被吸收**K~d~**（有些会因为角度而缺失，或者因为微面元遮挡），由此可得diffuse的表示方法



由此可得——

**Lamvertian（Diffuse）Shading**



**能够到达shading point的光强点乘物体表面能够吸收的光照强度**计算即可得出在某一点上的光照强度



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007180537232.png" alt="image-20241007180537232" style="zoom:50%;" />



 **一个物体为什么会有颜色？**

部分颜色会被吸收，不被吸收的就被反射出来

K~d~就是表示的是哪一个波长能够反射出来

给他三通道

RGB三个通道，哪个可以被反射出来，表现出来的就是漫反射的颜色了



在此，我们是默认反射的能量在各个方向是完全相同的

也就是从任意一个方向上看，反射出来的能量都是相同的，所以跟v没关系。



**漫反射说明了什么一个问题？**

![image-20241007181159767](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007181159767.png)

 就是说一个点从哪看都一样，和视角没什么关系。



漫反射到此为止。







#### 高光反射Specular

着色频率



渲染管线



高光都有一个特性，反射的方向都比较接近于镜面反射的方向。



给定I给定n即可求出镜面反射方向，越接近镜面反射方向越像镜子直接反射过来的。



高光的特性在于，其反射的方向通常接近于镜面反射的方向。这意味着当光线入射到物体表面时，其反射光线的方向非常接近根据镜面反射法则所计算的方向。

给定入射光向量 `I` 和法向量 `n`，可以通过镜面反射公式计算出反射光的方向。物体表面某点的观察者视角越接近于这个镜面反射方向，观察到的反射光就越强，从而显现出高光区域，类似于镜子直接反射光线的效果。



不同的材质有不同的镜面反射的光照分布，就比如说是个金属，镜面反射的样子差不多就像下面的R的形状

（好奇这个镜面反射的形状能不能改变的嘞）



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007210006886.png" alt="image-20241007210006886" style="zoom:33%;" />

视角方向越接近镜面反射的位置，越能看到高光

 

**用半程向量h，间接表示v和R接近。**

**发现当v和R很接近的时候，意味着h和n很接近。**

半程向量h由L和V的中间向量再做归一化而得。



![image-20241007210708485](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007210708485.png)

- n和h——能量保持不变，哪些方向保证了我能够看到高光——点乘n和h

- **k~S~**——默认是白的，默认是没做吸收，所以镜面反射系数默认是白的。差不多就是表示的就是亮度而已

- 指数P—— 不指数，直接做点乘，对高光的容忍度太大了，我们实际上要的高光就那么一点，离的非常非常近才认为是个高光点。几百的大小，差不多3°或5°都看不到了。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007211356737.png" alt="image-20241007211356737" style="zoom:50%;" />



看看横纵变化

![image-20241007211617109](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007211617109.png)



我们这里为什么不考虑多少能量被吸收了呢？（Diffuse里面的K~d~）

——简化了。



 Phong Reflectance Model——反射点乘v

Blinn-Phong Reflectance Model—— 半程点乘n（省计算）



#### 环境光照项（Ambient Term）

​	 **环境光照项**是光照模型中的一个基本组成部分，用于模拟来自各个方向的散射光。这种光源没有明确的方向和位置，代表了光线在场景中多次反射后形成的均匀光线。通过环境光照项，可以保证场景中没有任何地方完全处于黑暗之中，从而使得每个物体即使没有被直接光源照射，也能获得一定的基础亮度，确保整体视觉效果更加自然和和谐。



​	*事实上没那么简单。要真的完全计算出来，我们后面会运用到全局光照的知识，很难，后面再说。*



往下看，跟I,L,N,H,R什么都无关，实际上就是一个常数，提亮用的。然后可以给个颜色。（Blinn-Phong简化）

![image-20241007212031134](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007212031134.png)



#### 三项

![image-20241007212627702](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007212627702.png)



现实世界用到的的radiance辐射，光辉之类的东西，离得远会觉得暗，这是另外一种现象。



着色模型，我们考虑的任何一个点Shading point，这就涉及到——**着色频率**



### 着色频率

引入，同样的几何形状的球，不同的着色频率——

![image-20241007213630759](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007213630759.png)

1. 一个平面只做一次shading
2. 按照顶点做一次着色，顶点与顶点之间的光照通过插值生成。
3. 对每一个像素进行一次着色。对顶点的法线进行插值，然后每一个像素有一个自己的法线，每个像素可以做自己的着色。



#### **定义**

- **Flat Shading。**在三角形，根据两边叉积计算出三角形的面法线，三角形内部没有着色变化，按照面进行光照计算 。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007214151164.png" alt="image-20241007214151164" style="zoom:33%;" />

- **Gouraud Shading。**求得一个顶点的法线（顶点法线怎么求啊？），对每一个顶点进行着色，然后三角形内部的颜色用顶点着色后的颜色进行插值的方式计算出来。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007214324663.png" alt="image-20241007214324663" style="zoom: 33%;" />

- **Phong Shading。**对三个顶点求出各自的法线，然后对三角形里面每一个像素都插值得出他们各自的法线，然后对每个像素的法线都进行单独的着色，效果特别好。

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007214556582.png" alt="image-20241007214556582" style="zoom:33%;" />

  - 都是冯发明的模型，但是这两个不是同一个东西。一个是着色频率，一个是着色模型。



#### 着色横纵对比

比较简单的时候差别大，暴力加顶点得到的结果实际上视觉差距不大。

着色频率和几何本身的顶点，面的数量有关。 

当顶点数要比像素数量还要多的时候，说不定就是flat shading开销大了。

![image-20241007214934049](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007214934049.png)





#### **问题——**

##### **①如何得知顶点法线向量？**（顶点法线）

​									*——面法线通过叉乘相邻边获得。*

目前用的比较多的方法是，**共用该顶点的面**的法线进行加权平均，得到顶点法线。（右下角图）

（共用该顶点的面怎么求的？几何定义会说）

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007215617651.png" alt="image-20241007215617651" style="zoom:50%;" />



##### **②如何真正定义一个逐像素的法线？（像素法线）**  

​										—*—假设已知顶点法线。*

​														*别忘了归一化法线向量*

引入新的概念——**[重心坐标](https://blog.csdn.net/n5/article/details/122871039)**



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007221538872.png" alt="image-20241007221538872" style="zoom:50%;" />









## Graphics（Real-time Remderomh） Pipeline

我们学的



同时也提到了对新炸出来的采样点分别进行深度测试，写入frame buffer，这样才不会产生黑边（作业内容里的）

![image-20241007222305657](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007222305657.png)

 

硬件写好了的，整个三维场景在GPU里面大致都是这么个方法进行。



我们的着色，shading，可以自定义编程选择发生在顶点还是fragment，可<u>编程</u>的。

我们这里说的<u>编程</u>所提到的语言我们管它叫——**Shader**

控制顶点，像素怎么个方法进行着色，告诉GPU。

对我差不多就是 ShaderLab——HLSL——GPU





![image-20241007222911343](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007222911343.png)





然后提到纹理映射，怎么定义三角形内部不同的像素，显现出不同的像素。





<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007223219759.png" alt="image-20241007223219759" style="zoom:33%;" />



### Shader Programs

不用说写“每一个像素”这个么for循环，只需要告诉它一个顶点，一个像素怎么个做法即可。

已知GL的很像C++，简化了很多。

提到了**[shadertoy](https://www.shadertoy.com/)**

 

![image-20241007232528685](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007232528685.png)





## Texture Mapping纹理映射

如果想要让某个物体在不同地方的像素，各自反映出不同的漫反射。——定义任何一个像素点的不同的属性

![image-20241007233007626](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241007233007626.png)





三维物体

对于物体本身是三维的，但是表面是二维的。表面可以被映射成二维平面

直接把这个二维平面的纹理粘贴在三维物体上，就是一个映射的关系。

我们要找到**二维纹理上的每一个点和三维物体上的每一个点的对应关系——纹理映射**

对于三维上的每一个顶点如何对应，如何映射到二维上的纹理坐标——展UV



纹理坐标（UV坐标）是有坐标系的——[0,1]²



纹理上下左右的无缝衔接

四方连续

Tiled Texture







# Lecture 09 Shading3 (Texture Mapping cont.)

重心坐标（为了进行插值）

纹理怎么贴到表面上

纹理的应用







**What do we want to interpolate？**

我们要插值一些什么内容？

- 纹理坐标
- 顶点颜色
- 顶点法线
- 顶点切线
-  在顶点上的任意属性都可以进行插值



How do we interpolate？

**如何进行插值？**

↓

### Barycentric Coordinates（重心坐标）

根据三角形的三个顶点，给每个像素进行插值属性



==**在三角形ABC里面任意一点(x,y)都可以视为是三角形ABC的线性组合。**==——直接对三个系数进行相乘得出来的就是。



```C++
static Eigen::Vector3f interpolate(float alpha, float beta, float gamma, const Eigen::Vector3f &vert1, const Eigen::Vector3f &vert2, const Eigen::Vector3f &vert3, float weight)
// 重心坐标插值系数，传入三个权重值，三个顶点和权重值，返回插值后的顶点
{
    return (alpha * vert1 + beta * vert2 + gamma * vert3) / weight;
}
```

——三个顶点进行加权平均，得到的值再除weight

这个weight是三个顶点的w进行加权平均（单独做一次重心坐标插值得到）。





#### 定义①

**α+β+γ = 1**

- 如果满足α+β+γ = 1，并且三者都是非负的，那就说明这个点一定在三角形内；
- 如果满足α+β+γ = 1，但三者不一定是非负的，那就说明这个点在三角形所在的平面内。



![image-20241009213348847](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009213348847.png)





ABC三个点所在的重心坐标：

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009213844978.png" alt="image-20241009213844978" style="zoom:33%;" />





#### 定义②

面积比也能当做权重来看

**顶点对面三角形所占权重，即为α，β，γ的值**

![image-20241009214112850](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009214112850.png)



我记得求三角形面积有个**海伦公式**来算三角形面积来着，以前高中的时候感觉计算量很大很麻烦，用计算机算应该很方便吧。

 

#### 特殊的一个点

三角形本身的重心

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009214549134.png" alt="image-20241009214549134" style="zoom:50%;" />

有什么性质：

**连接三个顶点，把三角形分成了三个等面积的三角形。**



#### **==重心坐标公式==**

![image-20241009214800992](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009214800992.png)



好我们现在有了重心坐标了，怎么对三个顶点进行任意属性插值呢？





### 插值

**==三个顶点的任意属性都可以通过重心坐标进行线性插值==**，通过重心坐标进行线性组合算出任意一点的值

![image-20241009214912812](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009214912812.png)



### 问题

​	有一个问题，重心坐标在投影坐标下的插值是不准确的——算出屏幕上的投影坐标的重心坐标和被投影的三角形实际的重心坐标做一个计算发现是不一样的值。



#### 主要是想表达什么呢？

**==三维空间中的属性应该在三维空间做插值，然后再逆变换对应回来。==**

​	**在投影坐标的插值应该是：**

1. 在投影坐标进行三角形的重心坐标计算
2. 通过重心坐标还原到三维空间中的实际坐标
3. 在三维空间中的实际坐标对三个顶点进行线性插值
4. 将插值后的结果再重新映射回来



​	**怎么搞投影回来又投影回去？——投影矩阵逆变换即可**





### Applying Texture

点——插——查——贴





#### Texture Magnification——纹理的放大——==**Filtering**==





像素——pixel，画面的上的像素

纹素——texel，纹理上的像素



**现有的：**

- 双线性过滤（Bilinear Filtering）
- 三线性过滤（Trilinear Filtering）
- 各向异性过滤（Anisotropic Filtering）：各向异性过滤比双线性或三线性过滤提供了更清晰的纹理细节，特别是在斜视角下的表面或远处的地形等情况下效果明显。它通过考虑不同角度上的采样率差异来增强图像质量，能够提供更加逼真的纹理放大效果，尤其适合大角度观察纹理时的情况。





常见的纹理放大的变换方法：

![image-20241009220041907](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009220041907.png)

Nearest（临近）——非整数的（四舍五入）round成整数，太离散了

Bilinear（双线性）——

Bicubic（双向三线性）——



​	如果给定的坐标是一个非整数的坐标，怎么去得到他的值呢？

#### **双线性插值**

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009220637770.png" alt="image-20241009220637770" style="zoom:50%;" />

​	两个像素之间的距离肯定是1的，我们之前一直都这么说，然后加入在某个像素有那么一个要采样的点，我们就对临近的像素进行[0,1]进行**线性插值（Linear interpolation）**

- 上面两个像素之间插值，得到红点上面的像素点的颜色（水平）
- 下面两个像素之间插值，得到红点下面的像素点的颜色（水平）
- 对红点上下两个点的颜色进行插值（竖直）

​	<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009221111122.png" alt="image-20241009221111122" style="zoom: 50%;" />

​	**水平一趟插值（可反），竖直一趟插值，所以是双线性插值**



#### Bicubic

取周围临近的16个像素点，每次用4个做三次的插值



###  Texture Magnification——纹理缩小——==Mipmap==

纹理太大会带来什么问题？



**走样**，采样频率跟不上信号频率



![image-20241009222218186](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009222218186.png)



#### **问题在于：**

**纹理在屏幕上覆盖的纹理大小是不同的**

在远处，屏幕像素大小仍然不变，但是覆盖到的纹理像素却大了特别多。也就是信号变化过快，采样频率跟不上（实际上在这里我们采样频率没有变，只是信号在猛猛变快）



![image-20241009222351456](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009222351456.png)



如果按照之前的方法进行MASS抗锯齿超采样，512x确实可以得到还行的效果，但是开销特别大、

![image-20241009222613162](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241009222613162.png)



**理论上如何进行分析？**

遇到的仍然是走样的问题

在一个像素内他的信号频率很高，但我们只用了一个像素点去采样它，所以走样

 ——给更高频的信号增加更多的采样点（之前是模糊增加采样点）——太多了512x



**换一个思路**

不采样

预先处理，提前知道某个采样点，采样的区域的里面的平均值是多少。 



范围查询——区域性的查询方式

我们这用的是平均查询，有很多不同种类的

点查询？



#### **Mipmap范围查询**

只能做：

- 快速的
- 近似的
- 正方形的范围查询

![image-20241011120414515](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011120414515.png)





通过一张图生成一系列的图，逐半缩减。**Level = log~2~x**

![image-20241011120507002](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011120507002.png)



类似于一个图像金字塔，最高的是1 * 1的，最底的（level = 0）是原图

![image-20241011120903736](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011120903736.png)



计算存储数量：1 + 1/4 + 1/16 + 1/64 + 1/256 + ……  = 4/3

**只多了1/3**



立刻获得正方形区域内的平均值。任何像素都可以映射到纹理上的区域。



怎么得到像素点映射到纹理的区域大小呢？



屏幕空间4个4个取像素点，映射到纹理上，然后可以做一个近似——

**这里的微分的意义是计算出在屏幕坐标的xy移动的距离映射到uv坐标移动的距离是多少**，有的变化快有的变化慢。

两边求max获得正方形区域

大概意思就是在**像素上一个像素点的范围映射到纹理上的区域是多少**



![image-20241011121703653](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011121703653.png) 



**我怎么查询在纹理上那么个正方形的区域他的值是多少呢？**

预先说：层数**D = log~2~L**

如果该层（log~2~L）是4 * 4的纹素，那么下一层（log~2~L + 1）就会变成1 * 1的纹素

如果在纹理上是1 * 1 的像素点，那就直接取用像素就是区域上的颜色均值。

按照这个说法——

如果我在一个4*4像素大小的4个点上，那么该层（ 4 * 4 ）在下一层（log~2~L+1 ），也就是1 * 1的像素大小。我们就用下一层（log~2~L+1）的像素值（像素点对应过来的此处）来当做该层（log~2~L）的4个像素的颜色。

即

**下一层的纹素大小替代本层的4个纹素范围大小**，即做该正方形区域的纹素范围值查找



如果要找16 * 16纹素大小的。那就log~2~L + 2层去找

以此类推，我们提前把log~2~L + x 层所有都做个提前存储

然后

在我的屏幕上，一个像素大小对应的纹素大小是几乘几的**正方形**大小，然后去找log~2~x层的mimap，用那个层的该像素点对应的纹素来当做要采样的纹素（1 * 1大小的纹素）。（之前的方法直接采样会导致一个像素采样多个纹素）

核心——为了1 * 1的像素采样 1 * 1的纹素



基本都是：这个像素点投影到纹理上，是多大个区域

思维转换成

这么大个像素点。我要到第几层的mipmap处找它的像素平均值。

![image-20241011124743448](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011124743448.png)



**现在的查找有一个跳变，两个level之间似乎不怎么连续，来解决如何让level之间变得连续。**

我们之前只是算了离散的若干层log~2~x个层。

那两个层之间呢？

插值



**如何插值？**

看↓，这个像素（映射到纹素）在level D层，然后我们再找它的level D+1层，两层

这个两个像素之间做了双线性插值才找到红点像素的对应的颜色。

**查找到了在D层的颜色，和在D+1层的颜色。再对这两层颜色进行插值。——三线性插值（Trilinear Interpolation）**



**像素与像素之间可以进行插值得到浮点处的像素颜色。**

**层与层之间也可进行插值，获得浮点层的像素颜色。**

三个维度的颜色都可处理，颜色完全覆盖。

![image-20241011125601827](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011125601827.png)



三线性插值之后

![image-20241011130754503](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011130754503.png)





**有无解决问题？**



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011135116794.png" alt="image-20241011135116794" style="zoom:67%;" />



<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011135130876.png" alt="image-20241011135130876" style="zoom:67%;" />

远处产生了OverBlur

**为什么会产生这种问题？**

mipmap只能针对正方形的图形





**部分解决**

**各向异性过滤——**

各向异性：各个方向各不相同的表现

多了3倍的存储空间

打游戏实际上有多高开多高差别不大。只要显存足够，有多高开多高。

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011135328546.png" alt="image-20241011135328546" style="zoom:50%;" />



对不同的长宽比进行不同方向的压缩变换 ，对应不同的像素可以查询到被压缩之后的像素区域

![image-20241011135741686](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011135741686.png)



我们的mipmap只能计算正方形，那屏幕上框个正方形，实际上在纹理上的表现就是这样的：

![image-20241011140345938](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011140345938.png)

然后求的正方形的mipmap实际上是求了那么大一片区域的平均颜色范围（求上下左右正方形区域的平均颜色）

所以才会产生overblur



各向异性过滤可以解决纯矩形的查询，像是<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011140551186.png" alt="image-20241011140551186" style="zoom:33%;" />斜着的矩形，仍然不能解决问题。





EWA过滤

![image-20241011140614740](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011140614740.png)







### 纹理

**==纹理可以存储各种各样的信息，信息的利用取决于你如何在着色器中解释它。==**



### Environment Map

用纹理去描述环境光，然后用环境光去渲染其他物体（无限远处，只有方向）

![image-20241011152823427](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011152823427.png)





#### Spherical Environment Map

把不同的光记录在球面上

![image-20241011153152466](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011153152466.png)



球形环境光贴图的问题——上下扭曲

![image-20241011153237277](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011153237277.png)





#### Cubema

改进球形贴图

![image-20241011153418310](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011153418310.png)





### 纹理还可以影响着色

纹理不单单拿来改变漫反射的K~d~，它还可以拿来影响其他东西。

可以用来改变表面上任意一点的属性



**法线贴图**

![image-20241011153856685](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011153856685.png)



几何没有受到改变只是改变着色，看上去好像有改变

法线贴图主要是通过高度贴图对表面的点进行扰动，然后对某个点进行法线的重新计算。



![image-20241011154632536](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011154632536.png)

现在要求移动后的点的法线n



#### **在平面上**

![image-20241011161359372](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011161359372.png)



h(p)——p点的高度

dp——p点的切线高度

凹凸贴图定义切线，然后用切线来算法线

**求点P的法线：**

1. 原始表面的法线(0,1)——这里默认向上
2. 求得下一个点到该点的竖直方向偏差（1，dp）
3. 90°旋转，相当于变换一下位置：**（-dp，1）**.normalized()



#### 在3维上

![image-20241011162418769](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011162418769.png)



**步骤：**

1. 默认假设原始法线(0,0,1)——默认向上
2. 求得在uv方向变化一个单位，对应p变化多少——两个方向做微分
   1. dp/du
   2. dp/dv
3. 切线算出来了，按照（有推导方法，如果好奇）平面的90°旋转的，**(-dp/du,-dp/dv,1)**.normalized()



**我们一直在假设默认向上方向为(0,0,1)作为法线，我们怎么做到一直认为向上的方向是法线呢？**

**==切线空间==**——局部坐标系，法线，副法线，切线形成。法线永远是(0,0,1)

算完之后的切线空间的法线，再重新变换到世界坐标



### 贴图另一方面——位移贴图（Displacement mapping）

![image-20241011163421749](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011163421749.png)

给顶点真的做一个位移，但是三角形得足够细，顶点足够多





DX（只能用在window上）有一个动态曲面细分——在真的需要的时候，可以增加曲面细分，进行顶点增加。



#### 程序化纹理

3D空间真实的噪声（切开来里面真的有那么些点）

<u>Perlin noise</u>



#### AO

AO贴图，存储环境光遮蔽信息

![image-20241011164016052](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011164016052.png)





### 体渲染（Volume Rendering）

三维的纹理多用在此处





**什么是体渲染？**

比如医院里做的核磁共振，根据信息进行渲染显示成像。未来会讲。





# Lecture 10 : Geometry1

几何的解释

**显式几何**

- 点云(point cloud)
- 多边形网格(polygon mesh)
- 细分(subdivision,nurbs)
- ……



**隐式几何**

- 代数曲面（algebraic surface）
- 水平集(level sets)
- 距离函数(distance functions)
- ……



### **隐式几何**

规定几何的函数关系，但不直接说点在哪。满足某个关系的点，认为都在几何的表面上。



例如：x^2^+y^2^+z^2^=1



![image-20241011165622580](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011165622580.png)





**优点：**

- 很容易判断一个点是否在结构之内或之外或在结构上

- **拓扑变化处理方便**：隐式几何可以自然地处理复杂的拓扑变化，如几何体的合并、分裂、穿孔等现象。因为边界是隐式定义的，所以当几何体变化时，边界的拓扑结构不需要显式维护。

  **几何光滑性**：隐式函数常常是光滑的，因此它在处理曲面时具有更好的连续性和光滑性，这对于表面光滑性要求高的应用很有帮助。

  **几何分析简单**：通过隐式函数，几何属性（如法向量、曲率等）可以通过函数的导数计算出来，而不需要显式网格化。

  **物理现象模拟自然**：隐式几何特别适合模拟随时间变化的几何现象（如流体表面、火焰前沿、相变等），因为通过解偏微分方程就可以直接演化几何体的边界。

  **存储效率高**：在某些情况下，隐式几何使用标量场来表示几何形状，比存储大量显式顶点和边要更节省空间。

**缺点：**

- 根据式子直接看，很难看得出来结构长什么样

- 点的变化是连续的，我不可能把每个连续的点都带进去计算

- **重建复杂性**：由于几何形状是隐式定义的，直接可视化或构造显式网格（如三角形网格）较为复杂，通常需要额外的步骤（如 Marching Cubes 算法）来重建几何表面。

  **无法直接控制形状**：在一些精确的几何造型场合，隐式几何的控制较为间接，难以精确控制几何形状的局部特征。

  **计算代价高**：隐式几何的计算通常依赖于偏微分方程的数值解法，这在计算复杂场景（如流体动力学、材料科学模拟）中可能非常耗时。

  **不适合简单结构**：如果几何形状本身非常简单，使用隐式几何可能比显式几何更加复杂、冗余。



**特点：**

**方程定义几何**：形状由一个函数定义，而不是通过明确的顶点或边界。几何形状的边界是方程的解集。（没有顶点的概念）

**连续表示**：隐式几何是连续的，不依赖于具体的离散表示（如网格）。通常用于表示复杂的曲面，特别是流体模拟、CSD（碰撞检测）等。

**易于处理拓扑变化**：比如形状的合并、分割和穿孔等。隐式几何可以很好地描述这些变化。





### 显式表示

**直接给出**——就是个模型，显式地表达出来。

**参数映射**方式给出——**参数定义**

- 通过参数化的公式，直接给出每个点的具体坐标。例如，圆的参数方程可以表示为 \( (x, y) = (r · cos(t), r  · \sin(t)) \)，其中 t 是参数。



显式几何的特点是：

- **明确的边界表示**：几何体的形状由顶点和边界确定，易于渲染和处理。
- **易于直观理解**：特别适合处理和绘制对象的外形。
- **直接计算**：通过明确的顶点和边界表示，显式几何适合用于碰撞检测、光照计算等。



**优点：**

- **易于理解和操作**：显式几何可以通过直接访问顶点、边、面的坐标进行精确操作。适合几何造型、动画等场景，便于设计人员和程序进行控制。

  **渲染效率高**：显式几何特别适合于现代图形硬件进行渲染，三角形网格可以直接用于 GPU 加速，渲染速度快，计算开销低。

  **直接控制形状**：显式几何表示可以对几何形状的细节进行精确控制，如移动、缩放、旋转顶点，或者对网格进行局部调整。

  **处理简单几何有效**：对于简单或规则的几何体（如立方体、球体等），显式表示通常比隐式表示更加高效和直接，构建和计算开销较低。

**缺点：**

- **拓扑变化难处理**：显式几何的拓扑是显式定义的，当几何形状发生合并、分裂等拓扑变化时，需要进行复杂的重新定义和更新。

  **存储开销大**：复杂几何体需要存储大量的顶点、边、面信息，存储和内存开销较大。

  **不易处理动态变化**：显式几何在处理物体形状的连续动态变化时（如流体、烟雾等），由于需要不断更新几何结构，计算和存储的开销非常高。

  **计算属性困难**：几何体的一些属性（如曲率、法向量）可能需要额外计算或插值，特别是在几何体较为复杂的情况下。

  **光滑性较差**：显式几何（如多边形网格）常常是离散的，因此当需要表示非常光滑的表面时，显式几何可能需要大量的顶点和面片来近似，效率较低。





![image-20241011170558569](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011170558569.png)



根据参数化，代入需要知道的顶点进去，就可以知道物体的形状。

![image-20241011171053129](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011171053129.png)



但是判断点是否在物体里面还是表面还是外面却变难了。



**只能各取所需**







更多的隐式表达方法

可爱捏

![image-20241011171913223](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011171913223.png)



隐式方法表达一下——

![image-20241011171926242](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011171926242.png)





### 隐式

#### CSG（Constructive Solid Geometry）

通过一系列几何的基本运算来定义一个新的几何（布尔？）



![image-20241011172158221](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011172158221.png)

 

组合结合交差并

![image-20241011172203577](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011172203577.png)



#### 距离函数（Distance Functions）



空间距离函数

**空间中任意一个点**到想要表示的空间几何形体上面的任意一个点的最短距离（可正可负）。正的表示在外面，负的表示在内部



空间中的任意一个点都可以定义出那么个值来



对距离函数做融合



![image-20241011172359267](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011172359267.png)







**简单的例子：边界融合（Blending  a moving boundary）**

![image-20241011172813590](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011172813590.png)

上面的A和B他并不能表示某种运动信息，只是显隐，有无的信息

下面：

- 左边的A找到边界位置为0，左右有向的距离值
- 中间的B找到编辑位置为0，左右有向的距离值
- 然后两个进行融合，得到的就是中间边界0往左右的有向距离（负在内，正在外）
- 如果对sdf图进行恢复，大致意思就是中间是边界，左边是黑的，右边是白的



**融合blend**

​	 blending是线性组合, 通过线性合并点到N个几何表面的distance可得到点的新distance, 正负值表示它与新几何形体的距离, 描绘出blending过的点就可以构建新的几何



![image-20241011173608319](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011173608319.png)





#### 水平集（Level Set）

**二维：**

用于跟踪和表示动边界（moving boundaries）或界面（interfaces）的方法，广泛应用于计算机图形学、计算流体力学、图像处理和物理模拟等领域。

不用完全准确的值，而是用近似值。然后用里面的值进行融合，插值。插值得到有个0的地方，那就是边界。

![image-20241011174451717](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011174451717.png)



**三维：**

三维空间上的格子：

空间中的不同密度，根据三维信息提取物体表面。

如果有那么个密度的值等于某个值，那就是相当于找到了某个表面。有了表面那就找到了纹理。



#### 分型（Fractals）

自相似。

自己某个部分和整体很像。

![image-20241011175229790](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241011175229790.png)





# Lecture 11/12(Geometry2 Curves and Surfaces)

### 显式表达

- #### 点云（Point Cloud）

  - 用特别大量密集的点来模拟表面。点多得特别多没有缝隙的时候就是面了（怎么表示三角面？）
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013154153394.png" alt="image-20241013154153394" style="zoom:50%;" />

- #### 多边形面（polygon mesh）

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013154223024.png" alt="image-20241013154223024" style="zoom: 33%;" />

    



### Wavefront Object File(.obj)

我们的

![image-20241013154440339](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013154440339.png)



顶点

纹理坐标

顶点法线

f——组三角形的索引顺序（顶点，纹理坐标，顶点法线）





### Curves

- #### 贝塞尔曲线

  - 用一系列控制点去满足性质，定义一定经过起始点，
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013160251986.png" alt="image-20241013160251986" style="zoom: 33%;" />
  - ![image-20241014130304621](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014130304621.png)——伯恩斯坦**三阶**权重，即**三次贝塞尔曲线**（四个控制点)

- ##### Casteliau Algorithm（卡斯泰尔诺算法）

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013160848126.png" alt="image-20241013160848126" style="zoom:50%;" />
  - ![image-20241014144746931](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014144746931.png)
  - 递归找所有点t在曲线上的相对位置，求第一段上t相对位置，然后第二段，两点连起来再求个相对位置，就是曲线上的会经过的点
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013161057218.png" alt="image-20241013161057218" style="zoom:50%;" />
  - 

- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013161215181.png" alt="image-20241013161215181" style="zoom:50%;" />

- #### **计算方法（伯恩斯坦多项式）**：

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013162836899.png" alt="image-20241013162836899" style="zoom: 80%;" />
  - ![image-20241014130449806](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014130449806.png)
  - ![image-20241014130304621](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014130304621.png)
  - t表示所有在[0,1]上的点（遍历，0-1，每次增加0.001）——各点权重
  - ![image-20241014131540252](D:\TyporaImage\image-20241014131540252.png)组合数。是这么来的![image-20241014131602110](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014131602110.png)，也就是——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014131755609.png" alt="image-20241014131755609" style="zoom: 50%;" />
    - 例如，如果是3阶权重，则**n = 3**，也就是对于每一个点在t权重值的时候为：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014130824312.png" alt="image-20241014130824312" style="zoom: 67%;" />。
    - 对于每个点的权重计算之后，乘对应的点（p~i~），然后进行权重相加，就是在这个权重的时候，曲线上的点。
    - 只要点够密集，所有点相加，就成了一条线段了。

- ### 贝塞尔曲线性质：

  1. 一定会经过起始点
  2. 其实的切线方向<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013163450706.png" alt="image-20241013163450706" style="zoom:67%;" />
  3. 适用于仿射变换，适合在集合变换中变换
  4. 凸包性质
     - 贝塞尔曲线的形状总是在其控制点的凸包之内。凸包是将所有控制点连接形成的最小凸多边形。这个性质表明贝塞尔曲线不可能离开控制点构成的凸区域，这使得它的形状更容易预估。





贝塞尔曲线有个问题，引入逐段贝塞尔曲线

不是所有点共同确定一个贝塞尔曲线

而是多个点确定多段贝赛尔曲线





- #### 逐段贝塞尔曲线（piecewise Bezier Curves）

  - 多数是4个控制点定义三条贝赛尔曲线

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013164157119.png" alt="image-20241013164157119" style="zoom:50%;" />

    - 这里的四个点指的是<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013164215223.png" alt="image-20241013164215223" style="zoom:50%;" />

  - 如何让曲线**是否连续**？

    - 两头的控制杆的长度和方向是等距反向的

  - #### 连续的阶数

    -  **C^0^连续：a~n~ = b~0~**，几何上，第一条线段的终点是第二条线段的起始点。**——无断点**
    -  **C^1^连续：**也叫一阶导连续，保证C^0^连续的情况下，切线也连续，即：a~n~ = b~0~ = 1/2 * （a~n-1~ + b~1~）。也就是切线的方向相反，长度等长<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013165053686.png" alt="image-20241013165053686" style="zoom:33%;" />**——无突变**

- #### 其他曲线

  - #### 样条曲线（Spline）

    - a continuous curve constructed so as to pass through a given set of points and have a certain number of continuous derivatives. （a curve under control）

  - #### B-Spline

    - 为了局部性
    - basis splines 基函数样条
    - Bernstein Polynomial作为基函数，
    - 满足局部性
    - 可能是图形学里面最复杂的一部分
    - 是贝塞尔曲线的超集





### Surfaces

- **Bezier Surfaces**

  - 怎么通过贝塞尔曲线去获得贝塞尔曲面？
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013170405373.png" alt="image-20241013170405373" style="zoom:33%;" />
    - 上图是由4 x 4个控制点得到的
    - 步骤为：先水平/竖直方向先进行插值得到4条贝赛尔曲线<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013170745108.png" alt="image-20241013170745108" style="zoom:25%;" />，然后再不同的时间t上位置不同，能水平插值出一系列的曲线<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013170829018.png" alt="image-20241013170829018" style="zoom:33%;" />。对所有时间t上的积累能够画出一个面：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013170901463.png" alt="image-20241013170901463" style="zoom:33%;" />，16个点所经历的：![image-20241013171002119](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013171002119.png)

  - 两个不同时间t（u,v）

    4x4个点，四条4个控制点的贝塞尔曲线，取同一时间（比如说u）获得四个控制点，取时间v，即获得最后的曲面上的点

    - 通过uv进行参数映射过去找到对应的曲面上的点





- **Subdivision surfaces（triangles & quads）**
  - Geometry Processing
    - Mesh subdivision(网格细分)
    - Mesh simplification（简化）
    - Mesh regularization（正规化）



### Subdivision曲面细分

- 增加更多的点以增加更多的三角形
- 对顶点进行位移，使得物体能够更加光滑一点（越来越圆滑）



- ### Loop Subdivision

  - **只能用在三角形面。**
  - 连接三角形的中点生成更多的三角形。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013193120906.png" alt="image-20241013193120906" style="zoom:33%;" />
  - 对于新/旧顶点，采用不同的移动规则来改变顶点位置。
    - **新：**加权平均（具体算法推到不介绍）![image-20241013193718572](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013193718572.png)
    - **旧：**根据周遭数量进行加权平均。如果引入的u的规定：如果n=3，则u = 3/（8n），否则都是3/16![image-20241013195030817](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013195030817.png)





- ### Catmull-Clark Subdivision(General Mesh)

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013195736522.png" alt="image-20241013195736522" style="zoom:33%;" />
  - 很有用。第一次就会增加“非四边形面”个奇异点。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013200450617.png" alt="image-20241013200450617" style="zoom:33%;" />
  - 根据三个边的中点进行相连，然后形成形成一个新的奇异点。之后——
    - 非四边形面消失了，不会再增加“非四边形面”个奇异点
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013200311395.png" alt="image-20241013200311395" style="zoom:50%;" />
  - 自此，一直两点中点相连，不会增加奇异点的个数，也一直会是三角面。



### Mesh Simplification曲面简化

- 方法一：**边坍缩**
  - 实现方法：**二次误差度量（Quadric Error Metrics）**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013202029736.png" alt="image-20241013202029736" style="zoom:50%;" />
  - 二次误差：一个点如果和周围的几个点都有关系，那就求这个点到周围几个面的距离的平方和。然后优化位置，使得二次误差最小（排序方法的择取），找到最小的那个二次误差。这个操作过程即为**二次误差度量**
  - 蓝色点，找到一个到周围四个边的和的最短距离。找到最小的二次度量误差，求得最优的位置，使得对周围的边的影响是最小的。
  - 坍缩还有顺序：为每一个边打上一个分数，从小到大进行坍缩。比较小的坍缩的就多一些。但是如果是对小的边进行了坍缩，还是会对其他的边进行影响。
  - 我们要：①最小的代价取最小的二次度量误差；②以最小的代价，对二次度量误差做更新。——堆结构排序<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013202821554.png" alt="image-20241013202821554" style="zoom:67%;" />
  - 贪心算法，非全局最优。是以局部最优解映射到全局，不一定是最好的。
  - 有的放矢。







### Shadows

遗漏话题——光栅化里面是怎么生成阴影的

- ### Shadow mapping

  - 用光栅化的思想来解决阴影问题。
  - 图像空间的做法，此时不需要知道场景空间的几何信息
  - 会走样
  - 能被光照所看到，也能被摄像机所看到的，就是光照区域，反之则为阴影区域
  - 我们所说的就是点光源或者方向光

- Pass 1 ： Render from **Light**

  -  从光照作为摄像机出发，找向场景
  - 此时我们专门记录**深度图**，但是不记录颜色。

- Pass 2A：**Project** to light

  - 从视角方向出发，看见物体
  - 然后将摄像机视角看到的物体投影会光源视角，判断在光源视角下，看到这几个点是处于哪个位置。
  - 如果摄像机所看的深度值（转换到光源空间）和光源原本记录的的深度值是一样的 
  - 如果摄像机所看到的深度值（转换到光源空间）和原本光照所看到的深度值不一样（光源深度记录在遮挡光源的物体上了），那就是在阴影里面

- **存在的问题：**

  - “是否相等”的精度问题对于相等的判断有误差（大于也仍然不能解决问题）
  - shadowmap本身就有分辨率大小之分
  - 要渲染两遍，开销大。镜头一遍，光照一遍
  - 只能硬阴影  

- ### 软阴影

  - 由近及远会变淡
  - 由近及远会变软
  - ![image-20241013210023253](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241013210023253.png)
  - 平行光基本没有软阴影，==**有软阴影那一定是因为光源有一定的大小**==





# Lectur 13 Ray Tracing

**主要要解决的问题**

- 遗留的光栅化问题（有光栅化解决办法但是很麻烦）
  - **软阴影**
  - **Glossy reflection**——毛玻璃 <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014171041069.png" alt="image-20241014171041069" style="zoom:67%;" />
  - **间接光**

- **光栅化只是一种画质的快速近似，画质质量比较低。**——实时





**光线追踪问题**

- **很慢**——主要是用来做离线渲染



**用Ray Tracing的原因**

带来完完全全正确的算法，符合物理规律



### 前奏

**定义光线**

1. 光线沿直线传播（实际显示光是有波动性的）。
2. 光线与光线之间不会发生碰撞（实际上还是会有的）。
3. 光线是可逆的光路。光路有可逆性reciprocity。**从眼睛发出<u>感知光线</u>反弹到光源（光线追踪这个Tracing的意思就是指的这个）**。



#### **着色**

​	就是从摄像机隔着一个屏幕发出一条射线打在物体上某一点。这个点与光源做连线看是否有遮挡，没有遮挡说明就是一条可行的光路（不在阴影之内），我们可以计算光路上的能量（多段，如果有衰减可额外加），根据这个光路能量将颜色算出来。——光线投射



#### 假设

- 假设眼睛是针孔摄像机
- 光源是点光源
- 先无大小形状
- 对于被打到光线的物体，默认是完美的折射或反射



#### Eye ray

眼睛射线



#### Shadow Ray

判定是否在阴影

被打到的物体向光源的连线。如果有东西挡着，说明点在阴影里。

  

**解决了深度测试的问题，不用给像素写深度缓存了**

- Eye ray射线与物体产生的第一个交点就是最近的一个物体



**有**

- L
- N
- I
- 可以算着色，算出来，写入像素



![image-20241014173825207](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014173825207.png)









## Whitted-Style Ray Tracing

只要能正确计算出来反射方向与折射方向，那感知光线就可以继续传播下去

**每一个接触点都要和光进行一次着色计算，每一个弹射点的值都给加入到像素的值里面。** （被挡住了那就没有，就不加）。光路的能量损失定要考虑进去的

![image-20241014174808769](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014174808769.png)<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241014174817398.png" alt="image-20241014174817398" style="zoom:50%;" />

- 对于纯玻璃球，光线还是可以传过去，损失一点但至少下面的阴影不是全黑的





#### **目前技术问题总览：**

- **光线有效性判定**（光路有效）
  - 判断光线是否在场景中有效传播，即如何确定光线在传输过程中没有被物体遮挡（光线可见性问题）。
  - ——**包围盒层次（BVH）** 或 **光线盒相交检测**
- **[光线与物体交点计算](#Ray-Surface Intersection(交点判定))**（落点判定）
  - 确定光线与场景中几何体的交点，包含初次相交位置的计算以及后续反射和折射路径的交点判定。
  - ——**光线-三角形相交算法**（如 Möller–Trumbore 算法）
- **[反射光与折射光的方向计算](#Bidirectional Reflectance Distribution Function（BRDF）——双向反射分布函数)与[能量](#Radiometry ——Motivation)分配**（落点方向与衰减）
  - 计算反射光和折射光的传播方向，并准确描述它们的能量分配情况，确保遵循能量守恒的物理原理。
  - ——**BRDF**
- **光照能量衰减及能量守恒**（路径衰减）
  - 根据光源与交点的距离以及介质的特性，计算光照的能量衰减情况，确保整个光线追踪过程中的能量守恒。
  - ——**逆平方衰减** 和 **菲涅耳反射模型**







**开始解决技术问题**







### Ray-Surface Intersection(交点判定)

**定义光线**

- ### **==R(t) = o + td (0<= t < ∞)==**

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015000153677.png" alt="image-20241015000153677" style="zoom:67%;" />



**求t的意义就是光线会在什么时候和物体产生交点**



**光线如何与物体求交**

在某个形状上的某一点P

**以球为例**

![image-20241015000845175](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015000845175.png)

既要满足射线的方程，又要满足球的方程，即射线的值与球上一点相等。

代入即可

 求解t，其他都已知

根据初中数学

运算的知识：

![image-20241015001343510](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015001343510.png)

**满足实际意义：**

- t必须是正数
- b²>=4ac——才有交点（）
- 相离——无交点
- 相切——一个交点
- 相交——两个交点
  - ![image-20241015001644387](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015001644387.png)



 **引申到光线与隐式表面的交点**

实际上就只是将隐式表达式上，用光线表达式的值进行代入

![image-20241015004619333](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015004619333.png)

只有一个未知数求解时间 t

然后把t代入到Ray，即可求得光线的值

仍然需要满足：

- t必须是正数
- 有物理的实际意义——**实数**



**显式表面——**

#### ==求射线和三角面的交点==

判断是否有交点就可以得知：

- 是否光线被遮挡
- 是否有阴影
- 是否在物体内部（如果光线在物体内部发射，则交点为奇数；反之为偶数）



**做法——如何判断**

- 对所有线与三角形的交点都做判断，找到最短的那一根（图形学里面不考虑平行）
  - 特别慢，肯定有[方法](#Accelerating Ray-Surface Intersection——加速光线与表面交点计算)能够加速这个步骤
- 光线对三角形所在平面求交点，然后判断交点是否在三角形里面
  - 如何定义平面？——求平面的法线+三角形的某个点的平面内向量（向量是射线交点和三角形某一点）
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015010903609.png" alt="image-20241015010903609" style="zoom:50%;" />
  - 即——交点以射线的值代入，然后与三角形上的某一点形成向量。这个向量与法线点乘为0——垂直，即可求得t。代入t到射线的值，得到交点。判断交点是否在三角形内。
  - ![image-20241015011302002](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015011302002.png)



 **如何立刻求得与三角形的交点，然后判断在三角形内？↓**

#### Moller Trumbore Algorithm

![image-20241015011756486](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015011756486.png)

求解线性方程组（要解的时候去网上抄吧）

-  P：三个顶点
- O：射线起点
- D：方向向量
- t：参数变化量
- b：重心坐标
- S：三角形三个顶点到起点的向量

![image-20241018210156492](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018210156492.png)



- ### 为True的条件

  - 分母有意义
  - 右边三个都得大于等于0
  - 且1-u-v得大于等于0





### Accelerating Ray-Surface Intersection——加速光线与表面交点计算

 原本的方法的计算次数：**#像素（射线数量） * #三角形数量 * #反弹次数**



**概念**——

#### **包围盒（Bounding Volumes）**

特别有效

如果光线连物体的包围盒都碰不到，那就更碰不到里面的物体

![image-20241015012550991](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015012550991.png)



​         **长方体**——理解成三个对面形成的交集。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015012822257.png" alt="image-20241015012822257" style="zoom:33%;" />



**Axis-Aligned Bounding Box（AABB）（轴对齐包围盒）**





**如何判断光线与AABB包围盒求交？**

- ### 二维情况下
  
  - ![image-20241015013655358](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015013655358.png)
  - 分别求得 与两个平行的Y平面和X平面 求交点
  - 然后两条线段求交集
  
-  ### 三维情况下
  
  - 什么时候进入盒子？——**==用t作为判断依据==**
    - 光线进入三个对面内，则为进入；——**==即三个时间取最大（最后面的），作为进入时间t~enter~==**
    - 光线出其中一个对面内，视为退出。——**==即三个时间取最小（最先出去的），作为退出时间t~exit~==**
  -  3D包围盒的**t~enter~**就是最后一个进入三个对面内的时间
  - **t~exit~**就是最早从三个对面的其中一个对面出去的时间
  - 只要**t~enter~**早于**t~exit~**，那就说明光线在包围盒里面停留了一段时间
  - ![image-20241015014631235](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015014631235.png)





**在满足t~enter~<t~exit~前提下，如果t是负的怎么办？**

- 如果t~exit~<0，说明盒子在光线背后，不满足——**没有交点**
- 如果t~enter~ <0，但t~exit~>0，说明光源的起点在包围盒里面——**肯定有交点**



- ### t的求法

  - 根据公式：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241019163508498.png" alt="image-20241019163508498" style="zoom:67%;" />

  - 倒推为<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241019163522388.png" alt="image-20241019163522388" style="zoom:67%;" />

  - 由于六个面，我们需要进行6次计算取得不方向的时间

  - 但是此时时间是默认从Min→Max的，如果光照方向反过来从Max→Min，那我们就要进行一次反向——把接触Max的时间与接触Min的时间进行互换即可

    ```C++
    std::swap(t_Min_X, t_Max_X);
    ```

    



得出结论



- ### 何时有交点？

  - 当且仅当（iff）——**==t~enter~<t~exit~ && t~exit~ >=0==**==的时候才满足有交点==




###### 两种交点求法的差异

<u>包围盒的计算与直接平面求法线与向量垂直的计算量对比</u>

![image-20241015015552726](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015015552726.png)

省去了很多计算量

我们只要求某一方向上垂直的量即可。

**只要求得到包围盒的某方向分量，除以距离在 方向上的值的累积即可得出累积时间**（d在x方向上的变化倍率）。（反过来理解，x垂直分量通过t的时间倍增到距离d）

**分量——累积——时间**



##### AABB包围盒如何能够加速我们对于光线求交的计算？——加速结构为什么能够加速？

- **均匀格子——场景预处理**
  - 首先找到个AABB包围盒
  - 然后再生成小格子（小盒子）
  - 将和**物体表面**相交的格子（盒子）都标记成某一类型的格子，只标记相交的，在内部的不用
  - 光线与实际物体相交很慢，和格子（盒子）相交很快
  - 如果撞上了“包围了物体表面的格子（盒子）”，那就意味着光线有可能与物体相交了。
  - 然后判断光线是否与物体相交。  
    - **加速效果如何？** 
    - **尝试出来一种数，格子不能太稀疏也不能太密集，恰巧很平衡：27 * 物体个数（但我们不用这个）**
    - ![image-20241015142040337](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015142040337.png)
    - **所以那种加速效果比较好？**
    - 上面这种**均匀格子**在某些场合确实有用，但我们主要用下面的方法
- **空间划分**
  - 想法：密集的地方格子多一点，稀疏的地方格子少一点
  - 三种空间划分的结构：八叉树，KD树，BSP树
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015144431613.png" alt="image-20241015144431613" style="zoom: 33%;" />
  - 八叉树
    - 什么时候停下来？人为定义划分的差不多少的时候就可以停下来了。
      - 二维是四叉树，三维八叉树，2^x^叉树
  - **KD树**
    - 类似八叉树，X/Y/Z**交替**做**一次**对半划分（交替以至于不会越来越窄）
      - 比较好算
  - BSP树
    - 类似KD树，也是某个方向砍，只不过不是横平竖直的（不太好算）
      - 维度高会越来越不好算



##### KD树 加速结构预处理 ——找到交点！

蓝的也要砍

绿的也要再砍

全都要再砍

然后形成一棵树



![image-20241015150610833](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015150610833.png)





**如何建树？**

- 对于任意的节点，都应该知到要往哪个轴划分
- 划分的位置（不一定从正中间划分）
- 划分一次，两个子节点
- 实际的三角形（物体）只存在于叶子节点上，不在根节点



假设现在就长这样，先不做更细的划分

叶子结点就是12345(有颜色的那几个)



**判断**——从叶子结点找三角形

**逐层判断**——递归

1. 光线穿不穿过根节点（盒子）？穿过则求交，有个[t~min~ , t~max~]     ↓  说明和子节点可能有交点
2. 光线穿不穿过根节点的叶子结点（盒子）？穿过则      ↓  ，没交点就不判断，省！
3. 判断叶子节点（盒子）里面，光线与三角形（物体）是否有交点，有那就有，没有就没有。
4. 达到**“归”**没？没有就继续      ↓
5. 光线穿不穿过根节点（盒子）？穿过则求交，有个[t~min~ , t~max~]     ↓  说明和子节点可能有交点
6. 光线穿不穿过根节点的叶子结点？穿过则      ↓    ，没交点就不判断，省！
7. 判断叶子节点（盒子）里面，光线与三角形（物体）是否有交点，有那就有，没有就没有。
8. 达到**“归”**没？没有就继续      ↓
9. 光线穿不穿过根节点（盒子）？穿过则求交，有个[t~min~ , t~max~]     ↓  说明和子节点可能有交点
10. 光线穿不穿过根节点的叶子结点？穿过则      ↓    ，没交点就不判断，省！
11. 判断叶子节点（盒子）里面，光线与三角形（物体）是否有交点，有那就有，没有就没有。
12. 达到**“归”**没？没有就继续      ↓

![image-20241015152238009](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015152238009.png)



**==但是但是但是，我们仍然不用KD树来判断！！！==**



因为——



怎么判断三角形是否在格子里面呢？或者说，物体根哪个格子有交集？判断不了，难<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015152822226.png" alt="image-20241015152822226" style="zoom:50%;" />！**很难**。

- **问题①：**一个物体很有可能出现在很多个格子里面，一个多好干嘛在那么多个格子里面都有这个物体。
- **问题②：**KD树的建立很难，需要考虑三角形和盒子的求交，我们之前都没判断都只是很平整地切割了这么个格子而已







新的划分方法

- ### **从物体（Object Partitions）上做划分**



#### Object Partitions & Bounding Volume Hierarchy(BVH)

**BVH**很广泛的应用！实时的和离线的**基本都在用**





**仍然是一种数据结构的树查找结构**





![image-20241015153815045](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015153815045.png)







- ### **解决的KD树问题：**一个物体只会出现在一个Bounding Box里面



- ### **盒子求法：**

  - 物体分堆


  - 重新计算包围盒，求极值，xyz最小和最大

  - 划分到差不多一个节点里面只有五个三角形（自己定）

  - 物体存在叶子结构里面，其他部分用来做加速结构的判断（也就是我们内存里面实际上只存包围盒和它的子节点的指针，实际物体只在叶子结点的时候存）




- ## **BVH实现步骤**

  - 找到一个包围盒

  - 将包围盒里面的物体分成两个部分（可以和KD树一样XYZ交替换着拆分，或者找最长轴折半）

  - 重新计算两个部分的包围盒

  - 递

  - 归——叶子结点足够少的时候即可




- ## SAH

  - 是BVH的加速结构，一种新的划分策略

  - 实际上它是个分割策略，是**BVH的加速结构**。当找到最佳的分割点后，需要再次递归调用 BVH 加速结构。

- ### 原理

  - SAH涉及对时间成本的估计，以面积来作为概率，对此进行概率与时间成本消耗（怎么感觉有点像是负面期望）	

- ### 涵盖内容

  - 将总体的bound的面积考虑进去了，考虑了单个图元（三角形）在某个bound的面积占比——面积比代认为为概率

- ### 开销成本

  - 对单个图元被遍历到进行单次时间成本的期望计算，对同个盒子里面的三角形进行了全图元期望累加，

- ### 表现

  - 表现为——图元密集的区域划分面积少（仍然是轴向划分），图元稀疏区域面积大。图元密集区的判定框自然是越小越好，反之越大越好。

- ### 逻辑与思考

  - 逻辑上讲，图元小的区域，被射线穿中的可能性本身就小。如果探测面积大，那自然意味着更可能射中少图元的bound，加上其本身就很小的性质，被光线打中的可能性就更小了，时间探测开销也小；如果区域图元密集，假如探测区域大，被射线打中盒子的可能性就大，如果射线打中了，那就得被迫对密集的所有图元都进行一次遍历，判定是否与射线相交。射线击中后需要检查更多的图元，这会增加探测的时间成本。图元密集区域自然是希望判定盒越小越好，省的那个麻烦全遍历一次。

- ### 总

  - 所以，SVH 的核心思想是在划分时对区域进行更细化的调整：对于包含更多图元的区域，划分得更加精细，以减少射线击中后遍历多余图元的时间开销；而对于图元较少的区域，可以适当增加划分的边界面积，以降低射线击中后进入进一步检测的概率，从而整体上优化了探测的效率。

- ### 计算方法

  - 有实验说在12个物体之前仍然保持对半分会比SAH快一点
  - 以**节点**为单位，进行盒子的开销计算。根据开销，进行盒子细分。
  - ![image-20241020002049788](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241020002049788.png)
  - *C(A, B) 光线与这个**节点**相交的成本*
  - *t_traversal 是光线与中间节点相交的成本*——**自定义，一般为2（测试得出）**
  - *p_A* and *p_B* 是光线通过包围盒A与包围盒B的概率
  - *N_A* and *N_B* 是包围盒A和包围盒B中物体数量
  - *t_intersect* 是对一个物体的光线相交计算成本——**一般为1**

  - ​	![image-20241020002209520](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241020002209520.png)



肉眼上可以看到变快，但是数据上看不出来

![image-20241020012951677](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241020012951677.png)



- ### 参考

  - https://zhuanlan.zhihu.com/p/477316706
  - https://blog.csdn.net/ycrsw/article/details/124331686





**如何拆分节点？**

- 总是找到最长的轴进行折半。——如何折半？
  - 找到最长轴的三角形的中位数，对物体数量进行折半：n/2个
  - 或者用三角形的重心坐标，在某个轴进行排序，找中位数（找中位数其实不需要排序，用[快速选择算法](#4. 快速排序（Quick Sort）)）

​							<u>看来有空要回去复习一下算法哩</u>



**带来的问题**

- 空间没有太划分开，盒子可能会相交——对于怎么划分？很有讲究。很多研究都在做这个事情





**存储存什么**

- **节点存储**
  - 包围盒
  
  - 子节点的指针
  
- **叶子结点存储**

  - 包围盒
  - 物体列表

- 节点

  - **节点表示场景中图元的一个子集**
    - **所有对象都在子树中**





- ## ==**算法的真正实现**==

和KD树没有太大的区别

- ![image-20241015161132359](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015161132359.png)



至此，BVH加速结构，以及BVH的加速结构SAH结束。



*光线与加速结构的求交已完成*

------



## Radiometry ——Motivation（辐射度量学）

提这个的动机

定义 了一系列的方法和单位去定义光照

物理上准确地定义光照的方法

而不会总是说光强光强，实际上光的能量不只有光强来定义。



**新定义：**

- **[radiant flux](#Flux(Power))**——光通量，单位时间内通过的光总量
- **[intensity](#Radiant Intensity)**——光强，单位立体角光通量，瓦特
- **[irradiance](#Irradiance)**——辉度，单位面积光通量，瓦特/平方米
- **[radiance](#Radiance)**——光亮度

![image-20241015194231546](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015194231546.png)





### Radiant Energy and Flux(Power)

**能量——焦耳**

传出来的东西肯定都是能量，现在想办法用能量去描述它





**Radiant Energy**

**能量<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015194438409.png" alt="image-20241015194438409" style="zoom:50%;" />**

**——焦耳**

![image-20241015193750638](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015193750638.png)







#### **Flux(Power)**

**功率**

单位时间内的能量——单位瓦特（lumen）

![image-20241015193759116](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015193759116.png)  



**Flux——单位时间内通过光谱的数量**







### Radiant Intensity

![image-20241015194453111](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015194453111.png)——能量



![image-20241015194649501](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015194649501.png)



- 单位强度（单位能量）
- 单位立体角（单位能量）
- 单位强度/单位能量，，，，单位（cd ； candela）



#### 立体角

弧度制的角度在空间中的延伸

- 平面内，求角度，弧度除以半径
- 立体空间，求立体角，**面积**除以半径的平方

![image-20241015195129093](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015195129093.png)



- ### 单位立体角

  - **==dw = dA/r^2^ = sinθ dθ d∅==**
  - ![image-20241015195607611](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015195607611.png)
  - 立体角是三维的，它是一个**面**的**角度**
  - 式子推导可得面积为dA——面积得是正对球心的 
  - 面积除以半径的平方——dw
  -  将整个圆的单位立体角积分起来那就是4π
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015195749973.png" alt="image-20241015195749973" style="zoom:50%;" />

- ### 微分立体角

  - θ 和 ∅ 两个角度进行很细微的变化——dθ   d∅
  - **水平**方向得用r sinθ 来进行**d∅**的累积
  - **竖直**方向用r 来进行**dθ**的累积
  - ![image-20241015205136712](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015205136712.png)
  - 从式子上看，它有个sinθ，说明这个立体角在球上不是均匀变换的











#### 回归问题

由立体角的计算可得

任何一个方向上得到的**==Intensity = Power / 4π==**

power是通过单位光强对于整个圆的面积的积分为**4πI**



![image-20241015200247069](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015200247069.png)





**Ray Tracing3**

**Light Transport & Global Illumination**

目录

![image-20241015203331660](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015203331660.png)

![image-20241015203407197](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015203407197.png)



**概念重述**

后面讲的大多能量都是**单位时间内的能量**（随着时间的积累物体颜色可能会发生变化）

![image-20241015203544703](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015203544703.png)



之前的复习：

![image-20241015203631773](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015203631773.png)





### Irradiance

Power per unit area

**每一个区域的对应的能量**（**得是光线和面得是垂直的，不然不是单位能量。**不是垂直就**投影到垂直**——所以之前的布林冯模型的diffuse term才要做cosθ）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015210018430.png" alt="image-20241015210018430" style="zoom:33%;" />——顺便还能解释太阳和地球的角度变化带来的四季变化

公式人话说就是：用能量微分除以面积微分

![image-20241015205443926](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015205443926.png)



- ### Irradiance Falloff

  - ![image-20241015210608490](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015210608490.png)
  - 球的面积：4πr^2^
  - Irradiance——单位面积的能量
  - 实际上Intensity（单位立体角的能量）是没有变的，其实是Irradiance再做衰减
  - 这也能够解释为什么越远，辐照度越小，而不总是说光强越小。实际上辐射度量学不单单是Intensity，这就是Irradiance在衰减。





### Radiance

拿来描述光线用的，在一条光线上的属性



- #### **定义**

  - **单位立体角**且**单位面积**上有多少辐射——两次微分
  - ![image-20241015211502812](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015211502812.png)
  - 人话逻辑就是有一个方向有面积个量的能量辐射出来，差不多就是描述光的了 （往下理解）

- ### 定义理解

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015212259300.png" alt="image-20241015212259300" style="zoom:50%;" />
  -  将前面的两个量联系起来，也就是：
  - **Radiance：[Irradiance](#Irradiance) per solid angle**
  - **Radiance：[Intensity](#Radiant Intensity) per projected unity area**





- **Irradiance vs. Radiance**——区别与关系
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015214141174.png" alt="image-20241015214141174" style="zoom:50%;" />
  - 从对单位面积求立体角的微分，求出Intensity（用了cos计算衰减值）
  - 然后将此值进行面积上（H^2^：表示一个**单位<u>半球</u>**，覆盖从所有方向上来的光）的微分，得到的就是**Radiance**



**Radiance就是把四面八方（Intensity，能量在立体角上的积累）的Irradiance（能量在面积上的积累）给积分起来。**



**Irradiance 和 Radiance 之间差的就是方向性**



如何确定方向性？↓



## Bidirectional Reflectance Distribution Function（BRDF）——双向反射分布函数

  我们的理解角度是：我们有一个光线打到物体表面上的点，然后做一个能量吸收，然后再按照某一种规律反射出去。



**==决定了不同的材质对于光的不同反应==**



我们需要一种方式去定义这种——对于射过来的光怎么**吸收+反射**出去的**对应法则**，定义了如何去分配这个入射和反射的规律，那也就自然包含了：

- 入射角度
- 出射角度
- shading point

![image-20241015215926870](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015215926870.png)

![image-20241015220157570](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015220157570.png)



- 我的理解：
  - 我们想要对方向的反映
  - 我们求得反射出去的Radiance去除以per area的Irradiance，得到的就是per angle的“Radiance”，也就是特定angle的Intensity。



来看来看，重点来了↓





- 反射在固定方向（View pos）上的光照累积
- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015221431915.png" alt="image-20241015221431915" style="zoom:50%;" />
- 这个就是**光线传播着色模型**
- **解读**
  - 假设反射对应法则已知<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015221506013.png" alt="image-20241015221506013" style="zoom:50%;" />，那我们从定点的光照强度对于该方向上的贡献那就可求为<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015221543095.png" alt="image-20241015221543095" style="zoom:80%;" />，每一条光线对应的Radiance在此方向上的贡献。
  - 对于所有可视平面（有效光路，光可见区域）对于整个罩面上进行每条对于固定方向有贡献的光照的Radiance进行罩面（球面）完全累积，这整个**光线传播着色模型**就是固定方向上能接受到的光的Radiance
  - 关键词拆解——
    - 固定点入射罩面的Radiance
    - 光在垂直方向上的衰减
    - 到达固定点的Radiance
    - 反射到固定方向（BRDF）
    - 单次光线在视角方向的贡献值
    - 贡献量在所有有效光路的累积





这个是反射方程，咱还有个渲染方程



但还有个问题





- **这个反射方程（BRDF）传递一种讯息：**
  - ![image-20241015222512976](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015222512976.png)这个传入过来的光可不单单只是光源传递过来的光，还有可能是其他物体打过来的光（涉及全局光)——递归的定义方式
  - 出射的Radiance都有可能成为其他点的入射的Radiance





从反射方程推出更加通用的**渲染方程**





### 渲染方程

如果物体自己会发光L~e~

咱额外给他加上（我感觉这个“自发光“如果对自己而言可以是反射出去的光，对其他物体而言都是另类的光源）

KK！



#### **==渲染方程（Rendering Equation——Kajiya）：==**

![image-20241015222840492](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015222840492.png)

**单这一个方程即可描述所有的反射方程**







- **相关注解**

  - 光照方向仍然默认从点指向光源

  - **Ω^+^表示半球**——积分一般，负方向贡献为0，不计入。之前是max(0.0,cosθ)





怎么解方程？下节课说——会用到概率论来解



### 理解渲染方程

![image-20241015223714416](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015223714416.png)

- 多个灯就都加起来，如果是面光源就对面光源的面积进行积分





- 其他物体传递过来的间接光，那就当做是光源一样对待（方向变一下）

![image-20241015223809331](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015223809331.png)





- 唯独不知道各个点反射出去的Radiance是多少
- 简写渲染方程

![image-20241015223950416](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015223950416.png)



- 再更简写
- **==L = E + KL==**
  - 接收到的能量 = 自发光的能量 + 辐射到的能量反射过来的能量
- ![image-20241015224542537](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015224542537.png)

- 写成这样是因为需要解渲染方程
- 近似推导过程
- 涉及矩阵；泰勒展开式
- ![image-20241015224746482](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015224746482.png)

- 展出来的式子很有意义：
- ![image-20241015224840933](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015224840933.png)
  - K的幂就表示弹射次数
  - 然后就可以引出**全局光照**的概念——全局光照是**直接光照与间接光照的集合**
  - **另一个角度**看，**光栅化**可以做什么。光栅化很难做出间接光照的部分（E+KE），所以要引出光线传输的概念
  - 弹射次数（x-bounce global illumination）越多，会收敛于一个值（能量守恒 ）
  - 曝光（摄像机快门按得越久），是因为单位时间接受能量变成了长时间接受能量，能量会累积

 

### 概率论回顾（Probability Review）

​										*<u>我学的概率论终于知道用在哪里了，不止是滤波！</u>*

**为什么要用概率？**（可能是要判断返回到哪个方向的可能性吧）



很简单的随机变量……



随机变量到概率



不同的概率去取不同的值



- **随机变量和概率之间要满足什么属性**
  - 概率得是非负的
  - 概率总和为1

 

#### 期望

不断取随机变量然后取一个平均

- **期望计算方式：**
  - 值乘概率然后全部加起来

![image-20241015230523352](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015230523352.png)







这个随机变量还是离散的，我们要引入连续的





![image-20241015230908772](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015230908772.png)

正态分布？

这个时候所说的概率不是y轴，而是某个区域的面积才是概率

上面的曲线叫做概率**密度函数（PDF——OProbability Distribution Function）**——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015231020799.png" alt="image-20241015231020799" style="zoom:33%;" />这个取值是符合概率密度的。

后面的蒙特卡洛积分用



- **满足属性**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015231117160.png" alt="image-20241015231117160" style="zoom:67%;" />
  - 下面部分所有的值进行积分的数等于1（原本是求面积的为概率）
  - 期望**E[X]**计算方式：对于值 * 微分面积
- 如果求得是X的函数的期望怎么求？
  - 题意：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015231359260.png" alt="image-20241015231359260" style="zoom:50%;" />
  - 求期望不就是值乘概率然后所有加起来吗，那这个也照做
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241015231434392.png" alt="image-20241015231434392" style="zoom:50%;" />

后面就要用概率的方法去解渲染方程了





## Monte Carlo Path Tracing（蒙特卡洛路径追踪）

  

### 蒙特卡洛积分

面对写不出解析式的式子（分布函数那里）难以积分，解决积分问题

![image-20241018144257603](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018144257603.png)







#### 做法

我们实际上只是要那个定积分的数而已

 

**在f(x)上取一个点，然后连着a到b变成一个矩形，计算矩形的面积，近似为不定积分的面积。**

**以此法采样数点，后取平均，即为近似不定积分的面积**



“a连着b变成一个矩形”，因为矩形所有面积始终为1，所以他的**p（x）**就等于**1 / （b-a）**

——**平均采样**



**p（x）**——如何对积分域进行采样

- 得先知道积分域是谁
- 定义采样方式



我们需要这个**p（x）**，它在**均匀分布的函数**中，是一个**常数**

在**重要性采样**时，它可以采目标函数**变化较大**的地方

- 例如，假设 f(x)在一个区间有显著的峰值，我们可以选择 p(x)为一个与该峰值位置相符的概率分布，例如高斯分布：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018153941493.png" alt="image-20241018153941493" style="zoom: 67%;" />





然后**f（X~i~）**就是正常通过pdf采样出来的纵坐标的值

![image-20241018150409585](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018150409585.png)

​    

**好处：**

我们只需要以一种相同形式的积分域进行采样即可



- ### 特征

  - N越大，采样率越大，结果越接近
  -  **在哪一个点采样，就得在哪个点积分。**在X上采样，就得在X上积分





### Path Tracing（路径追踪）

- 和**Ray Tracing**有点区别
  - Whitted-style ray tracing——总是表现出镜面反射与折射
    - 停止于漫反射
    
  - **Ray Tracing（光线追踪）**：它模拟光线从摄像机（观测者）出发，通过场景中的物体进行一次或多次反射、折射，直到遇到光源。这种方法能很好地模拟直接照明和镜面反射，但它没有充分考虑间接照明和全局光效应。
    
    **Path Tracing（路径追踪）**：它是光线追踪的改进版，也从摄像机出发，但它模拟光线在场景中随机漫游，包括与物体表面的多次反射、折射和散射。它采用蒙特卡洛方法估计光线的路径，可以更好地模拟全局光效应，生成更真实的光照效果，包括**全局光照（GI）** 和 **漫反射反射**。



- **Reasonable**？
  - 改进Whitted-style，剔除一些不正确的算法
  - 问题①镜面反射的形式有的是不一样的
  - 问题②没有考虑全局光照，到漫反射就停掉了（认为漫反射是完全吸收掉了）
  - ——**渲染方程√**
    - ![image-20241018160908769](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018160908769.png)



- ### 解渲染方程

  - #### 蒙特卡洛积分

    - f（x） = <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018160928589.png" alt="image-20241018160928589" style="zoom:67%;" />
    - ![image-20241018161109910](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018161109910.png)
    - 积分表示为：
    - ![image-20241018161134424](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018161134424.png)

  - ### 伪代码（直接光照）

    - ![image-20241018161626225](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018161626225.png)

  - **间接光照**

    - 把反射源（已知它的直接光照Radiance）当做光源来计算另外一个点（我看到的Shading point）的光照累积

    - 伪代码：

      - ![image-20241018162004056](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018162004056.png)

      - P点是shading point ， q点是光源照射到的点。
      - 替换**L_i**部分，然后视为光源（可计算它反射过来的光是多少，与光源同等效应）——递！
      - p入射光线视为q的出射光线，方向翻转



但事情还没解决



- ### 问题

  - 递归过来的光线如果是连续的，数量会特别特别多；如果一个点射出特别多条光线，那就会指数型爆炸。
  - ![image-20241018162641717](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018162641717.png)
  - 只有N = 1的时候才不会爆炸式增长



- ### 此时伪代码

  - ![image-20241018162941616](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018162941616.png)


  - 此时，**N = 1**的时候，为**path tracing**，否则是**Distributed Ray Tracing（分布式射线追踪）**
  - N = 1 会带来很多的Noise

- 修改后对于单个像素的多个pass，**解决单次N造成的多噪声问题**

  - ![image-20241018163520608](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018163520608.png)

- **解决递归的归（stop）**

  - 方法一：**限制弹射递归的深度**，也就是限制光线的弹射次数，这个方法意味着能量的削减，能量就不守恒了。
  - 方法二：**Russian Roulette（RR）俄罗斯轮盘赌**——达到一定条件下停下来不追踪了
  - RR的方法只是为了让递归停下来，而不是光线无线传递
  - 期望值E（Lo） = Lo，保证期望值不变，我们引入一个自定义概率P（0<P<1）
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018165519094.png" alt="image-20241018165519094" style="zoom:33%;" />，一定概率情况下不发射，一定概率情况下发射，概率P仍然可以自定义，计算得到的期望值**E(Lo)**仍然不变
  - **俄罗斯轮盘赌伪代码——**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018165707199.png" alt="image-20241018165707199" style="zoom:67%;" />
  



已正确处理**Path Tracing**！！！



但如果一个像素打出的光线**（samples per pixel）**很少的话

![image-20241018170012930](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018170012930.png)



- ### 定位问题

  - 如果光线很小，那就有很多光线打不到光源，那就会有很多根光线**浪费**掉了——修改pdf采样方法，只要效果好即可
  - 如果在光源上采样，蒙特卡洛积分就不能用了。
  - 首先要明确一点，我们为什么能改变采样方法？因为：蒙特卡洛方法并没有规定pdf的选取，我们可以选择任意一个合适的pdf进行采样！以此来大大减少光线的浪费，此时**采样对象就从半球立体角dw转换到了光源表面的微面元dA**：![image-20241020171334772](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241020171334772.png)



- ### 解决光线浪费的问题

  - 积分全发生在光源上就不会浪费了，对光源进行均匀的pdf采样：**p(A) = 1 / A**
  - 在物理学中，根据**逆平方定律**，光的强度随距离的增大而衰减，且这种衰减是**与距离的平方成反比**的。
  - 因为蒙特卡洛积分只能对需要积分的点才有效，因此需要构建一个关系式，将光源映射到积分点上，即
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018173402978.png" alt="image-20241018173402978" style="zoom:50%;" />，对应模型<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018173415261.png" alt="image-20241018173415261" style="zoom:33%;" />
  - 根本方式就是改变积分域，将原先点上的积分改成光源的积分，通过光源映射回shading point上
    - ![image-20241018173558995](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018173558995.png)



- ### ==解决方案——拆解为直接光照与间接光照==

  - 加上下面提到的“小问题”即可作为**解决方案**
  - 光照贡献（**直接光**，不需要俄罗斯轮盘赌，改变积分域进行映射回来，解决光线浪费）
  - 物体反射贡献（**间接光**，俄罗斯轮盘赌（RR）解决）
  - 伪代码：
  - ![image-20241018173957897](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018173957897.png)



- ### 还是有个小问题

  - 仍需对光源到shading point之间是否有**物体遮挡**
  - ![image-20241018174344958](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018174344958.png)



- ### 遗留问题

  - 点光源呢？
  - 很难，不好处理，当做很小的面积光源处理先



- #### 照片级真实感

  - ![image-20241018174710064](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241018174710064.png)

  - [Cornell Box Comparison](https://www.graphics.cornell.edu/online/box/compare.html)



# Lecture 17 Materials and Appearances

**==Material == BRDF==**

**R——反射（BRDF）**

**T——折射（BTDF）**

**S散射，包括了R和T（BSDF）**

**BSDF = BRDF + BSDF**







- ### 漫反射的反射方式如此↓

![image-20241021171001036](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021171001036.png)

- 此时的 BRDF的范围在**[0,1/π]**
- 额外定义一个系数**ρ**，命名为albedo（如果是三个通道，那就是color）



- ### Glossy 材质（BRDF）
  - 经过打磨之后的金属，表面略微粗糙却仍然能够反射<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021171553327.png" alt="image-20241021171553327" style="zoom:50%;" />
- Ideal reflective / refractive 材质（BSDF*）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021171703291.png" alt="image-20241021171703291" style="zoom: 25%;" />
  - 有反射有折射 
    - 如果折射没被吸收——很透的玻璃
    - 如果折射被部分吸收——有点颜色的玻璃<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021171909462.png" alt="image-20241021171909462" style="zoom:33%;" />
    - 如果折射被完全吸收——黑的（金属完全吸收）

  

- ### 完美的镜面反射

  - 反射的计算，之前看的[反射的推导](https://www.cnblogs.com/graphics/archive/2013/02/21/2920627.html)

    ```C++
    Vector3f reflect(const Vector3f &I, const Vector3f &N) const
        {
            return I - 2 * dotProduct(I, N) * N;
        }
    ```

  - 根据老师给的：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021173108086.png" alt="image-20241021173108086" style="zoom:50%;" />

    - 我自己推导了一遍<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021173141886.png" alt="image-20241021173141886" style="zoom:50%;" />
    - 用的就是第一个的**平行四边形法则**

  - 第二个方法用到了方位角，从头顶看下去，然后实际上两个φ的差别只是相差一个π而已，将它加上去再取模，有方位角φ有θ然后算他的反射（不知道怎么从φ变成的，还有这个mod是什么？用φ和θ怎么求的反射向量？）



- ### 折射

  - 对于入IOR盒出IOR定义两个不同的折射率，使其满足：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021174729910.png" alt="image-20241021174729910" style="zoom:50%;" />

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021174815792.png" alt="image-20241021174815792" style="zoom: 80%;" />

  - 从方位角上来看，仍然满足相差一个π的关系

  - 相关折射表![image-20241021174856701](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021174856701.png)

  - ### 表现

    - 折射率越高，折射不同的波长的程度就越强，看上去就越闪闪发光



- ### 折射角的计算

  - 我们要计算它的余弦
  - ![image-20241021175023195](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021175023195.png)

  - 这个角度在某些情况下是没有意义的——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021175243175.png" alt="image-20241021175243175" style="zoom:50%;" />，也就是说在这种情况下是不会发生折射的。
  - 进一步推导，也就是<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021175308466.png" alt="image-20241021175308466" style="zoom:67%;" />。
  - 在此背后说表现的是：**==如果入射介质折射率大于出射介质折射率，则不发生折射==**。入射介质更密，出射介质系数稀疏
  - 例如——全反射现象的玻璃
  - （R是反射T是折射）



- ### 从高IOR到低IOR看过去

  - Snell‘s Window/Circle
  - ![image-20241021210446760](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021210446760.png)
  - 在最大97.2°三角锥之外会被直接反射到池子地下，称为全反射。因此我们在对玻璃进行折射计算的时候，需要考虑到全反射现象

- ### Fresnel Reflection（菲尼尔项） / Term

  - **反射强度随着掠射角的增大而增大**
  - **导体**的菲涅尔现象
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021211314992.png" alt="image-20241021211314992" style="zoom:50%;" />
  - **绝缘体**（镜子）的菲涅尔现象
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021211634793.png" alt="image-20241021211634793" style="zoom:50%;" />
    - 在各个角度的反射程度都几乎相同
  - 完全正确计算方式——对两个级数取平均
    - ![image-20241021211954642](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021211954642.png)
  - **Schlick’s 近似公式**
    - ![image-20241021212023100](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021212023100.png)



引出↓

- ### **微表面材质（Microfacet Material）**

  - **对于一个物体，远处看是材质，近看（特别微小的）是几何。**

  - 根据微表面的法线对光照进行分布

    - ![image-20241021212928555](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021212928555.png)

    - 根据微面元理论，推出微面元模型的计算公式：

    - ![image-20241021213404860](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021213404860.png)

      - **F(i,h)**——计算总共有多少能量会被反射
      - **G(i,o,h)——**修正阴影自遮挡，以及修正在掠射角异常发亮。
      - **D(h)**——决定法线的如何分布（主要决定如何反射，集中亦或是发散）

      - **==PBR都在用！==**

- ### **第二类材质——各向异性（Anisotropic）**

  - 根据微表面的方向性进行判断。各向异性的微表面具有明确的方向性
  - 各向同性，体现在不同的方位角的BRDF是相同的，相对不变既是相同的；
  - 各向异性，体现在不同的方位角的BRDF是不同的，考虑的是绝对的相同。
  - ![image-20241021214657440](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021214657440.png)



- ### BRDF性质

  - **非负性：**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021220117398.png" alt="image-20241021220117398" style="zoom:50%;" />，能量分布肯定是正的
  - **线性性质：**可以通过累加获得结果，跟各部分算结果然后累加是一样的。就像这个图里面好多个几何体都可以加起来一眼。![image-20241021220220898](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021220220898.png)

  - **可逆性：**出射光与入射光的方向可互换，中间不涉及转换<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021220356233.png" alt="image-20241021220356233" style="zoom: 33%;" />

  - **能量守恒**：能量只会不变或者越来越小，总体肯定不会超过原始输入能量<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021220514844.png" alt="image-20241021220514844" style="zoom:50%;" />

  - **各向同性：**

    - 实际上是**三维**的，只包含入射角度，出射角度以及两者的**相对角度**——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021220800287.png" alt="image-20241021220800287" style="zoom:33%;" />

    - 在测量与储存的时候用得上



- ### BRDF的测量

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021220929783.png" alt="image-20241021220929783" style="zoom:50%;" />
  - 所有的shading point都测一遍，然后枚举记录
  - “curse of dimensionality”——维度一上升，数据量就会多很多
  - 4D到3D，计算量骤减
  - 因为可逆性，所以只测一半即可

- ### BRDF的存储

  - 神经网络压缩
  - 测入射出射角，相对方位角的绝对值即可<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241021221340698.png" alt="image-20241021221340698" style="zoom:50%;" />
    - 将数值都表现出来然后可以存储到一个三维库里面







# Lecture 18 Advanced Topics in Rendering



​	*这节课的东西很多也很散，是从广度优先的。更多的只是介绍，但我认为这些前沿的知识恰巧是我目前用得上的，因此我仍然需要着重去记忆这里提到的东西。这些东西往往我之后可能会见到*





有好多个材质介质，记录下来，是个统合



## Advanced Light Transport（高级光线传播）

- **Unbiased light transport methods（无偏的光线传播）**
  - Bidirectional path tracing (BDPT)——双向路径追踪
  - Metropolis light transport (MLT)——<u>？大都市？</u>光线传播
- **Biased light transport methods（有偏的光线传播）**
  - Photon mapping——光子映射
  - Vertex connection and merging (VCM)——结合光子映射和双向路径追踪
- **Instant radiosity (VPL / many light methods)——试试辐射度**
  - 间接光表示为很多小光源





- ### Unbiased light transport methods（无偏的光线传播）

  - 结果得到的永远是我们希望得到的真实值，影视级别的物理绝对真实性——蒙特卡洛解定积分

- ### **Biased light transport methods（有偏的光线传播）**

  - 得到的值和期望值有偏差，但有个特殊性质——**一致性（consistent）**
    - **一致性：**当某种样本数量无限的情况下，得到的结果最终会收敛到物理绝对正确的值上。





### 无偏估计

#### Bidirectional path tracing (BDPT)

​		**双向路径追踪**



- 之前一直是依据光路可逆性从摄像机发出光线
- 双向就是拓展成从光源也射出一条路径——subpass（子路径），然后将两个路径连接起来
- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022143654973.png" alt="image-20241022143654973" style="zoom:50%;" />

- 实现起来**很困难**，而且比较慢，而且会慢很多

- 实现难度——**强光很容易被打中，光源第一跳有很多的diffuse**



#### Metropolis light transport (MLT)

​	

- A Markov Chain Monte Carlo (MCMC) application
  - 马尔可夫链帮助采样
    - 马尔可夫链：根据当前样本，根据一个概率分布，生成下一个相近的样本
  - Jumping from the current sample to the next with some PDF
- 可以做到以任意函数为pdf生成样本
- Key idea: Locally perturb an existing path to get a new path 有一个路径的情况下，可以生成相似的路径
- 好处：Very good at locally exploring difficult light paths 有了种子，就能找到更多相似的
  - 越难的场景做的越好
  - Caustics, Indirect Light Source（几乎全部物体都是简介光源打亮的）
- 坏处：
  - Difficult to estimate the convergence rate——很难去分析收敛的速度
    - Monte Carlo可以估计Variance，可以量化
  - Does not guarantee equal convergence rate per pixel——每个像素的收敛速率不一样
  - So, usually produces “dirty” results 看上去比较脏
  - Therefore, usually not used to render animations——经常表现为画面可能是不一样的，所以不会用来做动画



### 有偏估计

#### Photon Mapping（光子映射）

- 很适合拿来做**SDS（Specular-Diffuse-Specular）**的路径追踪生成，比如焦散
- A biased approach & A two-stage method



- ### 实现方法（其一）

  - Photon Mapping — Approach (variations apply)

    1. ###### Stage 1 — photon tracing

       - 光源出发，发散很多光子。当光子打到漫反射物体之后就停在那里，标记光子。

    2. ###### Stage 2 — photon collection (final gathering)

       - 从摄像机打出光线到周围物体上，到漫反射时停下来

    3. ###### Calculation — local density estimation 局部光子密度估计

       - 一种思想：光子越多的区域就越亮
       - 可以快速得到一个shading point周围的**N个光子**(通过树状结构实现算法，N是固定的，用**光子个数来决定面积**). 然后计算着N个光子所占据的面积（球）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022160741026.png" alt="image-20241022160741026" style="zoom:25%;" />，面积计算，然后**光子密度=光子数量/面积**
       - 光子数量少：面积大，噪声大；光子数量大：模糊。主要是——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022160904877.png" alt="image-20241022160904877" style="zoom:33%;" />，对于密度的估计是不正确的估计。↓

    4. ###### 模糊是因为有偏

       - Local Density estimation dN / dA != ΔN / ΔA **光子密度估计在数量趋向无限时才与真正光子密度相等**，所以biased but **==consistent!==**——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022160938695.png" alt="image-20241022160938695" style="zoom:33%;" />。 只要打出的光子不是趋于无线的，那它就是有偏的，那多少都会有一点糊。
       - 我们的面积是根据光子的数量来圈定面积的，也就是说这个是可变的。当光子的数量足够多时，圈定的面积也就越足够小，也就越来越接近于dA。



- 有偏和无偏的实用辨别方法：
  - 在渲染中：Biased == blurry——有模糊就有偏
  - 一致的Consistent == not blurry with infinite #samples——只要样本足够多那就可以收敛到一致的



#### Vertex Connection and Merging (VCM)

- 想法很简单：

  - A combination of BDPT and Photon Mapping
  - Key idea:  Let’s not waste the sub-paths in BDPT if their end points cannot be connected but can be merged, but Use photon mapping to handle the merging of nearby “photons”
  - ![image-20241022162040747](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022162040747.png)

  - 比如，$x_2$和$x^{*}_{2}$ 在同一个面上，但是没有重合，按照BDPT，这种就是浪费
  - 但是VCM决定利用这种情况，把其中一半光路转化成光子，进行Photon Mapping一样的计算





#### Instant Radiosity (IR)

- aka many-light approaches 很多光源的方法
- Key idea: Lit surfaces can be treated as light sources 被照亮的表面就像是光源。可以再次照亮其他物体
- 模拟从光源发出光线，打到的地方相当于二级光源。如果此时Sample某个场景点的颜色，那么遍历这些二级光源，叠加计算即可
- Shoot light sub-paths and assume the end point of each sub-path is a **Virtual Point Light (VPL)**, Then Render the scene as usual using these **VPLs**
- Pros: 很快，而且在漫反射的场景中结果很不错
- Cons:
  - 在两个很接近的点上会出现曝白 
  - 不能用于磨砂材质
  - 计算Intensity，在除光照衰减的时候有个除两点之间的距离，两个点挨得很近的时候值会特别大



工业界：**Path Tracing，不高端，但可靠**



## Advanced Appearance Modeling

​			**==Appearance → Material → BRDF==**

- **Non-surface models——和表面无关的模型**

  - Participating media——**散射介质**
  - Hair / fur / fiber (BCSDF)——**毛发，纤维介质**
  - Granular material——**颗粒介质**

- **Surface models**——和表面有关的模型

  - Translucent material (BSSRDF)——半透明材质

  - Cloth——衣服布料

  - Detailed material (non-statistical BRDF)——复杂，有细节度的模型

Procedural appearance——程序化生成





### **Non-surface models——和表面无关的模型**

#### Participating media——散射介质



更多的定义在**空间中**的介质，而不是表面上的介质，比如烟雾，体积云等



- 在一个介质点上，点的表现力和物体表面的表现能力类似。能够达到：**能量损耗，自发光，光线能量散射以及吸收光线**等行为。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022163638607.png" alt="image-20241022163638607" style="zoom:50%;" />

  - **怎么进行散射？**

    - 可以往各个不同的方向散射。（倾向于光的方向散射是不是可以说是光线步进？）
    - **==Phase Function==**（相位函数）来定义，自规定散射方式
    - ![image-20241022163936839](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022163936839.png)

  - ### **==Participating media散射介质渲染方法==**

    - 随机选择（自定义）一个方向打出光线（eyes）
    - 随机选择（自定义）走前进的距离
    - 在每一个shading point上，连接光线，然后计算整个路径的（每个sp的）贡献值

- 对于光线可能会进到物体里面的材质，都可能是散射材质。散射介质也有可能是巧克力酱、手指头（



#### Hair Appearance

头发的介质



![img](https://raw.githubusercontent.com/nininias/image-repo/main/img/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F4a4bd1ad-1b13-404a-a728-429c59cf0400%252FUntitled.png)

- 表面上看，有两种高光：无色&有色高光
  - 无色高光
  - 有色高光



- ### Kajiya-Kay Model——KK模型

  - 类似于基础模型
  - 光线打到圆柱上，散射除一个圆锥型，同时向四面八方散射。好像就单纯是把diffuse和specular加起来了一样。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022164949179.png" alt="image-20241022164949179" style="zoom:33%;" />
  - 效果<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022165133925.png" alt="image-20241022165133925" style="zoom:33%;" />





- ### Marschner Model

  - 广泛应用的模型，效果在不是单纯考虑blinn-phong
    - R——Reflection
    - T——Transmission
  - 考虑了三种情况（忽略了髓质的影响，单层结构，头发当成了玻璃圆柱）：
    - R——一部分直接圆柱体反射
    - TT——一部分折射进去再折射出来（中间空心的）
    - TRT——一部分折射进去，反射一次，然后折射出去
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022165515836.png" alt="image-20241022165515836" style="zoom:50%;" />
  - 思考的模型：
    - Glass-like cylinder
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022165846587.png" alt="image-20241022165846587" style="zoom:33%;" />
    - **皮质部分（cortex）**：有色素，会吸收部分能量
    - **角质层（cuticle）**：就是外层，穿透层
  - 效果：![image-20241022170030278](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022170030278.png)
  - 已经很不错啦，但我们只和1跟光线是这么说，如果是那么多那么多的头发……
  - 多次散射



#### Fur Appearance——毛发材质表现

我们用人的头发的模型不足以描述生物的毛发结构，需要重新研究生物的毛发结构



- 人类的头发和动物毛发的区别：
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022170510117.png" alt="image-20241022170510117" style="zoom:67%;" />
  - **髓质(Medulla)**——会散射能量。人类的髓质是比较小而集中的，动物的髓质有大又疏松
  - **皮质部分（cortex）**——有色素，会吸收部分能量
  - **角质层（cuticle）**——就是外层，穿透层
  - 图片右上角是人类的头发髓质
  - 图片右下角是动物的毛发髓质
  - 换句美术可能听得懂的话来说：人类头发表现的会闷黑一点（黑发），因为黑色素的吸收能力强，髓质散射能力弱，所以不透气。而动物的毛发的髓质又大又疏松，散射能力强，吸收能力较弱一点，所以表现出来的更加蓬松，颜色能够透的出去，呼吸感强。而且研究说明确实如此。
  - 加入了髓质的影响之后：人类的头发也更加透气一点（散射能力更强）
    - ![image-20241022171119035](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022171119035.png)



- ### Double Cylinder Model——双层圆柱体模型

  - 有点不懂“双层”是不是指的是双层的cuticle，但讲的好像关注在Medulla（麦当劳）而不是cuticle上
  - 从原本的**R,TT,TRT三个**考虑因素增加到**5个**，也就是增加了对穿过Medulla的影响
  - **==R+TT+TRT+TT^s^+TRT^s^==**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022171631615.png" alt="image-20241022171631615" style="zoom:50%;" />
  - 三个叠加的效果如下，我感觉已经特别好了，Yan双层圆柱形模型！
  - ![image-20241022171715690](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022171715690.png)



#### Granular Material——颗粒材质

![image-20241022172135286](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022172135286.png)

估计也只能从微观和宏观角度下手。

但可以**通过程序化建模**进行隐式**定义**！



### Surface Models——表面模型



- ### Subsurface Scattering——次表面反射

  - 玉石一类材质——通光材质的代表（Translucent Material）；半透明——semitransparent

  - 次表面散射作为BRDF的一个理论延伸，认为光线不会直接在表面进行反射，而是认为光线会在物体内部进行多次的散射，进而在某个点再反射出去。
  - 场景——胶体，丁达尔效应，较为通透的物体等<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022172827800.png" alt="image-20241022172827800" style="zoom:33%;" />

  - **==BSSRDF==**（中间加了个Subsurface Scattering）计算

    - ![image-20241022173139248](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022173139248.png)
    - ![image-20241022173229470](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022173229470.png)
      - 从哪个点进来
      - 从哪个方向进来
      - 从那个点出去
      - 从哪个方向出去
    - **不单单考虑方向，还需要考虑面积**，即从哪里反射出去也要进行考虑了

  - ### **Dipole Approximation近似方法**

    - 两个光源互相打，打出来的感觉就很像次表面散射打出来的结果
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022173611228.png" alt="image-20241022173611228" style="zoom:50%;" />
    - 效果不错

  - ### 应用场景

    - 通透的皮肤<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022173720448.png" alt="image-20241022173720448" style="zoom: 25%;" />
    - 玉石



- ### Cloth——针织与衣物

  - **Fibers（纤维）-Ply（股）-Yarn（线）**

  - 布：A collection of twisted fibers! 由纤维缠绕而成

    - 每一根纱线是由纤维缠绕而成
    - 每一股毛线是由纱线缠绕而成
    - 规模很恐怖

    **Render as Surface**

    - Given the weaving pattern, calculate the overall behavior
    - Render using a BRDF，根据不同的织法，给不同的BRDF
    - 但是布料并不仅仅是表面，天鹅绒那样的效果无法展现

    **Render as Participating Media**

    - Properties of individual fibers & their distribution -> scattering parameters
    - 空间中分布的体积→细小的格子，对每个格子里布料的性质进行采样，转化成对光线的渲染 （像对云的渲染那样）计算量很恐怖

    **Render as Actual Fibers**

    - Render every fiber explicitly!
    - 计算量更恐怖
    - 按照每一个纤维来渲染……**Fibers（纤维）-Ply（股）-Yarn（线）**



- ### Detailed Appearance——细节的表现

  - Not looking realistic, 因为太过完美，真实情况下，有很多复杂，不完美的东西。搞点脏脏搞点划痕。

​	

- ### 	 **Microfacet BRDF**

  - 微面元理论的BRDF模型。，最重要的是它的法线分布。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022175343549.png" alt="image-20241022175343549" style="zoom:50%;" />

  - 计算：**Surface = Specular microfacets + statistical normals**
    - 法线分布(NDF)用了正态分布，其实没有那么完美
    - 把法线分布改的复杂一点，就会获得更复杂的结果
    - 法线贴图200k x 200k，获得很好的效果，但是运算量爆炸（甚至一张图要一个月）
    - 这是因为在法线分布复杂的情况下，很难建立valid的光线通路（光源→表面→摄像机）
  - 解决办法：
    - **BRDF over a pixel**
    - 用一个像素覆盖多个微面元，然后计算像素覆盖的微面元的表面法线分布，然后用到模型里面
  - 一个像素对应了一块表面，把整块范围内的法线分布整合起来获得（P-NDF），以此简化计算
    - 小范围的P-NDF会有很多奇妙的样子
  - Application:
    - Detailed / Glinty Material
    - 海绵波光粼粼的效果

  

  - ### Recent Trend: Wave Optics

    - 之前所有的计算都把光线当作粒子来计算，但光具有波粒二象性，有很多只有在把光当作波的时候才会有的性质就会被忽略。
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022200134015.png" alt="image-20241022200134015" style="zoom:50%;" />
    - 然后就要考虑波动光学
      - 结果与几何光学类似，但不连续（干涉导致），不同波长也不一样，老师也不想提
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022200255395.png" alt="image-20241022200255395" style="zoom:50%;" />
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022200352944.png" alt="image-20241022200352944" style="zoom:33%;" />——波动光学





- ### Procedural Appearance 程序化生成材质

  - **==Procedural==**——随用随取，需要用的时候直接去找。并非真实建模。看到什么就能表现出什么，看不到的处于一种“待定”状态，等某种情况，比如切开表面看内部，就能观察到之前按照程序的逻辑拟写好的模样。
  - Explicitly or Implicitly 不一定需要真的生成，可以查询

问题：三维模型，三维材质，存储量爆炸

Can we define details without textures?

- Yes! Compute a noise function on the fly.
- 3D noise -> internal structure if cut or broken
- Thresholding (noise -> binary noise)

Complex noise functions can be very powerful.

- Perlin Noise
  - 生成地形
  - 木头的三维生成
- ...

Houdini: 程序化生成材质(Explicitly)



# Lecture 19 Cameras，Lanses and Light Fields

 前话

成像 = 合成+捕捉

Imaging = Synthesis + Capture

- Synthesis: raster | ray tracing 用**合成**的方法来成像
- Capture: **捕捉**真实世界来成像
  - 最典型：相机
  - Transient Imaging: 研究光在极短时间内的传播
  - Computational Photography: 计算成像学





- #### 相机的原理

  - 小孔成像<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022203019291.png" alt="image-20241022203019291" style="zoom:25%;" />
    - 针孔相机 Pinhole Camera
      - 无虚化
      - 全部都是清晰的
  - **Lens 透镜成像**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022203025701.png" alt="image-20241022203025701" style="zoom:25%;" />



- ### 相机的组成

  - Shutter 快门，控制光进入相机
  - Sensor 传感器（下面说的是胶片），——传感器上任意一个点，记录的是Irradiance（单位角度不同的面积能量） 



- Why Not Sensors Without Lenses?

  - 任何一个点都可能收集到不同方向过来的光
  - 传感器记录的是Irradiance

  针孔相机 Pinhole Camera——任何一个地方都是锐利的，看不到虚化的地方





### Field of View (FOV) 视场



![image-20241022204000521](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022204000521.png)

- **h:** 传感器高度
- **f:** 焦距，传感器与透镜的距离

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022204103919.png" alt="image-20241022204103919" style="zoom:33%;" />

- 通常以35mm(36 x 24mm)格式胶片为基准，通过定义焦距的方式来定义FOV（说的这些都是说转换到35mm格式的胶片来说的）：
  - 17mm is wide angle 104° 广角镜头
  - 50mm is a “normal” lens 47°
  - 200mm is telephoto lens 12°

- 单一变量：
  - 同样大小的传感器，焦距越大，视场越窄
  - 同样焦距，传感器越大，视场越宽
  - **同样视场，焦距和传感器是等比的**
- 传感器Sensor和胶片Film不一定一样
  - 平时两者等价
  - 在渲染器里面，Sensor收集irradiance，Film决定存成什么样的格式
  - ![img](https://raw.githubusercontent.com/nininias/image-repo/main/img/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252Fa6a23b02-0287-4cae-99c5-1466e375cb34%252FUntitled.png)



### Exposure 曝光

- **H = T x E**
- Exposure = time x irradiance
- **Exposure time (T)**
  - Controlled by shutter

- **Irradiance (E)**

  - 落在传感器上的单位面积的能量

  - 受光圈（aperture）和焦距（length）定义



- ### 曝光程度的控制

  - **Aperture size 自适应光圈大小**

    - 根据感光程度自动控制光圈大小，模拟瞳孔
    - 仿生学设计，模拟瞳孔
    - **F-Number (F-Stop)**

  - **Shutter speed 快门大小**

    - 改变期间收集到的光照能量
    - 越快，开放时间越短，相对越少光

  - **ISO gain 感光度**

    - 类似于后处理，直接乘

    - 改变传感器值和数字图像值之间的放大（模拟或数字化）

    - 硬件上 调节传感器灵敏度 或者 软件上 调整数字信号

    - 曝光的第三个可控变量

    - 胶片：改变ISO的敏感度

    - 数字信号: 对噪声的敏感度

      - 在模拟转换前对数字信号进行乘法运算
      - 具有线性效果

    - 为什么会产生噪声？

      - 因为光子数量很少

      - ![img](https://raw.githubusercontent.com/nininias/image-repo/main/img/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F1d9586d5-ad26-4c6d-ac25-bcf3ebac393b%252FUntitled.png)

      - 可以细微提升曝光度但是会带来很大的噪声



**光圈**

- **F-Number (F-Stop) 曝光等级 - 光圈大小**
  - 数字越小，光圈越大
  - 写作**FN**或者**F/N**
  - Informal understanding: the inverse-diameter of a round aperture 简单地理解为直径的倒数
- **Side Effect of Shutter Speed 快门速度过慢**
  - 动态模糊
  - 加倍快门的时间会加强运动模糊
  - 运动模糊可以带来速度感，有时候也不一定是坏事。不然看上去像是摆拍；而且有运动模糊还可以做到反走样的效果
- **Rolling shutter: different parts of photo taken at different times**
  - 物理快门并非瞬间打开，而是以某种方式“缓慢“打开的
  - 会导致高速运动的物体的成像与预想不一致





- **光圈数与快门按照这样的比例可以达到差不多的亮度**
- ![image-20241022213029288](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022213029288.png)
- **大光圈**可以带来**景深**，与此同时又会带来**更快的快门速度**，因此就会**削弱运动模糊**的效果







### Fast and Slow Photography



**High-Speed Photography 每秒极高帧率**

- 快门时间受限，
- Normal exposure = extremely fast shutter speed x (large aperture and/or high ISO)
- 可以拿来拍超高速的动态物体

**Long-Exposure Photography 延时摄影**

- 快门时间很长
- 光圈变小





### Thin Lens Approximation——薄透镜的近似

**Real Lens Designs Are Highly Complex**

- 目前的相机都用透镜组来控制成像
- 真实的透镜并不那么理想：无法将光线聚于一点 Aberrations
  - Real plano-convex lens (spherical surface shape). Lens does not converge rays to a point anywhere.

**Ideal Thin Lens – Focal Point**

- 理想的透镜可将光聚于一点 - 焦点
- (1) 所有进入透镜的平行光都经过焦点
- (2) 所有通过焦点的光线经过透镜后都是平行的
- (3) 焦距可以任意改变(实际上是这样子的→ 透镜组).



通过透镜组之间的相对位置变化来**模拟焦距的改变**

![image-20241022214451515](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022214451515.png)

- **Z~o~：物距**，物体到透镜的距离（物象在聚焦位置）
- **Z~i~：像距**，胶片到透镜的距离（成像在聚焦位置）
- **f：焦距**，两边的焦点到透镜的距离

The (Gaussian) Thin Lens Equation:![image-20241022214031445](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022214031445.png)

- 相似三角形证明

通过这个Thin Lens Approximation可以解释很多问题：



- ## 解释 Defocus Blur 虚化的原因

  Computing **Circle of Confusion (CoC)** Size

  ![img](https://raw.githubusercontent.com/nininias/image-repo/main/img/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252Fd88de14d-aabc-469c-ad91-644315bfbb14%252FUntitled.png)

  各种距离固定情况下，**CoC 与光圈大小成正比！**——表现为光圈越大，COC越大，越远的地方模糊越厉害。

  要想表现清晰，就得用小A，Coc才会小

  

  

- ### 光圈的完整定义

  - **==焦距/光圈直径==**——f / D

  - 也就是我们所说的F数

  - Common f-stops on real lenses: 1.4, 2, 2.8, 4.0, 5.6, 8, 11, 16, 22, 32

    the absolute aperture diameter (A) can be computed by dividing focal length (f) by the relative aperture (N). → An f-stop of 2 is sometimes written f/2, 因为 A=f/2

  - ![image-20241022215720754](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022215720754.png)

  - 将F数，光圈大小，焦距联系在了一起

  - 有→ **N 越大 → 光圈越小**



### Ray Tracing Ideal Thin Lenses

- 一种操作方法，可行的
  - 定义感光平面的大小，焦距和光圈大小
  - 定义物距z~o~
    - 根据透镜以及物距计算出深度Z~i~
- 渲染的时候
  - 遍历每一个胶片上的像素x‘
  - 在透镜上随机采样一个点x’‘
  - 通过计算，可以知道光线会穿过物距上的一个点x’‘’（公式<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022215720754.png" alt="image-20241022215720754" style="zoom: 67%;" />)
  - ![image-20241022220139502](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022220139502.png)



### Depth of Dield——景深



一次成像内，模糊的范围会因光圈大小而变化，而在focal plane肯定不会模糊。

Set circle of confusion as the maximum permissible blur spot on the image plane that will appear sharp under final viewing conditions

depth range in a scene where subject where the corresponding CoC is considered small enough



首先，当CoC（Circle of Confusion）小到一定程度，认为是清晰的

$z_o$附近的一段距离内都可以看作清晰（CoC小到一定程度），关键是这段距离有多长。→景深

光圈越小，景深范围越大

焦距越大，景深范围越小



http://graphics.stanford.edu/courses/cs178/applets/dof.html

![image-20241022221309985](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241022221309985.png) 

- **Aperture (光圈) A**：在图中光学系统的中心部分，表示光圈的大小。光圈控制通过透镜的光线数量，并且直接影响景深的范围。

  **Focal length (焦距) f**：透镜的焦距，指透镜把平行光线汇聚到焦点的距离，通常以 f 表示。焦距越长，景深越浅。

  **Object Distance (物体距离) D~S~**：这是图中从透镜到成像物体的距离，用   D~S~表示。在摄影或光学中，物体的距离影响成像效果。

  **Near limit of Depth of Field (景深的近端) D~N~**：这是在景深内的最靠近相机的可接受清晰区域的边界，表示景深的最近距离，用  D~N~表示。

  **Far limit of Depth of Field (景深的远端) D~F~**：这是在景深内最远的清晰区域边界，表示景深的最远距离，用 D~F~表示。

  **Circle of Confusion (弥散圆) C**：当光线没有完美地聚焦时，成像平面上的点会形成一个小圆，这个圆称为弥散圆。C 表示弥散圆的直径，决定了成像是否在视觉上可以认为是“清晰”的。

  **Near Focus Distance d~N~ **和 **Far Focus Distance d~F~**：这是图中光线在物体距离 d~N~ 和 d~F~ 处聚焦的情况，它们对应的是景深的前后极限。



### 光场 Light Field / Lumigraph

两个词指同一东西，属于历史遗留问题，是由两个组独立几乎同时发现的同样东西



- ### The Plenoptic Function（全光函数）

  - 灰度快照：从维度逐渐递推，定义一个对应法则，假如从场景中某一点某个角度所观察<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024120829148.png" alt="image-20241024120829148" style="zoom:50%;" />，看到的就是黑白的世界
  - 颜色快照：如果引入波长的概念，对于光有不同的波长<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024120953923.png" alt="image-20241024120953923" style="zoom:50%;" />，看到的就是彩色的世界
    - Seen from a single view point
    - At a single time
    - As a function of wavelength 彩色
  - 电影：再引入一个时间变量，可以达到帧效果，也就是可以动起来<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024121804608.png" alt="image-20241024121804608" style="zoom:50%;" />
    - is intensity of light
    - Seen from a single view point
    - Over time
    - As a function of wavelength 彩色
  - **全息电影/全光函数**：引入空间位置变量，使得可以从其他方位对场景进行观察，<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024122008853.png" alt="image-20241024122008853" style="zoom:50%;" />
    - is intensity of light
    - Seen from ANY viewpoint 任何位置看都可以
    - Over time
    - As a function of wavelength 彩色
    - 能在每一个时刻，从每一个位置，在每一个波长重建每一个可能的视角
    - 包含每一张照片，每一部电影，所有人见过的东西！它完全捕捉到了我们的视觉现实！

——要实现这样一个函数，需要：空间中任意一点的全方向光线信息



光场是全光函数的一个小部分



- ### Ray光线

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024122618906.png" alt="image-20241024122618906" style="zoom:50%;" />
  - 5D：3D position + 2D direction

- ### Ray Reuse光线再利用

  - 假设光线是恒定的↓
  - 4D : 2D direction（θφ） + 2D position（uv）
  - 非色散介质
  - 两个点可以定义一条光线

​	**要记录一个物体向四周展示的样子，只需要记录包围盒上表面各个点往各个方向发射的光线的信息：The surface of a cube holds all the radiance information due to the enclosed object. → 光场**（我好奇，对于一些静态物体，如果提前存储从包围盒发散到视点的向量，也就是提前把任意视角能看到的样子存起来，在实际运行的时候只是对于不同视角的调用而不进行实时计算（特别是需要高计算量的物体，比如毛发，头发，针织布料等），用空间换速度，是否可行？至少能够解决一部分人因为机型跑不动毛发而看到很多怪异的毛发吧？对于存储每一个像素不太现实，我们能否模拟插值存储的思维？光场存在于物体表面，然后我们将其纹理化存储起来，能否有个什么空间存储的技术？存储它的三个轴的空间纹理，然后需要的时候对三个轴进行空间坐标采样，碰撞盒就用原来的，能否优化性能？）

​	**光场就是包围盒上任意一点向任意方向发散的光照的强度**，光场只是全光函数的一部分。二维的位置，二维的方向。任何空间中的方向都能用θ和φ来表示

- ### 光场记录的方式（四维）

  - 2D position + 2D direction
  - 2D position + 2D position：用两个面上两个点来定义光线的方向→2 plane parameterization (u,v) → (s,t)
    - 记录所有的UV和ST的组合就是所有角度的光线

- 固定(u,v), 所有的(s,t)组成一张image，显示从(u,v)点看到的外部世界的样子。

  - Stanford camera array，单纯是个相机矩阵各自记录世界然后形成矩阵的模样

- 固定(s,t), 所有的(u,v)组成一张image，显示同一个点上从不同方向看过去的样子

  - Integral Imaging (苍蝇的眼睛Lenslets)
  - 从同一个点，将不同方向打过来的光线经过折射记录在不同的位置上
  - Spatially-multiplexed light field capture using lenslets: Impose fixed trade-off between spatial and angular resolution
  - 将单单记录Irradiance变成了不同方向的Radiance



- ### Light Field Camera

  - Lytro: founded by Prof. Ren Ng (UC Berkeley)

    - Microlens design

    - Most significant function

      - Computational Refocusing

      (virtually changing focal length & aperture size, etc. after taking the photo)

    - Each pixel (irradiance) is now stored as a block of pixels (radiance)<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024142311205.png" alt="image-20241024142311205" style="zoom:33%;" />

      - 恢复：A simple case — always choose the pixel at the bottom of each block
        - Then the central ones & the top ones
        - 可以虚拟地移动摄像机的位置
      - Computational / digital refocusing
        - Same idea: visually changing focal length, picking the refocused ray directions accordingly

    - The light field contains everything

  - 缺点

    - Insufficient spatial resolution (same film used for both spatial and directional information) 分辨率不足
      - 实际分辨率是**空间分辨率 * 方向分辨率** ，如果方向分辨率多，那空间分辨率就少
      - 感觉区别是：传统相机记录irradiance，光场相机记录radiance。把Irradiance添加了方向属性
    - High cost (intricate designation of microlenses) 高成本、难设计
    - Computer Graphics is about trade-offs 调节更灵活↔分辨率更高





**一个点记录一个方向的光：**

![image-20241024143103020](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024143103020.png)

**一个点记录多个方向的光照强度，就如：**

![image-20241024143008877](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024143008877.png)





# Lec 20 - Color and Perception





### Physical Basis of Color——基于物理的颜色

- 光=不同波长的电磁辐射（可见光光谱范围内400~700nm）

- 不同波长的光具有不同的折射率

- **Spectrum 光谱**：不同波长（频率）的光对应的类型

- **Spectral Power Distribution (SPD) 谱功率密度：**描述一束光在所有波长的分布
  - 短波高频
  - 长波低频.
  - 具有**线性性质**，多种光的同时照亮，直接加即可
  - ![image-20241024144318407](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024144318407.png)



- ### Color

  - Color 颜色：是人的一种感知，不是光线的一种根本属性 a phenomenon of human perception; it is not a universal property of light
  - 人怎么感知到颜色的↓
  - 人眼

- #### Eyes

  - 不同波长的光Different wavelengths of light are not “colors”

  人眼的简单介绍：瞳孔=光圈，晶状体=透镜，视网膜=感光元件

  视网膜上的感光细胞 Retinal Photoreceptor Cells

  - Rods：视杆细胞，棒状，很多，感知光的强度（而非波长）
  - Cones：视锥细胞，锥形，较少，产生“颜色”的感觉
    - Three types of cones S, M, and L (corresponding to peak response at short, medium, and long wavelengths), each with different spectral sensitivity![image-20241024145456758](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024145456758.png)
    - 不同的人的视锥细胞分布大不一样 Fraction of Three Cone Cell Types Varies Widely



### Tristimulus Theory of Color

Spectral Response of Human Cone Cells

于是不同视锥细胞的信号强度=其对所有波长的光的响应的积分

![image-20241024150225566](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024150225566.png)

任何一束光（光谱）→人眼视网膜 → (S,M,L) → Color 的对应，人眼只知道**(S,M,L)**，不知道原来的光线分布（SPD）



- ### Metamerism （同⾊异谱）

  - 同⾊异谱：不同的SPD → 同样的(S,M,L) = 同样的Color
  - ![image-20241024150615527](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024150615527.png)
  - 这些光谱SPD不一样，但是变成人眼能识别的SML之后，感知颜色是一样的
  - 在视觉成像设备里面，能够对光谱进行倒腾，使得设备SPD不一样但是人眼感知颜色一样，不同的设备调节出来的SPD，又会有不同的设备上的差异 —— 涉及到色彩空间
  - 同色异谱是两个不同的光谱（∞-dim），投射到相同的（S，M，L） （3-dim）响应。
    - These will appear to have the same color to a human The existence of metamers **is critical to color reproduction/**Matching
    - Don’t have to reproduce the full spectrum of a real world scene
    - Example: A metamer can reproduce the perceived color of a real-world scene on a display with pixels of only three colors 通过显示器的三色光，也可以混合出现实中的种种色彩（虽然背后的光谱一般完全不一样）



- ### Color Reproduction / Matching 颜色匹配、重建

  - 红黄蓝是色彩三原色，rgb是光学三原色；美术调色是减色系统，图形学叠色是加色系统。
  - 三个谱密度函数的线性组合
  - 通过混合的方式
  - 计算机的成像系统是加色系统：
    - 给定一组主色（例如RGB）的光谱分布（S，M，L？）$s_{R}(\lambda), s_{G}(\lambda), s_{B}(\lambda)$
    - 调整三种主色的强度并相加，得到一种颜色 $R s_{R}(\lambda)+G s_{G}(\lambda)+B s_{B}(\lambda)$
    - 于是这种颜色就可以用(R,G,B)这三个标量表示。
    - 于是也可以通过实验确定不同颜色的(R,G,B)表示->Additive Color Matching Experiment
  - ![image-20241024151255769](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024151255769.png)
  - 有些颜色怎么混合也混不出来，但是通过给原色加色，就可以混合出来，那么将最后混合的颜色中减去加上的颜色，就是对这种颜色的表示→系数会有负数！——理解为反过来实际上就是目标颜色加上某个颜色。![image-20241024151551608](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024151551608.png)
  - 



- #### CIE RGB Color Matching Experiment

  - CIE：一个组织

    主光 RGB 都是单一波长的光

  

  - ### 颜色匹配

  - 假如我们要这么一个光![image-20241024151759200](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024151759200.png)，然后我们用RGB去做颜色匹配，得到我需要的RGB各自的波长是多少<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024151834803.png" alt="image-20241024151834803" style="zoom:50%;" />

  - 做测试，测量多少强度的三种主光加起来会获得和测试光一样的颜色

    **==颜色匹配函数 color matching functions==** 描述了三种主光各自多少强度加起来会获得和测试光一样的颜色。

    Graph plots how much of each CIE RGB primary light must be combined to match a monochromatic light of wavelength given on x-axis

    Careful: these are not response curves or spectra!  

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024151920824.png" alt="image-20241024151920824" style="zoom:50%;" />

  - 所形成的这个图，就是为了拟合目标波长所需要的三个波长各自的波，即——**颜色匹配函数（color matching functions）**

  - 为了拟合人眼所看到的世界的颜色，就是需要如此的颜色匹配函数以匹配人眼对光谱的识别的颜色

  - **颜色匹配函数 color matching functions  横轴**：单一波长光的波长

  - **纵轴：**主光强度，对应波长上，不同颜色的波的强度

  - 现实的光线 = 许多不同强度单一波长光的积分
  - 现实的光线表示的颜色 = 许多不同强度单一波长光的Color Matching值的积分 → (R,G,B)

- 还记得之前说：任何一束光（光谱）→人眼视网膜 → (S,M,L) → Color 的对应，人眼只知道**(S,M,L)**，不知道原来的光线分布（SPD），这里实际上干的是差不多的事情（类似于响应积分，但不同）

  - 从外界光谱→RGB（通过颜色匹配函数）——思想一致，算法不同
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024152617924.png" alt="image-20241024152617924" style="zoom:50%;" />





### Color Spaces 颜色空间

- #### Standard Color Spaces

  - **Standardized RGB (sRGB)**
    - makes a particular monitor RGB standard
    - other color devices simulate that monitor by calibration
    - widely adopted today
    - gamut (⾊域) is limited



- #### A Universal Color Space: CIE XYZ

  - 同样定义了CIE XYZ color matching functions，但是并非实验测得，而是人造，虚拟的
  - ![image-20241024154025419](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024154025419.png)

  Imaginary set of standard color primaries X, Y, Z

  - Primary colors with these matching functions do not exist
  - **Y is luminance** (brightness regardless of color) (亮度)——Y在一定程度上可以表示亮度

  为何如此设计？

  - Matching functions are strictly positive 没有负数
  - Span all observable colors 覆盖所有可见光

  

将(X,Y,Z) 归一化获得(x,y,z)（x + y + z = 1）, 然后对(x,y)做枚举，获得一张二维图像，表示在Y（亮度）固定的情况下，不同X，Z对应的颜色。

![image-20241024154317485](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024154317485.png)

- ![image-20241024154439264](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024154439264.png)
- since x + y + z = 1, we only need to record two of the three
- usually choose x and y, leading to (x, y) coords at a specific brightness Y
- The curved boundary: spectral locus
  - corresponds to monochromatic light (each point representing a pure color of a single wavelength)
- Any color inside is less pure, mixed
- 亮度Y固定成某一个数，让X和Z发生某一个变化
- **色域**表现：
  - 中间白色混合颜色最多，最不纯（加性空间）
  - 周围边界处是最纯的
- 色域定义：
  - Gamut (⾊域)：由一组原色所产生的一组色度
    - 不同的色彩空间代表不同的色彩范围
    - 所以它们有不同的色域。
    - 它们覆盖了色度图上的不同区域



**Perceptually Organized Color Spaces**

- HSV Color Space (Hue-Saturation-Value)

  - Axes correspond to artistic characteristics of color

  - Widely used in a “color picker” 拾色器

- Hue(⾊调)

  - the “kind” of color, regardless of attributes
  - colorimetric correlate: dominant wavelength
  - artist’s correlate: the chosen pigment color

  Saturation (饱和度)

  - the “colorfulness”
  - colorimetric correlate: purity
  - artist’s correlate: fraction of paint from the colored tube

  Lightness (or value) (亮度)

  - the overall amount of light
  - colorimetric correlate: luminance
  - artist’s correlate: tints are lighter, shades are darker



**CIELAB Space (AKA L*a*b*)**

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024155602777.png" alt="image-20241024155602777" style="zoom:50%;" />

A commonly used color space that strives for perceptual uniformity

- L* is lightness (brightness)
- a* and b* are color-opponent pairs
- a* is red-green 正方向：红色，负方向：绿色
- b* is blue-yellow

- **基于互补色理论（Opponent Color Theory）**

  - There’s a good neurological basis for the color space dimensions in CIE LAB

    - the brain seems to encode color early on using three axes:
    - white — black, red — green, yellow — blue
    - the white — black axis is lightness; the others determine hue and saturation

    one piece of evidence: you can have a light green, a dark green, a yellow-green, or a blue-green, but you can’t have a reddish green (just doesn’t make sense)

    • thus red is the opponent to green

    another piece of evidence: afterimages (following slides)

- 视觉暂留

  - 根据视觉暂留，长时间停留在某一种颜色，突然出现白色的话会出现其互补色







人眼是奇怪的

- **视觉暂留**
- 视错觉
- 颜色的相对性





**减色系统**

典型：CMYK: A Subtractive Color Space

**墨水**越混越黑（印刷）

- Cyan, Magenta, Yellow, and Key
- 靛蓝、品红、黄色、黑色

- 黑色居多，总比能那三个颜色混成黑色，很贵





什么没有提

- HDR
- Gamma Correction矫正：Radiance → 颜色，因为显示器上颜色显示是非线性的，需要抵消
  - 每台计算机都会为我们做一次伽马空间的线性变换
  - 但往往在平时计算的时候我们会需要再线性空间计算
  - 因此在计算的时候需要考虑是否需要把颜色空间转换成线性空间进行计算，最后输出的时候再转换回伽马空间。





# Lec 21 Animation/simulation 动画/模拟仿真

**==先模拟，后渲染。各司其职==**

**History**

最早：狩猎鹿的动画(Shahr-e Sukhteh, Iran 3200 BCE)

圆盘旋转：(Phenakistoscope, 1831)

第一部Film：Edward Muybridge, “Sallie Gardner” (1878)

First Hand-Drawn Feature-Length (>40 mins) Animation：Disney, “Snow White and the Seven Dwarfs” (1937)

First Digital-Computer-Generated Animation：Ivan Sutherland, “Sketchpad” (1963) – Light pen, vector display

Early Computer Animation：Ed Catmull & Frederick Parke, “Computer Animated Faces” (1972)

Digital Dinosaurs!：Jurassic Park (1993)

First CG Feature-Length Film：Pixar, “Toy Story” (1995) （光栅化）

Computer Animation - 10 years ago：Sony Pictures Animation, “Cloudy With a Chance of Meatballs” (2009)

Computer Animation - last year：Walt Disney Animation Studios, “Frozen 2” (2019)



### Keyframe animation关键帧动画

- Animator (e.g. lead animator) creates keyframes 关键帧
- Assistant (person or computer) creates in-between frames (“tweening”) 渐变帧



- ### 关键的技术难点 - Interpolation 插值

  - 之前学的线性插值是最简单的一种插值，但是用来给动画是完全不够的
  - 怎样插值流畅怎样插值真实是有讲究的
  - 回忆光滑/可控插值的样条，之前对于线上的每一帧的插值，生成样条。同理，保持某种曲率的方式生成帧做到流畅过度<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024165428069.png" alt="image-20241024165428069" style="zoom:50%;" />
  - 手动或自动（线性插值）K关键帧



### Physical Simulation物理模拟

模拟、仿真：推导、实现公式，模拟出物体应该怎么变化

例子：布料模拟、流体模拟

**有力就会影响加速度→有加速度就会影响速度→有速度就会影响下一时刻更新的位置**

只要能够正确地把物理模型建立起来，虽然顶点很多，但总归是能够模拟出来的。

![image-20241024214002761](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024214002761.png)



- 流体对于物理仿真应用特别广



### 质点弹簧系统 Mass Spring System: Example of Modeling a Dynamic System

应用广泛，而且简单

- #### 最简单的弹簧

  - 没有初始长度
  - 随着拉力线性增长/缩短，线性系数是spring coefficient: stiffness
  - **劲度系数 * 力的方向**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024215511430.png" alt="image-20241024215511430" style="zoom:50%;" />

  - 力将点拉在一起
  - 力的强度与位移距离成正比（胡克定律）

- #### Non-Zero Length Spring

  - 初始长度Rest length（静止长度：指弹簧或其他弹性物体在没有受到外力作用时的自然长度）不为零

  - 由于没有能量损耗，所以会一直震动下去 oscillates forever

  - 力的大小就是弹簧被拉开来的长度

  - ![image-20241024220342197](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024220342197.png)

- 常用记法

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024220651660.png" alt="image-20241024220651660" style="zoom:33%;" />——一个点表示一阶导=速度，两个点表示二阶导=加速度

- ### 引入能量损耗

  - **Simple motion damping 阻尼**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241024220958750.png" alt="image-20241024220958750" style="zoom:50%;" />（绝对速度阻尼）
    - 表现得就像是有粘性的运动
  
    - 可以减慢不同方向的速度
  
    - K~d~是阻尼系数
  
    - 阻力的方向与速度方向相反
  - **问题**

    - 如果A和B同时向一个方向运动，表现不了弹簧内部的损耗，只能表示外部的力。换句话说就是只有外部有单独力作用才能引起阻力的产生
    - 和全局速度有关
    - 例如：当两个一起下落的时候，会因为damping下落的越来越慢。
  - **Internal Damping for Spring弹簧内部的阻尼** （相对速度阻尼）
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025133241633.png" alt="image-20241025133241633" style="zoom:50%;" />
      - 这个阻尼和长度没关系，只和速度有关系。和长度有关系的是上边[Non-Zero Length Spring](#Non-Zero Length Spring)
        - **f_b** 这是作用在物体 \( b \) 上的阻尼力。
        - **k_d **: 阻尼系数，表示阻尼效果的强度。
        - **\||b - a|| **: 表示 \( b \) 和 \( a \) 之间的距离（标量），用于归一化方向。
        - 中间那个是**点乘**，表示**相对速度投影到b指向a的单位向量上的投影**（点乘向量就是作投影的意思）
      - 仅在弹簧长度发生相对变化是才有的damping
        - 不会对两者同时相对运动产生阻力
      - Note: This is only one specific type of damping 只是一种阻尼的近似
  - **Structures from Springs**
    - 分子模型（这些物理模型的模拟通常都是对现实生活的一种简化，并不能完全代表真实世界）：
      - Sheets
      - Blocks
      - Others
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025135103893.png" alt="image-20241025135103893" style="zoom:50%;" />
    - **质点弹簧系统**抽象模型模拟
      - ①![image-20241025135811316](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025135811316.png)
        - 对角线拉扯会切变
        - 对折产生新层无力的作用变回去
      - ②![image-20241025135854116](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025135854116.png)
        - 往单方面对角线增加支撑
        - 单方面对角线有支撑
        - 另一方面对角线仍然可以被拉产生切变
        - 对折产生新层无力的作用变回去
      - ③![image-20241025135949612](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025135949612.png)
        - 两边都添加支撑
        - 对角线切变问题解决
        - 对折问题未解决
      - ④![image-20241025140019711](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025140019711.png)
        - 搁着一个点添加一根曲绳，力比蓝色的支撑要小很多
        - 解决了对折问题
        - 已经能够产生稍微正确的布料模拟





### FEM (Finite Element Method) Instead of Springs——有限元方法

广泛应用于车辆碰撞，很适合做这种**力的扩散**的场景

力传导扩散适合用有限元方法建模

不同的弹簧建模方法







### 动画系统之Particle Systems粒子系统

粒子系统还能模拟流体

- 粒子系统涉及到很多东西，很有挑战

- 每个粒子都有自己的属性

  - 粒子之间也有相互作用力
  - 自身也会受到力，内外部

- 将动力系统建模为大量粒子的集合

  每个粒子的运动由一组物理（或非物理）力决定

  在图形和游戏中流行的技术

  - 易于理解和实现
  - 可扩展：较少的粒子用于提高速度，更多的粒子用于更高的复杂性

  挑战

  - 可能需要大量粒子（例如流体）
  - 可能需要加速结构（例如，用于查找最近粒子的交互）

  动画中每一帧的步骤

  - [如有需要] 删除失效的粒子
  - 计算每个粒子上的力
  - 更新每个粒子的位置和速度
  - [如有需要] 生成新的粒子
  - 渲染粒子

- 

  定义个体和群体之间的关系

  定义modeling和解，学术界更关心如何解，更难



- **粒子系统之间的力**——**==本质上就是定义个体与群体之间的关系==**

  - **引力和排斥力**

    - 重力、电磁力，...
    - 弹簧、推进力，...
    - 像地球引力<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025142443078.png" alt="image-20241025142443078" style="zoom:33%;" />

    **阻尼力**

    - 摩擦力、空气阻力、粘性力，...

    **碰撞**

    - 墙壁、容器、固定物体，...
    - 动态物体、角色身体部位，...

  - **星系模拟，基于粒子的流体**

  - 流体就是从粒子模拟过来的，后面在上个材质渲染：先模拟，再渲染，各司其职



- 示例：将群集模拟为常微分方程 (ODE)

  - 定义鸟儿之间交互的规则：个体对群体的观察

  - 我的理解是，为每一个粒子（有点像程序化）提前定义好规则属性，提前定义好对于不同的行为变化所做出的不同的反应。通过先有鸡再有蛋的规律，先按照观察现有规律，为每一个粒子按照该观察规律进行程序化模拟

    - 将每只鸟建模为一个粒子，受到非常简单的力作用：

      - 吸引到邻近鸟群的中心
      - 排斥离个别邻居
      - 对齐到邻居的平均轨迹，模拟大量粒子系统的演变，数值上表现出复杂的涌现行为（也在鱼群、蜜蜂等中可见）

    - **示例：分子动力学**

      **示例：人群 + “岩石” 动力学**



### Kinematics——运动学

运动学：正向（FK）和反向（IK）





##### Forward Kinematics 正向运动学

明确骨骼之间的运动关系→计算出各个部位的位置<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025143425488.png" alt="image-20241025143425488" style="zoom:67%;" />

- **关节化骨架**

  - 拓扑结构（什么与什么连接）
  - 从关节得到的几何关系
  - 树结构（在没有环的情况下）

  **关节类型**

  - 销钉（1D 旋转）——单纯pin在上面，单平面扭动（角度是按照父骨骼与自身变动的夹角）
  - 球形（2D 旋转）——多了一个自由度，可以两个平面扭动
  - 滑动关节（平移）——可以拉伸<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025143655094.png" alt="image-20241025143655094" style="zoom:33%;" />

- **优点**

  - 直接控制很方便
  - 实现简单
  - 实现非常的物理

  **缺点**

  - 动画可能与物理不一致
  - 对艺术家来说耗时（很麻烦！！！）

- ![image-20241025143950518](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025143950518.png)





### Inverse Kinematics 逆运动学

限制各个部位（通常只有终端）的位置、限制骨骼的运动方式→计算骨骼的运动

方便控制形体整体形状

解特别复杂，可能并不唯一，这也就是为什么绑骨的时候要用极坐标来定义手肘和膝盖的弯曲方向。

- 可能会出现多解或无解的情况

相对于FK，他的解法——

![image-20241025144242995](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025144242995.png)

**解法：随机化算法（优化方法，梯度下降）**

**Numerical solution to general N-link IK problem**

- 选择初始配置
- 定义一个误差度量（例如，目标和当前位置之间距离的平方）
- 计算误差相对于配置的梯度
- 应用梯度下降（或牛顿法，或其他优化过程）

**例子**：基于风格的逆运动学 (Style-Based IK)





### Rigging

**Rigging** 是在角色上设置的高级控制，它允许更快速和直观地修改姿势、变形、表情等。

**重要性**

- 像操纵木偶的线一样
- 捕捉所有有意义的角色变化
- 因角色而异

**制作成本高**

- 手动操作：设定控制点、拉控制点（应该怎么定、应该怎么拉 → 动画师）
- 需要同时经过艺术和技术培训



- ### Blend Shapes 控制点间的位置插值计算

  - 不使用骨架，而是在表面之间直接进行插值

    例如，建模一组面部表情：

    - 最简单的方案：对顶点位置进行线性组合
    - 使用样条曲线控制随时间变化的权重选择



- ### Motion Capture

  - **真人控制点反映到虚拟角色中去，需要建立真实和虚拟的联系**

    **Data-driven approach to creating animation sequences**

    - 记录真实世界的表现（例如，某人执行某个动作）
    - 从收集的数据中提取随时间变化的姿势

    **优点**

    - 可以快速捕捉大量真实数据
    - 真实感可能很高

    **缺点**

    - 复杂且昂贵的设置（复杂、花钱）
    - 捕获的动画可能不符合艺术需求，需要修改（不符合艺术家要求，不可能实现的动画）

    **捕捉条件限制**

    **不同的捕捉方法**：

    - **光学方法**（更多内容在后续幻灯片中）
      - 在目标上放置标记
      - 通过多台摄像机进行三角测量定位
      - 8台以上摄像机，240 Hz，遮挡问题较难处理
    - **磁感应方法**：使用磁场推断位置/方向。需要连接线。
    - **机械测量方法**：直接测量关节角度。限制了运动。

    **很花钱**



- ### Motion Data

  - Data可以可视化成一些曲线

  - ![image-20241025150040629](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025150040629.png)

  - **面部动画的挑战**

    - 恐怖谷效应
      - 在机器人学和图形学中
      - 当人工角色的外观接近人类真实外观时，我们的情感反应会变得消极，直到它在表情的真实感上达到足够令人信服的水平
      - 总有人会去担心这个玩意

    **面部动作捕捉**

    **示例**：阿凡达



- ### 动画的制作流程

  - ![image-20241025150337649](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025150337649.png)





# Lec 22 - Animation Cont

Given the forces / physics / theory, how to simulate actual movements

### Euler

**已知**：对于每个单个粒子，v(t0)和 x(t0)

**目标**：x(t1)

- 之后，将推广到大量粒子

首先，假设粒子的运动由一个速度矢量场决定，该速度矢量场是位置和时间的函数

**场的概念**——

![image-20241025162036208](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025162036208.png)

速度可视化，读图信息可得切线（一次导）为速度，即——速度场：**==v(x,t)==**



- ### Ordinary Differential Equation (ODE) 常微分方程

  - 计算速度场内粒子的位置需要计算**一阶常微分方程**：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025162404409.png" alt="image-20241025162404409" style="zoom: 67%;" />

    - 解一阶常微分方程：

      - 连续：积分

      - 离散：Euler’s Method欧拉方法——用上一帧的属性来估计/计算下一帧的属性

        - Explicit Euler method （forward 前向、显式）

          - <u>始终用前一帧的状态来更新后一帧（将时间离散化）</u>
          - ![image-20241025162724300](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025162724300.png)
          - **下一帧的位置 = 当前帧位置 +下一帧时间*当前帧速度** 
          - **下一帧的速度 = 当前帧速度 + 下一帧时间*当前帧加速度**
          - 速度从一阶导来，积累时间就是距离；加速度从二阶导来，积累时间就是速度。

        - **显式欧拉方法的特性**

          - 简单的迭代方法

            常用方法

            <u>非常不准确</u>——会迅速地变得不稳定

            - 在数值积分中，误差会累积
            - 欧拉积分特别糟糕

            通常会变得不稳定

            - 容易出现正反馈，离正确结果越来越远
            - 如果步长**（△t）**定义的很长，得到的结果会和实际偏差很多<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025163501289.png" alt="image-20241025163501289" style="zoom:25%;" />
            - 可以通过缩小步长来优化，达到正确的模拟

          - 不稳定的体现：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025163725747.png" alt="image-20241025163725747" style="zoom:33%;" />

    - 一切用数值方法去解微分方程，都会遇到的问题——**误差**和**不稳定**

      - **误差** 不是特别大的问题
        - 每个时间步的误差会累积。随着模拟的进行，精度会降低
        - 在图形应用中，精度可能并不关键
      - **不稳定性** 很要命！
        - **误差**可能会**累积**，导致模拟发散，即使基础系统不收敛也很重要！
        - 缺乏稳定性是模拟中的一个基本问题，不可忽视

  - ### 解决不稳定性

    - #### 一些对抗不稳定的解决方法

      - **中点法 / 修改的欧拉法**——认为是平方级的东西 

        - 在起点和终点取平均速度

        **自适应步长**

        - 比较一步和两个半步，递归进行，直到误差在可接受范围内

        **隐式方法**

        - 使用下一个时间步的速度（难）

        **基于位置的方法 / Verlet 积分**

        - 在时间步后约束粒子的位置信息和速度

    - #### Midpoint Method（中点法）

      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025165801431.png" alt="image-20241025165801431" style="zoom:50%;" />
      - 先根据欧拉步骤计算出点a（在一个步长△t里面）
      - 然后取原点和a的中点b
      - 取得中点b的瞬时速度
      - 在原点根据中点b取得的瞬时速度和步长计算c点的位置（那条曲线是c按照我们所想的实际规律应该走的路径和应该到达点c的位置），与理想路线的点c位置几乎一样。
      - 逻辑公式：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025170222706.png" alt="image-20241025170222706" style="zoom:50%;" />
      - 计算公式：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025170312021.png" alt="image-20241025170312021" style="zoom:67%;" />
      - 为啥中点法能够准确模拟落点？
        - 因为多了个二次项，模拟了抛物线的运动轨迹

    - #### Adaptive Step Size——自适应

      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025171255295.png" alt="image-20241025171255295" style="zoom:33%;" />

      - 结合了之前的**中点法**，步骤：

        1. 先用欧拉计算出该步长的落点
        2. 尔后取中间的点，再求一次x‘（速度）
        3. 如果两者速度相差过大（自定义，归的过程）
        4. 则再取中点（修改步长）重新使用欧拉→中点法→取中点→判断中点与欧拉方法速度相差大不大
        5. 判断是否足够接近，否：则继续；是：停。

      - 基于误差估计选择步长的技术

        非常实用的技术

        但可能需要非常小的步长！

    - ### Implicit Euler Method——隐式欧拉方法

      - 与欧拉方法不同的是，这里用到的是下一步的东西（难）。欧拉方法都是前向的，用的都是当前步的加速度/速度之类的

      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025172059428.png" alt="image-20241025172059428" style="zoom:33%;" />

      - 由公式得

        - 是一个方程组，有三个未知数，两个式子，只能假设一个已知（猜）
        - 使用根求解算法，例如牛顿法

      - 提供更好的稳定性

      - #### 先知道如何确定/量化“稳定性”

        - 我们使用**局部截断误差**（每一步）/ 总累积误差（整体）

        - 绝对值不重要，重要的是相对于步长的**阶数**（研究误差和步长的关系，几次方？）

        - 隐式欧拉法是一阶的，这意味着：

          - 局部截断误差：**O(h^2^)**
          - 全局截断误差：**O(h)**（h是步长，即 Δt）

          对 **O(h)**的理解：

          - 如果我们将步长减半，我们期望误差也减半

    - ### Runge-Kutta Families——龙格库塔方法

      - 一类方法，很擅长拿来解ODE
      - 四阶版本是最常用的，即 **RK4**
      - 做法：
        - 初始条件![image-20241025173914528](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025173914528.png)
        - RK4解法![image-20241025173928269](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025173928269.png)
        - 其中![image-20241025173947918](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025173947918.png)
      - **更多**：数值分析对图形学有用（其他的：信号处理）

    - ### Position-based / Verlet integration——基于位置的方法 / Verlet 积分

      在时间步后约束粒子的位置信息和速度

      **假设弹簧无限大**

      不是基于物理的，**但是有时候挺好用。**下面的流体模拟就用到这个方法

      **思路**：

      - 在修改后的欧拉前向步骤之后，约束粒子的位置以防止发散和不稳定行为
      - 使用受约束的位置来计算速度
      - 这两个方法都会消散能量，使系统更稳定

      **优/缺点**：

      - 快速且简单
      - 不是基于物理的，会消散能量（误差）
      - 不保证能量守恒



### Rigid body simulation——刚体模拟

刚体，不会发生形变

- 简单情况
  - 类似于模拟一个粒子
  - 只是考虑多一点属性（拓展）

![image-20241025174945884](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025174945884.png)



- **X**: 位置

  **θ**: 旋转角度

  **ω**: 角速度（角度导）

  **F**: 力（速度导）

  **Γ**: 力矩

  **I**: 惯性矩（角速度导）

  

### Fluid simulation——流体模拟

**模拟只需要输出物体的位置，其他的就是渲染的问题了**，比如说这个粒子要长什么样，粒子和粒子之间怎么融合（我感觉这个融合长得有点像sdf的融合方法），那都是渲染要考虑的，这里只考虑模拟。



- ### Key idea

  - 假设水是由小球组成的
  - 假设水不可压缩（即，密度是常数）
  - 因此，只要密度在某处发生变化，就需要通过改变粒子的位置来“校正”——密度有变化，就要想着改回去，移动小球位置
  - 需要了解每个粒子位置对其周围密度的影响——一个小球位置的变化对其周围密度的影响
  - 更新？只需梯度下降！
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025180845467.png" alt="image-20241025180845467" style="zoom:50%;" />

  - 如果停不下来可以考虑人为地加入能量损失



- ### 流体模拟中两种不同的思路

  对于大规模流体粒子数特别多的情况下，用到的思路

  - Lagrangian：**质点法**，以每个元素为单位模拟<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025181658060.png" alt="image-20241025181658060" style="zoom:50%;" />

  - Eulerian：**网格法**，以空间为单位分割模拟<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241025181704727.png" alt="image-20241025181704727" style="zoom:50%;" />

  - #### Material Point Method (MPM)——混合方法，结合了欧拉和拉格朗日视角

    - **Lagrangian**：考虑粒子携带的物质属性——物质由粒子组成，粒子有属性

      **Eulerian**：使用网格进行数值积分——网格上计算如何运动

      **Interaction**：粒子将属性**传递给网格**，网格执行**更新**，然后将结果插值回粒子
      网格属性**写回网格内粒子**上
