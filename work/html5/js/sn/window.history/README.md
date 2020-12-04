浏览器针对每个网站窗口维护一个History栈。

执行pushState函数【修改】可压入url至当前指针上一个位置
当执行back操作时，history栈大小并不会改变（history.length不变），仅仅移动当前指针的位置；back和forward在当前大小的history栈中自由移动。


window.close();//关闭窗口 ajax 后使用才有效？
https://www.jb51.net/article/88587.htm 【关闭窗口的例子】

window对象引用不是必须的。所以window.history.go() == history.go()


window.location.reload();
window.history.go(-1);//不同与点击浏览器的后退按钮，浏览器的后退按钮不会刷新页面的哟


window.onload=function(){ 
    alert("页面加载完成！"); 
}

document.body.innerHTML = document.getElementById("#md").innerHTML;
//////////////////////////////////////////////////////////////////////////////
有趣：
history.go(-1);alert('hh');//这句alert写在go后面但是却能成功执行
history.back(-1); alert('ok'); location=document.referrer;//返回history栈前一页，同时还能让其重新加载，中间加一句alert是为了让后一个函数有时间反应过来，不然很可能失效
history.go(-1);//返回history栈前一页，但不让其重新加载

