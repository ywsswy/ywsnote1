202.202.43.106
//80x86微机原理及接口技术实验教程
//实验实习报告册（团队完成内容）
实验设备：TD-PITE
自己的电脑：TD-DEBUGER？轻松汇编
步骤：连接仪器（试验箱是真实的16机）
打开wmd86显示复位成功
第二章：
2.1系统认知实验
//CODE 与START交叉嵌套
//只有stack段定义时需要后跟stack
//(?)代表随机数
SSTACK SEGMENT STACK
	DW 32 DUP(?)
SSTACK ENDS
CODE SEGMENT
	ASSUME CS:CODE,SS:SSTACK
START:	PUSH 	DS
	XOR 	AX,AX
	MOV 	DS,AX
	MOV 	SI,3000H
	MOV 	CX,16
AA1:	MOV 	[SI],AL
	INC 	SI
	INC	AL
	LOOP	AA1
	MOV	AX,4C00H
	INT 21H
CODE ENDS
	END START
	

