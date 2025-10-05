# Problem242. Valid Anagram

## Solution

```commandline
    public boolean isAnagram(String s, String t) {
        // using Arrays's sort. because anagram is a word made by using the letter in different order.
        // so we can check this ascending order.
        // 0. before we can check with length.
        if(s.length() != t.length()) return false;
        // 1. make an array
        char[] arrs = s.toCharArray();
        char[] arrt = t.toCharArray();
        // 2. sort an array;
        Arrays.sort(arrs);
        Arrays.sort(arrt);
        // return boolean value it is equal or not
        return Arrays.equals(arrs,arrt);
    }
```