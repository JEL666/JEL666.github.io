---
title: Quick Sort
date: 2026-07-26
categories: [Algorithm, Sort]
tags: [algorithm, sort]
---

## Quick Sort

> pivot을 기준으로 `Divide and conquer`를 진행하여 정렬하는 알고리즘
> 
> 일반적인 상황에서는 빠른 성능을 보이는 정렬 알고리즘 중 하나

### 알고리즘
1. `pivot`을 선택
2. `pivot`을 기준으로 2개의 부분 배열로 분할
3. `pivot`으로 나뉘어진 부분 배열의 원소의 개수가 하나일 때까지 1번으로 이동

### 수도코드 (Lomuto Partition)
```text
# 오름차순으로 가정

# 인덱스 참고
# [low ~ i]: 작은 부분 배열
# [i+1 ~ j-1]: 큰 부분 배열
# [j ~ high-2]: 탐색 전의 원소
# [high-1]: pivot

function partition(low, high)
    pivot = A[high-1]   # 마지막 원소를 pivot으로 설정
    i = low - 1

    for j = low ~ high-2  
        if A[j] < pivot    # pivot보다 작은 원소를 찾는다면 # equal의 유무에 따라 pivot과 동일한 크기의 원소의 위치가 바뀜
            i += 1          # 작은 부분 배열의 크기를 +1
            swap(A[i], A[j])    # 작은 원소와 큰 원소의 위치를 교환

    swap(A[i+1], A[high-1])   # pivot의 위치 변경

    return i+1    # pivot의 index 반환

function quickSort(low, high)
    if (high - low <= 1)
        return

    pivot = partition(low, high)
    quickSort(low, pivot)
    quickSort(pivot+1, high)
```

### 수도코드 (Hoare Partition)
```
function partition(low, high)
    pivot = A[(low + high) / 2]
    i = low - 1
    j = high + 1

    while true
        do ++i while A[i] < pivot
        do --j while A[j] > pivot

        if i >= j
            return j

        swap(A[i], A[j])

function quickSort(low, high)
    if (high - low <= 1)
        return

    pivot = partition(low, high)
    quickSort(low, pivot)
    quickSort(pivot+1, high)
```

### 복잡도
**시간복잡도**
- 일반적인 경우: `O(N*logN)`
  - 배열을 분할하는 단계가 `O(logN)`
  - 각 단계별로 전체 원소를 비교하며 분할하므로 `O(N)`
  - 시간복잡도는 `O(N*logN)` = `O(logN)` * `O(N)`

- 최악의 경우: `O(N^2)`
  - `pivot`이 매번 배열의 끝으로 설정되는 경우 `O(N)` 번의 분할 단계를 거침
  - 각 단계별로 전체 원소를 비교하며 분할하므로 `O(N)`
  - 시간복잡도는 `O(N^2)` = `O(N) * O(N)`

**공간복잡도**
- 일반적인 경우: `O(logN)`
  - 배열을 균등하게 분할한다면 함수 호출 스택이 `O(logN)`

- 최악의 경우: `O(N)`
  - 만약 `pivot`이 매번 끝 부분인 경우 함수 호출 스택이 `O(N)`

### 장단점
**장점**
- in-place, 지역성의 이유로 일반적인 경우에 `Merge Sort`보다 더 빠른 수행속도

(`Merge Sort`를 배열로 구현 시 복사과정이 필요하거나 `LinkedList`로 구현 시 지역성이 떨어져 캐시 히트율이 낮을 수 있음)

**단점**
- 최악의 경우, 시간복잡도가 `O(N^2)`까지 늘어날 수 있음

(그래서 [`Introsort`](https://en.wikipedia.org/wiki/Introsort)에서는 재귀함수 호출 스택의 깊이가 특정 수준을 초과하면 HeapSort로 전환하여 대비함)
