构造函数是private的
写一个public的
inline static <CLASS> &getInstance()
{
static <CLASS> m_sInstance;
return m_sInstance;
}