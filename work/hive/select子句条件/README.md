CASE WHEN a THEN b [WHEN c THEN d]* [ELSE e] END
```
if a:
  echo b
elif c:
  echo d
else:
  echo e
```

这三者输出的结果相同，因为count的是行数，而不是每一行的内容
select id, count(1) as exp_pv from my_table group by id
select id, count(0) as exp_pv from my_table group by id
select id, count(*) as exp_pv from my_table group by id

同理，这三者也跟上面三个结果相同，因为不管输出的then后的值还是输出else后面的值，都是“输出”了，所以都被count计数了
select id, count(case when id = '2' then 2 else 0 end) as exp_pv from my_table group by id
select id, count(case when id = '2' then 1 else 0 end) as exp_pv from my_table group by id
select id, count(case when id = '2' then 0 else 0 end) as exp_pv from my_table group by id

如果只希望count统计某些条件的行，那么可以把不符合条件的不要输出，就是不要写else之后的内容
select id, count(case when id = '2' then 1 end) as exp_pv from my_table group by id 