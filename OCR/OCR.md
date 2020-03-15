# OCR

## 算法流程

>  1. 输入图像
>
>  2. 图像预处理
>
>     图像缩放，裁剪，旋转；边缘检测；倾斜矫正；特征提取；图像去噪
>
>  3. 文本检测
>
>     传统方法：特征工程，基于opencv的图像特征处理
>
>     深度学习：CTPN网络文本检测
>
>  4. 字符分割
>
>     水平投影，垂直投影
>
>  5. 字符匹配
>
>     传统方法：模板匹配
>
>     深度学习：cnn网络识别
>
>  6. 后处理
>
>     相似度分析，先验知识校验
>
>  7. 结果输出

# 处理说明

### 倾斜矫正

>1. 旋转矫正  
>
>2. 仿射变换：一个任意的仿射变换都能表示为 *乘以一个矩阵* (线性变换) 接着再 *加上一个向量* (平移)
>
>   可以用仿射变换来表示
>
>   a. 旋转 (线性变换)
>
>   b. 平移 (向量加)
>
>   c. 缩放操作 (线性变换)
>
>   通常使用 **2*3** 矩阵来表示仿射变换.
>
>   ![ocr-1.png](.\img\ocr-1.png)
>
>对 ![ocr-2.png](.\img\ocr-2.png) 进行仿射变换
>
>![ocr-3.png](.\img\ocr-3.png) 或者 ![ocr-4.png](.\img\ocr-4.png)
>
>![ocr-5.png](.\img\ocr-5.png)
>
>原始图像X （image 1）进过 M 仿射后，得到 T （image 2）
>
>![ocr-6.png](.\img\ocr-6.jpg)
>
>实际情况中，是根据 image2 仿射得到 image1 ，根据特征关系，要求得 M 
>
>```c++
>#include "opencv2/highgui/highgui.hpp"
>#include "opencv2/imgproc/imgproc.hpp"
>#include <iostream>
>#include <stdio.h>
>
>using namespace cv;
>using namespace std;
>
>/// 全局变量
>char* source_window = "Source image";
>char* warp_window = "Warp";
>char* warp_rotate_window = "Warp + Rotate";
>
>/** @function main */
> int main( int argc, char** argv )
> {
>   // 定义一些需要用到的变量, 比如需要用来储存中间和目标图像的Mat和两个需要用来定义仿射变换的二维点数组.  
>   Point2f srcTri[3];
>   Point2f dstTri[3];
>   Mat rot_mat( 2, 3, CV_32FC1 );
>   Mat warp_mat( 2, 3, CV_32FC1 );
>   Mat src, warp_dst, warp_rotate_dst;
>
>   /// 加载源图像
>   src = imread( argv[1], 1 );
>
>   /// 以与源图像同样的类型和大小来对目标图像初始化:
>   warp_dst = Mat::zeros( src.rows, src.cols, src.type() );
>
>   /// 仿射变换: 正如上文所说, 我们需要源图像和目标图像上分别一一映射的三个点来定义仿射变换:
>   srcTri[0] = Point2f( 0,0 );
>   srcTri[1] = Point2f( src.cols - 1, 0 );
>   srcTri[2] = Point2f( 0, src.rows - 1 );
>
>   dstTri[0] = Point2f( src.cols*0.0, src.rows*0.33 );
>   dstTri[1] = Point2f( src.cols*0.85, src.rows*0.25 );
>   dstTri[2] = Point2f( src.cols*0.15, src.rows*0.7 );
>
>   // 通过这两组点, 我们能够使用OpenCV函数 getAffineTransform 来求出仿射变换
>   // 我们获得了用以描述仿射变换的 2*3 的矩阵 (在这里是 warp_mat)
>   warp_mat = getAffineTransform( srcTri, dstTri );
>
>   /// 将刚刚求得的仿射变换应用到源图像
>   warpAffine( src, warp_dst, warp_mat, warp_dst.size() );
>
>   /** 对图像扭曲后再旋转 */
>
>   /// 计算绕图像中点顺时针旋转50度缩放因子为0.6的旋转矩阵
>   Point center = Point( warp_dst.cols/2, warp_dst.rows/2 );
>   double angle = -50.0;
>   double scale = 0.6;
>
>   /// 通过上面的旋转细节信息求得旋转矩阵
>   rot_mat = getRotationMatrix2D( center, angle, scale );
>
>   /// 旋转已扭曲图像
>   warpAffine( warp_dst, warp_rotate_dst, rot_mat, warp_dst.size() );
>
>   /// 显示结果
>   namedWindow( source_window, CV_WINDOW_AUTOSIZE );
>   imshow( source_window, src );
>
>   namedWindow( warp_window, CV_WINDOW_AUTOSIZE );
>   imshow( warp_window, warp_dst );
>
>   namedWindow( warp_rotate_window, CV_WINDOW_AUTOSIZE );
>   imshow( warp_rotate_window, warp_rotate_dst );
>
>   /// 等待用户按任意按键退出程序
>   waitKey(0);
>
>   return 0;
>  }
>```
>
>

