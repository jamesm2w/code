+++
title = "Daily Leetcode 18th August 2022"
date = "2022-08-18T11:31:52+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 1338: Reduce Array Size to the Half"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was no. [1338. Reduce Array Size to the Half](https://leetcode.com/problems/reduce-array-size-to-the-half/). Classified medium, but I found it quite easy.

The core of the problem was to find the miniumum size of a set of numbers to remove from an array to reduce it's size to under a half of the original length.

My solution was a pretty straightforward linear time algorithm, which first builds up a map of each element to it's count within the array. I think iterate through the maps key-value pairs in descending order of the counts, stopping when I would have removed over half the array.

This code ran very quickly, apparently beating 100% of other Rust solutions which I find hard to believe, but I will take it anyway. Some improvements I've made on my previous problem solutions using hashmaps was to mutate the direct entry, rather than getting and inserting, I think it looks a bit neater and it seems to perform well.

{{<code language="rust" title="Reduce Array Size to the Half">}}
impl Solution {
    pub fn min_set_size(arr: Vec<i32>) -> i32 {
        let len = arr.len() as i32;
        let half_len = len / 2;
        
        // Compute the count of each element in the arr. For ease this is just a map
        let mut count: HashMap<i32, i32> = HashMap::new();
        arr.iter().for_each(|el| {
            count.entry(*el).and_modify(|c| *c += 1).or_insert(1);
        });
        
        // Sort the entries in a vec by the count so we have most frequent first
        let mut itv = count.iter().collect::<Vec<_>>();
        itv.sort_by(|(_, a), (_, b)| b.cmp(a));
        
        // greedy strategy
        let mut total = len;
        let mut out = 0;
        while total > half_len {
            total = total - itv[out].1;
            out = out + 1;
        }
        
        out as _
    }
}
{{</code>}}

This code did take quite a lot of memory compared to most solutions apparently with it only beating 25% of other rust solutions, so maybe there are more optimisations to clean up the amount used. One idea could be to use an array of length of the max item in the array (defined to be `10^5`) and then sort that, but that could also be slower.

Lets see what tomorrows problem brings!