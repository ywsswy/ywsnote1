-- this is an example for SPJ execise
-- S 供应商
-- P 零件
-- J 工程
-- SPJ 关系
group: SPJ-DB
S = {
Sno:string, Sname:string, Status:number, City:string
S1, jingyi, 20, tianjin
S2, shengxi, 10, beijing
S3, dongfanghong, 30, beijing
S4, fengtaisheng, 20, tianjin
S5, weimin, 30, shanghai
}
P = {
 Pno:string, Pname:string, Color:string, Weight:string
P1, luomu, red, 12
P2, luoshuan, green, 17
P3, luosidao, blue, 14
P4, luosidao, red, 14
P5, tulun, blue, 40
P6, chilun, red, 30
}
J = {
 Jno:string, Jname:string, City:string
J1,` sanjian, beijing
J2, yiqi, changchun
J3, tanhuangchang, tianjin
J4, zaochuanchang, tianjin
J5, jichechang, tangshan
J6, wuxiandianchang, changzhou
J7, bandaotichang, nanjing
}
SPJ = {
 Sno:string, Pno:string, Jno:string, Qty:string
S1, P1, J1, 200
S1, P1, J3, 100
S1, P1, J4, 700
S1, P2, J2, 100
S2, P3, J1, 400
S2, P3, J2, 200
S2, P3, J4, 500
S2, P3, J5, 400
S2, P5, J1, 400
S2, P5, J2, 100
S3, P1, J1, 200
S3, P3, J1, 200
S4, P4, J1, 100
S4, P6, J3, 300
S4, P6, J4, 200
S5, P2, J4, 100
S5, P3, J1, 200
S5, P6, J2, 200
S5, P6, J4, 500
}

