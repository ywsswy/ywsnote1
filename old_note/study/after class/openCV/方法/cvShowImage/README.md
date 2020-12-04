CVAPI(void) cvShowImage( const char* name, const CvArr* image );
	cvShowImage("CSI1", img);
add:
	
	cvReleaseImage(&img);
