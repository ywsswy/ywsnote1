////////////////////////////////////
<!DOCTYPE html>
<html lang="zh-cn">

    <head>
        <meta charset="utf-8">
        <title>课堂演示</title>
    </head>

    <body>
        <script>
            var jsonstr = '[{"name":"aa","age":"b0"}]';
            var jsonobj = JSON.parse(jsonstr, function(key, value) {
                if (key == 'name') {
                    return 'VIP会员：' + value;
                } 
                else
                {
                    return value;//return everything else unchanged
                }
            });
            alert(jsonobj[0].age);
           // alert(jsonobj[0].age);
        </script>
    </body>

</html>

//解析
A：全
	   var jsonstr='[{"name":"aa","age":"30"}]';
           var jsonobj=JSON.parse(jsonstr);
           alert(jsonobj[0].age);
B:修饰着解析：见问题return
//序列化
A:全	
            var jsonstr=JSON.stringify(jsonobj)
B：筛选着得串
	//1，数组，第一项取obj中的age，第二项取name
	var jsonstr=JSON.stringify(jsonobj,['age','name'])
	//2.匿名函数，同解析
            var jsonstr=JSON.stringify(jsonobj,function(key,value){
            	if(key=='name'){return '名'+value
            	}else{return value}
            })
	//if(里面必须是等于)
	else{后面必须原样返回}
	//第三个参数

	
