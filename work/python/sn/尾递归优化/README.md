解决递归调用栈溢出的方法是通过尾递归优化


在函数返回的时候，调用自身本身，并且，return语句不能包含表达式


return n * fact(n - 1)#错误

return fact_iter(num - 1, num * product)#正确
