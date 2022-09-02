+++
title = "Daily Leetcode 2nd September 2022"
date = "2022-09-02T10:19:01+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 637. Average of Levels in Binary Tree"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem is [637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/). Basically just get the average value of each level of a bintree. 

Fairly simple to perform traversal (here I do a DFS) keeping track of which level each node is on. Then when exploring each node take it's value and update the level's average with the new value.

To keep track of average value I also use a vec of the `size` which just keeps track of the number of nodes on the level so far. Then it can just recurse on the children. 

To start the solution just start the search from the root on level 0 and then transform the averages vec into what is required.

The size of the vec of average/size are auto-growing where the first node on that level pushes it's values at the end.

Solution is efficient and O(n) in number of vertices.

{{<code language="rust" title="Average of Levels in Binary Tree">}}
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    fn dfs(root: &Option<Rc<RefCell<TreeNode>>>, level: usize, averages: &mut Vec<f64>, sizes: &mut Vec<f64>) {
        if let Some(cell) = root {
            let val = cell.borrow().val as f64;

            if averages.len() == level {
                averages.push(val);
                sizes.push(1.0);
            } else {
                let avg = averages[level];
                let size = sizes[level];
                averages[level] = ((avg * size) + val) / (size + 1.0);
                sizes[level] = size + 1.0;
            }
            
            Self::dfs(&cell.borrow().left, level + 1, averages, sizes);
            Self::dfs(&cell.borrow().right, level + 1, averages, sizes);
        }
    }
    
    pub fn average_of_levels(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<f64> {
        let mut averages = Vec::new();
        let mut sizes = Vec::new();
        Self::dfs(&root, 0, &mut averages, &mut sizes);
        averages.into()
    }
}
{{</code>}}