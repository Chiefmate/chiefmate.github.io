---
title: "합병 정렬 (Merge Sort) 배우기"
layout: posts
---

합병 정렬에 대하여 배워보자.

# 개요
1. 존 폰 노이만(John von Neumann)이 1945년에 개발했다.
1. 대표적인 분할 정복(Divide-and-Conquer) 방식 알고리즘
1. 시간복잡도, 공간복잡도
| 최악 시간복잡도 | 최선 시간복잡도 | 평균 시간복잡도 | 공간복잡도 |
| :---:			| :-------------: | :------------: | :-------: |
|O(n log n)		|	O(n log n)		|일반적으로, O(n log n) | 	O(n)	|

# 알고리즘

## n-way 합병 정렬
1. `정렬되지 않은 리스트`를 n개의 `하나의 원소를 갖는 부분리스트`로 분할한다. (원소가 1개이므로 정렬된 리스트)
1. `부분리스트`가 하나만 남을 때까지 반복하여 `병합`하며 `정렬된 부분리스트`를 생성한다. 마지막으로 남은 리스트는 모든 원소가 정렬된 리스트이다.

## 하향식 2-way 합병 정렬
1. `리스트`의 길이가 1 이하이면 이미 정렬된 것으로 본다.
1. 그렇지 않은 경우, 그 리스트를 **분할(divide)**한다.<br>정렬되지 않은 리스트를 절반으로 잘라 두 개의 `부분리스트`로 나눈다.
1. **정복(conquer)**: 각 `부분리스트`를 재귀적으로 `합병 정렬`을 이용하여 정렬한다.
1. 결합(combine): 두 개의 `부분리스트`를 다시 하나의 정렬된 리스트로 합병한다. 이 때 정렬 결과가 임시 배열에 저장된다.
1. 복사(copy): 임시 배열에 저장된 결과를 원래 배열에 복사한다.

# 소스코드

```
// Array A[] has the items to sort; array B[] is a work array.
TopDownMergeSort(A[], B[], n)
{
    CopyArray(A, 0, n, B);           // duplicate array A[] into B[]
    TopDownSplitMerge(B, 0, n, A);   // sort data from B[] into A[]
}

// Sort the given run of array A[] using array B[] as a source.
// iBegin is inclusive; iEnd is exclusive (A[iEnd] is not in the set).
TopDownSplitMerge(B[], iBegin, iEnd, A[])
{
    if(iEnd - iBegin < 2)                       // if run size == 1
        return;                                 //   consider it sorted
    // split the run longer than 1 item into halves
    iMiddle = (iEnd + iBegin) / 2;              // iMiddle = mid point
    // recursively sort both runs from array A[] into B[]
    TopDownSplitMerge(A, iBegin,  iMiddle, B);  // sort the left  run
    TopDownSplitMerge(A, iMiddle,    iEnd, B);  // sort the right run
    // merge the resulting runs from array B[] into A[]
    TopDownMerge(B, iBegin, iMiddle, iEnd, A);
}

//  Left source half is A[ iBegin:iMiddle-1].
// Right source half is A[iMiddle:iEnd-1   ].
// Result is            B[ iBegin:iEnd-1   ].
TopDownMerge(A[], iBegin, iMiddle, iEnd, B[])
{
    i = iBegin, j = iMiddle;

    // While there are elements in the left or right runs...
    for (k = iBegin; k < iEnd; k++) {
        // If left run head exists and is <= existing right run head.
        if (i < iMiddle && (j >= iEnd || A[i] <= A[j])) {
            B[k] = A[i];
            i = i + 1;
        } else {
            B[k] = A[j];
            j = j + 1;
        }
    }
}

CopyArray(A[], iBegin, iEnd, B[])
{
    for(k = iBegin; k < iEnd; k++)
        B[k] = A[k];
}
```

# 참고
[위키백과, 합병 정렬](https://ko.wikipedia.org/wiki/%ED%95%A9%EB%B3%91_%EC%A0%95%EB%A0%AC)
[k-way 병합 정렬의 시간복잡도 계산기](https://pro-programmer.tistory.com/entry/kway-Merge-Sort%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC%EC%9D%98-%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84-%EA%B3%84%EC%82%B0%ED%95%98%EA%B8%B0)