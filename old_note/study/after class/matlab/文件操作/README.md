g=imread('C:\Users\admin\Desktop\yhz\ljjx.JPG');
imshow(g)
g1=rgb2gray(g)
level = graythresh(g1)
g2=im2bw(g1,level)
g2=im2bw(g1,0.19)
fid = fopen('C:\Users\admin\Desktop\yhz\ljj1.txt','wt');
[m, n] = size(g2);
fprintf(fid,'[')
 for i = 1 : m
    for j = 1 : n % 逐行打印出来
        fprintf(fid, '%01d,', g2(i, j)); % 注意%f后面有一个空格
     end
     fprintf(fid, ';\n');
 end
fprintf(fid,']');
fclose(fid);
fid = fopen('C:\Users\admin\Desktop\yhz\ljj2.txt','wt');
[m, n] = size(g1);
fprintf(fid,'[')
 for i = 1 : m
    for j = 1 : n % 逐行打印出来
        fprintf(fid, '%03d,', g1(i, j)); % 注意%f后面有一个空格
     end
     fprintf(fid, ';\n');
 end
fprintf(fid,']');
fclose(fid);
imshow(uint8(f2))
