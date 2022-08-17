+++
title = "Daily Leetcode 17th August 2022"
date = "2022-08-17T14:13:47+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 804. Unique Morse Code Words"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [804. Unique Morse Code Words]().

The basic idea was given a list of words in lowercase ascii letters return how many unique morse code words are formed.

The algorithm is fairly simple to loop through each word and construct a morsecode representation through looking up chars in a map; then using some set to keep only unique strings and returning that sets length at the end.

This was a fairly quick solution given about 2ms with fairly small memory footprint.

Again, I could make more use of rusts iterators, but I'm usually more comfortable to write out for loops in the traditional way.

Of particular note is the way that the translation to morse code can be implemented with a static array of string slices, given we only have 26 possible characters we can just use the offset from 'a' as the "hash" to index the array.

{{<code language="rust" title="Unique Morse Code Words">}}
impl Solution {
    pub fn unique_morse_representations(words: Vec<String>) -> i32 {
        let morse_tl: [&str;26] = [".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."];   
    
        let mut transformations = HashSet::new();
        for word in words {
            
            let transformation = word.chars().map(|c| 
                morse_tl[(c as usize) - ('a' as usize)]
            ).fold(
                String::new(), 
                |mut acc, x| {acc.push_str(x); acc}
            );
            transformations.insert(transformation);
        }
        transformations.len() as i32
    }
}
{{</code>}}

I enjoyed this problem, not too hard but had to give it a bit of thought. Let's see what tomorrow brings.