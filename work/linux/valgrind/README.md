valgrind --tool=memcheck --leak-check=full --error-limit=no --log-file=v.log

## 如果中途自己停掉了没有core，Memcheck, a memory error detector可能是线程数不够
--max-threads=INT

# 关注点
## Conditional jump or move depends on uninitialised value(s)
使用了未初始化的变量

# 内存泄漏查看

==12774== 
==12774== 24 bytes in 1 blocks are definitely lost in loss record 18 of 199
==12774==    at 0x4C2E4B6: operator new(unsigned long) (vg_replace_malloc.c:344)
==12774==    by 0x40426E: main (writer.cc:36) // 泄漏的代码处
==12774==
==12774== LEAK SUMMARY:
==12774==    definitely lost: 24 bytes in 1 blocks //确定的内存泄漏
==12774==    indirectly lost: 0 bytes in 0 blocks
==12774==      possibly lost: 0 bytes in 0 blocks
==12774==    still reachable: 201,701 bytes in 2,053 blocks
==12774==         suppressed: 0 bytes in 0 blocks
==12774== Reachable blocks (those to which a pointer was found) are not shown.
==12774== To see them, rerun with: --leak-check=full --show-leak-kinds=all
==12774== 
==12774== For lists of detected and suppressed errors, rerun with: -s
==12774== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)


引起core的
Invalid read of size 


kill -15来停止服务