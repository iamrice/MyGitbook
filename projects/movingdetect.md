1. 图像相减 
	-图像相减之前需要转灰度吗
		-常用的颜色特征是像素点的灰度值或RGB值，它们是场景中视觉信息的最直观反映，但是两者都极易受到光照变化的影响，所以，在光线变化的环境中，基于灰度值和RGB值的运动目标检测方法都会产生大量的误检。
	-混合高斯背景减法：背景建模
2. 边框绘制 
	-二值化图片
	-连通域检测
3. 去噪
	-单纯图像相减，噪点巨多
	<img src='../images/workingPlan0.png'style='width:90% ;margin:5% ; float: left'/>

4. 可以改进的点
	-噪音，噪音有可能很大，物体也有可能小，不能只靠面积筛除
		-噪音的特点是他不是一个运动的状态，可能一闪而过，可能停留，所以想到可以为每一个物体的运动建模
	-遮挡，物体被视野中的固定物体（如电线杆)遮挡，可能会被分为两个部分
	-阴影