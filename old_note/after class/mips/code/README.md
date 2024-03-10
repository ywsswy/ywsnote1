	.data
userin:	.word 0
string1: .asciiz "Please input a positive integer:" 
	
	.text
main:	#input
	li	$v0,4
	la	$a0,string1
	syscall
	li	$v0,5
	syscall
	sw	$v0,userin
	##part 1
	#s2 = 1<<s3
	lw	$s4,userin
	li	$s1,1
	li	$s3,32
loo1:	sub	$s3,$s3,1
	sllv	$s2,$s1,$s3
	and	$s5,$s2,$s4
	
	li	$v0,1
	move 	$a0,$s5
	syscall 
	bgt	$s3,0,loo1
