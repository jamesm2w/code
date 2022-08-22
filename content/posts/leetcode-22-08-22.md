+++
title = "Daily Leetcode 22nd August 2022"
date = "2022-08-22T10:49:56+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 342. Power of Four"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [342. Power of Four](https://leetcode.com/problems/power-of-four/) which is pretty trivial. I went for a silly `HashSet` approach, since we're confined to an `i32` input, we know that we won't have a number above `4^15`. So with a simple lookup we can get good speeds for not too poor memory usage.

Of course there are probably many other ways to solve this, e.g. through logs, or by iteratively dividing by `4`. Or even just checking the bits of the number.

{{<code language="rust" title="">}}
use std::collections::HashSet;

impl Solution {
    pub fn is_power_of_four(n: i32) -> bool {
        
        let map: HashSet<i32> = HashSet::from([1, 4, 16, 64, 256, 1024, 4096, 16384, 65536, 262144, 1048576, 4194304, 16777216, 67108864, 268435456, 1073741824]);

        map.contains(&n)    
    }
}
{{</code>}}

This ran in 3ms which is nothing to scoff at, but perhaps if the set was statically created we could save a few more ms. Lets see what tomorrow has though!
