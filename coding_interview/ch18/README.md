# 18장 이진 검색

## 개요

> 이진 검색(Binary Search)이란 정렬된 배열에서 타겟을 찾는 검색 알고리즘이다.  

- 이진 탐색 트리(Binary Search Tree, BST)가 정렬된 구조를 저장하고 탐색하는 ‘자료구조’라면, 이진 검색은 정렬된 배열에서 값을 찾아내는 ‘알고리즘’ 자체를 지칭한다.

### 이진 검색으로 숫자를 맞추는 과정

![](https://user-images.githubusercontent.com/72365720/103542729-b5858780-4ee0-11eb-99c3-2101b19bf2ad.png)

- 먼저 50부터 탐색을 시작한다. 77은 50보다 크므로 왼쪽 포인터를 오른쪽으로 이동한다.  
- 그 다음은 75다. 77은 75보다 크므로 마찬가지로 왼쪽 포인터를 오른쪽으로 이동한다.  
- 다음은 87이다. 77은 87보다는 작으므로 이번에는 오른쪽 포인터를 왼쪽으로 이동한다.  
- 이런 식으로 점점 범위를 좁혀 나가면 7번 이내에 무조건 숫자를 맞출 수 있게 된다. 마찬가지로 이 그림에서도 7번의 비교로 정답을 찾아냈다.  
- 100의 이진 로그 log2 100의 결과를 이와 같이 코드로 계산해보면 6.6 정도로 7번 만에 모두 찾아낼 수 있다.  
- 반대로 얘기하면, 2n은 금방 커질 수 있다는 얘기이기도 하다.

## 문제65 이진 검색

> 정렬된 nums를 입력받아 이진 검색으로 target에 해당하는 인덱스를 찾아라.  

입력  
nums = [-1,0,3,5,9,12], target = 9  

출력  
4

### 풀이1 재귀 풀이

절반씩 범위를 줄여나가며 맞출 때까지 계속 재귀 호출하면 된다.


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        def binary_search(left, right):
            if left <= right:
                # 자료형을 초과하지 않는 중앙 위치 계산
                mid = left + (right - left) // 2

                if nums[mid] < target:
                    return binary_search(mid + 1, right)
                elif nums[mid] > target:
                    return binary_search(left, mid - 1)
                else:
                    return mid
            else:
                return -1

        return binary_search(0, len(nums) - 1)
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([-1,0,3,5,9,12], 9))
```

    4
    


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        def binary_search(left, right):
            print("left:",left,", right:",right,"일 때,")
            if left <= right:
                mid = left + (right - left) // 2 # 자료형을 초과하지 않는 중앙 위치 계산
                print("mid:",mid,", nums[mid]:",nums[mid],"\n")

                if nums[mid] < target:
                    return binary_search(mid + 1, right)
                elif nums[mid] > target:
                    return binary_search(left, mid - 1)
                else:
                    return mid
            else:
                return -1

        return binary_search(0, len(nums) - 1)
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([-1,0,3,5,9,12], 9))
```

    left: 0 , right: 5 일 때,
    mid: 2 , nums[mid]: 3 
    
    left: 3 , right: 5 일 때,
    mid: 4 , nums[mid]: 9 
    
    4
    

### 풀이2 반복 풀이

- 대부분의 재귀 풀이는 반복 풀이로 변경할 수 있다.  
- 대개는 재귀 풀이가 더 우아한 편이지만, 반복 풀이는 좀 더 직관적이라 이해가 쉽다는 장점이 있다.


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right) // 2

            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        return -1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([-1,0,3,5,9,12], 9))
```

    4
    


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            print("left:",left,", right:",right,"일 때,")
            mid = (left + right) // 2
            print("mid:",mid,", nums[mid]:",nums[mid],"\n")

            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        return -1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([-1,0,3,5,9,12], 9))
```

    left: 0 , right: 5 일 때,
    mid: 2 , nums[mid]: 3 
    
    left: 3 , right: 5 일 때,
    mid: 4 , nums[mid]: 9 
    
    4
    

### 풀이3 이진 검색 모듈

- 사실 파이썬에서는 이진 검색을 직접 구현할 필요가 없다.  
- 이진 검색 알고리즘을 지원하는 bisect 모듈을 기본으로 제공하기 때문이다.  
- 여러 가지 예외 처리를 포함한 이진 검색 알고리즘이 깔끔하게 모듈 형태로 구현되어 있으므로, 이 모듈을 이용하면 이진 검색을 파이썬다운 방식으로 다음과 같이 좀 더 간단히 풀이할 수 있다.


```python
import bisect
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        index = bisect.bisect_left(nums, target)

        if index < len(nums) and nums[index] == target:
            return index
        else:
            return -1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([-1,0,3,5,9,12], 9))
```

    4
    

`bisect.bisect_left(a, x[, lo[, hi]]) -> index`  
리스트 a에 항목 x를 삽입할 index를 반환합니다. (a가 정렬되었다고 가정)  
*Return the index where to insert item x in list a, assuming a is sorted.*

### 풀이4 이진 검색을 사용하지 않는 index 풀이

- 파이썬에서 제공하는, 해당 값의 인덱스를 찾아내는 index() 메소드를 활용하는 방법이다.  
- 이 경우 존재하지 않는 값이라면 에러가 발생하므로, 에러인 ValueError를 예외 처리하여 -1을 리턴하도록 처리하면 풀이가 가능하다.  
- 이 방식은 이진 검색이 아니며, 이진 검색을 요구하는데 이렇게 풀이해선 안 된다.


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        try:
            return nums.index(target)
        except ValueError:
            return -1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([-1,0,3,5,9,12], 9))
```

    4
    

### 정리

- 모든 풀이 방식은 속도 차이가 거의 없으며, 재귀를 이용한 풀이가 가장 느린 편이다.  
- 실제로 코딩 테스트 시에는 가급적 재귀나 반복으로 직접 이진 검색을 풀이하는 편이 나중에 코드 리뷰를 하게 되는 경우 더 좋은 평가를 받을 수 있을 것이다.  
- 앞에서부터 차례대로 찾는 index() 함수는 최악의 경우 O(n)으로, 뒤에 위치한 값일수록 점점 찾는 속도가 느려지며 1,000배 가까이 차이 나는 경우도 생길 수 있다.  
- 반면 이진 검색은 항상 일정한 속도를 보인다. 따라서 배열의 크기가 크고, 찾아야 하는 값이 항상 앞에만 있는 게 아니라면, 파이썬의 이진 검색 모듈인 bisect를 적극 활용해야 한다.

## 문제66 회전 정렬된 배열 검색

> 특정 피벗을 기준으로 회전하여 정렬된 배열에서 target 값의 인덱스를 출력하라.  

입력  
nums = [4,5,6,7,0,1,2], target = 1  

출력  
5  

설명  
- 정렬된 입력값은 [0,1,2,4,5,6,7]이며 여기서 이진 검색을 통해 1의 위치를 찾는다.(위치 1)  
- 원래의 입력값에서 얼마만큼 돌아가 있는지를 확인하여(4칸), '위치 1 + 4칸 = 인덱스 5'를 리턴한다.

### 풀이1 피벗을 기준으로 한 이진 검색


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # 예외 처리
        if not nums:
            return -1

        # 최소값 찾아 피벗 설정
        left, right = 0, len(nums) - 1
        while left < right:
            mid = left + (right - left) // 2

            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid

        pivot = left
        # 피벗 기준 이진 검색
        left, right = 0, len(nums) - 1
        while left <= right:
            # 자료형을 초과하지 않는 중앙 위치 계산
            mid = left + (right - left) // 2
            mid_pivot = (mid + pivot) % len(nums)

            if nums[mid_pivot] < target:
                left = mid + 1
            elif nums[mid_pivot] > target:
                right = mid - 1
            else:
                return mid_pivot
        return -1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([4,5,6,7,0,1,2], 1))
```

    5
    


```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # 예외 처리
        if not nums:
            return -1

        # 최소값 찾아 피벗 설정
        left, right = 0, len(nums) - 1
        while left < right:
            print("left:",left,", right:",right,"일 때,")
            mid = left + (right - left) // 2
            print("mid:",mid,", nums[mid]:",nums[mid],", nums[right]:",nums[right],"\n")

            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid

        pivot = left
        print("pivot:",pivot,"\n")
        # 피벗 기준 이진 검색
        left, right = 0, len(nums) - 1
        while left <= right:
            print("left:",left,", right:",right,"일 때,")
            mid = left + (right - left) // 2 # 자료형을 초과하지 않는 중앙 위치 계산
            print("mid:",mid)
            mid_pivot = (mid + pivot) % len(nums)
            print("mid_pivot:",mid_pivot,", nums[mid_pivot]:",nums[mid_pivot],"\n")

            if nums[mid_pivot] < target:
                left = mid + 1
            elif nums[mid_pivot] > target:
                right = mid - 1
            else:
                return mid_pivot
        return -1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.search([4,5,6,7,0,1,2], 1))
```

    left: 0 , right: 6 일 때,
    mid: 3 , nums[mid]: 7 , nums[right]: 2 
    
    left: 4 , right: 6 일 때,
    mid: 5 , nums[mid]: 1 , nums[right]: 2 
    
    left: 4 , right: 5 일 때,
    mid: 4 , nums[mid]: 0 , nums[right]: 1 
    
    pivot: 4 
    
    left: 0 , right: 6 일 때,
    mid: 3
    mid_pivot: 0 , nums[mid_pivot]: 4 
    
    left: 0 , right: 2 일 때,
    mid: 1
    mid_pivot: 5 , nums[mid_pivot]: 1 
    
    5
    

- 여기서 가장 작은 값을 찾는다면 해당 위치의 인덱스가 피벗이 될 수 있을 것 같다.  
- 최솟값 left를 찾아내 pivot으로 구성하고, 이를 기준으로 피벗의 위치만큼 살짝 틀어준 mid_pivot을 구성한 다음, 다시 이진 검색을 통해 target 값을 찾았다.  
- mid_pivot은 중앙의 위치 mid에 피벗 pivot만큼 이동하고, 배열의 길이를 초과할 경우 모듈로 연산으로 회전될 수 있도록 처리했다.  
- 이제 타겟과 값을 비교하는 부분은 mid가 아닌 mid_pivot을 기준으로 하되, left와 right는 mid를 기준으로 이동한다.

![](https://user-images.githubusercontent.com/72365720/103544846-0c409080-4ee4-11eb-8d24-83646c00dc44.png)

- 값에 대한 비교는 mid_pivot의 위치를 기준으로 하지만, mid의 이동은 기존 이진 검색과 동일하게 left, right를 기준으로 한다.  
- 즉 다른 포인터를 가리키는 셈이며, 실제로 값에 대한 비교는 pivot의 위치인, 4칸 우측으로 떨어진 mid_pivot을 기준으로 한다.  
- 당연히 최종 결과도 mid_pivot의 값을 리턴받아 결과로 삼는다.

## 문제67 두 배열의 교집합

> 두 배열의 교집합을 구하라.  

**예제 1**  

입력  
nums1 = [1,2,2,1], nums2 = [2,2]  

출력  
[2]  

**예제 2**  

입력  
nums1 = [4,9,5], nums2 = [9,4,9,8,4]  

출력  
[9.4]

### 브루트 포스로 계산


```python
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        for n1 in nums1:
            for n2 in nums2:
                if n1 == n2:
                    result.add(n1)

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([1,2,2,1], [2,2]))
```

    {2}
    


```python
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        for n1 in nums1:
            for n2 in nums2:
                if n1 == n2:
                    result.add(n1)

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([4,9,5], [9,4,9,8,4]))
```

    {9, 4}
    

- O(n2)으로 반복하면서 일치하는 경우 무조건 추가한다.  
- 데이터 타입은 집합set이기 때문에 속도는 느리긴 해도 중복된 값은 알아서 잘 처리해줄 것이다.

### 풀이2 이진 검색으로 일치 여부 판별


```python
import bisect
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        nums2.sort()
        for n1 in nums1:
            # 이진 검색으로 일치 여부 판별
            i2 = bisect.bisect_left(nums2, n1)
            if len(nums2) > 0 and len(nums2) > i2 and n1 == nums2[i2]:
                result.add(n1)

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([1,2,2,1], [2,2]))
```

    {2}
    


```python
import bisect
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        nums2.sort()
        nums1_index = 0
        for n1 in nums1:
            print("n1:",n1,", nums1[",nums1_index,"]")
            nums1_index = nums1_index + 1
            # 이진 검색으로 일치 여부 판별
            i2 = bisect.bisect_left(nums2, n1)
            print("i2:",i2)
            print("n1 == nums2[i2]:",n1 == nums2[i2],"\n")
            if len(nums2) > 0 and len(nums2) > i2 and n1 == nums2[i2]:
                result.add(n1)

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([1,2,2,1], [2,2]))
```

    n1: 1 , nums1[ 0 ]
    i2: 0
    n1 == nums2[i2]: False 
    
    n1: 2 , nums1[ 1 ]
    i2: 0
    n1 == nums2[i2]: True 
    
    n1: 2 , nums1[ 2 ]
    i2: 0
    n1 == nums2[i2]: True 
    
    n1: 1 , nums1[ 3 ]
    i2: 0
    n1 == nums2[i2]: False 
    
    {2}
    


```python
import bisect
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        nums2.sort()
        for n1 in nums1:
            # 이진 검색으로 일치 여부 판별
            i2 = bisect.bisect_left(nums2, n1)
            if len(nums2) > 0 and len(nums2) > i2 and n1 == nums2[i2]:
                result.add(n1)

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([4,9,5], [9,4,9,8,4]))
```

    {9, 4}
    

- 한쪽은 순서대로 탐색하고 다른 쪽은 정렬해서 이진 검색으로 값을 찾으면, 검색 효율을 획기적으로 높일 수 있다.  
- 이 문제를 18장에서 다루는 이유기도 하다. 이 경우 시간 복잡도는 O(n log n)이 될 것이다.  
- nums2는 정렬한 상태에서, nums1을 O(n) 순차 반복하면서 nums2를 O(log n) 이진 검색한다.  
- 최초 정렬에 소요되는 O(n log n)을 감안해도 전체 O(n log n)에 가능하므로 앞서 O(n2)에 비해 훨씬 좋은 성능을 보인다.

### 풀이3 투 포인터로 일치 여부 판별


```python
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        # 양쪽 모두 정렬
        nums1.sort()
        nums2.sort()
        i = j = 0
        # 투 포인터 우측으로 이동하며 일치 여부 판별
        while i < len(nums1) and j < len(nums2):
            if nums1[i] > nums2[j]:
                j += 1
            elif nums1[i] < nums2[j]:
                i += 1
            else:
                result.add(nums1[i])
                i += 1
                j += 1

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([1,2,2,1], [2,2]))
```

    {2}
    


```python
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        # 양쪽 모두 정렬
        nums1.sort()
        nums2.sort()
        i = j = 0
        # 투 포인터 우측으로 이동하며 일치 여부 판별
        while i < len(nums1) and j < len(nums2):
            if nums1[i] > nums2[j]:
                j += 1
            elif nums1[i] < nums2[j]:
                i += 1
            else:
                result.add(nums1[i])
                i += 1
                j += 1

        return result
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.intersection([4,9,5], [9,4,9,8,4]))
```

    {9, 4}
    

- 양쪽 다 정렬하여 투 포인터로 풀이할 수도 있다.  
- 마치 병합 정렬 시 마지막에 최종 결과를 비교하는 과정과 유사하다.  
- 다만 일치하는 값을 판별한다는 차이만 있을 뿐이다.  
- 값이 작은 쪽 배열의 포인터가 한 칸씩 앞으로 이동하는 형태로 해서, 어느 한쪽의 포인터가 끝까지 도달하면 종료한다.

## 문제68 두 수의 합 II

> 정렬된 배열을 받아 덧셈하여 타겟을 만들 수 있는 배열의 두 숫자 인덱스를 리턴하라.  
> ※ 주의: 이 문제에서 배열은 0이(Zero-Based) 아닌 1부터 시작하는 것으로 한다.  

입력  
numbers = [2,7,11,15], target = 9  

출력  
[1,2]

### 풀이1 투 포인터

- 앞서 7장에서 7번 '두 수의 합' 문제는 투 포인터로 풀 수 없다(풀이 #5)고 했다.  
- 그러나 그 문제의 다른 버전 격인 이 문제는 입력 배열이 정렬되어 있다는 가정이 추가됐다.  
- 따라서 이 문제는 투 포인터로도 잘 풀릴 것이다.  


```python
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        while not left == right:
            if numbers[left] + numbers[right] < target:
                left += 1
            elif numbers[left] + numbers[right] > target:
                right -= 1
            else:
                return left + 1, right + 1  # 리턴 값 +1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.twoSum([2,7,11,15], 9))
```

    (1, 2)
    

- 이 문제의 경우 특별히 0이 아닌 1부터 시작한다고 했으니, 리턴하는 부분의 값에 각각 +1을 하고, 문제에서 입력값에 해당하는 변수명이 기존 nums에서 numbers로 바뀌었으니 이 부분만 수정해주면 문제 없이 잘 풀릴 것 같다.

### 풀이2 이진 검색

- 현재 값을 기준으로 나머지 값이 맞는지 확인하는 형태의 이진 검색 풀이를 시도해볼 수 있을 것 같다.


```python
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            left, right = k + 1, len(numbers) - 1
            expected = target - v
            # 이진 검색으로 나머지 값 판별
            while left <= right:
                mid = left + (right - left) // 2
                if numbers[mid] < expected:
                    left = mid + 1
                elif numbers[mid] > expected:
                    right = mid - 1
                else:
                    return k + 1, mid + 1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.twoSum([2,7,11,15], 9))
```

    (1, 2)
    


```python
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            left, right = k + 1, len(numbers) - 1
            print("k:",k,", v:",v)
            expected = target - v
            print("expected:",expected,"\n")
            # 이진 검색으로 나머지 값 판별
            while left <= right:
                print("left:",left,", right:",right,"일 때,")
                mid = left + (right - left) // 2
                print("mid:",mid,", numbers[mid]:",numbers[mid],"\n")
                if numbers[mid] < expected:
                    left = mid + 1
                elif numbers[mid] > expected:
                    right = mid - 1
                else:
                    return k + 1, mid + 1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.twoSum([2,7,11,15], 9))
```

    k: 0 , v: 2
    expected: 7 
    
    left: 1 , right: 3 일 때,
    mid: 2 , numbers[mid]: 11 
    
    left: 1 , right: 1 일 때,
    mid: 1 , numbers[mid]: 7 
    
    (1, 2)
    

### 풀이3 bisect 모듈 + 슬라이싱


```python
import bisect
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            expected = target - v
            i = bisect.bisect_left(numbers[k + 1:], expected)
            if i < len(numbers[k + 1:]) and numbers[i + k + 1] == expected:
                return k + 1, i + k + 2
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.twoSum([2,7,11,15], 9))
```

    (1, 2)
    

- left와 right 변수도 필요 없고, 예상대로 코드는 엄청나게 깔끔해졌다.  
- 그런데 문제가 있다. 이 풀이의 실행 속도는 앞서 이진 검색 풀이보다 20배 이상 느려졌다.  
- 왜 그럴까? 성능 개선을 시도해보자.

### 풀이4 bisect 모듈 + 슬라이싱 최소화


```python
import bisect
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            expected = target - v
            nums = numbers[k + 1:]
            i = bisect.bisect_left(nums, expected)
            if i < len(nums) and numbers[i + k + 1] == expected:
                return k + 1, i + k + 2
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.twoSum([2,7,11,15], 9))
```

    (1, 2)
    

- 아무래도 파이썬 슬라이싱을 무리하게 적용한 게 원인인 듯 하여 nums 변수에 한 번만 사용해 담아두는 형태로 다음과 같이 개선을 시도해봤다.  
- 이렇게 하니 앞서 풀이3에 비해 2배가량 속도가 빨라졌다.  
- 그래도 풀이1에 비해서는 많이 느리다. 좀 더 개선할 방법이 필요하다.

### 풀이5 bisect 모듈 + 슬라이싱 제거


```python
import bisect
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            expected = target - v
            i = bisect.bisect_left(numbers, expected, k + 1)
            if i < len(numbers) and numbers[i] == expected:
                return k + 1, i + 1
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.twoSum([2,7,11,15], 9))
```

    (1, 2)
    

- `bisect.bisect_left(a, x, lo=0, hi=len(a))`
- 왼쪽 범위를 제한하는 파라미터인 lo를 찾아냈고, 이 값을 지정하는 것으로 풀이를 진행했다.

### 정리

- 비록 슬라이싱은 편리하고 빠른 모듈이지만, 이처럼 생각 없이 무분별하게 남용하다 보면 속도 저하의 주범이 될 수 있다.  
- 여기서는 테스트 케이스의 입력값이 매우 크기 때문에 슬라이싱에서 속도 저하가 발생한 듯하다.  
- 경우에 따라서는, 꼭 필요한 곳에만 적절히사용해야 실행 속도를 좀 더 최적화할 수 있다.

### 슬라이싱 성능

- 파이썬의 슬라이싱은 매우 효율적이고 빠르다고 했는데 왜 이런 일이 발생할까?  
- 그 이유는 슬라이싱은 매번 새롭게 객체를 생성해서 할당하게 되고, 엄청나게 큰 배열의 경우 슬라이싱으로 새로운 객체를 생성하는 데 상당한 시간이 들기 때문이다.  
- 마찬가지로 배열의 길이를 계산하는 데도 매번 길이를 계산하는 비용이 들기 때문에, 여기에 상당한 시간이 소요된다.  
- 슬라이싱을 하지 않을 경우 배열의 길이는 별도 속성으로 미리 계산해서 갖고 있는 값이므로 매번 계산할 필요 없이 해당 속성을 리턴하기만 하면 되지만, 슬라이싱은 매번 새롭게 배열의 길이를 계산해야 한다. 새로운 객체이기 때문이다.  
- 이처럼 매우 큰 배열의 경우 슬라이싱을 하게 되면, 전체를 복사하는 과정이나 배열의 길이를 계산하는 부분에서 상당한 시간이 소요되므로 주의가 필요하다.

## 문제69 2D 행렬 검색 II

> m×n 행렬에서 값을 찾아내는 효율적인 알고리즘을 구현하라. 행렬은 왼쪽에서 오른쪽, 위에서 아래 오름차순으로 정렬되어 있다.  

예제  
행렬은 다음과 같다.  
```
[
    [1, 4, 7, 11, 15],
    [2, 5, 8, 12, 19],
    [3, 6, 9, 16, 22],
    [10, 13, 14, 17, 24],
    [18, 21, 23, 26, 30]
]  
```
target=5일 경우, 값이 존재하므로 true를 리턴한다.  
target=20일 경우, 값이 존재하지 않으므로 false를 리턴한다.

### 풀이1 첫 행의 맨 뒤에서 탐색

- 첫 행의 맨 뒤 요소를 택한 다음, 타겟이 이보다 작으면 왼쪽으로, 크면 아래로 이동하게 하는 방법이다.  
- 행렬은 왼쪽에서 오른쪽, 위에서 아래로 오름차순으로 정렬되어 있기 때문에, 작으면 왼쪽, 크면 아래로 이동하면 원하는 위치에 어렵지 않게 도달할 수 있을 것이다.


```python
class Solution:
    def searchMatrix(self, matrix, target):
        # 예외 처리
        if not matrix:
            return False

        # 첫 행의 맨 뒤
        row = 0
        col = len(matrix[0]) - 1

        while row <= len(matrix) - 1 and col >= 0:
            if target == matrix[row][col]:
                return True
            # 타겟이 작으면 왼쪽으로
            elif target < matrix[row][col]:
                col -= 1
            # 타겟이 크면 아래로
            elif target > matrix[row][col]:
                row += 1
        return False
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.searchMatrix([
                            [1, 4, 7, 11, 15],
                            [2, 5, 8, 12, 19],
                            [3, 6, 9, 16, 22],
                            [10, 13, 14, 17, 24],
                            [18, 21, 23, 26, 30]
                        ], 5))
```

    True
    


```python
class Solution:
    def searchMatrix(self, matrix, target):
        # 예외 처리
        if not matrix:
            return False

        # 첫 행의 맨 뒤
        row = 0
        col = len(matrix[0]) - 1

        while row <= len(matrix) - 1 and col >= 0:
            if target == matrix[row][col]:
                return True
            # 타겟이 작으면 왼쪽으로
            elif target < matrix[row][col]:
                col -= 1
            # 타겟이 크면 아래로
            elif target > matrix[row][col]:
                row += 1
        return False
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.searchMatrix([
                            [1, 4, 7, 11, 15],
                            [2, 5, 8, 12, 19],
                            [3, 6, 9, 16, 22],
                            [10, 13, 14, 17, 24],
                            [18, 21, 23, 26, 30]
                        ], 20))
```

    False
    

### 풀이2 파이썬다운 방식

- 파이썬이 내부적으로 행렬에 값이 존재하는지 여부를 위에서부터 차례대로 한 줄씩 탐색하게 될 것이다.  


```python
class Solution:
    def searchMatrix(self, matrix, target):
        return any(target in row for row in matrix)
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.searchMatrix([
                            [1, 4, 7, 11, 15],
                            [2, 5, 8, 12, 19],
                            [3, 6, 9, 16, 22],
                            [10, 13, 14, 17, 24],
                            [18, 21, 23, 26, 30]
                        ], 5))
```

    True
    


```python
class Solution:
    def searchMatrix(self, matrix, target):
        return any(target in row for row in matrix)
    
    
if __name__ == '__main__':
    s = Solution()
    print(s.searchMatrix([
                            [1, 4, 7, 11, 15],
                            [2, 5, 8, 12, 19],
                            [3, 6, 9, 16, 22],
                            [10, 13, 14, 17, 24],
                            [18, 21, 23, 26, 30]
                        ], 20))
```

    False
    

- 언뜻 이진 검색과 관련된 문제로 보이고 이 장의 주제 또한 ‘이진 검색’이지만, 아이러니하게도 이 문제는 이진 검색으로 풀이가 어렵고, 조금 다른 방법을 사용해야 했다.  
- 이처럼 예상과 달리 다른 방법으로 풀리는 경우가 있으므로, 실제 코딩 테스트 시에도 예상 방식대로 풀이가 되지 않는다면 그 방식에 너무 많은 시간을 허비하지 않도록 유의해야 한다.
