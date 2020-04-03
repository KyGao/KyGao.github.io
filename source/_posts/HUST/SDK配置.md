---
title: 图像实验SDK配置
date: 2019-12-06 10:50:43
tags: [课内,大三上,C方向]
categories: 
- HUST
---

当时捣鼓怎么写相机的接口属实捣鼓了很久，也在这里感谢苏新明大佬的鼎力相助，希望能帮到大家。
以下为个人历程，并非最快速的配置方案，欢迎提出建议（虽然没开通评论功能哈哈哈）。

1. 我先按照`...\E_EM\Doc`路径下的VS开发手册学习vs的配置过程走了一遍，安装MFC
（有一个很迷的bug，按照他的文档配好之后会有20多个错，需要在属性页修改）

2. 然后后来其实用不上这个了，才知道老师是允许用opencv的数据类型，所以又转到opencv开发去了（不然用二维矩阵的形式表示图像实在太鸡肋了，因为想保存在数组里还需要大量压缩一下图像，不然没办法开辟那么大的数组）。opencv的话不需要参照doc里的opencv开发手册（说多了都是泪），只需要跑通`...\E_EM\SDK\Samples_CV`里面的例程就好了。

3. 例程这里两个应该都可以，我用的是**openCV_IplImage**，具体的环境配置方法还是可以参考opencv开发手册，添加`VC++目录-包含目录`，添加`VC++目录-库目录`，修改`连接器-输入-附加依赖项`，这里注意直接用sample里的opencv2的目录即可，不需要下载opencv。

4. 这里提一下开发手册没有提到的但是会遇到的问题：
  - 首先记得改平台为x64！！
  - 一个是说int8出现类型重定义
    这个是因为opencv定义了一个int8，相机的SDK库也定义了一个int8，所以随意注释掉其中一个的定义就好
  - 一个是![](.../dll.PNG)
    这个是因为没有添加动态链接库，需要把bin里面带dll后缀的（怎么查看文件扩展名我就不说了）全部拷贝到该工程目录下，比如我用的OpenCV_IplImage，就放在`Samples_CV\OpenCV_IplImage\`目录下就好了
  - 一个是会报找不到输出文件什么什么的，具体错误我就不回去截图了，没看到的可以忽略
    这个是因为输出文件设置的不合理，按如下方式填写即可：
    ![](.../out.PNG)

5. 到这里应该可以跑通demo了，那怎么加入自己的代码呢？先在前面引入头文件，我也不知道该引什么，所以我就都引了，报错的注释掉就好:
```C++
#include "opencv2/core/core_c.h"
#include "opencv2/core/types_c.h"
#include "opencv2/imgproc/imgproc_c.h" 
#include "opencv2/imgproc/types_c.h"  
//#include "opencv2/highgui/cap_ios.h"  
#include "opencv2/highgui/highgui_c.h"  
```
在**GCapDlg.cpp**文件中，`CGCapDlg::OnStreamCB( MV_IMAGE_INFO *pInfo)`函数内，应该可以发现可以直接截到Iplimage的数据类型，（有if分支，是根据相机采集图像的方式决定的，大家可以一个个试）。然后通过
```C++
cv::Mat my_img = cv::cvarrToMat(iplSrc);
```
就可以直接变成我们熟悉的Mat数据类型了（如果用了cv的命名空间的话就不需要前面加上cv::了），需要转灰度的就`cvtColor(my_img, m, CV_BGR2GRAY);`这样转就行了。
然后在这一行以下的就是我们的TODO了，我们只需要构造函数实现
```C
/***********************
	TODO: Add function here	
	input: Mat original_img
	output: Mat processed_img
************************/
```
就可以啦