CREATE VIEW Student（Sno，Sname，Ssex，Sage，Sdept）AS SELECT SX.Sno，SX.Sname，SY.Ssex，SX.Sage，SY.Sdept FROM SX，SY WHERE SX.Sno=SY.Sno；
mysql> create view view_choosebb(view_bb1,view_bb2,view_bb3)
    -> as select coursebb.Bb1, coursebb.bb4, coursebb.Bb5
    -> from coursebb;
Query OK, 0 rows affected (0.05 sec)
mysql>

