# Problem 14. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.
  
If there is no common prefix, return an empty string "".

Example 1:  
Input: strs = ["flower","flow","flight"]  
Output: "fl"  

Example 2:  
Input: strs = ["dog","racecar","car"]  
Output: ""  
Explanation: There is no common prefix among the input strings.
 
 
## Solution
```
 public String longestCommonPrefix(String[] strs) {


        Arrays.sort(strs);
        String a = strs[0];
        String b = strs[strs.length-1];
        String ans = "";
        int idx = 0;
        while(idx<a.length() && idx < b.length()){
            if(a.charAt(idx) == b.charAt(idx)){
                idx ++;
            }else{
                break;
            }
        }
        return a.substring(0,idx)   ;
    }
```

# problem 290. Word Pattern
Given a pattern and a string s, find if s follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in s. Specifically:

Each letter in pattern maps to exactly one unique word in s.
Each unique word in s maps to exactly one letter in pattern.
No two letters map to the same word, and no two words map to the same letter.  

Example 1:  
Input: pattern = "abba", s = "dog cat cat dog"  
Output: true

Explanation:  
The bijection can be established as:  
'a' maps to "dog".
'b' maps to "cat".  

Example 2:  
Input: pattern = "abba", s = "dog cat cat fish"  
Output: false  

Example 3:  
Input: pattern = "aaaa", s = "dog cat cat dog"  
Output: false

## Solution
```
HashMap<Character, String> ctos = new HashMap<>();
        HashMap<String, Character> stoc = new HashMap<>();
        String[] sarr = s.split(" ");
        // this is base case
        if(pattern.length() != sarr.length) return false;
        for(int i = 0; i< sarr.length; i++){
            String word = sarr[i];
            char pat = pattern.charAt(i);
            if(!ctos.containsKey(s.charAt(i))){
                ctos.put(pat,word);
            }
            if(!stoc.containsKey(sarr[i])){
                stoc.put(word, pat);
            }
            if(!stoc.get(word).equals(pat) || !ctos.get(pat).equals(word)) {
                return false;
            }
        }
        return true;
```

