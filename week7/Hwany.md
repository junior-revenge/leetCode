 ## Solution

```java
 public static void main(String[] args) {
        String s = "1, 23, 45, 6"; //→> "123 456"
       // String s2 = "1,23,34"; //=> "123 34"
        reFormat(s);
    }
    
    // bruteForce
    public static String reFormat(String s) {
  
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