
# 290. Word Pattern

## Solution
```python
class Solution(object):
    def wordPattern(self, pattern, s):
        pattern_to_s = {}
        s_to_pattern = {}
        
        words = s.split()
        
        if len(words) != len(pattern):
            return False
        
        for ch, word in zip(pattern, words):
            if ch not in pattern_to_s and word not in s_to_pattern:
                pattern_to_s[ch] = word
                s_to_pattern[word] = ch
            elif pattern_to_s.get(ch) != word or s_to_pattern.get(word) != ch:
                return False
        
        return True
```

## Key Takeway
이 문제는 205. Isomorphic Strings문제와 접근법이 비슷하다.
결국 공간정보와 매핑을 동시에 처리해야하기 때문에, 해시맵을 두개 만드는게 맞다.

그리고 토크나이제이션을 할 때는 파이썬은 split(delimter)를 쓴다는걸 기억해내야 한다. split(' ') 이런 형태.

사실 그부분 빼면 isomorphic strings랑 거의 같은 문제다.
서로 하나씩 매핑을 주고받고, 둘 중 하나라도 이미 매핑이 되어있으면 검증한다. 끝.

# 242. Valid Anagram

## Solution
```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
        if len(s) == 0 and len(t) == 0:
            return True

        s_map = Counter(s)

        for ch in t:
            if ch in s_map:
                s_map[ch] -= 1

        for ch in s_map:
            if s_map[ch] != 0:
                return False

        return True
```

## Key Takeway
어렵게 생각할 필요 없고 경우를 하나하나 따져본다.

1) 일단 길이가 다르면 무조건 False다
2) 둘 다 길이가 0이면 무조건 True다
3) 한쪽 단어의 글자의 갯수를 전부 세서 저장해놓고(Counter() 호출), 그 다음에 하나씩 뺀 뒤에 0이 아닌 값이 한번이라도 나타나면 False다.
4) 이 과정을 다 거쳤는데 아무 일 없었으면 True다.

# 49. Group Anagrams

## Solution
```python
class Solution(object):
    def groupAnagrams(self, strs):
        output = defaultdict(list) 
        for word in strs:
            count = [0] * 26 

            for c in word:
                count[ord(c) - ord("a")] += 1 
            output[tuple(count)].append(word) 

        return output.values()
```

## Key Takeway

이걸 brute force로 접근할 때 어떻게되는지 설명해보면,
우선 strs에 담긴 문자열 하나마다 나머지 문자열과 비교대조를 해보면서 애너그램이면 넣고
아니면 말고하는 식으로 그룹을 지을 수 있을 것이다.
그런데 어떤 두 문자열이 anagram인지 보려면 두 문자열을 정렬해서 같은지 봐야하는데,
그러면 그것만으로 일단 O(NlogN)이 걸린다.
그리고 전체 strs의 길이가 M이라면, 비교를 위해 모든 문자열을 정렬해보는 데에만 O(MNlogN)이 소요될 것이다.
```python
class Solution(object):
    def groupAnagrams(self, strs):
        n = len(strs)
        sorted_forms = ["".join(sorted(s)) for s in strs]
        visited = [False] * n
        result = []

        for i in range(n):
            if visited[i]:
                continue

            group = [strs[i]]
            visited[i] = True

            for j in range(i + 1, n):
                if not visited[j] and sorted_forms[i] == sorted_forms[j]:
                    group.append(strs[j])
                    visited[j] = True
            result.append(group)

        return result
```


여기선 일단 문제 조건상 알파벳을 a에서 z까지 소문자만 준다고 했으니 우리는 26개의 문자만 쓴다는 것을 알 수 있다.
그래서 언제나처럼 이렇게 나올 수 있는 문자의 종류가 한정되어있는 경우, 알려진 정렬알고리듬보다 더 효율적으로 문제를 풀 수 있다.
각 문자열마다 a-z 소문자가 각각 몇번 등장하는지를 가지고 애너그램인지 여부를 판단하는 것이다.
a-z까지를 key로 하는 해시맵을 만들 수 있을 것이다. 그렇게 만들어진 a-z까지의 갯수 패턴 자체를 또하나의 key로 삼아
별도의 해시맵을 만들면 strs의 각 문자열을 해당 애너그램에 넣을 수 있을 것이다.

이러면 시간복잡도가 strs의 길이 M, strs 내부의 각 문자열의 평균 길이 N, 그리고 소문자 알파벳의 갯수 26까지 해서
O(26MN)이 될 것이다. 즉 O(MN)이 되는 것이다.

여기선 defaultdict()가 매우 유용하게 쓰인다. (실행환경에 따라 import defaultdict from collections 해야할수도)
어떤 키에 어떤 값을 해시맵에 넣어달라고 했는데 키가 없으면 에러 안내고 적어준대로 key와 텅빈 value를 만들어주는 defaultdict(). list를 넘기면 []로, int를 넘기면 0으로 초기화후 값을 삽입해준다.

각 문자열마다 알파벳 갯수패턴을 조사해서 그걸 key로한 해시맵에 넣어줘야하므로 첫 for문 안에서 매번 count를 26개의 0으로 된 리스트로 초기화해준다.
그리고 해당 문자열의 각 문자마다 아스키 코드 값을 하나씩 구해서 그에 해당하는 count 리스트의 인덱스의 값이 1씩 올라가도록 해준다.

그렇게 다 돌리고 나면, 그 패턴이 자리잡힌 count를 key로 하는 해시맵에 그 조사했던 문자열을 값으로서 넣어줘야한다.
이때 defaultdict(list)로 만든 배열에 해시맵을 넣으면 되는데, 이때 해시맵의 key는 immutable해야하므로, Python에서는 list로는 안되고
tuple로 변환을 해줘야한다.

이러고나면 아웃풋 해시맵의 값들만 따로 예쁘게 뽑아주는 values() 메서드를 호출하면 끝.