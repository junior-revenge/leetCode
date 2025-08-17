```
public class ParkingSystem {
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

```
  public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> set2 = new HashSet<>();
        for(int num : nums1) set1.add(num);
        for(int num: nums2) set2.add(num);

        List<List<Integer>> answer = new ArrayList<>();
        answer.add(new ArrayList<>());
        answer.add(new ArrayList<>());

        for(int num : set1){
            if(!set2.contains(num)) answer.get(0).add(num);
        }
        for(int num:set2) {
            if(!set1.contains(num)) answer.get(1).add(num);
        }
        return answer;
    }
```

```
 public String longestCommonPrefix(String[] strs) {
        Arrays.sort(strs);
        String first = strs[0];
        String last = strs[strs.length-1];
        int c= 0;
        while(c<first.length()&& c<last.length()){
            if(first.charAt(c) == last.charAt(c)) c++;
            else break;
        }
        return first.substring(0,c);
    }
```