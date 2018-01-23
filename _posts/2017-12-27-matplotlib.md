---
layout: post
title: matplotlib常用方式练习
date: 2017-12-27
categories: blog
tags: [matplotlib，数据分析，python]
description: matplotlib常用方式。
---

* 学习matplotlib常见的使用方式，通过写本文进行梳理和记忆，并持续增补。示例中的数值并不一定是真实记录，只做练习使用。

> 参考资料：
[从零开始学Python【1】--matplotlib(条形图)](https://www.kesci.com/apps/home/project/59ed8d7418ec724555a9b4c0)  
[更多图形示例见官网](https://matplotlib.org/index.html#)

## 条形图
### 1.简单垂直条形图
示例：
中国的四个直辖市分别为北京市、上海市、天津市和重庆市，其2017年上半年的GDP分别为12406.8亿、13908.57亿、9386.87亿、9143.64亿。

代码如下：
```
# 导入绘图模块
import matplotlib.pyplot as plt
# 构建数据
GDP = [12406.8,13908.57,9386.87,9143.64]

# 中文乱码的处理
plt.rcParams['font.sans-serif'] =['Microsoft YaHei']
plt.rcParams['axes.unicode_minus'] = False

# 绘图
plt.bar(range(4), GDP, align = 'center',color='steelblue', alpha = 0.8)
# 添加轴标签
plt.ylabel('GDP')
# 添加标题
plt.title('四个直辖市GDP大比拼')
# 添加刻度标签
plt.xticks(range(4),['北京市','上海市','天津市','重庆市'])
# 设置Y轴的刻度范围
plt.ylim([5000,15000])

# 为每个条形图添加数值标签
for x,y in enumerate(GDP):
    plt.text(x,y+100,'%s' %round(y,1),ha='center')

# 显示图形
plt.show()
```

结果如下图：
![matplotlib1.png (640×480)](https://raw.githubusercontent.com/hujieying/Python-data-analysis/master/pic/matplotlib1.png)

代码解读：
* 由于matplotlib对中文的支持并不是很友好，所以需要提前对绘图进行字体的设置，即通过rcParams来设置字体，这里将字体设置为微软雅黑，同时为了避免坐标轴不能正常的显示负号，也需要进行设置；
* bar函数指定了条形图的x轴、y轴值，设置x轴刻度标签为水平居中，条形图的填充色color为铁蓝色，同时设置透明度alpha为0.8；
* 添加y轴标签、标题、x轴刻度标签值，为了让条形图显示各柱体之间的差异，将y轴范围设置在5000~15000；
* 通过循环的方式，添加条形图的数值标签；


### 2.简单水平条形图
示例：
《python数据分析实战》在亚马逊、当当网、中国图书网、京东和天猫的最低价格分别为39.5、39.9、45.4、38.9、33.34

代码如下：
```
# 导入绘图模块
import matplotlib.pyplot as plt
# 构建数据
price = [39.5,39.9,45.4,38.9,33.34]

# 中文乱码的处理
plt.rcParams['font.sans-serif'] =['Microsoft YaHei']
plt.rcParams['axes.unicode_minus'] = False

# 绘图
plt.barh(range(5), price, align = 'center',color='steelblue', alpha = 0.8)
# 添加轴标签
plt.xlabel('价格')
# 添加标题
plt.title('不同平台书的最低价比较')
# 添加刻度标签
plt.yticks(range(5),['亚马逊','当当网','中国图书网','京东','天猫'])
# 设置Y轴的刻度范围
plt.xlim([32,47])

# 为每个条形图添加数值标签
for x,y in enumerate(price):
    plt.text(y+0.1,x,'%s' %y,va='center')
# 显示图形    
plt.show()
```

结果如下图：
![matplotlib2.png (640×480)](https://raw.githubusercontent.com/hujieying/Python-data-analysis/master/pic/matplotlib2.png)

代码解读：
* 水平条形图的绘制与垂直条形图的绘制步骤一致，只是调用了barh函数来完成。需要注意的是，条形图的数值标签设置有一些不一样，需要将标签垂直居中显示，使用va参数即可。

### 3.水平交错条形图
示例：
利用水平交错条形图对比2016年和2017年亿万资产超高净值家庭数（top5）

代码如下：
```
# 导入绘图模块
import matplotlib.pyplot as plt
import numpy as np
# 构建数据
Y2016 = [15600,12700,11300,4270,3620]
Y2017 = [17400,14800,12000,5200,4020]
labels = ['北京','上海','香港','深圳','广州']
bar_width = 0.35

# 中文乱码的处理
plt.rcParams['font.sans-serif'] =['Microsoft YaHei']
plt.rcParams['axes.unicode_minus'] = False

# 绘图
plt.bar(np.arange(5), Y2016, label = '2016', color = 'steelblue', alpha = 0.8, width = bar_width)
plt.bar(np.arange(5)+bar_width, Y2017, label = '2017', color = 'indianred', alpha = 0.8, width = bar_width)
# 添加轴标签
plt.xlabel('Top5城市')
plt.ylabel('家庭数量')
# 添加标题
plt.title('亿万财富家庭数Top5城市分布')
# 添加刻度标签
plt.xticks(np.arange(5)+bar_width,labels)
# 设置Y轴的刻度范围
plt.ylim([2500, 19000])

# 为每个条形图添加数值标签
for x2016,y2016 in enumerate(Y2016):
    plt.text(x2016, y2016+100, '%s' %y2016)

for x2017,y2017 in enumerate(Y2017):
    plt.text(x2017+bar_width, y2017+100, '%s' %y2017)
# 显示图例
plt.legend()
# 显示图形
plt.show()
```
结果如下图：
![matplotlib3.png (640×480)](https://raw.githubusercontent.com/hujieying/Python-data-analysis/master/pic/matplotlib3.png)

代码解读：
* 水平交错条形图绘制的思想很简单，就是在第一个条形图绘制好的基础上，往左移一定的距离，再去绘制第二个条形图，所以在代码中会出现两个bar函数；
* 图例的绘制需要在bar函数中添加label参数；color和alpha参数分别代表条形图的填充色和透明度；
* 给条形图添加数值标签，同样需要使用两次for循环的方式实现；

## 饼图
饼图的绘制可以使用matplotlib库中的pie函数，先来解读函数：

> plt.pie(x, explode=None, labels=None, colors=None, autopct=None, pctdistance=0.6, shadow=False, labeldistance=1.1, startangle=None, radius=None, counterclock=True, wedgeprops=None, textprops=None, center=(0, 0), frame=False)

	x：指定绘图的数据；

	explode：指定饼图某些部分的突出显示，即呈现爆炸式；

	labels：为饼图添加标签说明，类似于图例说明；

	colors：指定饼图的填充色；

	autopct：自动添加百分比显示，可以采用格式化的方法显示；

	pctdistance：设置百分比标签与圆心的距离；

	shadow：是否添加饼图的阴影效果；

	labeldistance：设置各扇形标签（图例）与圆心的距离；

	startangle：设置饼图的初始摆放角度；

	radius：设置饼图的半径大小；

	counterclock：是否让饼图按逆时针顺序呈现；

	wedgeprops：设置饼图内外边界的属性，如边界线的粗细、颜色等；

	textprops：设置饼图中文本的属性，如字体大小、颜色等；

	center：指定饼图的中心点位置，默认为原点

	frame：是否要显示饼图背后的图框，如果设置为True的话，需要同时控制图框x轴、y轴的范围和饼图的中心位置；

示例：
借用芝麻信用近300万失信人群的样本统计数据，该数据显示，从受教育水平上来看，中专占比25.15%，大专占比37.24%，本科占比33.36%，硕士占比3.68%，剩余的其他学历占比0.57%。

代码如下：
```
# 导入第三方模块
import matplotlib.pyplot as plt

# 设置绘图的主题风格（不妨使用R中的ggplot分隔）
plt.style.use('ggplot')

# 构造数据
edu = [0.2515,0.3724,0.3336,0.0368,0.0057]
labels = ['中专','大专','本科','硕士','其他']

explode = [0,0.1,0,0,0]  # 用于突出显示大专学历人群
colors=['#9999ff','#ff9999','#7777aa','#2442aa','#dd5555'] # 自定义颜色

# 中文乱码和坐标轴负号的处理
plt.rcParams['font.sans-serif'] = ['Microsoft YaHei']
plt.rcParams['axes.unicode_minus'] = False

# 将横、纵坐标轴标准化处理，保证饼图是一个正圆，否则为椭圆
plt.axes(aspect='equal')

# 控制x轴和y轴的范围
plt.xlim(0,4)
plt.ylim(0,4)

# 绘制饼图
plt.pie(x = edu, # 绘图数据
        explode=explode, # 突出显示大专人群
        labels=labels, # 添加教育水平标签
        colors=colors, # 设置饼图的自定义填充色
        autopct='%.1f%%', # 设置百分比的格式，这里保留一位小数
        pctdistance=0.8,  # 设置百分比标签与圆心的距离
        labeldistance = 1.15, # 设置教育水平标签与圆心的距离
        startangle = 180, # 设置饼图的初始角度
        radius = 1.5, # 设置饼图的半径
        counterclock = False, # 是否逆时针，这里设置为顺时针方向
        wedgeprops = {'linewidth': 1.5, 'edgecolor':'green'},# 设置饼图内外边界的属性值
        textprops = {'fontsize':12, 'color':'k'}, # 设置文本标签的属性值
        center = (1.8,1.8), # 设置饼图的原点
        frame = 1 )# 是否显示饼图的图框，这里设置显示

# 删除x轴和y轴的刻度
plt.xticks(())
plt.yticks(())
# 添加图标题
plt.title('芝麻信用失信用户教育水平分布')

# 显示图形
plt.show()
```

结果如下图：
![matplotlib4.png (640×480)](https://raw.githubusercontent.com/hujieying/Python-data-analysis/master/pic/matplotlib4.png)

## 线箱图
等我后续再更新，哈哈



#### CHANGELOG
20171217 初稿
