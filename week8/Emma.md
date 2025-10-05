# Problem242. Valid Anagram

## Solution

```commandline
    public boolean isAnagram(String s, String t) {

        if(s.length() != t.length()) return false;
      
        char[] arrs = s.toCharArray();
        char[] arrt = t.toCharArray();
       
        Arrays.sort(arrs);
        Arrays.sort(arrt);
        return Arrays.equals(arrs,arrt);
    }
```