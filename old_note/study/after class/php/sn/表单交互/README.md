<html>
<body>
<form action="welcome.php" method="post">
Name: <input type="text" name="name"><br>
E-mail: <input type="text" name="email"><br>
<input type="submit">
</form>
</body>
</html>
【后台处理
<html>
<body>
Welcome 
<?
php echo $_POST["name"]; 
?><br>
Your email address is: 
<?php 
echo $_POST["email"]; 
?>
</body>
</html>
