---
created: 2025-02-10 17:10
분류: 
tags:
  - algorithm-desc
last_modified: 2025-02-10T17:59:00
---
## ✏️ 알고리즘 설명
> Next Permutation은 **사전순으로 다음에 오는 순열을 찾는** 방법이다.
---
### 🍪 알고리즘을 사용할 수 있는 예
- 사전순으로 정렬된 모든 순열 찾기
	- ex: 단어 순열 문제, 암호 생성 문제 등
- 가장 가까운 큰 수 찾기
	- `218765` -> `251678` (가장 작은 증가 순열로 변경)
	- ex: 숫자 재배치, 가장 가까운 다음 숫자 찾기 문제 등
- 조합 생성 및 탐색 최적화
	- 특정 조건을 만족하는 조합을 찾을 때 brute force 보다 효율적
	- ex: 조합 기반문제, 경로 탐색 문제 등
---
### 🍪 알고리즘 개념
#### 알고리즘 과정
1. **뒤에서부터 감소하는 첫 번째 숫자 찾기**:
	- 배열의 뒤쪽에서 탐색 -> 앞의 숫자가 뒤의 숫자보다 작은 첫 위치 `i`를 찾는다.
2. **해당 숫자보다 큰 숫자 찾기**:
	- 배열의 뒤쪽에서 탐색 -> `nums[i]`보다 큰 숫자 중 가장 오른쪽 숫자의 위치 `j`를 찾는다.
3. **두 숫자 교환**: 
	- `swap(i, j)`
4. **i번 이후의 숫자 뒤집기**
	- `i+1`부터 끝까지의 숫자를 뒤집어 사전순으로 가장 작은 상태로 만든다.

#### 예시: `1, 2, 3, 4`의 모든 순열을 사전순 정렬하면
> 위 알고리즘 과정을 생각하면서 아래 예시를 보자. 
```
1 2 3 4
1 2 4 3
1 3 2 4
1 3 4 2
1 4 2 3
2 1 3 4

...

4 3 2 1 (마지막 순열)
```

#### 코드 (Java)
```java
package Algorithm_Implementation.Algorithms;

import java.util.Arrays;

public class NextPermutation {
    public static void main(String[] args){
        // 1234 -> 1243
        int[] nums1 = {1, 2, 3, 4};
        nextPermutation(nums1);
        System.out.println(Arrays.toString(nums1));

        // 1342 -> 1423
        int[] nums2 = {1, 3, 4, 2};
        nextPermutation(nums2);
        System.out.println(Arrays.toString(nums2));

        // 4321 -> 1234
        int[] nums3 = {4, 3, 2, 1};
        nextPermutation(nums3);
        System.out.println(Arrays.toString(nums3));
    }
    private static void nextPermutation(int[] nums){
        // 뒤에서부터 탐색
        int i = nums.length - 2;

        // 📌 1단계: i번 숫자가 i+1번 숫자보다 큰 동안 반복 -> 작은 순간 종료
        while(i >= 0 && nums[i] >= nums[i+1]){
            i--;
        }

        if(i >= 0){
            // 뒤에서부터 탐색
            int j = nums.length - 1;
            
            // 📌 2단계: i번 숫자보다 처음으로 큰 첫 숫자를 찾으면 종료
            while(nums[j] <= nums[i]){
                j--;
            }
            // 📌 3단계: i와 j번 숫자를 swap
            swap(nums, i, j);
        }

        // 📌 4단계: i번 이후의(i+1부터) 배열을 뒤집어 오름차순 정렬
        reverse(nums, i+1);
    }


    private static void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

	// 내림차순 -> 오름차순 (가장 효율적인 방법은 단순히 뒤집는 것.)
	// Arrays.sort()는 O(NlogN)이지만, 뒤집는 것은 O(N)이다.
    private static void reverse(int[] nums, int start) {
        int end = nums.length-1;
        while(start < end){
            swap(nums, start, end);
            start++;
            end--;
        }
    }

}	
```

> 이 알고리즘은 암기가 필요할 것 같다.
---
## ⌛ 시공간 복잡도
### 🟡 시간 복잡도: O(N)
- i 찾기 (O(N))
- j 찾기 (O(N))
- 교환 (O(1))
- 뒤집기 (O(N))
> 뒤집는 방법을 `Arrays.sort()`를 사용했다면, O($NlogN$)일 것이다
### 🟢 공간 복잡도: O(1)
- 입력 배열을 그대로 수정하므로 추가 사용공간 없음

---
## 🔗 관련 알고리즘 문제 
- [10972번 (다음 순열) ★](https://github.com/M1nKyu/Coding-Challenges/blob/main/Baekjoon/%EC%88%98%ED%95%99/10972%EB%B2%88%20(%EB%8B%A4%EC%9D%8C%20%EC%88%9C%EC%97%B4)%20%E2%98%85.md)

---
> **참고 사이트**
> - []()
