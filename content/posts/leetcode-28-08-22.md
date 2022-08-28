+++
title = "Daily Leetcode 28th August 2022"
date = "2022-08-28T10:34:28+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 1329. Sort the Matrix Diagonally"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [1329. Sort the Matrix Diagonally](https://leetcode.com/problems/sort-the-matrix-diagonally/). The title is pretty self-explanatory, given a matrix return the same but sort the diagonals. 

My solution leveraged rust's iterators heavily and was a pretty basic approach. For each start of a diagonal (all cells on the top row and left column) put the cells into a binary heap to sort then put them back in their sorted configuration.

It starts but using zip to get all coords on the diagonal, and then maps them to the matrix value, then collects that into a sorted vector and then putting that into another iterator zipped with the diagonal co-ordinates to place them back in the matrix.

There's probably some optimisation to be done with re-using the same zips and iterators, maybe only requiring two `n*m` iterations through the mat instead of the like `n*min(m, n)` etc abomination I use.

{{<code language="rust" title="Sort the Matrix Diagonally">}}
use std::collections::BinaryHeap;

impl Solution {
    pub fn diagonal_sort(mat: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let m = mat[0].len();
        let n = mat.len();
        let mut mat = mat;
        
        for i in 0..m {
            (i..m).zip(0..n)
                .map(|(x, y)| mat[y][x])
                .collect::<BinaryHeap<_>>()
                .into_sorted_vec()
                .into_iter()
                .zip((i..m).zip(0..n))
                .for_each(|(sorted, (x, y))| mat[y][x] = sorted);
        }

        for j in 1..n {         
            (0..m).zip(j..n)
                .map(|(x, y)| mat[y][x])
                .collect::<BinaryHeap<_>>()
                .into_sorted_vec()
                .into_iter()
                .zip((0..m).zip(j..n))
                .for_each(|(sorted, (x, y))| mat[y][x] = sorted);
        }
        
        mat
    }
}
{{</code>}}

This ran fairly fast, and apparently beat 100% of other Rust submissions, which I doubt, but good to know it's at least fairly efficient. 

Let's see what tomorrow brings.