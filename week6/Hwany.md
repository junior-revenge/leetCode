# 125. Valid Palindrome

## A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers. Given a string s, return true if it is a palindrome, or false otherwise.

## Solution

```java
class Solution {
    public boolean isPalindrome(String s) {

        char[] ch = s.toCharArray();
        StringBuilder sb = new StringBuilder();

        for(char c : ch) {
            if(Character.isLetterOrDigit(c)){
                sb = sb.append(Character.toLowerCase(c));
            }
        }

        int left = 0, right = sb.length() - 1;
        while(left < right) {
            if(sb.charAt(left) != sb.charAt(right)){
                return false;
            }

            left++;
            right--;
        }

        return true;
        
    }
}
```
## 접근 아이디어 
문자열에 공백과 특수문자를 제거하기 위해서 StringBuilder를 사용하였다. 근데 최악의 경우 문자열이 모두 알파벡/숫자일 경우 O(n)공간복잡도는 O(n)이 되었는데 이를 최적화 O(1)로 하기 위해서는 two pointer로 풀어야함
