---
title: leetcode 314
date: "2022-07-02T21:28:37.121Z"
template: "post"
draft: false
slug: "leetcode"
category: "leetcode"
tags:
  - "BFS"
  - "Hashmap"
description: ""
socialImage: "/media/river.jpg"
---

# Leetcode 314 - Binary Tree Vertical Order Traversal
- Given a binary tree, return the vertical order traversal of its node's value.
## Intuition
- We can use BFS to traversal the tree, then record the colum index in map's key.

- The map template can sort the key from smaller to bigger, instead of using unordered_map templae.

```c
class Solution {
public:
        vector<vector<int>> res;
         if (!root) return res;
        queue<pair <int ,TreeNode*>> q;
        map<int, vector<int>> m;
        q.push ({0, root});
        while (!q.empty()) {
            auto tmp = q.front();
            q.pop();
            cout << tmp.first << "   ";
            m[tmp.first].push_back(tmp.second->val);
            if (tmp.second->left) {
                q.push({tmp.first - 1, tmp.second->left});
            }
            if (tmp.second->right) {
                q.push({tmp.first + 1, tmp.second->right});
            }
            cout << "--- " << endl;
        }
        for (auto x: m) {
            res.push_back(x.second);
        }
        return res;
    }
};
```
