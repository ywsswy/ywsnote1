http://dbis-uibk.github.io/relax/calc.htm?lang=en
UIBK – R, S, T
create new dataset
CQUPT-DB
preview
use group in editor
Relational Algreba
σ Sage = 20 (Student)
σ Sdept = 'IS' (Student)
【尽量先σ，再挑有用的π，不得以再⨝】
查询至少选修了一门其直接先行课为 5 号课程的学生姓名
//效率高
πSname (πSno (σCpno='5' Course⨝ πSno,Cno SC)⨝πSno,Sname Student
//效率低，但是不容易错的：
π Sname (σ Cpno='5' ((π Cno,Cpno (Course)) ⨝ (π Sno,Cno (SC)) ⨝ (π Sno,Sname (Student))))
//效率最低，但不容易错的：
π Sname (σ Cpno='5' (Course ⨝ SC ⨝ Student))
π Sno (σ Jno='J1' ∧ Pno='P1' (SPJ)) 
（and）
【谁的对：4）求没有使用天津供应商生产的红色零件的工程号 JNO： 】
πJno SPJ - (πJno (πJno,Pno SPJ⨝πJno (σCity='tianjin' J)⨝πPno (σColor='red' P)))
π Jno (SPJ) - π Jno ( σ City='tianjin' ∧ Color ='red' (S⨝SPJ⨝P))
R⨝R.C<S.E S
【使用注意】
括号/表前是数字/字母，则括号/表与数字/字母之间要有空格
