---
title: Universal Algorithems
category: Algorithems
order: 8
---
### Sorting

<div class="content-box">
특정 값을 기준으로 데이터를 순서대로 배치하는 방법
</div>

|정렬 종류|θ(평균)|Big O|보조메모리|안정성|
|-|-|-|-|-|
|Bubble|n²|n²|1|O|
|Insertion|n²|n²|1|O|
|Selection|n²|n²|1|O|
|Merge|nlogn|nlogn|n|O|
|Heap|nlogn|nlogn|1|O|
|Quick|nlogn|n²|logn|O|
|Tree|nlogn|n²|n|O|
|Radix|dn|dn|n+k|O|
|Counting|n+k|n+k|n+k|O|
|Shell| - |n²|1|X|

##### QuickSort

```java
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            // 피벗을 선택 및 분할
            int pivotIndex = partition(arr, low, high);

            // 피벗을 기준으로 분할된 두 개의 부분 배열에 대해 재귀적으로 퀵 정렬 수행.
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    public static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // 피벗 배열 마지막 요소 선택.
        int i = low - 1; // 작은 요소들의 마지막 인덱스 초기화.

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                swap(arr, i, j); // 작은 요소들 왼쪽으로 이동.
            }
        }

        swap(arr, i + 1, high); // 피벗 올바른 위치로 이동.
        return i + 1;
    }

    public static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
```
### BinarySearch

<div class="content-box">
- 정렬된 상태의 데이터에서 특정값을 빠르게 탐색<br>
- 찾고자 하는 값과 데이터 중앙의 값 비교 <br>
- 작으면 중간값 왼쪽, 크면 오른쪽에서 다시 이진탐색
</div>

```java
  public static int binarySearch(int[] arr, int target) {
        int low = 0;
        int high = arr.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target) {
                return mid; // 타겟을 찾은 경우 인덱스 반환.
            } else if (arr[mid] < target) {
                low = mid + 1; // 타겟이 중간 값보다 큰 경우 오른쪽 탐색
            } else {
                high = mid - 1; // 타겟이 중간 값보다 작은 경우 왼쪽 탐색
            }
        }

        return -1; // 타겟을 찾지 못한 경우 -1을 반환.
    }
```

![](//placehold.it/800x600)


### Greedy

![](//placehold.it/800x600)


### Two Pointers

![](//placehold.it/800x600)


### Divide & Conquer

![](//placehold.it/800x600)


### Back Tracking

![](//placehold.it/800x600)


### Shortest Path

![](//placehold.it/800x600)


### Dinamic Programing

![](//placehold.it/800x600)