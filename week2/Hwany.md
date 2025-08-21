# Ransom Note 383

## 문제 설명
주어진 `ransomNote` 문자열이 `magazine` 문자열 안에 있는 문자들로 구성될 수 있는지 확인하는 문제.  
- 각 문자는 magazine에서 한 번만 사용 할 수 있다.
- ransomNote를 만들 수 있으면 `true`, 아니면 `false`를 반환한다.

---

## solution

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        
        // 문자열을 char 배열로 변환
        char[] r = ransomNote.toCharArray();
        char[] m = magazine.toCharArray();

        // HashMap 사용: key = 문자, value = 문자 카운트
        Map<Character, Integer> map = new HashMap<>();

        // ransomNote 문자들을 먼저 map에 넣어 카운트
        for(char c : r) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }

        // magazine 문자들을 순회하며 map의 key와 비교
        for(char c : m) {
            if(map.containsKey(c)) { // key가 있으면
                int count = map.get(c) - 1; // count 감소
                if(count == 0) {
                    map.remove(c); // 다 사용했으면 제거
                } else {
                    map.put(c, count);
                }
            }
        }

        // map이 비어있으면 모든 문자를 소진 → ransomNote 만들 수 있음
        return map.isEmpty();    
    }
}

```

## 접근 아이디어 
HashMap 사용 이유 (문자열이 고정이 아니다 고정이였으면 배열 사용)
- ransomNote와 magazine의 문자열을 비교하며 중복 문자를 카운팅하기 위해 사용.
- HashMap을 사용하면 문자별 등장 횟수를 쉽게 관리할 수 있고, O(1) 접근이 가능.
- 간단하고 직관적이며 O(n + m) 시간복잡도