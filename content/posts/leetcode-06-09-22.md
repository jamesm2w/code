+++
title = "Daily Leetcode 6th September 2022"
date = "2022-09-06T08:31:16+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 814. Binary Tree Pruning"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [814. Binary Tree Pruning](https://leetcode.com/problems/binary-tree-pruning/). The essential idea was to prune any subtrees of a binary tree which didn't include a node with `1`.

The approach is fairly simple, a DFS exploration of the tree and if any subtrees of a DFS return false we can prune that child (set it's value to `None`).

To determine whether a node has a subtree containing `1`: just have to check left subtree, right subtree and the value of the node. If either contain a 1 there's a subtree which contains a 1.

This approach is efficient and requires about `O(n)` where `n` is the number of nodes.

Finally, just have to cover the edge case of no subtrees satisfying in which case we just return an empty tree (`None`).

{{<code language="rust" title="Binary Tree Pruning">}}
impl Solution {
    
    // DFS -> true a if descendant of node is 1
    //     -> false if no descendant of node is 1 (no 1 in subtree and it should be pruned)
    pub fn dfs(root: &Option<Rc<RefCell<TreeNode>>>) -> bool {
        let value = if let Some(cell) = root {
            
            let left = Self::dfs(&cell.borrow().left);
            let right = Self::dfs(&cell.borrow().right);
            let mid = cell.borrow().val == 1;
            
            if !left {
                cell.borrow_mut().left = None;
            }
            
            if !right {
                cell.borrow_mut().right = None;
            }
            
            left || right || mid
        } else {
            false
        };
        
        //println!("{:?}\t1 in subtree {:?}", root, value);
        value
    }
    
    pub fn prune_tree(root: Option<Rc<RefCell<TreeNode>>>) -> Option<Rc<RefCell<TreeNode>>> {
        if !Self::dfs(&root) {
            None
        } else {
            root
        }
    }
}
{{</code>}}

Lets see what tomorrow brings.