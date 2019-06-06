
### 1. balanced parenthesis
  - LC 678
  - scenario: we have ( and ) and * to represent any character(l,r,'')
  - we use two counters to finish this problem in one pass, high means we regards all * to be ), while low to be (
  ```
  public boolean checkValidString(String s) {
        int low = 0, high = 0;
        for(char c: s.toCharArray()) {
            if(c == '(') {
                low++;
                high++;
            }else if(c == ')') {
                low--;
                high--;
            }else{
                low--;
                high++;
            }
            
            if(high < 0) return false;
            low = Math.max(0, low);
        }
        return low == 0;
    }
  ```


### 2. remove invalid parenthesis
- LC 301
- scenario: we will be given an invalid parenthesis. We need to remove least parenthesis to make it valid
- we first find out the number of lft and rgt parenthesis need to be removed and then run a dfs to create valid one
```
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        //right parenthesis cannot appear before left
        int left = 0, right = 0;
        for(int i = 0;i< s.length();i++) {
            char cur = s.charAt(i);
            if(cur == '(') left++;
            else if(cur == ')') {
                if(left > 0) left--;
                else right++;
            }
        }
        Set<String> ret = new HashSet<>();
        buildAns(ret, s, new StringBuilder(), 0, left, right, 0);
        return new ArrayList<>(ret);
    }
    public void buildAns(Set<String> ret, String s, StringBuilder sb, int i, int left, int right, int open) {
        if(left < 0 || right < 0 || open < 0) return;
        
        if(i == s.length()) {
            if(left == 0 && right == 0 && open == 0) {
                ret.add(sb.toString());
            }
            return;
        }

        char cur = s.charAt(i);
        int len = sb.length();
        if(cur == '(') {
            buildAns(ret, s, sb, i+1, left-1, right,open);
            buildAns(ret, s, sb.append(cur), i+1, left, right, open+1);
        }else if(cur == ')') {
            buildAns(ret, s, sb, i+1, left, right-1,open);
            buildAns(ret, s, sb.append(cur),i+1, left, right, open-1);            
        }else{
            buildAns(ret, s, sb.append(cur), i+1, left, right,open);
        }
        sb.setLength(len);
        return;
    }
}
```


### 3. min stack optimal solution
- trivially we can store tuple into stack, with each element contains value itself and the current global minimal value.
- However, we can actually store global minimum only when we need to. Whenever we encountered a new global minimum, we can push previous minimum and update it, so that when we are popping elements out, we can reset previous minimumif we already reach the current global minimum
```
class MinStack {

    /** initialize your data structure here. */
    Stack<Integer> stack;
    int min = Integer.MAX_VALUE;
    public MinStack() {
        stack = new Stack();
    }
    
    public void push(int x) {
        if(x <= min) {
            stack.push(min);
            min = x;
        }
        stack.push(x);
    }
    
    public void pop() {
        if(stack.pop() == min) min = stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return min;
    }
}
```

### 4. increasing stack
LC1063 & QUORA interview
- we only need to track the items that are increasing in some case.
