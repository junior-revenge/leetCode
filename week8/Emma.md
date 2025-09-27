# Problem 448 FindAllNumbersDisappearedInAnArray

## Brute Force

```commandline
    HashSet<Integer> set = new HashSet<>();
    for(int n : nums){
        set.add(n);
    }
    List<Integer> list = new ArrayList<>();
    for(int i = 1; i<= nums.length; i++){
        if(!set.contains(i)) list.add(i);
    }
    return list;
```

## Solution.
```commandline
    int index = -1;
    for(int i = 0; i< nums.length;i ++){
        index = Math.abs(nums[i])-1;

        if(nums[index]>0) nums[index] *=-1;
    }
    List<Integer> li = new ArrayList<>();
    for (int i = 0; i<nums.length; i++){
        if(nums[i]>0) li.add(i+1);
    }
    return li;
```