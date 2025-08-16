# 27.Remove Element

## Solution
```java

public int removeElement(int[] nums, int val) {

    int left = 0, right = nums.length - 1, k = 0;

    while(left <= right){
        if(nums[left] == val) {
            nums[left] = nums[right];
            right--;
        }else {
            left++;
            k++;
        }
    }
    return k; 
}

```
### 접근 아이디어 
- in-place → 새로운 배열 사용 불가
- 순서 유지 필요 없음 → swap 방식 사용 가능
- Two Pointer 패턴 → left → 앞에서부터 검사, right → 뒤에서부터 검사

### 전체 설명 
이 문제는 in-place로 구현해야 하고, 배열의 순서를 유지할 필요가 없기 때문에 두 포인터를 이용한 swap 방식이 적합하다고 생각했다.
근데 변수도 하나만 할당하고 for문으로 더 간단하게 구현할수있다. 순서는 상관없으니까, swap방식말고 배열에 요소를 앞에서 부터 val이 아닌값으로 overwrite 할 수 있다. (k값이 배열의 요소와 val값이 같은 위치를 가리킬수있다.)

```java

public int removeElement(int[] nums, int val) {

    int k = 0;

    for(int i = 0; i < nums.lenght; i++){
        if(nums[i] == val){nums[k++] = nums[i];} 
    }
    return k; 
}

```
--- 
# 26.Remove Duplicates from Sorted Array

## Solution
```java
    public int removeDuplicates(int[] nums) {

        int p2 = 1;

        for(int p1 = 0; p1 < nums.length - 1; p1++) {
            if(nums[p1] != nums[p1+1]) {
                nums[p2++] = nums[p1+1];
            }
        }

        return p2;
    }

```

### 접근 아이디어 
앞뒤요소가 같은지 확인하면 계속 반복문을 실행한다. 위치 포인터, 계속 탐색할 포인터가 필요하다 그래서 two pointer 사용하여 배열에 overwrite로 요소들을 채워넣는다. 

--- 

# 80.Remove Duplicates from Sorted Array ii

## Solution
```java
class Solution {
    public int removeDuplicates(int[] nums) {

        int k = 0;

        for(int i = 0; i< nums.length; i++) {
            if(k < 2 || nums[i] != nums[k-2]){
                nums[k++] = nums[i];
            }
        }
    }
}
```

### 접근 아이디어 
처음에 배열 요소의 앞뒤값을 비교하여 카운트를 세고 2이상이면 교체되는 형식으로 하려고했는데 그렇게 하면 인덱스가 깨져셔 똑같은값이 또 그 자리를 들어올 수도 있다(중복이 허용된다면 앞뒤 비교가능함). 루프를 돌면서, 현재 값이 두 번 이상 연속되는지 확인하는 방법은 앞뒤 비교가 아니라, 현재 쓰는 위치에서 -2 위치에 있는 값과 지금 값을 비교하는 것이다. 만약 같으면 중복이 세 번 이상 되는 거니까 넣지 않고, 다르면 넣는다.

