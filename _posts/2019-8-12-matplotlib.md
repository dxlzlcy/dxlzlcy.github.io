---
category: 学习
title: 学习matplotlib
---

### 如何支持中文

[参考](https://blog.csdn.net/Fantasy_Muse/article/details/78585049)

下载`SimHei.ttf`，拷贝到`/anaconda3/lib/python3.7/site-packages/matplotlib/mpl-data/fonts/ttf`

修改`/anaconda3/lib/python3.7/site-packages/matplotlib/mpl-data/matplotlibrc`文件，添加两行

```
axes.unicode_minus  : False 
font.family         : SimHei
```



删除`/Users/liuchengyi/.matplotlib/fontlist-v300.json`文件，之后会新生成一个文件

重启kernel，搞定。



### 柱状图

```python
df = pd.DataFrame(np.random.rand(5,3)*100,columns=list('xyz'))
plt.bar(np.arange(5),df['x'],align='edge')
plt.bar(np.arange(5),df['y'],bottom=df.x,align='edge')
plt.bar(np.arange(5),df['z'],bottom=df.x+df.y,align='edge')
plt.xlabel('班级')
plt.ylabel('成绩')
plt.title('各班语数英总成绩')

plt.show()
```

横向

```python
df = pd.DataFrame(np.random.rand(5,3)*100,columns=list('xyz'))
plt.barh(np.arange(5),df['x'],align='edge')
plt.barh(np.arange(5),df['y'],left=df.x,align='edge')
plt.barh(np.arange(5),df['z'],left=df.x+df.y,align='edge')
plt.xlabel('班级')
plt.ylabel('成绩')
plt.title('各班语数英总成绩')

plt.show()
```

折线图

```python
plt.figure(figsize=[10,8])
plt.plot(np.arange(10),np.random.rand(10),'magenta',marker='o',alpha=0.3,label='品红色')
plt.plot(np.arange(10),np.random.rand(10),'c',marker='s',alpha=0.3,label='青色',linewidth=4,linestyle='--')
plt.xlim([-2,12])
plt.ylim([-1,2])
plt.xticks(np.arange(-2,12))


plt.legend()
# plt.savefig('a.png')

plt.show()

```



设置刻度

```python
plt.figure(figsize=[12,8])
x = np.linspace(-5,5,200)
plt.plot(x,np.sin(x),label='$sine$')
plt.plot(x,np.cos(x),'c--',label='$cosine$')
plt.legend()

plt.ylim(-1.5,1.5)
plt.xticks([-np.pi,-np.pi/2,0,np.pi/2,np.pi],['$-\pi$','$-\pi/2$','0','$\pi/2$','$\pi$'])

plt.show()
```

设置dpi

```python
plt.rcParams['figure.dpi'] = 300
```

