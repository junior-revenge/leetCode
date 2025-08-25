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

# Problem 169. Majority Element
Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

 ## Solution
 We need to store how many times each number appears in the array. The best data structure is Hashmap. So first declare a hashmap, along with an integer variable `major` to store the current majority frequency, and another integer vartiable `majorNum` to store the number that correspondes to the majority. As we traverse the array, we put each number into hashmap with the number as the key, and its frequency as the value. Since key can not be duplicated, if we put same key, we increase its frequency by 1 using getOrDefault method (_this was the part I didn't know before).  
 While adding value to the hashMap, we compare the frequency of current number with the current majority(`major`). If current value is greater than `major`, we update `major` to the frequency of current number and set `majorNum` to the current number.
 ```
  public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> hash = new HashMap<>();
        int major = 0;
        int majorNum = 0;
        
        for(int n : nums ){
            hash.put(n, hash.getOrDefault(n,0)+1);
            if(hash.get(n)>major) {
                major = hash.get(n);
                majorNum = n;
            }
        }
        return majorNum;
    }
 ```