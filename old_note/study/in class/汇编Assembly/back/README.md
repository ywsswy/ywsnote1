;-----------------------------------------------------------------------
;	This EXE program displays "Hiwang!" to screen.
;-----------------------------------------------------------------------
;
;.486
;.model flat
;include C:\RadASM\Masm\Inc\RADbg.inc
assume	cs:code,ds:segd1,ss:segs1
ymaxlen	equ	9;ymaxlen-1 + '$'
ystr1		equ	ystr
ystr2		equ	nametab
yshowda	equ	100h
ylen		equ	5h
segd1	segment para
ydatapar	label	byte
ydata1	db	ymaxlen
ydata2	db	?
ystr		db	ymaxlen dup(?)
crlf		db	0dh,0ah,'$'
str7	db	'PLEASE INPUT:$'
nametab	db	ymaxlen dup(' '-1)
;bbb	db	10,4,10h
;www	dw	100,100h,-5
;ddd	dd	3*20,0fffdh
segd1	ends
segs1	segment	para stack
	dw	100h dup(?)
	bot	label	word
segs1	ends
code	segment para public 'code'
promain	proc	far
start:
	mov	ax,segs1
	mov	ss,ax
	mov	sp,offset bot
	
	push	ds
	sub	ax,ax
	push	ax
	
	call	proini
	
	call	procrlf
	ret
promain	endp
protest	proc 	near
	mov	ax,10h
	mov	bx,09h
do1:
	add	ax,10h
	sub	bx,08h
	cmp	ax,20h
	je	do1
	ret
protest	endp
proscst	proc	near
;ydatapar label byte,label[0] maxlen
;label[1] reallen
;out:label[2..2+maxlen-1] str
;change:ah dx bx
	mov	ah,0ah
	lea	dx,ydatapar
	int	21h
	mov	bh,0
	mov	bl,ydata2
	mov	ystr1[bx],'$'
	ret
proscst endp
proprda	proc	near
;change:dl cx ah bx
;yshowda:memory length
;ystr1:start address
;call:proprch
;label:prdalo
	mov	cx,yshowda
	mov	ah,02
	lea	bx,ystr1
prdalo:
	mov	dl,[bx]
	inc	bx
	call	proprch
	loop	prdalo
	ret
proprda endp
proscch	proc	near
;keyboard input a char(showback,notneed enter)
;out:al
;change:ah
	mov	ah,1
	int	21h
	ret
proscch endp
proprch	proc	near
;in:dl
;change:ah
	mov	ah,2
	int	21h
	ret
proprch endp
proini	proc	near	
	mov	ax,segd1
	mov	ds,ax
	ret
proini endp
procrlf	proc	near
;change:dl ah
	mov	dl,0dh
	mov	ah,2
	int 	21h
	mov	dl,0ah
	mov	ah,2
	int	21h
	ret
procrlf	endp
proprst	proc	near
;ystr1:start address
;change:ah dx
	mov	ah,9
	mov	dx,offset ystr1
	int	21h
	ret
proprst endp
prostor	proc	near
;movstr
;ylen:strlen
;ystr1:source start address
;ystr2:destination start address
;change:ax,ds,es,si,di,cx
	mov	ax,ds
	mov	es,ax
	cld
	lea	si,ystr1
	lea	di,ystr2
	mov	cx,ylen
	rep	movsb
	ret
prostor endp
proscnum	proc	near
;scan a num(0-65535)(stop while not in 0-9)
;out:bx
;change:bx,ax,cx
;lalel:newchar,exit
	mov	bx,0
newchar:
	mov	ah,1
	int	21h
	sub	al,30h
	jl	exit
	cmp	al,9d
	jg	exit
	cbw
	xchg	ax,bx
	mov	cx,0ah
	mul	cx
	xchg	ax,bx
	add	bx,ax
	jmp	newchar
exit:
	ret
proscnum endp
protohex	proc	near
;show num(0-0xff) in hex
;in:bx
;change:cx,ax,dl
;label:rotate printit
	mov	ch,4
rotate:
	mov	cl,4
	rol	bx,cl
	mov	al,bl
	and	al,0fh
	add	al,30h
	cmp	al,3ah
	jl	printit
	add	al,7h
printit:
	mov	dl,al
	mov	ah,2
	int	21h
	dec	ch
	jnz	rotate
	ret
protohex endp
progetkey	proc	near
;get keyboard buffer
;call:protohex
;change:ax,bx,cx,dx
	mov	ah,1
	int	16h
	mov	bx,ax
	call	protohex
	ret
progetkey endp
code	ends
	end	start

