  y=imread(['F:\fc\buf\log.jpg'],'jpg'); %同规范文件夹相同
  yy=rgb2gray(y);
  l=graythresh(yy);
  yyy=im2bw(yy,l);
  
  fid=fopen(['F:\fc\buf\p.txt'],'at');
  [m,n]=size(yyy);
  if(m>7)
    for i=1:8:m-7
      for j=1:n
        numm=0;
        for kk=7:-1:0
          numm=numm+yyy(i+7-kk,j)*(2^kk);
        end
        fprintf(fid,'%d,',numm);
      end
      fprintf(fid,'\n');
    end
  end
  fprintf(fid,'\n\n');
  fclose(fid);
