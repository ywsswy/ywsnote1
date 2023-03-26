@echo off
ipconfig
pause
@echo off
rem reg add HKEY_CURRENT_USER\Software\xxx /v v2 /t REG_SZ /d hh3 /f
echo 当前目录是：%cd%
reg add HKEY_CURRENT_USER\Software\xxx /v v2 /t REG_SZ /d %cd% /f
pause
//修改默认键值【ve】
@echo off
reg add "HKEY_CURRENT_USER\Software\Microsoft\DevStudio\6.0\AddIns\FileTool.DSAddIn.1" /ve /t REG_SZ /d 3 /f
pause
//修改普通键值
reg add "HKEY_CURRENT_USER\Software\Microsoft\DevStudio\6.0\Build 
System\Components\Platforms\Win32 (x86)\Directories" /v "Include Dirs" /t REG_SZ /d %cd%
\VC98\INCLUDE;%cd%\VC98\MFC\INCLUDE;%cd%\VC98\ATL\INCLUDE /f
reg add "HKEY_CURRENT_USER\Software\Microsoft\DevStudio\6.0\Build 
System\Components\Platforms\Win32 (x86)\Directories" /v "Library Dirs" /t REG_SZ /d %cd%
\VC98\LIB;%cd%\VC98\MFC\LIB /f
reg add "HKEY_CURRENT_USER\Software\Microsoft\DevStudio\6.0\Build 
System\Components\Platforms\Win32 (x86)\Directories" /v "Path Dirs" /t REG_SZ /d %cd%
\Common\MSDev98\Bin;%cd%\VC98\BIN;%cd%\Common\TOOLS;%cd%\Common\TOOLS\WINNT /f
reg add "HKEY_CURRENT_USER\Software\Microsoft\DevStudio\6.0\Build 
System\Components\Platforms\Win32 (x86)\Directories" /v "Source Dirs" /t REG_SZ /d %cd%
\VC98\MFC\SRC;%cd%\VC98\MFC\INCLUDE;%cd%\VC98\ATL\INCLUDE;%cd%\VC98\CRT\SRC /f
