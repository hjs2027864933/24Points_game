# 24Points_game
```python3
import tkinter as tk, os  # 导入tkinter模块
from tkinter.messagebox import *
import random  # 导入random模块
import time
import operator
import itertools
import re

pai_list = ['1', '10', '11', '12', '13',
            '1', '2', '3', '4', '5', '6', '2', '7', '8', '9', '10', '11', '12', '13',
            '1', '2', '3', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13',
            '4', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '5', '11', '12', '13',
            '6', '7', '8', '9']
cardNum = []  # 存放随机牌组
listSet = []  # 存放随机牌组对
cardGroup = ()  # 调用牌组
symbols = ["+", "-", "*", "/"]  # 存放运算符
cardOne = 0
cardTwo = 0
cardThr = 0
cardFor = 0  # 存放卡牌信息
resultOne = 0
resultTwo = 0
resultThr = 0  # 存放运算计算结果
cardValue = []  # 保存结果打印信息
cardResult = []  # 存放运算结果


class Application( tk.Frame ):  # 定义GUI应用程序类，派生于Frame类
    def __init__(self, master=None):  # 构造函数，master为父窗口
        self.files = os.listdir( r'C:\Users\hjs2027864933\Desktop\Python_实验\card' )  # 获取图像文件名列表
        self.index = 52  # 图片索引，初始显示
        self.answers=[]
        self.ans_nums=[]
        self.imgs = [tk.PhotoImage( file=r'C:\Users\hjs2027864933\Desktop\Python_实验\card' + '\\' + self.files[self.index] ) for i in range(4)]
        tk.Frame.__init__( self, master )  # 调用父类的构造函数
        self.pack()  # 调用组件的pack方法，调整其显示位置和大小
        f1 = tk.Frame( self, bg='Green' )
        f1.grid( row=0, column=0 )
        tk.Label( f1, text="Enter an expression" ).grid( row=0, column=0 )
        self.btnRef = self.CrtbtnRef( f1 )
        self.verify = self.veritfy( f1 )
        self.ans = self.Eny_ans( f1 )  # 第一行框架
        f2 = tk.Frame( self )
        f2.grid( row=1, column=0 )  # 第二行框架
        f3 = tk.Frame( self )
        f3.grid( row=2, column=0 )
        self.btnGet_ans = self.CrtbtnGet_ans( f3 )  # 第三行框架
        self.Solution = self.solution( f3 )
        self.LBImages=self.createWidgets( f2 )  # 调用对象方法，创建子组件

    def createWidgets(self, f2):  # 对象方法：创建子组件
        LBImage=[]
        for i in range(0,4):
            LBImage.append(tk.Label( f2, width=80, height=160 ))
            LBImage[i]['image']=self.imgs[i]
            LBImage[i].grid(row=1,column=i)
        return LBImage

    def solution(self, f3):
        temp = tk.StringVar()
        entry_sloution = tk.Entry( f3, textvariable=temp )
        temp.set( 'Solution to be displayed here!' )

        # temp.set(str(answer))
        # 按键之后，覆盖显示参考答案
        entry_sloution['width'] = 26
        entry_sloution.grid( row=0, column=2 )
        return entry_sloution

    def veritfy(self, f1):
        button = tk.Button( f1, text='veritfy', command=self.Veritfy )
        button.grid( row=0, column=2 )
        button['width'] = 20
        return button

    def Eny_ans(self, f1):
        temp = tk.StringVar()
        entry_answer = tk.Entry( f1, textvariable=temp )
        temp.set( 'Please enter you answer!' )
        # temp.set(str)
        entry_answer['width'] = 26
        entry_answer.grid( row=0, column=1 )
        return entry_answer

    def CrtbtnGet_ans(self, f3):
        button = tk.Button( f3, text="Find a Solution", command=self.Solution )
        button.grid( row=0, column=1 )
        button['width'] = 20
        return button

    def CrtbtnRef(self, f1):
        button = tk.Button( f1, text="Refresh", command=self.refresh )
        # button.bind("<Button-1>", self.refresh)
        button.grid( row=0, column=3 )
        button['width'] = 16
        # button.pack(side="right")
        return button

    def refresh(self):
        # 定义事件处理程序
        while True:
            self.ans_nums.clear()
            s = []
            while (len( s ) < 4):
                x = random.randint( 0, 51 )
                if (x not in s):
                    s.append( x )
            for i in s:
                value = pai_list[i]
                self.ans_nums.append( int( value ) )
            self.ans_nums.sort()
            self.answers=self.cardCompute()
            if self.answers:
                break

        for i in range(0,4):
            self.imgs[i]=tk.PhotoImage(file=r'C:\Users\hjs2027864933\Desktop\Python_实验\card' + '\\' + self.files[s[i]] )
            self.LBImages[i]['image']=self.imgs[i]

    def Veritfy(self):
        a = []
        answer = eval( self.ans.get() )
        my_str = str( self.ans.get() )
        print( my_str )
        str1 = 'You Got it!'
        str2 = 'You must use the four cards shown'
        str3 = str( self.ans.get() ) + 'is not 24'
        a = re.findall( r'\d+', my_str )
        for i in range( 0, len( a ) ):
            a[i] = int( a[i] )
        a.sort()

        if (operator.eq( self.ans_nums, a ) and answer == 24):
            showinfo( "Correct", str1 )
        elif (operator.eq( self.ans_nums, a ) and answer != 24):
            showerror( "Incorrect", str3 )
        else:
            showerror( "Incorrect", str2 )

    # 计算方法
    def cardCompute(self):
        # print(w)
        cardValue = []
        cardList = list( set( itertools.permutations( self.ans_nums, 4 ) ) )
        for i in range( len( cardList ) ):
            cardGroup = cardList[i]
            cardOne = cardGroup[0]
            cardTwo = cardGroup[1]
            cardThr = cardGroup[2]
            cardFor = cardGroup[3]
            flag = False
            # 下面的循环运算体系会有数学上逻辑上的报错，所以用try检测
            try:
                for s1 in symbols:
                    resultOne = 0
                    if s1 == "+":
                        resultOne = cardOne + cardTwo
                    elif s1 == "-":
                        resultOne = cardOne - cardTwo
                    elif s1 == "*":
                        resultOne = cardOne * cardTwo
                    elif s1 == "/":
                        resultOne = cardOne / cardTwo
                    for s2 in symbols:
                        resultTwo = 0
                        if s2 == "+":
                            resultTwo = resultOne + cardThr
                        elif s2 == "-":
                            resultTwo = resultOne - cardThr
                        elif s2 == "*":
                            resultTwo = resultOne * cardThr
                        elif s2 == "/":
                            resultTwo = resultOne / cardThr
                        for s3 in symbols:
                            resultThr = 0
                            resultelse = 0
                            if s3 == "+":
                                resultThr = resultTwo + cardFor
                                resultelse = cardThr + cardFor
                            elif s3 == "-":
                                resultThr = resultTwo - cardFor
                                resultelse = cardThr - cardFor
                            elif s3 == "*":
                                resultThr = resultTwo * cardFor
                                resultelse = cardThr * cardFor
                            elif s3 == "/":
                                resultThr = resultTwo / cardFor
                                resultelse = cardThr / cardFor

                            # 判断最终结果是否为24
                            if resultThr == 24:
                                cardValue.append( "((%s %s %s) %s %s ) %s %s = 24" % (
                                cardOne, s1, cardTwo, s2, cardThr, s3, cardFor) )
                                flag = True
                            # 括号与括号的运算
                            elif resultThr != 24 and 24 % resultOne == 0:
                                for s4 in symbols:
                                    resultThr = 0
                                    if s4 == "+":
                                        resultThr = resultOne + resultelse
                                    elif s4 == "-":
                                        resultThr = resultOne - resultelse
                                    elif s4 == "*":
                                        resultThr = resultOne * resultelse
                                    elif s4 == "/":
                                        resultThr = resultOne / resultelse
                                    if resultThr == 24:
                                        cardValue.append( "(%s %s %s) %s (%s %s %s) = 24" % (
                                        cardOne, s1, cardTwo, s4, cardThr, s3, cardFor) )
                                        flag = True
                                    if flag:
                                        break
                                        # 如果得到结果，就退出3次运算的循环
                            if flag:
                                break
                        if flag:
                            break
                    if flag:
                        break
            except ZeroDivisionError:
                pass
        return cardValue

    def Solution(self):
        print(self.answers)
        temp = tk.StringVar()
        self.Solution['textvariable']= temp
        temp.set( self.answers[random.randint(0,len(self.answers)-1)] )


root = tk.Tk()  # 创建1个Tk根窗口组件root
root.title( '24点' )  # 设置窗口标题
# root.geometry('450x220')#设置窗口大小
root.configure( bg='Green' )
# tk.Entry(f1).grid(row=2,column=1,columnspan=2)
app = Application( master=root )  # 创建Application的对象实例
app.mainloop()  # 调用组件的mainloop方法，进入事件循环
