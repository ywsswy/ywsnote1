CVAPI(int) cvNamedWindow( const char* name, int flags CV_DEFAULT(CV_WINDOW_AUTOSIZE) );
	cvNamedWindow("CNW1", CV_WINDOW_AUTOSIZE);
add:
	cvDestroyWindow("CNW1");
