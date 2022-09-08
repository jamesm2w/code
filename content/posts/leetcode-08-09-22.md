+++
title = "Daily Leetcode 8th September 2022"
date = "2022-09-08T11:02:01+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 94. Binary Tree Inorder Traversal"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/). Which is pretty trivial graph traversal. In-order traversal is simply exploring the left child, appending the value to the output and then exploring the right child.

Simple to translate into Rust with a mutuable vec reference for the output.

{{<code language="rust" title="Binary Tree Inorder Traversal">}}
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, output: &mut Vec<i32>) {
        if let Some(cell) = root {
            Self::dfs(&cell.borrow().left, output);
            output.push(cell.borrow().val);
            Self::dfs(&cell.borrow().right, output);
        }
    }
    
    pub fn inorder_traversal(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut output = Vec::new();
        Self::dfs(&root, &mut output);
        output
    }
}
{{</code>}}

Obviously this ran very fast and I'm not sure there's a better way to do it conceptually, although maybe there are some optimisations which could be made to a rust solution.

Lets see what tomorrow brings.