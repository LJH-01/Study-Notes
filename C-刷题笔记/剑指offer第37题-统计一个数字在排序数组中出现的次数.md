# 剑指offer第37题-统计一个数字在排序数组中出现的次数

## 题目描述

统计一个数字在排序数组中出现的次数。

---

此题比较无意义，补二分查找的两种写法：

      //递归写法：

public static int recursionBinarySearch(int[] arr, int key, int  low, int high) {

            if (key < arr[low] || key > arr[high] || low > high) {

                  return -1;

            }

            int middle = (low + high) / 2; // 初始中间位置

            if (arr[middle] > key) {

                  // 比关键字大则关键字在左区域

                  return recursionBinarySearch(arr, key, low, middle -  1);

            } else if (arr[middle] < key) {

                  // 比关键字小则关键字在右区域

                  return recursionBinarySearch(arr, key, middle + 1,  high);

            } else {

                  return middle;

            }

      }

//非递归写法

    public static int commonBinarySearch(int[] arr, int key) {

            int low = 0;

            int high = arr.length - 1;

            int middle = 0; // 定义middle

            if (low > high || key < arr[low] || key > arr[high] ) {

//这里一定要将 low>high 判断放到最前面，先判断另两个，对空数组可能数组下标越界

                  return -1;

            }

            while (low <= high) {

                  middle = (low + high) / 2;

                  if (arr[middle] > key) {

                        // 比关键字大则关键字在左区域

                        high = middle - 1;

                  } else if (arr[middle] < key) {

                        // 比关键字小则关键字在右区域

                        low = middle + 1;

                  } else {

                        return middle;

                  }

            }

            return -1; // 最后仍然没有找到，则返回-1

      }
