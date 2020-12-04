translate(x,y) 括号的百分比数据，会以本身的长宽做参考，比如，本身的长为100px，高为100px. 那填(50%,50%)就是向右，向下移动50px，添加负号就是向着相反的方向移动
.load_img{
    position:fixed;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
    z-index:1000; 
}
left50%后是左边界到画面左50%，这样中心其实偏右，所以再用translate往回移动一点
