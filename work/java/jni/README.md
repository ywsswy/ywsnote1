https://www3.ntu.edu.sg/home/ehchua/programming/java/JavaNativeInterface.html
```
// String入参的情况：
JNIEXPORT void JNICALL Java_HelloJNI_wtf
  (JNIEnv * env, jobject, jstring str) {
        const char *cstr = env->GetStringUTFChars(str, NULL);  // 这个要求不能有不可打印字符，不能有中文
        int slen = env->GetStringLength(str);
        std::string s(cstr, slen);
        std::cout << s << std::endl;
        env->ReleaseStringUTFChars(str, cstr);  // 释放由str创建的cstr
}

// String[]入参的情况：
JNIEXPORT void JNICALL Java_HelloJNI_wtf
  (JNIEnv * env, jobject, jobjectArray inas) {
  int len = env->GetArrayLength(inas);
    for (int i = 0; i < len; i++) {
        jstring str = (jstring) env->GetObjectArrayElement(inas, i); 
        const char *cstr = env->GetStringUTFChars(str, NULL);
        int slen = env->GetStringLength(str);
        std::string s(cstr, slen);
        std::cout << s << std::endl;
        env->ReleaseStringUTFChars(str, cstr);  // 释放由str创建的cstr
    }   
}

// String出参的情况：

JNIEXPORT jstring JNICALL Java_HelloJNI_getss
  (JNIEnv *env, jobject, jint n) {
    std::string s("xhh\na bc");
    s[5] = 0;
    jsize len = s.size();
    jchar* ustr = new jchar[len + 1]; 
    for (int i = 0; i < len; i++) {
        ustr[i] = (jchar)(s[i]);
    }   
    jstring jstr = env->NewString(ustr, len);
    delete[] ustr;
    return jstr;
}

// jbyteArray字符数组出参的情况：
  jbyteArray jdata = env->NewByteArray(s.size()); // 创建Java字节数组
  env->SetByteArrayRegion(jdata, 0, s.size(), (jbyte*)s.c_str()); // 将C++字符串复制到Java字节数组中
  return jdata;

```