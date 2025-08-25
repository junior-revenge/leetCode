# Problem 205. Isomorphic Strings
Given two strings s and t, determine if they are isomorphic.

Two strings s and t are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

## Solution. 
> Use two hashmaps to record the index position of each character and compare them.
```
public boolean isIsomorphic(String s, String t) {
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
    }
```

# Problem 605. Can Place Flowers
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return true if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule and false otherwise.

##  Solution
We check whether the left and right plots are 0(unplanted) or 1(planted). At index 0 or at the last index, the left or right side is out of bounds, so we can treat it as unplated.
If both the left and right plots are 0, we set current index to 1 and decrease n by 1.
If n is less than 0, it means we have already planted at least the required number of flowers, so we return true.

```
 public boolean canPlaceFlowers(int[] flowerbed, int n) {
        for(int i = 0; i< flowerbed.length; i++){
            boolean left = i ==0 || flowerbed[i-1] ==0;
            boolean right = i == flowerbed.length-1 || flowerbed[i+1] == 0;
            
            if(left && right && flowerbed[i] ==0 ){
                flowerbed[i] =1;
                n --;
            }
        }
            return n <= 0;
    }
```