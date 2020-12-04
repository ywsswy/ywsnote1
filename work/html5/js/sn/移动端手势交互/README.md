ystate = 0;//0：未触摸 1：触摸屏幕左 2：触摸屏幕右
var yy = document.getElementById('yScreen');
yy.ontouchstart = function(e){
	//console.log(e.touches[0].screenX);
	//console.log(window.screen.width/2);
	if(e.touches[0].screenX < window.screen.width/2){
		ystate = 1;
	}
	else{
		ystate = 2;
	}
}
yy.ontouchend = function(){
	ystate = 0;
}
