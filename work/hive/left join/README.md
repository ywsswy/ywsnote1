SELECT t1.id, t1.guid, t1.doclist as newlist FROM <table1> t1 LEFT JOIN <table1> t2
ON t1.guid = t2.guid where t1.ds = '1'