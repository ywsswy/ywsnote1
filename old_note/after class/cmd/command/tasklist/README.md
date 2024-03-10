显示所有进程
获取pid信息
tasklist /m /fo list /FI "PID eq 1316"
tasklist /FI "PID eq 1316"
C:\Users\Administrator>wmic process where name="tim.exe" get ExecutablePath
ExecutablePath
C:\Program Files (x86)\Tencent\TIM\Bin\TIM.exe

