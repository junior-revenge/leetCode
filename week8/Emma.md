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

# problem 1603 Design Parking System.
Design a parking system for a parking lot. The parking lot has three kinds of parking spaces: big, medium, and small, with a fixed number of slots for each size.

Implement the ParkingSystem class:

ParkingSystem(int big, int medium, int small) Initializes object of the ParkingSystem class. The number of slots for each parking space are given as part of the constructor.
bool addCar(int carType) Checks whether there is a parking space of carType for the car that wants to get into the parking lot. carType can be of three kinds: big, medium, or small, which are represented by 1, 2, and 3 respectively. A car can only park in a parking space of its carType. If there is no space available, return false, else park the car in that size space and return true.
 
 ### Solution
 To solve this problem, declare array and initialize array in constructor. and addCar method reduce size of array.
```
public class ParkingSystem {
    /*
    The questin is asking design a parking system for a parking lot.
    there are three type of car type, and each type initialize with constructor
    and make addCar method to check whether there is parking lot or not

     */
    private int[] size ;
    public ParkingSystem(int big, int medium, int small) {
        size = new int[]{big, medium, small};
    }

    public boolean addCar(int carType) {
        return size[carType-1] -- > 0;
    }
}

```

# Problem 118 Pascal's Triangle
Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

### Solution
Every first row is 1. so first set the first row with one. and iterate numRow-1 times. Making dummy row with list to calculate easily. fill first and last element with 0 since every first and last elements is one respectivly.
Iterate dummy row to add adjacent number.


```
 public List<List<Integer>> generate(int numRows) {
        // declare List for answer;
        List<List<Integer>> res = new ArrayList<>();
        // the first row is always one;
        res.add(List.of(1));
        // iterate one less than numRows times since first row is one;
        for(int i = 0 ; i < numRows-1 ; i++){
            // declare dummy list for setting easily
            List<Integer> dummy = new ArrayList<>();
            // the size of dummy always 2 bigger than prev row
            // so add 0 at first and last elements, and duplicate res.
            dummy.add(0);
            // res size -1 means last index of res so that means prev row since index start with 0
            dummy.addAll(res.get(res.size()-1));
            dummy.add(0);

            // iterate dummy row to add adjacent numbers
            List<Integer> row = new ArrayList<>();
            for(int j = 0; j< dummy.size()-1; j++){
                row.add(dummy.get(j) + dummy.get(j+1) );
            }
            res.add(row);
        }
        return res;

    }
```