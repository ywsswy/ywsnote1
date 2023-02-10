报错： 
Access restriction:The type JPEGCodec is not accessible due to restriction on required library C:\Program Files\Java\jre6\lib\rt.jar 
  
解决方法： 
Project -> Properties -> libraries， 
先remove掉JRE System Library，然后再Add Library重新加入。 
