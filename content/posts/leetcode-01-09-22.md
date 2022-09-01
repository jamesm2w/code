+++
title = "Daily Leetcode 1st September 2022"
date = "2022-09-01T10:43:18+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 1448. Count Good Nodes in Binary Tree"
showFullContent = false
readingTime = true
hideComments = false
+++

A new month! Today's problem was [1448. Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/submissions/). Essentially to count the number of nodes for which the path from root to node had no inbetween nodes >= the dest node.

The can be simply solved using a Depth-First-Search on the binary tree. And keeping track of the max value so far in each call. Then if the value on the node is >= than the max so far, we know that the current node is valid and then pass on the new max value to the child nodes.

As an implementation detail there are some issues with rust ownership and references. It's mostly legible but there's some overhead in getting these references, compared to other languages.

{{<code language="rust" title="Count Good Nodes in Binary Tree">}}
impl Solution {
    pub fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, max_on_path: i32) -> i32 {
        if let Some(cell) = root {
            
            let val = cell.borrow().val;
            let left = &cell.borrow().left;
            let right = &cell.borrow().right;
            
            if val >= max_on_path { // This node is good + recuse
                
                1 + Self::dfs(left, val) + Self::dfs(right, val)
            } else { // This node is bad + recurse
                
                Self::dfs(left, max_on_path) + Self::dfs(right, max_on_path)
            }
        } else { // None -> 0
            
            0
        }
    }
    
    pub fn good_nodes(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let val = root.as_ref().unwrap().borrow().val;
        Self::dfs(&root, val)
    }
}
{{</code>}}