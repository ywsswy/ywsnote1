<html>
<body>
<?php
$TxtFileName = "Demo.txt";
//以读写方式打写指定文件，如果文件不存则创建
if( ($TxtRes=fopen ($TxtFileName,"w+")) === FALSE){
echo("创建可写文件：".$TxtFileName."失败");
exit();
}
echo ("创建/打开可写文件".$TxtFileName."成功！</br>");
$StrConents = "Welcome To ItCodeWorld!";//要写进文件的内容
if(!fwrite ($TxtRes,$StrConents)){ //将信息写入文件
echo ("尝试向文件".$TxtFileName."写入".$StrConents."失败！");
fclose($TxtRes);
exit();
}
echo ("尝试向文件".$TxtFileName."写入".$StrConents."成功！");
fclose ($TxtRes); //关闭指针
?>
</body>
</html>
