# JupyterNotebook实践
## 1. 对数据的操作
### 步骤1
%matplotlib inline
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
### 步骤2
df = pd.read_csv('fortune500.csv')
df.head()
### 图
![image](https://github.com/user-attachments/assets/2fac0c95-1e41-4b82-8043-0301070dd286)
### 步骤3
df.tail()
### 图
![image](https://github.com/user-attachments/assets/bdabb6b9-9cd7-4fd0-8f3c-04dfd0760466)
### 步骤4
df.columns = ['year', 'rank', 'company', 'revenue', 'profit']
len(df)
### 图
![image](https://github.com/user-attachments/assets/87b3ee7c-c8fe-495a-99e3-f072188dfd41)
### 步骤5
df.dtypes
### 图
![image](https://github.com/user-attachments/assets/aa18c3e0-08a3-446b-8766-95506e2025d6)
### 步骤6
non_numberic_profits = df.profit.str.contains('[^0-9.-]')
df.loc[non_numberic_profits].head()
### 图
![image](https://github.com/user-attachments/assets/2b8e4bb9-99d0-479f-8c12-5a16681bcf02)
### 步骤7
len(df.profit[non_numberic_profits])
### 图
![image](https://github.com/user-attachments/assets/ab45566c-0116-4c1b-bd8d-f3e57100b666)
### 步骤8
bin_sizes, _, _ = plt.hist(df.year[non_numberic_profits], bins=range(1955, 2006))
### 图
![image](https://github.com/user-attachments/assets/4b1f2d00-d978-45d1-9e95-a00704087331)
### 步骤9
df = df.loc[~non_numberic_profits]
df.profit = df.profit.apply(pd.to_numeric)
len(df)
### 图
![image](https://github.com/user-attachments/assets/266bbb1e-0f62-4d36-9daa-4765031c04a0)
### 步骤10
df.dtypes
### 图
![image](https://github.com/user-attachments/assets/a78e6290-f402-4d77-9259-74c9b275383a)
### 步骤11
group_by_year = df.loc[:, ['year', 'revenue', 'profit']].groupby('year')
avgs = group_by_year.mean()
x = avgs.index
y1 = avgs.profit
def plot(x, y, ax, title, y_label):
    ax.set_title(title)
    ax.set_ylabel(y_label)
    ax.plot(x, y)
    ax.margins(x=0, y=0)
fig, ax = plt.subplots()
plot(x, y1, ax, 'Increase in mean Fortune 500 company profits from 1955 to 2005', 'Profit (millions)')
### 图
![image](https://github.com/user-attachments/assets/a1216f4e-8e6d-48b3-a48e-817a6a6ab597)
### 步骤12
y2 = avgs.revenue
fig, ax = plt.subplots()
plot(x, y2, ax, 'Increase in mean Fortune 500 company revenues from 1955 to 2005', 'Revenue (millions)')
### 图
![image](https://github.com/user-attachments/assets/b836eb6e-d9ff-4c70-a26a-561d6d434362)
### 步骤13
def plot_with_std(x, y, stds, ax, title, y_label):
    ax.fill_between(x, y - stds, y + stds, alpha=0.2)
    plot(x, y, ax, title, y_label)
fig, (ax1, ax2) = plt.subplots(ncols=2)
title = 'Increase in mean and std Fortune 500 company %s from 1955 to 2005'
stds1 = group_by_year.std().profit.values
stds2 = group_by_year.std().revenue.values
plot_with_std(x, y1.values, stds1, ax1, title % 'profits', 'Profit (millions)')
plot_with_std(x, y2.values, stds2, ax2, title % 'revenues', 'Revenue (millions)')
fig.set_size_inches(14, 4)
fig.tight_layout()
### 图
![image](https://github.com/user-attachments/assets/37b28aa6-9161-4077-a966-8064b22fe9e0)

## 1. 排序算法的实现
def selection_sort(arr):
    """
    选择排序算法实现
    :param arr: 待排序的列表
    :return: 排序后的列表
    """
    n = len(arr)
    for i in range(n):
        # 找到未排序部分的最小元素索引
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        # 将最小元素交换到已排序部分的末尾
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
        print(f"第 {i+1} 轮排序后: {arr}")  # 打印每轮排序结果
    return arr

# 测试排序
unsorted_list = [64, 25, 12, 22, 11]
print("原始数组:", unsorted_list)
sorted_list = selection_sort(unsorted_list.copy())  # 使用copy避免修改原列表
print("排序后数组:", sorted_list)

![image](https://github.com/user-attachments/assets/460d1447-3360-4174-a94a-45c8592759c5)

