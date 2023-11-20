---
title: 프로그래머스 - 시소짝궁(Lv2)
category: Algorithems
order: 9
---

### 이분탐색이란? 
이분탐색이란, 정렬된 배열에서 특정 값을 찾는 탐색 알고리즘이다. 
주어진 배열을 반으로 나누어 탐색 범위를 좁혀가는 방식으로 동작한다. <br>
매 단계마다 탐색 범위를 절반으로 줄여나가므로 배열의 크기에 대해 로그 시간 복잡도를 가진다.<br> 
배열을 정렬하는 데에 `O(LogN)` 시간 복잡도를 가지고 <br>
이분탐색을 사용하여 값을 찾는 과정 또한 로그 시간 복잡도로 이루어지며 중간을 기준으로<br> 
탐색대상을 절반씩 줄여가므로 전체적으로 `O(LogN)`의 시간 복잡도를 가지게 된다.<br>
이는 완전탐색의 O(N)에 비하면 매우 빠른속도이다. <br>
`O(logN)`만에 값을 찾을 수 있는 이유는 중간을 기준으로 탐색 대상을 절반씩 줄여나가기 때문이다.
![예시](https://drive.google.com/uc?id=1mc41CfineV12XGVqKxgilOm5ZNvyfU4I)

##### 알고리즘 기본 예시문제

![예시](https://drive.google.com/uc?id=1ndxnKUf8fvWC-8Hc096kBdt62dte8zIL)

### 프로그래머스 문제
[프로그래머스 Lv2. 시소짝꿍](https://school.programmers.co.kr/learn/courses/30/lessons/152996)<br>

어느 공원 놀이터에는 시소가 하나 설치되어 있습니다. 이 시소는 중심으로부터 2(m), 3(m), 4(m) 거리의 지점에 좌석이 하나씩 있습니다.<br>
이 시소를 두 명이 마주 보고 탄다고 할 때, 시소가 평형인 상태에서 각각에 의해 시소에 걸리는 토크의 크기가 서로 상쇄되어 완전한 균형을 이룰 수 있다면 그 두 사람을 시소 짝꿍이라고 합니다.<br> 즉, 탑승한 사람의 무게와 시소 축과 좌석 간의 거리의 곱이 양쪽 다 같다면 시소 짝꿍이라고 할 수 있습니다.<br>
사람들의 몸무게 목록 `weights`이 주어질 때, 시소 짝꿍이 몇 쌍 존재하는지 구하여 `return` 하도록 `solution` 함수를 완성해주세요.<br>

![문제 분석](https://drive.google.com/uc?id=1dtath0Oa1nCc3CQgIovM5RaETVNys1p9)

##### 문제분석
문제 제한사항에서 배열의 길이가 10,000이라고 주어졌으니 <br>
완전탐색을 돌리면 10억이 넘아가므로 성능테스트에 걸릴것으로 예상.<br>
이분탐색을 돌리면 될것이다.<br>
일단 배열 정렬 후 만족조건을 생각해보면 <br>
1m짜리 자리가 없기때문에 아래와 같은 4개 조건만 만족하면된다.<br>
**1.** `a == b` 일것 (이는 곧 `a*n==b*n`, 즉`1:1`) <br>
**2.** `a*4 == b*2` 일것 (이는 곧 `a*2 == b`, 즉 `2:1`) <br>
**3.** `a*3 == b*2` 일것 (즉 `3:2`)<br>
**4.** `a*4 == b*3` 일것 (즉 `4:3`)<br>

과 같다. a는 언제나 b 보다 작으므로 반대의 경우는 굳이 고려할 필요가 없다. (예 : `a*1 == b*2` 는 항상 거짓이다.)


##### 문제풀이
위 조건에 따라 이분탐색을 진행하면 아래와 같다. 

```java
import java.util.*;

public class Solution {
    // 메서드간의 파라메터 를 줄이기 위해 스태틱 변수사용
    static long answer = 0; // 결과를 저장할 변수
    static int[] arr; // 입력된 weights 배열
    static int len; // weights 배열의 길이

    public long solution(int[] weights) {
        arr = weights; // weights 배열을 정렬하기 위해 arr에 할당
        len = arr.length; // 배열의 길이 저장
        Arrays.sort(arr); 
        // 오름차순으로 배열 정렬 (이진탐색 위함임)

        // 배열의 각 요소 반복
        for (int i = 0; i < len; i++) {
            findSameValues(i); // 현재 값과 동일한 값 개수 찾기

            // 1:2 비율 탐색하여 발견 시 answer 증가
            if (binarySearch(arr[i] * 2)) {
                answer++;
            }

            // 현재 값이 3의 배수인 경우
            if (arr[i] % 3 == 0){
                // 2:3, 4:3 비율의 값 탐색하여 발견 시 answer 증가
                if(binarySearch(arr[i] * 2 / 3)){
                    answer++;
                }
                if(binarySearch(arr[i] * 4 / 3)){
                    answer++;
                }
            }
        }
        return answer; // 최종 결과 반환
    }
    
    // 현재 값과 동일한 값 개수 찾기
    void findSameValues(int val){
        for (int i = val + 1; i < len; i++) {
            if (arr[i] == arr[val]) {
                answer++; // 동일 값 발견 시 answer 증가
            } else {
                break; // 다른 값이 나오면 반복문 종료
            }
        }
    }

    // 이분 탐색
    boolean binarySearch(int target) {
        int left = 0;
        int right = len - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        if (left >= len || left < 0) {
            return false; // 배열 범위를 벗어나면 false 반환
        }

        // 이분 탐색으로 찾은 값이 target과 일치할 경우
        if (arr[left] == target) {
            for (int i = left + 1; i < arr.length; i++) {
                if (arr[left] == arr[i]) {
                    answer++; // 중복 값 발견 시 answer 증가
                } else {
                    break; // 다른 값이 나오면 반복문 종료
                }
            }
        }

        return arr[left] == target; // 찾은 값 반환
    }
}

```

**1.** `findSameValues()`  같은 수가 있는지 점검 <br>
**2.** `binarySearch` 각 비율에 맞춰 이진탐색을 진행, 만약내부에서 같은 값이 발견 시 `answer++` 이후 true반환시에도 `answer++` 

![문제 분석](https://drive.google.com/uc?id=1x3TQtIIvYVS3-J5h5Xz5vtG0rWQmX42t)
