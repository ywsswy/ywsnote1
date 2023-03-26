///////////////////
//Convert to string
Dim yina,yinb,yinc,ySta,yEnd,yDel,yi,yTim
yina = InputBox("Plese input the start line:")
yinb = InputBox("Plese input the end line:")
yinc = InputBox("Plese input the delay time of every line(55-333):")
//32767
ySta = CInt(yina)
yEnd = CInt(yinb)
yDel = CInt(yinc)
MsgBox "start:"+CStr(ySta)+" end:"+CStr(yEnd)+" delay:"+CStr(yDel)
yi = ySta

