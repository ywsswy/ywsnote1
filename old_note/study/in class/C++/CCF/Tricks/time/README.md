    clock_t start_time=clock();
    cout << start_time << endl;
    for(int i = 0;i<gn;i++){
        gve1[i] = i*i;
        //gma1[i] = i*i;
    }
    clock_t end_time=clock();
    cout << end_time << endl;
    cout<< ": "<<static_cast<double>(end_time-start_time)/CLOCKS_PER_SEC*1000<<"ms"<<endl;//输出运行时间
#!/bin/bash
#gettime.sh
st=`date "+%M:%S.%N"`
$1
et=`date "+%M:%S.%N"`
smi=${st:0:2}
emi=${et:0:2}
ss=${st:3:2}
es=${et:3:2}
sms=${st:6:3}
ems=${et:6:3}
dmi=`expr $emi - $smi`
ds=`expr $es - $ss`
dms=`expr $ems - $sms`
dtotal=`expr \( $dmi \* 60 + $ds \) \* 1000 + $dms`
echo "***** Gettime.sh ***** Used: $dtotal ms *****"
#
yGcd
1e7 -> 3.5s
