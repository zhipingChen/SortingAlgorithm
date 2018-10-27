![](https://upload-images.jianshu.io/upload_images/9738807-49705f26ac74381f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

归并排序是通过分治的方式，将待排序集合拆分为多个子集合，对子集合排序后，合并子集合成为较大的子集合，不断合并最终完成整个集合的排序。

> **以下所讲归并都是指二路归并：**
之前的冒泡、选择和插入排序都是维持一个待排序集合和一个已排序集合，在每次的迭代过程中从待排序集合中移动一个元素到已排序集合中，通过不断的迭代来完成排序，所以需要进行的迭代次数一般都是 $O(N)$ 级别。而归并排序则是每轮迭代消除半数的待排序子集合，所以需要进行的迭代次数为 $O(log_2N)$ 级别。

### 算法过程

> 以递增排序为例

1. 将集合尽量拆分为两个元素个数相等的子集合，并对子集合继续拆分，直到拆分后的子集合元素个数为 1；
2. 将相邻子集合进行合并成为有序集合，若集合个数为奇数则最末尾集合不参与此次合并；
3. 重复步骤 2，直到集合个数为 1
---
##### 合并操作

设有两个已排序集合 $L_1$ 和 $L_2$，集合中元素个数分别为 $n_1$ 和 $n_2$，则合并 $L_1$ 和 $L_2$ 的操作为：

1. 声明一个大小为 $n_1+n_2$ 的集合 $L$ 用于存放合并后元素；
2. 声明 $index_1$ 和 $index_2$ 两个变量分别指向两个集合中的首元素；
3. 比较 $index_1$ 和 $index_2$ 指向的元素大小，将较小的元素存放到集合 $L$ 中，并更新变量指向下一个元素；
4. 重复步骤 3，直到 $L_1$ 和 $L_2$ 中一个集合的元素已全部移动到集合 $L$ 中，则将另一个集合中未移动的元素全部添加到集合 $L$ 中

##### 合并操作示例

![merge](https://upload-images.jianshu.io/upload_images/9738807-4acea8c0bb37ee39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

$index_1$ 指向集合一中首元素位置，即指向元素 1 ，$index_2$ 指向集合二中首元素位置，即指向元素 3。比较 1 和 3 并将元素 1 存放到临时集合中，更新 $index_1$ 指向元素 5。比较 5 和 3  并将元素 3 存放到临时集合中，更新 $index_2$ 指向元素 7。再次比较并将元素 5 存放到临时集合中，此时集合一中所有元素都放到了临时集合中，则将集合二中剩余所有元素添加到临时集合中。

通过以上过程可以发现，若集合一和集合二中元素个数皆是 $n$：
* 若集合一中最小元素，大于集合二中最大元素，则只需要比较 $n$ 次即可
* 若两集合中元素呈现交叉形式，如：[1, 3, 5]、[2, 4, 6]，则需要的比较次数为 $2*n-1$

##### 合并操作代码

```
def merge(arr, left, right, end):
    # 声明index_1、index_2分别指向两个集合的首元素位置
    # 声明temp_arr_index指向临时集合的首元素位置
    index_1, index_2, temp_arr_index = left, right, left
    # 比较两集合元素，选择较小的元素添加到临时集合中
    while index_1 < right and index_2 <= end:
        if arr[index_1] <= arr[index_2]:
            temp_arr[temp_arr_index] = arr[index_1]
            index_1 = index_1 + 1
        else:
            temp_arr[temp_arr_index] = arr[index_2]
            index_2 = index_2 + 1
        temp_arr_index = temp_arr_index + 1
    # 集合1元素并未全部添加到临时集合，则添加剩余元素到临时集合中
    while index_1 < right:
        temp_arr[temp_arr_index] = arr[index_1]
        index_1 = index_1 + 1
        temp_arr_index = temp_arr_index + 1
    # 集合2元素并未全部添加到临时集合，则添加剩余元素到临时集合中
    while index_2 <= end:
        temp_arr[temp_arr_index] = arr[index_2]
        index_2 = index_2 + 1
        temp_arr_index = temp_arr_index + 1
    temp_arr_index = left
    # 将临时集合中元素复制回arr集合中，即完成了两个子集合的合并
    while temp_arr_index <= end:
        arr[temp_arr_index] = temp_arr[temp_arr_index]
        temp_arr_index = temp_arr_index + 1
```
---
### 算法过程示例

![recursive merge sort](https://upload-images.jianshu.io/upload_images/9738807-243855a33a26f827.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图中所示为使用递归方式完成归并排序的过程。若集合只有一个元素，则该集合为有序的，所以将原始集合拆分为多个只有单个元素的子集合后，则每次合并选择的两个集合都是有序集合。然后将合并后的有序集合再进行合并，回溯执行，直到合并后的集合包含所有元素，即完成了排序。

### 算法递归代码

```
def mergeSort(arr, left, right):
    if not right > left:
        return
    middle = left + (right - left) // 2
    mergeSort(arr, left, middle)   # 左半部分集合进行排序
    mergeSort(arr, middle + 1, right)   # 右半部分集合进行排序
    merge(arr, left, middle + 1, right)   #将左右子集合合并，即完成整个集合的排序
```
---
### 非递归实现

归并排序的思路是通过不断合并有序子集合来构成更大的有序集合，直到合并后的集合包含所有元素。

##### 循环合并过程

![non_recursive merge sort](https://upload-images.jianshu.io/upload_images/9738807-1b7888869425ed4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在循环方式的归并排序中，随着集合中元素个数的增多，不断调整集合与下一个集合的间距来完成合并。

### 非递归代码

```
def non_recursive_mergeSort(arr):
    global temp_arr   #临时序列存储空间
    temp_arr = [None] * len(arr)
    gap_len = 1  #集合之间的间距
    while gap_len < len(arr):
        left = 0
        while left + gap_len < len(arr): #判断下一个集合是否存在
            right = left + gap_len
            if right + gap_len - 1 < len(arr):  #判断下一个集合的尾元素位置
                end = right + gap_len - 1
            else:
                end = len(arr) - 1
            merge(arr,left,right,end)
            left = left + gap_len * 2
        gap_len = gap_len * 2
```
##### 代码分析 ：

* 第一层循环为判断停止条件，即集合之间的间距不小于原始集合的长度时排序完成。因为集合的间距以指数形式增长，所以元素个数为 $N$ 的集合，迭代的次数为 $log_2N$ 级别；
* 嵌套循环作用是遍历合并相邻的两个子集合。
### 算法分析

归并排序是一种稳定排序算法，排序过程中，如果两个元素值相等，则不交换元素位置。对于 $N$ 个元素的序列：

* 最坏情况下，当每两个待合并集合的元素大小呈现交叉形式时，需要比较的次数为两集合元素个数之和减一。即 $N$ 个元素的集合，共需要比较的次数最多为：$(2^0*2-1)*N/2^1 + (2^1*2-1)*N/2^2+...+(2^{log_2N-1}*2-1)*N/2^{log_2N}$，公式中的每一项，括号中表示两个集合合并需要进行的比较次数，即元素个数之和减一。括号后乘的系数为拆分后每一层有多少组集合。即最坏情况下的比较次数为：$Nlog_2N-N+1$

* 最好情况下，当待合并的两个集合中，其中一个集合的最小元素大于另一个集合的最大元素时，需要比较的次数为其中一个集合的元素个数。即 $N$ 个元素的集合，共需要比较的次数最多为：$2^0*N/2^1 + 2^1*N/2^2+...+2^{log_2N-1}*N/2^{log_2N}$，即最好情况下的比较次数为：$(Nlog_2N)/2$

无论是最好情况或者最坏情况下，每两个集合的合并操作都需要移动全部元素到临时集合中，再从临时集合中移动回原集合中，所以归并排序中元素的移动次数为：$(Nlog_2N)/2*2*2=2Nlog_2N$。根据算法执行的比较次数和元素移动次数可知，算法的时间复杂度为：$O(Nlog_2N)$。算法执行过程中，需要申请额外的序列空间来保存临时元素，所以算法的空间复杂度为 $O(N)$。
