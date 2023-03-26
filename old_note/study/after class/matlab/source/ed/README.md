function d = test3(k1,k2)
%T = 2*pi/k1;%周期由k1确定
%dt = 4321987.164352;%11111.5*T;%采样点
fprintf('有用信号A频率%dHz,幅值%fV\n',k1,1);
fprintf('干扰信号B频率%dHz,幅值%fV\n',k2,1);
dt = 0.016152;
syms x
dx=0:0.0000001:0.0001*k2;
y1 = sin(2*pi*k1*x);
dy1 = sin(2*pi*k1*dx);
y2 = sin(2*pi*k2*x);
dy2 = sin(2*pi*k2*dx);
y3 = y1^2+y2^2+2*y1*y2;
dy3 = dy1+dy2;
r1 = sqrt(eval(int(y1^2,0,dt)/dt));
r2 = sqrt(eval(int(y2^2,0,dt)/dt));
r3 = sqrt(eval(int(y3,0,dt)/dt));
r4 = max(dy1);
r5 = max(dy2);
r6 = max(dy3);
fprintf('\n有用信号A:\n');
fprintf('有效值:%fV\n',r1);
fprintf('最大值:%fV\n',r4);
fprintf('满足√2关系的相对误差为%f%%\n',100*abs(1-sqrt(2)*r1/r4));
fprintf('\n干扰信号B:\n');
fprintf('有效值:%fV\n',r2);
fprintf('最大值:%fV\n',r5);
fprintf('满足√2关系的相对误差为%f%%\n',100*abs(1-sqrt(2)*r2/r5));
fprintf('\n叠加信号C:\n');
fprintf('有效值%fV\n',r3);
fprintf('最大值:%fV\n',r6);
fprintf('满足√2关系的相对误差为%f%%\n',100*abs(1-sqrt(2)*r3/r6));
%0.7071067811865475
% 求有效值的芯片要很高速

