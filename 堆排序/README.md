![](https://upload-images.jianshu.io/upload_images/9738807-a1b28e0a473470e0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 从[二叉搜索树](https://www.jianshu.com/p/ff4b93b088eb)和[平衡二叉树](https://www.jianshu.com/p/655d83f9ba7b)的介绍中，可以发现二叉树这种结构具有一个很好的特性，当有序的二叉树构造完成之后，更改树中节点后，只需要 $O(log_2N)$ 的时间复杂度即可将二叉树重新调整为有序状态。若构造出一种具有特殊节点顺序的二叉树，使得每次对二叉树执行插入或删除节点操作后，都调整保持二叉树根节点的值为树中节点的极值，则 $N$ 个元素的集合，构造出这种二叉树后，只需要对树执行 $N-1$ 次的取根节点操作，即可获得一个有序序列。整个取节点加调整操作的时间复杂度为 $O(Nlog_2N)$，若构造这种二叉树的时间复杂度不高于 $O(Nlog_2N)$，则采用构造这种二叉树的方式来完成排序的时间复杂度为 $O(Nlog_2N)$。

### 堆定义

上面提到的利用具有特殊节点顺序的二叉树完成排序的方式，就是堆排序。这里所说的节点顺序是指：树中每个节点的值都不小于（不大于）它的子节点值。堆描述的是一颗完全二叉树，在对数组进行排序的过程中，并不是真的构建一个二叉树结构，只是将数组中元素下标映射到完全二叉树，利用元素下标来表示父节点和子节点关系。

![list type](https://upload-images.jianshu.io/upload_images/9738807-8aa76404a03c4199.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![tree type](https://upload-images.jianshu.io/upload_images/9738807-7b5c0e761c7f0546.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过以上两张图可知，堆中父子节点的下标关系为：
* 下标为 $i$ 的节点，其左子节点下标为 $2*i+1$
* 下标为 $i$ 的节点，其右子节点下标为 $2*i+2$
* 下标为 $i$ 的节点，其父节点下标为 $\lfloor {\frac {i-1} 2} \rfloor(i\ge1)$

### 算法过程

> 以递增排序为例，集合初始为待排序集合，已排序集合为空

1. 构造最大堆，即调整待排序集合，使得元素映射出的完全二叉树，满足每个节点元素值都不小于其子节点值
2. 替换待排序集合中第一个元素和最后一个元素值，即在待排序集合映射出的完全二叉树上，将根节点值和树中最下面一层、最右边的节点值进行替换
3. 调整堆结构使其满足节点大小顺序，标记待排序集合最后一个元素为已排序
4. 重复步骤2, 3，直到待排序集合只有一个元素

### 演示示例

##### 调整为最大堆结构

要保证每个节点的值不小于其左右子节点的值，只需要从后往前遍历集合中每个具有子节点的节点，使得其节点值不小于左右子节点的值即可（递归与子节点进行比较）。已知下标为 $i$ 的节点，其父节点下标为 $\lfloor {\frac {i-1} 2} \rfloor(i\ge1)$，所以具有 $N$ 个元素的集合，起始遍历节点的下标为 $\lfloor { {\frac {N} 2} -1} \rfloor(i\ge1)$。

> 起始待调整元素下标为 4，即值为 2 的节点

![](https://upload-images.jianshu.io/upload_images/9738807-4db61e9b150300b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 1 次调整后，下一个待调整元素下标为 3，即值为 0 的节点

![](https://upload-images.jianshu.io/upload_images/9738807-08afc613966ea36c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 2 次调整后，下一个待调整元素下标为 2，即值为 4 的节点

![](https://upload-images.jianshu.io/upload_images/9738807-8c6af943e1c064bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 3 次调整后，下一个待调整元素下标为 1，即值为 3 的节点。这里注意，节点 3 与子节点 9 比较并交换后，需要递归与子节点进行比较，直到值不小于子节点值

![step_1](https://upload-images.jianshu.io/upload_images/9738807-058af1328932368f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![step_2](https://upload-images.jianshu.io/upload_images/9738807-0ea625599239700d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 4 次调整后，下一个待调整元素下标为 0，即值为 5 的节点。同样涉及递归操作

![](https://upload-images.jianshu.io/upload_images/9738807-d3c42d535bbfdad1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 5 次调整后，当前结构即为最大堆

![](https://upload-images.jianshu.io/upload_images/9738807-156ae70bd59e10ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 调整代码

```
def transformToHeap(arr, index, end):
    targetIndex, leftChildIndex, rightChildIndex = index, 2 * index + 1, 2 * index + 2
    if leftChildIndex < end and arr[leftChildIndex] > arr[targetIndex]:
        targetIndex = leftChildIndex
    if rightChildIndex < end and arr[rightChildIndex] > arr[targetIndex]:
        targetIndex = rightChildIndex
    if not targetIndex == index:
        arr[index], arr[targetIndex] = arr[targetIndex], arr[index]
        transformToHeap(arr, targetIndex, end)
```
代码中声明 $targetIndex$ 用于指向根节点、左右子节点中的最大节点，若需要替换节点值，则递归调整替换后的根节点和其左右子节点。$end$ 变量用于标志待排序集合的边界。

##### 迭代获取堆顶元素

重复将待排序集合首元素和尾元素进行替换，标记替换后的尾元素为已排序，并调整堆结构使其重新成为最大堆。

> 起始待替换根节点为 9，第 1 次替换并调整后结构后（调整过程上面已列出）
待排序集合：[8, 7, 4, 6, 5, 1, 2, 3, 0]
已排序集合：[9]

![](https://upload-images.jianshu.io/upload_images/9738807-7b38beb9498e5aa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 下一个待替换根节点为 8，第 2 次替换并调整后结构后
待排序集合：[7, 6, 4, 3, 5, 1, 2, 0]
已排序集合：[8, 9]

![](https://upload-images.jianshu.io/upload_images/9738807-b83249eae8475cb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
...
...
...
> 下一个待替换根节点为 0，第 9 次替换并调整后结构后
待排序集合：[0]
已排序集合：[1, 2, 3, 4, 5, 6, 7, 8, 9]

![](https://upload-images.jianshu.io/upload_images/9738807-d2ea34a2bac42fb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

观察以上过程可知，每次排序后待排序集合元素数减一。$N$ 个元素的序列，经过 $N-1$ 次排序后，待排序集合元素数为一，即完成排序。

##### 迭代操作代码

```
def heapSort(arr):
    index = len(arr) // 2 - 1
    while index >= 0:
        transformToHeap(arr, index, len(arr))  # transform arr to heap arr
        index = index - 1
    num = 1
    while num < len(arr):
        arr[0], arr[-num] = arr[-num], arr[0]
        transformToHeap(arr, 0, len(arr) - num)  # transform arr to heap arr
        num = num + 1
```
代码中第一个循环为构造最大堆，第二个循环为替换待排序集合首尾元素，并调整最大堆。

### 算法分析

堆排序是一种不稳定排序算法，对于 $N$ 个元素的序列，构造堆过程，需要遍历的元素次数为 $O(N)$，每个元素的调整次数为 $O(log_2N)$，所以构造堆复杂度为 $O(Nlog_2N)$。迭代替换待排序集合首尾元素的次数为 $O(N)$，每次替换后调整次数为 $O(log_2N)$，所以迭代操作的复杂度为 $O(Nlog_2N)$。由此可知堆排序的时间复杂度为 $O(Nlog_2N)$，排序过程属于原地排序，不需要额外的存储空间，所以空间复杂度为 $O(1)$。
