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

