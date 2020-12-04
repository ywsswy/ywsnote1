struct DMatch
{              //三个构造函数
          DMatch():
queryIdx(-1),trainIdx(-1),imgIdx(-1),distance(std::numeric_limits<float>::max()) {}
          DMatch(int  _queryIdx, int  _trainIdx, float  _distance ) :
queryIdx( _queryIdx),trainIdx( _trainIdx), imgIdx(-1),distance( _distance) {}
          DMatch(int  _queryIdx, int  _trainIdx, int  _imgIdx, float  _distance ) :                   queryIdx(_queryIdx), trainIdx( _trainIdx), imgIdx( _imgIdx),distance( _distance) {}
          intqueryIdx;  //此匹配对应的查询图像的特征描述子索引
          inttrainIdx;   //此匹配对应的训练(模板)图像的特征描述子索引
          intimgIdx;    //训练图像的索引(若有多个)
          float distance;  //两个特征向量之间的欧氏距离，越小表明匹配度越高。
          booloperator < (const DMatch &m) const;
};
