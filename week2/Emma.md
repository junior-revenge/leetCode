# Problem 929. Unique Email Addresses
Every valid email consists of a local name and a domain name, separated by the '@' sign. Besides lowercase letters, the email may contain one or more '.' or '+'.
For example, in "alice@leetcode.com", "alice" is the local name, and "leetcode.com" is the domain name.
If you add periods '.' between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name. Note that this rule does not apply to domain names.

For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.
If you add a plus '+' in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered. Note that this rule does not apply to domain names.

For example, "m.y+name@email.com" will be forwarded to "my@email.com".
It is possible to use both of these rules at the same time.

Given an array of strings emails where we send one email to each emails[i], return the number of different addresses that actually receive mails.

## Solution
```
  public int numUniqueEmails(String[] emails) {
       Set<String> set = new HashSet<>();
       for(String email:emails){
           int atIdx = email.indexOf("@");
           int plusIdx = email.indexOf("+");
           String localName = "";
           if(plusIdx>=0) localName = email.substring(0,plusIdx);
           else localName = email.substring(0,atIdx);
           localName = localName.replace(".","")+email.substring(atIdx);
           set.add(localName);
       }
       return set.size();
    }
```

# Problem 27. Remove Element
Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.

## Solution
```
 public int removeElement(int[] nums, int val) {
      int k = 0;
        for(int i = 0; i<nums.length; i++){
            if(nums[i] != val){
                nums[k] = nums[i];
                k++;
            }
        }
        return k;
    }
```

# Problem 1603 Design Parking System
Design a parking system for a parking lot. The parking lot has three kinds of parking spaces: big, medium, and small, with a fixed number of slots for each size.

Implement the ParkingSystem class:

ParkingSystem(int big, int medium, int small) Initializes object of the ParkingSystem class. The number of slots for each parking space are given as part of the constructor.
bool addCar(int carType) Checks whether there is a parking space of carType for the car that wants to get into the parking lot. carType can be of three kinds: big, medium, or small, which are represented by 1, 2, and 3 respectively. A car can only park in a parking space of its carType. If there is no space available, return false, else park the car in that size space and return true.

## Solution
```
class ParkingSystem {
 private int[] size;
    public ParkingSystem(int big, int medium, int small) {
        this.size = new int[]{big, medium, small};
    }

    public boolean addCar(int carType) {
        if(size[carType-1]>0){
            size[carType-1] --;
            return true;
        }
        return false;
    }
}

```