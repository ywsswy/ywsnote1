-- this is an example for CQUPT Database System Course
group: CQUPT-DB
Student = {
Sno:string, Sname:string, Ssex:string, Sage:number, Sdept:string
201215121, liyong, male, 20, CS
201215122, liuchen, female, 19, CS
201215123, wangmin, female, 18, MA
201215125, zhangli, male, 19, IS
}
Course = {
 Cno:string, Cname:string, Cpno:string, Ccredit:number
1, Database, 5, 4
2, Math, null, 2
3, InfomationSys, 1, 4
4, OS, 6, 3
5, DataSturcture, 7, 4
6, DataProcessing, null, 2
7, Pascal, 6, 4
}
SC = {
 Sno:string, Cno:string, Grade:number
201215121, 1, 92
201215121, 2, 85
201215121, 3, 88
201215122, 2, 90
201215122, 3, 80
}
