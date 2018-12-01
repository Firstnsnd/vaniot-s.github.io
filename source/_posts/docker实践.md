---
title: docker实践
date: 2018-10-28 20:38:55
tags: [git,docker]
categories: tool
---
## 利用dockerfile安装monoDB
### dockerfile的基础结构
public static void shellSort(int[] arr) {
    int gap = Math.round(arr.length / 2);
    while (gap > 0) {
        for (int i = 0;i<gap;i++){
            for (int j = i + gap;j<arr.length;j+=gap){
                if (arr[j] > arr[j-gap]){
                    int temp = arr[j];
                    int k = j - gap;
                    while (k >= 0 && arr[k] > temp)
                    {
                        arr[k + gap] = arr[k];
                        k -= gap;
                    }
                    arr[k + gap] = temp;
                }
            }
        }
    }
}

