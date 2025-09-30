# 482. License Key Formatting

You are given a license key represented as a string s that consists of only alphanumeric characters and dashes. The string is separated into n + 1 groups by n dashes. You are also given an integer k.

We want to reformat the string s such that each group contains exactly k characters, except for the first group, which could be shorter than k but still must contain at least one character. Furthermore, there must be a dash inserted between two groups, and you should convert all lowercase letters to uppercase.

Return the reformatted license key.

Example 1:

Input: s = "5F3Z-2e-9-w", k = 4
Output: "5F3Z-2E9W"
Explanation: The string s has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.

Example 2:

Input: s = "2-5g-3-J", k = 2
Output: "2-5G-3J"
Explanation: The string s has been split into three parts, each part has 2 characters except the first part as it could be shorter as mentioned above.

## Solution

```cpp
string licenseKeyFormatting(string s, int k) {
    auto buf = stringstream(s);
    auto tmp = string();
    auto v = vector<string>();

    auto bud = stringstream();

    while(getline(buf, tmp, '-')) {
        bud << tmp;
    }

    bud.seekg(0, ios::end);
    int size = bud.tellg();

    auto head = (size % k) == 0 ? k : size % k;
    auto ans = stringstream();
    auto str = bud.str();

    auto it = str.substr(0, head);
    std::transform(it.begin(), it.end(), it.begin(), ::toupper);
    ans << it;

    for(auto i=head;i<size;i += k) {
        auto s = str.substr(i,k);
        std::transform(s.begin(), s.end(), s.begin(), ::toupper);
        ans <<"-"<<s;
    }

    return ans.str();
}
```

# 32. Longest Valid Parentheses

Given a string containing just the characters '(' and ')', return the length of the longest valid (well-formed) parentheses.

Example 1:

Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".

Example 2:

Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".

Example 3:

Input: s = ""
Output: 0

## Solution

### Korean

보통 문제를 해결하고자 할 때엔 브루트 포스 접근법부터 떠올리지만, 이 문제는 그런 접근법이 더 힘들어보인다. 일단 문제의 조건은 유효한 괄호 쌍 중에서 제일 길이가 긴 것을 찾는 것인데, 일단 스택을 이용한 방법을 떠올릴 수가 있다. 왜냐하면 기본적으로 괄호의 여는 괄호와 닫는 괄호가 숫자가 동일해야 하며, 먼저 여는 괄호가 나타나고 그 다음 닫는 괄호가 나타나야 유효한 괄호 쌍이 되기 때문이다.

그래서 여는 괄호가 나타나면 스택에 쌓고, 닫는 괄호가 나타나면 팝하고 터뜨리는 것이다. 하지만 단순히 유효성 체크라면 괄호를 스택에 쌓고, 팝하는 접근법이 괜찮을지도 모르겠다. 하지만 우리는 유효한 괄호의 최대 길이를 구해야한다. 여기서 각 괄호의 위치를 저장해보는 것이 어떨까하고 떠올려 볼 수 있다.

그렇다면 스택과 위치값을 어떻게 다루어야 유효한 답을 도출 해낼 수가 있을까? 일단, 여는 괄호를 쌓고 닫는 괄호를 만나면 팝을 한다는 점을 생각해보자. 여기서 여는 괄호가 존재하는 위치를 스택에 넣어야 한다는 것을 자연스럽게 떠올릴 수가 있다. 그러면 단순히 닫는 괄호가 나타났을 때에 이전 괄호의 위치를 스택에서 꺼내고, 그 차이만큼 구하면 될까?

이런 아이디어는 대부분의 경우에는 잘 작동 할 것이다. 하지만 몇 몇 엣지 케이스들에 대해서도 잘 동작하지는 알 수가 없다. 다음 예시를 살펴보도록 하자.

 (())

처음에 0, 1 이렇게 스택에 저장 될 것이다. 그 다음 1 을 스택에서 팝하고, 2 - 0 을 하여서 유효한 길이 2 를 구할 수가 있다. 그 다음 0 을 팝하고 나면 유효한 길이 4를 얻어야 하는데 스택에 아무것도 없다! 처음부터 난관이다. 이 경우엔 스택에 시작 지점을 넣는 것으로 해결 할 수 있다. 우리가 탐색을 하기전에 스택에 미리 -1 을 넣어보자.

그러면 3 - (-1) 로 우리가 원하는 값 4 를 얻을 수가 있다. 그러면 다음 경우엔 어떻게 할 것인가?

()))()()

한눈에 봐도 값이 4가 나와야 함은 어렵지 않게 알 수 있다. 하지만 어떻게? 먼저, 기존에 제안한 방법으로 2를 얻을 수 있을 것이다. 그러면 그 다음에 4를 어떻게 얻을 것인가가 문제다. 먼저, 괄호가 한 짝을 이룰 때에 이전의 경계 값을 통해 길이를 구한다는 점을 생각해보자.

여는 괄호의 경우엔 단순히 쌓아두기만 해도 충분하다. 그렇다면 닫는 괄호는? 기본적으로 닫는 괄호를 처리 할 때에 스택의 맨 위에 있는 값을 꺼내 팝을 한다는 것을 생각해보자. 그래서 모든 괄호 쌍을 만난 다음 닫는 괄호가 나올 경우 스택은 비어있을 것이다. 이 경우엔 유효한 괄호 쌍이 아니기 때문에 닫는 괄호의 위치를 스택에 저장하도록 하자.

그리고 계속해서 닫는 괄호가 나타날 경우, 해당 괄호의 위치를 기반으로 하여 마지막 경계값이 갱신 될 것이다. 다시 위의 예시로 돌아가보자.

()))()()

0, 1 까지는 문제 없이 진행이 될 것이다. 그리고 2, 3 여기서부터 닫는 괄호들이 나타나는데, 매번 스택에서 이전 괄호를 팝하고 새로운 괄호로 갱신을 한다. 그리고 4, 5 번은 유효한 괄호 쌍이기 때문에 5 - 3 으로 2를 얻을 수 있다. 마찬가지로 6, 7 도 7 - 3 으로 우리가 원하는 정답 4를 구할 수가 있다.

### English

Usually when trying to solve a problem, we start by thinking of a brute force approach, but for this problem, that approach seems more difficult. The problem's condition is to find the longest valid parentheses pair. First, we can think of a method using a stack. This is because fundamentally, the number of opening and closing parentheses must be equal, and the opening parenthesis must appear first, followed by the closing parenthesis, to form a valid parentheses pair.

So when an opening parenthesis appears, we push it onto the stack, and when a closing parenthesis appears, we pop it off. However, if we're simply checking validity, the approach of pushing and popping parentheses onto the stack might be sufficient. But we need to find the maximum length of valid parentheses. Here, we can think about storing the position of each parenthesis.

Then how should we handle the stack and position values to derive a valid answer? First, let's think about the fact that we push opening parentheses and pop when we encounter a closing parenthesis. Here, we can naturally think of putting the position where the opening parenthesis exists into the stack. Then, when a closing parenthesis appears, should we simply pop the position of the previous parenthesis from the stack and calculate the difference?

This idea will work well in most cases. However, we can't be sure if it will work well for some edge cases. Let's look at the following example:

(())

Initially, 0 and 1 will be stored in the stack. Then we pop 1 from the stack and calculate 2 - 0 to get a valid length of 2. Then after popping 0, we should get a valid length of 4, but there's nothing in the stack! We're in trouble from the start. In this case, we can solve it by putting a starting point in the stack. Let's put -1 in the stack before we start our search.
Then with 3 - (-1), we can get the value 4 that we want. Then what do we do in the next case?

()))()()

At a glance, it's not difficult to see that the value should be 4. But how? First, we can get 2 using the previously proposed method. Then the problem is how to get 4 next. First, let's think about the fact that when parentheses form a pair, we calculate the length through the previous boundary value.

For opening parentheses, it's sufficient to just stack them. Then what about closing parentheses? Basically, let's think that when processing a closing parenthesis, we pop by taking out the value at the top of the stack. So if a closing parenthesis appears after meeting all parentheses pairs, the stack will be empty. In this case, since it's not a valid parentheses pair, let's store the position of the closing parenthesis in the stack.

And if closing parentheses continue to appear, the last boundary value will be updated based on that parenthesis's position. Let's go back to the example above.

()))()()

Up to 0 and 1, it will proceed without problems. And from 2 and 3 onward, closing parentheses appear, and each time we pop the previous parenthesis from the stack and update with the new parenthesis. And since 4 and 5 are valid parentheses pairs, we can get 2 from 5 - 3. Similarly, for 6 and 7, we can get the answer 4 that we want from 7 - 3.

### Code

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> st;
        st.push(-1);
        
        int maxLength = 0;
        
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(') {
                st.push(i);
            } else {
                st.pop();
                if (st.empty()) {
                    st.push(i);
                } else {
                    maxLength = max(maxLength, i - st.top());
                }
            }
        }
        
        return maxLength;
    }
};
```

# 35. Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [1,3,5,6], target = 5
Output: 2

Example 2:

Input: nums = [1,3,5,6], target = 2
Output: 1

Example 3:

Input: nums = [1,3,5,6], target = 7
Output: 4

## Solution

### Code 

```cpp
int searchInsert(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    int mid = -1;

    while(left <= right) {
        mid = (left + right) / 2;

        if(nums[mid] == target) {
            return mid;
        }
        else if(nums[mid] > target) {
            right = mid - 1;
        }
        else {
            left = mid + 1;
        }
    }

    return nums[mid] > target ? mid : mid + 1;
}
```