
#########################################################prodict##############################################################################################
import pandas as pd
from study.Descover4 import discover_later
from study.DescoverF import discover_front
from study.Sitnubm import Sit_nubm
from study.Tbet1 import Tbet1_hot
import time
#fp = pd.read_excel('betbigC.xlsx',sheet_name='Sheet1',header=None)
def Invest_numb(start,fp,money,numb,bout,ranking,qs):
    x = 1
    y = 1
    er = 0
    k = 1
    z = 0
    o = 0
    F = 0#连续错误次数
    T = 0#连续正确次数
    R = 0
    i=0
    gl = 0
    op = 0
    groList=[1,2,3,4,5,6,7,8,9,10]
    u = 0
    listR = []
    money2 = money
    en = 0
    listp=[]
    while qs>0:#投注次数
         
        ma = discover_later(numb=numb, start=start,data=2, fp=fp, bout=bout,ranking =ranking)#确定numb后面容易跟什么
        print(ma)
        choose = Sit_nubm(start=start,data=2,fp=fp,numb=numb)+1#确定numb的位置
#         if T == 3:
#             for i in ma:
#                 groList.remove(i)
#             ma = groList
#             money = money*2
#             u = 1
#             groList =[1,2,3,4,5,6,7,8,9,10]
#         if F == 3:
#             money = money*2
#             u = 1
        if Tbet1_hot(ma=ma,choose=choose,x=start+1,fp=fp): #下注
#             if u ==1:
#                 money = money/2
#                     u = 0
            moy = (money/ranking)*10#记录盈利收益
               
            T = T+1
            F = 0 
            z = z+1
        else:
            listR.insert(i, T)#记录连续失败次数到列表
            moy = -money
            F = F+1
            o = o+1
            T = 0
            i= i+1
        if T>u:
            u = T
        if F>R:
            R = F    
        print(choose)    
        er = moy+er#记录总收益
        er = round(er)
        if y<=180:#以180为一天记录收益到表格。  
            df.iloc[x,y]=er
            y = y+1
        else: 
            y = 1
            x = x+1
        print("--------正在执行第",k,"次投注--------")
        qs = qs -1
        k = k+1
        start = start+1
    print("成功次数",z)
    print("失败次数",o)
    print("最大连续成功次数：",u)
    print("最大连续失败次数：",R)
    u = 0
    for gl in listR:
        
        u = gl+u
        if gl <11:
            
            en = en+1
        else:
            en = 0
        listp.insert(op, en)
        op = +1
    Gi = u/len(listR)#计算平均连续次数
    print("成功次数列表listR==",listR)
    print("平均连续次数：",Gi)
    return R
if __name__ == '__main__':
    pop = 0
    start =181
    listrt = []
    print("读取表格中...........")
    s1 = time.perf_counter()#记录读取表格时间
    fp = pd.read_excel('betbigC.xlsx',sheet_name='Sheet1',header=None)
    df = pd.read_excel('prodict.xlsx',sheet_name='Sheet1',header=None)
    s2 = time.perf_counter()
    print("读取总时间：",s2-s1,"s")
    while pop <=600:#按天数记录最大连续错误次数
        a =Invest_numb(start=start,fp=fp,money=180,numb=1,bout=25,ranking=8,qs=180)
        start = start+180
        listrt.insert(pop, a)
        pop = pop+1
    print(listrt)
    g = pd.value_counts(listrt).to_frame()#统计每天连续错误次数
    
    #print(g)
    g1=g.reset_index()
    print(g1)    
        
  
    #df.to_excel('prodict4.xlsx')
    print("保存成功")      
    



# list = discover_later(numb=1, start=189999, data=2, fp=fp, bout=15)
# a = pd.value_counts(list).to_frame()
# p = a.reset_index()
# getlist = p['index'].tolist()

# print(getlist[0:5])



#######################################################PrdictPro##############################################################################################


import pandas as pd 
from study.DescoverF import discover_front



#------------------斜方向号码---------------------
#当get= 1时返回出现数值次数列表，其它情况返回出现数值降序列表
def Predict_Modle1(H,ranking,fp,tensit,get):##H==开始行数（倒叙）----ranking==结果名次1,2,3在表格中的y值即名次加一---------fp==excel对象-----tensit表述第十名在表格中的y值
    x = H-1
    y = ranking+1#传递名次
    i = 0
    list = []
    while y<=tensit:
        list.insert(i, fp.iloc[x,y])
        x = x-1
        y = y+1
    print(list)
    act = pd.value_counts(list).to_frame()#转化list为pd.frame类型
    act1 = act.reset_index()
    a = act1['index'].tolist()
    b = act1['count'].tolist()
#     
    print(act1)
#     print("----------------")
#     print(a)
#     print("----------------")
#     print(list)
       
    
    if get == 1:
        return b
    else:
        return a 
    
    
#--------------出现数字numb的几个位置------------------    
def Predict_Modle2(numb,bout,start,data,fp):#返回的是列表中numb开在的真实名次列表,bout==统计之前期数的开奖频次
   
    b = 0
    getlist = []
    while 0 <= bout:
        a = 1
        list = discover_front(start=start,data=data,fp=fp)#获取开奖号码
        print(list)
        for r in list:
            if r == numb:
                print(a)
                getlist.insert(b, a)
                b = b+1
              
                
            a = a+1
        bout = bout-1
        start = start-1
    
    act = pd.value_counts(getlist).to_frame()
    act1 = act.reset_index()
    print(act)
    a = act1['index'].tolist()
    b = act1['count'].tolist()
    print(a)
    return a
    
    
    
