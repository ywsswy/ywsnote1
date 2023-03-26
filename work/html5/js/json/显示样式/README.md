//////////////////////////////////////////
	<script>
             var jsonstr = '[{"name":"aa","age":"30"},{"name":"bb","age":"31"}]'
            //alert(jsonstr)
            function textJson(){
            	var jsonobj=JSON.parse(jsonstr)
            	// alert(jsonstr)
            	// alert(jsonobj)
            	output(jsonobj)
            }
            function output(o){
            	document.write('<table><tr><th>名字</th><th>年龄</th></tr>')
            	for(var i in o){
            		for(var j in o[i]){
            			document.write('<tr><td>'+j+'</td><td>'+o[i][j]+'</td></tr>')
            		}
            	}
            	document.write('</table>')
            }
            textJson();
        </script>
