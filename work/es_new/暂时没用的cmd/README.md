curl -X<VERB> '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>' -d '<BODY>'
VERB HTTP方法：GET, POST, PUT, HEAD, DELETE，可以使用DELETE方法删除文档，使用HEAD方法检查某文档是否存在。如果想更新已存在的文档，我们只需再PUT一次,POST方法可以不指定_id，这样就在表中自增id的方式插入了一个文档
