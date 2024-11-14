**GAMES202**


------

录像回放：https://www.bilibili.com/video/BV1YK4y1T7yY/
课程PPT：链接: https://pan.baidu.com/s/1nkqxMTcnt1BtqFvu5wz-Rg 密码: sv4g

主页：[GAMES202: 高质量实时渲染 (ucsb.edu)](https://sites.cs.ucsb.edu/~lingqi/teaching/games202.html)

论坛：[Games202-高质量实时渲染 – 计算机图形学与混合现实在线平台 (games-cn.org)](https://games-cn.org/forums/forum/games202/)

作业发布：[GAMES202作业汇总 – 计算机图形学与混合现实在线平台 (games-cn.org)](https://games-cn.org/forums/topic/games202zuoyehuizong/)





# Lec2 Recap of CG Basics

## 计算机图形学（CG）基础回顾

- 基本 GPU 硬件管线
- OpenGL
- OpenGL 着色语言 (GLSL)
- 渲染方程
- 微积分



### 基本GPU硬件管线

- ### 基本 GPU 硬件管线

  1. **顶点处理**：接收顶点数据，应用几何变换（如模型视图和投影变换）。
  2. **图元装配**：将处理过的顶点组合成基本图元（如三角形、线）。
  3. **光栅化**：将图元转换为像素碎片（fragments），准备填充。
  4. **片段着色**：对每个碎片进行着色计算，决定其颜色和纹理。
  5. **测试和合并**：进行深度测试、模板测试等，将最终像素写入帧缓冲区。



### GpenGL

在CPU端对GPU进行自定义调度

优点：

- 跨平台
- 很好用的API

缺点：

- 很多版本
- C风格，有点不简单
- 不能debug



**OpenGL的流程（类比）**

- 重要的类比：油画
  - A. 放置物体/模型
  - B. 设置画架的位置
  - C. 将画布固定在画架上
  - D. 在画布上绘画
  - E. （将其他画布固定到画架上并继续绘画）
  - F. （使用以前的画作作为参考）



- **重要的类比：油画**
  - **A. 放置物体/模型**：相当于设置场景中的几何模型数据。在 OpenGL 中，这可以对应于将顶点数据加载到缓冲区中（如 VBO）。
  - **B. 设置画架的位置**：相当于设置摄像机的位置和视角。在 OpenGL 中，这通常包括设置视图矩阵和投影矩阵。
  - **C. 将画布固定在画架上**：相当于创建和绑定帧缓冲区。它决定了渲染的目标是屏幕还是另一个纹理。
  - **D. 在画布上绘画**：相当于执行顶点和片段着色器来绘制图像。在 OpenGL 中，这包括使用 `glDrawArrays` 或 `glDrawElements` 调用来进行绘制。
  - **E. 将其他画布固定到画架上并继续绘画**：相当于**多重渲染目标 (MRT)** 或后处理。在 OpenGL 中，这可能涉及绑定不同的帧缓冲区进行多次渲染。
  - **F. 使用以前的画作作为参考**：相当于使用纹理映射或渲染到纹理 (Render-to-Texture)。在 OpenGL 中，这可以是将之前渲染的结果作为纹理应用到新的几何体上。



- ### A. 放置物体/模型

  - **模型定义**
    - 模型规格说明
    - 模型变换
  - **用户指定对象的顶点、法线、纹理坐标，并以顶点缓冲对象 (VBO) 的形式发送到 GPU**
    - 与 `.obj` 文件非常相似
  - **使用 OpenGL 函数获取矩阵**
    - 例如：`glTranslate`，`glMultMatrix` 等
    - 不需要手动编写矩阵

- ### B. 设置画架
  - **视图变换**
  - **创建/使用帧缓冲区**
  - **设置摄像机（视图变换矩阵）**
    
    - 通过调用 `gluPerspective` 设置，例如：
      ```c++
      void gluPerspective(GLdouble fovy, GLdouble aspect, GLdouble zNear, GLdouble zFar);
      ```

- ### C. 将画布固定在画架上
  
  - **油画类比**：可以在同一个画架上绘制多幅画作（在同一个framebuffer可以绘制多张不同的纹理，深度，颜色，lightmap，光照贴图索引之类的，由fragment指定绘制到哪一个纹理上去）
  - **OpenGL 中的一次渲染通道**
    - 指定要使用的帧缓冲区
    - 指定**一个或多个纹理**作为输出（例如：着色、深度等）
    - 渲染（片段着色器指定每个纹理上的内容） 
  
- ### D. 在画布上绘画
  - **执行着色**
    - 这一步中会使用顶点/片段着色器
  - **对每个顶点并行执行**
    - OpenGL 调用用户指定的顶点着色器：
      - 转换顶点（模型视图、投影等）
  - **对每个基本图元，OpenGL 进行光栅化**
    - 为每个像素生成片段

  - **对每个片段并行执行**
    - OpenGL 调用用户指定的片段着色器：
      - 执行着色和光照计算
    - OpenGL 自动处理 z-buffer 深度测试，除非被覆盖

  - **这是最重要的步骤**
    - 用户定义的顶点、片段着色器执行
    - 其他操作大部分已被封装，甚至可以以 GUI 形式呈现

- ### 总结：每个渲染通道的步骤  

  - 指定对象、摄像机、MVP 矩阵等
  - 指定帧缓冲区和输入/输出纹理
  - 指定顶点/片段着色器
  - （当 GPU 上的所有内容都指定好后）进行渲染！

  ### F. 接下来的内容？
  - 多次渲染通道！
    - 使用先前的渲染作为参考



### OpenGL 着色语言 (GLSL)

### 顶点/片段着色（Vertex/Fragment Shading）
- 顶点和片段着色由小程序描述
- 使用类似 C 语言的语言编写，但有一定限制

- **历史沿革**
  - Cook 的 Shade Trees 论文和 Renderman，用于离线渲染
  - 早期：在 GPU 上用汇编语言编写
  - 斯坦福实时着色语言，SGI 的工作
  - 较早期：NVIDIA 的 Cg 语言
  - DirectX 中的 HLSL（顶点 + 像素）
  - OpenGL 中的 GLSL（顶点 + 片段）
  
- ### 着色器初始化（着色器本身稍后讨论）
  - 创建着色器（顶点和片段）
  - 编译着色器
  - 将着色器附加到程序
  - 链接程序
  - 使用程序

  - **着色器源码**：仅为一串字符串序列
  - **步骤类似于编译普通程序**



### The Rendering Equation

- ### ==渲染中最重要的方程==
  
  - **描述光传输**
  
  ![image-20241029161100095](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029161100095.png)
  
  - **各项解释**：
    - \( $L_o$ \): 出射辐射亮度（outgoing radiance）
    - \( $L_e$ \): 自发光（emission）
    - \( $f_r$ \): 双向反射分布函数（BRDF）
    - \( $L_i $\): 入射辐射亮度（incident radiance）
  
- ### 实时渲染 (Real-Time Rendering, RTR)
  
  - **可见性**通常被显式考虑
  - **BRDF**通常与余弦项一起考虑
  
  ![image-20241029161116525](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029161116525.png)
  
  - **各项解释**：
    - \( $L_o $\): 出射光亮度（outgoing lighting）
    - \( $L_i $\): 入射光亮度（来自光源的 incident lighting）
    - \( $f_r \cdot \cos \theta_i$ \): 余弦加权的 BRDF
    - \( $V $\): 可见性（visibility）



### Calculus

后面边说边提及





# Lec 3/4 Shadows

### 回顾：阴影映射 (Shadow Mapping)

- **阴影映射中的问题与解决方案**
- 阴影映射的数学原理
- 百分比更接近软阴影 (Percentage Closer Soft Shadows)——PCSS
- 基本滤波技术



 **知识补充——<u>点光源</u>和<u>方向光源</u>就应该产生<u>硬阴影</u>，而<u>面光源</u>才产生<u>软阴影</u>**



### Shadow Mapping

- ### 阴影映射的 2-Pass 算法

  - **2-Pass 算法**
    - 光照通道生成阴影贴图 (Shadow Map, SM)
    - 摄像机通道使用阴影贴图

  - **图像空间算法 (Image-Space Algorithm)**
    - 优点：不需要了解场景的几何信息
    - 缺点：会产生自遮挡问题和锯齿化问题

  - **著名的阴影渲染技术**
    
    - 即使在早期的离线渲染中（如《玩具总动员》），也是一种基本的阴影技术
    
	- #### **实现步骤**
	  
		- #### Pass 1: 从光源渲染
		
  	  - 从光源的视角渲染场景，并输出深度贴图（depth texture）。
  	  - 这张深度贴图记录了从光源视角看每个像素的距离，帮助判断哪些部分在阴影中。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029212012588.png" alt="image-20241029212012588" style="zoom:50%;" />
  	- #### Pass 2: 投影到光源以生成阴影
  	
  	  - 将摄像机视角中的可见点重新投影回光源方向，以判断这些点是否在阴影中。
  	  - 如果重新投影的深度值与光源深度贴图中的深度值相匹配，则该点为**可见**（没有遮挡）。
  	  - 如果两者不匹配，则该点被遮挡，位于**阴影中**。
  	  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029212024769.png" alt="image-20241029212024769" style="zoom:50%;" />
  	
  - #### **带来的问题**
  
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029213844138.png" alt="image-20241029213844138" style="zoom:33%;" />——奇怪的纹路
    - 由于从光照贴图中记录的**阴影是离散的**，因此每**一个像素会有实际的面积**。当这个**面积**在地板上的深度是一个**常数**，而由于**倾斜角度问题**，所记录的离散点的面积又会根据Light的角度发生更大的遮挡<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029214015942.png" alt="image-20241029214015942" style="zoom:50%;" />——**掠射角越大，前面的深度就遮挡的越厉害**
  
  - #### **解决方案**
  
    - 定义某一段距离内不算被遮挡。可以根据掠射角的大小进行反比调整
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029214311783.png" alt="image-20241029214311783" style="zoom:50%;" />
  
  - **解决方案带来的其他问题（no free lunch）**
  
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029214518857.png" alt="image-20241029214518857" style="zoom: 33%;" />
    - 阴影断连，因为前面说“某一段距离不算被遮挡”，如果在这段距离里面真的出现了物体，就会断连
    - 工业界有些只是在找一些参数使得恰好平衡
  
  - **学术界解决方案**
  
    - #### 阴影映射中的问题：二次深度阴影映射 (Second-Depth Shadow Mapping)
  
      - **方法**：在阴影贴图中使用第一个和第二个深度之间的中点<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241029215617215.png" alt="image-20241029215617215" style="zoom:50%;" />
  
        - 把这个中点用来当做原本要拿来被深度比较的贴图，我们拿shading point的深度和这个计算之后的中点做比较
  
      - **局限性**：
  
        - 要求对象具有封闭的表面（watertight），否则可能产生不准确的阴影
        - 增加了额外的计算开销，可能不值得应用
      - **应用场景**：
        - 没什么人用，因为要求物体一定是<u>watertight</u>——就是不能是一张纸，哪怕是地板都要做成一个很薄很薄的盒子
  





**==实时渲染不关注复杂度，只管速度快==**（我们不仅仅在乎**复杂度**，还在乎**常数**）



科学和技术不对等。科学说到这里，但技术是推陈出新的。止步于此不能让我更进一步，我需要学习更多陈新的技术。



### The Math Behind Shadow Mapping

![image-20241030120642335](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030120642335.png)

- （柯西-施瓦茨不等式）对两个微分方程相乘求积分再平方→收敛于对单个平方再求积分再相乘（）
  - 两个函数的积在积分后的平方不超过每个函数的平方积的积分之积。
- （Minkowski 不等式）对两个微分方程的和的平方求积分再求1/2次幂→收敛于各自先方再积分再1/2次幂相加
  - 两个函数和的积分的平方根不超过每个函数平方积分的平方根之和。



- ### RTR重点放在近似相等

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030154603991.png" alt="image-20241030154603991" style="zoom:67%;" />
  -  如果两个函数的积的积分，可以拆成约等于。归一化积分一个与另一个积分的乘积
  - #### **何时认为可以接受？**
    
    1. 当**g(x)**的**Ω**很小的范围的时候（实际积分域很小，**剩下留在积分里面的那个函数**,一些高光积分域）
    2. **g(x)**足够光滑（低频，积分域内变动不大，**剩下留在积分里面的那个函数**，漫反射）
       - 综上两个说的不就是BRDF嘛。
  - 应用场景——
    - ![image-20241030155140373](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030155140373.png)近似成
    - ![image-20241030155147870](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030155147870.png)
      - $$
        L_o(p, \omega_o) \approx \frac{\int_{\Omega+} V(p, \omega_i) \, d\omega_i}{\int_{\Omega+} \, d\omega_i} \cdot \int_{\Omega+} L_i(p, \omega_i) f_r(p, \omega_i, \omega_o) \cos \theta_i \, d\omega_i
        $$
      
        
      
      - **L<sub>o</sub>(p, ω<sub>o</sub>) ≈ ∫<sub>Ω+</sub> L<sub>i</sub>(p, ω<sub>i</sub>) f<sub>r</sub>(p, ω<sub>i</sub>, ω<sub>o</sub>) cos θ<sub>i</sub> dω<sub>i</sub>**
      
      - **何时近似有效？**（二选一）
  - **支持范围小**：适用于**点光源**或**方向性光源**（面光源以及glossy材质不太合适）
  - **被积函数光滑**：适用于**漫反射 BSDF** 或**恒定辐射的区域光源**

​		**解释**：
- 该近似式将可见性积分与辐射积分分开处理。
- 在支持范围小（例如点光源）或被积函数较为光滑（例如漫反射材质）时，这种分离可以获得较高的准确性。

  - ### 在AO的时候还会再提






### PCF (Percentage Closer Filtering)

- 原本是拿来做**抗锯齿**的<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030162250972.png" alt="image-20241030162250972" style="zoom:33%;" />

- ①不是对已经有锯齿的阴影进行fiter；②不是直接fiter shadow map，因为就算fiter shadowmap，和z buffer一对比，这不仍然还是非0即1的，还是会有锯齿，只是锯齿的位置会发生变化

- ### 提供阴影边缘的抗锯齿 (Anti-Aliasing)

  - **抗锯齿**：用于阴影边缘的平滑处理
    - 不适用于软阴影（PCSS 适用于软阴影，将在后续介绍）
    - 对阴影比较结果进行滤波，以减少边缘的锯齿效果

  ### 为什么不直接对阴影贴图进行滤波？

  - **纹理滤波**：仅对颜色分量进行平均，这会导致阴影贴图变得模糊
  - **深度值平均**：平均深度值后进行比较，结果仍然是**二值可见性**，无法生成平滑过渡的阴影

  **总结**：直接对阴影贴图进行滤波无法生成平滑的阴影边缘，因此通常通过滤波阴影比较的结果来实现抗锯齿。

- ### 实际做法

  - 已生成shadowmap，然后我在摄像机找shading point在shadowmap对应的点的时候，不单单找一个点，我们找好多个点，对多个与shading point的比较的值进行取平均。（就是我们找我们查的点，而且也不是二值）

  - 找周围的点都和中间的shading point进行二值比较，将得到的所有0和1进行平均得到摄像机对应的shading point的阴影程度。**每一个shading point**我都要**查好多个shadow map**，7 * 7或9 * 9，每个都这样，然后取平均，可想而知**计算量多大**。

  - #### 实际做法的详细解释
    
    - **步骤**：
      
      1. 对每个片段执行多次深度比较（例如 7x7 区域）。
      2. 然后，对比较结果进行平均。
      
    - **示例**：对于地板上的点 \(P\)
      
      1. 将其深度与周围像素（如 3x3 的红色区域）内的所有像素深度进行比较。
      2. 获取比较结果，例如：
         ```
         1, 0, 1
         1, 0, 1
         1, 1, 0
         ```
      3. 取平均值，得到可见性，例如 0.667。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030164544560.png" alt="image-20241030164544560" style="zoom:50%;" />
      
    - **效果**：通过对多次比较结果的平均，可以得到平滑的阴影过渡效果，有效减少阴影边缘的锯齿。![image-20241030164923732](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030164923732.png)
    
    - Filter大小的选择决定了阴影的软硬，且与<u>投射平面就遮挡物的距离</u>有关



### PCSS (Percentage Closer Soft Shadows)

在PCF的基础上，做软阴影

实际上就是**==在PCF之前，做了Blocker的计算，用Blocker Depth来估算W~Penumbra~的面积，通过面积来进行卷积范围的修改。==**

![image-20241030212303507](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030212303507.png)



相关名词解释

![image-20241030161548066](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030161548066.png)

为了实现软阴影所使用的工具——**PCF (Percentage Closer Filtering)**

- **Filter尺寸是否重要？**
  
  - 较小的滤波尺寸 -> 阴影边缘更锐利
  - 较大的滤波尺寸 -> 阴影边缘更柔和
  
- **是否可以用 PCF 实现软阴影效果？**
  - PCF 通过调整滤波窗口的大小来实现柔和的阴影效果
  - <u>投射平面就遮挡物的距离</u>**（blocker distance）**越远，filter越大
  
- **关键思考点**
  - 从硬阴影到软阴影的过渡
  - 滤波的**最佳尺寸**是多少？
  - 这种滤波尺寸是否统一适用？

-  **关键观察 [Fernando et al.]**

  - **问题**：阴影的哪些部分更锐利？哪些部分更柔和？

  - **观察**：
    - 阴影靠近物体边缘时通常更锐利。
    - 随着阴影距离物体边缘越来越远，阴影边缘逐渐变得柔和。
    
  - **解释**：这种现象是由于阴影边缘的柔化是由光源的大小以及物体与投影表面之间的距离决定的。近处的阴影部分几乎没有扩散，而远处的阴影部分会更柔和。

  这种观察是实现柔和阴影（如 PCSS）的一种基础原理。

- #### **关键结论**

  - **滤波尺寸与遮挡物距离的关系**：
    - 滤波尺寸（即阴影边缘的柔化程度）与遮挡物（blocker）到接收面（receiver）的距离成正比。
    - 更准确地说，它取决于遮挡物的**相对平均投影深度**。

  - **数学“翻译”**：
    - 半影区域宽度 \( $$w_{Penumbra}$$ \) 计算公式：
      ![image-20241030171457285](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030171457285.png)
      其中：
      - \( $d_{Receiver} $\)：接收面到光源的距离
      - \( $d_{Blocker}$ \)：遮挡物到光源的距离
      - \( $w_{Light}$ \)：光源的宽度

  **解释**：这个公式表明，随着遮挡物离光源越远，阴影的边缘（半影区域）变得更宽，实现更自然的软阴影效果。

- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030171304186.png" alt="image-20241030171304186" style="zoom:50%;" />
  
  - 取决于**Light的大小；Blocker的距离**
  - 这里的Blocker深度指的是平均的深度，指的是对应的shading point所被遮挡的在shadowmap的像素的区域平均值。
  - 面光源如果每个点一个shadowmap那就特别多，所以面光源的shadowmap的生成是根据**中心点**来生成的，用**点光源的方式来生成shadowmap**。
- ### 问题：
  - 唯一的问题是：**遮挡物的深度 d~Blocker~ 如何确定？**

  ### PCSS（Percentage Closer Soft Shadows）完整算法

  1. **Step 1: <u>遮挡物</u>搜索**
     - 在特定区域内找到平均**遮挡物深度** d~Blocker~。（只找遮挡物的深度，假如是7，就只找小于7的<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030215642096.png" alt="image-20241030215642096" style="zoom:25%;" />)
     - **blocker的深度**指的是——特定区域内被遮挡的值进行平均，个数上进行累加，值上只对产生了遮挡的值进行平均
     
  2. **Step 2: 半影区域估计**（PCF）
     - 使用**<u>平均</u>遮挡物深度d~Blocker~**来确定**滤波尺寸（Filter Size）**，以计算半影区域宽度。
     
  3. **Step 3: Percentage Closer Filtering**（PCF）
     - 应用 PCF 对阴影边缘进行柔化处理，实现柔和阴影效果。
     - 根据 **d~Blocker~**动态调整Filter的大小
- **选择哪个区域进行遮挡物搜索？**（我怎么知道要选多大的filter size？）

  -  我们要用PCSS就是为了找**多大区域**，而我们计算**d~Blocker~**就需要用到这么个**区域**——鸡生蛋？蛋生鸡？
  -  如何找这个**<u>区域</u>**？——可以设置为固定的区域（例如 5x5），但使用**启发式方法**可能效果更好。
     -  **启发式方法**
        -  <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030175827749.png" alt="image-20241030175827749" style="zoom:50%;" />
        -  **在哪个区域（阴影贴图上）进行遮挡物搜索？**（红色区域取多大）
           - 取决于光源的大小
           - 以及接收面到光源的距离
           - 假设shadow map放在light的<u>近平面</u>（shading point 和light 和shadowmap组成类似摄像机的视椎体），就单纯连接shading point与面光源Light的各个顶点，与近平面相交的区域是多大。我们就对这个区域进行**深度平均**，然后取平均深度到shading point 的距离就是d~Blocker~
- ——好大的开销（工业界提出了不同的方法）

### A Deeper Look at PCF

- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030204806772.png" alt="image-20241030204806772" style="zoom:50%;" />——shading point周围（圆）进行filter

- **Filter/convolution**

  - $$
    [w * f](p) = \sum_{q \in \mathcal{N}(p)} w(p, q) f(q)
    $$
  
    - **参数解释：**

      - **p**：当前像素的位置。
      - **q**：表示 **p** 周围的一个采样点。
      - **N(p)**：**p** 的邻域，包含了所有要参与平均的采样点。
      - **w(p, q)**：加权函数，决定了 **q** 对 **p** 的贡献程度。
      - **f(q)**：采样点 **q** 的阴影值（0 表示不在阴影中，1 表示在阴影中）。
      - **w \* f**：表示在位置 **p** 的滤波（卷积）结果。
      - **∑{q ∈ N(p)}**：求和操作，遍历 **p** 邻域 **N(p)** 中的所有采样点 **q**。
  
    - **在 PCSS（Percentage Closer Soft Shadows）中**:
  
    - $$
      V(x) = \sum_{q \in \mathcal{N}(p)} w(p, q) \cdot \chi^+[D_{SM}(q) - D_{scene}(x)]
      $$
  
      - **V(x)**：位置 **x** 的可见性值，用于表示该点是否在阴影中。值范围在 0 和 1 之间，0 表示完全在阴影中，1 表示完全可见。
  
      - **∑{q ∈ 𝒩(p)}**：在 **p** 的邻域 **𝒩(p)** 中对所有采样点 **q** 求和。
  
      - **w(p, q)**：加权函数，用来控制邻域内每个采样点 **q** 对 **p** 的贡献。
  
      - **χ⁺[ D~SM~(q) - D~scene~(x) ]**：一个指示函数，如果 **D~SM~(q) - D~scene~(x)** 为正，则返回 1，表示 **q** 相对于 **x** 是可见的；否则返回 0，表示被遮挡。
  
    - 在 **PCF** (Percentage Closer Filtering) 中，不直接对shadowmap进行滤波并再比较，而是对每个采样点单独计算可见性。![image-20241030211017035](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030211017035.png)
  
      - 说的是并非直接对shadowmap进行滤波然后与shading point的深度进行比较，这得到的仍然是二值化的没有虚化的值
  
    - 并且 PCF 也不是对带有二值可见性的结果图像进行滤波，即对比得到哪里是阴影哪里是亮部的图之后直接进行滤波
      $$
      V(x) \neq \sum_{y \in \mathcal{N}(x)} w(x, y) \cdot V(y)
      $$
  
    - 而应当是**对范围得到的掩码进行滤波**！



### Revisiting PCSS

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030212402750.png" alt="image-20241030212402750" style="zoom: 67%;" />

- ### PCSS 的完整算法    

1. **步骤 1**：Blocker search  
   - 在特定区域内获取平均遮挡者深度。

2. **步骤 2**：半影估计  
   - 使用平均Blocker Depth来确定滤波大小。

3. **步骤 3**：百分比更接近滤波（PCF）

- ### 哪些步骤可能较慢？

  - **检查区域内的每个纹素**（步骤 1 和 3）。

  - 阴影越柔和，所需的滤波区域越大，导致处理速度变慢。

- 工业界里面存在在图像空间通过时间累积猜测该时刻的图像的方式进行**图像空间的降噪**的方式解决，以后讲

- 而且目前工业界对于噪声的容忍度越来越大，因为可以获得空间的信息和时间的信息，对于图像层面的降噪效果可以做的越来越好，归功于一系列Temporal方法——TAA

  - 甚至可以达到实时光追1SPP降噪成很能接受的效果。




边缘可能会造成**Flicker（闪烁，鬼影）**



针对上面PCSS算法很慢的方式（第1、3步）提出解决方案VSMM



- **Variance soft shadow mapping（VSMM）**
- **MIPMAP and Summed-Area Variance Shadow Maps**
- **Moment shadow mapping（MSM）**



### Variance Soft Shadow Mapping

很快无噪声，值得学习

针对PCSS的第一步查找，第三部滤波很慢的问题，提供了解决方案



- ###   ==第三步==

  - 原本不是在找shading point 通过加权计算未被遮挡的**区域占比**作为值。如果可以跳过计算，而是通过直接查找的方式，将会节省特别多的计算和对比的时间。——近似的认为有一个正态分布。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030222143796.png" alt="image-20241030222143796" style="zoom:33%;" />——根据某一个点估计出所在的**区域占比**。
  - 需要正态分布，那就需要知道：**均值(期望/mean),方差(variance)**
    - **均值/期望（Mean/Average）获取方法**
      - Hardware **MIPMAPing**（方形的）
      - Summed Area Tables(SAT)（求任意2D矩形的表的数据结构方法）
    - **方差（Variance）获取方式**
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241030222748536.png" alt="image-20241030222748536" style="zoom:67%;" />
      - 顺手多生成一个深度的平方的贴图存储在另一个通道
    
  - **回到问题**
    
    - 目标：计算比着色点更近的纹素百分比
    - 计算：你需要计算阴影区域的面积
    - 提示：可以通过计算高斯分布的累积分布函数（CDF）得到准确答案
    
    - **关于高斯分布的累积分布函数（CDF）**
    
      - 左图展示了概率密度函数（PDF），其中阴影部分代表了累积分布的面积
    
      - 右图展示了累积分布函数（CDF），随着 x 值的增加，CDF 值逐渐趋近 1
      - ![image-20241031114423113](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031114423113.png)
      - 为了得到数值解，C++里面有一个`std::erf`的函数可以直接查表获得
    
  -  **切比雪夫不等式**（t > Mean）——不等式**当做约等式**，**近似计算CDF**
  
    - ![image-20241031115118167](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031115118167.png)
  
      - **μ：均值（mean）**
  
        **σ^2^：方差（variance）**
  
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031115152765.png" alt="image-20241031115152765" style="zoom:50%;" />
  
    - 当给定均值和方差，不需要知道是不是正态分布，就可以大致估算到这个CDF的走势。即使我们不要求分布是正态分布，切比雪夫不等式仍能提供概率上界，有助于进行概率估计。
  
    - 我们就要这么个CDF，是不是正态分布实际上并不关心。
  
    - **满足条件：**
  
      - **t > Mean**
  
- ### 性能

  - **Shadow Map生成：**
    - “square depth map”：并行生成，伴随shadow map生成，参与像素的数量为#pixels。
    - 其他需要考虑的内容？

  - **运行时间：**
    - 范围内深度的平均值计算： \(O(1)\)
    - 范围内深度平方的平均值计算： \(O(1)\)
    - 切比雪夫计算： \(O(1)\)
    - 不需要采样或循环！

  - **步骤3（滤波）是否完美解决？** （待确认）
    - mipmap在硬件操作上已经很流畅，几乎不花时间。


- ### ==第一步==

  - ![image-20241031121037236](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031121037236.png)

  - ### 回到步骤1：遮挡器搜索（在一个区域内）

    - 还需要采样（循环）较早的部分，效率低下。
    - 遮挡器的平均深度。
    - 不是总体的平均深度 <span>Z<sub>avg</sub></span>。
    - 是那些深度 <span>z &lt; t</span> 的像素的平均深度。

    - ### 关键概念

      - **遮挡器**（<span>z &lt; t</span>），平均深度：<span>Z<sub>occ</sub></span>——√

      - **非遮挡器**（<span>z &gt; t</span>），平均深度：<span>Z<sub>unocc</sub></span>

  - ### 关键概念

    - **遮挡器**（<span>z &lt; t</span>），平均深度：<span>Z<sub>occ</sub></span>（我们想要计算）
    - **非遮挡器**（<span>z &gt; t</span>），平均深度：<span>Z<sub>unocc</sub></span>

    $\frac{N_1}{N} Z_{unocc} + \frac{N_2}{N} Z_{occ} = Z_{avg}$
  
- 近似：<span>N_1 / N = P(x > t)</span>，使用切比雪夫不等式！——直接查询遮挡概率
    - 近似：<span>N_2 / N = 1 - P(x > t)</span>
    - <span>Z<sub>unocc</sub></span> 的真实值未知
    - 近似：<span>Z<sub>unocc</sub> ≈ t</span>（即假设阴影接受面是**平面**）
    
- ### 结论
  
  - 步骤1通过忽略额外成本解决。
  
- **效果**
  
  - ![image-20241031122136912](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031122136912.png)



### Mipmap and Summed-Area Variance Shadow Maps

- ### Mipmap![image-20241031141257982](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031141257982.png)

  - 仍然只是对于一个像素的近似，如果涉及到周边的其他像素那就需要通过插值查找（猜），不精确





- **SAT**（数据结构）

  - 对应算法“前缀和”

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031152707176.png" alt="image-20241031152707176" style="zoom:33%;" />

    - **预计算：**从前面一直累加到尾部，然后如果项要求哪里到哪里的和，直接用最后面的SAT减去前面的SAT即可

  - 如果是二维的，亦可以用一维的方式去存储计算。也可以用左和右，上到下的方式计算

    - ![image-20241031153404663](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031153404663.png)

    - **经典数据结构与算法**

      - 在二维情况下的计算方式如图

      **注意事项**：这种方法精确，但需要 O(n)的时间和存储空间来构建

      - 存储可能不是问题
      - 我们能加速构建 SAT（Summed Area Table）吗？

 

### Moment Shadow Mapping

- ### 	  VSSM的局限性

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031154718856.png" alt="image-20241031154718856" style="zoom:33%;" />
    - 如图，此时左边密密麻麻很多深度穿插（类似overdraw，哪里是深度密集区），确实可以认为就是符合某种正态分布的；
    - 如果是右边，越简单的情况，只有三个深度，他本应该表示三个波峰<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031154837951.png" alt="image-20241031154837951" style="zoom:33%;" />
    - 如图（**CDF表示有多少深度比shading point要大——挡不住**）就会算的稍微不准。
      - 亮的地方稍微变暗——可以接受
      - 暗的地方变亮——不能接受
    - ![image-20241031155117047](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031155117047.png)——漏光。深度密集位于下部分。
      - 我看出来的规律是：物体深度密集区域越接近于接收物，越有可能产生**Light leaking**
    - 带来的坏处
      - Light Leaking
      - non-planarity artifact<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031155632917.png" alt="image-20241031155632917" style="zoom:50%;" />



#### **为了让VSSM对深度分布描述地更准确**

- **目标**
  - 更准确地表示一个分布（但存储代价仍然不应过高）

- **思路**
  - 使用 **高阶矩** 来表示一个分布





- ###  Moments

  - - **矩**
      - 关于定义有很多种变体
      - 我们使用最简单的：
        \( x, x<sup>2</sup>, x<sup>3</sup>, x<sup>4</sup>, ……)
      - 因此，VSSM 实际上使用的是 **前两个阶的矩**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031161316814.png" alt="image-20241031161316814" style="zoom:67%;" />
  - CDF怎么倒腾的没讲。

- ### **Moments 的作用**

  - **结论**：前 <i>m</i> 阶的矩可以表示具有 <i>m</i>/2 个步骤的函数
  - 通常来说，4 阶的矩已经足够近似实际深度分布的 CDF
  - 如何恢复与给定矩相匹配的 CDF（此处部分内容被划掉）

- **Moment Shadow Mapping**
  - 与 VSSM 非常相似
  - 在生成阴影贴图时，记录 <i>z</i>, <i>z<sup>2</sup></i>, <i>z<sup>3</sup></i>, <i>z<sup>4</sup></i>
  - 在遮挡物搜索和 PCF 过程中恢复 CDF

![image-20241031161648826](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031161648826.png)

——Light Leaking得到完美解决

- **优点**: 结果非常好
- **缺点**:
  - 存储成本较高（可能可以接受）
  - 性能成本较高（主要在重建过程中）





# Lec 5/6 Real-Time Environment Mapping

- **Finishing up on shadows**
  - 距离场软阴影（DF）

- **Shading from environment lighting**
  - 拆分和求和近似

- **Shadow from environment lighting**







### Distance Field Soft Shadows

效果对比

![image-20241031165121306](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031165121306.png)

- **Distance functions**:

  - 在任意点上，给出到物体上最近位置的**最小距离**（可以是有符号的距离）。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031165638671.png" alt="image-20241031165638671" style="zoom:50%;" />
  -  

- ### **SDF**

  - 它可以做不同形状的Blending。通过到边界的点的有向距离记录，可以实现 边界的推移。（形状的融合？）

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031192240336.png" alt="image-20241031192240336" style="zoom:50%;" />

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031192346698.png" alt="image-20241031192346698" style="zoom:50%;" />

    - 涉及理论——**最优传输（Optimal Transport）**（机器学习多出现）



### Sign Distance Fields

——搞定visibility项

- ### 应用场景

  - **光线追踪（光线步进/Ray marching/视点）**：定义个光线，用光线与SDF的隐含表面进行求交。空间中的SDF有一个安全距离，在此距离一定不会和物体发生碰撞。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031195848766.png" alt="image-20241031195848766" style="zoom:50%;" />

    - 使用光线步进（球体追踪）来执行光线-SDF 交点计算
    - 背后是一个非常聪明的想法：
      - SDF 的值表示一个“安全”距离
      - 因此，每次在 **p** 点，只需前进 **SDF(p)** 距离
    - *多个物体取最小的sdf即可，所以sdf的计算很独立*

  - **软阴影生成（光）**：我们将sdf记录的安全距离改成记录的是安全角度，然后再作为visibility的值的映射。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031203749374.png" alt="image-20241031203749374" style="zoom:50%;" />

    - **用法 2**

      - 使用 SDF 来确定**遮挡的<u>大致</u>百分比**
      - SDF 的值表示从**视点**看到的“安全”角度（生成阴影的话那就是光源的）
      - 不考虑遮挡物的位置
    
    - **观察**
      - “安全”角度越小 <-> 光照函数的**visibility项**越低
      
    - **从视点出发计算安全角度**,如图
    
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031204121859.png" alt="image-20241031204121859" style="zoom: 67%;" />
    
        - 好像是由P1得知P2的安全角度（那个切线，然后求角度），由P2得P3的安全角度。
          - P1,P2,P3的安全距离就是根据**SDF（基于场景中的几何形状预先计算的，或者在运行时通过数学公式或查找表来得到）**生成出来的，但是步长是根据前一个点的安全距离与视点的射线的交点确定的
        - 安全角度的占比用来评估当前摄像机的遮挡的大致百分比来影响visibility项用的
        - 然后我们去**最小的**（θ~3~）安全角度来估计阴影的visibility
    
      - #### **如何计算这里提到的角度θ呢？**
    
        - $$
          \arcsin \frac{\text{SDF}(p)}{|p - o|} \quad
          $$

$$
\min \left\{ \frac{k \cdot \text{SDF}(p)}{|p - o|}, 1.0 \right\}
$$

  - 公式5计算量太大，公式6就很对（主要就只是求角度值，这里是sin）
    - **|p - o|**：点到sdf的点的距离
    - k：缩放因子，表现为可以调整阴影的软硬，越大越硬阴影
      - 越大的K表现为可以切除掉的半影就越多，边缘更加硬


​    
  - **抗锯齿**：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031212413285.png" alt="image-20241031212413285" style="zoom:50%;" />
  
    - 之前看到有一些方法可以用sdf来做文字的抗锯齿
    - https://github.com/protectwise/troika/tree/master/packages/troika-three-text![Text with a texture](https://raw.githubusercontent.com/nininias/image-repo/main/img/screenshot4.png)





- **场景里面Distance field大**概长这样，好像是绑定在物体上的SDF
  - ![image-20241031210814373](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031210814373.png)







- ### 优点

  - 快（指的只是在忽略生成情况下，单纯写一个ray marching，生成shadow map的时间才是大头）
  - 质量高

- ### 缺点

  - 需要预计算
  - 存储需要很多空间
  - 还会产生一些artifact（我想到单纯在场景里面求图行不行，我不是可以对物体进行剔除嘛，想个办法对场景中的物体三个维度进行剔除直到能得到我想要的点的sdf,）





### Shading from Environment Lighting

​	在渲染里面我们认为环境光照是从无穷远的地方来的，也就是无论放的地方在哪接受到的环境光照都是对于个体而言的，没有距离区分。

- **区分**

  - Spherical map vs. cube map

- ### **IBL（Image-Based Lighting）**

  - ——==对于**高频和低频的信息都能**很好的表现光照信息==，通过使用环境贴图（如立方体贴图）来表示周围环境的光照信息，从而实现更真实的光照效果。

  - 使用实时渲染积分函数

  - ![image-20241031214243252](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031214243252.png)

  - 这里好像是打算将图片作为自发光，然后对其进行采样来算

  - **通用解决方案 — 蒙特卡罗积分**
  
    - 数值计算
    - 需要大量样本

  - **问题 — 可能会很慢**
  
    - 一般来说，在着色器中**不推荐采样**（现在不一定）
    - **我们能避免采样吗？**

  - ![image-20241031220122659](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031220122659.png)
  
    - 左边的BRDF属于glossy——support很小
    - 右边的BRDF属于diffuse——很光滑
    - 应用约等式

      - ![image-20241031220315592](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031220315592.png)
  
      - Ω~G~表示的是**G函数有值的地方的积分区域**
        - 之前不是说如果很support或者很光滑的时候，这个约等式就比较准。**BRDF恰好满足这种要求**。

  - ### **约等式应用到BRDF**

    - ![image-20241031220732880](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031220732880.png)

    - Li好像是很support
  
    - 把光照项按照约等式提出来。在有光区域的光照辐射度的积分除以面积（空积分），使得我们的Lighting项可以被为我们prefilter——预计算生成
  
      - 不同于shadows的生成，**它提取的是visibility**，我们这里提出来的是**light radiance**。**<u>如果只考虑shading而不考虑shadow，那就不考虑visibility项。</u>**![image-20241031220918194](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031220918194.png)
      - 根据需要自行提取
  
    - ### **积分并且normalize是什么意思**
  
      - **filter：**多范围取平均，那不就是**滤波**嘛。滤波的核决定了我的BRDF占多大
        - **环境光的预过滤**
          - 预生成一组不同滤波的环境光
          - 介于中间的滤波尺寸可以通过三线性插值来近似。很类似于生成mipmap时候使用的三线性插值。（如果想要filter值5的话，就去filter值4到8之间进行插值获取即可）
      - 本来查的是一个点的值，查区域值直接插filter之后的值
        - ![image-20241031221638447](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031221638447.png)
        - 我们说的filter，实际上就是这个立体角的大小

    - ### 求解积分之分离之后，二阶段

      - 那刚才的积分我们把L~i~的积分拆出来搞定了，那右边的BRDF怎么进行积分呢？![image-20241031223832198](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031223832198.png)

        - **预计算**——首先我们需要一个通用的BRDF来求解，它得是适合所有的形式

        - **微面元的BRDF**

          - ![image-20241031224252549](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031224252549.png)

          - ### F（i,h）

            - 入射，半程。在这里我们是用Schlick近似的方式：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031224532639.png" alt="image-20241031224532639" style="zoom:67%;" />，近似结果如图<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031224545806.png" alt="image-20241031224545806" style="zoom:67%;" />

            - **R~0~**——初始反射率

          - ### D（h）

            - 主要是控制法线分布，表现为控制类似于**粗糙度**的玩意**（用这个就可以实现高低频信息的切换）**。原理上是控制法线分布的开不开。集中的话就接近镜面反射，分散的话就接近漫反射分布
  
            - #### Beckmann分布<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031224843932.png" alt="image-20241031224843932" style="zoom:67%;" /><img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031225010072.png" alt="image-20241031225010072" style="zoom: 50%;" />
  
              - **α**——定义胖瘦（就那个蓝色的球的胖瘦<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031225031762.png" alt="image-20241031225031762" style="zoom:33%;" />)
              - **θ~h~**——半程和法线的夹角（可以自行修改成和入射角有关的函数，反正这些都是可以很方便转换的，近似手段都行）

          - #### **三维的预计算**（三个颜色的方框都是变量）还是太麻烦了，想办法变成二维的↓
  
            - **想法与观察**
  
              - 尝试再次分离变量！
              - Schlick 近似的菲涅耳项更简单：只需“基础颜色” \( R<sub>0</sub> \) 和半角θ

            - **将 Schlick 近似应用于第二项**
  
              - **“基础颜色”**被提取出来！
  
              $$
              \int_{\Omega+} f_r(p, \omega_i, \omega_o) \cos \theta_i \, d\omega_i \approx R_0 \int_{\Omega+} \frac{f_r}{F} \left(1 - (1 - \cos \theta_i)^5\right) \cos \theta_i \, d\omega_i +
              \int_{\Omega+} \frac{f_r}{F} (1 - \cos \theta_i)^5 \cos \theta_i \, d\omega_i
              $$

            - ![image-20241031230042085](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031230042085.png)

            - 将Schlick的公式进行显式地调用，如此做来**R~0~**就可以被放到外面去，降维成功

            - 菲涅尔项不再需要依赖**R~0~**了
  
            - 然后对仅剩的两个维度的值进行打表，需要时进行查询即可<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031230312899.png" alt="image-20241031230312899" style="zoom:67%;" />，θ~v~当做**入射角θ~i~**即可，因为我们一直带着cosθ~i~嘛，给它扯关系，计算也方便。只要是种类相同，这个预计算出来的图就是固定的，对于各种各样的微面元BRDF都可以应付。
  
              - **cosθ~i~：**我们一直带着，作为查表使用
              - **Roughtness**：也是直接查表，这样D（h）里面的α是知道的。==**——就用这个来进行高低频信息的切换**==

            - 把这个式子带入进去：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031224252549.png" alt="image-20241031224252549" style="zoom:50%;" />，然后就发现F项被约掉了，积分里面和R~0~没有什么关系了。实际上要关注的就是G（i,o,h）和D（h）。说是，G项也只有粗糙度和角度两个维度，如果D（h）项里面的α作为粗糙度的话，那确实也是只有两个维度。用两个维度就能计算出来<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031231559862.png" alt="image-20241031231559862" style="zoom: 50%;" />这个积分的近似值。
            
            - **cosθ~i~**：可以认为是BRDF项，所以一直带着
  
        - 之后需要计算shaing point的实时环境光照的时候就再也不需要采样了，需要对应的点直接查表即可代如积分进行预计算。
  
        - ### 实时计算领域不会写积分——split sum
        
          - 用求和符号![image-20241031231843818](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241031231843818.png)
          - 叫做**split sum**而不会是split integral
          - 对于一些**高频和低频**的光照信息都能表现特别好——
          - ![image-20241101125401995](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101125401995.png)
  



- ### Preconputed Radiance Transfer（PRT）

  - **总结**
    - 来自环境光的阴影
  - **背景知识**
    - 频率与滤波
    - 基函数

  - **实时环境光照（和全局光照）**
    - **球谐函数（Spherical Harmonics，SH）**——暗部提亮
    - 预滤波的环境光照
    - 预计算辐射传输（Precomputed Radiance Transfer，PRT）

- ### 从环境光照实时生成阴影（AO）

  - 通常情况下，实时渲染**==非常困难==**
  - 不同视角的看法：
    - 作为**多光源问题**：
      - 阴影映射（SM）的成本与光源数量成线性关系
      - 原本就是特别多光，每个光一个阴影那还得了。而且还是线性增加的
    - 作为**采样问题**：
      - 可见性项 \( V \) 可能会变得极其复杂
      - 并且 \( V \) 无法轻易从环境中分离出来
  - AO就是从环境光生成出来的阴影，但它是默认从**全白或者全灰**的环境光下产生的阴影，实际上是不怎么准的。

- ### **工业解决方案**

  - 从最亮的光源生成一个（或稍多一些）阴影

- **相关研究**（感兴趣可以去搜索）
  
  - 不完美阴影映射（Imperfect Shadow Maps）
  - 光切割（Light Cuts）
  - 实时光线追踪（RTRT，<u>可能是最终解决方案</u>）
  - ==**预计算辐射传输（Precomputed Radiance Transfer）**==

 

- ### 对于filter的通识

  - 任何乘积积分都可以看作是一种滤波

    $$ \int_{\Omega} f(x)g(x) \, dx $$

  - 低频 == 平滑函数 / 缓慢变化 / 等等

  - 积分的**频率**是各个函数中**最低的频率**

- ### **基函数**（Basis Functions）

  - 一组可以用来表示其他函数的函数集

    $$ f(x) = \sum_i c_i \cdot B_i(x) $$

  - 傅里叶级数是一组基函数（全局近似，泰勒展开属于局部近似）
  - 多项式级数也可以是一组基函数



### Spherical Harmonics(SH球谐函数)

- **我的理解：**

​	==核心作用在于**高效地存储和重现低频的光照信息**==

![v2-7bc5ba617f7f08924635ac864fff6316_1440w](https://raw.githubusercontent.com/nininias/image-repo/main/img/v2-7bc5ba617f7f08924635ac864fff6316_1440w-1730437443657-2.png)

​	*——这张图也反映了为什么求谐只适合用来存储低频信号。球谐函数的基本性质决定了它在表示低频信号时具有良好的表现力和计算效率。高频信号（如细致的阴影和高对比光照）需要更高的阶数才能准确重建，但这会导致性能和存储开销大幅增加。因此，在实际应用中，球谐函数通常只用于低频光照的近似。*

​	——如果想要存储高频光照信息，就需要使用适合高频细节的技术。例如：

- 光照贴图（Lightmaps），反射探针，屏幕空间反射，阴影映射（Shadow Mapping）
- IBL技术技能够处理**低频光照信息**，也能够处理**高频光照细节**，适用于模拟复杂的环境光照。



------

**课程内容：**

- 一组在球面上定义的二维基函数 \( B~i~ω)

- 类似于一维中的傅里叶级数

- 三维空间用二维的角度θ和φ就可以描述，所以我们就用这两个值来记录一个球谐函数的值。

- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101140336821.png" alt="image-20241101140336821" style="zoom:50%;" />

  - L表示的是阶数，也就是频率。频率越高越能拟合原来的函数，计算量也越大。表示那个球的拉伸严不严重。
  - 同一频率，计算的函数的类型不同。越往蓝色偏亮的地方去，值越大；反之绝对值越大
    - 同一频率有2 * l + 1个类型的函数
    - 前n阶一共有n^2^个
  -  它本身就很连续，很适合用来分析球面上的性质。如果是展开成平面再映射，会有条线

- 每个球谐基函数 \( B~i~ω\) 都与一个（勒让德）多项式相关联。

- **投影：**获取每个球谐基函数的系数
  ![image-20241101142025322](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101142025322.png)
  
  - f（w）——任意一个**球面函数**（环境光照就是一个球面）

  - B~i~（w）——基函数。傅里叶变换里面的<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101143334979.png" alt="image-20241101143334979" style="zoom:50%;" />这里表示的一系列函数的表示方式
  
  - c~i~——常数，每项有每项的对应的常数
  
- **重构：**使用（截断的）系数和基函数来还原原始函数。

  *通常说的都是用“前多少阶的基函数我都用”*  



### PRT

#### PRT相关解释

- ### Prifiltering

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101150746077.png" alt="image-20241101150746077" style="zoom: 50%;" />
    - 对于filter之后的单次查询等价于没filtering的多次查询

- ### **解析辐照度公式**

  -  **漫反射 BRDF 起到低通滤波器的作用**
  - **==E<sub>lm</sub> = A<sub>l</sub> L<sub>lm</sub>==**
    - 两个函数点乘求积分，就类似于在求filtering。就用环境光去照亮diffuse基本看不到什么高频信息。
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101151440951.png" alt="image-20241101151440951" style="zoom:50%;" />——**对于BRDF项**（这里说的不是SH的阶数）大概只需要3阶就能把图像恢复的很好。那就类似于低通滤波的作用。



- ### 9个参数近似

  - 那既然BRDF用3阶就可以还原的很不错，那Light的阶数还原也用那么少阶数行不行？——行！

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101152106705.png" alt="image-20241101152106705" style="zoom: 67%;" />

  - **描述<u>光照</u>**，用三阶表示效果分别是：

    <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101152128822.png" alt="image-20241101152128822" style="zoom:50%;" /><img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101152143076.png" alt="image-20241101152143076" style="zoom: 50%;" /><img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101152215397.png" alt="image-20241101152215397" style="zoom:50%;" />

  - 得出结论——

    - ## **==对于任何BRDF的Diffuse，都可以用3阶的SH来描述光照==**
    



#### Real-Time Rendering（FYI/仅供参考）


简单的程序化渲染方法（无需纹理）
- 仅需要矩阵-向量乘法和点积运算
- 可在软件或 NVIDIA 顶点编程硬件中实现



#### 简要总结

- **我们已经了解了基函数的用途**
  - 表示任意函数（只要有足够多的基，就可以表示任意的函数）
  - 保持特定频率内容（使用较少的基，取某一些阶数的即可表示）
  - 将积分简化为点积（如果两个函数都是拿sh的两个基函数来表示的，那他们的乘然后积分实际上就是两个基函数的点乘）

- **但目前仍然是来自环境光照的着色**
  - 还未涉及阴影，要引入阴影，不能只shading而不shadows

- **接下来：预计算辐射传输（PRT）**
  - 处理阴影和全局光照！
  - 成本是多少？开销是多少？副作用？

#### Rendering under environment lighting

- ### 在环境光照下渲染

  $$
  L(o) = \int_\Omega L(i) V(i) \rho(i, o) \max(0, \mathbf{n} \cdot \mathbf{i}) \, \text{d}i
  $$

  - **lighting**  
    环境光照图像![image-20241101162326106](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101162326106.png)

  - **visibility**  
    可见性遮挡信息![image-20241101162331966](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101162331966.png)

  - **BRDF**  
    反射率分布函数（BRDF）![image-20241101162338827](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101162338827.png)

  - **i/o**: 入射/观察方向
  - **蛮力计算**
    - 分辨率：6 × 64 × 64
    - 每个点需要计算 6 × 64 × 64 次！



- ### 预计算的基本思想

  $$
  L(o) = \int_\Omega L(i) V(i) \rho(i, o) \max(0, \mathbf{n} \cdot \mathbf{i}) \, \text{d}i
  $$

  - **lighting**：光照信息
  - **light transport**：光传输
  - 公式中，**L(o)** 表示物体表面点 o的光照。
  
    **L(i)** 是从方向 i入射的环境光。——由于**在漫反射中各个方向的光照强度是相同的，因此在漫反射的计算的时候这个值被当做了常数C~i~**
  
    ​	——漫反射下![image-20241104154555905](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104154555905.png)
  
    ​	<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104154603350.png" alt="image-20241104154603350" style="zoom: 67%;" />
  
    **V(i)** 是可见性函数，表示从点 o沿方向 i是否有障碍物。
  
    **ρ(i,o)** 是反射率函数（BRDF），表示光从 i到 o的反射特性。
    
    **max⁡(0,n⋅i)** 是光线和表面法线 n的夹角的余弦值，用于判断入射光是否照射到表面。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101163658087.png" alt="image-20241101163658087" style="zoom:50%;" />
    
    - 分成两个部分，一个部分是lightin，一个部分是light transport。
    - **左边**应用在只变动光源所带来的积分影响；
      - **lighting**——再拆成basis funcions（基函数）
    - **右边**计算其在不同入射光方向下的光照响应，包括自阴影、次表面散射等复杂光照现象。将**上述光照传输函数**投影到一组基函数（如球谐函数）上，以压缩和表示这些高维数据。
      - **除了light可以变，其他所有东西都不变的情况下**——相当于每个shading point自己的性质，是不变的，那也就是说这部分是可以**渲染之前提前算好**的。
  
  ---
  
  - **使用基函数近似光照**
    - **$$ L(i) \approx \sum l_i B_i(i) $$**
    
  - **预计算阶段**
    - 计算光传输，并投影到基函数空间
    
  - **运行时阶段**
    - 点积运算（漫反射）或矩阵-向量乘法（光泽）
  
  
  
  
  

#### Diffuse Case

  - 首先肯定是满足<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101170031007.png" alt="image-20241101170031007" style="zoom:33%;" /> 
  - 我们将此代入<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101170049558.png" alt="image-20241101170049558" style="zoom: 50%;" />，这是我们的<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101170132858.png" alt="image-20241101170132858" style="zoom:67%;" />——傅里叶变换的表达式。系数乘基函数然后累加。
    - 得到<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101170245926.png" alt="image-20241101170245926" style="zoom: 50%;" />，L~i~是个系数可以直接往外提。先积分后累加还是先累加后积分在图形学里面基本不考虑。然后右边的实际上就是我们刚才说的light transport。也就是我们刚刚说的“常数”。为什么它是个“常数”？因为他是可以被预计算，提前积分然后记录下来的（前提是场景得是静止的）
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101170617329.png" alt="image-20241101170617329" style="zoom:67%;" />——把预计算写成**T~i~**，我们会得到更加简洁的式子。我们的渲染只需要一个点积就可以把光照给算出来（好多限制条件，漫反射，静止等）。
  - ![image-20241112014553315](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241112014553315.png)
    - 这个是算**每个基函数的系数用的，用到了采样点的光照信息投影到基函数上的值**




- ### 基函数**B(i)**

  - **球谐函数** (Spherical Harmonics, SH)
  - **球谐函数的优点**：
    
    - 正交归一 (orthonormal)
    - 简单的投影/重建 (simple projection/reconstruction)
    - <u>简单的**旋转 (simple rotation)**</u>——所以对于场景中我们的光照是可以随意旋转的，不影响预计算的值，而且也可以快速得到旋转后不同方向的shaing point的计算
    - 简单的卷积 (simple convolution)
    - 少量基函数：适合低频信号 (few basis functions: low freqs)
    
  - 对于**简单的旋转**

    - 我对于物体的旋转，就相当于是对于basis函数的旋转。
    - 而如果是对basis函数进行旋转，我们可以使用同阶数的其他basis函数进行线性组合即可得到原来的basis函数。

  - **SH是一组正交基**

    - 有这样的性质，投影到自己就是1，投影到其他基函数就是0.![image-20241101172412582](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101172412582.png)
    - **还原效果如图**

    - ![image-20241101172244504](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101172244504.png)

  - ### ==重建方式==

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101173112519.png" alt="image-20241101173112519" style="zoom:50%;" />——这个是原始的光照信息场景，通常表示一个全景或者cubemap，为**Environment map**。

      - 他的**构建**方式是<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101173305064.png" alt="image-20241101173305064" style="zoom:50%;" />，在这里我们的**l~i~**就是我们的光照参数（标量），他需要我们通过<u>**预计算**</u>（↓）获得。将预计算的值投影到基函数，作为贡献值进行累加。
        - 预计算——**主要目的是计算每个球谐基函数的投影系数 $L_i$，即如何通过环境光分布来确定这些球谐系数。**一共9个的话，那就得针对**9个基函数计算对应的系数$L_i$,**这个数得**根据已定的环境（Cubemap）**来进行**各方位的采样与积分**（累加），是这么说的：**==将环境光 $L{(i)}$投影到球谐基函数 $B{i}$上，计算出各个基函数的系数  $L{(i)}$​==**
    
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101173403202.png" alt="image-20241101173403202" style="zoom: 33%;" />——这个就是我们的**l~i~**了，我们的参数存储之后就长这样。通过积分将环境光数据投影到 SH 空间，计算出一组 SH 系数（lighting coefficients）,每个系数 **l~i~**对应一个 SH 基函数 **B~i~(i)**，每个都是三维的。
    
      - 当我们拥有一张Environment map之后，我们是通过该方式进行值存储的——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101173647848.png" alt="image-20241101173647848" style="zoom:50%;" />。
    
      - ![image-20241101174954622](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101174954622.png)
    
      - 球上找一个点，都可以通过这个方程来计算该方向上的SH系数。
    
      - **重建的时候代入指定方向的环境光向量进去，针对每个方向的光照强度累计存到了对应的9个位置，当需要对指定方向进行重建的时候，只需要对基函数进行指定方向向量代入，然后对结果进行<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101173305064.png" alt="image-20241101173305064" style="zoom:50%;" />or<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241112014918841.png" alt="image-20241112014918841" style="zoom: 50%;" />（△w指的是采样点的立体角权重），全部累乘然后加起来，就是重建某个方向的光照了。计算量主要集中在预计算的$L_i$**
    
      - ### 实时渲染
    
        $$L(o) \approx \rho \sum l_i T_i$$
      
        - 每个点的渲染被简化为一个点积
          - ==**首先，将光照投影到基函数以获得 \( l~i~ \)**==
          - 或者，旋转光照而不重新投影
          - 然后，计算点积
    
        - 实时：可以轻松地在着色器中实现
      
      - ![image-20241101175340727](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101175340727.png)

- **Light**视为：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101195814072.png" alt="image-20241101195814072" style="zoom:50%;" />

- **Light Transport**视为：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101195824955.png" alt="image-20241101195824955" style="zoom:50%;" />

- 将其带回我们的光照计算函数为：![image-20241101195952539](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101195952539.png)

  - 之前提到，两个函数点乘求积分，就相当于一个函数在另一个函数上的投影。

  - 既然如此，则右边为B~p~(w~i~)或B~q~(w~i~)在对方的投影，**积分**得到的是**投影的系数**是多少。

  - 如果两个基向量想投影，也就只有p = q的情况下，右边才有值，否则结果为0.(基函数特性)

  - 满足p = q 的情况只有在对角线上

  - #### 为什么这是一个点积？（这看起来是 \(O(n^2^)\) 而不是 \(O(n)\)？）

    -  提示：这是球谐函数 (SH) 的一个特性
    - 因为只有p = q 的情况才有值



#### Glossy Case

<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101210504311.png" alt="image-20241101210504311" style="zoom:50%;" />   

-   由于这里的**L(i)**和漫反射的不同，这里的光照强度是会随着视角的变化而变化的，因此这个时候的**L(i)**不能像diffuse case一样当做一个常数C~i~来对待，而是要**当做一个关于观察方向的函数**。

- 此时的**ρ(i,o)**由于不同的出射角度有不同的入射角度，所以从二维(ρ，θ)的考虑情况升级成四维(出射(ρ，θ)，入射(ρ，θ))的情况。（因为是可逆的，传入某个方向就传出另一个方向）

- 对于投影生成与否，就在这个**V（i）**上

- 这个函数就是把入射方向的函数投影到了出射方向上的函数（四维降成了二维）

  - 把这个通过二次定位的常数（矩阵）投影到基函数<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101211831269.png" alt="image-20241101211831269" style="zoom:50%;" />——得到反射辐射系数。

  - 代入可得<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101211858769.png" alt="image-20241101211858769" style="zoom:50%;" />

  - ![image-20241101212258942](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101212258942.png)

  - **反射辐射系数（Reflected Radiance Coefficient，系数*矩阵）**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101213633719.png" alt="image-20241101213633719" style="zoom:50%;" />：这是我们最终需要计算的目标，表示在某个表面点上反射出来的辐射亮度。该系数通过光照信息和传输特性计算得到。

    **光照系数l~i~（Light Coefficient，中间那个记录的）**![image-20241101212757883](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101212757883.png)：这是基于球谐函数等基函数投影得到的光照信息的分解结果。它表示了在不同基函数下的光照强度，这些光照系数可以在预计算阶段获得。

    **传输矩阵t~ij~（Transport Matrix,右边那个传输矩阵被可视化了）**![image-20241101213647635](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101213647635.png)：这个矩阵表示从入射光到达表面点再反射出射的传输特性。传输矩阵包含了不同方向上光的传输信息，例如入射角、反射率和遮挡关系等。

    **向量-矩阵乘法**：在渲染过程中，反射辐射系数可以通过对光照系数和传输矩阵进行向量-矩阵乘法计算得到。这种计算方式可以简化复杂的积分运算，将其转化为线性代数问题，从而加快实时渲染的计算速度。

    ![image-20241101213510830](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101213510830.png)——这个就那个直接查表的基函数。

    然后对这几个点乘累加就有得到出射辐射量![image-20241101213719752](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101213719752.png)

- ### 缺点

  - 向量和矩阵乘要比向量和向量的乘要复杂
  - 阶数大的时候存储矩阵要很大来存（因为glossy就是要很高阶数的）
  - 特别高频效果不好，理论可行实际不能实现（接近镜面反射的情况，不如直接采样）

- ### 消耗总览

  - ![image-20241101214057894](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101214057894.png)

- ### 效果（03年）

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241101214121474.png" alt="image-20241101214121474" style="zoom:50%;" />

- #### 如果是不单单存储环境光的SH，还存储自身的反射出来的光的SH如何实现？

  - 我们可以将任意复杂的transport都给预计算出来

  - **实时运行的时间与传输复杂度是相互独立的**

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102013648398.png" alt="image-20241102013648398" style="zoom:50%;" />

    - L：从light来的光
    - E：传到眼睛里
    - G：Glossy
    - D：Diffuse

  - #### 如何计算存储的带有反射的SH？

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102013737554.png" alt="image-20241102013737554" style="zoom:50%;" />
      - 把**Light transport部分投影到SH**上面去
      - 如果把这个<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102014024308.png" alt="image-20241102014024308" style="zoom: 33%;" />换成**Li（i）**，那张的不就像是拿一个球形的光照来渲染这个场景一样了 
    - 我们的预计算之前都是在球上来可视化的，如果是物体上
      - ![image-20241102014211155](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102014211155.png)
      - 我们所预计算的Transport lighting的部分，就好像是对于物体提前打了一个奇奇怪怪的光上去然后进行预计算。预计算手段不限，能出来就行，反正都是预计算，不是实时计算所关心的。
      - 这里是有负数的（好像蓝色是负数吧？——老师说是蓝色）

  - ### 效果预览

    - ![image-20241102014518692](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102014518692.png)



- ## 总结

  - 使用基函数（SH）近似光照和光传输
    - 光照 -> 光照系数
    - 光传输 -> 系数 / 矩阵
  - 预计算并存储光传输
  - 渲染简化为：
    - **Diffuse：点积**
    - **Glossy：向量矩阵乘法**

- ### 局限性

  - **适合低频**
    - 由于需要特别高频才能表示Glossy的表示结果，实际上往往更适用于表示低频结果。
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102014941484.png" alt="image-20241102014941484" style="zoom:33%;" />——26阶才能像那么回事
  - **必须是固定场景（因为预计算就要求固定），但可动态光**
    - 如果预计算了材质，那就连材质也不能改变
  - **存储数据量大**

- ### 相关研究

  - 更多基函数 (Basis Functions)

  - 点积 (dot product) => 三重积 (triple products)
  - 静态场景 (Static scene) => 动态场景 (dynamic scene)
  - 固定材质 (Fixed material) => 动态材质 (dynamic material)
  - 其他效果：半透明 (translucent)、头发 (hair) 等
  - 预计算 (Precomputation) => **解析计算 (analytic computation)**

    - 之前有提到split sum就是解析计算，轻量级
    - 预计算可以理解为物理材质的一部分

  - ### 更多的基函数

    - 球谐函数 (Spherical Harmonics, SH)
    - 小波 (Wavelet)
    - 带谐函数 (Zonal Harmonics)
    - 球面高斯函数 (Spherical Gaussian, SG)
    - 分段常数 (Piecewise Constant)

- #### Wavelet（简单记）

  - 2D Haar 小波 (Haar Wavelet)

  - 投影：
    - 小波变换 (Wavelet Transformation)
    - 保留少量非零系数

  - 一种非线性近似

  - 全频率表示

  - #### 近似表现

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102020324475.png" alt="image-20241102020324475" style="zoom:50%;" />→<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102020404100.png" alt="image-20241102020404100" style="zoom:50%;" />
    - 不断拆分高低频<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102020422621.png" alt="image-20241102020422621" style="zoom:50%;" />（方式有点像JPEG格式）

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102020510712.png" alt="image-20241102020510712" style="zoom:50%;" />

  - ### 局限性

    - 小波不支持快速旋转（如果转了得重新解开生成再投影）



# Lec 7/8/9 Global Illumination(in 3D)

## Real-Time Global Illumination<u>(in 3D)</u>

- **Reflective Shadow Maps(RSM)**
- **Light Propagation Volumes(LPV)**
- **Voxel Global Illumination(VXGI)**



- ### 图像空间的有

  - SSAO
  - SSDO
  - Screen Space Reflection（SSR）



- 在RTR里面，人们更像通过简单采样快速解决1 bounce的间接光照

- ### 初步理解建立

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102141947006.png" alt="image-20241102141947006" style="zoom:33%;" />一个光源打到Q，咱认为Q就是一个自己光源发射光去照亮点P（对点P而言），即——**已经被直接光照照亮的物体表面，会作为光源去照亮别人**
  - RSM用的就是这个思路



### **一、Reflective Shadow Maps(RSM)**

​	**被shadow map所看到的，即直接光照所能照射到的物体，视为次级光源去照亮其他物体的方法。**

- #### Key Observations

  - 要通过间接照明照亮任意点 *p* 需要什么？
  - **Q1**: 哪些表面片段直接被照亮？
    - 提示：什么技术可以告诉你这个信息？——Shadow map

  - **Q2**: 每个表面片段对点 *p* 的贡献是多少？
    - 然后把所有表面片段的贡献相加
    - 提示：每个表面片段就像一个区域光——我们之前有学过对点的积分转移到对区域光的积分

- ### 新问题

  - **Q1**: 哪些表面片段是直接被照亮的
    - 使用经典的阴影图(Shadow Map)可以完美解决
    - 阴影图(Shadow Map)上的每个像素都是一个小的表面片段
  - 每个像素的精确出射辐射度已知
    - 但只针对朝向摄像机的方向

  - 假设
    - 任何反射体都是**漫反射(Diffuse)**
    - 因此，**出射辐射度**在所有方向上都是**均匀**的

- ### 回顾

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102144118952.png" alt="image-20241102144118952" style="zoom:50%;" />
    - 左边单位立体角所接受到的辐射度
    - 中间是单位面积所接受到的辐射度
    - 右边是单位立体角单位面积所接收到的辐射度 

- ### 做法

  - **问题2：每个表面片对点 p 的贡献是什么？**
    - 对表面片所覆盖的立体角进行积分
    - 可以转化为对表面片面积的积分
  - 我们就用**积分变量换元**，不在shading point积分，而是在光源积分，换元之后公式如下<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102150717317.png" alt="image-20241102150717317" style="zoom:67%;" />
  - ![image-20241102151038332](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102151038332.png)

  - #### 对于点q

    - 点q的BRDF为 ——  **f~r~ = ρ/π**
    - 对于出射Radiance（已知O~Radiance~ = f~r~ * I~Radiance~）,有**L~i~ = f~r~ * φ/dA**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102151710405.png" alt="image-20241102151710405" style="zoom:50%;" />——φ就是入射光通量，这是由**光源决定**的（我们这里说的就是1 bounce的光），非标准定义，得出来的**L~i~**就是**反射flux**——主要是要这个**Reflective flux**，咱这个方法就叫**Reflective Shadow Maps**
    - **V(p,w~i~)**——对于q点能不能看到点p，**搞不定，不算**

  - ### 最终有

    - ![image-20241102213447102](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102213447102.png)
      - 这个**φ~p~**是**f~r~ * φ**

- ### 注意事项

  - 在RSM里面并非所有的像素都有贡献

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102152840133.png" alt="image-20241102152840133" style="zoom:50%;" />
    - 像某些自己本身不会被照到的
    - 离x很远的（我们不能判断这些可见的x能否对x可见，我们一律认为可见）

  - #### 计算贡献

    - 不是把所有RSM里面的点都遍历计算贡献，而是**在shadowmap附近某个范围内采样一下**即可
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102153153555.png" alt="image-20241102153153555" style="zoom:33%;" />——paper提到采样的权重分布

- ### 如果是RSM，光源需要存储哪些值？

  - 深度
  - 世界坐标
  - 法线
  - 光通量等
  - ![image-20241102153410072](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102153410072.png)

- ### 应用场景

  - **手电筒**
  - ![image-20241102153427537](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102153427537.png)

  - 小细节，有一些地方的会接收到手电筒的次级光照

- ### 归类

  - 3D空间信息

- ### 优缺点

  - **优点：**
    - 好算，容易实现
  - **缺点：**
    - 性能随着直接光源数的增加而降低(因为需要计算更多的shadow map)
    - 对于间接光照，没有做可见性检查，无法判断次级光源的当前shading Point 的可见性关系
    - 做了很多假设：反射物需要是漫反射的，shadowmap的距离当做深度
    - 需要在质量和采样率之间做平衡



### 二、Light Propagetion Volumes(LPV)

预生产场景里面看不见的格子用来存储Radiance，不断像周围六个面进行传播，shading point取网格里的Radiance来渲染

- ###  为什么要学？

  - 又快质量又好（但难写）
  - 一定程度环节RSM所带来的问题
  - 对Glossy的接收物也能进行反射

- #### 主要问题

  - 在所有的shading point都要收集周围的所有方向的辐射度

- #### 概念区分

  - **radiance（辐亮度）**在传播过程中是不发生改变的（之前图的最右边的线）
  - **Irradiance（辐照度）**或**Intensity（光强）**会厚道距离平方衰减的影响

- #### 主要思想

  - Radiance在直线传输过程中不发生改变

- #### 主要解决方式

  - 使用3D网格将辐射度从直接光照表面传播到其他任何地方<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102163227252.png" alt="image-20241102163227252" style="zoom: 50%;" />

- ### **步骤**  

  1. 生成辐亮度点集场景表示（radiance point set scene representation）  
  2. 将虚拟光源点云注入到辐亮度体积（radiance volume）中  
  3. 进行体积辐亮度传播（volumetric radiance propagation）  
  4. 使用最终的光传播体积（light propagation volume）进行场景光照

- ### 步骤详解

  1. **产生虚拟光源**——就用**RSM**来照到每一个表面上可见的次级光源，可以**自行增减次级光源的数量**以平衡质量与渲染速度
  2. **注入虚拟光源**——3D空间预先划分3D存储格子，用来存储SH，然后将各个点朝各个方向的光都存储在格子里<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102165220870.png" alt="image-20241102165220870" style="zoom:33%;" />。对于每一个格子照到最近的虚拟光源，把间接光全部加起来。SH用前两阶就够了。
  3. **传播**——（生物学里Propagetion 是<u>繁殖</u>的意思，在这里用这个理解感觉有意思点）传播到六个面去（保持Radiance在直线传播不变原则），然后沿途不断收集原来就有的Radiacne，贡献过去然后加起来。直到整个网格平稳下来（迭代个四五次就稳定下来了）。估计是这里，传播到离次级光源很远的时候，原本很聚集的次级光源已经被散开来了，所以到达比较远的shading point的时候已经是很少很少了。
  4. **渲染**——照到对应的shading point的gird cell（对应生物学里有细胞的意思，可以说是网格单元）所存储的Radiance，直接渲染（默认在单位格子上Radiance是不变的，这会带来问题）。

- ### 带来的问题

  - Light Leaking——上面刚刚讲了在单位格子上认为Radiance是相同的，那就会出现自己照亮自己的一种情况<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102170130766.png" alt="image-20241102170130766" style="zoom:50%;" />
  - 仍然不考虑格子到下一个格子能否看到（visibility）

- ### 解决方法

  - 细分更多的格子
    - 存储量更多
    - 传播时要考虑的格子数更多了
  - 自适应格子划分（工业界多用<u>cast cat</u>？ LOD）

- ### 补充

  - 每一次迭代就是像周围六个面传播次数
  - 迭代多次肯定能稳定下来
  - 对Glossy的接收物一样生效
  - 这个是实时的计算
  - 每个格子存的相当于就是Light Transport（我们SH存的就这玩意）



### 三、Voxel Global Illumination（VXGI）

- ### 特性

  - 和RSM一样，也是两趟（2 pass）

- ### 区别

  - ##### 和RSM的区别

    - RSM是像素级别的，VXGI是**格子**级别的（提前离散化，大小自定义，可动态划分，从而建立出一棵树）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102173949551.png" alt="image-20241102173949551" style="zoom:33%;" /><img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102175036815.png" alt="image-20241102175036815" style="zoom:33%;" />
    - 需要在RSM的基础上采样，反射出来的不是一根光线，而是一个锥

- ### 步骤

  - #### Pass1——From Light（存）

    - 从光线出发，记录直接光源从哪些范围过来（绿色部分），记录各个反射表面的法线（橙色），可以准确计算出出射方向的分布（应该是作为下一个层级的“光源”）
    - 在后面摄像机出发的时候再反算，然后贡献值记录
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102175734026.png" alt="image-20241102175734026" style="zoom:67%;" />

  - #### Pass2——From the Camera（找）

    - 对于任何一个像素，相当于知道了Camera Ray的方向
    - **Glossy：**向反射方向追踪出一个锥形（cone）区域

    - 基于追踪出的圆锥面的大小，对格子的层级进行查询（越远，锥体越大，层级越低，越大，省时），就是对于场景中的所有体素都要判断是不是与这个锥形相交，如果相交的话就要把对于这个点的间接光照的贡献算出来。
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102181714090.png" alt="image-20241102181714090" style="zoom:50%;" />
    - **Diffuse**：VXGI 通常会将出射方向看做若干圆锥，而忽略锥与锥之间的间隙。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102180903994.png" alt="image-20241102180903994" style="zoom: 33%;" />

- ### 补充

  - LPV是把所有的次级光源发出的Radiance传播到了场景中的所有位置，只需要做一次从而让场景每个Voxel都有自己的radiance，但是由于LPV使用的3D网格特性，并且采用了SH进行表示和压缩，因此结果并不准确，而且由于使用了SH因此只能考虑diffuse的,但是速度是很快的。

    VXGI把场景的次级光源记录为一个层次结构，对于一个Shading Point，我们要去通过Corn Tracing从摄像机出发，找到哪些次级光源能够照亮这个点。

- ### 效果

  - **很不错的效果，很接近ray tracing**
  - 但是开销很大
  - 体素化需要预处理
  - 动态实时体素化就很慢
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102182328814.png" alt="image-20241102182328814" style="zoom:50%;" />



## Real-Time Global Illumination（screen space）

- **屏幕空间环境光遮蔽**（Screen Space Ambient Occlusion，SSAO）
- **屏幕空间方向遮蔽**（Screen Space Directional Occlusion，SSDO）
- **屏幕空间反射**（Screen Space Reflection，SSR）



### 屏幕空间

- 屏幕上能看到什么是什么信息
- 比如如果在沙发背后有个红色的什么玩意屏幕上看不到，周围是不会反射出沙发背后的红色的玩意的



### **屏幕空间环境光遮蔽**（Screen Space Ambient Occlusion，SSAO）

- ### 闭塞![image-20241102194247188](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102194247188.png)

- ### SSAO是什么

  - 全局光照的近似（立体感）
  - 在屏幕空间
  - dcc软件里面称为天光

- ### 主要想法

  - ①
    - 我们不知道间接光是什么
    - 假设间接光是个常数
    - 就好像Blinn-Phong提到的<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102194629515.png" alt="image-20241102194629515" style="zoom:50%;" />
  - ②
    - 对于每一个不同的shading point开始要考虑visibility项（朝all directions）了
    - 但是**仍然假设是一个diffuse物体**
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102194851802.png" alt="image-20241102194851802" style="zoom:33%;" />

- ### 分析

  - 还是从rendering equation开始![image-20241102195256990](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102195256990.png)
  - 老师提到的RTR近似方程（课堂上用的）![image-20241102195348263](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102195348263.png)
  - 按照约等式将Visibility独立开来，得到<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102200002169.png" alt="image-20241102200002169" style="zoom:50%;" />
    - 蓝色方框里面为<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102200410378.png" alt="image-20241102200410378" style="zoom:50%;" />（cosθ~i~也被带出来了，它被积了之后是π）
      - 每个点从各个方向的权重累积（根据cosθ~i~的调整来选择权重），标准的加权平均
    - 黄色方框里面为<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102200526270.png" alt="image-20241102200526270" style="zoom:50%;" />
      - 由于漫反射的入射辐射度和反射率都可自定义，所以这个就是自定义颜色和反射率

- ### 更深入的分析

  - 一
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102200921303.png" alt="image-20241102200921303" style="zoom:33%;" />前面做的K~A~项实际上就是加权平均
    - 而且因为g(x)是一个常数，漫反射 * BRDF，很平滑
    - 所以这是一个很标准的近似，误差很小
  - 二
    -  为什么**cosθ~i~dω~i~**积出来是个π呢？
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102201535854.png" alt="image-20241102201535854" style="zoom: 33%;" />
    - 这其实是有个明确定义的Projected solid angle **dx~⊥~**（$$ \mathrm{d}\omega_{i} $$）
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102201628131.png" alt="image-20241102201628131" style="zoom:50%;" />
      - 表现在图片上就是把立体角（单位立体角就是单位球上的面积）投影到**单位圆**上，并且对单位球的面积积分得到的**面积就是π**（πr^2^）
        - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102201705831.png" alt="image-20241102201705831" style="zoom:67%;" />

- ### 正常地理解

  - 其实，这是一种更简单的理解

    - 均匀入射光——**L<sub>i</sub>** 是常量

    - 漫反射**BSDF-\( f<sub>r</sub> = <sup>ρ</sup>&#8260;<sub>π</sub> \)** 也是常量

    - 因此，将二者从积分中提取：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102204412235.png" alt="image-20241102204412235" style="zoom:50%;" />
      - 留了一个π在外面和K~A~的分母π相抵消了


- ### 如何实时计算**K~A~**的值？

  - 在物体空间（object space）
    - 针对几何体进行光线投射
    - 较慢，需要简化和/或空间数据结构
    - 依赖场景的复杂度

  - 在屏幕空间（screen space）
    - 在后期渲染过程中完成
    - 不需要预处理
    - 不依赖场景的复杂度
    - 简单
    - 物理上不准确

  - ### 近似算法（工业界）

    - 会假设有一个有限的很小的一个半径，半球局部闭塞

    - 效率高，效果好

    - ![image-20241102210127581](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102210127581.png)

    - 随机选择采样点，根据每一个shading point的深度缓冲作为场景几何的深度近似值，然后再每个像素周围的球体中采样，然后对缓冲进行测试（有个点有问题，但是我们工业界能接受）

    - 屏幕空间能拿到的信息很少，有个深度图很不错了（以前拿不到normal）

    - ### 增加一个新的判断条件

      - 当且仅当，红色点的数量（遮挡点）大于一个shading point的采样点的一半的时候，才开始考虑AO的问题
      - 而且只考虑未被遮挡的一半的球里面的采样点里面红色点的占比是多少

    - 这似乎只是一个普通的平均而不是加权平均。但是在没有法线信息的情况下目前就这样子了只能。如果有法线信息是可以计算从屏幕空间出发，各个shading point的反射所被遮挡的数量的占比的（加权）

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102211147238.png" alt="image-20241102211147238" style="zoom:50%;" />——可能还需要考虑shading point和周围一个球内的深度差距过大是否剔除的问题

  - ### 如果有法线

    - 就知道半球在哪了
    - 可以进行visibility加权计算了

- ### 补充

  - 更多的采样 -> 更高的精度
  - 为了获得良好的结果，需要大量的采样，但为了性能通常只使用大约 16 个采样。
  - 位置从随机化的纹理中提取，以避免产生条纹。
  - 产生噪声结果，使用保边模糊进行模糊处理。

- ### HBAO

  - 当有了场景normal之后，我们就知道去采样哪半球,就可以只去算半球的情况了，同时也可以对不同的方向来加权（靠近中间大，靠近两边小），使用**多次光线步进**找到**着色点切平面**与**遮挡物采样点**形成的最大角度，再通过这个角度来计算物体与物体间的遮挡程度，从而完成对环境光遮蔽的近似。
  - 不会对**非遮挡物**进行判断，真考虑了一个半球R<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102212306401.png" alt="image-20241102212306401" style="zoom: 33%;" />
  - **SSAO与HBAO**效果对比
    - ![image-20241102212132295](D:\TyporaImage\image-20241102212132295.png)![image-20241102212154051](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102212154051.png)





### 五、Screen Space Direction Occlusion(SSDO)

- ###  SSDO是什么

  - SSAO进阶版
  - 考虑了更多的实际的间接光照

- ### 主要思想

  - 为什么我们必须假设均匀的入射间接光照？ 
  - 已经知道了一些间接光照的信息！ 
  - 听起来耳熟吗？

- ### 解析

  - 既然是基于屏幕上的，那么我们所使用的**次级光照**就是我们在**View视角**所能看到的在光亮区域的地方，它会作为次级光源去照亮周围的物体。![image-20241102224312664](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102224312664.png)

  - 从图中看，SSDO在类似于AO之后还加了一层光线的bounce，这使得闷黑的AO区域有了一丝间接光照射的提亮，并且有一定的颜色倾向了。
  - SSAO是闷黑的，而且可能会亮一点，但是完全没有间接光的颜色。

- ### 观察

  - #### 很类似于path tracing

    - 在着色点 <i>p</i>，发射一条随机光线
    - 如果**没有遇到**障碍物，则为**直接光照**
    - 如果**遇到**了障碍物，则为**间接光照**
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102225511654.png" alt="image-20241102225511654" style="zoom: 50%;" />

  - #### 对比SSAO

    - 与SSAO完全相反
      - ![image-20241102225912140](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102225912140.png)
      - ![image-20241102225922761](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102225922761.png)
      - AO：假设P点本身就可以被四面八方的光所照亮，因为被周围物体挡住所以无法被照亮——0与1，看不见就是看不见
      - DO：假设P点本身就没有光，只有能打到周围物体（周围物体视为发光体）才有光线——有光与无光 ，看不见就计算
        - AO是假设光线是从无限远的地方过来的
        - DO是假设光线从附近的物体来的
        - 实际上应该两个都考虑但是工业界没人两个都用

- ### SSDO计算思想

  - ![image-20241102231634464](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102231634464.png)
  - 我们在这里关心的是V=0的部分，这才是间接光的部分。上面光照部分交给光照考虑。

- ### SSDO计算核心

  - ![image-20241102233419105](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102233419105.png)

  - PA能不能挡住？不管，只看EA（眼睛当做E）、EA的深度判定发现更浅，那说明PA有遮挡物。反之，C点发现EA更深，说明没用遮挡物，PC延伸过去那就是没有间接光的贡献。

  - 按照这么一看，然后计算ABD光照的计算对点P的贡献值

  - ### 但是

    - 实际上，E能看到的不代表P真的能看到，E看不到的也不代表P看不到。屏幕空间的E与场景的一个点形成的射线确实是不能代表从点P射出射线指向某一个点，肯定有误差。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102233701161.png" alt="image-20241102233701161" style="zoom:50%;" />
    - 提升空间，真trace一根光线——SSR。
    - 这里是光栅化，不是光追

- ### 效果与问题

  - #### 效果上

    - 质量上很接近离线渲染

  - #### 问题

    - GI是一个很小范围的，旁边那个绿墙太远了就看不到<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102235005325.png" alt="image-20241102235005325" style="zoom:50%;" />
    - SS所带有的通病，看不到的地方就会丢失信息：图1还有黄色的间接光，图三就没了
    - ![image-20241102234847122](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102234847122.png)





### Screen Space Reflection/RayTracing（SSR）

反射的本质就是个全局光照

- ### 什么是SSR？

  - 在屏幕空间做光线追踪（之前是有空间的光追），只通过屏幕空间尚有的信息来计算
  - 表现光线追踪
  - 但是不需要3D空间信息

- ### 两个计算点

  - 在**任何射线（不单单是反射）**与场景的求交
  - 在求交的像素之间计算着色点数据
    - 对于每个片段
      - 计算反射光线
      - 沿光线方向追踪（使用深度缓冲）
      - 使用交点的颜色作为反射颜色

- ### 应用场景

  - smoothness可自定义调整的，多反射几条光线的事情
    - 镜面反射（高smoothness）
    - 非镜面反射（低smoothness）
    - Glossy反射（中能smoothness）
  - 不单单只有smoothness
    - 有法线的
    - 自定义可变的smoothness（赃迹）

- ### SSR具体做法

  - ![image-20241103001820292](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103001820292.png)

  - ### 线性光线行进（Linear Raymarch）

    **目标**：找到交点

    - 每一步检查**深度值**
    - 质量取决于**步长**（**固定距离**，这里没有sdf给安全距离）
    - 可以进行精细化
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103002137923.png" alt="image-20241103002137923" style="zoom:50%;" />

  - ### 如何跳步？

    - **深度贴图的Mip-Map**

      - 每次边长取半，值用min来取。下一层的像素用上一层的四个像素的最小值来取。
      - 也就是如果是两个像素取最小值，会取上面的红线处，而不是两个深度的中间值<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103003818375.png" alt="image-20241103003818375" style="zoom:50%;" />

    - **用处**

      - 3D场景里面我们会用层级关系（BVH,KD-Tree）来做加速结构

      - 对于树根都不相交的，叶子结点那就不可能相交

      - 同理，这里用深度图来当做层级，越浅越先接触，那层级就往上走

        - 如果与高层级都不想交，那就肯定不会和低层级的相交<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103004611065.png" alt="image-20241103004611065" style="zoom:33%;" />，和最浅的部分相交了，说明有可能和更深的物体相交

        - 相交点的位置就是下一层的节点<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103004837018.png" alt="image-20241103004837018" style="zoom:33%;" />
    
    - ### 用法
    
      - 试探的方法
        - 如果前面层没有相交，步数（Level+1）就+1（就是层级关系+1，步子大一点）
        - 如果与最大层级（level2）产生交点<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103005253673.png" alt="image-20241103005253673" style="zoom:33%;" />，那就层数减一（Level1）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103005349951.png" alt="image-20241103005349951" style="zoom:33%;" />，但是这里分**左右边的level0**
          - **①**如果左边level0没有交点<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103005703678.png" alt="image-20241103005703678" style="zoom:25%;" />，那就会level+1判断下个level是否有交点<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103005814571.png" alt="image-20241103005814571" style="zoom:25%;" />
        - 如果level1有交点，那就判断最小层级（level0）是否有交点<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103005804871.png" alt="image-20241103005804871" style="zoom:25%;" />
        - 如果最小层级有交点，说明此层级的深度会被射线打中，结束，
      - ![image-20241103005846989](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103005846989.png)
        - 停止判断条件应该加一个：如果碰到了交点。这里只说了如果没碰到
    
    - #### 步骤规范化
    
      - 上面的①的步骤，实际上与节点求交之后是知道交点在哪个节点的，不需要一个个试。也就是不需要从level2到level1的时候区分左右边
    
    - **缺点**
    
      - 做不了不是2^k^的范围查询 
  
- ### SSR的缺点

  - **SS的问题**
    - 看不到的东西是真没有<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103012216371.png" alt="image-20241103012216371" style="zoom:33%;" />
    - Edge Cut Off很远的东西看不到<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103012635443.png" alt="image-20241103012635443" style="zoom:25%;" />
      - 解决方法，根据距离做衰减，远的直接糊掉<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103012704696.png" alt="image-20241103012704696" style="zoom:33%;" />


  - **自身的缺点**
    - 在**漫反射情况**下效率不高（现在好像在做这玩意）
    - 缺少屏幕外的信息

- ### SSR的优点

  
  - 可以写在shader里面
  - 效果好
  - 速度快
  - 对于光亮和高光反射**性能较快**
  - **质量好**
  - 无尖峰和遮挡问题
  



- ### 如何shading计算

  *照搬pass tracing的做法即可*

  - **反射物得是<u>diffuse</u>**的（我们计算不了反射物到shading point 的radiance，在这里我们屏幕空间算不了，没这个信息），但是shading point可以是glossy的  ——也就是q点的出射等于p点的入射，然后传到摄像机
  - ![image-20241103013720182](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103013720182.png)
  - **处理好了次级光源到shading point的可见性问题**。
    - 在做BRDF sampling的时候不会出现可见性问题和距离平方衰减的问题
    - 在做Light sampling的时候一定会出现可见性问题和距离平方衰减的问题，而且都不好做

- ### 可以实现的需求

  - **锐利和模糊反射**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103014607821.png" alt="image-20241103014607821" style="zoom:25%;" />
    - 只是对于BRDF的不同数量的改变
  - **接触硬化（Contact hardening）**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103014751095.png" alt="image-20241103014751095" style="zoom:33%;" />
    - 自然而然实现的，因为shading point发出的反射锥就是会随着物体的远离而逐渐扩大（cone）
  - **高光拉伸**（下雨天看红绿灯的倒影很常见）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103014937834.png" alt="image-20241103014937834" style="zoom:33%;" />
    - 认为地面都是各向同性的，本身就是会拉伸的，正确考虑就是会出现的现象<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103015157734.png" alt="image-20241103015157734" style="zoom:25%;" />
  - **每像素粗糙度和法线**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103015214459.png" alt="image-20241103015214459" style="zoom:33%;" />
    - 不同的几何都可以做，不同的粗糙度、法线之类的都照样可以实现，因为是shading point，单个线一个个shading的
  - 其他
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103015504911.png" alt="image-20241103015504911" style="zoom:25%;" />不一定要均匀采样，还可以自定义分布采样（离线渲染用）。扭一扭改一改这个采样形状就可以在smoothness里面变化
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103020025247.png" alt="image-20241103020025247" style="zoom:25%;" />复用其他点trace到的结果
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103020119707.png" alt="image-20241103020119707" style="zoom:25%;" />做filter来代替范围查询（注意前后景的filter别混在一起）



# Lec10/11 Physically-based Materials(surface models)

-  Microfacet BRDF（微面元BRDF）
- Disney principled BRDF(迪士尼原则的BRDF)

### 概况

- **基于物理的渲染 (Physically-Based Rendering, PBR)**
  - 渲染中的一切都应该基于物理原理
  - 包括材质、光照、相机、光传输等
  - 不仅仅是材质，但**通常指的是材质** :):smile:
- **实时渲染 (Real-Time Rendering, RTR) 中的 PBR 材质**
  - 实时渲染社区比离线渲染社区落后很多
  - 在实时渲染中，“基于物理”通常并不真正基于物理 ;) :smile:



*RTR中的PBR材质，对于表面，大部分只是microfacet模型（使用错误，措意不是PBR）和迪士尼原则性BRDF（对艺术家友好，但仍然不是PBR）*



- **实时光线追踪 (RTR) 中的物理基渲染 (PBR) 材质**
  - **对于表面**，主要使用微表面模型和迪士尼原理的 BRDF（双向反射分布函数）
  - **对于体积**，主要集中在快速和近似的单次散射和多次散射
    （用于云、头发、皮肤等）



- **实时光线追踪 (RTR) 中的物理基渲染 (PBR) 材质**
  - **对于表面**，主要使用微表面模型（使用方式不当，所以不是 PBR）和迪士尼原理的 BRDF（对艺术家友好，但仍不是 PBR）
  - **对于体积**，主要集中在快速和近似的单次散射和多次散射（用于云、头发、皮肤等）
  - 通常没有太多新的理论，但有很多实现上的技巧
  - 性能（速度）仍然是需要考虑的关键因素





### 复习：微面元BRDF

:place_of_worship:



![image-20241103161923205](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103161923205.png)



- ### ==Fresnel Term==

  - 涉及grazing angle<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103162147246.png" alt="image-20241103162147246" style="zoom:33%;" />
  - 导体——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103162257018.png" alt="image-20241103162257018" style="zoom:33%;" />（金属全程都很多 :sweat_smile:）
  - 正常考虑极化(polarization)的话<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103162458353.png" alt="image-20241103162458353" style="zoom:33%;" />
  - 通过Schlick’s近似<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103162513803.png" alt="image-20241103162513803" style="zoom:67%;" />
    - 实际应用就是定义一个参数，这个参数R~0~由n~1~和n~2~构成
    - **n~1~是入射介质的折射率，n~2~是出射介质的折射率**

- ### ==D(h)——Normal Distribution Function(NDF)==

  - 直观看看![image-20241103163058925](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103163058925.png)

    - 想要让球变扁，那就需要将微表面上下scale变大
    - 直接把球拍扁，差不多就是光的模样

  - **法线分布函数（Normal Distribution Function，NDF）**

    - 注意：与统计学中的正态分布无关
    - 描述NDF的多种模型
      - Beckmann、GGX等
      - 详细模型 [Yan 2014, 2016, 2018, …]

  - ## Beckmann NDF

    - ![image-20241103164603627](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103164603627.png)

    - 公式解释

      - Beckmann 法线分布函数（NDF）
        - 类似于**高斯分布**
        - 假设是个各向同性
        - 但定义在**斜率空间（slope space）**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103164700378.png" alt="image-20241103164700378" style="zoom:50%;" />
          - 这里红色的线就是**Slpoe space**，确保这个值一直都是90°以内，使得值能够有效。

      $$
      D(h) = \frac{e^{-\frac{\tan^2 \theta_h}{\alpha^2}}}{\pi \alpha^2 \cos^4 \theta_h}
      $$

      - <i>α</i>：表面粗糙度（值越小，表面越接近镜面/高光），就是之前提到的将微面元上下Scale拉大，那个球球它就变大或者变扁——类似于**正态分布**里面通过**方差**来更改函数的胖瘦  
      - <i>θ<sub>h</sub></i>：半向量 <i>h</i> 与法线 <i>n</i> 之间的角度，控制每个shading point它自己的角度变化所带来对光的响应方式

  - ## GGX NDF（or Trowbridge-Reitz）

    - 一个很典型的特征——长长的尾巴（好像光在表面晕染开来，光晕，很好看）
    - 对比**Beckmann**，它没那么快衰减到0。高光比Beckmann要尖锐，但会拖一条尾巴
    - ![image-20241103170218705](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103170218705.png)

    - ![image-20241103170500078](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103170500078.png)

  - ### Extending GGX 

    - 引入了一个新的参数来修改拖尾——γ
    - GTR（Generalized Trowbridge - Reitz）
    - ![image-20241103171050821](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103171050821.png)

   


- ### ==G(i,o,h)-Shadowing -Masking Term==

- ***G:Geometry**

  - ![image-20241103171247017](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103171247017.png)

  - 涉及到grazing angle，越是grazing angle越容易被挡住

    - 左：Shadowing——from light
    - 右：Masking——from eye
    - ![image-20241103171415419](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103171415419.png)

  - #### 这一项的重要性

    - 如果没有这一项，在grazing angle特别大的时候，分母的n点乘i会变得特别大，几乎全白。需要有一个随着grazing angle的变大而抑制效果越强的参数来抑制这不断扩大的分母。

  - ## Smith shadowing-masking term

    - 将shadowing 和masking分开来考虑<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103172121357.png" alt="image-20241103172121357" style="zoom:50%;" />
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103172139942.png" alt="image-20241103172139942" style="zoom: 80%;" />



- ### 问题

  - ![image-20241103172608442](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103172608442.png)
  - 当**粗糙度**很高的时候，会变暗，似乎有能量损失
  - 下面那一行就应该跟第一个一样与背景融为一体
  - 先不考虑颜色，颜色有颜色自己的能量损失形式，也有颜色的能量损失补充方法

- ### 发现问题根源

  - **如果只考虑一次bounces**

    - 当粗糙度scale很大的时候，应该更容易对周围的微面元进行遮挡<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103172911450.png" alt="image-20241103172911450" style="zoom:50%;" />

  - **将损失的能量<u>加回来</u>**

    - 实际上是有这个方法的

    - 但是在RTR里面太慢了

      

  - ##### 基础思想

    -  被遮挡==意味着要发生下一次的弹射

- ### ==寻求解决方法——Kulla-Conty近似==（无颜色的情况）

  表现的是当物体粗糙度提高的时候，如何保证能量不会被自遮挡所损失掉

  - #### **首先得先知道有多少的能量丢失了**

  - ![image-20241102195256990](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241102195256990.png)

    *放着参考↑*

  - ![image-20241103181340209](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103181340209.png)

    - 这个就是**==考虑了能量补充的BRDF==**

    - 这里默认**L~i~ = 1**（假设，主要是为了拿来验证之后代入的因子是否正确，仅此而已）

    - 因为是shading，所以visibility项也不考虑

    - 积分上进行换元

    - **E(μ~o~)——**出射光的能量，未被遮挡掉的能量。（不同视角产生的出射光能量不同，已考虑）

    - ### <u>损失掉的能量为</u>——<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103182643935.png" alt="image-20241103182643935" style="zoom: 67%;" />

      - 损失多少就补上多少
      - **==这个就是我们损失的能量。==**
      - **我要得到它！！！**:angry:**这个就是我需要的值。**现在我需要一个函数能够表现这个我所得到的值，这个是理想的准确值，我可以通过各种函数计算得到这个值。下面提供其中一种较为简单的函数计算方式。（只要最后积分能得到它，那就是对的。只不过拟合方式不一样）
        - 即——现在我需要一个式子<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103183956852.png" alt="image-20241103183956852" style="zoom:50%;" />，使得它代入到**“考虑了能量的BRDF”**之后，能够得到**“损失掉的能量”**
        - 这个式子是拿来算**“损失掉的能量”**用的一种BRDF表达式

  - ### 考虑到对称性的方式拟合

    - 入的能量损失以及出的能量损失**应该计算为**
    - **==双向能量损失补偿因子（Bidirectional Energy Loss Compensation Factor）。==**![image-20241103182900032](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103182900032.png)
      - **c——**用于归一化的某个常数或者某个值

  - ### 根据对称性进行拟合

    - 根据对称性，提出了一种拟合方式，为：

    - ![image-20241103183053981](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103183053981.png)

      - **E~avg~：**是一个值，得到的 c 是一个常数

    - ### 验证

      - ![image-20241103183656529](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103183656529.png)
      - 得到的确实是**损失掉的能量**，说明这个<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103182900032.png" alt="image-20241103182900032" style="zoom:50%;" />模型确确实实是可以得到损失掉的能量是多少。这玩意好像叫做**双向能量损失补偿因子**（Bidirectional Energy Loss Compensation Factor）。
      - **作用：**反正就是有**这么个因子使得带入到我的BRDF模型进行积分之后，我可以得到损失掉的能量。**

  - ### 然后我们来说说这个==E~avg~==怎么搞

    - ![image-20241103190051842](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103190051842.png)

    - ### 预计算/打表（纬度低-依赖少）

    - 为了提前**解决掉这个E~avg~**的计算，我们就预计算。发现他依赖于**μ~o~**和**粗糙度**

    - **μ = sinθ**

    - **粗糙度（rooughness）**：用在<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103185727232.png" alt="image-20241103185727232" style="zoom: 67%;" />这里E~avg~里面的E(μ)的BRDF公式里面的**D(h)**法线分布函数里面的**α**值。<u>之前有提到过</u>![image-20241103185826615](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103185826615.png)，他就是拿来影响这个玩意的（我寻思要是不物理的话自定义一个粗糙度也不是不行)。对于何时表正确，或者怎么打表，我直接网上要一张表应该不是问题吧？

    - ![image-20241103185542900](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103185542900.png)

  - ### Kulla-Conty近似的结果如何（无颜色）

    - ![image-20241103194751526](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103194751526.png)

- ### Kulla-Conty近似（有颜色的情况）

  ==表现的是在粗糙度提高的时候，确保颜色不会在自己发生吸收的情况下因为自遮挡而损失能量==

  - #### 分析

    - 颜色 == 吸收 == 能量损失（本身就该这样）
    - 所以我们只需要单独计算能量损失即可

  - #### 定义新变量

    - 定义一个平均菲涅尔项（当作反射了多少的能量）
    - ![image-20241103195435288](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103195435288.png)
      - **μ：**应该还是sinθ
      - **F~avg~**：RGB,对于有颜色的值它应该是小于1的（RGB肯定有一些波被吸收了）
    - 回顾刚才说的**E~avg~**指的是有多少的能量能够看到，也就是不会在未来的bounces中参与计算，它是逸散出来的，没逸散出来的要参与计算的是 **1 - E(μ)**
      - **E~avg~：**RGB，也是三维向量

  - ### Kulla-Conty近似公式（有颜色的情况）

    - ①对于直接可见的，公式为![image-20241103201116203](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103201116203.png)（我不太理解为什么用乘法，应该是因为BRDF里面对于计算微面元的时候就是用乘法)
    - ②在接受了一次bounce之后可见为：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103201157887.png" alt="image-20241103201157887" style="zoom:80%;" />
    - ③K次bounce之后可见的能量为：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103201231701.png" alt="image-20241103201231701" style="zoom:80%;" />
    - 将③全部加起来之后可得，**==颜色值的表达式==**为![image-20241103201452043](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103201452043.png)
    - 将这个值直接**==乘==**到**没有颜色的BRDF**（但有能量损失的计算）上

  - **Kulla-Conty效果**

    - ![image-20241103202158662](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103202158662.png)





### Shading Microface Models using Linearly Transformed Cosines（LTC）

- ### Linearly Transformed Cosines（LTC）

  *具体的实现方法请网上找，这里只做介绍*

  可从[图形学基础|基于LTC的面光源渲染 - CSDN博客](https://blog.csdn.net/qjh5606/article/details/119682254)

  [Real-Time Polygonal-Light with LTC - 知乎](https://zhuanlan.zhihu.com/p/84714602)等

  - 主要解决的是：**实时地实现多边形面光源的反射，并且能有基于物理的BRDF效果。**LTC 解决了多边形光源（不局限于长方形）的辐照度**实时精确解**问题。下面主要讲的应该都是对于面光源的着色方法。
  - 主要是解决微面元模型的Shading问题
  - 主要针对的是**GGX模型**，其他模型也可以
  - 没有阴影（只shading，不shadow）
  - **在多边形照明下**（好像做的是微表面多边形的自发光？面光源？），如果不用这个方法做面光源，那就涉及到**采样**，然后涉及到遮挡，以及阴影的形成<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103203640028.png" alt="image-20241103203640028" style="zoom:80%;" />

- ### 主要思想

  - 对于任意的一个出射的2D **BRDF瓣**是可以通过线性变换变换成一个**cos函数**的。<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103211316587.png" alt="image-20241103211316587" style="zoom:50%;" />

    -  图示的意思是用作图的灯光照亮这个点的brdf函数跟右图的光源照亮这个点的brdf的函数是一样的。
    - 对于任意的shading point各自的brdf，给定其中一种光源，实际上不好做积分
    - 这个cos函数是有解析解的。对于解这个cos函数比直接在表面解2D BRDF lobe要可行。

  - ### 变换方法

    - ![image-20241103212146716](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103212146716.png)
      - 我们实际上需要一种变换方式，使得从cos变换成原有的2D BRDF Lobe。
      - 通过逆变换可以还原
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103212301603.png" alt="image-20241103212301603" style="zoom:50%;" />
      - 这就是我们要使用的线性变换的变换方式
      - 是旧的 乘 M的逆变换 变成新的
    - ![image-20241103212320081](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103212320081.png)
      - 由于Lobe的样式发生了更改，对于面光源也要进行相应的线性变换

  - ### 近似计算

    - 首先我们需要有一个换元入射光的东西<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103213101966.png" alt="image-20241103213101966" style="zoom:50%;" />，换元之后积分域也要变
      - 线性变化之后得进行归一化，因为长度会发生改变
      - 注意顺序，是**新的 乘 M 变成原来的**
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103213702902.png" alt="image-20241103213702902" style="zoom:50%;" />
      - **L~I~**——认为在各处的面光源他的光照强度是一样的，是个常数，所以提出去
      - **cos(w~i~’)**——这个是变换后的形状（已知）
      - **积分域P要变成P‘**
      - **如何把df(x)变成dx**？——微积分
        - ![image-20241103213829413](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103213829413.png)，
      - 积分可解
      - 遗留问题：M怎么算
        - 需要对不同角度进行矩阵M预计算打表



### Disney’s Principled BRDF

- #### 为什么需要Disney原则BRDF

  - 有些材质微面元BRDF表现不了。微面元BRDF只能表示单层的，多层的表示不了——说明Disney’s Principled BRDF可以补充一些材质表现
  - 这些很物理的材质对艺术家不友好——说明Disney’s Principled BRDF对艺术家友好。

- #### 发明目的

  - 为了让艺术家使用方便，因此并不要求在物理上一定正确
  - 但是当PBR的时候，还是会认为Disney’s Principled BRDF是PBR的

- ### 有哪些Principled

  - 会用艺术家平时常说的一些量来代替相关的物理量
  - 参数尽量越少越好
  - 最好是在范围内可调，并且范围最好是[0,1]的
  - 有需要的时候参数可以超过[0,1]
  - 在调整范围内得是好看的，能给艺术家接受的
  - ![image-20241103215727193](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241103215727193.png)
    - 次表面反射：比diffuse还要diffuse，越来越平坦
    - 金属度
    - 镜面程度
    - 镜面程度颜色值：保持镜面反射的情况下更偏向于原来的颜色的程度
    - 粗糙程度
    - 各向异性程度
    - 光泽：grazing angle变大的时候提供一种天鹅绒的毛毛的效果
    - 光泽颜色：绒毛的效果偏向于物体原本的颜色
    - 透明层：透明外层程度
    - 图层光滑：透明外层的光滑程度

- ### 优点缺点

  - 参数易懂
  - 对单个模型可以有很多材质描述
  - 它的实现方式是开源的
  - 并非基于物理的
    - 但问题不大
    - 好处大
  - 参数空间大（难控制却自由度高）
  - 满足能量守恒



### Non-Photorealistic Rendering（NPR） == <u>（fast and reliable）</u>stylization

- ### 特征

  - 从**真实感渲染**开始
  - 利用**抽象**
  - **强化**重要部分

- ### 应用场景

  - 艺术
  - 可视化
  - 指导
  - 教育
  - 娱乐
  - ……

- ##### 我们可以从这张图片中总结出哪些风格？

  - **粗轮廓（实际上是边框）**
  - 色块
  - 表面上的笔触





### ==Outline Rendering==

  - 外轮廓线其实有很多
  - 轮廓不仅仅是边框
    - **[B]oundary / 边界边**——算出
    - **[C]rease / 折痕**——可自定义给出
      - 满足 **由多个面共享的边界**
    - **[M]aterial edge / 材质边**——可自定义给出
    - **[S]ilhouette edge / 轮廓边**——算出
      - 满足得是外轮廓
      - 满足 **由多个面共享的边界**
  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104013729407.png" alt="image-20241104013729407" style="zoom:67%;" />

  - ### 制作方法

    - Shading
    - post-process

- ### Outline Rendering——Shading

  - ### **阴影法线轮廓边**

    - 使阴影**法线**与**观察方向**垂直的表面区域变暗![image-20241104014612900](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104014612900.png)
    - 存在的问题
      - 描边粗细不一样
      - 效果不是很好

- ### Outline Rendering——Geometry

  - **背面法线加粗**
    - 正常渲染前面
    - “加粗”背面后再次渲染
    - 扩展：沿顶点法线加粗
    - 样式可定，可以切角，可加粗
    - 外扩是可以尽量保持粗细一致的，在**屏幕空间做**。或者外扩方向修正为**垂直于摄像机朝向**即可
      - ![image-20241104015051293](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104015051293.png)

- ### Outline Rendering——Image

  - **后期图像边缘检测**
    - 通常使用Sobel卷积因子
    - 高通滤波卷积
    - 很快
    - ![image-20241104015530210](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104015530210.png)

  -  不单单是后处理，还可以综合很多阶段来辅助生成
  - ![image-20241104021618000](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104021618000.png)
    - 图一可以提供内遮挡边界；图二可以提供外轮廓边界





### Color Blocks——色块

- **两种不同的方法**

  - 硬阴影：在阴影上进行阈值处理
  - 色调分离：在最终图像颜色上进行阈值处理（也可以多值化）
  - ![image-20241104022114521](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104022114521.png)

- ### 不同风格不同组合

  - ![image-20241104022214813](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104022214813.png)
  - 有时你不想要色块
  - 而是想要模仿素描效果![image-20241104022317558](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104022317558.png)

  ### 想法

    - 用预生成的笔触纹理替代逐点的阴影
    - 密度？
    - 连续性？



### 风格化表面纹理



- **色调艺术地图**（Tonal Art Maps，简称 **TAMs**）

  - ![image-20241104022628560](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104022628560.png)
  - 不同密度的笔触
  - 每种密度都有一个 **MIPMAP**
    - 对于比较远的素描不至于显得特别黑
    - 感觉屏幕空间UV可以解决

- #### 拼纹理

- ![image-20241104022830492](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104022830492.png)





- ### 主要思想

  - **非真实感渲染（NPR）是以艺术驱动的**

  - 需要有能力将艺术家的需求“翻译”成渲染方面的见解
    - ​	例如：边缘

  - 沟通非常重要

  - 有时甚至需要为每个角色或部分单独处理

- **有些人还没有太多关注的内容**

  - 在非真实感渲染（NPR）中，**==写实模型==**非常重要

- 示例：布料







# Lec12 Real-Time Ray-Tracing

**1 SPP = Extremely noisy results**

10G根光线每秒，每秒又还有x帧，算下来也就只够一次SPP

![image-20241104144237040](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104144237040.png)



就这么一个像素都只能打1次SPP

![image-20241104144257718](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104144257718.png)



就连1SPP都能降噪到这种程度（SOAD）

![image-20241104144816363](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104144816363.png)

**技术分析**



- ### 目标（在SPP下）

  - 要有好的质量（没有overblur，没有 artifacts，保持原有的细节）
  - 速度必须要快（<2毫秒 来每帧降噪）

- **Mission impossible**

  - 切变滤波系列（Sheared filtering series）：SF、AAF、FSF、MAAF 等
  - 其他离线滤波方法（Other offline filtering methods）：IPP、BM3D、APR 等
  - 深度学习系列（Deep learning series）：CNN、Autoencoder 等

  *对于实时光线追踪的降噪很少*





- ### 工业界对实时渲染光线追踪的解决办法

  - **Tempora！！**！——时间

  - ==**Motion Vectors 方法**==

  - ### 主要思想（时间上的复用）
  
    - 前一帧已经滤波好了，并且当前帧仍然需要滤波——降噪
    - 使用**==Motion vectors==**来找到**先前的位置**<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104153446205.png" alt="image-20241104153446205" style="zoom:33%;" />（假设前一帧已经滤波好了）
    - 根本上扩大了SPP（因为前一帧也用了前前帧）
    - 这是二位情况，那三维空间上呢？

  - ### 前置知识
  
    - **几何缓冲区G-Buffers**（Geometry buffer）
      - 只用于存储**==屏幕空间==**的信息
      - 这些**备用信息**可以在渲染的时候**免费获得**（获取很轻量级，**顺手得到**）
      - 可以存储**屏幕空间，像素深度，法线，世界坐标**等
      - ![image-20241104154319438](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104154319438.png)

  - ### Back Proection

    - 为了得到上一帧物体的**屏幕空间坐标**

    - 我们需要从屏幕空间推回世界坐标。我们需要i-1帧的像素所在的世界坐标位置和当前帧i的像素所在的世界坐标位置。

      - **Back Projection获得从屏幕空间获取上一帧世界坐标位置**（好像叫光流）

        - 如果G-buffer有屏幕空间生成的**世界坐标位置s**，直接取用

        - 否则就需要<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104155514972.png" alt="image-20241104155514972" style="zoom:67%;" />。深度值仍然需要保持有。

        - 动态位置更新：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104155655071.png" alt="image-20241104155655071" style="zoom:67%;" />

        - 上一帧的**屏幕空间位置**根据世界坐标可还原为
  
        - $$
          x' = E' P' V' M' s'
          $$

  - ### 如何将上一帧的（已filter好）物体拿到这一帧来用？

    - 定义<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104160743952.png" alt="image-20241104160743952" style="zoom:67%;" />

    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104161039648.png" alt="image-20241104161039648" style="zoom:50%;" />
      - 直接进行线性插值，重点放在上一帧（如果两针变化很大就不能依赖上一帧）
      - 当前帧自己的降噪（都不会）非常好
      - **α**的值一般取**0.1-0.2**
      - **G-Buffer里面的信息很多是从摄像机发出射线打到物体是顺便得到的**
    
  - #### 还提到了一点HDR和LDR的区间阈值以及色调映射的小知识
  
    - ![image-20241104161917610](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104161917610.png)
    - ![image-20241104162218770](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104162218770.png)
      - 实际上本就应该看到图二这么亮，只是图1的成像亮度区间超过了[0,255]，在LDR的显示器上显示不出来。它本身就应该像第二张图这样。
      - **滤波本就不应该让一张图变亮或者变暗。**
    - 实际真的光线追踪渲染出来的图
      - ![image-20241104162605590](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104162605590.png)
      - 与1spp相差就是AO方面，Ground Truth要真实很多
    
  - ### 如果Temporal失效了怎么办？
    
    - ### 失败场景——Geometry层面
    
      - **第一帧或者换一个场景（burn-in period预热）**
        - 突变光源
        - 切镜头，给特写等巨变
      - **倒退着走**（**视野范围内信息越来越多**——SS通病），画面多了信息量
      - 突然背景表现为不遮挡（遮挡漏出）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104163324290.png" alt="image-20241104163324290" style="zoom:33%;" />
    
    - ### 怎么办？
    
      - 照样用——**造成Lagging 拖尾**，**鬼影**，之前玩游戏看到垃圾堆上苍蝇的拖尾就是因为这个<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104163544412.png" alt="image-20241104163544412" style="zoom:50%;" />
    
      - #### 针对失败场景进行调整
    
        - **钳制**
          - 将先前的值钳制向当前值<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104161039648.png" alt="image-20241104161039648" style="zoom:50%;" />
          - 说是把上一帧的结果拉到当前帧？
        - **检测**
          - 使用例如**对象 ID** 来**检测时间上的失败**
          - 如果motion vector不可信，则**调整 α**，可以是二进制或连续的——**导致结果更加noise**
          - 可能加强或扩大空间滤波
        - **问题**: **重新引入噪声！**
    
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104170718137.png" alt="image-20241104170718137" style="zoom:50%;" />——看上去引入noise还能接受，拖影变成噪声了
    
    - ### 失败场景——Shading层面
    
      - **传统的motion vector无法解决静止时物体的shading问题**
    
      - #### **复用自己上一帧的阴影**
    
        - 如果几何上场景保持不变的情况下，用上一帧的位置可能会取到自己上一帧的阴影，造成严重的拖影问题![image-20241104171522890](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104171522890.png)
        - 如果不用上一帧的信息用这一帧的信息就会带来noise![image-20241104171550113](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241104171550113.png)
    
      - #### Glossy reflected image 会发生一定的延迟
    
        - 考虑移动的椅子
        - **光亮反射图像**的运动向量是什么？
    
      
  
- ### **时间累积受到时间抗锯齿 (TAA) 的启发**

  - 它们非常相似，之前是增加**SPP的数量**，这里是用时间**累积采样点的数量**
  - **时间重用本质上提高了采样率**



- ### 对当前帧如何进行filter？

  - 借助G-Buffer的信息是可以filter的很好 的

  - 如何过滤当前帧？
    - **双边滤波器？**([双边滤波器](https://en.wikipedia.org/wiki/Bilateral_filter))
    - **交叉/联合双边滤波器（及其变体）**
      - 考虑更多信息
      - G-缓冲区：法线 / 深度 / 对象 ID 等。



- ### 如何实现对当前帧的filter-目录

  - **实现空间滤波器**
    - **交叉/联合双边滤波 (Cross / Joint Bilateral Filtering)**
    - **实现大范围滤波器 (Implementing Large Filters)**
    - **异常值去除 (Outlier Removal)**

  - **实时光线追踪的特定滤波方法 (Specific Filtering Approaches for Real-Time Ray Tracing, RTRT)**
    - **时空方差引导滤波 (Spatiotemporal Variance-Guided Filtering, SVGF)**
    - **递归自动编码器 (Recurrent AutoEncoder, RAE)**



### 实现滤波

- **假设**我们使用的是**低通滤波器**

  - 用来**去除噪声（通常是一些高频的）**
    - 会不会去除掉一些glossy的信息呢？——会
    - 会不会有一些低频的噪声呢？——会
  - 现在只关注**空间域**

- **输入**
  
  - 一幅带噪声的图像 $\tilde{C}$
  - 一个滤波核 $K$，可能对每个像素不同
  
- **输出** —— 滤波后的图像$\bar{C}$

- ## **==滤波的实现方法==**

  - #### 定义

    - **i是我们想要得到的滤波之后的像素，j是即将要被滤波的像素（可以包括i本身），即原本像素。**
    - **基于i和j的距离，来对j加权其贡献值**
    - ![image-20241105014553885](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241105014553885.png)

  - ### 伪代码

    - ![image-20241105014547188](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241105014547188.png)
      - 最后一部用于归一化，以至于对于**各种不同样式的滤波**都能进行**内部归一化**的操作。
      - 依据**距离的不同**，通过**滤波样式**进行像素的**权重分配**
      - 对于每一个**像素原有的值**（**可以是颜色**，三维或者一维）进行**权重累积**
      - 对于**权重单独累积**，用于**<u>权重</u>的归一化**，而不是对于数值的归一化（这不就没做嘛）
      - 像素贡献值起码是1，至少**包括自己**可以贡献给自己，否则可能需要对于`sum_of_weights`（权重的累积）进行0的分母判定
      - **权重的累积**`sum_of_weights`是个**单通道的值**



### Bilateral Filtering（双边滤波）

- ### **高斯滤波的问题**
  
  - 只会通过低频信息，表现为画面糊掉
  - 也会**模糊边界**
  - 但边界是我们想要保留的高频部分
  
- ### 观察

  - 我们所说的边界，在计算机眼里就是颜色变化剧烈的东西

- ### 主要思想

  - 如何保留边界？——颜色差距大的贡献值就小
  - **如果像素 <i>j</i> 的颜色与 <i>i</i> 的差异过大，让其贡献更小**
  - **只需在滤波核中增加更多控制**![image-20241107003059156](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107003059156.png)
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107003401905.png" alt="image-20241107003401905" style="zoom:33%;" />——**表示像素位置 $(i, j)$ 和 $(k, l)$ 之间的<u>空间距离</u>。$\sigma_d$ 控制空间距离的权重。**
    - **<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107003421071.png" alt="image-20241107003421071" style="zoom:33%;" />——表示像素值 $I(i, j)$ 和 $I(k, l)$ 之间的<u>颜色差异</u>。$\sigma_r$​ 控制颜色差异的权重。**
  - 效果：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107005949045.png" alt="image-20241107005949045" style="zoom:67%;" />
  - 一定程度上是合理的。（想想看噪声不是本来就特别多差别很大的颜色像素嘛）





### Cross/Joint Bilateral Filtering（联合双边滤波）



**==Cross/Joint Bilateral Filtering（联合双边滤波）很适合解决路径追踪的噪声！==**



- ### 观察法线

  - **高斯滤波**: 使用一个度量（距离）
  - **双边滤波**: 使用两个度量（位置距离和颜色距离）
  - 我们只要增加更多的规则来引导滤波的生成，就可以让滤波在需要的地方。



- ### 背景知识

  - 渲染的时候很多摄像头的信息是可以**免费获得**的

  - **G-Buffer的信息**对于**摄像机**而言是**无损的**，对于自己是**没有噪声**的

  - 可以获得很多，法线，深度，世界坐标，物体ID，Albedo等等，都是高精度的。![image-20241107011732626](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107011732626.png)

  - #### 不需要考虑核是否是归一化的，在用的时候直接归一化就行了

  - 不一定要选高斯模糊，只要符合某种“以距离远近而发生变化”即可——可自定义函数

    - 还可以指数函数，cos之类的
    - 高斯也不一定要完全抄写，只要大体模样对，前面和上边的系数可以自己根据就要求修改
    - ![image-20241107012022599](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107012022599.png)

- ### ==对区域噪声的区分方式==

  - **添加更多的标准，来指导像素滤波的程度**
  - 假设我们能得到**深度信息，法线信息，颜色信息**等
  - 下面提供的三个“feature”就**可以区分大多数像素之间的不同**了
  - 这些参数**都有各自自己的$\sigma$​**,确实是**同时乘起来**的（回去看式子也能看得出来确实只是两个高斯相乘而已，只是具体谁的贡献大谁的贡献小，根据**==各自的$\sigma$==**来决定）
    - 那这些参数如何决定？——现实中这些参数是需要**手动调节**的。根据自己需要的贡献进行变化。（炼丹）。松紧自调。
    - **好像每个人的指导参数都是不一样的。**但基本思想是这样的，而且可以自己根据需要自行调节。
    - 缺点不是很明显，**好处很多**。
  - ![image-20241107012121461](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107012121461.png)





### 使用特别大的滤波器

- ### 观察

  - 如果用小滤波，表现还行
  - 如果用特别大的滤波，表现开销很大，算的很慢

- ### 有**<u>两个办法</u>**解决large filters

  - ### 一、拆分为两个Pass

    - 水平的pass模糊一次，在此基础上再垂直方向pass模糊一次，得到的效果和直接模糊差不多

    - NxN拆分成1 x N 和 N x 1

    - **查询次数**: 从 N^2^ 下降到 N+N

    - ![image-20241107111451340](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107111451340.png)

    - #### 深刻理解

      - 2D的高斯滤波可以进行拆分<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107112641192.png" alt="image-20241107112641192" style="zoom:67%;" />
      - 滤波就意味着卷积，卷积那就得积分，则有![image-20241107112707373](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107112707373.png)
        - 可见对于两个方向的滤波可以对方向拆开来
        - 理论上，**双边滤波**往后是不可以这样子拆开来的，但实际上人们**往往照样这么用**

  - ### 二、逐渐增大取值间隔

    - 随着时间增长，而扩大卷积间隔。**每一<u>趟</u>（i）的大小相同，只是间隔扩大。**

      - 具体来说是`a-trous wavelet`（空洞小波）

        - **多次处理**，每次都使用一个 $5 \times 5$ 滤波器。

          **样本之间的间隔不断增大**（$2^i$），例如从 $64^2$ 优化为 $5^2 \times 5$​。

        - ![image-20241107115029870](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107115029870.png)

      - #### 更深的理解（信号处理的角度——本质）是

        - **低通滤波器应用的更大的滤波，意味着会移除更低频的信息**，表现为画面更加模糊与平滑。（注意区分低通滤波器和高通滤波器与小滤波核和大滤波核的区别）

          - *高通还是低通滤波取决于函数（滤波器种类）的使用*

        - 在频域中，采样会导致信号频谱的周期性重复。

          通过选择性跳过某些样本，可以达到同样的效果，而不会引入额外的频率混叠问题。

        - ![image-20241107115354077](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107115354077.png)第一个Pass就是蓝色部分，然后一直往黄色部分逐渐靠，也就是逐渐去掉低频的意思

          - 右边的图就是第一次Pass之后留下的完美信号（假设完美，没有选择性遗留信息）
          - 然后对右边的pass进行第二次pass采样（<u>频谱搬移</u>就是采样的意思），然后第一次pass遗留的信息搬移到黄色区域内，恰巧能够完全重合黄色部分（设计巧妙之处），意味着这是没有锯齿的采样

        - ### 产生的问题

          - 实际情况会考虑在高频保留一些信息，在下一趟搬移的时候经常看到一些格子状的artifact
          - 没听明白也无所谓:anguished:



### Outlier Removal

- ### 谁是outlier

  - ![image-20241107121622763](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107121622763.png)
  - 蒙特卡洛积分是没有上限的。
  - 如果滤波**特别亮的点**，那周围滤波之后特别亮的点会被摊成**更大的亮点**
  - 有时候会变得更加noisy，甚至是blocky
  - 这些超级亮的像素就是outliers

- ### 如何解决outliers

  - 在**滤波之前**就要处理掉

  - #### 如何定义outliers？

    - 他还叫fire fly
    - 对于每一个像素都找 **7x7的相邻像素**
    - 计算中位数（均值）和方差
    - 定义只要超出范围**[μ - k$\sigma$，μ + k$\sigma$]**，那就是**outlier（k通常取1 到 3）**

  - ### 如何做outliers removal？

    - 把值限制在 **[μ - k$\sigma$，μ + k$\sigma$]**里面
    - 这里说的outliers并不是把它扔了，只是clamp到有效范围之内（工业界TAA实现的要更复杂一些）

- #### Temporal Clamping（上面说到要clamp值，这里说说怎么个时间积累的clamp法）

  - **如果直接使用时间上的颜色可能会导致重影问题**
    - 因为 $$C^{(i-1)}$$ 与 $$\overline{C}^{(i)}$$​ （上一帧已经累积了filter的结果的和这一帧没有filter好的）可能差异很大。
    - 解决方法：在时间重用中，可以将 $$C^{(i-1)}$$ 限制到 $$\overline{C}^{(i)}$$​ 附近，使它们更接近。
      - 也就是：$$C^{(i)} = \alpha \overline{C}^{(i)} + (1 - \alpha) C^{(i-1)}$$，在这里**先将上一帧的值进行限制**：$$\Rightarrow \text{clamp}(C^{(i-1)}, \mu - k\sigma, \mu + k\sigma)$$​  ，**把上一帧的结果**拉到**这一帧7x7区域内的范围结果**之后再和当前帧进行blending		
      - 注意是将**==上一帧拉到当前帧统计学==**附近，更相信当前帧，否则鬼影会更加严重
      - 如果有光源怕把光源干掉，那就先把场景的outliers removal之后再把光源放进去即可。



在RTRT体系里面基本上都是noise，残影，overblur之间相互权衡

SVGF

RAE





### 总结：RTRT（实时光线追踪）的特定过滤解决方案

- **时空方差引导过滤**（Spatiotemporal Variance-Guided Filtering，SVGF）
- **循环自编码器**（Recurrent AutoEncoder，RAE）

### 实用的工业解决方案
- **抗锯齿**（Anti-aliasing）
- **超采样和深度学习超采样**（Super sampling 和 DLSS）
- **级联/多分辨率解决方案**（Cascaded / Multi-resolution solutions）
- **分块/延迟着色、粒子、引擎**（Tiled/Deferred shading, particles, engines）



### RTRT（实时光线追踪）的特定过滤解决方案——Spatiotemporal Variance-Guided Filtering，SVGF

- ### 时空方差引导过滤（Spatiotemporal Variance-Guided Filtering, Schied 等人提出）
  - 非常类似于基本的时空去噪方案
  - 但加入了一些额外的**方差分析**和**技巧**
  - 效果对比![image-20241107134856114](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107134856114.png)

- ### 有<u>三个</u>引导因素

  - ### **①深度**

    **==根据深度差异大小反向调节深度值的贡献引导==**

    - ![image-20241107152519153](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107152519153.png)

      - 只要整体趋势**有个衰减的趋势**即可，并不一定要求高斯

      - ### 公式解释：
        $$
        w_z = \exp\left( - \frac{|z(p) - z(q)|}{\sigma_z |\nabla z(p) \cdot (p - q)| + \epsilon} \right)
        $$

        1. **\( z(p), z(q) \)**：
           - 代表点 \( p \) 和 \( q \) 在**某种特征（如深度）的值**。
           - 这些值可以是深度图或者其他关于几何特征的值。

        2. **\( $\sigma_z $)**：
           - 控制参数，决定权重对**深度差异的敏感性**。
           - 该参数可以调整公式对特征差异的反应程度。

        3. **$|\nabla z(p) \cdot (p - q)| $**：
           - \($ \nabla z(p)$ \) 表示在点 **\( p \) 处的梯度（变化率）**。
           - (p - q)  是点 \( p \) 和 \( q \) 的**相对距离(屏幕空间上的相对距离)**。
           - 该项表示特征（如深度）的变化在局部的方向性，得到的是一种**特征的变化量**。

        4. **\( $\epsilon $)**：
           - **防止分母为零的小常数**，起到数值稳定的作用。

      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107154650611.png" alt="image-20241107154650611" style="zoom:50%;" />当出现如图情况时，AB因为 深度差异|z(p) - z(q)|会比较大，同时分母得到的$|\nabla z(p) \cdot (p - q)| $​也会变大，因此**此特征可以描述同方向的侧向面的深度表现**，**更多的是用来描述切平面上的深度差异**。

  - ### **②法线**

    **==根据角度差调整贡献值大小==**

    - ![image-20241107155233888](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107155233888.png)
    - 点乘得到cos表示的夹角数据，给幂次类似于Phong的高光
    - 这里的权重并非一定是高斯分布。
    - 若存在法线贴图（Normal Maps），优先使用**宏观法线**（Macro Normals，可以是几何normal也可以是插值后得到的shading point的normal，但是不要用被法线贴图扰动之后的normal）以保留更大的几何特征。（也就是我们不会用法线贴图的法线）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107155453368.png" alt="image-20241107155453368" style="zoom:50%;" />

  - ### ③明度(灰度)信息

    ==**颜色信息随着标准差的扩大而缩小**，表现为颜色信息相差越大，贡献越少==
    $$
    w_l = \exp \left( \frac{-|l_i(p) - l_i(q)|}{\sigma_l \sqrt{g_{3 \times 3}(\text{Var}(l_i(p)))} + \epsilon} \right)
    $$

    - #### 各符号含义：

      - <code>$l_i(p)$</code> 和 <code>$l_i(q)$</code>：点 **p** 和点 **q** 的**灰度值（亮度值）**。

      - <code>$\text{Var}(l_i(p))$</code>：点 **p** 处灰度值的**方差**。（开根号是为了转换成标准差，先算7x7）

      - <code>$g_{3 \times 3}$</code>：一个 3×3 的高斯核，用于**对方差进行平滑**。（然后在已经糊掉了的情况下不放心再糊一次，少量多次。得到的是平滑之后的**方差**）

      - <code>$\sigma_l$</code>：**控制亮度差异的标准差参数**。

      - <code>$\epsilon$​​​</code>：**避免分母为零的小值常数**。
      - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107161352593.png" alt="image-20241107161352593" style="zoom:67%;" />

    - #### 方差（Variance）——在点B

      - **方差越大，越不能相信两点之间的颜色差异**，所以做分母
      - 在**空间上以 7x7 的区域计算**。
      - 使用运动矢量（motion vectors）在**时间上进行平均**。
      - 在使用之前，**再应用一个 3x3 的空间滤波器**进行平滑处理。

    



### RTRT（实时光线追踪）的特定过滤解决方案——RAE

利用到了神经网络



- ### 交互式重建蒙特卡洛图像序列（Interactive Reconstruction of Monte Carlo Image Sequences using a Recurrent denoising AutoEncoder）[Chaitanya et al.]

  - 后处理网络，用于去噪（从有噪声到无噪声）。
  - 借助 G-buffers。
  - 网络自动执行时间累积（temporal accumulation）。

  ### 关键架构设计（Key architecture design）

  - AutoEncoder（或 U-Net）结构。
  - 递归卷积块（Recurrent convolutional block）。

- ### AutoEncoder

  - 跳跃连接/残差连接（Skip / residual connections），用于更快和更好的训练。

- ### Recurrent block

  - 累积（并逐渐遗忘）来自前一帧的信息。

![image-20241107163823155](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107163823155.png)

![image-20241107163816339](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107163816339.png)

**低频噪声-沸腾的水（Boiling artifact）-帧与帧之间抖动**

![image-20241107164032029](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107164032029.png)





# 实际工业上的一些解决方案

仍然来学术界的角度理解



### Temporal Anti-Aliasing（TAA）

- TAA原本就是拿来做抗锯齿的，但因为**TAA的成功**才有**RTRT**里面**时间上的运用**

- ### 回顾：为什么会有锯齿效应？

  - 在光栅化过程中，每像素的采样点不足。
  - 因此，最终的解决方案是增加采样点数量。

  ### Temporal Anti-Aliasing（时间抗锯齿）

  - 在帧之间（时间上）分布和**重用采样点**。
  - 与**实时光线追踪（RTRT）中的方法几乎完全相同。**

  - 静止场景：<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107170022335.png" alt="image-20241107170022335" style="zoom: 50%;" />

- **如果上一帧不能用于当前帧时，不可用的时候就用clamp的操作，更多的相信当前帧**（有noise但接受）
- 如果场景运动了需要重新根据像素点找回上一帧像素位置所在的物体的空间位置在当前帧的空间位置所呈现出来的像素位置在哪里。（像素位置~pre~→空间位置~pre~→空间位置~custom~→像素位置~custom~）





### 其他的AA（Anti-Alising）

- ### **MSAA（Multisample） vs SSAA（Supersampling）**

  - ### SSAA（Supersampling）

    - 很直观的理解，而且是绝对正确的
    - 按照原像素的多少倍进行直接采样然后再捏回原像素大小的像素去
    - 放大多少倍帧率就掉多少倍，计算开销直观增加

  - ### MSAA（Multisample）

    - 在SSAA上进行改进，有一定的效率提升（部分像素共享采样点的贡献值），打游戏的时候相比SSAA，**更倾向于用MSAA**
    - **一、对于同一个图元，都只做一次shading**（取一种具有代表性的位置）<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107171300547.png" alt="image-20241107171300547" style="zoom:67%;" />
      - 每个感知点有某个深度或者颜色（图元）然后根据表格进行就计算
    - **二、允许在空间上使用采样点重用** 两个采样点在中间，近似的看成对两边像素都做贡献<img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107171429053.png" alt="image-20241107171429053" style="zoom:33%;" />

  

- ### 在图像层面变成没有锯齿的图

  - ### 基于图像的AA解决方案

  - **SMAA（Enhanced subpixel morphological AA）**——打游戏就选这个

  - ### 补充说明 2

    - **最先进的基于图像的抗锯齿解决方案。**
    - **SMAA（增强的子像素形态抗锯齿，Enhanced Subpixel Morphological AA）**。
    - 历史发展：FXAA → MLAA（Morphological AA）→ SMAA。

- ### ==G-Buffers不能反走样==



### DLSS

**打游戏就低分辨率开DLSS就好了**，主要解决的是如何使用低分辨率跑高分辨率的画质

- ### 超分辨率（或超采样）

  - 字面理解：**增加分辨率**，渲小的，然后拉大，猜像素。
  - 来源 1（DLSS 1.0）：**完全猜测（out of nowhere）。**
  - 来源 2（DLSS 2.0）：**基于时间信息（temporal information）**。

  ### 深度学习超采样（DLSS）2.0 的核心思想

  - 又一个类似于时间抗锯齿（TAA）的应用。
  - 利用时间信息重用样本以提高分辨率。

- ### 主要问题

  - 当出现**时间失败（temporal failure）时，简单的限制（clamping）不再适用**。
    - 新增出来的分辨率必须和原有的分辨率有本质上的不同，不然新增出来的分辨率完全按照之前的帧的分辨率直接clamp的话画面就会糊掉
  - 因为我们需要为**每个更小的像素**提供一个清晰的值。 
  - 因此，关键是**如何比单纯限制更聪明地使用时间信息**。
    - 当前帧增加的采样点是上一帧的采样点
    - 神经网络主要输出上一帧的信息如何使用，**==指导当前帧如何使用上一帧的信息==**
  - ![image-20241107173509041](https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107173509041.png)

- ### 一个重要的实际问题

  - 如果DLSS（深度学习超级采样技术）本身每帧需要 **30ms** 运行时间，那么它已经无法使用。
  - 网络推理性能优化（机密信息）。





### Deffered Shading

- **最初的目的**：为了**节省着色时间**，把shading延迟到第二趟的pass来进行。

- ### 考虑光栅化过程

  - 三角形 -> 片段 -> 深度测试 -> 着色 -> 像素

  - 每个片段都需要着色（在什么场景下？）

  - 复杂度：$$O(\#\text{fragment} \times \#\text{light})$$​
  - 对于严格意义上从后往前排序的物体，都会从后往前shading做好再做下一个物体
    - **导致好多物体的shading白做了**，因为被遮挡了
    - 我们期望从前往后排序，这样被遮挡了的就不会通过深度测试了，远处的就不用shade了

  - **能不能先把东西排好，只渲染看到到的fragment**


- ### 关键观察

  - 大多数片段不会出现在最终图像中。
    - 由于深度测试或遮挡。

  - 是否可以只对**可见片段（fragment）**进行着色？



- ### Deferred Shading过程

  - **修改光栅化过程**
    - 只需对场景进行**两次光栅化**：
      - **第一次**：不进行着色，**仅更新深度缓冲区。**
      - **第二次**：上一pass已更新深度缓冲区，这一次可以根据上一次的深度缓冲区来进行深度比较（而不是一个物体一个物体比较），如果小于等于则做shading。
    - 这一过程隐含地假设**光栅化场景**的**速度远快于对所有不可见片段进行着色**（通常是成立的）。
    - 复杂度变化：$$O(\#\text{fragment} \times \#\text{light}) \to O(\#\text{visible fragments} \times \#\text{light})$$​
      - 复杂度得到了优化，只有可见片元了
  - **问题**
    - **很难实现抗锯齿。**——因为是根据深度值通过的像素才可shading的，而且我们不能去对深度图进行抗锯齿，所以深度图有锯齿那就是有锯齿的。
    - 但几乎可以通过**时间抗锯齿（TAA，使用上一帧的信息积累采样点）**完全解决。（或者是基于图像的AA）



观察复杂度变化，如果想要加速那就要从光源入手



### Tiled Shading

- 在Deffered shading的基础上做的

- ### Tiled Shading 改进和关键观察

  - **改进：Tiled Shading**
    - 将屏幕划分为小块（例如 32x32 像素的 tile），然后分别对每个 tile 进行着色。
    
  - **关键观察：**
    - 并非所有光源都能照亮每个 tile。
    - 这是因为光强随距离平方递减（平方衰减原则）。

  - **复杂度分析：**
    - 原始复杂度：$$O(\#\text{visible fragments} \times \#\text{lights})$$
    - 改进后复杂度：$$O(\#\text{visible fragments} \times \text{average} \ \#\text{lights per tile})$$

- <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241107182216487.png" alt="image-20241107182216487" style="zoom:50%;" />

- ### Clustered Shading

  - 深度上也做切片，包含的是物体信息，而非像素

  - ### Clustered Shading 进一步改进和关键观察

    - **进一步改进：Clustered Shading**
      - 进一步将每个 tile 细分为不同的深度段。
      - 实际上是将视锥体（view frustum）细分为 3D 网格。

    - **关键观察：**
      - 每个 tile 的深度范围可能很大。
      - 因此，可能有许多光源被识别为有潜力照亮该 tile。
      - 但某些光源可能只照亮一个很小的深度范围。

    - **复杂度分析：**
      - 从 $$O(\#\text{visible fragments} \times \text{avg } \#\text{lights per tile})$$ 
      - 改进到 $$O(\#\text{visible fragments} \times \text{avg } \#\text{lights per cluster})$$​
      - 避免了很多没有意义的光照的shading

 ### Level of Detail Solutions

  - ### 细节层次（Level of Detail, LoD）的重要性

    - **LoD 非常重要**
      - 回顾：纹理 MIPMAP-ing 技术。
      - 选择正确的细节层次可以节省计算成本。

    - **使用多个细节层次**
      - 在实时光线追踪（RTR）行业中通常称为“级联”（Cascaded）。

  - **如果对shadow map运用LOD，那就是级联阴影CSM（Casscaded Shadow Maps）**

  - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241108124506982.png" alt="image-20241108124506982" style="zoom:50%;" />

  - **CSM（Casscaded Shadow Maps）**

    - 越近的地方选择越精细的shadow map，越远的用比较不精细的shadow map
    - 重叠部分（蓝色部分）偏向哪里就主要用哪里。

  - **Cascaded LPV**

    - 光线传输的时候近处精细传输步伐小，远处大一点传输步伐大
    
    - <img src="https://raw.githubusercontent.com/nininias/image-repo/main/img/image-20241108190031156.png" alt="image-20241108190031156" style="zoom:50%;" />
    
  - ### 关键挑战

    - **关键挑战**
      - 在不同层次之间进行**过渡**。
      - 通常需要在边界附近进行一些重叠和混合。

  - ### 另一个例子：几何 LoD（Level of Detail）

    - 回顾：预生成一组具有不同三角面数的简化对象。
    - 根据与摄像机的距离，选择适当的对象或其部分进行显示，确保没有三角形大于一个像素。
    - **跳变伪影（Popping artifacts）**？交给 TAA（时间抗锯齿）来处理！
    - 这就是 UE5 中的 **Nanite** 技术（但 Nanite 功能远不止这些）。
    
  - ### 一些（很大的）技术难点

    - 不同位置存在不同的细节层次，如何处理**裂缝**问题？
    - 动态加载和调度不同的细节层次，如何最大化**缓存和带宽**的使用效率？
    - 使用**三角形**或**几何纹理**来表示几何体？
    - **裁剪**和**剔除**，以获得更高的性能？
    - ...



### Global Illumination Solutions

- ### 从这门课程中，我们可以了解到

  - **回顾**：屏幕空间光线追踪（SSR）在哪些情况下会失败？
  - 除了**实时光线追踪（RTRT）**外，没有一种全局光照（GI）解决方案能适用于所有情况。
  - 但完全依赖RTRT在当前代仍然成本过高。
  - 因此，业界倾向于使用**混合解决方案**。

- ### 例如，一种可能的GI解决方案包括

  - 使用**SSR**进行粗略的GI近似（类似于我们HW3中的方法）。
  - 在SSR失效时，切换到更复杂的光线追踪：
    - 可以是硬件实现的**RTRT**，也可以是软件实现的（？）。
  
- ### 软件光线追踪（Software Ray Tracing）

  - **==高质量的SDF（HQ SDF）==**用于近距离的单个物体。
  - **==低质量的SDF（LQ SDF）==**用于整个场景。
  - **==反射阴影贴图（RSM）==**，适用于强方向光或点光源。
  - 使用探针（Probes）将辐照度存储在三维网格中（动态漫反射GI，或称**DDGI**）。
  
- ### 硬件光线追踪（Hardware Ray Tracing）
  
  - ==不需要使用原始几何体，而是使用低多边形代理（low-poly proxies）。==
  - **探针（RTXGI）**用于优化光线追踪性能。
  
- ### 最终解决方案
  - 通过混合上述的**==高亮解决方案==**，实现了**Lumen**，用于**虚幻引擎5（UE5）**。





- ### 仍有许多未涉及的主题

  - **SDF（Signed Distance Function）纹理化**  
  - **透明材质**与**顺序无关的透明性（Order-independent Transparency）**  
  - **粒子渲染**  
  - **后处理效果**（如景深、运动模糊等）  
  - **随机种子**与**蓝噪声（Blue Noise）**  
  - **注视点渲染（Foveated Rendering）**  
  - 基于**探针的全局光照（Probe-based Global Illumination）**  
  - **ReSTIR**、**神经辐射缓存（Neural Radiance Caching）**等技术  
  - **多光源理论（Many-light Theory）**及**光剪裁（Light Cuts）**  
  - **参与介质（Participating Media）**与**次表面散射（SSSSS）**  
  - **头发外观渲染**  
  - **其他内容**...



------

**Computer Graphics is Awesome！**
