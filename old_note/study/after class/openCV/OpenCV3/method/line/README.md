////////////////////////////////////////
//! draws the line segment (pt1, pt2) in the image
CV_EXPORTS_W void line(CV_IN_OUT Mat& img, Point pt1, Point pt2, const Scalar& color,
                     int thickness=1, int lineType=8, int shift=0);
