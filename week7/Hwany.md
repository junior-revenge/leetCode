# 567. Permutation in String
## Solution
```java
class Solution {

     public boolean useBruteForce(String s1, String s2){
       
        int len1 = s1.length(), len2 = s2.length();

        if(len2 < len1) return false;

        // s2의 문자열을 하나씩 순회합니다. 근데 s1의 길이만큼 이동하면서 확인하기 때문에 
        // 근데 마지막이 끝까지 가야함 
        for(int start = 0; start < len2 - len1 + 1; start++) {
            int[] nums = new int[128];

            // 두번째는 0에서 s1의 길이까지 문자열 체크 
            for(int i = 0; i < len1; i++){
               nums[s1.charAt(i)]++;
               nums[s2.charAt(start+i)]--;
            }

            boolean isPermutation = true;
            for(int n : nums) {
                if(n != 0) {
                    isPermutation = false;
                    break;
                }
            } 
            
            if(isPermutation) {
                return true;
            }
        }
        
        return false;
    }
    
}
```
# 482. License Key Formatting
## Solution
```java
class Solution {

    public String licenseKeyFormatting(String s, int k) {
        StringBuilder sb = new StringBuilder();

        // 1. 전체 유효 문자 개수 구하기
        int digitCount = 0;
        for (char c : s.toCharArray()) {
            if (Character.isLetterOrDigit(c)) digitCount++;
        }

        // 2. 첫 그룹 크기 (나머지)
        int firstGroup = digitCount % k;
        if(firstGroup == 0) firstGroup = k; // 첫 그룹이 정확히 k개일수도있음;
        System.out.println(firstGroup);
        int count = 0; // 현재 그룹안에서 몇개 넣었는지 

        for(char ch : s.toCharArray()) {
            if(Character.isLetterOrDigit(ch)){
               
                if(firstGroup == count) {
                    sb.append('-');
                    count = 0;
                    firstGroup = k;// 이후부터는 모두 k단위 그룹 
                }
                
                sb.append(ch);  
                count++;

            }
        }
        return new String(sb);
    }    
}
```
 
## Solution
```java

class Solution {

    public static reFormat(String s) {
        // insert는 미리 공간을 확보해야한다. 그래서 앞에서 넣든 뒤에서 넣을때 중복을 피할수있다. 
        // 미리 만들어야할 공간크기는 숫자의 갯수가 필요하다 
        int digitCount = 0;
        for(char ch : s.toCharArray()){
            if(ch >= '0' && ch <= '9'){
                digitCount++; 
            }
        }

        int space = digitCount + (digitCount / 3) - 1; 
        char[] result = new char[space];

        int i = 0, j = 0, count = 0;
        while(i < s.length()) {
            
            // j가 증가할때는 i의 문자열값이 숫자일때 하나씩 증가합니다. 
            char ch = s.charAt(i);
            if(ch >= '0' && ch <= '9'){
                result[j] = ch;
                j++;
                count++;

                if(count % 3 == 0 && count < digitCount) {
                    result[j] = ' ';
                    j++; 
                }
            }
           
            i++;
        }
        
        return new String(result);
    }
   
}
 


```