# 算法

共计13道

### Q1:什么是AVL树?

AVL树是平衡二叉查找树，增加和删除节点后通过树形旋转重新达到平衡。右旋是以某个节点为中心，
将它沉入当前右子节点的位置，而让当前的左子节点作为新树的根节点，也称为顺时针旋转。同理左旋，
是以某个节点为中心，将它沉入当前左子节点的位置，而让当前的右子节点作为新树的根节点，也称为
逆时针旋转。

### Q2:什么是红黑树?

红黑树是1972年发明的，称为对称二叉B树，1978 年正式命名红黑树。主要特征是在每个节点上增
加一个属性表示节点颜色，可以红色或黑色。红黑树和AVL树类似，都是在进行插入和删除时通过旋
转保持自身平衡，从而获得较高的查找性能。与AVL树相比，红黑树不追求所有递归子树的高度差不
超过1,保证从根节点到叶尾的最长路径不超过最短路径的2倍，所以最差时间复杂度是O(logn)。红
黑树通过重新着色和左右旋转，更加高效地完成了插入和删除之后的自平衡调整。
红黑树在本质上还是二叉查找树，它额外引入了5个约束条件:①节点只能是红色或黑色。②根节点
必须是黑色。③所有NIL节点都是黑色的。④一条路径上不能出现相邻的两个红色节点。⑤在任何
递归子树中，根节点到叶子节点的所有路径上包含相同数目的黑色节点。
这五个约束条件保证了红黑树的新增、删除、查找的最坏时间复杂度均为0(logn)。如果-个树的左子
节点或右子节点不存在，则均认定为黑色。红黑树的任何旋转在3次之内均可完成。
更多红黑树的介绍可以看这篇文章:我画了20张图，给女朋友讲清楚红黑树



### Q3: AVL树和红黑树的区别?

红黑树的平衡性不如AVL树，它维持的只是一种大致的平衡，不严格保证左右子树的高度差不超过1。
这导致节点数相同的情况下，红黑树的高度可能更高，也就是说平均查找次数会高于相同情况的AVL
树。
在插入时，红黑树和AVL树都能在至多两次旋转内恢复平衡，在删除时由于红黑树只追求大致平衡，因
此红黑树至多三次旋转可以恢复平衡，而AVL树最多需要0(logn)次。AVL树在插入和删除时，将向上
回溯确定是否需要旋转，这个回溯的时间成本最差为O(logn)，而红黑树每次向上回溯的步长为2,回
溯成本低。因此面对频繁地插入与删除红黑树更加合适。



### Q4: B树和B+树的区别?

B树中每个节点同时存储key和data,而B+树中只有叶子节点才存储data,非叶子节点只存储key。
InnoDB对B+树进行了优化，在每个叶子节点上增加了一个指向相邻叶子节点的链表指针，形成了带
有顺序指针的B+树，提高区间访问的性能。
B+树的优点在于:①由于B+树在非叶子节点上不含数据信息，因此在内存页中能够存放更多的
key,数据存放得更加紧密，具有更好的空间利用率，访问叶子节点上关联的数据也具有更好的缓存命
中率。②B+树的叶子结点都是相连的，因此对整棵树的遍历只需要-次线性遍历叶子节点即可。而B
树则需要进行每一层的递归遍历，相邻的元素可能在内存中不相邻，所以缓存命中性没有B+树好。但
是B树也有优点，由于每个节点都包含key和value,因此经常访问的元素可能离根节点更近，访问也
更迅速。



### Q5:排序有哪些分类?

排序可以分为内部排序和外部排序，在内存中进行的称为内部排序，当数据量很大时无法全部拷贝到内
存需要使用外存，称为外部排序。
内部排序包括比较排序和非比较排序，比较排序包括插入/选择/交换/归并排序,非比较排序包括计数/
基数/桶排序。
插入排序包括直接插入/希尔排序，选择排序包括直接选择/堆排序，交换排序包括冒泡/快速排序。



### Q6:直接插入排序的原理?

稳定，平均/最差时间复杂度O(n2),元素基本有序时最好时间复杂度O(n),空间复杂度0(1)。
每一趟将一一个待排序记录按其关键宇的大小插入到已排好序的一组记录的适当位置上,直到所有待排序
记录全部插入为止。
public void insertionsort(int[] nums) {
for Cint i = 1; i < nums.1ength; i++) {
int insertNum = nums[i];
int insertIndex;
for CinsertIndex = i - 1; insertIndex >= 0 88 nums [insertIndex] >
insertNum; insertIndex--) {
nums[insertIndex + 1] = nums[insertIndex];
}
nums[insertIndex + 1] = insertNum;
直接插入没有利用到要插入的序列已有序的特点，插入第i个元素时可以通过二分查找找到插入位置
insertIndex，再把i-inserlndex之间的所有元素后移一-位， 把第i个元素放在插入位置上。
public void binaryInsertionsort(int门] nums) {
for (int i = 1; i < nums.1ength; i++) {
int insertNum = nums[i];
int insertIndex = -1;
int start = 0;
intend=i-1;
while (start <= end) {
int mid = start + (end - start) / 2;
if CinsertNum > nums[mid])
start = mid + 1;
else if CinsertNum < nums [mid])
end=mid一1;
else {
insertIndex = mid + 1;
break;
}
if (insertIndex == -1)
insertIndex = start;
if (i - insertIndex >= 0)
System. arraycopyCnums. insertIndex. nums. insertIndex + 1. i -
insertIndex) ;
nums[insertIndex] = insertNum;





### Q7:希尔排序的原理?

又称缩小增量排序，是对直接插入排序的改进，不稳定，平均时间复杂度0(n^1 .3^)，最差时间复杂度
O(n2)，最好时间复杂度O(n)，空间复杂度0(1)。
把记录按下标的一-定增量分组，对每组进行直接插入排序，每次排序后减小增量，当增量减至1时排序
完毕。
public void shel1Sort(int[] nums) {
for (int d = nums.1ength / 2;d>0 ; d/=2) {
for (int i = d; i < nums.1ength; i++) {
int insertNum = nums[i] ;
int insertIndex;
for (insertIndex = i一d; insertIndex >= 0 && nums [insertIndex]

> insertNum; insertIndex -= d) {
> nums [insertIndex + d] = nums [insertIndex];
> nums [insertIndex + d] = insertNum;
> }
> }



### Q8:直接选择排序的原理?

不稳定，时间复杂度O(n2),空间复杂度0(1)。
每次在未排序序列中找到最小元素,和未排序序列的第一个元素交换位置， 再在剩余未排序序列中重复
该操作直到所有元素排序完毕。
pub1ic void selectSort(int[] nums) {
int mi nIndex;
for (int index = 0; index < nums. length - 1; index++){
minIndex = index;
for (int i = index + 1;i < nums. length; i++){
if(nums[i] < nums [mi nIndex] )
minIndex = i ;
}
if (index != minIndex){
swap(nums，index， mi nIndex) ;
}
}



### Q9:堆排序的原理?

是对直接选择排序的改进，不稳定，时间复杂度0(nlogn),空间复杂度0(1)。
将待排序记录看作完全叉树， 可以建立大根堆或小根堆，大根堆中每个节点的值都不小于它的子节点
值，小根堆中每个节点的值都不大于它的子节点值。
以大根堆为例，在建堆时首先将最后一个节点作为当前节点，如果当前节点存在父节点且值大于父节
点，就将当前节点和父节点交换。在移除时首先暂存根节点的值，然后用最后-个节点代替根节点并作
为当前节点，如果当前节点存在子节点且值小于子节点，就将其与值较大的子节点进行交换，调整完堆
后返回暂存的值。
public void add(int[] nums, int i，int num){
nums[i] = num;
int curIndex = i;
while (curIndex > 0) {
int parentIndex = (curIndex - 1) / 2;
if (nums[parentIndex] < nums [curIndex])
swap(nums, parentIndex, curIndex) ;
else break;
curIndex = parentIndex;
public int remove(int[] nums， int size){
int result = nums[O]; 
nums[0] = nums[size - 1];
int curIndex = 0;
while (true) {
int 1eftIndex = curIndex* 2 + 1;
int rightIndex = curIndex * 2 + 2;
if (leftIndex >= size) break;
int maxIndex = 1eftIndex;
if (rightIndex < size && nums[maxIndex] < nums [ri ghtIndex])
maxIndex = rightIndex;
if (nums[curIndex] < nums [maxIndex])
swap(nums，curIndex, maxIndex) ;
else break;
curIndex = maxIndex;
}
return result;



### Q10:冒泡排序的原理?

稳定，平均/最坏时间复杂度O(n2),元素基本有序时最好时间复杂度O(),空间复杂度0(1)。
比较相邻的元素，如果第一个比第 二个大就进行交换，对每一对相邻元素做同样的工作，从开始第- -对
到结尾的最后一对，每一轮排序后末尾元素都是有序的，针对n个元素重复以上步骤n-1次排序完
毕。
public void bubblesort(int[] nums) {
for(inti=0;i<nums.1ength一1;i++){
for (int index = 0; index < nums.1ength 一1一i; index++) {
if (nums[index] > nums[index + 1])
swap(nums，index， index + 1)
}
}
当序列已经有序时仍会进行不必要的比较，可以设置一个标志记录是否有元素交换，如果没有直接结束
比较。
public void betterBubblesort(int[] nums) {
boolean swap;
for(inti=0;i<nums.1ength一1;i++){
swap = true;
for (int index = 0; index < nums.1ength 一1一i; index++) {
if (nums[index] > nums[index + 1]) {
swap(nums，index ， index + 1);
swap = false;
if (swap) break; 



### Q11:快速排序的原理?

是对冒泡排序的一种改进，不稳定，平均/最好时间复杂度O(nlogn),元素基本有序时最坏时间复杂度
0(n2)，空间复杂度(logn)。
首先选择一个基准元素，通过-趟排序将要排序的数据分割成独立的两部分，-部分全部小于等于基准
元素，一部分全部大于等于基准元素，再按此方法递归对这两部分数据进行快速排序。
快速排序的- -次划分从两头交替搜索，直到low和high指针重合，-趟时间复杂度0(n),整个算法的
时间复杂度与划分趟数有关。
最好情况是每次划分选择的中间数恰好将当前序列等分，经过log(n)趟划分便可得到长度为1的子表，
这样时间复杂度0(nlogn)。
最坏情况是每次所选中间数是当前序列中的最大或最小元素，这使每次划分所得子表其中-一个为空表,
这样长度为n的数据表需要n趟划分，整个排序时间复杂度0(n2)。
public void qui ckSort(int[] nums， int start, int end) {
if (start < end) {
int pivotIndex = getPivotIndex(nums, start, end);
qui ckSort(nums, start, pivotIndex - 1);
quickSort(nums，pivotIndex + 1，end);
public int getPivotIndex(int[] nums， int start, int end) { 
int pivot = nums[start] ;
int 1ow = start;
int high = end;
while (1ow < high) {
while (low <= high & nums[low] <= pivot)
1ow++; 
while (low <= high & nums[high] > pivot)
high--;
if (1ow < high)
swap(nums，1ow, high);
swap(nums，start, high) ; 
return high;





### Q12:归并排序的原理?

归井排序基于归井操作，是一种稳定的排序算法，任何情况时间复杂度都为O(nlogn),空间复杂度为
O(n).
基本原理:应用分治法将待排序序列分成两部分，然后对两部分分别递归排序,最后进行合并，使用一
个辅助空间并设定两个指针分别指向两个有序序列的起始元素，将指针对应的较小元素添加到辅助空
间，重复该步骤到某- -序列到达末尾，然后将另- -序列剩余元素合井到辅助空间末尾。
适用场景:数据量大且对稳定性有要求的情况。
int[] help; 
public void mergeSort(int[] arr) {
int] help = new int[arr.1ength] ;
sort(arr, 0, arr.1ength - 1);
public void sort(int[] arr, int start, int end) {
if (start == end) return;
int mid = start + (end - start) / 2;
sort(arr，start, mid);
sort(arr, mid + 1，end); 
merge(arr, start, mid, end);
public void merge(int[] arr, int start, int mid, int end) {
if (end + 1 - start >= 0) System . arraycopy(arr, start, help, start, end

+ 1- start);
int p = start;
intq=mid+1;
int index = start;
while(p<=mid&q<=end){
if (he1p[p] < he1p[q])
arr[index++] = he1p[p++];
else
arr[index++] = he1p[q++];
}
while (p <= mid) arr[index++] = he1p[p++];
while (q <= end) arr[index++] = he1p[q++];

### Q13:排序算法怎么选择?

数据量规模较小，考虑直接插入或直接选择。当元素分布有序时直接插入将大大减少比较和移动记录的
次数，如果不要求稳定性，可以使用直接选择，效率略高于直接插入。
数据量规模中等，选择希尔排序。
数据量规模较大，考虑堆排序(元素分布接近正序或逆序)、 快速排序(元素分布随机)和归并排序
(稳定性)。
-般不使用冒泡。