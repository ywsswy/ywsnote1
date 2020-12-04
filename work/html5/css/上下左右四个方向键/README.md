<div class="change_camera_direction">
	<div class="direction_content">
		<div class="direction_div top_direction">
			<a href="?dic=1" id="up"><img class="camera_img_up" src="img/up.png" /></a>
		</div>
		<div class="direction_div left_direction">
			<a href="?dic=2" id="right"><img class="camera_img_left" src="img/left.png" /></a>
		</div>
		<div class="direction_div bottom_direction">
			<a href="?dic=4" id="down"><img class="camera_img_bottom" src="img/down.png" /></a>
		</div>
		<div class="direction_div right_direction">
			<a href="?dic=3" id="right"><img class="camera_img_right" src="img/right.png" /></a>
		</div>
	</div>
</div>
.change_camera_direction{
	text-align: center;
	position: absolute;
	width: 200px;
	height: 200px;
}
.change_camera_direction .camera_title{
	color: white;
	font-size: 15px;
	margin: 5% auto;
}
.direction_content{
	 width: 100%;
	 height: 70%;
	 position: relative;
}
.direction_div{
	position:relative;
	width: 30px;
	height: 30px;
}
.direction_div img{
	width: 100%;
	height:  100%;
}
/*left*/
.left_direction {
	top: 5%;
	left: 20%; 
}
/*bottom*/
.bottom_direction{
	top:5%;
	left: 40%;
}
/*right*/
.right_direction{
	top: -39%;
	left: 61%;
}
/*top*/
.top_direction{
	top: 2%;
	left: 40%; 
}
