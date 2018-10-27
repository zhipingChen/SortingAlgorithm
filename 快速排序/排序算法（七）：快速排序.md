![](https://upload-images.jianshu.io/upload_images/9738807-7a5e22b5735aef3d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


快速排序是通过分治的方式，根据选定元素将待排序集合拆分为两个值域的子集合，并对子集合递归拆分，当拆分后的每个子集合中元素个数为一时，自然就是有序状态。

> 归并排序也是基于分治的思想，不过归并流程是将子集合合并成为有序的集合，递归执行来完成整个集合的排序。快速排序的分治流程是根据选定元素，将集合分隔为两个子集合，一个子集合中所有元素不大于选定元素值，另一个子集合中所有元素不小于选定元素值，则用于拆分集合的选定元素即为已排序元素。即每次拆分都会形成一个已排序元素，所以 $N$ 个元素的序列，拆分的次数级别为 $O(N)$。将拆分过程类比为二叉树形式，考虑普通二叉树和斜树的情况，则二叉树高度级别为 $O(log_2N)$~$O(N)$。

### 算法过程

1. 在所有集合中均选定某一个元素；
2. 根据选定元素，将每个集合拆分为元素值不大于该元素值的子集合，和元素值不小于该元素值的子集合；
3. 重复步骤 1，2，直到每个集合中元素个数为 1。

### 演示示例

假设每个集合中的选定元素 $Tr$ 为集合中的最后一个元素。

##### 拆分过程

拆分过程也就是将集合中元素值不大于 $Tr$ 的元素，和元素值不小于 $Tr$ 的元素，通过交换元素位置的方式分别移动到 $Tr$ 元素的两侧，这里不妨称之为正确区域。由此可知，在拆分过程中，若已将集合中所有小于 $Tr$ 值的元素移动到正确区域中，则拆分过程完成。

如下示例中 $B1$、$B2$ 元素值不小于 $Tr$，$S1$、$S2$和 $S3$ 元素值小于 $Tr$。

![](https://upload-images.jianshu.io/upload_images/9738807-ba7b3b28678e41f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在集合由左向右的遍历过程中，若当前元素值小于 $Tr$ 值时，则将当前元素替换到正确区域中。所以在拆分过程中需要维持两个变量 $index$ 和 $index_{right}$，分别指向当前遍历的元素位置，和正确区域尾部的下一个元素位置，或者称之为带加入正确区域的元素位置。



> 首次访问： $index = 0$，$index_{right} = 0$，皆指向 $S1$
此时正确区域为空，所以正确区域尾部的下一个元素位置也就是起始元素位置

![](https://upload-images.jianshu.io/upload_images/9738807-6bfe976a0da6201f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


因为 $S1$ 值小于 $Tr$，所以替换 $index$ 和 $index_{right}$ 指向的元素值（其实不用替换，就是同一个元素），移动 $index$ 和 $index_{right}$ 各自指向下一个元素位置。

> 第 2 次访问： $index = 1$，$index_{right} = 1$，皆指向 $B1$
此时正确区域元素为：[$S1$]，所以正确区域尾部的下一个元素就是 $B1$ 

![](https://upload-images.jianshu.io/upload_images/9738807-65e9a5e46fdef48b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

$B1$ 元素值不小于 $Tr$，所以  $index$ 指向下一个元素，$index_{right}$ 指向不变

> 第 3 次访问： $index = 2$，指向 $B2$，$index_{right} = 1$，指向 $B1$
此时正确区域元素为：[$S1$]，所以正确区域尾部的下一个元素就是 $B1$ 

![](https://upload-images.jianshu.io/upload_images/9738807-c601e0ebbb3aa0ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

$B2$ 元素值不小于 $Tr$，所以  $index$ 指向下一个元素，$index_{right}$ 指向不变

> 第 4 次访问： $index = 3$，指向 $S2$，$index_{right} = 1$，指向 $B1$
此时正确区域元素为：[$S1$]，所以正确区域尾部的下一个元素就是 $B1$ 

![](https://upload-images.jianshu.io/upload_images/9738807-4d6fb825fda12522.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

$S2$ 元素值小于 $Tr$，所以替换 $index$ 和 $index_{right}$ 指向的元素值，移动 $index$ 和 $index_{right}$ 各自指向下一个元素位置。

> 第 5 次访问： $index = 4$，指向 $S3$，$index_{right} = 2$，指向 $B2$
此时正确区域元素为：$[S1, S2]$，所以正确区域尾部的下一个元素就是 $B2$ 

![](https://upload-images.jianshu.io/upload_images/9738807-5408aba0ee49fe4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

$S3$ 元素值小于 $Tr$，所以替换 $index$ 和 $index_{right}$ 指向的元素值，移动 $index$ 和 $index_{right}$ 各自指向下一个元素位置。

> 第 6 次访问： $index = 5$，指向 $Tr$，$index_{right} = 3$，指向 $B1$
此时正确区域元素为：$[S1, S2, S3]$，所以正确区域尾部的下一个元素就是 $B1$ 

![](https://upload-images.jianshu.io/upload_images/9738807-15b9ee3fc4372a6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为访问到了集合尾部的选定元素，此时替换 $index$ 和 $index_{right}$ 指向的元素值，完成拆分过程。

![](https://upload-images.jianshu.io/upload_images/9738807-776db062b3d27b03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时可以发现，选定元素的左右两侧皆为正确区域，即左侧元素值都不大于 $Tr$ 值，右侧元素值都不小于 $Tr$ 值。所以下一轮进行拆分的则为 $[S1, S2, S3]$ 构成的集合和 $[B2, B1]$ 构成的集合。

拆分过程存在一种现象，例如当前情况下是取集合的最后一个元素为选定元素进行拆分，若初始序列为有序状态，则每一次拆分后的两个集合，一个集合元素个数为 $N-1$，另一个集合为空，递归进行拆解时情况同样如此，也就是走势宛如斜树一般。

### 算法示例

```
def sort(arr, left, right):
    if left < right:
        divide = quickSort(arr, left, right)   # the divide operation
        sort(arr, left, divide - 1)   # recursive sorting
        sort(arr, divide + 1, right)

def quickSort(arr, left, right):
    lessIndex, partitionValue = left, arr[right]
    for i in range(left, right):  #Traversing
        if arr[i] < partitionValue:
            arr[i], arr[lessIndex] = arr[lessIndex], arr[i]
            lessIndex = lessIndex + 1
    arr[lessIndex], arr[right] = partitionValue, arr[lessIndex]
    return lessIndex
```
***quickSort*** 操作中的循环用于遍历集合中元素，每一次遍历结束，可以形成两个正确区域，即不大于选定元素值的元素区域，和不小于选定元素值得元素区域。因为是直接根据位置进行替换，所以相比较于两两相邻元素比较替换效率要高许多，当然也导致了算法的不稳定性。

### 算法分析

快速排序是一种非稳定排序算法，形式上类似于归并排序，操作上刚好相反，一个是对集合先拆分后操作，一个是对集合先操作后拆分。对于 $N$ 个元素的初始集合，因为在每个子集合的拆分过程中，都需要对集合进行遍历比较，所以若对 $K$ 个元素的集合进行拆分，则比较次数级别为 $O(K)$，平均交换次数为 $\frac K2$，即交换次数级别为 $O(K)$。因为拆分集合的过程存在普通二叉树和斜树的情况，所以树高为  $log_2N$~$N$，每一层的平均比较和交换复杂度为 $O(N)$，所以累加可得快速排序的比较和交换复杂度为 $Nlog_2N$ ~ $N^2$。排序过程属于原地排序，不需要额外的存储空间，所以空间复杂度为 $O(log_2N)$~$O(N)$。




