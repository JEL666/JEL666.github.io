---
title: Merge Sort
date: 2026-07-25
categories: [Algorithm, Sort]
tags: [algorithm, sort]
---

## Merge Sort

> 데이터를 Divide and conquer(분할-정복)하여 항상 `O(n*logn)`의 시간복잡도를 보장하는 정렬

### 알고리즘 (재귀)
1. 배열을 절반씩 잘라 원소가 오직 하나일 때까지 반복
2. 정렬된 두 부분 배열을 병합하며 정렬
3. 모든 배열이 합쳐져 하나의 배열이 될 때까지 반복

### 수도코드 (재귀)
```text
# 오름차순 정렬로 가정
A[]
temp[A.length]    # A와 동일한 크기의 임시배열

function mergeSort(start, end)
    if end - start <= 1
        return

    mid = start + (end - start) / 2
    mergeSort(start, mid)
    mergeSort(mid, end)
    merge(start, mid, end)

function merge(start, mid, end)
    left = start
    right = mid
    idx = start
    
    while left < mid and right < end  # 두 부분 배열을 비교하여 temp에 정렬시킴
        if A[left] <= A[right]    # 동일한 값에 대해 왼쪽 부분 배열을 우선적으로 선택하여 Stable Sorting
            temp[idx] = A[left]
            idx += 1
            left += 1
        else
            temp[idx] = A[right]
            idx += 1
            right += 1
    
    while left < mid    # 남은 left 부분 배열을 temp에 입력
        temp[idx] = A[left]
        idx += 1
        left += 1
    
    while right < end   # 남은 right 부분 배열을 temp에 입력
        temp[idx] = A[right]
        idx += 1
        right += 1

    copy temp to A (index range start -> end-1)   # 정렬된 temp 배열을 A에 다시 복사하여 저장
```
### 수도코드 (Bottom-up)
```
function merge(start, mid, end) # 재귀 코드의 merge 함수와 동일

width = 1

while width < A.length
    start = 0

    while start < A.length     # 동일한 width 크기의 부분 배열을 합치는 사이클
        mid = min(start + width, A.length)
        end = min(start + 2 * width, A.length)

        if mid < end    # right의 부분 배열이 하나라도 남아있으면? -> merge
          merge(start, mid, end)
        
        start += 2 * width    # 다음 부분 배열

    width *= 2    # 다음 width 사이클
```

### 복잡도
**시간복잡도**
- 배열을 분할하는 단계가 `logN`
- 각 단계별로 전체 원소를 비교하며 병합하므로 `N`
- 따라서 시간복잡도는 `O(N*logN)`

**공간복잡도**
- 배열 기반으로 구현한 경우, 원본 배열 크기와 동일한 임시저장할 배열이 필요하므로 `O(N)`
- 재귀 방식은 분할 과정이 호출스택에 쌓이므로 추가적으로 `O(logN)`
- 따라서 공간복잡도는 `O(N)`(배열 기반)
- 만약 LinkedList로 구현한다면 `O(logN)`(재귀) 혹은 `O(1)` (Bottom-up 방식)

### 장단점
**장점**
- 입력 데이터 상태와 관계없이 데이터량에 비례하여 실행시간이 일정하다. (최선, 최악이어도 시간복잡도가 `O(N*logN)`으로 일정)
- Stable Sort

**단점**
- 배열 기반으로 구현할 경우, 임시배열을 복사하는 과정이 비용이 듦 (LinkedList를 사용하면 복사비용x)
