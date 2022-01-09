# Sort
## 1. Selection Sort

최소값(최대값)을 찾아서 맨 앞에 위치한 값(맨 뒤에 위치한 값)과 swap한다. swap이 완료된 맨 앞(맨 뒤)값을 제외하고 같은 과정을 반복한다.

 * 시간복잡도
**O(n^2)**

첫 번째 회전에서의 비교횟수 : 1 ~ (n-1) => n-1

두 번째 회전에서의 비교횟수 : 2 ~ (n-1) => n-2

...

(n-1) + (n-2) + .... + 2 + 1 => n(n-1)/2

알고리즘이 단순하며 정렬하고자 하는 배열 안에서 교환하는 방식이므로 다른 공간을 필요로 하지 않는다. (제자리 정렬)

## 2. Bubble Sort

selection sort와 유사한 알고리즘으로, 

1회전에 첫 번째 원소와 두 번째 원소, 두 번째 원소와 세 번째 원소 .. 대소비교하여 swap한다. 2회전부터는 맨 앞에(맨 뒤에) 위치한 가장 작은(가장 큰) 원소를 제외하고 같은 과정을 반복한다.
* 시간복잡도
**O(n^2)**

선택정렬과 마찬가지로 

(n-1) + (n-2) + .... + 2 + 1 => n(n-1)/2

## 3. Insert Sort

selection sort와 유사하지만, 조금 더 효율적이다. 

두 번째 원소부터 시작해서 왼쪽에 위치한 원소 모두와 대소비교하며 swap한다. 

int arr[] = {2,3,5,6,7,8,**4**,1};

해당 배열의 오름차순 정렬을 위해서, 4는 8과 비교 후 swap, 7과 비교 후 swap .. 3과 비교 후 swap을 멈추고 그 자리에 자리잡는다. 
```cpp
// 시작
int arr[] = {2,3,5,6,7,8, 4 ,1};
// 4가 자리잡기까지
int arr[] = {2,3,5,6,7, 4 ,8,1};
int arr[] = {2,3,5,6, 4 ,7,8,1};
int arr[] = {2,3,5, 4 ,6,7,8,1};
int arr[] = {2,3, 4 ,5,6,7,8,1};
```
* 시간복잡도

최악의 경우 **O(n^2)**

그러나 모두 정렬이 되어있는 경우(Optimal)한 경우, 한번씩 밖에 비교를 안하므로 **O(n)**
## **분할정복법에 의한 정렬 알고리즘**
작은 크기로 분할 -> 각각의 작은 문제를 순환적으로 해결 -> 작은 문제들의 해를 합해 원래 문제에 대한 해를 구함

위 단계를 거치는 정렬 알고리즘
## 4. Merge Sort
최대한 작은 크기로 분할한 후, 합치면서 정렬해나가는 방식이다. 따라서 합치는 부분인 merge 메소드가 핵심이다. 

오름차순 정렬을 만들고 싶을 때

(pseudo code)
```c 
배열 A[] = {p . . . . . . . . . r}

mergeSort(A[], p, r) {
    if (p < r) {                    // p>r 인 경우는 남은 원소가 1개거나 없을 때
        q = (p + q) / 2             // p와 r의 중간지점 q
        mergeSort(A, p, q)          // 전반부 정렬
        mergeSort(A, p+1, r)        // 후반부 정렬
        merge(A, p, q, r)           // 합병
    }
}

merge(A[], p, q, r) {
    i = p, j = q+1, k = p           /* 각 배열의(정렬된) 최솟값의 인덱스(가장 왼쪽 인덱스)를 각각 i와 j로 정하고 
                                    임시정렬의 가장 왼쪽 인덱스를 k로 정한다. */
    
    tmp[A.length()]                 // 임시정렬을 만든다.
    
    while (i <= q && j <= r) {      /* i가 중간값 q에 도달하거나 j가 마지막값 r에 도달하면 
                                    반복문이 종료된다. */
        if (A[i] <= A[j]) {         // 두 최솟값들을 비교하고 더 작은 최솟값을 임시정렬에 넣는다. 
            tmp[k++] = A[i++]       /* 이 때, 최솟값이 담겨있던 인덱스를 1 증가시켜 
                                    다음 수를 최솟값으로 재설정한다. */
        } else {
            tmp[k++] = A[j++]
        }
    }
    
    while (i <= q) {                /* 임시정렬에 값이 다 옮겨진, 위 while문에서 종료를 유발한 정렬 외! 
                                    나머지 정렬에 남은 값들을 임시정렬로 마저 옮긴다. */
        tmp[k++] = A[i++]
    }
    while (j <= r) {
        tmp[k++] = A[j++]
    }
    for (copy = p; copy < r; copy++) {  // 임시정렬을 원본정렬에 복붙한다.
        A[copy] = tmp[copy]
    }
}
```
* 시간복잡도

언제나 **O(nlogn)**

## 5. Quick Sort
합병정렬과 같이 분할정복법 방식이지만, 합병정렬과 달리 기준값(pivot)을 정해서 {pivot보다 작은 값, pivot, pivot보다 큰 값} 으로 분할한 후 각 부분을 순환적으로 정렬한다. 따라서 절반 분할이 보장되지 않는다. 합병 과정은 없다.

(pseudo code)
```c
배열 A[] = {p . . . . . . . . . r}
quickSort(A[], p, r) {              
    if (p < r) then {               
        q = partition(A, p, r)      // pivot q에 의한 분할
        quickSort(A, p, q-1)        // 분할 왼쪽 배열을 정렬
        quickSort(A, q+1, r)        // 분할 오른쪽 배열을 정렬
    }
}

partition(A[], p, r) {
    /* 배열 A[]가 {4,2,5,9,10, 8 ,3,11,7}, pivot값은 7, 현재 A[5]인 8으로 분류 진행 예정, A[2]까지가 pivot값보다 작은 값, A[3]부터가 pivot값보다 큰 값으로 나눠진 상태라고 가정한다면 */

    x = 8, i = 2, j = 5             /* A배열 가장 오른쪽에 있는 인덱스 x를 pivot값으로 지정한다. i는 pivot보다 작은 값들 중 가장 마지막 값의 인덱스고, j는 지금 검사하려는 값의 인덱스. */
    if (A[j] >= x) {                // 지금 검사하려는 값이 pivot값보다 크거나 같다면
        j++;                        // 그대로 두고 한칸 이동하여 새로운 검사를 준비
    } else {                        // pivot값보다 작으면
        i++;                        // 인덱스 값을 미리 1 증가시키고
        swap A[i] and A[j]          /* 지금 검사하는 값과 +1 된 인덱스의 값의 위치를 바꾼다. pivot값보다 작은 값에 포함시켜야하니까 작은 값들의 인덱스중 가장 마지막 바로 뒤에 붙여주는 것 */
        j++;                        // 한칸 이동하여 새로운 검사를 준비한다.
    }
    /* 한번의 순회가 끝나면 {4,2,5, 8 ,9,10,3,11,7} */
}
```
pivot 값 바로 전까지 분류를 완료하면 pivot을 pivot보다 작은 값들의 가장 마지막 인덱스 + 1(혹은 pivot보다 큰 값들의 가장 처음 인덱스 - 1) 자리에 넣어준다.


* 시간복잡도

최선의 경우 **O(nlog₂n)**

최악의 경우(정렬하고자 하는 배열이 이미 정렬되어 있는 상태) **O(n^2)**

### (C++ STL) sort
* **sort(arr, arr + sizeof(arr))**
* **sort(v.begin(), v.end())** vector의 처음과 끝 범위 내의 함수를 정렬. 오름차순이 default.
* **sort(v.begin(), v.end(), less<자료형>())**

**참고한 자료**

https://gyoogle.dev/blog/algorithm/Quick%20Sort.html

https://blockdmask.tistory.com/178

https://www.youtube.com/watch?v=2YvFRAC8UTM&list=PL52K_8WQO5oUuH06MLOrah4h05TZ4n38l&index=10