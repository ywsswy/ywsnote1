//IplImage IPL 图像头     do3yQ7eF3i
typedef struct _IplImage     
{         
	int  nSize;         /* IplImage大小 */   
		//与png、jpg还是bmp无关  
		//112
		//112    
	int  ID;            /* 版本 (=0)*/         
	int  nChannels;     /* 大多数 OPENCV函数支持 1,2,3 或 4 个通道 */  
		//3       
	int  alphaChannel;  /* 被 OpenCV 忽略 */         
	int  depth;         /* 像素的位深度: 
		IPL_DEPTH_8U, （8）//unsigned char 0-255
		IPL_DEPTH_8S, 
		IPL_DEPTH_16U,                                
		IPL_DEPTH_16S, 
		IPL_DEPTH_32S, 
		IPL_DEPTH_32F,（32）
		IPL_DEPTH_64F 可支持 */ 
		//矩阵中深度和通道数同时表示
		//图像中深度通道数分开表示  
	char colorModel[4]; /* 被 OpenCV 忽略 */     //RGB    
	char channelSeq[4]; /* 同上 */         	//BGR
	int  dataOrder;     /* 0 - 交叉存取颜色通道,
				IPL_DATA_ORDER_PIXEL
				BGRBGRBGR这样排列 （weight == 3）
 				1 - 分开的颜色通道.    
				IPL_DATA_ORDER_PLANE                            
			只有 cvCreateImage 可以创建交叉存取图像？？ */         
	int  origin;        /* 0 - 顶—左结构,  
			IPL_ORIGIN_TL 坐标原点为左上角                              
			1 - 底—左结构 (Windows bitmaps 风格) 
			IPL_ORIGIN_BL	左下角*/         
	int  align;         /* 图像行排列 (4 or 8). OpenCV 忽略 它，使用 widthStep 代替 */         
	int  width;         /* 图像宽像素数 */       
	int  height;        /* 图像高像素数*/         
	struct _IplROI *roi;/* 图像感兴趣区域. 
			当该值非空只对该区域进行处理 */  
		//成员变量
		coi//设置为非0值，对图像的操作就作用于被指定的通道上
		xOffset
		yOffset
		width
		height       
	struct _IplImage *maskROI; /* 在 OpenCV 中必须置 NULL */         
	void  *imageId;     /* 同上*/         
	struct _IplTileInfo *tileInfo; /*同上*/         
	int  imageSize;     /* 图像数据大小
		在交叉存取格式下 
		imageSize = image->height * image->widthStep
		单位字节*/ 
		//311364=38K
	char *imageData;  /* 指向排列的图像数据 */         
	int  widthStep;   /* 排列的图像行大小，以字节为单位 */  
		//类似CvMat的step参数  
		//weight == 332 &&  nChannel == 3 ==>widthStep = 996
		//	298		3		896
		//还需要是4的倍数？
		
	int  BorderMode[4]; /* 边际结束模式, 被 OpenCV 忽略 */         
	int  BorderConst[4]; /* 同上 */         
	char *imageDataOrigin; /* 指针指向一个不同的图像数据结构（不是必须排列的），
			是为了纠正图像内存分配准备的 */     
}IplImage;   
/*IplImage结构来自于 Intel Image Processing Library （是其本身所具 有的） . 
OpenCV 只支持其中的一个子集:  
• alpha通道在 OpenCV中被忽略.  
• colorModel 和channelSeq 被OpenCV忽略. 
OpenCV颜色转换的 唯一个函数 cvCvtColor把原图像的颜色空间的目标图像的颜色 空间作为一个参数.  
• 数据顺序 必须是IPL_DATA_ORDER_PIXEL (颜色通道是交叉存 取), 
然面平面图像的被选择通道可以被处理，就像COI（感兴趣 的通道）被设置过一样.  
• 当 widthStep 被用于去接近图像行序列，排列是被OpenCV忽略 的.  
• 不支持maskROI . 处理MASK的函数把他当作一个分离的参数. MASK在 OpenCV 里是 8-bit, 然而在 IPL他是 1-bit.  
• 名字信息不支持.  
• 边际模式和边际常量是不支持的. 每个 OpenCV 函数处理像素的 邻近的像素，通常使用单一的固定代码边际模式.  
除了上述限制, OpenCV处理ROI有不同的要求.要求原图像和目标图像 的尺寸或 ROI的尺寸必须（根据不同的作操，例如cvPyrDown 目标图 像的宽（高）必须等于原图像的宽（高）除 2 ±1)精确匹配,
而IPL处理 交叉区域，如图像的大小或ROI大小可能是完全独立的。
*/
