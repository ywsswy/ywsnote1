#Filename:xml2.py
from lxml import etree
import numpy
import re
wb_data='''
<html><head>
        <title>.:::::::教务在线  学生课表  :::::::.</title>
        <meta http-equiv="Content-type" content="text/html;charset=utf-8">    
        <script type="text/javascript" src="../js/jquery-1.12.0.min.js"></script>
        <link href="../jqUI/smoothness/jquery-ui.min.css" rel="stylesheet">  
        <script type="text/javascript" src="../jqUI/smoothness/jquery-ui.min.js"></script> 
        <link href="../css/tpl.css" rel="stylesheet">
        <script type="text/javascript"> 
            $(document).ready(function(){
                $( "#stuKbTabs" ).tabs({
                    beforeLoad: function( event, ui ) {
                        ui.panel.html('Loading...');
                    }}
                );
                 $( "#zcGuolv" ).change(function(event){
                    event.preventDefault(); 
                    $zc=$(this).val();
                   if($zc==0)$("#stuPanel table div.kbTd").show();
                   else (
                   $("#stuPanel table div.kbTd").each(function(){
                       $zcStr=$(this).attr('zc');
                    $a=parseInt($zcStr.substr($zc-1,1));
                    if($a==0) $(this).hide();
                    else $(this).show();
                   })
                   )
                });
            });
        </script>
    </head>
    <body>
        <div id="head">
            <div style="width: 1000px;margin: 0px auto;">
                <div style="float: left;">
                    <ul><li><a href="../index.php"><img src="../images/room.png" width="50px" alt=""></a></li>
                        <li>〉〉2017-2018学年2学期 学生课表&gt;&gt;2017210886  </li>
                    </ul>
                </div>
                <div style="float: right;color: rgb(221, 221, 221);"> 
                    今天是第 4 周 星期 6                       
                </div>
            </div>
        </div> 
        <div id="logo" style="background: #E0E0E0;vertical-align: middle;height: 30px;">    
        </div>
        <div style="width: 1200px;margin: auto;min-height: 500px;padding-top: 10px;">
            <div id="stuKbTabs" class="ui-tabs ui-widget ui-widget-content ui-corner-all">
                <ul class="ui-tabs-nav ui-helper-reset ui-helper-clearfix ui-widget-header ui-corner-all" role="tablist">
                    <li class="ui-state-default ui-corner-top ui-tabs-active ui-state-active" role="tab" tabindex="0" aria-controls="kbStuTabs-table" aria-labelledby="ui-id-1" aria-selected="true" aria-expanded="true"><a href="#kbStuTabs-table" class="ui-tabs-anchor" role="presentation" tabindex="-1" id="ui-id-1">按表格查询</a></li> 
                    <li class="ui-state-default ui-corner-top" role="tab" tabindex="-1" aria-controls="kbStuTabs-list" aria-labelledby="ui-id-2" aria-selected="false" aria-expanded="false"><a href="#kbStuTabs-list" class="ui-tabs-anchor" role="presentation" tabindex="-1" id="ui-id-2">按列表查询</a></li>
                    <li class="ui-state-default ui-corner-top" role="tab" tabindex="-1" aria-controls="kbStuTabs-ttk" aria-labelledby="ui-id-3" aria-selected="false" aria-expanded="false"><a href="#kbStuTabs-ttk" class="ui-tabs-anchor" role="presentation" tabindex="-1" id="ui-id-3">调停课查询</a></li>   
                </ul>
                <div id="kbStuTabs-table" aria-labelledby="ui-id-1" class="ui-tabs-panel ui-widget-content ui-corner-bottom" role="tabpanel" aria-hidden="false">
                    <div class="printTable">
                        <form onsubmit="return false;">
                            <input type="hidden" name="xh" value="2017210886">
                            周次筛选：第<select name="zc" size="1" id="zcGuolv">
                            <option value="0">全部</option>
                                <option value="1">1</option><option value="2">2</option><option value="3">3</option><option value="4">4</option><option value="5">5</option><option value="6">6</option><option value="7">7</option><option value="8">8</option><option value="9">9</option><option value="10">10</option><option value="11">11</option><option value="12">12</option><option value="13">13</option><option value="14">14</option><option value="15">15</option><option value="16">16</option><option value="17">17</option><option value="18">18</option><option value="19">19</option>                            </select>周
                        </form>
                        <div id="stuPanel">
                            <style type="text/css">.kbTd{margin-top: 8px;}</style><table><thead><tr><td>&nbsp;</td><td style="text-align:center;">星期一</td><td style="text-align:center;">星期二</td><td style="text-align:center;">星期三</td><td style="text-align:center;">星期四</td><td style="text-align:center;">星期五</td><td style="text-align:center;">星期六</td><td style="text-align:center;">星期日</td></tr></thead><tbody><tr style="text-align:center"><td style="font-weight:bold;">1、2节</td><td><div class="kbTd" zc="11111111111111110000">A00172A1050050004<br>A1050050-大学英语3<br>地点：4301 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">丁义 必修 4.0学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1050050004" target="_blank">选课学生名单</a></b></div><div class="kbTd" zc="00000000000000001000">SJ02172A2140010005<br>A2140010-金工实习B<br>地点：数控加工实训室101 
            <br>17周<font color="#FF0000">4节连上</font><br><span style="color:#0000FF">朱小均 必修 1.0学分</span><br><b><a href="kb_stuList.php?jxb=SJ02172A2140010005" target="_blank">选课学生名单</a></b></div></td><td><div class="kbTd" zc="00000000000000001000">SJ02172A2140010005<br>A2140010-金工实习B<br>地点：数控加工实训室101 
            <br>17周<font color="#FF0000">4节连上</font><br><span style="color:#0000FF">朱小均 必修 1.0学分</span><br><b><a href="kb_stuList.php?jxb=SJ02172A2140010005" target="_blank">选课学生名单</a></b></div></td><td><div class="kbTd" zc="11011111111111100000">A00172A1110100001<br>A1110100-工科数学分析（下）<br>地点：3508 
            <br>1-2周,4-15周<font color="#FF0000"></font><br><span style="color:#0000FF">张蓉 必修 5.5学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1110100001" target="_blank">选课学生名单</a></b></div><div class="kbTd" zc="00000000000000001000">SJ02172A2140010005<br>A2140010-金工实习B<br>地点：数控加工实训室101 
            <br>17周<font color="#FF0000">4节连上</font><br><span style="color:#0000FF">朱小均 必修 1.0学分</span><br><b><a href="kb_stuList.php?jxb=SJ02172A2140010005" target="_blank">选课学生名单</a></b></div></td><td><div class="kbTd" zc="00000000000000001000">SJ02172A2140010005<br>A2140010-金工实习B<br>地点：数控加工实训室101 
            <br>17周<font color="#FF0000">4节连上</font><br><span style="color:#0000FF">朱小均 必修 1.0学分</span><br><b><a href="kb_stuList.php?jxb=SJ02172A2140010005" target="_blank">选课学生名单</a></b></div></td><td><div class="kbTd" zc="01010101010101010000">SK02172A1040050001<br>A1040050-C++语言程序设计<br>地点：计算机教室（十二）(综合实验楼C408/C409) 
            <br>2-16周双周<font color="#FF0000"></font><br><span style="color:#0000FF">李盘林 必修 .0学分</span><br><b><a href="kb_stuList.php?jxb=SK02172A1040050001" target="_blank">选课学生名单</a></b></div></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">3、4节</td><td></td><td><div class="kbTd" zc="11111111111111110000">A00172A1100030010<br>A1100030-马克思主义基本原理<br>地点：4304 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">何宏兵 必修 3.0学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1100030010" target="_blank">选课学生名单</a></b></div></td><td></td><td><div class="kbTd" zc="01010101010101010000">A00172A1110290003<br>A1110290-大学物理A（上）<br>地点：3103 
            <br>2-16周双周<font color="#FF0000"></font><br><span style="color:#0000FF">陈希明   必修 3.0学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1110290003" target="_blank">选课学生名单</a></b></div></td><td></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">中午间歇</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">5、6节</td><td><div class="kbTd" zc="11111111111111110000">A00172A1110100001<br>A1110100-工科数学分析（下）<br>地点：3508 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">张蓉 必修 5.5学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1110100001" target="_blank">选课学生名单</a></b></div></td><td></td><td><div class="kbTd" zc="11111111000000000000">R172A1125060003<br>A1125060-电影艺术与文化反思<br>地点：8143 
            <br>1-8周<font color="#FF0000"></font><br><span style="color:#0000FF">吴倩 选修 1.0学分</span><br><b><a href="kb_stuList.php?jxb=R172A1125060003" target="_blank">选课学生名单</a></b></div></td><td></td><td><div class="kbTd" zc="11111101111111100000">A00172A1110100001<br>A1110100-工科数学分析（下）<br>地点：3508 
            <br>1-6周,8-15周<font color="#FF0000"></font><br><span style="color:#0000FF">张蓉 必修 5.5学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1110100001" target="_blank">选课学生名单</a></b></div></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">7、8节</td><td></td><td><div class="kbTd" zc="11111111111111110000">A00172A1110290003<br>A1110290-大学物理A（上）<br>地点：3103 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">陈希明   必修 3.0学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1110290003" target="_blank">选课学生名单</a></b></div></td><td><div class="kbTd" zc="11111111111111110000">A00172A1050050004<br>A1050050-大学英语3<br>地点：4301 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">丁义 必修 4.0学分</span><br><b><a href="kb_stuList.php?jxb=A00172A1050050004" target="_blank">选课学生名单</a></b></div></td><td></td><td><div class="kbTd" zc="11111111111111110000">A00172A1090020002<br>A1090020-大学体育2-武术<br>地点：运动场1 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">闫增印 必修 1.0学分</span><br><b><a href="kb_stuList.php?jxb=T172A1090020129" target="_blank">选课学生名单</a></b></div></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">下午间歇</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">9、10节</td><td></td><td><div class="kbTd" zc="11111111111111110000">R172A1125030001<br>A1125030-音乐基础与欣赏<br>地点：8131 
            <br>1-16周<font color="#FF0000"></font><br><span style="color:#0000FF">程敬瑜 选修 2.0学分</span><br><b><a href="kb_stuList.php?jxb=R172A1125030001" target="_blank">选课学生名单</a></b></div></td><td></td><td><div class="kbTd" zc="11111101111111111000">A02172A1040050001<br>A1040050-C++语言程序设计<br>地点：3501 
            <br>1-6周,8-17周<font color="#FF0000"></font><br><span style="color:#0000FF">李盘林 必修 3.0学分</span><br><b><a href="kb_stuList.php?jxb=A02172A1040050001" target="_blank">选课学生名单</a></b></div></td><td></td><td></td><td></td></tr><tr style="text-align:center"><td style="font-weight:bold;">11、12节</td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>                        </div>
                    </div>
                </div>
                <div id="kbStuTabs-list" aria-labelledby="ui-id-2" class="ui-tabs-panel ui-widget-content ui-corner-bottom" role="tabpanel" aria-hidden="true" style="display: none;">
                    <div class="printTable">
                        <table><thead><tr><td>教学班</td><td>课程号-课程名</td><td>类别</td><td>教学班分类</td><td>选课状态</td><td>教师</td><td>上课时间</td><td>地点</td><td>学生名单</td><td>备注</td></tr></thead><tbody><tr><td rowspan="2">A00172A1050050004</td>
                    <td rowspan="2">A1050050-大学英语3</td>
                    <td rowspan="2">必修</td><td rowspan="2" align="center">理论</td><td rowspan="2" align="center">正常 </td><td>丁义</td> 
                    <td>星期1第1-2节 1-16周</td><td>4301</td>
                    <td rowspan="2" align="center"><a href="kb_stuList.php?jxb=A00172A1050050004" target="_blank">名单</a></td>
                    <td rowspan="2"></td>
                    </tr><tr><td>丁义 </td>
                    <td>星期3 第7-8节1-16周 </td><td>4301</td></tr><tr><td rowspan="1">A00172A1090020002</td>
                    <td rowspan="1">A1090020-大学体育2-武术</td>
                    <td rowspan="1">必修</td><td rowspan="1" align="center">理论</td><td rowspan="1" align="center">正常 </td><td>闫增印</td> 
                    <td>星期5第7-8节 1-16周</td><td>运动场1</td>
                    <td rowspan="1" align="center"><a href="kb_stuList.php?jxb=A00172A1090020002" target="_blank">名单</a></td>
                    <td rowspan="1"></td>
                    </tr><tr><td rowspan="1">A00172A1100030010</td>
                    <td rowspan="1">A1100030-马克思主义基本原理</td>
                    <td rowspan="1">必修</td><td rowspan="1" align="center">理论</td><td rowspan="1" align="center">正常 </td><td>何宏兵</td> 
                    <td>星期2第3-4节 1-16周</td><td>4304</td>
                    <td rowspan="1" align="center"><a href="kb_stuList.php?jxb=A00172A1100030010" target="_blank">名单</a></td>
                    <td rowspan="1"></td>
                    </tr><tr><td rowspan="3">A00172A1110100001</td>
                    <td rowspan="3">A1110100-工科数学分析（下）</td>
                    <td rowspan="3">必修</td><td rowspan="3" align="center">理论</td><td rowspan="3" align="center">正常 </td><td>张蓉</td> 
                    <td>星期1第5-6节 1-16周</td><td>3508</td>
                    <td rowspan="3" align="center"><a href="kb_stuList.php?jxb=A00172A1110100001" target="_blank">名单</a></td>
                    <td rowspan="3"></td>
                    </tr><tr><td>张蓉 </td>
                    <td>星期3 第1-2节1-2周,4-15周 </td><td>3508</td></tr><tr><td>张蓉 </td>
                    <td>星期5 第5-6节1-6周,8-15周 </td><td>3508</td></tr><tr><td rowspan="2">A00172A1110290003</td>
                    <td rowspan="2">A1110290-大学物理A（上）</td>
                    <td rowspan="2">必修</td><td rowspan="2" align="center">理论</td><td rowspan="2" align="center">正常 </td><td>陈希明  </td> 
                    <td>星期2第7-8节 1-16周</td><td>3103</td>
                    <td rowspan="2" align="center"><a href="kb_stuList.php?jxb=A00172A1110290003" target="_blank">名单</a></td>
                    <td rowspan="2"></td>
                    </tr><tr><td>陈希明   </td>
                    <td>星期4 第3-4节2-16周双周 </td><td>3103</td></tr><tr><td rowspan="1">A02172A1040050001</td>
                    <td rowspan="1">A1040050-C++语言程序设计</td>
                    <td rowspan="1">必修</td><td rowspan="1" align="center">理论</td><td rowspan="1" align="center">正常 </td><td>李盘林</td> 
                    <td>星期4第9-10节 1-6周,8-17周</td><td>3501</td>
                    <td rowspan="1" align="center"><a href="kb_stuList.php?jxb=A02172A1040050001" target="_blank">名单</a></td>
                    <td rowspan="1"></td>
                    </tr><tr><td rowspan="1">R172A1125030001</td>
                    <td rowspan="1">A1125030-音乐基础与欣赏</td>
                    <td rowspan="1">选修</td><td rowspan="1" align="center">理论</td><td rowspan="1" align="center">正常 </td><td>程敬瑜</td> 
                    <td>星期2第9-10节 1-16周</td><td>8131</td>
                    <td rowspan="1" align="center"><a href="kb_stuList.php?jxb=R172A1125030001" target="_blank">名单</a></td>
                    <td rowspan="1"></td>
                    </tr><tr><td rowspan="1">R172A1125060003</td>
                    <td rowspan="1">A1125060-电影艺术与文化反思</td>
                    <td rowspan="1">选修</td><td rowspan="1" align="center">理论</td><td rowspan="1" align="center">正常 </td><td>吴倩</td> 
                    <td>星期3第5-6节 1-8周</td><td>8143</td>
                    <td rowspan="1" align="center"><a href="kb_stuList.php?jxb=R172A1125060003" target="_blank">名单</a></td>
                    <td rowspan="1"></td>
                    </tr><tr><td rowspan="4">SJ02172A2140010005</td>
                    <td rowspan="4">A2140010-金工实习B</td>
                    <td rowspan="4">必修</td><td rowspan="4" align="center">实验实践</td><td rowspan="4" align="center">正常 </td><td>朱小均</td> 
                    <td>星期1第1-4节 17周</td><td>数控加工实训室101</td>
                    <td rowspan="4" align="center"><a href="kb_stuList.php?jxb=SJ02172A2140010005" target="_blank">名单</a></td>
                    <td rowspan="4"></td>
                    </tr><tr><td>朱小均 </td>
                    <td>星期2 第1-4节17周 </td><td>数控加工实训室101</td></tr><tr><td>朱小均 </td>
                    <td>星期3 第1-4节17周 </td><td>数控加工实训室101</td></tr><tr><td>朱小均 </td>
                    <td>星期4 第1-4节17周 </td><td>数控加工实训室101</td></tr><tr><td rowspan="1">SK02172A1040050001</td>
                    <td rowspan="1">A1040050-C++语言程序设计</td>
                    <td rowspan="1">必修</td><td rowspan="1" align="center">实验实践</td><td rowspan="1" align="center">正常 </td><td>李盘林</td> 
                    <td>星期5第1-2节 2-16周双周</td><td>计算机教室（十二）(综合实验楼C408/C409)</td>
                    <td rowspan="1" align="center"><a href="kb_stuList.php?jxb=SK02172A1040050001" target="_blank">名单</a></td>
                    <td rowspan="1"></td>
                    </tr></tbody></table>                    </div>
                </div>
                <div id="kbStuTabs-ttk" aria-labelledby="ui-id-3" class="ui-tabs-panel ui-widget-content ui-corner-bottom" role="tabpanel" aria-hidden="true" style="display: none;">
                    <table class="pTable">
                        <thead>
                            <tr><td>序号</td><td>学期</td><td>类型</td><td>教学班</td><td>课程名称</td><td>教师</td><td>停课周</td><td>停课时段</td><td>补（代）课时间</td><td>补课地点</td><td>代课老师</td></tr>  
                        </thead>
                        <tbody>
                                                    </tbody>
                    </table>
                </div>
            </div>
        </div>
    
  </body></html>
'''
g_week = 20
def yDeal(s1,yn):
	if s1.find(',') != -1 :
		bufl = s1.split(',')
		for i in bufl:
			yDeal(i,yn)
	else:
		flag = 0
		if s1.find('双') != -1 :
			flag = 1
		elif s1.find('单') != -1:
			flag = 2
		else :
			flag = 0
		s1 = re.sub('[^\d-]','',s1)
		conti_flag = 0
		if s1.find('-') != -1:
			conti_flag = 1
		if conti_flag == 0 :
			yweek = eval(s1)-1
			yn[(yweek)] = 0
		else :
			[start,end] = s1.split('-')
			start = eval(start)-1
			end = eval(end)-1
			for i in range(start,end+1):
				if flag == 0:
					yn[(i)] = 0
				elif flag == 1 and i%2 == 1:
					yn[(i)] = 0
				elif flag == 2 and i%2 == 0:
					yn[(i)] = 0
				else:
					None
	return None
	
yn1 = numpy.ones((6,7,g_week))# g_week(0s) #1 free 0 class
yxpath = etree.HTML(wb_data)
for ytime in range(0,6):
	for yday in range(0,7):
		ystr1 = '//*[@id="stuPanel"]/table/tbody/tr['+repr(ytime+1)+']/td['+repr(yday+2)+']'
		#print(ystr1)#
		ytd = yxpath.xpath(ystr1)[0]
		ytd_len = len(ytd)
		yn_week = numpy.ones(g_week)
		for i in range(0,ytd_len):
			ystr2 = ystr1+'/div['+repr(i+1)+']/text()[4]'
			yraw = yxpath.xpath(ystr2)[0]
			yDeal(yraw,yn_week)
		#print(yn_week)
		yn1[(ytime,yday)] = yn_week.copy()
#//*[@id="stuPanel"]/table/tbody/tr[1]/td[3]/div/font
#print(yn1)
ystr1 = '//*[@id="stuPanel"]/table/tbody/tr[1]/td[2]/div[2]/font'
ytd = yxpath.xpath(ystr1)[0]
print(ytd.xpath('string(.)'))
print(ytd)
#print(help(ytd))
	
#last 3 
	
	
	
	
	
	
	
	
	
	
	
	
	

