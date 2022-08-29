+++
title = "Daily Leetcode 29th August 2022"
date = "2022-08-29T10:46:03+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 200. Number of Islands"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [200. Number of Islands](https://leetcode.com/problems/number-of-islands/). Given a 2D vector representing a map of some ocean and land return the number of islands (connected areas of land with ocean surrounding them).

The basic approach is to perform basic graph search algorithm e.g. depth-first-search from each bit of land to explore all connected pieces of land. This gets you a whole island, then you can move on looking for more bits of land until all bits have been explored and return the number of components.

This is practically achieved through a simple 2D loop through all grid indices, and when you encounter some land which has not been explored before, start a DFS/BFS (I used DFS) search to explore all adjacent land, increment the number of components, and move on.

Implementing DFS in rust was pretty simple, just got to check that the indicies are valid for the grid, then can insert the coords into some set or other structure which keeps track of which cells have been visited then recurse to the adjacent cells.

This solution was pretty efficient, although there might be optimisations using slices or a vec instead of a hashset which might be overkill computing hashes of points which would be in a specific range. Furthermore, the `get_cell` method is a bit verbose I'd like to simplify it but I'm not entirely sure how.

{{<code language="rust" title="Number of Islands">}}
impl Solution {
    pub fn get_cell(cell: (isize, isize), grid: &Vec<Vec<char>>) -> char {
        let (x, y) = cell;
        if x < 0 || x >= grid[0].len() as _ || y < 0 || y >= grid.len() as _ {
            return '0';
        }
        
        grid[y as usize][x as usize]
    }
    
    pub fn dfs(cell: (isize, isize), grid: &Vec<Vec<char>>, visited: &mut HashSet<(isize, isize)>) {
        if visited.contains(&cell) || Self::get_cell(cell, grid) == '0' {
            return;
        }
        visited.insert(cell);
        let (x, y) = cell;
        
        Self::dfs((x - 1, y), grid, visited);
        Self::dfs((x, y - 1), grid, visited);
        Self::dfs((x + 1, y), grid, visited);
        Self::dfs((x, y + 1), grid, visited);
    }
    
    pub fn num_islands(grid: Vec<Vec<char>>) -> i32 {
        let mut explored_island = HashSet::new();
        let mut island_count = 0;
        
        let m = grid.len() as isize;
        let n = grid[0].len() as isize;
        //println!("m {:?} n {:?}", m, n);
        
        for y in 0..m {
            for x in 0..n {
        
                if Self::get_cell((x, y), &grid) == '0' || explored_island.contains(&(x, y)) {
                    continue;
                } else {
                    // dfs from this starting point as it's a new island
                    Self::dfs((x, y), &grid, &mut explored_island);
                    island_count += 1;
                }
            } 
        }
        
        island_count
    }
}
{{</code>}}

Lets see what tomorrow brings!