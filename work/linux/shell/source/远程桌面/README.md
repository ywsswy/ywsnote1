if [ $# == 1 ];then
nohup rdesktop -f -a 16 -u Administrator -r clipboard:PRIMARYCLIPBOARD -r disk:share=/home/hill/72A2-A068 123.206.27.78:9833 > ~/yfolder/rdesktop.log 2>&1 &
else
nohup rdesktop -a 16 -u Administrator -r clipboard:PRIMARYCLIPBOARD -r disk:share=/home/hill/72A2-A068 123.206.27.78:9833 > ~/yfolder/rdesktop.log 2>&1 &
fi