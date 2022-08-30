+++
title = "Daily Leetcode 30th August 2022"
date = "2022-08-30T09:54:20+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 48. Rotate Image"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [48. Rotate Image](https://leetcode.com/problems/rotate-image/). The basic idea is to rotate an `nxn` square matrix 90 degrees clockwise. The trick is to do it in-place without allocating another `n^2` vec.

Probably multiple approaches to this, including following the cycles of indices replacing in turn, but I decided to instead perform a transpose then reverse each row. Which achievs the same effect.

Transposing is a simple operation just replace `M[i, j]` with `M[j, i]` in one diagonal half of the matrix. I did it using a temporary variable but it could also be achieved without using the `x+y` then `x-y` trick. 

Reversing vec's in-place with rust is simple and the std library provides the reverse function. Putting it together is a succint in-place solution which ran in about 1ms. 

{{<code language="rust" title="Rotate Image">}}
impl Solution {
    pub fn rotate(matrix: &mut Vec<Vec<i32>>) {
        let n = matrix.len();
        let mut temp;
        
        // Transpose matrix
        for i in 0..n-1 {
            for j in i+1..n {
                temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        
        // Reverse rows
        for i in 0..n {
            matrix[i].reverse();
        }
        
    }
}
{{</code>}}

Quite satisfied with the solution today. Let's see what tomorrow brings.