+++
title = "Daily Leetcode 24th August 2022"
date = "2022-08-24T10:41:25+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 326. Power of Three"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [326. Power of Three](https://leetcode.com/problems/power-of-three/submissions/). Similar to the other problem, power of four. This is trivial with many different solutions possible. I just went for a simple loop dividing by three until you get to 1 or a number which is not divisible by three, and then easy. Simple, `O(log3(n))` I guess, because you're gonna need max floor log3(n) iterations to either get to a number not divisible by three or `1`. 

Of course another solution would be to have a map for every possible value considering in the signed range there's only 20 it'd be trivial to list them out.

{{<code language="rust" title="Power of 3">}}
impl Solution {
    pub fn is_power_of_three(n: i32) -> bool {
        let mut n = n;
        loop {
            let (quot, rem) = (n / 3, n % 3);
            if rem != 0 || quot == 0 {
                break;
            }   
            n = quot;
        }        
        n == 1
    }
}
{{</code>}}

Lets see what tomorrow brings.