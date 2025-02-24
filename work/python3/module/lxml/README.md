ystr1 = '//*[@id="stuPanel"]/table/tbody/tr[1]/td[2]/div[2]/font'
ytd = yxpath.xpath(ystr1)[0]
print(ytd.xpath('string(.)')) #获取一个节点下的显示内容
