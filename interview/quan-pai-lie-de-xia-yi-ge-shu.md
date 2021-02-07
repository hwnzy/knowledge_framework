# 全排列的下一个数

## 全排列的下一个数

题目 给出一个正整数，找出这个正整数所有数字全排列的下一个数。

说通俗点就是在一个整数所包含数字的全部组合中，找到一个大于且仅大于原数的新整数。让我们举几个例子。

如果输入 1234512345，则返回 1235412354。

如果输入 1235412354，则返回 1243512435。

如果输入 1243512435，则返回 1245312453。

小灰发现的「规律」如下。

输入 1234512345，返回 1235412354，那么

12354 - 12345 = 9 12354−12345=9

刚好相差 99 的一次方。

输入 1235412354，返回 1243512435，那么

12435 - 12354 = 81 12435−12354=81

刚好相差 99 的二次方。

所以，每次计算最近的换位数，只需要加上 99 的 nn 次方即可。

解题思路

举一个例子。

给出 1、2、3、4、51、2、3、4、5 这几个数字。

最大的组合：5432154321。

最小的组合：1234512345。

没错，数字的顺序和逆序，是全排列中的两种极端情况。

那么普遍情况下，一个数和它最近的全排列数存在什么关联呢？

例如给出整数 1235412354，它包含的数字是 1、2、3、4、51、2、3、4、5，如何找到这些数字全排列之后仅大于原数的新整数呢？

为了和原数接近，我们需要尽量保持高位不变，低位在最小的范围内变换顺序。

至于变换顺序的范围大小，则取决于当前整数的逆序区域。

如图所示，1235412354 的逆序区域是最后两位，仅看这两位已经是当前的最大组合。若想最接近原数，又比原数更大，必须从倒数第 33 位开始改变。

怎样改变呢？1234512345 的倒数第 33 位是 33，我们需要从后面的逆序区域中找到大于 33 的最小的数字，让其和 33 的位置进行互换。

互换后的临时结果是 1245312453，倒数第 33 位已经确定，这个时候最后两位仍然是逆序状态。我们需要把最后两位转变为顺序状态，以此保证在倒数第 33 位数值为 44 的情况下，后两位尽可能小。

这样一来，就得到了想要的结果 1243512435。

获得全排列下一个数的 3 个步骤。

从后向前查看逆序区域，找到逆序区域的前一位，也就是数字置换的边界。 让逆序区域的前一位和逆序区域中大于它的最小的数字交换位置。 把原来的逆序区域转为顺序状态 。

```java
public static int[] findNearestNumber(int[] numbers){
    //1.从后向前查看逆序区域，找到逆序区域的前一位，也就是数字置换的边界
    int index = findTransferPoint(numbers);
    //如果数字置换边界是0，说明整个数组已经逆序，无法得到更大的相同数
    //字组成的整数，返回null
    if(index == 0){
        return null;
    }
    //2.把逆序区域的前一位和逆序区域中刚刚大于它的数字交换位置
    //复制并入参，避免直接修改入参
    int[] numbersCopy = Arrays.copyOf(numbers, numbers.length);
    exchangeHead(numbersCopy, index);
    //3.把原来的逆序区域转为顺序
    reverse(numbersCopy, index);
    return numbersCopy;
}

private static int findTransferPoint(int[] numbers){
    for(int i=numbers.length-1; i>0; i--){
        if(numbers[i] > numbers[i-1]){
            return i;
        }
    }
    return 0;
}

private static int[] exchangeHead(int[] numbers, int index){
    int head = numbers[index-1];
    for(int i=numbers.length-1; i>0; i--){
        if(head < numbers[i]){
            numbers[index-1] = numbers[i];
            numbers[i] = head;
            break;
        }
    }
    return numbers;
}

private static int[] reverse(int[] num, int index){
    for(int i=index,j=num.length-1; i<j; i++,j--){
        int temp = num[i];
        num[i] = num[j];
        num[j] = temp;
    }
    return num;
}

public static void main(String[] args) {
    int[] numbers = {1,2,3,4,5};
    //打印12345之后的10个全排列整数
    for(int i=0; i<10;i++){
        numbers = findNearestNumber(numbers);
        outputNumbers(numbers);
    }
}

//输出数组
private static void outputNumbers(int[] numbers){
    for(int i : numbers){
        System.out.print(i);
    }
    System.out.println();
}
```



