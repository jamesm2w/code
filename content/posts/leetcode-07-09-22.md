+++
title = "Daily Leetcode 7th September 2022"
date = "2022-09-07T10:55:09+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 606. Construct String from Binary Tree"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [606. Construct String from Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree/). The basic idea is to use a pre-order traversal to generate a representation of a bin-tree using `(` and `)`. 

The basic idea is a DFS based pre-order traversal which outputs the value, followed by the left and right child. The twist is that you have to check if the nodes are null and return an empty `()` we can omit it. Similarly if there's a child `()` which is not necessary we can delete it. 

I commented a little table which shows in what combination of scenarios for which output string. I used `format!` macro to quickly create the strings, although to improve performance I could have used more mutuable concatenation with string slices `&str` instead.

{{<code language="rust" title="Construct String from Binary Tree">}}
impl Solution {
    pub fn post_order(root: &Option<Rc<RefCell<TreeNode>>>) -> String {
        match root {
            None => { String::from("()") },
            Some(cell) => {
                let val = cell.borrow().val;
                let mut output = format!("{}", val);
                
                // l\ r |  Some | None
                //------|-------|------
                // Some | (l)(r)| (l)
                // None |  ()(r)| ___
                
                let left = Self::post_order(&cell.borrow().left);
                let right = Self::post_order(&cell.borrow().right);
                
                match cell.borrow().left {
                    None => match cell.borrow().right {
                        None => {},
                        Some(_) => {
                            output.push_str(format!("()({})", right).as_str());
                        }
                    }
                    Some(_) => match cell.borrow().right {
                        None => output.push_str(format!("({})", left).as_str()),
                        Some(_) => {
                            output.push_str(format!("({})({})", left, right).as_str());
                        }
                    }
                };
                
                output
            }
        }
    }
    
    pub fn tree2str(root: Option<Rc<RefCell<TreeNode>>>) -> String {
        Self::post_order(&root)
    }
}
{{</code>}}
