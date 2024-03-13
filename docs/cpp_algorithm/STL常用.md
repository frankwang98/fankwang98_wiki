## 数组

## 栈和队列

### 基本计算器（表达式求值）

```cpp
class Solution {
public:
    int calculate(string s) {
        // 用栈stack这种类型 只有加减的表达式
        stack<int> ops;
        ops.push(1);
        int sign = 1;

        int ret = 0;
        int n = s.length();
        int i = 0;
        while (i < n) {
            if (s[i] == ' ') {
                i++;
            } else if (s[i] == '+') {
                sign = ops.top();
                i++;
            } else if (s[i] == '-') {
                sign = -ops.top();
                i++;
            } else if (s[i] == '(') {
                ops.push(sign);
                i++;
            } else if (s[i] == ')') {
                ops.pop();
                i++;
            } else {
                long num = 0;
                while (i < n && s[i] >= '0' && s[i] <= '9') {
                    num = num * 10 + s[i] - '0';
                    i++;
                }
                ret += sign * num;
            }
        }
        return ret;
    }
};
```

## 单调栈

## 优先队列

## 双端队列

### 滑动窗口最大值
```cpp
#include <iostream>
#include <vector>
#include <deque>

using namespace std;

vector<int> slidingWindowMax(const vector<int>& nums, int k) {
    vector<int> result;
    deque<int> dq;

    for (int i = 0; i < nums.size(); ++i) {
        // 在队尾保持递减顺序
        while (!dq.empty() && nums[i] >= nums[dq.back()]) {
            dq.pop_back();
        }
        dq.push_back(i);

        // 如果队头元素已不在窗口范围内，弹出
        if (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }

        // 将窗口内的最大值加入结果
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }

    return result;
}

int main() {
    vector<int> nums = {1, 3, -1, -3, 5, 3, 6, 7};
    int k = 3;
    
    vector<int> result = slidingWindowMax(nums, k);

    cout << "滑动窗口最大值：";
    for (int num : result) {
        cout << num << " ";
    }
    cout << endl;

    return 0;
}
```

## 哈希表

## 多重集合和映射