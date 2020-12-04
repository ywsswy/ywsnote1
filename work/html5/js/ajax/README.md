////////////////////////


/lib/jquery/1.9.1/jquery.min.js
创建对象
var btn=document.getElementById('btn')
        btn.onclick=function(){
        	var myajax=new XMLHttpRequest()
        }
//IE6不兼容，有其他

连接服务器
open(method,url,async)
method：GET
async:true:异步，

   	myajax.open('GET','',true);
发送请求
   	myajax.send(null);

接收数据（服务器响应）
XMLHttpRequest：
responseText 属性（返回字符串形式的响应）
onreadystatechange事件（readyState属性变化时触发）
readyState：0：未初始化，1：已建立连接，2：请求已接受
	3：请求处理中，4：请求已完成，且响应已就绪
status：200：OK
	404：未找到



var btn=document.getElementById('btn')
        btn.onclick=function(){
        	var myajax=new XMLHttpRequest()

        	myajax.open('GET','tyws.txt',true);
        	myajax.send(null);

        	myajax.onreadystatechange=function(){
        		if(myajax.readyState==4){
        			if(myajax.status==200){
        				alert('成功')
        			}else{
        				alert('失败')
        			}
        		}
        	}
        }


