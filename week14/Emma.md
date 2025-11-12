# Problem 367 Valid perfect Sqare
Given a positive integer num, return true if num is a perfect square or false otherwise.

A perfect square is an integer that is the square of an integer. In other words, it is the product of some integer with itself.

You must not use any built-in library function, such as sqrt.


Example 1:  
Input: num = 16  
Output: true  
Explanation: We return true because 4 * 4 = 16 and 4 is an integer. 

Example 2:  
Input: num = 14  
Output: false  
Explanation: We return false because 3.742 * 3.742 = 14 and 3.742 is not an integer.

## Solution Code
```
 public boolean isPerfectSquare(int num) {
        /*
        The question is asking whether given num is a perfect square or not
        But, I can't use any built-in library function.

        num 16 is perfect square.
        becauce 4*4 is 16. but How can I found this?

        first maybe I need to split? or how?
        using binery search? can I find?

        left = 0, right= 16, mid = 8
        8 * 8 = 64. right pointer move to left one less,
        left = 0, right = 7 mid = 3
        3 * 3 = 9, 9 vs 16 left pointer move to right
        left = 4, right = 7, mid = 5
        5*5 = 25 vs 8 is bigger. right pointer move to left one less than mid
        left = 4, right = 4 mid = 4
        4 * 4 = 16 is right.

        another example,14
        left = 1, right = 14 mid = 7
        7*7 = 49, right pointer move to left one less.
        left = 1, right = 6 mid = 3
        3*3 = 9, left pointer move to right one more than mid
        left = 4, right = 6 mid = 5
        5*5 = 25 vs 14. right pointer move to left one less.
        left = 4, right = 5 mid = 4
        4*4 = 16 vs 14. right pointer move to left one less than mid
        left = 5, right = 5 mid = 5
        5*5 = 25 it doesn't
         */

        int left = 0;
        int right = num;
        while(left <= right){
            long mid = left + (right-left)/2;
            long square = mid*mid;
            if(square>num){
                right = (int) mid-1;
            }else if(mid*mid < num){
                left = (int) mid +1;
            }else{
                return true;
            }
        }
        return false;
    }
```
## Approach
The question is asking whether given num is a perfect square or not
        But, I can't use any built-in library function.

        num 16 is perfect square.
        becauce 4*4 is 16. but How can I found this?

        first maybe I need to split? or how?
        using binery search? can I find?

        left = 0, right= 16, mid = 8
        8 * 8 = 64. right pointer move to left one less,
        left = 0, right = 7 mid = 3
        3 * 3 = 9, 9 vs 16 left pointer move to right
        left = 4, right = 7, mid = 5
        5*5 = 25 vs 8 is bigger. right pointer move to left one less than mid
        left = 4, right = 4 mid = 4
        4 * 4 = 16 is right.

        another example,14
        left = 1, right = 14 mid = 7
        7*7 = 49, right pointer move to left one less.
        left = 1, right = 6 mid = 3
        3*3 = 9, left pointer move to right one more than mid
        left = 4, right = 6 mid = 5
        5*5 = 25 vs 14. right pointer move to left one less.
        left = 4, right = 5 mid = 4
        4*4 = 16 vs 14. right pointer move to left one less than mid
        left = 5, right = 5 mid = 5
        5*5 = 25 it doesn't