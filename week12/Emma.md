# Problem. 118. Pascal's Triangle

Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

Example 1:

Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
Example 2:

Input: numRows = 1
Output: [[1]]

## Solution.
```
 public List<List<Integer>> generate(int numRows) {
        
        List<List<Integer>> ans = new ArrayList<>();
        
        ans.add(List.of(1));
        for(int i =0; i< numRows-1; i++){
            List<Integer> dum = new ArrayList<>();
            dum.add(0);
            dum.addAll(ans.get(ans.size()-1));
            dum.add(0);
            
            List<Integer> row = new ArrayList<>();
            for(int j = 0; j<dum.size()-1; j++){
                row.add(dum.get(j)+dum.get(j+1));
            }
            ans.add(row);
        }
        return ans;
    }
```

# Problem 929. Unique Email Addresses
Every valid email consists of a local name and a domain name, separated by the '@' sign. Besides lowercase letters, the email may contain one or more '.' or '+'.

For example, in "alice@leetcode.com", "alice" is the local name, and "leetcode.com" is the domain name.
If you add periods '.' between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name. Note that this rule does not apply to domain names.

For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.
If you add a plus '+' in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered. Note that this rule does not apply to domain names.

For example, "m.y+name@email.com" will be forwarded to "my@email.com".
It is possible to use both of these rules at the same time.

Given an array of strings emails where we send one email to each emails[i], return the number of different addresses that actually receive mails.

Example 1:

Input: emails = ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
Output: 2
Explanation: "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails.
Example 2:

Input: emails = ["a@leetcode.com","b@leetcode.com","c@leetcode.com"]
Output: 3

## Solution
```
public int numUniqueEmails(String[] emails) {
        
        HashSet<String > hash = new HashSet<>();
        for(String email : emails){
            int atIndex = email.indexOf("@");
            String local = email.substring(0,atIndex);
            String domain = email.substring(atIndex+1);

            int plusIdx = local.indexOf("+");
            if(plusIdx>0){
                local = local.substring(0,plusIdx);
            }
            local = local.replace(".","");
            local = domain+local;
            hash.add(local);
        }
        return hash.size();
    }
```