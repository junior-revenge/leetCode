# Problem 205. Isomorphic Strings
Given two strings s and t, determine if they are isomorphic.

Two strings s and t are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

## Solution. 
> Use two hashmaps to record the index position of each character and compare them.
```
HashMap<Character, Integer> charIndexS = new HashMap<>();
        HashMap<Character, Integer> charIndexT = new HashMap<>();

        if(s.length()!= t.length()) return false;

        for(int i = 0; i<s.length(); i++) {
            if(!charIndexS.containsKey(s.charAt(i))){
                charIndexS.put(s.charAt(i), i);
            }
            if(!charIndexT.containsKey(t.charAt(i))){
                charIndexT.put(t.charAt(i),i);
            }

            if(!charIndexS.get(s.charAt(i)).equals(charIndexT.get(t.charAt(i)))){
                return false;
            }
        }
        return true;
```