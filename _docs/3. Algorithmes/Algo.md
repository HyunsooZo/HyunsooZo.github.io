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




### Greedy

<div class="content-box">
- 매 순간 현재 기준 최선의 답을 선택 해 나가는 기법<br>
- 다만 항상 최적해 인 것은 아님.  
</div>

```java
public static int coinChangeGreedy(int[] coins, int amount) {
        Arrays.sort(coins); // 동전들을 오름차순으로 정렬
        int numCoins = 0;
        int index = coins.length - 1;

        while (amount > 0 && index >= 0) {
            if (coins[index] <= amount) {
                int count = amount / coins[index]; // 현재 동전으로 거슬러 줄 수 있는 최대 개수 계산.
                numCoins += count; // 동전 개수 더하기
                amount -= count * coins[index]; // 거슬러 준 금액 차감.
            }
            index--; // 다음 동전으로 이동.
        }

        if (amount == 0) {
            return numCoins; // 거스름돈을 완전히 거슬러 줄 수 있는 경우 동전의 개수 반환
        } else {
            return -1; // 거스름돈을 거슬러 줄 수 없는 경우 -1 반환
        }
    }
```


### Two Pointers

<div class="content-box">
- 배열에서 2개의 포인터를 사용해 원하는 결과 얻음<br>
- 두개의 포인터 배치 <br>
- 다중 for문의 복잡도를 좀더 선형적으로 풀 수 있음. 
</div>

```java
public  boolean isPairSumPresent(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;

        while (left < right) {
            int sum = arr[left] + arr[right];

            if (sum == target) {
                return true; // 합이 타겟과 일치하는 쌍을 찾은 경우 true 반환.
            } else if (sum < target) {
                left++; // 합이 타겟보다 작은 경우 왼쪽 포인터를 오른쪽으로 이동.
            } else {
                right--; // 합이 타겟보다 큰 경우 오른쪽 포인터를 왼쪽으로 이동.
            }
        }

        return false; // 쌍을 찾지 못한 경우 false를 반환.
    }

```
![](//placehold.it/800x600)


### Divide & Conquer

<div class="content-box">
- 큰 문제를 작은부분으로 나누어 해결하는 알고리즘<br>
분할 정복과 이진 탐색은 유사한 개념이지만, 약간의 차이 존재.<br> 
이진 탐색은 정렬된 배열에서 특정 값을 찾는 데 사용되는 반면,<br> 
분할 정복은 주어진 문제를 작은 부분으로 나누어 해결하는 알고리즘.<br><br>
1. 문제를 하나이상의 작은 부분들로 분할 <br>
2. 부분들을 각각 정복 <br>
3. 부분들의 해답을 통합하여 전체의 답을 구함. 
</div>

|Pros|Cons|
|-|-|
|문제를 나누어 처리하여 어려운 문제 해결 가능|메모리사용이 많음(재귀구조)|
|병렬처리에 이점 존재||

```java
public int getMax(int[] arr, int left, int right){
    int mid = (left+right)/2;
    if(left==right){
        return arr[left];
    }
    left = getMax(arr,left,mid); // 재귀 호출
    right = getMax(arr,m+1,right); // 재귀 호출

    return left>right?left:right;
}
```


### Back Tracking

![](//placehold.it/800x600)


### Shortest Path

![](//placehold.it/800x600)


### Dinamic Programing

<div class="content-box">
- 큰 문제를 부분문제로 나누어 답을 찾아가는 과정에서<br>
- 계산된 결과를 <b>기록</b>하고 <b>재활용</b>하여 문제의 답을 구하는 알고리즘<br>
- 중간계산 결과를 기록하는 메모리가 필요<br> 
- 한번 계산한 부분을 다시 계산하지 않으므로 속도 빠름<br> 
</div>

|분할정복과의 차이|Greedy와의 차이|
|-|-|
|분할정복은 부분문제 중복 없음|그리디는 순간의 최선을 구하는 방식(근사치)|
|DP는 부분문제가 중복되어 재활용에 사용| DP는 모든방법을 확인 후 최적해를 구함.|

##### Tabulation/Memoization

|Tabulation(bottom up)|Memoization(top down)|
|-|-|
|상향식 접근방법|하향식 접근방법|
|작은하위문제부터 풀며 올라감|큰문제에서 하위문제를 확인해가며 진행|
|모두 계산하여 차례대로 진행|계산이 필요한 순간 계산하며 진행|

```java

// 배낭문제예시

// 배낭에 물품을 담으려함. 
// 배낭에는 k무게만큼의 물품을 담을 수 있음. 
// n개의 물품이 주어지고 이물품의 무게와 가치정보가 items 2차원 배열에 주어짐
// 최대가치가 되도록 물품을 담았을 때의 가치를 출력. 

// 예
// items{{6,13},{4,8},{3,6},{5,12}}
// n = 4 , k = 7
// 답 : 14

public int solution(int[][] items, int n , int k){
    int[][] dp = new int[n+1][k+1];
    for(int i = 0 ; i < n ; i++){
        for(int j =1 ; j <= k ; j++>){
            if(items[i][0]>j){
                dp[i+1][j] = dp[i][j];
            }else{
                dp[i+1][j] = Math.max(dp[i][j],dp[i][j-items[i][0]+items[i][1]);
            }
        }
    }
    return dp[n][k];


}
```