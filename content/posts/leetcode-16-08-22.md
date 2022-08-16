+++
title = "Daily Leetcode 16th August 2022"
date = "2022-08-16T09:47:46+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 387. First Unique Character in a String"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/).

A fairly easy problem which is quite trivial to solve in linear time and constant space. My basic attempt just used a hashmap to record the number of times a character had been seen, and then uses a quick iterator to get the first char which has a hashmap value of 0.

There are probably some easy optimisations which could be applied here given the restrictions on the input to the problem but I am pretty satisfied with this solution. It ran in approx 30ms and with a small memory footprint.

{{<code language="rust" title="First Unique Character in a String">}}
impl Solution {
    pub fn first_uniq_char(s: String) -> i32 {
        let mut chars = HashMap::<char, i32>::new();
        s.chars().for_each(|c| {
            match chars.get(&c) {
                Some(count) => chars.insert(c, count + 1),
                None => chars.insert(c, 0)
            };
        });
        
        match s.chars().position(|c| chars.get(&c).unwrap_or(&-1) == &0) {
            Some(i) => i as i32,
            None => -1_i32
        }
    }
}
{{</code>}}