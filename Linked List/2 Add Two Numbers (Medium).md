# Add Two Numbers (Medium)

### Description

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example**

```
Input: [2,4,3]
Output: [7,0,8]
Explanation: 342 + 465 = 807
```



### Code Snippets

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        // Answer
    }
}
```



**隐藏需求**

- 函数需要返回一个指向链表结构的指针
- 该指针指向的链表包含结果
- 指针应该位于链表的首位



### Debug Code

```cpp
void trimLeftTrailingSpaces(string &input) {
    input.erase(input.begin(), find_if(input.begin(), input.end(), [](int ch) {
        return !isspace(ch);
    }));
}

void trimRightTrailingSpaces(string &input) {
    input.erase(find_if(input.rbegin(), input.rend(), [](int ch) {
        return !isspace(ch);
    }).base(), input.end());
}

vector<int> stringToIntegerVector(string input) {
    vector<int> output;
    trimLeftTrailingSpaces(input);
    trimRightTrailingSpaces(input);
    input = input.substr(1, input.length() - 2);
    stringstream ss;
    ss.str(input);
    string item;
    char delim = ',';
    while (getline(ss, item, delim)) {
        output.push_back(stoi(item));
    }
    return output;
}

ListNode* stringToListNode(string input) {
    // Generate list from the input
    vector<int> list = stringToIntegerVector(input);

    // Now convert that list into linked list
    ListNode* dummyHead = new ListNode(0);
    ListNode* ptr = dummyHead;
    for(int item : list) {
        ptr->next = new ListNode(item);
        ptr = ptr->next;
    }
    ptr = dummyHead->next;
    delete dummyHead;
    return ptr;
}

string listNodeToString(ListNode* node) {
    if (node == nullptr) {
        return "[]";
    }

    string result;
    while (node) {
        result += to_string(node->val) + ", ";
        node = node->next;
    }
    return "[" + result.substr(0, result.length() - 2) + "]";
}

int main() {
    string line;
    while (getline(cin, line)) {
        ListNode* l1 = stringToListNode(line);
        getline(cin, line);
        ListNode* l2 = stringToListNode(line);
        
        ListNode* ret = Solution().addTwoNumbers(l1, l2);

        string out = listNodeToString(ret);
        cout << out << endl;
    }
    return 0;
}
```



### 代码分析

#### `stringToListNode()`

```cpp
ListNode* stringToListNode(string input) {
    // 首先建立向量
    vector<int> list = stringToIntegerVector(input);

    // 将向量转化为链表
    // =============
	// 创建指向一个临时动态指针，指向一个节点，该节点初始值为0
    ListNode* dummyHead = new ListNode(0);
    // 拷贝赋值
    ListNode* ptr = dummyHead;
    for(int item : list) {
        // 创建下一个节点
        ptr->next = new ListNode(item);
        // 移动到下一个节点
        ptr = ptr->next;
    }
    // 指向初始节点
    ptr = dummyHead->next;
    // 删除临时指针
    delete dummyHead;
    return ptr;
}
```



### 实现

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        char res = 0, carry = 0, i = l1->val, j = l2->val;
        bool movable1 = true, movable2 = true;
        ListNode* root = new ListNode(0);
        ListNode* ptr = root;
        // while 如果下一个节点指针不为空
        while (movable1 || movable2)
        {
            // 当前值相加
            res = i + j + carry;
            // 进位归零
            carry = 0;
            // 得到下个值。
            if (l1->next) {
                l1 = l1->next;
                i = l1->val;
            }
            else {
                i = 0;
                movable1 = 0;
            }
            if (l2->next) {
                l2 = l2->next;
                j = l2->val;
            }
            else {
                j = 0;
                movable2 = 0;
            }            
            // 若进位
            if (res > 9) {res -= 10; carry = 1; }
            // 创建一个指向动态链表首节点的指针，将结果赋予该节点val，移动指针
            ptr->next = new ListNode(res);
            ptr = ptr->next;
            
        }
        // 若最后只剩余进位，则直接移动
        if (carry) ptr->next = new ListNode(1);
        // 移动指针到首位
        ptr = root->next;
        delete root;
        return ptr;        
    }
```



### 其他答案

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* result;
    ListNode* dump = new ListNode((int)0);
    ListNode** sum = &result;
    int carry = 0;
    int now = 0;
    while (l1 != dump || l2 != dump || carry != 0)
    {
        now = (l1->val + l2->val + carry) % 10;
        carry = (l1->val + l2->val + carry) / 10;
        *sum = new ListNode(now);
        sum = &(*sum)->next;
        if (l2->next == NULL)
            l2 = dump;
        else l2 = l2->next;
        if (l1->next == NULL)
            l1 = dump;
        else l1 = l1->next;
    }
    delete dump;
    return result;
}
```



#### 关键代码分析

```cpp
*sum = new ListNode(now);
sum = &(*sum)->next;
```

- 第一次循环时 `sum` 指向 `result` ，此时赋值则为 `result` 创建动态节点
- 随后指向 `result` 的后续节点，帮助链表进行扩展
- 自第一次赋值以来，`result` 始终指向首位
- 活用 `dump` 在多处进行比较

