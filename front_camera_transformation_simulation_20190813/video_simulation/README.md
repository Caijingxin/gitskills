# 前置图像像素坐标投影——仿真

## 成像模型

&emsp;根据小孔成像原理, 世界坐标系中一点$\mathbf{P_{W}}=[X_{W}, Y_{W}, Z_{W}, 1]^{T}$在Camera1上的投影为:
$$
s_{1}{\begin{bmatrix}
u_1\\ 
v_1\\
1\\
\frac{1}{s_1}
\end{bmatrix}} = \mathbf{K_1}\mathbf{T_1}\begin{bmatrix}
X_{W}\\ 
Y_{W}\\ 
Z_{W}\\ 
1
\end{bmatrix}. \tag{1}
$$
其中, $[u_{1}, v_{1}]^{T}$为$\mathbf{P_{W}}$映射到Camera1图像平面的像素坐标, $ s_1$为Camera1的尺度因子. $ \mathbf{K_1}=\begin{bmatrix}
 f_{x_1}&  0&  u_{01}& 0\\ 
 0&  f_{y_1}&  v_{01}& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix} \in \mathbb{R}^{4×4}$ 为Camera1相机内参, $ \mathbf{T_1}=\begin{bmatrix}
 R_{00}&  R_{01}&  R_{02}& t_x\\ 
 R_{10}& R_{11}&  R_{12}& t_y\\ 
 R_{20}&  R_{21}&  R_{22}& t_z\\ 0& 0& 0& 1
\end{bmatrix} \in \mathbb{R}^{4×4}$ 为世界坐标系到Camera1相机坐标系到转换矩阵.



&emsp;同样地, 点$\mathbf{P_{W}}$在Camera2上的投影为:
$$
s_{2}{\begin{bmatrix}
u_2\\ 
v_2\\
1\\
\frac{1}{s_2}
\end{bmatrix}} = \mathbf{K_2}\mathbf{T_2}\begin{bmatrix}
X_{W}\\ 
Y_{W}\\ 
Z_{W}\\ 
1
\end{bmatrix}. \tag{2}
$$
其中, $[u_{2}, v_{2}]^{T}$为点$\mathbf{P_{W}}$映射到Camera2图像平面的像素坐标, $ s_2$为Camera2的尺度因子. $ \mathbf{K_2}$ 和$\mathbf{T_2}$分别为Camera2的相机内参和世界坐标系到Camera2相机坐标系的转换矩阵.



&emsp; 求解目标: Camera1图像像素坐标到Camera2图像像素坐标的转换:

## 仿真1

&emsp;假设$ s_1$未知,则:
$$
s_2{\begin{bmatrix}
u_2\\ 
v_2\\
1\\
\frac{1}{s_2}
\end{bmatrix}} =\mathbf{K_2}\mathbf{T_2}\mathbf{T_1^{-1}}\mathbf{K_1^{-1}}{\begin{bmatrix}
u_1\\ 
v_1\\
1\\
1
\end{bmatrix}}. \tag{3}
$$
&emsp;设图像大小为960*1280, 根据式(3)进行仿真.

&emsp;仿真1.1和1.2是为了模拟当目标只存在垂直于光轴方向的平移而没有沿着光轴方向的运动时, Camera1像素坐标到Camera2像素坐标的投影情况. 仿真1.3是为了模拟当目标仅沿着相机光轴方向移动时的像素投影情况.

### 仿真1.1

&emsp;参数设置:  $\mathbf{T_1}=  \mathbf{I} =\begin{bmatrix}
 1&  0&  0&0\\ 
0& 1&  0&0\\ 
 0&  0& 1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, 也就是说, 在仿真中, 世界坐标系和Camera1相机坐标系重合.

$\mathbf{K_1}=\begin{bmatrix}
 1600&  0&  640& 0\\ 
 0&  1600&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, $\mathbf{T_2}= \begin{bmatrix}
 0.802725& 0.596144&  0.0156502&0.05\\ 
-0.595785& 0.800548&  0.0645244&0\\ 
 0.0259371&  -0.0611195& 0.997793& 0\\ 0& 0& 0& 1
\end{bmatrix}$, (yaw: -36.599$^{o}$, pitch: 0.8967$^{o}$, roll: -3.7$^{o}$). $\mathbf{K_2}=\begin{bmatrix}
 800&  0&  640& 0\\ 
 0&  800&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$



&emsp;世界坐标$\mathbf{P_{W}}$的变化趋势如下表所示:

|            /m             |  $\mathbf{{P_1}_{W}}$   |  $\mathbf{{P_2}_{W}}$   |  $\mathbf{{P_3}_{W}}$   |  $\mathbf{{P_4}_{W}}$   |
| :-----------------------: | :---------------------: | :---------------------: | :---------------------: | :---------------------: |
| $\mathbf{P_{W}\_{begin}}$ | (-3.4, -2.4, 10.0, 1.0) | (-2.4, -2.4, 10.0, 1.0) | (-3.4, -1.7, 10.0, 1.0) | (-2.4, -1.7, 10.0, 1.0) |
|  $\mathbf{P_{W}\_{end}}$  |  (2.3, 2.1, 10.0, 1.0)  |  (3.3, 2.1, 10.0, 1.0)  |  (2.3, 2.8, 10.0, 1.0)  |  (3.3, 2.8, 10.0, 1.0)  |
|       $\Delta{X_W}$       |           0.3           |           0.3           |           0.3           |           0.3           |
|       $\Delta{Y_W}$       |           0.3           |           0.3           |           0.3           |           0.3           |
|       $\Delta{Z_W}$       |           0.0           |           0.0           |           0.0           |           0.0           |

&emsp;在世界坐标系下, 矩形的四个顶点分别为$\mathbf{{P_1}_{W}}$, $\mathbf{{P_2}_{W}}$, $\mathbf{{P_3}_{W}}$, 和$\mathbf{{P_4}_{W}}$. $\mathbf{P_{W}\_{begin}}$和$\mathbf{P_{W}\_{end}}$ 分别为这四个点的起始位置和终点位置. 在仿真的过程中, 点在世界坐标系中$X$轴方向的变化量$\Delta{X_W}=0.3$, $Y$轴方向的变化量$\Delta{Y_W}=0.3$, $Z$轴方向的变化量$\Delta{Z_W}=0.0$.

<div align=center>
    <img src="./video_simulation/img1.png">
</div>

&emsp;如上如所示, 在仿真过程中, 由公式(1)和(2)得到的区域用白色矩形框表示, 分别记为**BBox_cam1**和**BBox_cam2**, 由公式(3)得到的Camera1像素坐标到Camera2像素坐标的投影用蓝色矩形框表示, 记为**BBox_cam1to2**. 其中, 白色的矩形框的4个边界点被标记出来. 右侧图像左上角的值为白色框与蓝色框之间的IOU.

&emsp;仿真结果详见以下视频:

<video src="./video_simulation/simulation1.1.mp4" controls="controls"></video>
&emsp;**BBox_cam2**和**BBox_cam1to2**之间的IOU基本保持恒定, 且两框之间的相对位置也无明显变化. 此参数下, Camera2中蓝色框和白色框IOU均值为0.248.

### 仿真1.2

&emsp;参数设置: $\mathbf{T_2}= \begin{bmatrix}
 1& 0&  0&0.05\\ 
0& 1&  0&0\\ 
 0&  0& 1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, 即Camera2和世界坐标之间只存在平移, 不存在旋转. 其余参数和**仿真1.1**完全相同.

&emsp;仿真视频如下:

<video src="./video_simulation/simulation1.2.mp4" controls="controls"></video>
&emsp;**BBox_cam2**和**BBox_cam1to2**之间的IOU基本保持恒定, 且两框之间的相对位置也无明显变化. 此参数下, Camera2中蓝色框和白色框IOU均值为0.373.

### 仿真1.3

&emsp;为了模拟目标离相机越来越远的情况下两相机像素坐标之间的投影情况, 进行参数设置: $\mathbf{T_1}=  \mathbf{I} =\begin{bmatrix}
 1&  0&  0&0\\ 
0& 1&  0&0\\ 
 0&  0& 1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, $ \mathbf{K_1}=\begin{bmatrix}
 1600&  0&  640& 0\\ 
 0&  1600&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, $\mathbf{T_2}= \begin{bmatrix}
 1& 0&  0&0.05\\ 
0& 1&  0&0\\ 
 0&  0& 1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, $ \mathbf{K_2}=\begin{bmatrix}
 800&  0&  640& 0\\ 
 0&  800&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$, 即和**仿真1.2**参数相同.

&emsp;目标在世界坐标系中的位置及其变化跟之前的有所不同, 具体见下表:

|            /m             |   $\mathbf{P_{W}^{1}}$   |  $\mathbf{P_{W}^{2}}$   |  $\mathbf{P_{W}^{3}}$   |  $\mathbf{P_{W}^{4}}$  |
| :-----------------------: | :----------------------: | :---------------------: | :---------------------: | :--------------------: |
| $\mathbf{P_{W}\_{begin}}$ |  (-1.5, -0.3, 5.0, 1.0)  |  (1.0, -0.3, 5.0, 1.0)  |  (-1.5, 1.2, 5.0, 1.0)  |  (1.0, 1.2, 5.0, 1.0)  |
|  $\mathbf{P_{W}\_{end}}$  | (-1.5, -0.3, 150.0, 1.0) | (1.0, -0.3, 150.0, 1.0) | (-1.5, 1.2, 150.0, 1.0) | (1.0, 1.2, 150.0, 1.0) |
|       $\Delta{X_W}$       |           0.0            |           0.0           |           0.0           |          0.0           |
|       $\Delta{Y_W}$       |           0.0            |           0.0           |           0.0           |          0.0           |
|       $\Delta{Z_W}$       |           2.0            |           2.0           |           2.0           |          2.0           |

&emsp;世界坐标系中矩形的四个顶点坐标及变化趋势如上表所示. 在仿真的过程中, 目标点在世界坐标系中$ X$轴方向的变化量$ \Delta{X_W}=0.0$, $Y$轴方向的变化量$\Delta{Y_W}=0.0$, $Z$轴方向的变化量$\Delta{Z_W}=2.0$. 目标与相机在沿着光轴方向的距离由5.0m逐步变为150.0m.

&emsp;仿真详情如下:

<video src="./video_simulation/simulation1.3.mp4" controls="controls"></video>
&emsp;随着目标物体离相机越来越远, **BBox_cam2**和**BBox_cam1to2**之间的IOU越来越小, 最后甚至为0. 此参数下, Camera2中蓝色框和白色框IOU均值为0.113. 

### 结论:

&emsp;当$s_1$未知且目标仅有左右往返运动, 即$\Delta{Z_W}=0$时, **BBox_cam2**和**BBox_cam1to2**之间的IOU相对恒定, 不会因为位置不同而发生明显的变化. 当目标离两相机越来越远时, **BBox_cam2**和**BBox_cam1to2**之间的IOU越来越小, 最终完全为0.



## 仿真2

&emsp;假设$ s_1$已知,
$$
s_2{\begin{bmatrix}
u_2\\ 
v_2\\
1\\
\frac{1}{s_2}
\end{bmatrix}} =\mathbf{K_2}\mathbf{T_2}\mathbf{T_1^{-1}}\mathbf{K_1^{-1}}{\begin{bmatrix}
u_1\\ 
v_1\\
1\\
\frac{1}{s_1}
\end{bmatrix}}. \tag{4}
$$
&emsp;假设图像大小为960*1280

&emsp;**仿真2.1-2.3**和**仿真1.1-1.3**之间仅有公式(3)和(4)的差别, 其余完全一样.

### 仿真2.1

&emsp;参数设置: 和**仿真1.1**完全相同.

&emsp;实验视频:

<video src="./video_simulation/simulation2.1.mp4" controls="controls"></video>
&emsp;**BBox_cam2**和**BBox_cam1to2**之间的IOU几乎为1.0, 所以两矩形框几乎重合, 蓝色覆盖了白色, 所以只能看到一个蓝色矩形框.

### 仿真2.2

&emsp;参数设置: 和**仿真1.2**完全相同.

&emsp;实验视频:

<video src="./video_simulation/simulation2.2.mp4" controls="controls"></video>
### 仿真2.3

&emsp;参数设置: 和**仿真1.3**完全相同.

&emsp;实验视频:

<video src="./video_simulation/simulation2.3.mp4" controls="controls"></video>
### 结论

&emsp;当$s_1$已知时, Camera1上的像素坐标几乎始终可以完全正确地投影到Camera2上.



## 仿真3

&emsp;假设$ s_1$未知, 验证两相机之间的旋转量和平移量对像素反投影的影响:
$$
s_2{\begin{bmatrix}
u_2\\ 
v_2\\
1\\
\frac{1}{s_2}
\end{bmatrix}} =\mathbf{K_2}\mathbf{T_2^{'}}\mathbf{T_1^{-1}}\mathbf{K_1^{-1}}{\begin{bmatrix}
u_1\\ 
v_1\\
1\\
1
\end{bmatrix}}. \tag{5}
$$
其中, $ \mathbf{T_2^{'}}=\begin{bmatrix}
 R_{00}^{'}&  R_{01}^{'}&  R_{02}^{'}& 0\\ 
 R_{10}^{'}& R_{11}^{'}&  R_{12}^{'}& 0\\ 
 R_{20}^{'}&  R_{21}^{'}&  R_{22}^{'}& 0\\ 0& 0& 0& 1
\end{bmatrix} \in \mathbb{R}^{4×4}$.

### 仿真3.1

&emsp;仿真平移向量对反投影后矩形框精度的影响. 令$ \mathbf{T_2^{'}}= \begin{bmatrix}
 0.802725& 0.596144&  0.0156502&0\\ 
-0.595785& 0.800548&  0.0645244&0\\ 
 0.0259371&  -0.0611195& 0.997793& 0\\ 0& 0& 0& 1
\end{bmatrix}$. 其余参数和**仿真1.1**相同.

&emsp;仿真视频:

<video src="./video_simulation/no_translation.mp4" controls="controls"></video>
&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.851. 可以看出, 当两相机之间的平移量为[0.05, 0, 0]时, 反投影时忽略平移矩阵比考虑平移矩阵会得到更大的IOU.

### 仿真3.2

 &emsp;仿真欧拉角roll对反投影后矩形框精度的影响. 参数设置: (yaw: -36.599$^{o}$, pitch: 0.8967$^{o}$, roll: -3.7$^{o}$*1.5), 其余和**仿真3.1**相同.

&emsp;仿真视频:

<video src="./video_simulation/roll_1.5.mp4" controls="controls"></video>
&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.340.

### 仿真3.3

&emsp;仿真欧拉角pitch对反投影后矩形框精度的影响. 参数设置: (yaw: -36.599$^{o}$, pitch: 0.8967$^{o}$*1.5, roll: -3.7$^{o}$) 

&emsp;仿真视频:

<video src="./video_simulation/pitch_1.5.mp4" controls="controls"></video>
&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.906.

### 仿真3.4

&emsp;仿真欧拉角yaw对反投影后矩形框精度的影响. 参数设置: (yaw: -36.599$^{o}*1.1$, pitch: 0.8967$^{o}$, roll: -3.7$^{o}$) 

&emsp;仿真视频:

<video src="./video_simulation/yaw_1.1.mp4" controls="controls"></video>
&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.665.

### 结论

&emsp;两相机的外参矩阵包含旋转矩阵和平移向量, 在做两相机之间的像素转换时, 不考虑它们之间的平移向量会得到更大的IOU. 同时, 旋转矩阵也会对IOU造成一定的影响, 但这种影响不会随着图像不同区域之间有较大的差异.(我们现在遇到的问题是在图像右侧可以得到满意的IOU, 而图像左侧的IOU有待进一步提高.)



## 仿真4

$$
s_2{\begin{bmatrix}u_2\\ v_2\\1\\\frac{1}{s_2}\end{bmatrix}} =\mathbf{K_2}\mathbf{T_2}\mathbf{T_1^{-1}}\mathbf{{K_1^{'}}^{-1}}{\begin{bmatrix}u_1\\ v_1\\1\\1\end{bmatrix}}. \tag{6}
$$

&emsp;本仿真主要为了评估相机内参对投影结果的影响, 为了方便对投影结果的查看, 我们使矩形框在两仿真平面间存在较小的旋转, 设置欧拉角(yaw: 0.75$^{o}$, pitch: -2.25$^{o}$, roll: -0.5$^{o}$), 旋转矩阵(tx: 0.05, ty: 0.0, tz: 0.0).

### 仿真4.1

&emsp;令 $\mathbf{K_1^{'}}=\begin{bmatrix}
 1600*1.2&  0&  640& 0\\ 
 0&  1600&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$

&emsp;仿真视频:

<video src="./video_simulation/fx_1.2.mp4" controls="controls"></video>
&emsp;蓝色框相对于白色框在图像左边太靠右, 在图像右边太靠左, 且图像宽度相对较小. 

&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.578.

### 仿真4.2

&emsp;$\mathbf{K_1^{'}}=\begin{bmatrix}
 1600&  0&  640& 0\\ 
 0&  1600*1.2&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$

<video src="./video_simulation/fy_1.2.mp4" controls="controls"></video>
&emsp;蓝色框相对于白色框在图像上边太靠下, 在图像下边太靠上, 且图像高度相对较小. 

&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.501.

### 仿真4.3

&emsp;$\mathbf{K_1^{'}}=\begin{bmatrix}
 1600&  0&  640*1.1& 0\\ 
 0&  1600&  480& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$ 

<video src="./video_simulation/u0_1.1.mp4" controls="controls"></video>
&emsp;蓝色框相对于白色框在图像上太靠左.

&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.378.

### 仿真4.4

&emsp;$\mathbf{K_1^{'}}=\begin{bmatrix}
 1600&  0&  640& 0\\ 
 0&  1600&  480*1.1& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}$ 

<video src="./video_simulation/v0_1.1.mp4" controls="controls"></video>
&emsp;蓝色框相对于白色框在图像上太靠上.

&emsp;此仿真Camera2中蓝色框和白色框IOU均值为0.375.



### 结论

&emsp; 对于Camera1的内参来说, 如果$u_0$太大, 则投影后的框相对于真值来说太靠左, 反之, 则太靠右; 如果$v_0$太大, 则投影后的框相对于真值来说太靠上, 反之, 则太靠下; 如果$f_{x_0}$太大, 则投影后的框相对于真值来说在图像左边太靠左, 在图像右边太靠右, 且矩形框宽度较小, 反之, 则情况相反; 如果$f_{y_0}$太大, 则投影后的框相对于真值来说在图像上边太靠下, 在图像下边太靠上, 且矩形框高度较小, 反之, 则情况相反.

&emsp;Camera2的内参对投影后矩形框的影响与Camera1完全相反, 这里就不具体视频展示了.



## 真实数据集

&emsp;根据以上仿真结果, 可以得到如下结论:

* 相比于考虑两相机之间的平移向量, 将其忽略可以得到更好的投影结果.
* 两相机间的旋转矩阵不是造成反投影矩形框在图像左右两边相差较大的主要因素.
* 相机内参对反投影矩阵的影响较大, 且其对图像不同区域的反投影矩形框有着不同的影响. 

### 参数

&emsp;对我们采集到的图像数据进行实验对比, 我们仅对长焦距相机的内参进行一定程度的修改:

&emsp;长焦距相机的标定内参为:
$$
\mathbf{K_1}=\begin{bmatrix}
 2388.07&  0&  1045.0& 0\\ 
 0&  2585.31&  636.0& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}
$$

&emsp;本文修正后的内参为:
$$
\mathbf{K_1^{'}}=\begin{bmatrix}
 2388.07 * 1.06&  0&  1045.0 * 0.99& 0\\ 
 0&  2585.31&  636.0 * 0.92& 0\\ 
 0&  0&  1& 0\\ 0& 0& 0& 1
\end{bmatrix}
$$

&emsp;实验结果如下:

&emsp;其中, 左侧蓝色框为手动标记的区域, 右侧蓝色框为$\mathbf{K_1}$时的映射框, 右侧绿色区域为$\mathbf{K_1^{'}}$时得到的映射区域.

<figure class="half" align =center>
    <img src="./video_simulation/out1/157.bmp" width = "390"> 
    <img src="./video_simulation/out2/157.bmp" width = "390">
</figure>



<figure class="half" align =center>
    <img src="./video_simulation/out1/158.bmp" width = "390"> 
    <img src="./video_simulation/out2/158.bmp" width = "390">
</figure>



<figure class="half" align =center>
    <img src="./video_simulation/out1/159.bmp" width = "390"> 
    <img src="./video_simulation/out2/159.bmp" width = "390">
</figure>




<figure class="half" align =center>
    <img src="./video_simulation/out1/160.bmp" width = "390"> 
    <img src="./video_simulation/out2/160.bmp" width = "390">
</figure>



<figure class="half" align =center>
    <img src="./video_simulation/out1/240.bmp" width = "390"> 
    <img src="./video_simulation/out2/240.bmp" width = "390">
</figure>



<figure class="half" align =center>
    <img src="./video_simulation/out1/241.bmp" width = "390"> 
    <img src="./video_simulation/out2/241.bmp" width = "390">
</figure>



<figure class="half" align =center>
    <img src="./video_simulation/out1/243.bmp" width = "390"> 
    <img src="./video_simulation/out2/243.bmp" width = "390">
</figure>



&emsp; 可以看出, 对Camera1的内参做调整之后, 得到的矩形框要明显好于之前的.