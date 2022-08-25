+++
title = "Daily Leetcode 25th August 2022"
date = "2022-08-25T11:52:27+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 383. Ransom Note"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [383. Ransom Note](https://leetcode.com/problems/ransom-note/). Simple enough task to determine if a string has enough characters which could be re-arragned to form another string.

My simple approach was to count the occurances of chars in each string and then compare every one to check it was less than or equal to the available.

Since the problem was restricted lowercase english characters an array serves easily for the mapping, although a `HashMap` could also work if the solution needed to be extended.

{{<code language="rust" title="Ransom Note">}}
impl Solution {
    pub fn can_construct(ransom_note: String, magazine: String) -> bool {
        let mut ransom_map: [i32; 26] = [0; 26];
        let mut magazine_map: [i32; 26] = [0; 26];
        
        ransom_note.chars().for_each(|ch| {
            ransom_map[ch as usize - 'a' as usize] += 1
        });
        
        magazine.chars().for_each(|ch| {
            magazine_map[ch as usize - 'a' as usize] += 1
        });
        
        ransom_map.iter().enumerate().all(|(i, rv)| magazine_map.get(i).map_or(false, |mv| *rv <= *mv))
    }
}
{{</code>}}

This solution worked well and was quite fast. Reportedly beating 66% of other rust solutions. Although there might be an easier way to solve it, this solution is `O(n)` time and `O(1)` additional storage.

Lets see what tomorrow brings.