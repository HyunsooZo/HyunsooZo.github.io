---
title: DFS
category: Algorithems
order: 9
---

### 알고리즘
##### DFS란?
![DFS](https://drive.google.com/uc?id=1ctZWV-TtoEhDOJfV8UYhDYwcbx3g2qg7)

DFS는 '깊이 우선 탐색'을 의미한다. <br>
이는 그래프나 트리와 같은 자료 구조에서 사용되는 탐색 알고리즘 중 하나이며 <br> 
DFS는 문자그대로 가능한 한 깊숙한 깊이까지 들어가면서<br> 더 이상 탐색할 곳이 없을 때까지 탐색을 진행한다.<br>
<br>
탐색을 시작한 노드에서 한 방향으로 최대한 깊게 들어가다가 더 이상 진행할 수 없게 되면,<br> 되돌아가서 다른 경로로 탐색을 진행하는데 <br>이러한 과정을 재귀적으로 구현하거나 스택을 사용하여 반복적으로 구현할 수 있다<br>

### 프로그래머스 문제 (피로도)
> 문제 설명

XX게임에는 피로도 시스템(0 이상의 정수로 표현합니다)이 있으며, 일정 피로도를 사용해서 던전을 탐험할 수 있습니다. 이때, 각 던전마다 탐험을 시작하기 위해 필요한 "최소 필요 피로도"와 던전 탐험을 마쳤을 때 소모되는 "소모 피로도"가 있습니다. "최소 필요 피로도"는 해당 던전을 탐험하기 위해 가지고 있어야 하는 최소한의 피로도를 나타내며, "소모 피로도"는 던전을 탐험한 후 소모되는 피로도를 나타냅니다. 예를 들어 "최소 필요 피로도"가 80, "소모 피로도"가 20인 던전을 탐험하기 위해서는 유저의 현재 남은 피로도는 80 이상 이어야 하며, 던전을 탐험한 후에는 피로도 20이 소모됩니다.

이 게임에는 하루에 한 번씩 탐험할 수 있는 던전이 여러개 있는데, 한 유저가 오늘 이 던전들을 최대한 많이 탐험하려 합니다. 유저의 현재 피로도 k와 각 던전별 "최소 필요 피로도", "소모 피로도"가 담긴 2차원 배열 dungeons 가 매개변수로 주어질 때, 유저가 탐험할수 있는 최대 던전 수를 return 하도록 solution 함수를 완성해주세요.

>제한사항

k는 1 이상 5,000 이하인 자연수입니다.
dungeons의 세로(행) 길이(즉, 던전의 개수)는 1 이상 8 이하입니다.
dungeons의 가로(열) 길이는 2 입니다.
dungeons의 각 행은 각 던전의 ["최소 필요 피로도", "소모 피로도"] 입니다.
"최소 필요 피로도"는 항상 "소모 피로도"보다 크거나 같습니다.
"최소 필요 피로도"와 "소모 피로도"는 1 이상 1,000 이하인 자연수입니다.
서로 다른 던전의 ["최소 필요 피로도", "소모 피로도"]가 서로 같을 수 있습니다.

##### 문제분석 
이 문제는 유저가 현재 가진 피로도를 이용하여 던전을 탐험하여 최대한 많은 던전을 탐험하는 경우의 수를 찾는 문제다.<br>
먼저 DP로 도전을 해보았는데 절대 안풀렸다. <br>
DFS는 이럴때 DP와 유사한 기능을 하며 탐색을 효과적으로 할 수 있는 방법임<br> 
이 문제에서 유저는 현재 가진 피로도를 이용하여 각 던전을 탐험해야 하는데<br> 
주어진 던전의 개수가 작고,<br> 
각 던전을 방문할 때마다 유저의 현재 피로도가 변경되며 던전을 탐험하는 경우,<br> DFS를 사용하여 유저가 탐험할 수 있는 최대 던전 수를 찾을 수 있다.<br> 
각 던전을 방문할 때마다 유저의 현재 피로도를 고려하고,<br> 탐험 가능한 던전으로 이동하여 최대한 많은 던전을 탐험하는 방식으로 DFS를 활용할 수 있다<br>

DFS는 이러한 상황에서 현재 상태를 유지하면서 가능한 모든 경로를 탐색하므로,<br> 주어진 조건에서 최대한 많은 던전을 탐험하는 경우를 찾을 수 있다.<br>
 따라서, 주어진 문제를 DFS를 활용하여 푸는 것이 적합한 방법이라고 생각했다.<br>

 이후 DP로 풀수있을것 같아서 질문하기를 보니 나와같은생각을 한사람이 있었고, "최소 필요 피로도"와 "소모 피로도"의 차이를 기준으로 던전 배열을 정렬하면 DP로 풀이가 가능하다는 힌트를 얻었다. 이를통해 DP로도 다시 풀어보았고 성공했음. 

![DFS](https://drive.google.com/uc?id=12ets1F_NRgE7sfm52dbjGn5M7lDtt3Si)

##### 문제풀이

```java
class Solution {
    // 던전의 개수
    static int len;
    // 던전 정보를 담을 배열
    static int[][] arr;
    // 던전 방문 여부를 나타내는 배열
    static boolean[] visited;
    // 최종적으로 반환될 정답
    static int answer;

    // 주어진 던전 탐험 문제를 해결하는 메서드
    public int solution(int k, int[][] dungeons) {
        // 전역 배열에 입력된 던전 정보 할당
        arr = dungeons;
        // 던전 개수 설정
        len = dungeons.length;
        // 던전 방문 여부를 표시할 배열 초기화
        visited = new boolean[len];
        
        // DFS 호출하여 던전 탐험 시작
        recursive(0, k);
        
        // 최종 정답 반환
        return answer;
    }

    // DFS를 통해 던전을 탐험하는 메서드
    private void recursive(int idx, int k) {
        // 모든 던전을 순회
        for (int i = 0; i < len; i++) {
            // 이미 방문한 던전이거나, 현재 던전을 갈 수 없는 경우 건너뜀
            if (visited[i] || arr[i][0] > k) {
                continue;
            }
            
            // 현재 던전을 방문했다고 표시
            visited[i] = true;
            // 다음 던전으로 이동하며 체력 조정
            recursive(idx + 1, k - arr[i][1]);
            // 현재 던전 방문을 취소하여 다른 경로에서의 탐색을 위해 초기화
            visited[i] = false;
        }
        // 가장 많이 방문한 던전 수 업데이트
        answer = Math.max(idx, answer);
    }
}

```

```java
import java.util.*;

public class Solution {
    public int solution(int k, int[][] dungeons) {
        // 던전의 개수
        int len = dungeons.length;
        
        // 던전을 "최소 필요 피로도"와 "소모 피로도"의 차이를 기준으로 정렬한다.
        Arrays.sort(dungeons, new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                // "최소 필요 피로도"와 "소모 피로도"의 차이를 비교하여 정렬한다.
                return (a[0] - a[1]) - (b[0] - b[1]);
            }
        });

        // DP 테이블 초기화
        // dp[i][j]: i번째 던전까지 고려했을 때, j 피로도로 얻을 수 있는 최대 던전 개수
        int[][] dp = new int[len + 1][k + 1];

        // 각 던전을 순차적으로 탐색하며 DP 테이블을 채운다.
        for (int i = 1; i <= len; i++) {
            for (int r = 1; r <= k; r++) {
                // 현재 던전의 필요 피로도가 현재 피로도를 초과하는 경우,
                // 현재 던전을 탐험할 수 없으므로 직전까지의 값으로 업데이트한다.
                if (dungeons[i - 1][0] > r) {
                    dp[i][r] = dp[i - 1][r];
                } else {
                    // 현재 던전을 탐험할 수 있는 경우,
                    // 현재 던전을 탐험했을 때와 탐험하지 않았을 때 중 더 큰 값을 선택하여 DP 테이블을 갱신한다.
                    dp[i][r] = Math.max(
                        dp[i - 1][r], 1 + dp[i - 1][r - dungeons[i - 1][1]]
                    );
                }
            }
        }
        
        // 마지막 던전까지 고려한 후, 주어진 피로도로 얻을 수 있는 최대 던전 개수를 반환한다.
        return dp[len][k];
    }
}

```

![결과](https://drive.google.com/uc?id=12ieuhiN1x20wkjhBZxKum0LqbXkE4_PZ)


### 프로그래머스 문제 (이모티콘 할인행사)

카카오톡에서는 이모티콘을 무제한으로 사용할 수 있는 이모티콘 플러스 서비스 가입자 수를 늘리려고 합니다.<br>
이를 위해 카카오톡에서는 이모티콘 할인 행사를 하는데, 목표는 다음과 같습니다.<br>

이모티콘 플러스 서비스 가입자를 최대한 늘리는 것.<br>
이모티콘 판매액을 최대한 늘리는 것.<br>
1번 목표가 우선이며, 2번 목표가 그 다음입니다.<br>

이모티콘 할인 행사는 다음과 같은 방식으로 진행됩니다.<br>

n명의 카카오톡 사용자들에게 이모티콘 m개를 할인하여 판매합니다.<br>
이모티콘마다 할인율은 다를 수 있으며, 할인율은 10%, 20%, 30%, 40% 중 하나로 설정됩니다.<br>
카카오톡 사용자들은 다음과 같은 기준을 따라 이모티콘을 사거나, 이모티콘 플러스 서비스에 가입합니다.<br>

각 사용자들은 자신의 기준에 따라 일정 비율 이상 할인하는 이모티콘을 모두 구매합니다.<br>
각 사용자들은 자신의 기준에 따라 이모티콘 구매 비용의 합이 일정 가격 이상이 된다면, <br>이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입합니다.<br>
다음은 2명의 카카오톡 사용자와 2개의 이모티콘이 있을때의 예시입니다.<br>

사용자	비율	가격<br>
1	40	10,000<br>
2	25	10,000<br>
이모티콘	가격<br>
1	7,000<br>
2	9,000<br>
1번 사용자는 40%이상 할인하는 이모티콘을 모두 구매하고,<br> 이모티콘 구매 비용이 10,000원 이상이 되면 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입합니다.<br>
2번 사용자는 25%이상 할인하는 이모티콘을 모두 구매하고,<br> 이모티콘 구매 비용이 10,000원 이상이 되면 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입합니다.<br>

1번 이모티콘의 가격은 7,000원, 2번 이모티콘의 가격은 9,000원입니다.<br>

만약, 2개의 이모티콘을 모두 40%씩 할인한다면, 1번 사용자와 2번 사용자 모두 1,2번 이모티콘을 구매하게 되고, 결과는 다음과 같습니다.<br>

사용자	구매한 이모티콘	이모티콘 구매 비용	이모티콘 플러스 서비스 가입 여부<br>
1	1, 2	9,600	X<br>
2	1, 2	9,600	X<br>
이모티콘 플러스 서비스 가입자는 0명이 늘어나고 이모티콘 판매액은 19,200원이 늘어납니다.<br>

하지만, 1번 이모티콘을 30% 할인하고 2번 이모티콘을 40% 할인한다면 결과는 다음과 같습니다.<br>

사용자	구매한 이모티콘	이모티콘 구매 비용	이모티콘 플러스 서비스 가입 여부<br>
1	2	5,400	X<br>
2	1, 2	10,300	O<br>
2번 사용자는 이모티콘 구매 비용을 10,000원 이상 사용하여 이모티콘 구매를 모두 취소하고 이모티콘 플러스 서비스에 가입하게 됩니다.<br>
따라서, 이모티콘 플러스 서비스 가입자는 1명이 늘어나고 이모티콘 판매액은 5,400원이 늘어나게 됩니다.<br>

카카오톡 사용자 n명의 구매 기준을 담은 2차원 정수 배열 users, 이모티콘 m개의 정가를 담은 1차원 정수 배열 emoticons가 주어집니다. <br>이때, 행사 목적을 최대한으로 달성했을 때의 이모티콘 플러스 서비스 가입 수와 이모티콘 매출액을 1차원 정수 배열에 담아 return 하도록 solution 함수를 완성해주세요.
<br>
제한사항<br>
1 ≤ users의 길이 = n ≤ 100<br>
users의 원소는 [비율, 가격]의 형태입니다.<br>
users[i]는 i+1번 고객의 구매 기준을 의미합니다.<br>
비율% 이상의 할인이 있는 이모티콘을 모두 구매한다는 의미입니다.<br>
1 ≤ 비율 ≤ 40<br>
가격이상의 돈을 이모티콘 구매에 사용한다면, 이모티콘 구매를 모두 취소하고 <br>이모티콘 플러스 서비스에 가입한다는 의미입니다.<br>
100 ≤ 가격 ≤ 1,000,000<br>
가격은 100의 배수입니다.<br>
1 ≤ emoticons의 길이 = m ≤ 7<br>
emoticons[i]는 i+1번 이모티콘의 정가를 의미합니다.<br>
100 ≤ emoticons의 원소 ≤ 1,000,000<br>
emoticons의 원소는 100의 배수입니다.<br><br>
입출력 예<br>
users	emoticons	result<br>
[[40, 10000], [25, 10000]]	[7000, 9000]	[1, 5400]<br>
[[40, 2900], [23, 10000], [11, 5200], [5, 5900], [40, 3100], <br>[27, 9200], [32, 6900]]	[1300, 1500, 1600, 4900]	<br>[4, 13860]<br>

##### 문제분석 
시간복잡도는 O(n * m * 4**n)이므로 DFS를 이용한 완전 탐색으로 충분히 풀 수 있다!. 

##### 코드

```java
class Solution {
    // 가능한 할인율 배열
    static int[] discountRates = {10, 20, 30, 40};
    // 최대 가입자 수
    static int maxOfSubscribe;
    // 최대 판매 금액
    static int maxOfCost;


    public int[] solution(int[][] users, int[] emoticons) {
        // 이모티콘 수
        int emoticonLength = emoticons.length;
        // 할인율 배열 초기화
        int[] discounts = new int[emoticonLength];

        // 할인율 배열 초기값 설정
        for (int i = 0; i < emoticonLength; i++) {
            discounts[i] = discountRates[0];
        }

        // 계산 시작
        calculate(0, emoticonLength, discounts, users, emoticons);

        // 결과 반환
        return new int[]{maxOfSubscribe, maxOfCost};
    }

    // 가능한 할인율 조합 계산 함수
    public void calculate(int index, int emoticonLength, int[] discounts, int[][] users, int[] emoticons) {
        // 할인율 배열이 모두 설정되면 (탈출조건)
        if (index == emoticonLength) {
            // 가입자 수와 판매 금액 계산
            int subscribe = 0;
            int cost = 0;

            // 모든 사용자에 대해 처리
            for (int[] user : users) {
                int userDiscountRate = user[0];
                int userMaxCost = user[1];
                int totalCost = 0;

                // 사용자별로 구매 가능한 이모티콘 금액 계산
                for (int i = 0; i < emoticonLength; i++) {
                    if (discounts[i] >= userDiscountRate) {
                        totalCost += emoticons[i] / 100 * (100 - discounts[i]);
                    }
                }

                // 구매 조건 확인 후 가입자 수 및 판매 금액 갱신
                if (totalCost >= userMaxCost) {
                    subscribe++;
                } else {
                    cost += totalCost;
                }
            }

            // 최적의 결과 확인 및 업데이트
            if (subscribe > maxOfSubscribe) {
                maxOfSubscribe = subscribe;
                maxOfCost = cost;
            } else if (subscribe == maxOfSubscribe) {
                maxOfCost = Math.max(maxOfCost, cost);
            }
            return;
        }

        // 가능한 모든 할인율에 대해 조합 탐색
        for (int rate : discountRates) {
            discounts[index] = rate;
            //재귀
            calculate(index + 1, emoticonLength, discounts, users, emoticons);
        }
    }
}
```

