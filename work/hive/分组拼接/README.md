<table>表有两个字段guid，doclist
相同guid的合并（分组），然后合并时把doclist使用空格拼接起来

SELECT guid, concat_ws(' ',collect_list(doclist)) as doclist from <table> group by guid;