## 由虚幻GlobalShader实现的体积云(非体积云组件)

该体积云项目完全由本人独立开发，为自己的硕士研究方向。该项目利用UE4的Global Shader与RDG进行自定义渲染，通过空间上采样与重投影的方法提高体积云渲染性能，在1050显卡上仅耗费***1.5ms**能够渲染出高质量体积云，渲染性能高于虚幻(**渲染用时4ms**)。同时优化体积云多重散射，进一步提高体积云的真实性。

### 1. 效果图

![image-20231109155616220](assets/image-20231109155616220.png)

![image-20231101163808780](assets/image-20231101163808780.png)

![image-20231121194132118](assets/image-20231121194132118.png)

![ScreenShot00019](assets/ScreenShot00019.png)

![ScreenShot00030](assets/ScreenShot00030.png)

### 2. 重投影与空间上采样降低整体每一帧渲染的像素数量

在空间上，将屏幕分辨率降低为原来的一半，最后利用插值的办法将结果进行上采样，有效降低整体渲染的像素数量。在时间上，通过每一帧渲染一个4x4的block中的一个像素，从而降低每一帧需要渲染的像素数量。以下为算法的流程：

![image-20231121194435245](assets/image-20231121194435245.png)

![image-20231121193440222](assets/image-20231121193440222.png)

连续16帧绘制出完整的画面

![1](assets/1.png)![2](assets/2.png)![3](assets/3.png)![4](assets/4.png)![5](assets/5.png)![6](assets/6.png)![7](assets/7.png)![8](assets/8.png)![9](assets/9.png)![10](assets/10.png)![11](assets/11.png)![12](assets/12.png)![13](assets/13.png)![14](assets/14.png)![15](assets/15.png)![16](assets/16-17005670239421.png)

通过重投影消除伪影

![image-20231130132714794](assets/image-20231130132714794.png)

空间上采样的对比图

![image-20231121194514014](assets/image-20231121194514014.png)

### 3. 多重散射近似

只有beer powder

![image-20231121194814555](assets/image-20231121194814555.png)

添加光线衰减

![image-20231121194913874](assets/image-20231121194913874.png)

添加大气透视

![image-20231121194939634](assets/image-20231121194939634.png)

添加多重散射近似

![image-20231121195014987](assets/image-20231121195014987.png)

添加Ambient Light

![image-20231121195039544](assets/image-20231121195039544.png)

添加能量守恒方程

![image-20231121195106961](assets/image-20231121195106961.png)

### 4. 自己提出的多重散射近似方法与其余两种方法的对比

![image-20231130132831613](assets/image-20231130132831613.png)

### 5. 性能对比实验

![image-20231130133007532](assets/image-20231130133007532.png)
