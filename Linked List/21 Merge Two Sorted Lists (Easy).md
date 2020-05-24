# 21 Merge Two Sorted Lists (easy)

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example**

```cpp
Input: [1,2,4] [1,3,4]
Output: [1,1,2,3,4,4]
```



### 实现

```cpp
auto temp = new ListNode(0);
auto result = temp;
while (l1 || l2)
{
	if (l1 && l2)
    {
    	if (l1->val >= l2->val)
        {
            result->next = new ListNode(l2->val);
            result = result->next;
            l2 = l2->next;
        }
        else
        {
            result->next = new ListNode(l1->val);
            result = result->next;
            l1 = l1->next;
        }                
    }
    else if (l1)
    {
        result->next = new ListNode(l1->val);
        result = result->next;
        l1 = l1->next;
    }
    else
    {
        result->next = new ListNode(l2->val);
        result = result->next;
        l2 = l2->next;
    }
} 
result = temp->next;
delete temp;
return result;
```



### 心得

- 循环条件使用指针来判断
- 节点数值使用完毕后直接移动指针，因为一定存在，如果节点没有值，那么指针会是 `nullptr`
- 相当于二次排序

